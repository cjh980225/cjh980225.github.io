---
title: "[Seoul_Subway] 서울 지하철 이용 패턴 분석 : 2025년 1분기 데이터로 살펴본 지하철 이용 행태"

categories: Analysis_Reports

tags: [python, data, analysis, EDA, Visulaization, Report, Seoul_Subway]

---

# 서울 지하철 이용 패턴 분석 : 2025년 1분기 데이터로 살펴본 지하철 이용 행태

✅ 서울 지하철은 하루 평균 수백만 명이 이용하는 대중교통 수단으로, 시민들의 일상 속에 깊이 자리잡고 있습니다. 사람들의 이동 패턴은 도시의 리듬을 보여주는 중요한 지표이기도 합니다. 이번 분석에서는 2025년 1월 1일부터 3월 31일까지의 CardSubwayStatsNew데이터를 통해 서울 시민들의 지하철 이용 패턴을 다양한 관점에서 분석해보았습니다. 

# 데이터 소개

✅ 분석에 사용된 데이터는 2025년 1분기 (1월 ~ 3월) 서울 지하철 승하차 정보로, 다음과 같은 특성을 가지고 있습니다. 

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
import seaborn as sns 
import requests

# 인증키와 날짜 설정
api_key = "??"
start_date = '20250101'
end_date = '20250331'

# 날짜 범위 생성 (2025년 1월 1일 ~ 3월 31일)
date_range = pd.date_range(start=start_date, end=end_date)

# 빈 리스트 생성
all_data = []

# 날짜별로 API 요청 보내기
for date in date_range.strftime('%Y%m%d'):
    # API 요청 URL
    url = f"http://openapi.seoul.go.kr:8088/{api_key}/json/CardSubwayStatsNew/1/1000/{date}"
    
    # 데이터 요청
    response = requests.get(url)
    data = response.json()
    
    # 요청한 날짜에 해당하는 데이터가 있을 때만
    if 'CardSubwayStatsNew' in data:
        df = pd.DataFrame(data['CardSubwayStatsNew']['row'])
        all_data.append(df)

# 데이터프레임으로 합치기
CardSubwayStatsNew_df = pd.concat(all_data, ignore_index=True)
```


```python
CardSubwayStatsNew_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 55528 entries, 0 to 55527
    Data columns (total 6 columns):
     #   Column           Non-Null Count  Dtype  
    ---  ------           --------------  -----  
     0   USE_YMD          55528 non-null  object 
     1   SBWY_ROUT_LN_NM  55528 non-null  object 
     2   SBWY_STNS_NM     55528 non-null  object 
     3   GTON_TNOPE       55528 non-null  float64
     4   GTOFF_TNOPE      55528 non-null  float64
     5   REG_YMD          55528 non-null  object 
    dtypes: float64(2), object(4)
    memory usage: 2.5+ MB
    

✅ 총 55528개의 데이터 포인트

✅ 포함 정보 : 일자, 호선명, 역이름, 승차인원, 하차인원, 등록일자

# 데이터 전처리

✅ 전처리 내용 : 컬럼명 수정, 등록일자 삭제, 요일 컬럼 추가, 공휴일 여부 컬럼 추가, 일종 컬럼 추가 

✅ 요일 : 0(월요일)부터 6(일요일)까지 숫자로 표현

✅ 공휴일 여부는 1(공휴일), 0(공휴일 아님)으로 구분 

✅ 일종은 '공휴일', '주말', '평일'로 구분 


```python
CardSubwayStatsNew_df.rename(columns={
    'USE_YMD': '일자',
    'SBWY_ROUT_LN_NM': '호선명',
    'SBWY_STNS_NM': '역이름',
    'GTON_TNOPE': '승차인원',
    'GTOFF_TNOPE': '하차인원',
}, inplace=True)

CardSubwayStatsNew_df.drop(columns=['REG_YMD'], inplace=True)

CardSubwayStatsNew_df['일자'] = pd.to_datetime(CardSubwayStatsNew_df['일자'], format='%Y%m%d')
CardSubwayStatsNew_df['요일'] = CardSubwayStatsNew_df['일자'].dt.weekday  # 0:월요일, 1:화요일, ..., 6:일요일

CardSubwayStatsNew_df['공휴일여부'] = CardSubwayStatsNew_df['일자'].isin([
    pd.Timestamp('2025-01-01'),  # 신정
    pd.Timestamp('2025-01-27'),
    pd.Timestamp('2025-01-28'),
    pd.Timestamp('2025-01-29'),
    pd.Timestamp('2025-01-30'),  # 설 연휴
    pd.Timestamp('2025-03-03')   # 삼일절 대체휴일
]).astype(int)

def classify_day(row):
    if row['공휴일여부'] == 1:
        return '공휴일'
    elif row['요일'] >= 5:
        return '주말'
    else:
        return '평일'

CardSubwayStatsNew_df['일종'] = CardSubwayStatsNew_df.apply(classify_day, axis=1)

CardSubwayStatsNew_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 55528 entries, 0 to 55527
    Data columns (total 8 columns):
     #   Column  Non-Null Count  Dtype         
    ---  ------  --------------  -----         
     0   일자      55528 non-null  datetime64[ns]
     1   호선명     55528 non-null  object        
     2   역이름     55528 non-null  object        
     3   승차인원    55528 non-null  float64       
     4   하차인원    55528 non-null  float64       
     5   요일      55528 non-null  int32         
     6   공휴일여부   55528 non-null  int32         
     7   일종      55528 non-null  object        
    dtypes: datetime64[ns](1), float64(2), int32(2), object(3)
    memory usage: 3.0+ MB
    

## 전체 승차량


```python
import calplot
import matplotlib.pyplot as plt

# 일자별 승차량 합계
calendar_data = CardSubwayStatsNew_df.groupby('일자')['승차인원'].sum()

# 캘린더 히트맵
calplot.calplot(
    calendar_data, 
    cmap = 'YlGnBu',
    colorbar = True,
    suptitle = '2025년 일자별 서울 지하철 승차인원',
    figsize = (18, 5)
)
```
    
![png](/assets/images/Analysis_Report/1/output_7_3.png)

# 가설 검증

## 가설 1️⃣ : 평일과 주말/공휴일의 지하철 이용 패턴은 다를 것이다. 


```python
import matplotlib.pyplot as plt
import numpy as np

week_avg = CardSubwayStatsNew_df.groupby('요일')['승차인원'].mean()
labels = ['월', '화', '수', '목', '금', '토', '일']
values = week_avg.tolist()
values += values[:1]  # 원형으로 만들기 위해 첫값 추가

angles = np.linspace(0, 2 * np.pi, len(labels) + 1)

plt.figure(figsize=(6, 6))
ax = plt.subplot(111, polar = True)
ax.plot(angles, values, linewidth = 2, linestyle = 'solid')
ax.fill(angles, values, alpha = 0.3)
ax.set_xticks(angles[:-1])
ax.set_xticklabels(labels)
plt.title('요일별 평균 승차 인원')
plt.show()
```


    
![png](/assets/images/Analysis_Report/1/output_9_0.png)
    



```python
df = CardSubwayStatsNew_df.copy()

df['월'] = df['일자'].dt.month

group = df.groupby(['월', '일종'])['승차인원'].mean().reset_index()

group['조합'] = (group['월'].astype(str) + '월_' + 
              group['일종'] + '_승차')

group = group.drop('월', axis = 1)
group = group.drop('일종', axis = 1)

group
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
      <th>승차인원</th>
      <th>조합</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5072.575365</td>
      <td>1월_공휴일_승차</td>
    </tr>
    <tr>
      <th>1</th>
      <td>7829.191903</td>
      <td>1월_주말_승차</td>
    </tr>
    <tr>
      <th>2</th>
      <td>12023.230457</td>
      <td>1월_평일_승차</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8053.991896</td>
      <td>2월_주말_승차</td>
    </tr>
    <tr>
      <th>4</th>
      <td>12366.336683</td>
      <td>2월_평일_승차</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6955.465041</td>
      <td>3월_공휴일_승차</td>
    </tr>
    <tr>
      <th>6</th>
      <td>8830.128384</td>
      <td>3월_주말_승차</td>
    </tr>
    <tr>
      <th>7</th>
      <td>13185.171827</td>
      <td>3월_평일_승차</td>
    </tr>
  </tbody>
</table>
</div>




```python
import matplotlib.pyplot as plt
from matplotlib.patches import Patch

# 색상 지정
color_map = {
    '평일': '#90CAF9',
    '주말': '#FFAB91',
    '공휴일': '#A5D6A7'
}

# 색상 매핑
colors = [color_map[entry.split('_')[1]] for entry in group['조합']]

# 그래프 그리기
plt.figure(figsize = (20, 15))
bars = plt.bar(group['조합'], group['승차인원'], color = colors)

# 제목 및 라벨
plt.title('월별 평일과 주말/공휴일 승차인원', fontsize = 24)
plt.ylabel('승차인원 평균', fontsize = 16)
plt.xticks(rotation = 45, fontsize = 14)
plt.yticks(fontsize = 14)

# y축 범위 설정
plt.ylim(0, 15000)

# 막대 위에 수치 표시
for bar in bars:
    height = bar.get_height()
    plt.text(
        bar.get_x() + bar.get_width()/2,  # 막대 중앙
        height + 1000,                    # 막대 위 약간 띄움
        f'{int(height):,}',              # 천 단위 콤마 표시
        ha = 'center', va = 'bottom', fontsize = 12
    )

# 범례
legend_elements = [
    Patch(facecolor = color_map['평일'], label = '평일'),
    Patch(facecolor = color_map['주말'], label = '주말'),
    Patch(facecolor = color_map['공휴일'], label = '공휴일')
]
plt.legend(
    handles = legend_elements,
    title = '일종',
    fontsize = 14,
    title_fontsize = 16
)

plt.tight_layout()
plt.show()
```


    
![png](/assets/images/Analysis_Report/1/output_11_0.png)
    


✅ 평일 평균 이용객 수는 주말이나 공휴일보다 많음 이는 출퇴근 인구의 영향이 크게 작용한 것으로 보입니다. 

✅ 평일 평균 이용객 수는 주말보다 53%가 더 많고, 공휴일에 비해 평일 이용객 수는 2배 이상 많았습니다. 

## 가설 2️⃣ : 요일에 따라 승차/하차 인원은 일정한 패턴을 보일 것이다. 


```python
df = CardSubwayStatsNew_df.copy()

df = df.drop('요일', axis = 1)
df = df.drop('공휴일여부', axis = 1)

group = df.groupby(['일자', '일종'])[['승차인원']].sum().reset_index()

plt.figure(figsize = (20, 12))

sns.lineplot(data = group, x = '일자', y = '승차인원', 
            marker = 'o', linewidth = 1)

# 제목, 축 레이블 설정
plt.title('일자별 승차인원 변화', fontsize=24)  # 제목
plt.ylabel('승차인원', fontsize=18)  # y축

# x, y 축 값의 글씨 크기 수정
plt.xticks(fontsize = 14)
plt.yticks(fontsize = 14)

# 그래프 표시
plt.tight_layout()
plt.show()
```


    
![png](/assets/images/Analysis_Report/1/output_14_0.png)
    


✅ lineplot에서 주목할 만한 현상이 발견되었습니다. 요일별로 전반적으로 일정한 패턴을 유지하던 그래프가 공휴일 지점에서 급격한 변동(스파이크)을 보이는 현상이 관찰되었습니다. 이러한 불규칙의 패턴은 '일종'이 '공휴일'인 데이터로 인해 발생한 것으로 판단됩니다. 

✅ 이를 더 정확히 검증하기 위해 '일종'이 '공휴일'인 행을 제거한 후 라인플롯을 다시 그려보았습니다. 


```python
# '일종'이 '공휴일'인 행을 삭제
group_drop_public = group[group['일종'] != '공휴일']
group_drop_public
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
      <th>일자</th>
      <th>일종</th>
      <th>승차인원</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>2025-01-02</td>
      <td>평일</td>
      <td>7224979.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2025-01-03</td>
      <td>평일</td>
      <td>7620278.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2025-01-04</td>
      <td>주말</td>
      <td>5499672.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2025-01-05</td>
      <td>주말</td>
      <td>3926346.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2025-01-06</td>
      <td>평일</td>
      <td>7359852.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>85</th>
      <td>2025-03-27</td>
      <td>평일</td>
      <td>8180323.0</td>
    </tr>
    <tr>
      <th>86</th>
      <td>2025-03-28</td>
      <td>평일</td>
      <td>8547782.0</td>
    </tr>
    <tr>
      <th>87</th>
      <td>2025-03-29</td>
      <td>주말</td>
      <td>6326644.0</td>
    </tr>
    <tr>
      <th>88</th>
      <td>2025-03-30</td>
      <td>주말</td>
      <td>4485935.0</td>
    </tr>
    <tr>
      <th>89</th>
      <td>2025-03-31</td>
      <td>평일</td>
      <td>7858343.0</td>
    </tr>
  </tbody>
</table>
<p>84 rows × 3 columns</p>
</div>




```python
plt.figure(figsize = (20, 12))

sns.lineplot(data = group_drop_public, x = '일자', y = '승차인원', 
            marker = 'o', linewidth = 1)

# 제목, 축 레이블 설정
plt.title('일자별 승차인원 변화', fontsize = 24)  # 제목
plt.ylabel('승차인원', fontsize = 18)  # y축

# x, y 축 값의 글씨 크기 수정
plt.xticks(fontsize = 14)
plt.yticks(fontsize = 14)

# 그래프 표시
plt.tight_layout()
plt.show()
```


    
![png](/assets/images/Analysis_Report/1/output_17_0.png)
    


✅ 그 결과 그래프가 훨씬 더 일관된 패턴을 보이며 요일에 따른 자연스러운 변동만 남게 되었습니다. 이는 공휴일의 이동 패턴이 평일이나 주말과는 현저히 다르며, 요일별 패턴 분석 시 노이즈로 작용할 수 있음을 시사합니다. 

✅ 따라서 요일별 패턴을 정확히 분석하기 위해서는 공휴일 데이터를 별도로 처리하는 것이 더 정확한 결과를 얻는데 도움이 될 것입니다. 

## 가설 3️⃣ : 공휴일이 평일 중간에 있을 경우, 전날이나 다음 날 승하차 인원은 변동이 생긴다.


```python
df = CardSubwayStatsNew_df.copy()

import pandas as pd

# 기준일 설정
start_date = pd.Timestamp("2025-01-01")

# 각 행의 날짜와 기준일 사이의 차이 계산
df['요일_정수'] = df['일자'].dt.weekday  # 0:월 ~ 6:일, 하지만 기준은 0:월요일
df['days_since_start'] = (df['일자'] - start_date).dt.days

# 기준일이 수요일(2025-01-01)이고, 주차 기준은 일요일~토요일이므로
# 시작일로부터 며칠이 지났는지 기준으로 주차 계산
# 시작일로부터 몇 번째 '일요일'을 만났는지 계산
df['주차'] = ((df['일자'] - start_date).dt.days + start_date.weekday()) // 7 + 1

# 필요 없는 중간 컬럼 삭제
df.drop(columns=['요일_정수', 'days_since_start'], inplace=True)
```


```python
# 1. 주차별 공휴일만 추출
holidays = df[df['공휴일여부'] == 1][['일자', '주차']].copy()
holidays = holidays.sort_values('일자')

# 2. 각 주차에 포함된 공휴일 날짜 목록 만들기
week_holiday_groups = holidays.groupby('주차')['일자'].apply(list).reset_index()
week_holiday_groups.columns = ['주차', '공휴일목록']

# 3. 주차별로 공휴일이 연속인지, 단독인지 판단
def analyze_holidays(dates):
    if len(dates) == 0:
        return '없음'
    sorted_dates = sorted(dates)
    diffs = [(b - a).days for a, b in zip(sorted_dates[:-1], sorted_dates[1:])]
    if any(d == 1 for d in diffs):
        return '연속'
    elif len(sorted_dates) == 1:
        return '하루짜리'
    else:
        return '불연속'

week_holiday_groups['공휴일유형'] = week_holiday_groups['공휴일목록'].apply(analyze_holidays)

# 4. 전체 주차 목록 생성 (공휴일 없는 주도 포함)
all_weeks = pd.DataFrame({'주차': sorted(df['주차'].unique())})

# 5. 병합해서 공휴일 없는 주도 포함되도록
final = all_weeks.merge(week_holiday_groups, on='주차', how='left')
final['공휴일목록'] = final['공휴일목록'].apply(lambda x: x if isinstance(x, list) else [])
final['공휴일유형'] = final['공휴일유형'].fillna('없음')
```


```python
# 예를 들어 group 테이블과 merge 후
merged = pd.merge(df, final[['주차', '공휴일유형']], on = '주차')

def custom_label(row):
    if row['공휴일유형'] == '불연속' and row['주차'] == 1:
        return '1주차 불연속'
    elif row['공휴일유형'] == '불연속' and row['주차'] == 10:
        return '10주차 불연속'
    elif row['공휴일유형'] == '연속' and row['주차'] == 5:
        return '5주차 연속'
    elif row['공휴일유형'] == '없음':
        return '없음'
    else:
        return '제외'

# 새로운 컬럼 추가
merged['공휴일범례'] = merged.apply(custom_label, axis=1)
```


```python
# '제외'는 분석에서 제거
filtered = merged[merged['공휴일범례'] != '제외']

# '없음'은 평균, 나머지는 그대로 유지
# => 요일별 승차인원 평균 계산
grouped = (
    filtered.groupby(['공휴일범례', '요일'])['승차인원']
    .mean()
    .reset_index()
)

grouped
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
      <th>공휴일범례</th>
      <th>요일</th>
      <th>승차인원</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10주차 불연속</td>
      <td>0</td>
      <td>6955.465041</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10주차 불연속</td>
      <td>1</td>
      <td>12281.009724</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10주차 불연속</td>
      <td>2</td>
      <td>12911.692058</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10주차 불연속</td>
      <td>3</td>
      <td>13131.962783</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10주차 불연속</td>
      <td>4</td>
      <td>13566.200972</td>
    </tr>
    <tr>
      <th>5</th>
      <td>10주차 불연속</td>
      <td>5</td>
      <td>10379.933549</td>
    </tr>
    <tr>
      <th>6</th>
      <td>10주차 불연속</td>
      <td>6</td>
      <td>7443.081301</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1주차 불연속</td>
      <td>2</td>
      <td>5193.557536</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1주차 불연속</td>
      <td>3</td>
      <td>11709.852512</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1주차 불연속</td>
      <td>4</td>
      <td>12390.695935</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1주차 불연속</td>
      <td>5</td>
      <td>8928.038961</td>
    </tr>
    <tr>
      <th>11</th>
      <td>1주차 불연속</td>
      <td>6</td>
      <td>6353.310680</td>
    </tr>
    <tr>
      <th>12</th>
      <td>5주차 연속</td>
      <td>0</td>
      <td>6344.121556</td>
    </tr>
    <tr>
      <th>13</th>
      <td>5주차 연속</td>
      <td>1</td>
      <td>4587.215559</td>
    </tr>
    <tr>
      <th>14</th>
      <td>5주차 연속</td>
      <td>2</td>
      <td>4085.998379</td>
    </tr>
    <tr>
      <th>15</th>
      <td>5주차 연속</td>
      <td>3</td>
      <td>5151.983793</td>
    </tr>
    <tr>
      <th>16</th>
      <td>5주차 연속</td>
      <td>4</td>
      <td>10311.776699</td>
    </tr>
    <tr>
      <th>17</th>
      <td>5주차 연속</td>
      <td>5</td>
      <td>8230.353323</td>
    </tr>
    <tr>
      <th>18</th>
      <td>5주차 연속</td>
      <td>6</td>
      <td>6277.123377</td>
    </tr>
    <tr>
      <th>19</th>
      <td>없음</td>
      <td>0</td>
      <td>12289.279882</td>
    </tr>
    <tr>
      <th>20</th>
      <td>없음</td>
      <td>1</td>
      <td>12444.511108</td>
    </tr>
    <tr>
      <th>21</th>
      <td>없음</td>
      <td>2</td>
      <td>12478.247571</td>
    </tr>
    <tr>
      <th>22</th>
      <td>없음</td>
      <td>3</td>
      <td>12590.848087</td>
    </tr>
    <tr>
      <th>23</th>
      <td>없음</td>
      <td>4</td>
      <td>13082.970006</td>
    </tr>
    <tr>
      <th>24</th>
      <td>없음</td>
      <td>5</td>
      <td>9763.293603</td>
    </tr>
    <tr>
      <th>25</th>
      <td>없음</td>
      <td>6</td>
      <td>7010.641387</td>
    </tr>
  </tbody>
</table>
</div>




```python
import seaborn as sns
import matplotlib.pyplot as plt

plt.figure(figsize = (12, 6))
sns.lineplot(data = grouped, x = '요일', y = '승차인원', hue = '공휴일범례', marker = 'o', palette = 'Dark2')

plt.title('요일별 평균 승차인원 - 공휴일 유형 세분화', fontsize = 18)
plt.xlabel('요일 (0 = 월, 6 = 일)', fontsize = 14)
plt.ylabel('평균 승차인원', fontsize = 14)
plt.xticks(ticks = range(7), labels = ['월', '화', '수', '목', '금', '토', '일'])
plt.legend(title = '공휴일 범례', fontsize = 12, title_fontsize = 13)
plt.grid(axis = 'y', linestyle = '--', alpha = 0.5)
plt.tight_layout()
plt.show()
```


    
![png](/assets/images/Analysis_Report/1/output_24_0.png)
    


✅ 1주차 불연속 공휴일 : 수요일 (신정)

✅ 5주차 연속 공휴일 : 월요일, 화요일, 수요일, 목요일 (구정 연휴)

✅ 10주차 불연속 공휴일 : 월요일 (삼일절 대체휴가)

✅ lineplot을 확인해봤을 때, 공휴일의 지하철 이용량이 적은 것을 확인할 수 있었습니다. 

✅ 공휴일 없음 기준 변화율 

◾ 1주차 불연속 공휴일 : 이용량 58.4% 급감 

◾ 5주차 연속 공휴일 : 월요일 ~ 목요일 구간 이용량 50 ~ 67% 감소, 특히 화요일 63%, 수요일 67% 감소로 매우 큰 영향 

◾ 10주차 불연속 공휴일 : 이용량 43.4% 감소

## 가설 4️⃣ : 특정 요일에 특정 역에서의 이용량이 급증하거나 급감하는 패턴이 존재할 수 있다.


```python
df = CardSubwayStatsNew_df.copy()

week_avg = df.groupby(['역이름', '요일'])[['승차인원', '하차인원']].mean().reset_index()

target = ['강남', '홍대입구', '여의도', '종로3가']

target_week = week_avg[week_avg['역이름'].isin(target)]
target_week
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
      <th>역이름</th>
      <th>요일</th>
      <th>승차인원</th>
      <th>하차인원</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>77</th>
      <td>강남</td>
      <td>0</td>
      <td>82028.846154</td>
      <td>80311.923077</td>
    </tr>
    <tr>
      <th>78</th>
      <td>강남</td>
      <td>1</td>
      <td>84866.083333</td>
      <td>83055.583333</td>
    </tr>
    <tr>
      <th>79</th>
      <td>강남</td>
      <td>2</td>
      <td>79719.846154</td>
      <td>78052.384615</td>
    </tr>
    <tr>
      <th>80</th>
      <td>강남</td>
      <td>3</td>
      <td>86383.230769</td>
      <td>84455.923077</td>
    </tr>
    <tr>
      <th>81</th>
      <td>강남</td>
      <td>4</td>
      <td>95241.692308</td>
      <td>94510.153846</td>
    </tr>
    <tr>
      <th>82</th>
      <td>강남</td>
      <td>5</td>
      <td>63309.461538</td>
      <td>62695.000000</td>
    </tr>
    <tr>
      <th>83</th>
      <td>강남</td>
      <td>6</td>
      <td>36214.538462</td>
      <td>34883.076923</td>
    </tr>
    <tr>
      <th>2485</th>
      <td>여의도</td>
      <td>0</td>
      <td>30335.115385</td>
      <td>30385.307692</td>
    </tr>
    <tr>
      <th>2486</th>
      <td>여의도</td>
      <td>1</td>
      <td>32190.083333</td>
      <td>32408.000000</td>
    </tr>
    <tr>
      <th>2487</th>
      <td>여의도</td>
      <td>2</td>
      <td>30207.307692</td>
      <td>30428.115385</td>
    </tr>
    <tr>
      <th>2488</th>
      <td>여의도</td>
      <td>3</td>
      <td>32858.615385</td>
      <td>33031.923077</td>
    </tr>
    <tr>
      <th>2489</th>
      <td>여의도</td>
      <td>4</td>
      <td>34611.461538</td>
      <td>34578.346154</td>
    </tr>
    <tr>
      <th>2490</th>
      <td>여의도</td>
      <td>5</td>
      <td>16707.538462</td>
      <td>16720.769231</td>
    </tr>
    <tr>
      <th>2491</th>
      <td>여의도</td>
      <td>6</td>
      <td>11167.000000</td>
      <td>10564.538462</td>
    </tr>
    <tr>
      <th>3108</th>
      <td>종로3가</td>
      <td>0</td>
      <td>16637.538462</td>
      <td>16155.256410</td>
    </tr>
    <tr>
      <th>3109</th>
      <td>종로3가</td>
      <td>1</td>
      <td>16827.527778</td>
      <td>16235.361111</td>
    </tr>
    <tr>
      <th>3110</th>
      <td>종로3가</td>
      <td>2</td>
      <td>16421.461538</td>
      <td>15911.820513</td>
    </tr>
    <tr>
      <th>3111</th>
      <td>종로3가</td>
      <td>3</td>
      <td>17748.769231</td>
      <td>17123.487179</td>
    </tr>
    <tr>
      <th>3112</th>
      <td>종로3가</td>
      <td>4</td>
      <td>18989.230769</td>
      <td>19105.923077</td>
    </tr>
    <tr>
      <th>3113</th>
      <td>종로3가</td>
      <td>5</td>
      <td>20071.923077</td>
      <td>19781.051282</td>
    </tr>
    <tr>
      <th>3114</th>
      <td>종로3가</td>
      <td>6</td>
      <td>11062.410256</td>
      <td>10555.794872</td>
    </tr>
    <tr>
      <th>3612</th>
      <td>홍대입구</td>
      <td>0</td>
      <td>27506.333333</td>
      <td>28260.820513</td>
    </tr>
    <tr>
      <th>3613</th>
      <td>홍대입구</td>
      <td>1</td>
      <td>26799.833333</td>
      <td>27769.750000</td>
    </tr>
    <tr>
      <th>3614</th>
      <td>홍대입구</td>
      <td>2</td>
      <td>26809.871795</td>
      <td>27849.538462</td>
    </tr>
    <tr>
      <th>3615</th>
      <td>홍대입구</td>
      <td>3</td>
      <td>28432.871795</td>
      <td>30028.974359</td>
    </tr>
    <tr>
      <th>3616</th>
      <td>홍대입구</td>
      <td>4</td>
      <td>32070.615385</td>
      <td>35360.717949</td>
    </tr>
    <tr>
      <th>3617</th>
      <td>홍대입구</td>
      <td>5</td>
      <td>36944.794872</td>
      <td>40849.256410</td>
    </tr>
    <tr>
      <th>3618</th>
      <td>홍대입구</td>
      <td>6</td>
      <td>28987.846154</td>
      <td>29108.538462</td>
    </tr>
  </tbody>
</table>
</div>




```python
import matplotlib.pyplot as plt
import seaborn as sns

# 시각화를 위해 먼저 데이터프레임을 하나로 정리
# 예: 승차인원 기준으로 보기
plot_df = target_week.copy()  # 위에서 구한 DataFrame 사용
plot_df['요일명'] = plot_df['요일'].map({
    0: '월', 1: '화', 2: '수', 3: '목', 4: '금', 5: '토', 6: '일'
})

# 승차인원 barplot
plt.figure(figsize = (12, 6))
sns.barplot(data = plot_df, x = '요일명', y = '승차인원', hue = '역이름')
plt.title('요일별 역별 평균 승차인원 비교')
plt.ylabel('평균 승차인원')
plt.xlabel('요일')
plt.legend(title = '역이름')
plt.tight_layout()
plt.show()
```


    
![png](/assets/images/Analysis_Report/1/output_28_0.png)
    



```python
import matplotlib.pyplot as plt
import seaborn as sns

target_week.loc[:, '요일명'] = target_week['요일'].map({
    0: '월', 1: '화', 2: '수', 3: '목', 4: '금', 5: '토', 6: '일'
})

target_week.loc[:, '요일명'] = pd.Categorical(
    target_week['요일명'],
    categories = ['월', '화', '수', '목', '금', '토', '일'],
    ordered = True
)


# 꺾은선 그래프 그리기
plt.figure(figsize = (12, 6))
sns.lineplot(data = target_week, x = '요일명', y = '승차인원', hue = '역이름', marker = 'o')
plt.title('요일별 역별 평균 승차인원 변화')
plt.ylabel('평균 승차인원')
plt.xlabel('요일')
plt.legend(title = '역이름')
plt.grid(True)
plt.tight_layout()
plt.show()
```

    C:\Users\jihun\AppData\Local\Temp\ipykernel_49776\135975185.py:4: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      target_week.loc[:, '요일명'] = target_week['요일'].map({
    


    
![png](/assets/images/Analysis_Report/1/output_29_1.png)
    


✅ 강남

◾ 평일에 가장 혼잡, 주말엔 약 42% 감소 

◾ 전형적인 출퇴근 중심 상권으로 추정됩니다. 

✅ 여의도

◾ 강남보다 더 큰 차이. 주말에 승하차 인원이 절반 이하로 급감 

◾ 업무 밀집 지역으로 추정됨 (주말 비활성)

✅ 종로 3가역

◾ 변동이 거의 없음 (10% 이내 감소)

◾ 주중/주말 균형된 유동인구 → 생활밀착형 지역일 가능성 높음

✅ 홍대입구역

◾ 주말에 오히려 승차 + 하차 인원 증가 

◾ 주말 유흥 · 문화 중심지의 전형적인 흐름

## 평일 승하차 많은 역 Top10


```python
df = CardSubwayStatsNew_df.copy()
```


```python
import matplotlib.pyplot as plt
import seaborn as sns

# 승차량 상위 10개 역
top10_boarding = merged[merged['일종'] == '평일'].groupby('역이름')['승차인원'].mean().sort_values(ascending = False).head(10)

# 하차량 상위 10개 역
top10_alighting = merged[merged['일종'] == '평일'].groupby('역이름')['하차인원'].mean().sort_values(ascending = False).head(10)

# 그래프 설정
fig, axes = plt.subplots(1, 2, figsize=(15, 6))

# 승차량 (가로 막대 그래프)
sns.barplot(y = top10_boarding.index, x = top10_boarding.values, ax = axes[0])
axes[0].set_title('평일 승차 많은 역 Top 10')
axes[0].set_xlabel('평균 승차인원')
axes[0].set_ylabel('역이름')

# 하차량 (가로 막대 그래프)
sns.barplot(y = top10_alighting.index, x = top10_alighting.values, ax = axes[1])
axes[1].set_title('평일 하차 많은 역 Top 10')
axes[1].set_xlabel('평균 하차인원')
axes[1].set_ylabel('역이름')

plt.tight_layout()
plt.show()
```


    
![png](/assets/images/Analysis_Report/1/output_33_0.png)
    



```python
# 승차와 하차의 평균을 계산
boarding_avg = merged[merged['일종'] == '평일'].groupby('역이름')['승차인원'].mean().sort_values(ascending = False).head(10)
alighting_avg = merged[merged['일종'] == '평일'].groupby('역이름')['하차인원'].mean().sort_values(ascending = False).head(10)

# 상위 10개 역만 선택
top10_boarding = boarding_avg.head(10)
top10_alighting = alighting_avg.head(10)

# 스캐터 플롯 그리기
plt.figure(figsize = (8, 6))
plt.scatter(top10_boarding, top10_alighting, color = 'purple')
plt.title('평일 승차 vs 하차 많은 역 Top 10')
plt.xlabel('평균 승차인원')
plt.ylabel('평균 하차인원')

# 각 점에 역 이름 추가
for i, txt in enumerate(top10_boarding.index) :
    plt.annotate(txt, (top10_boarding[i], top10_alighting[i]), fontsize=9)

plt.tight_layout()
plt.show()
```


    
![png](/assets/images/Analysis_Report/1/output_34_0.png)
    


✅ 분석 기간 동안 평일 기준 승하차 인원이 가장 많은 10개 역을 살펴본 결과, 각 역의 승차 인원과 하차 인원 간에 상관관계 (y = x 그래프)가 나타났습니다. 이는 대체로 많은 사람들이 출근시 이용하는 여겡서 퇴근 시에도 많은 인원이 이동하는 패턴을 보여줍니다. 

✅ 하지만 본 분석은 일자별 집계 데이터를 기반으로 하여 시간대별 흐름을 파악하는 데 한계가 있습니다. 향후 시간대별 승하차 데이터를 통해 각 역의 특성을 더 자세히 분석할 필요가 있습니다. 특히 출퇴근 시간대(오전 7 - 9시, 오후 6 - 8시)와 점심 시간대(오후 12 - 1시)의 이동 패턴을 비교하면 각 지역의 특성 (업무 중심, 상업 중심, 주거 중심 등)을 더 명확히 파악할 수 있을 것입니다. 
