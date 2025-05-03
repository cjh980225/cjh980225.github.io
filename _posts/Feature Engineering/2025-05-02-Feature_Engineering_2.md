---
title: "[Seoul Subway] 지하철 이용 데이터로 알아보는 Wide Format vs Long Format"

categories: Feature_Engineering

tags: [python, data, Feature_Engineering, melt, Seoul_Subway]

---

# 지하철 이용 데이터로 알아보는 Wide Format vs Long Format

## 서론 

✅ 데이터 분석을 진행하다 보면 데이터 구조가 분석 과정에 큰 영향을 미친다는 것을 경험하게 됩니다. 같은 정보를 담고 있어도 데이터 구조에 따라 분석의 편의성과 효율성이 달라질 수 있습니다. 

✅ 이번 포스트에서는 데이터 구조의 두가지 중요한 형식인 'Wide Format'과 'Long Format' 에 대해 알아보고, 서울 지하철 승하차 데이터를 활용한 실제 변환 사례를 통해 각 형식의 특징과 활용법을 살펴보겠습니다. 

## 데이터 구조 이해하기 

### Wide Format이란? 

✅ Wide Format은 데이터를 구성할 때 각 관측값이 행(row)으로 표현되고, 여러 변수들이 열(column)로 표현되는 형식입니다. 특히 시간, 카테고리 등의 정보가 여러 열로 펼쳐져 있는 구조를 말합니다. 

✅ Wide Format은 일반적으로 스프레드시트에서 보기 좋은 형태로, 한 행에서 다양한 변수 값들을 한눈에 확인할 수 있습니다. 예를 들어, 지하철 역별로 시간대별 승하차 인원이 각각 다른 열로 구분되어 있을 때 Wide Format이라고 할 수 있습니다. 

### Long Format이란? 

✅ Long Format은 Wide Format과 달리 여러 변수의 값을 하나의 열로 모아서 표현하는 형식입니다. 이때 어떤 변수인지를 나타내는 식별자(identifier)열이 추가됩니다. 

✅ 예를 들어, 시간대별 승하차 인원이 각각 다른 열로 존재하던 Wide Format 데이터를 '시간대'라는 식별자 열과 '승차/하차'라는 유형 열, 그리고 '인원'열로 재구성하면 Long Format이 됩니다. 

## 두 포맷의 비교 

### Wide Format의 장단점 

◾ 장점

✅ 한 눈에 여러 변수의 값을 동시에 확인할 수 있어 직관적입니다. 

✅ 데이터가 간결하게 정리되어 있어 전체적인 패턴 파악이 용이합니다. 

✅ 엑셀과 같은 도구에서 보기 좋고 일반적인 테이블 형태와 유사합니다. 

◾ 단점 

✅ 변수(열)가 많아지면 관리가 어려워집니다. 

✅ 특정 변수에 대한 분석이나 그룹화가 복잡할 수 있습니다. 

✅ 시각화 도구나 통계 분석에 직접 활용하기 어려운 경우가 많습니다. 

### Long Format의 장단점 

◾ 장점 

✅ 데이터 분석 및 시각화 도구와의 호환성이 뛰어납니다. 

✅ 필터링, 그룹화, 집계 작업이 더 직관적이고 간단해집니다. 

✅ 변수 간 관계를 파악하거나 다양한 분석을 수행하기 쉽습니다. 

◾ 단점 

✅ 데이터 행 수가 크게 증가하여 전체 데이터 크기가 커집니다. 

✅ 한 눈에 여러 값을 비교하기 어려울 수 있습니다. 

✅ 스프레드 시트에서 직관적으로 보기에는 불편할 수 있습니다. 

## 지하철 승하차 데이터 변환 사례 

### 원본 데이터 구조(Wide Format)

✅ 서울 지하철 시간대별 승하차 데이터를 살펴보면, 원본 데이터는 전형적인 Wide Format 구조로 되어 있습니다. 


```python
import matplotlib.pyplot as plt
import matplotlib.font_manager as fm
import matplotlib as mpl

# 한글 폰트 설정 (Windows)
plt.rcParams['font.family'] = 'Malgun Gothic'

# 마이너스 깨짐 방지
mpl.rcParams['axes.unicode_minus'] = False
```


```python
import pandas as pd 
import numpy as np
import requests

# 인증키와 날짜 설정
api_key = "-----"
start_date = '2025-01'
end_date = '2025-03'

# 월 단위로 날짜 범위 생성
date_range = pd.date_range(start=start_date, end=end_date, freq='MS')

# 빈 리스트 생성
all_data = []

# 날짜별로 API 요청 보내기
for date in date_range.strftime('%Y%m'):
    # API 요청 URL
    url = f"http://openapi.seoul.go.kr:8088/{api_key}/json/CardSubwayTime/1/1000/{date}"
    
    # 데이터 요청
    response = requests.get(url)
    data = response.json()
    
    # 요청한 날짜에 해당하는 데이터가 있을 때만
    if 'CardSubwayTime' in data:
        df = pd.DataFrame(data['CardSubwayTime']['row'])
        all_data.append(df)

# 데이터프레임으로 합치기
CardSubwayTime_df = pd.concat(all_data, ignore_index=True)
```


```python
rename_dict = {
    'USE_MM': '이용월',
    'SBWY_ROUT_LN_NM': '호선명',
    'STTN': '역이름',
    'JOB_YMD': '수집일'
}

# 시간대별 컬럼들 추가로 변환
for hour in range(4, 24):  # 4시부터 23시
    rename_dict[f'HR_{hour}_GET_ON_NOPE'] = f'{hour}시_승차'
    rename_dict[f'HR_{hour}_GET_OFF_NOPE'] = f'{hour}시_하차'

for hour in range(0, 4):  # 0시부터 3시
    rename_dict[f'HR_{hour}_GET_ON_NOPE'] = f'{hour}시_승차'
    rename_dict[f'HR_{hour}_GET_OFF_NOPE'] = f'{hour}시_하차'

CardSubwayTime_df.drop(columns=['JOB_YMD'], inplace=True)
    
CardSubwayTime_df.rename(columns=rename_dict, inplace=True)
```


```python
CardSubwayTime_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>이용월</th>
      <th>호선명</th>
      <th>역이름</th>
      <th>4시_승차</th>
      <th>4시_하차</th>
      <th>5시_승차</th>
      <th>5시_하차</th>
      <th>6시_승차</th>
      <th>6시_하차</th>
      <th>7시_승차</th>
      <th>...</th>
      <th>23시_승차</th>
      <th>23시_하차</th>
      <th>0시_승차</th>
      <th>0시_하차</th>
      <th>1시_승차</th>
      <th>1시_하차</th>
      <th>2시_승차</th>
      <th>2시_하차</th>
      <th>3시_승차</th>
      <th>3시_하차</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>202501</td>
      <td>1호선</td>
      <td>서울역</td>
      <td>560.0</td>
      <td>51.0</td>
      <td>8353.0</td>
      <td>7685.0</td>
      <td>20145.0</td>
      <td>46128.0</td>
      <td>61204.0</td>
      <td>...</td>
      <td>28021.0</td>
      <td>23793.0</td>
      <td>4266.0</td>
      <td>6765.0</td>
      <td>239.0</td>
      <td>237.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>202501</td>
      <td>1호선</td>
      <td>시청</td>
      <td>89.0</td>
      <td>0.0</td>
      <td>1633.0</td>
      <td>4268.0</td>
      <td>3078.0</td>
      <td>20969.0</td>
      <td>6047.0</td>
      <td>...</td>
      <td>8471.0</td>
      <td>3044.0</td>
      <td>429.0</td>
      <td>811.0</td>
      <td>9.0</td>
      <td>24.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>202501</td>
      <td>1호선</td>
      <td>종각</td>
      <td>138.0</td>
      <td>3.0</td>
      <td>4176.0</td>
      <td>4949.0</td>
      <td>3897.0</td>
      <td>28257.0</td>
      <td>5592.0</td>
      <td>...</td>
      <td>24793.0</td>
      <td>4161.0</td>
      <td>1714.0</td>
      <td>1049.0</td>
      <td>12.0</td>
      <td>16.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>202501</td>
      <td>1호선</td>
      <td>종로3가</td>
      <td>140.0</td>
      <td>9.0</td>
      <td>3338.0</td>
      <td>2424.0</td>
      <td>3227.0</td>
      <td>9593.0</td>
      <td>4432.0</td>
      <td>...</td>
      <td>14304.0</td>
      <td>4001.0</td>
      <td>1378.0</td>
      <td>1692.0</td>
      <td>14.0</td>
      <td>38.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>202501</td>
      <td>1호선</td>
      <td>종로5가</td>
      <td>33.0</td>
      <td>1.0</td>
      <td>1504.0</td>
      <td>2880.0</td>
      <td>2611.0</td>
      <td>12776.0</td>
      <td>4801.0</td>
      <td>...</td>
      <td>4851.0</td>
      <td>3106.0</td>
      <td>255.0</td>
      <td>809.0</td>
      <td>0.0</td>
      <td>21.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 51 columns</p>
</div>




```python
CardSubwayTime_df.columns 
```




    Index(['이용월', '호선명', '역이름', '4시_승차', '4시_하차', '5시_승차', '5시_하차', '6시_승차',
           '6시_하차', '7시_승차', '7시_하차', '8시_승차', '8시_하차', '9시_승차', '9시_하차', '10시_승차',
           '10시_하차', '11시_승차', '11시_하차', '12시_승차', '12시_하차', '13시_승차', '13시_하차',
           '14시_승차', '14시_하차', '15시_승차', '15시_하차', '16시_승차', '16시_하차', '17시_승차',
           '17시_하차', '18시_승차', '18시_하차', '19시_승차', '19시_하차', '20시_승차', '20시_하차',
           '21시_승차', '21시_하차', '22시_승차', '22시_하차', '23시_승차', '23시_하차', '0시_승차',
           '0시_하차', '1시_승차', '1시_하차', '2시_승차', '2시_하차', '3시_승차', '3시_하차'],
          dtype='object')



✅ 이 데이터는 각 역에 대해 시간대별 승차 인원과 하차 인원이 각각 별도의 열로 구성되어 있습니다. 

### 변환 수 데이터 구조(Long Format)

✅ Wide Format의 데이터를 Long Format으로 변환하면 다음과 같은 구조가 됩니다. 


```python
CardSubwayTime_long.columns
```




    Index(['이용월', '호선명', '역이름', '인원', '승차/하차', '시간대'], dtype='object')



✅ 변환 후에는 '승차/하차'와 '시간대'가 변수(카테고리)로 변환되고, 실제 데이터 값인 '인원'이 하나의 열로 통합되었습니다. 

## melt() 함수 활용하기

### melt() 함수 소개

✅ pandas 라이브러리의 melt()함수는 Wide Format 데이터를 Long Format으로 변환하는 대표적인 도구입니다. 이 함수는 기존 데이터프레임의 특정 열들을 식별자로 유지하고, 나머지 열들을 변수(variable)와 값(value)의 쌍으로 재구성합니다. 

### 사용 시기와 이유 

◾ melt() 함수를 사용하는 주요 상황은 다음과 같습니다.

1️⃣ 시간대별 데이터 분석 : 시간대가 여러 열로 분산되어있을 때, 시간을 하나의 변수로 통합하여 시간에 따른 패턴을 분석하기 용이하게 합니다. 

2️⃣ 카테고리별 데이터 분석 : 여러 카테고리가 각각의 열로 존재할 때, 카테고리를 하나의 변수로 모아 비교분석하기 쉽게 합니다. 

3️⃣ 시각화 준비 : 많은 시각화 도구들이 Long Format 데이터를 더 효과적으로 처리할 수 있습니다. 

4️⃣ 광범위한 데이터 관리 : 열이 너무 많아 관리가 어려울 때, 더 구조화된 형태로 재구성할 수 있습니다. 

5️⃣ 통계 분석 준비 : 많은 통계 함수들이 Long Format 데이터를 선호합니다. 

### 코드 예시 

✅ 지하철 승하차 데이터를 Long Format으로 변환하는 예시 코드를 살펴보겠습니다. 


```python
# 'melt'로 시간대별 승차/하차 데이터를 long format으로 변환
CardSubwayTime_long = pd.melt(CardSubwayTime_df,
                              id_vars=['이용월', '호선명', '역이름'],  # 고정된 열들
                              value_vars=[col for col in CardSubwayTime_df.columns if '승차' in col or '하차' in col],  # 승차/하차 관련 열들
                              var_name='시간대_승하차',  # 새로 생성할 열 이름
                              value_name='인원')  # 인원 수

# '시간대_승하차' 열에서 '승차'와 '하차' 구분
CardSubwayTime_long['승차/하차'] = CardSubwayTime_long['시간대_승하차'].apply(lambda x: '승차' if '승차' in x else '하차')

# '시간대'와 '승차/하차'로 구분
CardSubwayTime_long['시간대'] = CardSubwayTime_long['시간대_승하차'].str.extract('(\d+)시')[0]  # '4시', '5시' 추출

# 불필요한 열 제거
CardSubwayTime_long = CardSubwayTime_long.drop(columns=['시간대_승하차'])

# 결과 확인
CardSubwayTime_long.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>이용월</th>
      <th>호선명</th>
      <th>역이름</th>
      <th>인원</th>
      <th>승차/하차</th>
      <th>시간대</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>202501</td>
      <td>1호선</td>
      <td>서울역</td>
      <td>560.0</td>
      <td>승차</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>202501</td>
      <td>1호선</td>
      <td>시청</td>
      <td>89.0</td>
      <td>승차</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>202501</td>
      <td>1호선</td>
      <td>종각</td>
      <td>138.0</td>
      <td>승차</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>202501</td>
      <td>1호선</td>
      <td>종로3가</td>
      <td>140.0</td>
      <td>승차</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>202501</td>
      <td>1호선</td>
      <td>종로5가</td>
      <td>33.0</td>
      <td>승차</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



## 실전 활용 사례 

### 시간대별 분석 

✅ Long Format으로 변환된 데이터를 활용하면 시간대별 승하차 패턴을 쉽게 분석할 수 있습니다. 


```python
# 시간대별 총 승차 인원 분석
time_boarding = CardSubwayTime_long[CardSubwayTime_long['승차/하차'] == '승차'].groupby('시간대')['인원'].sum().reset_index()

# 시간대별 총 하차 인원 분석
time_alighting = CardSubwayTime_long[CardSubwayTime_long['승차/하차'] == '하차'].groupby('시간대')['인원'].sum().reset_index()

# 시간대별 승하차 패턴 시각화
import matplotlib.pyplot as plt

plt.figure(figsize=(12, 6))
plt.plot(time_boarding['시간대'], time_boarding['인원'], 'b-', label='승차')
plt.plot(time_alighting['시간대'], time_alighting['인원'], 'r-', label='하차')
plt.title('시간대별 승하차 패턴')
plt.xlabel('시간대')
plt.ylabel('인원')
plt.legend()
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```


    
![png](/assets/images/Feature_Engineering/2/output_14_0.png)


### 승하차 패턴 분석 

✅ 특정 역의 승하차 패턴을 분석하는 것도 Long Format에서 더 간단합니다. 


```python
# 강남역의 승하차 패턴 분석
gangnam_data = CardSubwayTime_long[CardSubwayTime_long['역이름'] == '강남']

# 승하차별로 그룹화하여 시각화
gangnam_pivot = gangnam_data.pivot_table(
    index='시간대', 
    columns='승차/하차', 
    values='인원', 
    aggfunc='sum'
)

gangnam_pivot.plot(kind='bar', figsize=(14, 7))
plt.title('강남역 시간대별 승하차 패턴')
plt.xlabel('시간대')
plt.ylabel('인원')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```


    
![png](/assets/images/Feature_Engineering/2/output_16_0.png)
    


### 역별 이용 추이 분석 

✅ 가장 이용객이 많은 역을 찾는 분석도 Long Format에서 더 직관적입니다. 


```python
# 역별 총 이용객 (승차 + 하차) 분석
station_total = CardSubwayTime_long.groupby('역이름')['인원'].sum().reset_index().sort_values('인원', ascending=False)

# 상위 10개 역 시각화
plt.figure(figsize=(12, 6))
plt.bar(station_total['역이름'][:10], station_total['인원'][:10])
plt.title('이용객이 가장 많은 상위 10개 역')
plt.xlabel('역이름')
plt.ylabel('총 이용객 수')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```


    
![png](/assets/images/Feature_Engineering/2/output_18_0.png)
    


## 결론

✅ Wide Format과 Long Format은 데이터 분석 과정에서 상황에 따라 적절히 선택하여 활용해야 하는 중요한 데이터 구조 형식입니다. 특히 pandas의 melt() 함수는 Wide Format 데이터를 Long Format으로 손쉽게 변환할 수 있는 강력한 도구입니다. 

✅ 지하철 승하차 데이터 예시에서 볼 수 있듯이, 데이터 분석의 목적에 따라 적절한 데이터 형식을 선택하면 분석 과정이 더욱 효율적이고 직관적으로 진행될 수 있습니다. 시간대별, 승하차별 패턴 분석과 같은 복잡한 분석에는 Long Format, 전체적인 데이터 파악과 같은 간단한 조회에는 Wide Format이 더 적합할 수 있습니다. 

✅ 데이터 분석을 진행할 때는 항상 분석 목적과 방법에 맞게 데이터 구조를 선택하고, 필요시 적절히 변환하는 능력이 중요합니다.
