---
title: "[Dacon 기업 성공 예측] Catboost를 통한 Dacon 기업 성공 예측"

categories: Competitions&Challenges

tags: [python, Catboost, KFold, Dacon, Hyper_Parameters, Feature_Engineering, Tuning, Clipping]
---

# Dacon 기업 성공 예측

[링크](https://dacon.io/competitions/official/236475/overview/description)

✅ 다양한 기업 데이터를 기반으로 AI 알고리즘을 개발하여, 기업의 성공 가능성을 예측하는 것을 목표로 합니다. 

✅ 여러 모델을 경험하고 다양한 기법, 다양한 방법을 경험하는 것을 목표로 임하였습니다. 

# Import


```python
import pandas as pd
import numpy as np
import optuna
from sklearn.preprocessing import LabelEncoder
from sklearn.metrics import mean_absolute_error
from sklearn.model_selection import KFold
from catboost import CatBoostRegressor, Pool
```

# Data Load


```python
train = pd.read_csv('Dacon_data/train.csv')
test = pd.read_csv('Dacon_data/test.csv')
sample_submission = pd.read_csv('Dacon_data/sample_submission.csv')
```

# Data Preprocessing


```python
# 기준 연도 설정
CURRENT_YEAR = 2025

# 기업 나이 생성
train['기업나이'] = CURRENT_YEAR - train['설립연도']
test['기업나이'] = CURRENT_YEAR - test['설립연도']

# 기존 컬럼 제거
train = train.drop(columns=['설립연도'])
test = test.drop(columns=['설립연도'])

train = train.drop(columns=['ID'], axis = 1)
test = test.drop(columns=['ID'], axis = 1)

# 수치형 처리
numeric_features = ['기업나이', '직원 수', '고객수(백만명)', '총 투자금(억원)', '연매출(억원)', 'SNS 팔로워 수(백만명)']

# 이진형 처리
bool_features = ['인수여부', '상장여부']
bool_map = {'Yes': 1, 'No': 0}
for col in bool_features :
    train[col] = train[col].map(bool_map)
    test[col] = test[col].map(bool_map)

# 범주형 처리
category_features = ['국가', '분야', '투자단계', '기업가치(백억원)']
encoders = {}

for feature in category_features:
    encoders[feature] = LabelEncoder()
    train[feature] = train[feature].fillna('Missing')
    test[feature] = test[feature].fillna('Missing')
    train[feature] = encoders[feature].fit_transform(train[feature])
    test[feature] = encoders[feature].transform(test[feature])
    
# 고객수(백만명): 전체 평균으로 채우기
customer_mean = train['고객수(백만명)'].mean()
train['고객수(백만명)'] = train['고객수(백만명)'].fillna(customer_mean)
test['고객수(백만명)'] = test['고객수(백만명)'].fillna(customer_mean)

# 직원 수: 국가별 평균으로 채우기
train['직원 수'] = train.groupby('국가')['직원 수'].transform(lambda x: x.fillna(x.mean()))

# test 데이터는 국가별 평균을 train에서 가져와서 채우기
country_mean = train.groupby('국가')['직원 수'].mean()
test['직원 수'] = test.apply(
    lambda row: country_mean[row['국가']] if pd.isnull(row['직원 수']) else row['직원 수'], axis=1
)

## Feature Engineering 
train['투자 대비 매출'] = train['연매출(억원)'] / (train['총 투자금(억원)'] + 1)
test['투자 대비 매출'] = test['연매출(억원)'] / (test['총 투자금(억원)'] + 1)

# 1인당 매출
train['1인당 매출'] = train['연매출(억원)'] / (train['직원 수'] + 1)
test['1인당 매출'] = test['연매출(억원)'] / (test['직원 수'] + 1)

# 1인당 투자금
train['1인당 투자금'] = train['총 투자금(억원)'] / (train['직원 수'] + 1)
test['1인당 투자금'] = test['총 투자금(억원)'] / (test['직원 수'] + 1)

# 1인당 고객 수
train['1인당 고객'] = train['고객수(백만명)'] / (train['직원 수'] + 1)
test['1인당 고객'] = test['고객수(백만명)'] / (test['직원 수'] + 1)

# 고객당 연매출
train['고객당 연매출'] = train['연매출(억원)'] / (train['고객수(백만명)'] + 1)
test['고객당 연매출'] = test['연매출(억원)'] / (test['고객수(백만명)'] + 1)

# SNS 팔로워 대비 연매출
train['SNS 팔로워 대비 매출'] = train['연매출(억원)'] / (train['SNS 팔로워 수(백만명)'] + 1)
test['SNS 팔로워 대비 매출'] = test['연매출(억원)'] / (test['SNS 팔로워 수(백만명)'] + 1)

# 기업가치 대비 연매출
train['가치 대비 매출'] = train['연매출(억원)'] / (train['기업가치(백억원)'] + 1)
test['가치 대비 매출'] = test['연매출(억원)'] / (test['기업가치(백억원)'] + 1)

# 로그 변환: 왜곡이 심한 컬럼
for col in ['연매출(억원)', '총 투자금(억원)', '직원 수', '고객수(백만명)', 'SNS 팔로워 수(백만명)']:
    train[f'log_{col}'] = np.log1p(train[col])
    test[f'log_{col}'] = np.log1p(test[col])
    
train = train.drop(columns = ['연매출(억원)', '총 투자금(억원)', '직원 수', '고객수(백만명)', 'SNS 팔로워 수(백만명)'])
test = test.drop(columns = ['연매출(억원)', '총 투자금(억원)', '직원 수', '고객수(백만명)', 'SNS 팔로워 수(백만명)'])
    
# 컬럼명의 띄어쓰기를 언더바로 대체
train.columns = train.columns.str.replace(" ", "_")
test.columns = test.columns.str.replace(" ", "_")

train = train.drop(columns = ["인수여부", "상장여부"])
test = test.drop(columns = ["인수여부", "상장여부"])
```

[Feature Engineering](https://cjh980225.github.io/model_training&tuning/ML_2)

# Catboost Hyper Parameter

```python
# ✅ Feature, Target 정의
X = train.drop(columns=['성공확률'])
y = train['성공확률']

# ✅ Optuna objective 함수 정의
def objective(trial):
    params = {
        'iterations': 3000,
        'learning_rate': trial.suggest_float('learning_rate', 0.01, 0.3),
        'depth': trial.suggest_int('depth', 4, 10),
        'l2_leaf_reg': trial.suggest_float('l2_leaf_reg', 1e-3, 10.0, log=True),
        'bagging_temperature': trial.suggest_float('bagging_temperature', 0.0, 1.0),
        'random_strength': trial.suggest_float('random_strength', 1e-3, 10.0, log=True),
        'border_count': trial.suggest_int('border_count', 64, 254),
        'min_data_in_leaf': trial.suggest_int('min_data_in_leaf', 1, 20),
        'grow_policy': trial.suggest_categorical('grow_policy', ['SymmetricTree', 'Depthwise', 'Lossguide']),
        'loss_function': 'MAE',
        'early_stopping_rounds': 100,
        'verbose': 0,
        'random_state': 42
    }

    kf = KFold(n_splits=5, shuffle=True, random_state=42)
    mae_list = []

    for train_idx, valid_idx in kf.split(X):
        X_train, X_valid = X.iloc[train_idx], X.iloc[valid_idx]
        y_train, y_valid = y.iloc[train_idx], y.iloc[valid_idx]

        train_pool = Pool(X_train, y_train)
        valid_pool = Pool(X_valid, y_valid)

        model = CatBoostRegressor(**params)
        model.fit(train_pool, eval_set=valid_pool, use_best_model=True)

        preds = model.predict(valid_pool)
        preds = np.clip(preds, 0, 1)  # ← 이 줄 추가
        mae = mean_absolute_error(y_valid, preds)
        mae_list.append(mae)

    return np.mean(mae_list)

# ✅ 튜닝 시작
sampler = optuna.samplers.TPESampler(seed=42)
study = optuna.create_study(direction='minimize', sampler=sampler)
study.optimize(objective, n_trials=100)

# ✅ 결과 출력
print("Best trial:")
print(study.best_trial.params)
```

    (중략)
    
    Best trial:
    {'learning_rate': 0.02528730846845928, 'depth': 10, 'l2_leaf_reg': 0.1589791488312824, 'bagging_temperature': 0.054469271620564606, 'random_strength': 0.002793402531131714, 'border_count': 242, 'min_data_in_leaf': 1, 'grow_policy': 'Depthwise'}
    

# Catboost로 K-Fold 교차검증 수행


```python
# ✅ Feature, Target 정의
X = train.drop(columns=['성공확률'])
y = train['성공확률']

# ✅ best_params 가져오기 (복사 후 수정 권장)
best_params = study.best_trial.params.copy()

# ✅ 추가로 필요한 고정 파라미터 넣기
best_params.update({
    'iterations': 3000,
    'loss_function': 'MAE',
    'random_state': 42
})

# ✅ 모델 저장용 리스트
models = []

# ✅ 10-Fold 교차검증
kf = KFold(n_splits=5, shuffle=True, random_state=42)

# ✅ 예측값 저장용
val_scores = []

for fold, (train_idx, val_idx) in enumerate(kf.split(X)):
    print(f"🔁 Fold {fold+1}/5")

    X_train, X_val = X.iloc[train_idx], X.iloc[val_idx]
    y_train, y_val = y.iloc[train_idx], y.iloc[val_idx]

    # ✅ Pool 객체 정의
    train_pool = Pool(X_train, y_train)
    val_pool = Pool(X_val, y_val)

    # ✅ 모델 정의
    model = CatBoostRegressor(**best_params)
    model.fit(train_pool, eval_set=val_pool, use_best_model=True)

    # ✅ 모델 저장
    models.append(model)

    # ✅ 검증 점수 저장 (MAE 기준)
    val_pred = model.predict(val_pool)
    val_pred = np.clip(val_pred, 0, 1)  # ✅ 여기도 필요
    score = mean_absolute_error(y_val, val_pred)
    print(f"✅ Fold {fold+1} MAE: {score:.5f}")
    val_scores.append(score)

# ✅ 전체 평균 점수 출력
print(f"\n📊 평균 MAE (튜닝된 CatBoost): {np.mean(val_scores):.5f}")

```

    (중략)

    Shrink model to first 2353 iterations.
    ✅ Fold 5 MAE: 0.19837
    
    📊 평균 MAE (튜닝된 CatBoost): 0.19780
    

# Catboost K-Fold Model Prediction


```python
X_test = test

# 모델의 예측 결과 저장용
predictions = []

for model in models:
    preds = model.predict(X_test)
    predictions.append(preds)

# 모델의 평균 예측
final_prediction = np.mean(predictions, axis=0)

# 최종 예측값 클리핑
final_prediction = np.clip(final_prediction, 0, 1)
```

[Hyper Parameters](https://cjh980225.github.io/model_training&tuning/ML_3)


# Submission

```python
# 제출 양식에 맞게 ID + 예측값 붙이기
sample_submission['성공확률'] = final_prediction
sample_submission.to_csv('./baseline_submission.csv', index = False, encoding = 'utf-8-sig')
```

# Result 

📌 제출 점수 : 0.21235

🏅 등수 (2025.04.26 기준): 12등

# Library version 

```python
import pandas as pd
import numpy as np
import optuna
import sklearn
import catboost
import sys


print("Python:", sys.version)
print("pandas:", pd.__version__)
print("numpy:", np.__version__)
print("optuna:", optuna.__version__)
print("scikit-learn:", sklearn.__version__)
print("catboost:", catboost.__version__)
```

    Python: 3.11.4 (tags/v3.11.4:d2340ef, Jun  7 2023, 05:45:37) [MSC v.1934 64 bit (AMD64)]
    pandas: 2.2.3
    numpy: 1.26.4
    optuna: 4.2.1
    scikit-learn: 1.6.1
    catboost: 1.2.7