---
title: "[EDA & Visualization] 건강검진 데이터로 가설검정" 


categories: EDA&Visualization


tag: [python, Data_Science, Data, visualization, Preprocessing]
---

# 음주 여부에 따라 건강검진 수치 차이가 있을까? 


---
# 신장과 허리둘레의 크기는 체중과 상관관계가 있을까? 
- 분석을 통해 가설을 검정해 봅니다. 
---
# 라이브러리 로드


```python
# 분석에 사용할 pandas, 수치계산에 사용할 numpy, 시각화에 사용할 seaborn을 불러옵니다. 
# 또, 구 버전의 주피터 노트북에서는 %matplotlib inline 설정이 되어야 노트북 안에서 그래프를 시각화 합니다. 

import pandas as pd 
import numpy as np 
import seaborn as sns 
import matplotlib.pyplot as plt 

%matplotlib inline 
```

---
# 한글폰트 설정


```python
# # Google Colab 사용 시 아래 주석을 풀고 폰트설정을 합니다. 
# # 로컬 아나콘다 사용 시에는 그대로 주석처리 해놓으시면 됩니다. 
# # 나눔고딕 설치 
# # 이 코드를 사용시 아래에 있는 폰트를 로드할 경우 colab에서는 오류가 발생하니 
# # 아래에 있는 폰트 설정은 꼭 주석처리를 해주세요. 
# apt -qq -y install fonts-nanum > /dev/null 

# import matplotlib.font_manager as fm 

# fontpath = 'user/share/fonts/truetype/nanum/NanumBarunGothic.ttf'
# font = fm.FontProperties(fname=fontpath, size = 9)
# fm._rebuild()

# # Colab 의 한글 폰트 설정 
# plt.rc('font', family = 'NanumGothic')
# # 마이너스 폰트 깨지는 문제에 대한 대처 
# plt.rc("axes", unicode_minus = False)
```


```python
# 한글폰트를 설정해 주지 않으면 그래프 상에서 한글이 깨져보입니다. 
# 한글이 출력될 수 있도록 폰트 설정을 해줍니다. 
import matplotlib.pyplot as plt 
# 운영체제별 설정을 위해 로드 합니다. 
import os 

# 윈도우, 맥 이외의 OS는 별도로 설정해 주세요. 
if os.name == 'posix' : 
    plt.rc("font", family = "AppleGothic")
else : 
    plt.rc("font", family = "Malgun Gothic")

# 마이너스 폰트 깨지는 문제에 대한 대처 
plt.rc("axes", unicode_minus = False)
```


```python
# 레티나 설정을 해주면 글씨가 좀 더 선명하게 보입니다. 
# 폰트의 주변이 흐릿하게 보이는 것을 방지합니다. 
%config InlineBackend.figure_format = 'retina'
```

---

# 데이터 불러오기 
- 건강검진정보란 2002년부터 2013년까지의 국민건강보험의 직장가입자와 40세 이상의 피부양자, 세대주인 지역가입자와 40세 이상의 지역가입자의 일반건강검진 결과와 이들 일반건강검진대상자 중에 만 40세와 만 66세에 도달한 이들이 받게 되는 생애전환기건강진단 수검이력이 있는 각 연도별 수진자 100만 명에 대한 기본정보(성, 연령대, 시도코드 등)와 검진내역(신장, 체중, 총콜레스테롤, 혈색소 등)으로 구성된 개방데이터이다. 

[공공데이터 개방서비스](http://nhiss.nhis.or.kr/op/it/index.do)에서도 다운로드 받을 수 있음. 

- [건강검진정보 (2017) 다운로드 받기](https://www.data.go.kr/dataset/15007122/fileData.do)
- 2018년 데이터로 실습을 하셔도 됩니다. 다만 encoding과 컬럼명이 달라서 2018년 데이터에 맞게 고쳐주시면 됩니다. 
- 2018년 외 다른연도의 데이터로드 실습을 하고자 한다면 컬럼명과 인코딩에 주의해 주세요. 


```python
# 다운로드 받은 파일을 판다스의 read_csv 를 통해 읽어옵니다. 
# 파일을 읽어온 후 shape 로 행과 열의 수를 출력합니다. 
df = pd.read_csv("data/국민건강보험공단_건강검진정보_2023.CSV", encoding = 'cp949', low_memory = False)
df.shape
```




    (1000000, 33)



## 데이터 미리보기


```python
# sample, head, tail을 통해 데이터를 미리보기 합니다. 
df.sample()
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
      <th>기준년도</th>
      <th>가입자일련번호</th>
      <th>시도코드</th>
      <th>성별코드</th>
      <th>연령대코드(5세단위)</th>
      <th>신장(5cm단위)</th>
      <th>체중(5kg단위)</th>
      <th>허리둘레</th>
      <th>시력(좌)</th>
      <th>시력(우)</th>
      <th>...</th>
      <th>혈청지피티(ALT)</th>
      <th>감마지티피</th>
      <th>흡연상태</th>
      <th>음주여부</th>
      <th>구강검진수검여부</th>
      <th>치아우식증유무</th>
      <th>결손치 유무</th>
      <th>치아마모증유무</th>
      <th>제3대구치(사랑니) 이상</th>
      <th>치석</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>635799</th>
      <td>2023</td>
      <td>1088815</td>
      <td>48</td>
      <td>1</td>
      <td>5</td>
      <td>180</td>
      <td>100</td>
      <td>99.0</td>
      <td>1.5</td>
      <td>1.0</td>
      <td>...</td>
      <td>91.0</td>
      <td>59.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 33 columns</p>
</div>




```python
df.head()
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
      <th>기준년도</th>
      <th>가입자일련번호</th>
      <th>시도코드</th>
      <th>성별코드</th>
      <th>연령대코드(5세단위)</th>
      <th>신장(5cm단위)</th>
      <th>체중(5kg단위)</th>
      <th>허리둘레</th>
      <th>시력(좌)</th>
      <th>시력(우)</th>
      <th>...</th>
      <th>혈청지피티(ALT)</th>
      <th>감마지티피</th>
      <th>흡연상태</th>
      <th>음주여부</th>
      <th>구강검진수검여부</th>
      <th>치아우식증유무</th>
      <th>결손치 유무</th>
      <th>치아마모증유무</th>
      <th>제3대구치(사랑니) 이상</th>
      <th>치석</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2023</td>
      <td>34735</td>
      <td>46</td>
      <td>2</td>
      <td>9</td>
      <td>155</td>
      <td>70</td>
      <td>92.0</td>
      <td>1.2</td>
      <td>1.2</td>
      <td>...</td>
      <td>24.0</td>
      <td>50.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2023</td>
      <td>4105118</td>
      <td>11</td>
      <td>1</td>
      <td>17</td>
      <td>160</td>
      <td>55</td>
      <td>86.0</td>
      <td>0.9</td>
      <td>9.9</td>
      <td>...</td>
      <td>11.0</td>
      <td>31.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2023</td>
      <td>362482</td>
      <td>36</td>
      <td>2</td>
      <td>13</td>
      <td>150</td>
      <td>65</td>
      <td>96.0</td>
      <td>1.0</td>
      <td>0.8</td>
      <td>...</td>
      <td>29.0</td>
      <td>24.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2023</td>
      <td>653166</td>
      <td>11</td>
      <td>1</td>
      <td>13</td>
      <td>160</td>
      <td>70</td>
      <td>85.0</td>
      <td>1.0</td>
      <td>1.2</td>
      <td>...</td>
      <td>21.0</td>
      <td>27.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>1</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2023</td>
      <td>4152237</td>
      <td>41</td>
      <td>1</td>
      <td>12</td>
      <td>165</td>
      <td>65</td>
      <td>84.5</td>
      <td>1.0</td>
      <td>1.2</td>
      <td>...</td>
      <td>33.0</td>
      <td>49.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 33 columns</p>
</div>




```python
df.tail()
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
      <th>기준년도</th>
      <th>가입자일련번호</th>
      <th>시도코드</th>
      <th>성별코드</th>
      <th>연령대코드(5세단위)</th>
      <th>신장(5cm단위)</th>
      <th>체중(5kg단위)</th>
      <th>허리둘레</th>
      <th>시력(좌)</th>
      <th>시력(우)</th>
      <th>...</th>
      <th>혈청지피티(ALT)</th>
      <th>감마지티피</th>
      <th>흡연상태</th>
      <th>음주여부</th>
      <th>구강검진수검여부</th>
      <th>치아우식증유무</th>
      <th>결손치 유무</th>
      <th>치아마모증유무</th>
      <th>제3대구치(사랑니) 이상</th>
      <th>치석</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>999995</th>
      <td>2023</td>
      <td>3265034</td>
      <td>26</td>
      <td>1</td>
      <td>8</td>
      <td>170</td>
      <td>65</td>
      <td>78.0</td>
      <td>1.2</td>
      <td>1.0</td>
      <td>...</td>
      <td>13.0</td>
      <td>22.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>999996</th>
      <td>2023</td>
      <td>1421865</td>
      <td>41</td>
      <td>1</td>
      <td>10</td>
      <td>165</td>
      <td>80</td>
      <td>96.1</td>
      <td>0.9</td>
      <td>1.2</td>
      <td>...</td>
      <td>65.0</td>
      <td>160.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>999997</th>
      <td>2023</td>
      <td>3889375</td>
      <td>41</td>
      <td>2</td>
      <td>11</td>
      <td>155</td>
      <td>65</td>
      <td>87.0</td>
      <td>0.5</td>
      <td>0.7</td>
      <td>...</td>
      <td>26.0</td>
      <td>25.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>999998</th>
      <td>2023</td>
      <td>2618086</td>
      <td>41</td>
      <td>2</td>
      <td>7</td>
      <td>160</td>
      <td>55</td>
      <td>69.0</td>
      <td>1.0</td>
      <td>1.2</td>
      <td>...</td>
      <td>20.0</td>
      <td>16.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>999999</th>
      <td>2023</td>
      <td>1279122</td>
      <td>31</td>
      <td>2</td>
      <td>12</td>
      <td>160</td>
      <td>50</td>
      <td>75.0</td>
      <td>0.8</td>
      <td>0.7</td>
      <td>...</td>
      <td>21.0</td>
      <td>34.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 33 columns</p>
</div>



---

# 기본정보 보기 


```python
# info 를 통해 데이터의 크기, 형식, 메모리 사용량 등을 봅니다. 
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1000000 entries, 0 to 999999
    Data columns (total 33 columns):
     #   Column         Non-Null Count    Dtype  
    ---  ------         --------------    -----  
     0   기준년도           1000000 non-null  int64  
     1   가입자일련번호        1000000 non-null  int64  
     2   시도코드           1000000 non-null  int64  
     3   성별코드           1000000 non-null  int64  
     4   연령대코드(5세단위)    1000000 non-null  int64  
     5   신장(5cm단위)      1000000 non-null  int64  
     6   체중(5kg단위)      1000000 non-null  int64  
     7   허리둘레           999589 non-null   float64
     8   시력(좌)          999816 non-null   float64
     9   시력(우)          999823 non-null   float64
     10  청력(좌)          999863 non-null   float64
     11  청력(우)          999862 non-null   float64
     12  수축기혈압          994253 non-null   float64
     13  이완기혈압          994253 non-null   float64
     14  식전혈당(공복혈당)     994186 non-null   float64
     15  총콜레스테롤         338606 non-null   float64
     16  트리글리세라이드       338606 non-null   float64
     17  HDL콜레스테롤       338606 non-null   float64
     18  LDL콜레스테롤       332753 non-null   float64
     19  혈색소            994183 non-null   float64
     20  요단백            988844 non-null   float64
     21  혈청크레아티닌        994186 non-null   float64
     22  혈청지오티(AST)     994184 non-null   float64
     23  혈청지피티(ALT)     994184 non-null   float64
     24  감마지티피          994187 non-null   float64
     25  흡연상태           999911 non-null   float64
     26  음주여부           999944 non-null   float64
     27  구강검진수검여부       1000000 non-null  int64  
     28  치아우식증유무        346848 non-null   float64
     29  결손치 유무         0 non-null        float64
     30  치아마모증유무        0 non-null        float64
     31  제3대구치(사랑니) 이상  0 non-null        float64
     32  치석             346848 non-null   float64
    dtypes: float64(25), int64(8)
    memory usage: 251.8 MB
    


```python
# 컬럼의 수가 많습니다. 컬럼만 따로 출력합니다. 
df.columns
```




    Index(['기준년도', '가입자일련번호', '시도코드', '성별코드', '연령대코드(5세단위)', '신장(5cm단위)',
           '체중(5kg단위)', '허리둘레', '시력(좌)', '시력(우)', '청력(좌)', '청력(우)', '수축기혈압',
           '이완기혈압', '식전혈당(공복혈당)', '총콜레스테롤', '트리글리세라이드', 'HDL콜레스테롤', 'LDL콜레스테롤',
           '혈색소', '요단백', '혈청크레아티닌', '혈청지오티(AST)', '혈청지피티(ALT)', '감마지티피', '흡연상태',
           '음주여부', '구강검진수검여부', '치아우식증유무', '결손치 유무', '치아마모증유무', '제3대구치(사랑니) 이상',
           '치석'],
          dtype='object')




```python
# dtypes 를 통해 데이터 형식만 출력합니다. 
df.dtypes
```




    기준년도               int64
    가입자일련번호            int64
    시도코드               int64
    성별코드               int64
    연령대코드(5세단위)        int64
    신장(5cm단위)          int64
    체중(5kg단위)          int64
    허리둘레             float64
    시력(좌)            float64
    시력(우)            float64
    청력(좌)            float64
    청력(우)            float64
    수축기혈압            float64
    이완기혈압            float64
    식전혈당(공복혈당)       float64
    총콜레스테롤           float64
    트리글리세라이드         float64
    HDL콜레스테롤         float64
    LDL콜레스테롤         float64
    혈색소              float64
    요단백              float64
    혈청크레아티닌          float64
    혈청지오티(AST)       float64
    혈청지피티(ALT)       float64
    감마지티피            float64
    흡연상태             float64
    음주여부             float64
    구강검진수검여부           int64
    치아우식증유무          float64
    결손치 유무           float64
    치아마모증유무          float64
    제3대구치(사랑니) 이상    float64
    치석               float64
    dtype: object



---

# 결측치 보기 


```python
# isnull 을 통해 결측치를 bool 값으로 표시하고 sum을 하면 컬럼마다의 결측치 수를 세어줍니다. 
df.isnull().sum()
```




    기준년도                   0
    가입자일련번호                0
    시도코드                   0
    성별코드                   0
    연령대코드(5세단위)            0
    신장(5cm단위)              0
    체중(5kg단위)              0
    허리둘레                 411
    시력(좌)                184
    시력(우)                177
    청력(좌)                137
    청력(우)                138
    수축기혈압               5747
    이완기혈압               5747
    식전혈당(공복혈당)          5814
    총콜레스테롤            661394
    트리글리세라이드          661394
    HDL콜레스테롤          661394
    LDL콜레스테롤          667247
    혈색소                 5817
    요단백                11156
    혈청크레아티닌             5814
    혈청지오티(AST)          5816
    혈청지피티(ALT)          5816
    감마지티피               5813
    흡연상태                  89
    음주여부                  56
    구강검진수검여부               0
    치아우식증유무           653152
    결손치 유무           1000000
    치아마모증유무          1000000
    제3대구치(사랑니) 이상    1000000
    치석                653152
    dtype: int64




```python
# isna로도 결측치 여부를 확인하고 sum을 통해 결측치 수를 집계할 수 있습니다.
null_count = df.isna().sum()
null_count
```




    기준년도                   0
    가입자일련번호                0
    시도코드                   0
    성별코드                   0
    연령대코드(5세단위)            0
    신장(5cm단위)              0
    체중(5kg단위)              0
    허리둘레                 411
    시력(좌)                184
    시력(우)                177
    청력(좌)                137
    청력(우)                138
    수축기혈압               5747
    이완기혈압               5747
    식전혈당(공복혈당)          5814
    총콜레스테롤            661394
    트리글리세라이드          661394
    HDL콜레스테롤          661394
    LDL콜레스테롤          667247
    혈색소                 5817
    요단백                11156
    혈청크레아티닌             5814
    혈청지오티(AST)          5816
    혈청지피티(ALT)          5816
    감마지티피               5813
    흡연상태                  89
    음주여부                  56
    구강검진수검여부               0
    치아우식증유무           653152
    결손치 유무           1000000
    치아마모증유무          1000000
    제3대구치(사랑니) 이상    1000000
    치석                653152
    dtype: int64




```python
# 판다스에 내장 된 plot을 통해 시각화를 합니다. 

null_count.plot.barh(figsize = (10, 9))
```




    <AxesSubplot:>




    
![png](/assets/images/EDA&Visualization/second/output_19_1.png)
    


---

# 일부 데이터 요약하기 


```python
# 여러 컬럼을 가져옵니다. 
# "혈청지피티(ALT)", "혈청지오티(AST)" 를 가져와 미리보기 합니다. 
df[["혈청지피티(ALT)", "혈청지오티(AST)"]].head()
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
      <th>혈청지피티(ALT)</th>
      <th>혈청지오티(AST)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>24.0</td>
      <td>26.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>11.0</td>
      <td>22.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>29.0</td>
      <td>22.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21.0</td>
      <td>22.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>33.0</td>
      <td>23.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# "혈청지피티(ALT)", "혈청지오티(AST)" 를 요약합니다. 
df[["혈청지피티(ALT)", "혈청지오티(AST)"]].describe()
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
      <th>혈청지피티(ALT)</th>
      <th>혈청지오티(AST)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>994184.000000</td>
      <td>994184.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>26.363751</td>
      <td>26.841220</td>
    </tr>
    <tr>
      <th>std</th>
      <td>26.100922</td>
      <td>20.909722</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>15.000000</td>
      <td>19.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>21.000000</td>
      <td>23.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>30.000000</td>
      <td>29.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>6297.000000</td>
      <td>5097.000000</td>
    </tr>
  </tbody>
</table>
</div>



---

# value_counts 로 값 집계하기 


```python
df.columns
```




    Index(['기준년도', '가입자일련번호', '시도코드', '성별코드', '연령대코드(5세단위)', '신장(5cm단위)',
           '체중(5kg단위)', '허리둘레', '시력(좌)', '시력(우)', '청력(좌)', '청력(우)', '수축기혈압',
           '이완기혈압', '식전혈당(공복혈당)', '총콜레스테롤', '트리글리세라이드', 'HDL콜레스테롤', 'LDL콜레스테롤',
           '혈색소', '요단백', '혈청크레아티닌', '혈청지오티(AST)', '혈청지피티(ALT)', '감마지티피', '흡연상태',
           '음주여부', '구강검진수검여부', '치아우식증유무', '결손치 유무', '치아마모증유무', '제3대구치(사랑니) 이상',
           '치석'],
          dtype='object')




```python
# value_counts 를 통해 성별코드로 그룹화 하고 갯수를 집계합니다. 
df["성별코드"].value_counts()
```




    1    515035
    2    484965
    Name: 성별코드, dtype: int64




```python
# value_counts 를 통해 흡연상태로 그룹화 하고 갯수를 집계합니다. 
df["흡연상태"].value_counts()
```




    1.0    642875
    3.0    186491
    2.0    170545
    Name: 흡연상태, dtype: int64



---

# groupby 와 pivot_table 사용하기 
## groupby


```python
df.columns 
```




    Index(['기준년도', '가입자일련번호', '시도코드', '성별코드', '연령대코드(5세단위)', '신장(5cm단위)',
           '체중(5kg단위)', '허리둘레', '시력(좌)', '시력(우)', '청력(좌)', '청력(우)', '수축기혈압',
           '이완기혈압', '식전혈당(공복혈당)', '총콜레스테롤', '트리글리세라이드', 'HDL콜레스테롤', 'LDL콜레스테롤',
           '혈색소', '요단백', '혈청크레아티닌', '혈청지오티(AST)', '혈청지피티(ALT)', '감마지티피', '흡연상태',
           '음주여부', '구강검진수검여부', '치아우식증유무', '결손치 유무', '치아마모증유무', '제3대구치(사랑니) 이상',
           '치석'],
          dtype='object')




```python
# groupby를 통해 데이터를 그룹화 합니다. 
# 성별코드로 그룹화 한 데이터를 세어봅니다. 
df.groupby(["성별코드"])["가입자일련번호"].count()
```




    성별코드
    1    515035
    2    484965
    Name: 가입자일련번호, dtype: int64




```python
# 성별코드와 음주여부로 그룹화를 하고 갯수를 세어봅니다. 
df.groupby(["성별코드", "음주여부"])["가입자일련번호"].count()
```




    성별코드  음주여부
    1     0.0     117268
          1.0     397742
    2     0.0     222901
          1.0     262033
    Name: 가입자일련번호, dtype: int64




```python
# 성별코드와 음주여부로 그룹화를 하고 감마지티피의 평균을 구합니다. 
df.groupby(["성별코드", "음주여부"])["감마지티피"].mean()
```




    성별코드  음주여부
    1     0.0     33.458433
          1.0     50.209221
    2     0.0     23.303326
          1.0     23.895202
    Name: 감마지티피, dtype: float64




```python
# 성별코드와 음주여부로 그룹화를 하고 감마지티피의 요약수치를 구합니다. 
df.groupby(["성별코드", "음주여부"])["감마지티피"].describe()
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
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
    <tr>
      <th>성별코드</th>
      <th>음주여부</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">1</th>
      <th>0.0</th>
      <td>115860.0</td>
      <td>33.458433</td>
      <td>39.937340</td>
      <td>1.0</td>
      <td>18.0</td>
      <td>25.0</td>
      <td>36.0</td>
      <td>2951.0</td>
    </tr>
    <tr>
      <th>1.0</th>
      <td>396838.0</td>
      <td>50.209221</td>
      <td>86.278357</td>
      <td>1.0</td>
      <td>21.0</td>
      <td>32.0</td>
      <td>54.0</td>
      <td>9999.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">2</th>
      <th>0.0</th>
      <td>219912.0</td>
      <td>23.303326</td>
      <td>34.686439</td>
      <td>1.0</td>
      <td>13.0</td>
      <td>17.0</td>
      <td>25.0</td>
      <td>9999.0</td>
    </tr>
    <tr>
      <th>1.0</th>
      <td>261522.0</td>
      <td>23.895202</td>
      <td>34.875537</td>
      <td>1.0</td>
      <td>13.0</td>
      <td>17.0</td>
      <td>24.0</td>
      <td>3086.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# agg을 사용하면 여러 수치를 함께 구할 수 있습니다. 
df.groupby(["성별코드", "음주여부"])["감마지티피"].agg(["count", "mean", "median"])
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
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>median</th>
    </tr>
    <tr>
      <th>성별코드</th>
      <th>음주여부</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">1</th>
      <th>0.0</th>
      <td>115860</td>
      <td>33.458433</td>
      <td>25.0</td>
    </tr>
    <tr>
      <th>1.0</th>
      <td>396838</td>
      <td>50.209221</td>
      <td>32.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">2</th>
      <th>0.0</th>
      <td>219912</td>
      <td>23.303326</td>
      <td>17.0</td>
    </tr>
    <tr>
      <th>1.0</th>
      <td>261522</td>
      <td>23.895202</td>
      <td>17.0</td>
    </tr>
  </tbody>
</table>
</div>



## pivot_table
- [Pandas-pivot_table](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.pivot_table.html)
- pivot : 연산을 하지 않고 데이터의 형태를 보고자 할 때 사용 
- pivot_table : 데이터의 연산을 하고자 할 때 사용 


```python
# 음주여부에 따른 그룹화된 수를 피봇테이블로 구합니다. 
df.pivot_table(index = "음주여부", values = "가입자일련번호", aggfunc = "count")
# pivot_table은 데이터프레임 형태로 반환 
# groupby 는 시리즈 형태로 반환
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
      <th>가입자일련번호</th>
    </tr>
    <tr>
      <th>음주여부</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0.0</th>
      <td>340169</td>
    </tr>
    <tr>
      <th>1.0</th>
      <td>659775</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 음주여부에 따른 감마지티피의 평균을 구합니다. 
pd.pivot_table(df, index = "음주여부", values = "감마지티피")
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
      <th>감마지티피</th>
    </tr>
    <tr>
      <th>음주여부</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0.0</th>
      <td>26.807402</td>
    </tr>
    <tr>
      <th>1.0</th>
      <td>39.756437</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 기본값은 평균을 구하지만 aggfunc을 통해 지정해 줄 수도 있습니다. 
pd.pivot_table(df, index = "음주여부", values = "감마지티피", aggfunc = "mean")
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
      <th>감마지티피</th>
    </tr>
    <tr>
      <th>음주여부</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0.0</th>
      <td>26.807402</td>
    </tr>
    <tr>
      <th>1.0</th>
      <td>39.756437</td>
    </tr>
  </tbody>
</table>
</div>




```python
# aggfunc에 여러 값을 한번에 지정할 수도 있습니다. 
pd.pivot_table(df, index = "음주여부", values = "감마지티피", aggfunc = ["mean", "median"])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>mean</th>
      <th>median</th>
    </tr>
    <tr>
      <th></th>
      <th>감마지티피</th>
      <th>감마지티피</th>
    </tr>
    <tr>
      <th>음주여부</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0.0</th>
      <td>26.807402</td>
      <td>19.0</td>
    </tr>
    <tr>
      <th>1.0</th>
      <td>39.756437</td>
      <td>25.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# aggfunc에 describe를 사용해 통계요약값을 한번에 볼 수도 있습니다. 
pd.pivot_table(df, index = "음주여부", values = "감마지티피", aggfunc = "describe")
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
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>count</th>
      <th>max</th>
      <th>mean</th>
      <th>min</th>
      <th>std</th>
    </tr>
    <tr>
      <th>음주여부</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0.0</th>
      <td>14.0</td>
      <td>19.0</td>
      <td>29.0</td>
      <td>335772.0</td>
      <td>9999.0</td>
      <td>26.807402</td>
      <td>1.0</td>
      <td>36.900647</td>
    </tr>
    <tr>
      <th>1.0</th>
      <td>16.0</td>
      <td>25.0</td>
      <td>42.0</td>
      <td>658360.0</td>
      <td>9999.0</td>
      <td>39.756437</td>
      <td>1.0</td>
      <td>71.665318</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 성별코드, 음주여부에 따른 감마지티피 값의 평균을 구합니다. 
pd.pivot_table(df, index = ["성별코드", "음주여부"], values = "감마지티피", aggfunc = "mean")
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
      <th></th>
      <th>감마지티피</th>
    </tr>
    <tr>
      <th>성별코드</th>
      <th>음주여부</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">1</th>
      <th>0.0</th>
      <td>33.458433</td>
    </tr>
    <tr>
      <th>1.0</th>
      <td>50.209221</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">2</th>
      <th>0.0</th>
      <td>23.303326</td>
    </tr>
    <tr>
      <th>1.0</th>
      <td>23.895202</td>
    </tr>
  </tbody>
</table>
</div>



- groupby로 할 수 있는 작업은 pivot_table로도 대부분 수행 가능합니다. 
- groupby의 장점 : 빠르다
- pivot_table의 장점 : 직관적인 사용법을 가지고 있다. 

---

# 전체 데이터 시각화 하기 
- 100만개가 넘는 데이터를 시각화할 때는 되도록이면 groupby 혹은 pivot_table로 연산을 하고 시각화를 하는 것을 권장합니다. 
- 100만개가 넘는 데이터를 seaborn과 같은 고급 통계 연산을 하는 그래프를 사용하게 되면 많이 느릴 수 있습니다. 

## 히스토그램 
- 판다스의 info 기능을 통해 대부분 수치 데이터로 이루어 진것을 확인할 수 있었습니다. 
- 히스토그램을 사용하면 수치데이터를 bin의 갯수만큼 그룹화 해서 도수분포표를 만들고 그 결과를 시각화 합니다. 
- 이 데이터에는 수치데이터가 많기 때문에 판다스의 hist를 사용해서 히스토그램을 그립니다. 


```python
# 전체 데이터에 대한 히스토그램을 출력합니다. 
h = df.hist(figsize = (12, 12))
```


    
![png](/assets/images/EDA&Visualization/second/output_43_0.png)
    


## 슬라이싱을 사용해 히스토그램 그리기
- 슬라이싱 기능을 사용해서 데이터를 나누어 그립니다. 
- 슬라이싱 사용시 iloc를 활용하면 인덱스의 순서대로 슬라이싱이 가능합니다. 
- iloc[행, 열]순으로 인덱스를 써주면 해당 인덱스만 불러오며, 전체 데이터를 가져오고자 할 때는 [:, :]을 사용합니다. 
- 슬라이싱을 해주는 대괄호 안의 콜론 앞뒤에 숫자를 써주게 되면 해당 시작인덱스 : 끝나는인덱스(+1)를 지정할 수 있습니다. 


```python
# 슬라이싱을 사용해 앞에서 12개 컬럼에 대한 데이터로 히스토그램을 그립니다. 
# [행, 열]
h = df.iloc[:, :12].hist(figsize = (12, 12))
```


    
![png](/assets/images/EDA&Visualization/second/output_45_0.png)
    



```python
# 슬라이싱을 사용해 앞에서 12번째부터 23번째까지 (12:24) 컬럼에 대한 데이터로 히스토그램을 그립니다. 
# bins = 막대를 몇 개를 그릴지 설정해주는 값
h = df.iloc[:, 12:24].hist(figsize = (12, 12), bins = 100)
```


    
![png](/assets/images/EDA&Visualization/second/output_46_0.png)
    



```python
# 슬라이싱을 사용해 앞에서 24번째부터 마지막까지 (24:) 컬럼에 대한 데이터로 히스토그램을 그립니다. 
h = df.iloc[:, 24:].hist(figsize = (12, 12), bins = 10)
```


    
![png](/assets/images/EDA&Visualization/second/output_47_0.png)
    


---

# 샘플데이터 추출하기 
- seaborn 의 그래프는 내부에서 수학적 연산이 되기 때문에 데이터가 많으면 속도가 오래 걸립니다. 
- 따라서 전체 데이터를 사용하면 너무 느리기 때문에 일부만 샘플링해서 사용합니다. 


```python
# df.sample을 통해 일부 데이터만 샘플데이터를 추출합니다. 
# random_state 를 사용해 샘플링되는 값을 고정할 수 있습니다. 
# 실험을 통제하기 위해 random_state를 고정하기도 합니다, 
# 여기에서는 1을 사용하겠습니다. 이 값은 높든 낮든 상관 없이 값을 고정시키는 역할만 합니다. 

df_sample = df.sample(1000, random_state = 1)
df_sample.shape 
```




    (1000, 33)



---

# 데이터 시각화 도구 Seaborn 사용하기 
- [Seaborn](https://seaborn.pydata.org/)
- seaborn은 [Matplotlib](https://matplotlib.org/)을 사용하기 쉽게 만들어 졌으며, 간단하게 고급 통계 연산을 할 수 있습니다. 

---

# 범주형(카테고리) 데이터 시각화 
- countplot은 범주형 데이터의 수를 더한 값을 그래프로 표현합니다. 
- value_counts로 구한 값을 시각화 한다고 보면 됩니다. 
## countplot - 음주여부 


```python
# 음주여부에 따른 countplot을 그립니다. 
df["음주여부"].value_counts().plot.bar()
```




    <AxesSubplot:>




    
![png](/assets/images/EDA&Visualization/second/output_51_1.png)
    



```python
sns.countplot(x = "음주여부", data = df)
```




    <AxesSubplot:xlabel='음주여부', ylabel='count'>




    
![png](/assets/images/EDA&Visualization/second/output_52_1.png)
    


## hue 옵션 사용하기


```python
# 음주여부에 따른 countplot을 그리고 hue 를 사용해 성별코드로 색상을 구분해 그립니다. 
# 여기에서 hue는 포토샵에 있는 hue 메뉴를 떠올리면 됩니다. 색상을 의미합니다. 
# 또, seaborn 에서 제공하는 폰트 설정을 사용하실 수도 있습니다. 
# 다만, 이 때 seaborn 의 기본 스타일이 적용되는 것을 확인해 주시는 것이 좋습니다. 
# MAC
# sns.set(font_scale = 1.5, font = "AppleGothic")
# Window
# sns.set(font_scale = 1.5, font = "Malgun Gothic")

sns.set(font_scale = 1, font = "Malgun Gothic")
sns.countplot(data = df, x = "음주여부", hue = "성별코드")
```




    <AxesSubplot:xlabel='음주여부', ylabel='count'>




    
![png](/assets/images/EDA&Visualization/second/output_54_1.png)
    



```python
# countplot으로 연령대별 음주여부를 봅니다. 
# hue를 사용해 다른 색상으로 표현합니다. 
sns.countplot(data = df, x = "연령대코드(5세단위)", hue = "음주여부")
```




    <AxesSubplot:xlabel='연령대코드(5세단위)', ylabel='count'>




    
![png](/assets/images/EDA&Visualization/second/output_55_1.png)
    


## countplot - 키와 몸무게 
- 키와 몸무게는 연속형 데이터라고 볼 수 있습니다. 
- 하지만 이 데이터는 키는 5cm, 체중은 5kg 단위로 되어 있습니다. 
- 이렇게 특정 범위로 묶게 되면 연속형 데이터라기 보다는 범주형 데이터라고 불 수 있습니다. 


```python
# countplot으로 키를 봅니다. 
plt.figure(figsize = (15, 4))
sns.countplot(data = df, x = "신장(5cm단위)")
```




    <AxesSubplot:xlabel='신장(5cm단위)', ylabel='count'>




    
![png](/assets/images/EDA&Visualization/second/output_57_1.png)
    



```python
# countplot으로 체중을 봅니다. 
plt.figure(figsize = (15, 4))
sns.countplot(data = df, x = "체중(5kg단위)")
```




    <AxesSubplot:xlabel='체중(5kg단위)', ylabel='count'>




    
![png](/assets/images/EDA&Visualization/second/output_58_1.png)
    



```python
# countplot으로 신장 (5cm단위)를 봅니다. 
# 성별에 따른 키의 차이를 봅니다. 

plt.figure(figsize = (15, 4))
sns.countplot(data = df, x = "신장(5cm단위)", hue = "성별코드")
```




    <AxesSubplot:xlabel='신장(5cm단위)', ylabel='count'>




    
![png](/assets/images/EDA&Visualization/second/output_59_1.png)
    



```python
# 성별에 따른 체중의 차이를 봅니다. 

plt.figure(figsize = (15, 4))
sns.countplot(data = df, x = "체중(5kg단위)", hue = "성별코드")
```




    <AxesSubplot:xlabel='체중(5kg단위)', ylabel='count'>




    
![png](/assets/images/EDA&Visualization/second/output_60_1.png)
    


## barplot - 수치형 vs 범주형 데이터 시각화 


```python
# 연령대코드와 총 콜레스테롤을 봅니다. 
# hue 로 색상을 다르게 표현할 수 있습니다. 음주여부를 함께 봅니다. 

plt.figure(figsize = (15, 4))
sns.barplot(data = df, x = "연령대코드(5세단위)", y = "총콜레스테롤", hue = "음주여부")
```




    <AxesSubplot:xlabel='연령대코드(5세단위)', ylabel='총콜레스테롤'>




    
![png](/assets/images/EDA&Visualization/second/output_62_1.png)
    



```python
# 연령대코드와 총 콜레스테롤을 봅니다. 
# 콜레스테롤과 연령대코드(5세단위)를 흡연상태에 따라 barplot으로 그립니다. 
plt.figure(figsize = (15, 4))
sns.barplot(data = df, x = "연령대코드(5세단위)", y = "총콜레스테롤", hue = "흡연상태")
```




    <AxesSubplot:xlabel='연령대코드(5세단위)', ylabel='총콜레스테롤'>




    
![png](/assets/images/EDA&Visualization/second/output_63_1.png)
    



```python
# 트리글리세라이드(중성지방)에 따른 연령대코드(5세단위)를 음주여부에 따라 barplot으로 그립니다.
# ci = 신뢰구간 (default = 95%)
plt.figure(figsize = (15, 4))
sns.barplot(data = df, x = "연령대코드(5세단위)", y = "트리글리세라이드", hue = "음주여부")
```




    <AxesSubplot:xlabel='연령대코드(5세단위)', ylabel='트리글리세라이드'>




    
![png](/assets/images/EDA&Visualization/second/output_64_1.png)
    



```python
# 연령대코드(5세단위)와 체중(5kg 단위)를 성별에 따라 봅니다. 
plt.figure(figsize = (15, 4))
sns.barplot(data = df, x = "연령대코드(5세단위)", y = "체중(5kg단위)", hue = "성별코드")
```




    <AxesSubplot:xlabel='연령대코드(5세단위)', ylabel='체중(5kg단위)'>




    
![png](/assets/images/EDA&Visualization/second/output_65_1.png)
    



```python
# 연령대코드(5세단위) 에 따른 체중(5kg 단위)을 음주여부에 따라 barplot으로 그립니다. 
plt.figure(figsize = (15, 4))
sns.barplot(data = df, x = "연령대코드(5세단위)", y = "체중(5kg단위)", hue = "음주여부")
```




    <AxesSubplot:xlabel='연령대코드(5세단위)', ylabel='체중(5kg단위)'>




    
![png](/assets/images/EDA&Visualization/second/output_66_1.png)
    


## lineplot and pointplot 


```python
# 연령대코드(5세단위)에 따른 체중(5kg 단위) 을 성별코드에 따라 lineplot으로 그립니다. 
plt.figure(figsize = (15, 4))
sns.lineplot(data = df, x = "연령대코드(5세단위)", y = "체중(5kg단위)", hue = "성별코드", errorbar = "sd")
```




    <AxesSubplot:xlabel='연령대코드(5세단위)', ylabel='체중(5kg단위)'>




    
![png](/assets/images/EDA&Visualization/second/output_68_1.png)
    



```python
# 연령대코드(5세단위)에 따른 신장(5cm 단위) 을 성별코드에 따라 lineplot으로 그립니다. 
plt.figure(figsize = (15, 4))
sns.lineplot(data = df, x = "연령대코드(5세단위)", y = "신장(5cm단위)", hue = "성별코드", errorbar = "sd")
```




    <AxesSubplot:xlabel='연령대코드(5세단위)', ylabel='신장(5cm단위)'>




    
![png](/assets/images/EDA&Visualization/second/output_69_1.png)
    



```python
# 연령대코드(5세단위)에 따른 체중(5kg 단위) 을 음주여부에 따라 pointplot과 barplot으로 그립니다.
plt.figure(figsize = (15, 4))
sns.barplot(data = df, x = "연령대코드(5세단위)", y = "체중(5kg단위)", hue = "음주여부")
sns.pointplot(data = df, x = "연령대코드(5세단위)", y = "체중(5kg단위)", hue = "음주여부")
```




    <AxesSubplot:xlabel='연령대코드(5세단위)', ylabel='체중(5kg단위)'>




    
![png](/assets/images/EDA&Visualization/second/output_70_1.png)
    



```python
# 연령대코드(5세단위)에 따른 신장(5cm 단위) 을 성별코드에 따라 pointplot으로 그립니다. 
plt.figure(figsize = (15, 4))
sns.pointplot(data = df, x = "연령대코드(5세단위)", y = "신장(5cm단위)", hue = "성별코드", errorbar = "sd")
```




    <AxesSubplot:xlabel='연령대코드(5세단위)', ylabel='신장(5cm단위)'>




    
![png](/assets/images/EDA&Visualization/second/output_71_1.png)
    



```python
# 연령대코드(5세단위)에 따른 혈색소를 음주여부에 따라 lineplot으로 그립니다. 
plt.figure(figsize = (15, 4))
sns.lineplot(data = df, x = "연령대코드(5세단위)", y = "혈색소", hue = "음주여부")
```




    <AxesSubplot:xlabel='연령대코드(5세단위)', ylabel='혈색소'>




    
![png](/assets/images/EDA&Visualization/second/output_72_1.png)
    


## boxplot
- [pandas](https://pandas.pydata.org/pandas-docs/stable/user_guide/visualization.html)
- [pandas_DataFrame_boxplot](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.boxplot.html)
- [상자 수염 그림 - 위키백과, 우리 모두의 백과사전](https://ko.wikipedia.org/wiki/상자_수염_그림#/media/파일:Michelsonmorley-boxplot.svg)
- 가공하지 않은 자료 그대로를 이용하여 그린 것이 아니라, 자료로부터 얻어낸 통계량인 5가지 요약 수치로 그린다. 
- 5가지 요약 수치란 기술통계학에서 자료의 정보를 알려주는 아래의 다섯 가지 수치를 의미한다. 

1. 최솟값 
2. 제 1사분위수 
3. 제 2사분위수(), 즉 중앙값 
4. 제 3사분위수()
5. 최댓값 
6. Box plot 이해하기 : 
    - 박스 플롯에 대하여 ::-[|]- Box and Whisker 
    - UnderstandingBoxplots - Towards Data Science 


```python
# boxxplot으로 신장(5cm단위) 에 따른 체중(5kg단위) 을 그리며, 성별코드에 따라 다른 색상으로 표현되게 됩니다. 
plt.figure(figsize = (15, 4))
sns.boxplot(data = df, x = "신장(5cm단위)", y = "체중(5kg단위)", hue = "성별코드")
```




    <AxesSubplot:xlabel='신장(5cm단위)', ylabel='체중(5kg단위)'>




    
![png](/assets/images/EDA&Visualization/second/output_74_1.png)
    


## violinplot 


```python
# violinplot 신장(5cm단위) 에 따른 체중(5kg단위) 을 그리며, 음주여부에 따라 다른 색상으로 표현되게 합니다. 
plt.figure(figsize = (15, 4))
sns.violinplot(data = df_sample, x = "신장(5cm단위)", y = "체중(5kg단위)", hue = "음주여부")
```




    <AxesSubplot:xlabel='신장(5cm단위)', ylabel='체중(5kg단위)'>




    
![png](/assets/images/EDA&Visualization/second/output_76_1.png)
    



```python
# violinplot의 split 기능을 사용해 봅니다. 
plt.figure(figsize = (15, 4))
sns.violinplot(data = df_sample, x = "신장(5cm단위)", y = "체중(5kg단위)", hue = "음주여부", split = True)
```




    <AxesSubplot:xlabel='신장(5cm단위)', ylabel='체중(5kg단위)'>




    
![png](/assets/images/EDA&Visualization/second/output_77_1.png)
    



```python
# violinplot 연령대코드 (5세단위)에 따른 혈색소를 그리며, 음주여부에 따라 다른 색상으로 표현되게 합니다. 
plt.figure(figsize = (15, 4))
sns.violinplot(data = df_sample, x = "연령대코드(5세단위)", y = "혈색소", hue = "음주여부", split = True)
```




    <AxesSubplot:xlabel='연령대코드(5세단위)', ylabel='혈색소'>




    
![png](/assets/images/EDA&Visualization/second/output_78_1.png)
    


## swarm plot 
- 범주형 데이터를 산점도로 시각화하고자 할 때 사용합니다. 


```python
# swarmplot으로 신장(5cm단위)에 따른 체중(5kg단위) 을 그리며, 음주여부에 따라 다른 색상으로 표현되게 합니다. 
# 점을 하나씩 찍기 때문에 오래 걸리는 코드는 전체로 그려보기 전에 일부만 가져와 그려봅니다. 
plt.figure(figsize = (15, 4))
sns.swarmplot(data = df_sample, x = "신장(5cm단위)", y = "체중(5kg단위)", hue = "음주여부")
```

    C:\Users\jihun\anaconda3\lib\site-packages\seaborn\categorical.py:3399: UserWarning: 36.5% of the points cannot be placed; you may want to decrease the size of the markers or use stripplot.
      warnings.warn(msg, UserWarning)
    C:\Users\jihun\anaconda3\lib\site-packages\seaborn\categorical.py:3399: UserWarning: 52.3% of the points cannot be placed; you may want to decrease the size of the markers or use stripplot.
      warnings.warn(msg, UserWarning)
    C:\Users\jihun\anaconda3\lib\site-packages\seaborn\categorical.py:3399: UserWarning: 53.1% of the points cannot be placed; you may want to decrease the size of the markers or use stripplot.
      warnings.warn(msg, UserWarning)
    C:\Users\jihun\anaconda3\lib\site-packages\seaborn\categorical.py:3399: UserWarning: 50.9% of the points cannot be placed; you may want to decrease the size of the markers or use stripplot.
      warnings.warn(msg, UserWarning)
    C:\Users\jihun\anaconda3\lib\site-packages\seaborn\categorical.py:3399: UserWarning: 50.0% of the points cannot be placed; you may want to decrease the size of the markers or use stripplot.
      warnings.warn(msg, UserWarning)
    C:\Users\jihun\anaconda3\lib\site-packages\seaborn\categorical.py:3399: UserWarning: 22.9% of the points cannot be placed; you may want to decrease the size of the markers or use stripplot.
      warnings.warn(msg, UserWarning)
    




    <AxesSubplot:xlabel='신장(5cm단위)', ylabel='체중(5kg단위)'>



    C:\Users\jihun\anaconda3\lib\site-packages\seaborn\categorical.py:3399: UserWarning: 36.5% of the points cannot be placed; you may want to decrease the size of the markers or use stripplot.
      warnings.warn(msg, UserWarning)
    C:\Users\jihun\anaconda3\lib\site-packages\seaborn\categorical.py:3399: UserWarning: 52.3% of the points cannot be placed; you may want to decrease the size of the markers or use stripplot.
      warnings.warn(msg, UserWarning)
    C:\Users\jihun\anaconda3\lib\site-packages\seaborn\categorical.py:3399: UserWarning: 53.1% of the points cannot be placed; you may want to decrease the size of the markers or use stripplot.
      warnings.warn(msg, UserWarning)
    C:\Users\jihun\anaconda3\lib\site-packages\seaborn\categorical.py:3399: UserWarning: 50.9% of the points cannot be placed; you may want to decrease the size of the markers or use stripplot.
      warnings.warn(msg, UserWarning)
    C:\Users\jihun\anaconda3\lib\site-packages\seaborn\categorical.py:3399: UserWarning: 50.0% of the points cannot be placed; you may want to decrease the size of the markers or use stripplot.
      warnings.warn(msg, UserWarning)
    C:\Users\jihun\anaconda3\lib\site-packages\seaborn\categorical.py:3399: UserWarning: 22.9% of the points cannot be placed; you may want to decrease the size of the markers or use stripplot.
      warnings.warn(msg, UserWarning)
    


    
![png](/assets/images/EDA&Visualization/second/output_80_3.png)
    



```python
# swarmplot으로 연령대코드(5세단위)에 따른 음주여부 그리며, 성별코드에 따라 다른 색상으로 표현되게 합니다. 
# 점을 하나씩 찍기 때문에 오래 걸리는 코드는 전체로 그려보기 전에 일부만 가져와 그려봅니다. 
plt.figure(figsize = (15, 4))
sns.swarmplot(data = df_sample, x = "연령대코드(5세단위)", y = "혈색소", hue = "음주여부")
```

    C:\Users\jihun\anaconda3\lib\site-packages\seaborn\categorical.py:3399: UserWarning: 9.3% of the points cannot be placed; you may want to decrease the size of the markers or use stripplot.
      warnings.warn(msg, UserWarning)
    C:\Users\jihun\anaconda3\lib\site-packages\seaborn\categorical.py:3399: UserWarning: 11.3% of the points cannot be placed; you may want to decrease the size of the markers or use stripplot.
      warnings.warn(msg, UserWarning)
    C:\Users\jihun\anaconda3\lib\site-packages\seaborn\categorical.py:3399: UserWarning: 18.7% of the points cannot be placed; you may want to decrease the size of the markers or use stripplot.
      warnings.warn(msg, UserWarning)
    




    <AxesSubplot:xlabel='연령대코드(5세단위)', ylabel='혈색소'>



    C:\Users\jihun\anaconda3\lib\site-packages\seaborn\categorical.py:3399: UserWarning: 17.9% of the points cannot be placed; you may want to decrease the size of the markers or use stripplot.
      warnings.warn(msg, UserWarning)
    C:\Users\jihun\anaconda3\lib\site-packages\seaborn\categorical.py:3399: UserWarning: 9.3% of the points cannot be placed; you may want to decrease the size of the markers or use stripplot.
      warnings.warn(msg, UserWarning)
    C:\Users\jihun\anaconda3\lib\site-packages\seaborn\categorical.py:3399: UserWarning: 11.3% of the points cannot be placed; you may want to decrease the size of the markers or use stripplot.
      warnings.warn(msg, UserWarning)
    C:\Users\jihun\anaconda3\lib\site-packages\seaborn\categorical.py:3399: UserWarning: 17.9% of the points cannot be placed; you may want to decrease the size of the markers or use stripplot.
      warnings.warn(msg, UserWarning)
    


    
![png](/assets/images/EDA&Visualization/second/output_81_3.png)
    



```python
# lmplot으로 그리기 
plt.figure(figsize = (15, 4))
sns.lmplot(data = df_sample, x = "연령대코드(5세단위)", y = "혈색소", hue = "음주여부", col = "성별코드")
```




    <seaborn.axisgrid.FacetGrid at 0x233022375b0>




    <Figure size 1080x288 with 0 Axes>



    
![png](/assets/images/EDA&Visualization/second/output_82_2.png)
    


- 몰려있는 데이터들을 파악하기 어렵다면 swarnplot 사용하는 것이 좋습니다. 어디에 데이터가 분포되어 있는지 더욱 쉽게 확인할 수 있습니다. 
- lnplot : 다변수를 시각화할 수 있습니다. 

---

# 수치형 데이터 시각화 

## scatterplot - 산점도 
- 수치형 vs 수치형 데이터의 상관 관계를 볼 때 주로 사용합니다. 
- 점의 크기를 데이터의 수치에 따라 다르게 볼 수 있습니다. 


```python
# scatterplot으로 "혈청지오티(AST)", "혈청지피티(ALT)"을 그리고 음주여부에 따라 다른 색상으로 표현되게 합니다. 
plt.figure(figsize = (8, 7))
sns.scatterplot(data = df, x = "혈청지오티(AST)", y = "혈청지피티(ALT)", hue = "음주여부", 
                size = "체중(5kg단위)")
```




    <AxesSubplot:xlabel='혈청지오티(AST)', ylabel='혈청지피티(ALT)'>




    
![png](/assets/images/EDA&Visualization/second/output_85_1.png)
    


## lmplot - 상관 관계를 보기 


```python
# lmplot 으로 신장(5cm단위)에 따른 체중(5kg단위) 을 그리며, 음주여부에 따라 다른 색상으로 표현되게 합니다. 
sns.lmplot(data = df_sample, x = "신장(5cm단위)", y = "체중(5kg단위)", hue = "음주여부")
```




    <seaborn.axisgrid.FacetGrid at 0x23323ba3b80>




    
![png](/assets/images/EDA&Visualization/second/output_87_1.png)
    



```python
# lmplot의 col 기능을 통해 음주여부에 따라 서브플롯을 그려봅니다. 
sns.lmplot(data = df_sample, x = "신장(5cm단위)", y = "체중(5kg단위)", hue = "성별코드", col = "음주여부")
```




    <seaborn.axisgrid.FacetGrid at 0x23303e4ca00>




    
![png](/assets/images/EDA&Visualization/second/output_88_1.png)
    



```python
# lmplot 으로 수축기, 이완기혈압을 그리고 음주여부에 따라 다른 색상으로 표현되게 합니다. 
sns.lmplot(data = df_sample, x = "수축기혈압", y = "이완기혈압", hue = "음주여부")
```




    <seaborn.axisgrid.FacetGrid at 0x23303bd0fd0>




    
![png](/assets/images/EDA&Visualization/second/output_89_1.png)
    


`AST와 ALT` 

    AST와 ALT는 간세포에 들어 있는 효소 입니다. 
    
    간이 손송돼 간세포가 파괴되면 그 안에 있던 AST와 ALT가 빠져 나와 혈액 속에 섞여 돌아다니게 됩니다. 따라서 간이 손상되면 AST와 ALT 수치가 높아집니다. 
    
    정상 수치는 병원에 따라 기준치가 다소 차이가 있으나 AST가 5 ~ 35 IU/L, ALT가 5~40 IU/L 정도 입니다.
    
    간혹 전날 술을 마시거나 몸이 피곤하면 일시적으로 AST와 ALT 수치가 정상치를 웃돌 수 있으므로 딱 한 번의 검사만으로 간질환 여부를 판단하는 경우는 드뭅니다. 하지만 AST와 ALT같은 간수치는 간의 상태를 일차적으로 파악하는데 아주 중요한 기준이 됩니다. 


```python
df.columns 
```




    Index(['기준년도', '가입자일련번호', '시도코드', '성별코드', '연령대코드(5세단위)', '신장(5cm단위)',
           '체중(5kg단위)', '허리둘레', '시력(좌)', '시력(우)', '청력(좌)', '청력(우)', '수축기혈압',
           '이완기혈압', '식전혈당(공복혈당)', '총콜레스테롤', '트리글리세라이드', 'HDL콜레스테롤', 'LDL콜레스테롤',
           '혈색소', '요단백', '혈청크레아티닌', '혈청지오티(AST)', '혈청지피티(ALT)', '감마지티피', '흡연상태',
           '음주여부', '구강검진수검여부', '치아우식증유무', '결손치 유무', '치아마모증유무', '제3대구치(사랑니) 이상',
           '치석'],
          dtype='object')




```python
# lmplot으로 "혈청지오티(AST)", "혈청지피티(ALT)" 을 그리고 음주여부에 따라 다른 색상으로 표현되게 합니다. 
sns.lmplot(data = df_sample, x = "혈청지오티(AST)", y = "혈청지피티(ALT)", hue = "음주여부")
```




    <seaborn.axisgrid.FacetGrid at 0x233098e2df0>




    
![png](/assets/images/EDA&Visualization/second/output_92_1.png)
    


## 이상치 다루기 
- 이상치가 있으면 데이터가 자세히 보이지 않거나 이상치로 인해 회귀선이 달라지기도 합니다. 
- 시각화를 통해 찾은 이상치를 제거하고 보거나 이상치만 따로 모아 보도록 합니다. 


```python
# "혈청지오티(AST)"와 "혈청지피티(ALT)"가 400 이하인 값만 데이터프레임 형태로 추출해서 
# df_ASLT 라는 변수에 담습니다. 
df_ASLT = df_sample[(df_sample["혈청지오티(AST)"] < 400) & (df_sample["혈청지피티(ALT)"] < 400)]
df_ASLT
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
      <th>기준년도</th>
      <th>가입자일련번호</th>
      <th>시도코드</th>
      <th>성별코드</th>
      <th>연령대코드(5세단위)</th>
      <th>신장(5cm단위)</th>
      <th>체중(5kg단위)</th>
      <th>허리둘레</th>
      <th>시력(좌)</th>
      <th>시력(우)</th>
      <th>...</th>
      <th>혈청지피티(ALT)</th>
      <th>감마지티피</th>
      <th>흡연상태</th>
      <th>음주여부</th>
      <th>구강검진수검여부</th>
      <th>치아우식증유무</th>
      <th>결손치 유무</th>
      <th>치아마모증유무</th>
      <th>제3대구치(사랑니) 이상</th>
      <th>치석</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>276826</th>
      <td>2023</td>
      <td>996623</td>
      <td>41</td>
      <td>2</td>
      <td>16</td>
      <td>155</td>
      <td>50</td>
      <td>77.0</td>
      <td>1.9</td>
      <td>1.9</td>
      <td>...</td>
      <td>26.0</td>
      <td>16.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>849425</th>
      <td>2023</td>
      <td>3071119</td>
      <td>11</td>
      <td>1</td>
      <td>12</td>
      <td>165</td>
      <td>65</td>
      <td>96.0</td>
      <td>0.8</td>
      <td>1.0</td>
      <td>...</td>
      <td>25.0</td>
      <td>28.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>504499</th>
      <td>2023</td>
      <td>2098584</td>
      <td>27</td>
      <td>2</td>
      <td>10</td>
      <td>150</td>
      <td>50</td>
      <td>72.0</td>
      <td>1.2</td>
      <td>1.2</td>
      <td>...</td>
      <td>16.0</td>
      <td>14.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>601054</th>
      <td>2023</td>
      <td>215047</td>
      <td>28</td>
      <td>2</td>
      <td>5</td>
      <td>155</td>
      <td>55</td>
      <td>70.0</td>
      <td>0.6</td>
      <td>0.9</td>
      <td>...</td>
      <td>10.0</td>
      <td>9.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>980221</th>
      <td>2023</td>
      <td>4867650</td>
      <td>27</td>
      <td>1</td>
      <td>6</td>
      <td>170</td>
      <td>75</td>
      <td>84.0</td>
      <td>1.5</td>
      <td>1.2</td>
      <td>...</td>
      <td>28.0</td>
      <td>16.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>1</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>890013</th>
      <td>2023</td>
      <td>2867229</td>
      <td>41</td>
      <td>1</td>
      <td>11</td>
      <td>160</td>
      <td>75</td>
      <td>96.2</td>
      <td>0.9</td>
      <td>1.2</td>
      <td>...</td>
      <td>34.0</td>
      <td>38.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>875389</th>
      <td>2023</td>
      <td>2953072</td>
      <td>11</td>
      <td>2</td>
      <td>7</td>
      <td>160</td>
      <td>65</td>
      <td>84.0</td>
      <td>1.2</td>
      <td>1.0</td>
      <td>...</td>
      <td>13.0</td>
      <td>26.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>358458</th>
      <td>2023</td>
      <td>1248039</td>
      <td>29</td>
      <td>2</td>
      <td>5</td>
      <td>165</td>
      <td>55</td>
      <td>63.0</td>
      <td>0.8</td>
      <td>0.8</td>
      <td>...</td>
      <td>6.0</td>
      <td>11.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>808228</th>
      <td>2023</td>
      <td>3920987</td>
      <td>41</td>
      <td>2</td>
      <td>12</td>
      <td>160</td>
      <td>60</td>
      <td>71.1</td>
      <td>0.6</td>
      <td>0.4</td>
      <td>...</td>
      <td>14.0</td>
      <td>12.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>317698</th>
      <td>2023</td>
      <td>1625308</td>
      <td>45</td>
      <td>2</td>
      <td>12</td>
      <td>160</td>
      <td>65</td>
      <td>87.0</td>
      <td>0.5</td>
      <td>0.8</td>
      <td>...</td>
      <td>16.0</td>
      <td>20.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>996 rows × 33 columns</p>
</div>




```python
# 이상치를 제거한 "혈청지오티(AST)"와 "혈청지피티(ALT)"를 lmplot으로 그리며 
# 음주여부에 따라 다른 색으로 표현합니다. 
sns.lmplot(data = df_ASLT, x = "혈청지오티(AST)", y = "혈청지피티(ALT)", hue = "음주여부")
```




    <seaborn.axisgrid.FacetGrid at 0x23301f15f70>




    
![png](/assets/images/EDA&Visualization/second/output_95_1.png)
    



```python
df.columns 
```




    Index(['기준년도', '가입자일련번호', '시도코드', '성별코드', '연령대코드(5세단위)', '신장(5cm단위)',
           '체중(5kg단위)', '허리둘레', '시력(좌)', '시력(우)', '청력(좌)', '청력(우)', '수축기혈압',
           '이완기혈압', '식전혈당(공복혈당)', '총콜레스테롤', '트리글리세라이드', 'HDL콜레스테롤', 'LDL콜레스테롤',
           '혈색소', '요단백', '혈청크레아티닌', '혈청지오티(AST)', '혈청지피티(ALT)', '감마지티피', '흡연상태',
           '음주여부', '구강검진수검여부', '치아우식증유무', '결손치 유무', '치아마모증유무', '제3대구치(사랑니) 이상',
           '치석'],
          dtype='object')




```python
# "혈청지오티(AST)"와 "혈청지피티(ALT)"가 400 이상인 값만 데이터프레임 형태로 추출해서 
# df_ASLT_high 라는 변수에 담습니다. 
df_ASLT_high = df[(df["혈청지오티(AST)"] > 400) | (df["혈청지피티(ALT)"] > 400)]
df_ASLT_high
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
      <th>기준년도</th>
      <th>가입자일련번호</th>
      <th>시도코드</th>
      <th>성별코드</th>
      <th>연령대코드(5세단위)</th>
      <th>신장(5cm단위)</th>
      <th>체중(5kg단위)</th>
      <th>허리둘레</th>
      <th>시력(좌)</th>
      <th>시력(우)</th>
      <th>...</th>
      <th>혈청지피티(ALT)</th>
      <th>감마지티피</th>
      <th>흡연상태</th>
      <th>음주여부</th>
      <th>구강검진수검여부</th>
      <th>치아우식증유무</th>
      <th>결손치 유무</th>
      <th>치아마모증유무</th>
      <th>제3대구치(사랑니) 이상</th>
      <th>치석</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2154</th>
      <td>2023</td>
      <td>1311957</td>
      <td>30</td>
      <td>1</td>
      <td>7</td>
      <td>170</td>
      <td>100</td>
      <td>118.0</td>
      <td>1.5</td>
      <td>1.2</td>
      <td>...</td>
      <td>402.0</td>
      <td>124.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3723</th>
      <td>2023</td>
      <td>102375</td>
      <td>41</td>
      <td>2</td>
      <td>12</td>
      <td>150</td>
      <td>60</td>
      <td>84.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>...</td>
      <td>329.0</td>
      <td>808.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>20248</th>
      <td>2023</td>
      <td>3696069</td>
      <td>41</td>
      <td>1</td>
      <td>12</td>
      <td>170</td>
      <td>75</td>
      <td>96.0</td>
      <td>0.9</td>
      <td>1.5</td>
      <td>...</td>
      <td>1399.0</td>
      <td>122.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20562</th>
      <td>2023</td>
      <td>832864</td>
      <td>41</td>
      <td>2</td>
      <td>7</td>
      <td>155</td>
      <td>45</td>
      <td>63.2</td>
      <td>0.8</td>
      <td>0.5</td>
      <td>...</td>
      <td>122.0</td>
      <td>15.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>22553</th>
      <td>2023</td>
      <td>517905</td>
      <td>26</td>
      <td>2</td>
      <td>13</td>
      <td>150</td>
      <td>55</td>
      <td>73.0</td>
      <td>0.9</td>
      <td>0.8</td>
      <td>...</td>
      <td>281.0</td>
      <td>147.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>1</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>986068</th>
      <td>2023</td>
      <td>3855182</td>
      <td>47</td>
      <td>1</td>
      <td>14</td>
      <td>155</td>
      <td>45</td>
      <td>78.0</td>
      <td>0.6</td>
      <td>0.7</td>
      <td>...</td>
      <td>154.0</td>
      <td>199.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>988451</th>
      <td>2023</td>
      <td>3000414</td>
      <td>27</td>
      <td>2</td>
      <td>6</td>
      <td>155</td>
      <td>65</td>
      <td>79.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>...</td>
      <td>1602.0</td>
      <td>183.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>993406</th>
      <td>2023</td>
      <td>4272195</td>
      <td>27</td>
      <td>1</td>
      <td>13</td>
      <td>160</td>
      <td>60</td>
      <td>83.0</td>
      <td>0.7</td>
      <td>0.6</td>
      <td>...</td>
      <td>310.0</td>
      <td>599.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>993435</th>
      <td>2023</td>
      <td>3904165</td>
      <td>41</td>
      <td>2</td>
      <td>8</td>
      <td>160</td>
      <td>65</td>
      <td>87.0</td>
      <td>1.0</td>
      <td>0.5</td>
      <td>...</td>
      <td>450.0</td>
      <td>28.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>997360</th>
      <td>2023</td>
      <td>4755551</td>
      <td>30</td>
      <td>1</td>
      <td>8</td>
      <td>170</td>
      <td>80</td>
      <td>89.6</td>
      <td>1.5</td>
      <td>1.0</td>
      <td>...</td>
      <td>1820.0</td>
      <td>60.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
<p>368 rows × 33 columns</p>
</div>




```python
# 위에서 구한 df_ASLT_high 데이터 프레임에 담겨진 혈청지오티가 높은 데이터만 따로 봅니다. 
sns.lmplot(data = df_ASLT_high, x = "혈청지오티(AST)", y = "혈청지피티(ALT)", hue = "음주여부")
```




    <seaborn.axisgrid.FacetGrid at 0x233049c29d0>




    
![png](/assets/images/EDA&Visualization/second/output_98_1.png)
    



```python
df_ASLT_high_5000 = df_ASLT_high[df_ASLT_high["혈청지오티(AST)"] > 5000]
df_ASLT_high_5000.iloc[:, 4:27]
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
      <th>연령대코드(5세단위)</th>
      <th>신장(5cm단위)</th>
      <th>체중(5kg단위)</th>
      <th>허리둘레</th>
      <th>시력(좌)</th>
      <th>시력(우)</th>
      <th>청력(좌)</th>
      <th>청력(우)</th>
      <th>수축기혈압</th>
      <th>이완기혈압</th>
      <th>...</th>
      <th>HDL콜레스테롤</th>
      <th>LDL콜레스테롤</th>
      <th>혈색소</th>
      <th>요단백</th>
      <th>혈청크레아티닌</th>
      <th>혈청지오티(AST)</th>
      <th>혈청지피티(ALT)</th>
      <th>감마지티피</th>
      <th>흡연상태</th>
      <th>음주여부</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>449377</th>
      <td>8</td>
      <td>165</td>
      <td>80</td>
      <td>106.0</td>
      <td>0.7</td>
      <td>0.8</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>130.0</td>
      <td>80.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>14.9</td>
      <td>4.0</td>
      <td>1.1</td>
      <td>5097.0</td>
      <td>3019.0</td>
      <td>260.0</td>
      <td>3.0</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 23 columns</p>
</div>



## displot 
- 히스토그램
- 확률 밀도 함수 


```python
# 수치형 데이터로 된 컬럼을 찾기 위해 컬럼명만 따로 출력합니다. 

df.columns 
```




    Index(['기준년도', '가입자일련번호', '시도코드', '성별코드', '연령대코드(5세단위)', '신장(5cm단위)',
           '체중(5kg단위)', '허리둘레', '시력(좌)', '시력(우)', '청력(좌)', '청력(우)', '수축기혈압',
           '이완기혈압', '식전혈당(공복혈당)', '총콜레스테롤', '트리글리세라이드', 'HDL콜레스테롤', 'LDL콜레스테롤',
           '혈색소', '요단백', '혈청크레아티닌', '혈청지오티(AST)', '혈청지피티(ALT)', '감마지티피', '흡연상태',
           '음주여부', '구강검진수검여부', '치아우식증유무', '결손치 유무', '치아마모증유무', '제3대구치(사랑니) 이상',
           '치석'],
          dtype='object')




```python
# "총콜레스테롤"에 따른 distplot 을 그립니다. 
sns.displot(data = df, x = "총콜레스테롤")
```




    <seaborn.axisgrid.FacetGrid at 0x233038f5670>




    
![png](/assets/images/EDA&Visualization/second/output_102_1.png)
    



```python
df_chol0 = df[df["음주여부"] == 0]
df_chol0
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
      <th>기준년도</th>
      <th>가입자일련번호</th>
      <th>시도코드</th>
      <th>성별코드</th>
      <th>연령대코드(5세단위)</th>
      <th>신장(5cm단위)</th>
      <th>체중(5kg단위)</th>
      <th>허리둘레</th>
      <th>시력(좌)</th>
      <th>시력(우)</th>
      <th>...</th>
      <th>혈청지피티(ALT)</th>
      <th>감마지티피</th>
      <th>흡연상태</th>
      <th>음주여부</th>
      <th>구강검진수검여부</th>
      <th>치아우식증유무</th>
      <th>결손치 유무</th>
      <th>치아마모증유무</th>
      <th>제3대구치(사랑니) 이상</th>
      <th>치석</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>8</th>
      <td>2023</td>
      <td>1073372</td>
      <td>44</td>
      <td>1</td>
      <td>13</td>
      <td>165</td>
      <td>70</td>
      <td>91.0</td>
      <td>1.5</td>
      <td>1.2</td>
      <td>...</td>
      <td>33.0</td>
      <td>34.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2023</td>
      <td>3493754</td>
      <td>29</td>
      <td>2</td>
      <td>13</td>
      <td>150</td>
      <td>50</td>
      <td>75.0</td>
      <td>0.6</td>
      <td>0.7</td>
      <td>...</td>
      <td>15.0</td>
      <td>14.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2023</td>
      <td>58548</td>
      <td>47</td>
      <td>2</td>
      <td>18</td>
      <td>145</td>
      <td>55</td>
      <td>83.0</td>
      <td>0.4</td>
      <td>0.6</td>
      <td>...</td>
      <td>11.0</td>
      <td>22.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2023</td>
      <td>2114616</td>
      <td>26</td>
      <td>2</td>
      <td>15</td>
      <td>150</td>
      <td>50</td>
      <td>80.2</td>
      <td>0.8</td>
      <td>0.6</td>
      <td>...</td>
      <td>23.0</td>
      <td>19.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>23</th>
      <td>2023</td>
      <td>2540342</td>
      <td>11</td>
      <td>2</td>
      <td>16</td>
      <td>150</td>
      <td>50</td>
      <td>78.2</td>
      <td>0.6</td>
      <td>0.5</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>999988</th>
      <td>2023</td>
      <td>3085327</td>
      <td>30</td>
      <td>2</td>
      <td>17</td>
      <td>130</td>
      <td>30</td>
      <td>80.0</td>
      <td>0.5</td>
      <td>0.6</td>
      <td>...</td>
      <td>22.0</td>
      <td>15.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>999992</th>
      <td>2023</td>
      <td>4088314</td>
      <td>28</td>
      <td>2</td>
      <td>13</td>
      <td>145</td>
      <td>50</td>
      <td>79.0</td>
      <td>0.9</td>
      <td>1.0</td>
      <td>...</td>
      <td>23.0</td>
      <td>24.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>999994</th>
      <td>2023</td>
      <td>1464509</td>
      <td>26</td>
      <td>2</td>
      <td>12</td>
      <td>160</td>
      <td>55</td>
      <td>71.1</td>
      <td>0.8</td>
      <td>1.0</td>
      <td>...</td>
      <td>20.0</td>
      <td>16.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>999998</th>
      <td>2023</td>
      <td>2618086</td>
      <td>41</td>
      <td>2</td>
      <td>7</td>
      <td>160</td>
      <td>55</td>
      <td>69.0</td>
      <td>1.0</td>
      <td>1.2</td>
      <td>...</td>
      <td>20.0</td>
      <td>16.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>999999</th>
      <td>2023</td>
      <td>1279122</td>
      <td>31</td>
      <td>2</td>
      <td>12</td>
      <td>160</td>
      <td>50</td>
      <td>75.0</td>
      <td>0.8</td>
      <td>0.7</td>
      <td>...</td>
      <td>21.0</td>
      <td>34.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
    </tr>
  </tbody>
</table>
<p>340169 rows × 33 columns</p>
</div>




```python
df_chol1 = df[df["음주여부"] == 1]
df_chol1
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
      <th>기준년도</th>
      <th>가입자일련번호</th>
      <th>시도코드</th>
      <th>성별코드</th>
      <th>연령대코드(5세단위)</th>
      <th>신장(5cm단위)</th>
      <th>체중(5kg단위)</th>
      <th>허리둘레</th>
      <th>시력(좌)</th>
      <th>시력(우)</th>
      <th>...</th>
      <th>혈청지피티(ALT)</th>
      <th>감마지티피</th>
      <th>흡연상태</th>
      <th>음주여부</th>
      <th>구강검진수검여부</th>
      <th>치아우식증유무</th>
      <th>결손치 유무</th>
      <th>치아마모증유무</th>
      <th>제3대구치(사랑니) 이상</th>
      <th>치석</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2023</td>
      <td>34735</td>
      <td>46</td>
      <td>2</td>
      <td>9</td>
      <td>155</td>
      <td>70</td>
      <td>92.0</td>
      <td>1.2</td>
      <td>1.2</td>
      <td>...</td>
      <td>24.0</td>
      <td>50.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2023</td>
      <td>4105118</td>
      <td>11</td>
      <td>1</td>
      <td>17</td>
      <td>160</td>
      <td>55</td>
      <td>86.0</td>
      <td>0.9</td>
      <td>9.9</td>
      <td>...</td>
      <td>11.0</td>
      <td>31.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2023</td>
      <td>362482</td>
      <td>36</td>
      <td>2</td>
      <td>13</td>
      <td>150</td>
      <td>65</td>
      <td>96.0</td>
      <td>1.0</td>
      <td>0.8</td>
      <td>...</td>
      <td>29.0</td>
      <td>24.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2023</td>
      <td>653166</td>
      <td>11</td>
      <td>1</td>
      <td>13</td>
      <td>160</td>
      <td>70</td>
      <td>85.0</td>
      <td>1.0</td>
      <td>1.2</td>
      <td>...</td>
      <td>21.0</td>
      <td>27.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>1</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2023</td>
      <td>4152237</td>
      <td>41</td>
      <td>1</td>
      <td>12</td>
      <td>165</td>
      <td>65</td>
      <td>84.5</td>
      <td>1.0</td>
      <td>1.2</td>
      <td>...</td>
      <td>33.0</td>
      <td>49.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>999991</th>
      <td>2023</td>
      <td>930481</td>
      <td>41</td>
      <td>1</td>
      <td>10</td>
      <td>165</td>
      <td>65</td>
      <td>80.8</td>
      <td>1.5</td>
      <td>1.5</td>
      <td>...</td>
      <td>19.0</td>
      <td>41.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>1</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>999993</th>
      <td>2023</td>
      <td>3833709</td>
      <td>41</td>
      <td>1</td>
      <td>6</td>
      <td>180</td>
      <td>75</td>
      <td>77.0</td>
      <td>0.8</td>
      <td>0.8</td>
      <td>...</td>
      <td>20.0</td>
      <td>16.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>999995</th>
      <td>2023</td>
      <td>3265034</td>
      <td>26</td>
      <td>1</td>
      <td>8</td>
      <td>170</td>
      <td>65</td>
      <td>78.0</td>
      <td>1.2</td>
      <td>1.0</td>
      <td>...</td>
      <td>13.0</td>
      <td>22.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>999996</th>
      <td>2023</td>
      <td>1421865</td>
      <td>41</td>
      <td>1</td>
      <td>10</td>
      <td>165</td>
      <td>80</td>
      <td>96.1</td>
      <td>0.9</td>
      <td>1.2</td>
      <td>...</td>
      <td>65.0</td>
      <td>160.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>999997</th>
      <td>2023</td>
      <td>3889375</td>
      <td>41</td>
      <td>2</td>
      <td>11</td>
      <td>155</td>
      <td>65</td>
      <td>87.0</td>
      <td>0.5</td>
      <td>0.7</td>
      <td>...</td>
      <td>26.0</td>
      <td>25.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>659775 rows × 33 columns</p>
</div>




```python
# 음주여부가 1인 값에 대한 "총콜레스테롤" 을 displot 으로 그립니다. 

sns.displot(data = df_chol1, x = "총콜레스테롤", hue = "음주여부")
```




    <seaborn.axisgrid.FacetGrid at 0x2330993d610>




    
![png](/assets/images/EDA&Visualization/second/output_105_1.png)
    



```python
# 음주여부가 0인 값에 대한 "총콜레스테롤" 을 displot 으로 그립니다. 

sns.displot(data = df_chol0, x = "총콜레스테롤", hue = "음주여부")
```




    <seaborn.axisgrid.FacetGrid at 0x23332a15580>




    
![png](/assets/images/EDA&Visualization/second/output_106_1.png)
    



```python
# 음주여부 값에 대한 "총콜레스테롤" 을 displot 으로 그리며, 하나의 그래프에 표시되도록 합니다. 
sns.displot(data = df, x = "총콜레스테롤", hue = "음주여부", kde = True, bins = 100)

plt.axvline(df["총콜레스테롤"].mean(), linestyle = ":")
plt.axvline(df["총콜레스테롤"].median(), linestyle = "--")
```




    <matplotlib.lines.Line2D at 0x23333c929a0>




    
![png](/assets/images/EDA&Visualization/second/output_107_1.png)
    



```python
# 감마지티피 값에 따라 음주여부를 시각화 합니다. 

sns.displot(data = df_sample, x = "감마지티피", hue = "음주여부", kde = True)
```




    <seaborn.axisgrid.FacetGrid at 0x23333c8c880>




    
![png](/assets/images/EDA&Visualization/second/output_108_1.png)
    


---

# 상관 분석 

r 이 -1.0과 -0.7 사이이면, 강한 음적 선형관계, 

r 이 -0.7과 -0.3 사이이면, 뚜렷한 음적 선형관계, 

r 이 -0.3과 -0.1 사이이면, 약한 음적 선형관계, 

r 이 -0.1과 +0.1 사이이면, 거의 무시될 수 있는 선형관계, 

r 이 +0.1과 +0.3 사이이면, 약한 양적 선형관계, 

r 이 +0.3과 +0.7 사이이면, 뚜렷한 양적 선형관계, 

r 이 +0.7과 +1.0 사이이면, 강한 양적 선형관계


```python
# 상관관계에 사용할 컬럼을 변수에 담습니다. 
columns = ["연령대코드(5세단위)", "체중(5kg단위)", "신장(5cm단위)", "허리둘레", 
          "시력(좌)", "시력(우)", "청력(좌)", "청력(우)", "수축기혈압", "이완기혈압", "식전혈당(공복혈당)", 
          "총콜레스테롤", "트리글리세라이드", "HDL콜레스테롤", "LDL콜레스테롤", "혈색소", "요단백", 
          "혈청크레아티닌", "혈청지오티(AST)", "혈청지피티(ALT)", "감마지티피", "흡연상태", "음주여부"]
columns 
```




    ['연령대코드(5세단위)',
     '체중(5kg단위)',
     '신장(5cm단위)',
     '허리둘레',
     '시력(좌)',
     '시력(우)',
     '청력(좌)',
     '청력(우)',
     '수축기혈압',
     '이완기혈압',
     '식전혈당(공복혈당)',
     '총콜레스테롤',
     '트리글리세라이드',
     'HDL콜레스테롤',
     'LDL콜레스테롤',
     '혈색소',
     '요단백',
     '혈청크레아티닌',
     '혈청지오티(AST)',
     '혈청지피티(ALT)',
     '감마지티피',
     '흡연상태',
     '음주여부']



## 상관계수 구하기 


```python
# 샘플컬럼만 가져와서 df_small 이라는 데이터프레임에 담은 뒤 상관계수를 구합니다. 
df_small = df_sample[columns]
df_corr = df_small.corr()
df_corr
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
      <th>연령대코드(5세단위)</th>
      <th>체중(5kg단위)</th>
      <th>신장(5cm단위)</th>
      <th>허리둘레</th>
      <th>시력(좌)</th>
      <th>시력(우)</th>
      <th>청력(좌)</th>
      <th>청력(우)</th>
      <th>수축기혈압</th>
      <th>이완기혈압</th>
      <th>...</th>
      <th>HDL콜레스테롤</th>
      <th>LDL콜레스테롤</th>
      <th>혈색소</th>
      <th>요단백</th>
      <th>혈청크레아티닌</th>
      <th>혈청지오티(AST)</th>
      <th>혈청지피티(ALT)</th>
      <th>감마지티피</th>
      <th>흡연상태</th>
      <th>음주여부</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>연령대코드(5세단위)</th>
      <td>1.000000</td>
      <td>-0.218013</td>
      <td>-0.379160</td>
      <td>0.148843</td>
      <td>-0.127396</td>
      <td>-0.188795</td>
      <td>0.183535</td>
      <td>0.195334</td>
      <td>0.349800</td>
      <td>0.159607</td>
      <td>...</td>
      <td>-0.008990</td>
      <td>-0.069520</td>
      <td>-0.135059</td>
      <td>0.001206</td>
      <td>0.018989</td>
      <td>0.083863</td>
      <td>-0.049474</td>
      <td>0.016992</td>
      <td>-0.100189</td>
      <td>-0.328453</td>
    </tr>
    <tr>
      <th>체중(5kg단위)</th>
      <td>-0.218013</td>
      <td>1.000000</td>
      <td>0.633472</td>
      <td>0.810004</td>
      <td>0.037348</td>
      <td>0.072104</td>
      <td>-0.006374</td>
      <td>-0.022580</td>
      <td>0.202821</td>
      <td>0.289494</td>
      <td>...</td>
      <td>-0.474402</td>
      <td>-0.047627</td>
      <td>0.469982</td>
      <td>0.045226</td>
      <td>0.174937</td>
      <td>0.219666</td>
      <td>0.414519</td>
      <td>0.176637</td>
      <td>0.322813</td>
      <td>0.249291</td>
    </tr>
    <tr>
      <th>신장(5cm단위)</th>
      <td>-0.379160</td>
      <td>0.633472</td>
      <td>1.000000</td>
      <td>0.300283</td>
      <td>0.095645</td>
      <td>0.141252</td>
      <td>-0.019221</td>
      <td>-0.039121</td>
      <td>-0.014566</td>
      <td>0.098946</td>
      <td>...</td>
      <td>-0.263950</td>
      <td>0.022036</td>
      <td>0.484071</td>
      <td>-0.014542</td>
      <td>0.212614</td>
      <td>0.052889</td>
      <td>0.176992</td>
      <td>0.110185</td>
      <td>0.379509</td>
      <td>0.369439</td>
    </tr>
    <tr>
      <th>허리둘레</th>
      <td>0.148843</td>
      <td>0.810004</td>
      <td>0.300283</td>
      <td>1.000000</td>
      <td>-0.027205</td>
      <td>-0.000727</td>
      <td>0.080931</td>
      <td>0.096487</td>
      <td>0.327536</td>
      <td>0.340540</td>
      <td>...</td>
      <td>-0.448829</td>
      <td>-0.041462</td>
      <td>0.367066</td>
      <td>0.081971</td>
      <td>0.160110</td>
      <td>0.280977</td>
      <td>0.401564</td>
      <td>0.206141</td>
      <td>0.240071</td>
      <td>0.069029</td>
    </tr>
    <tr>
      <th>시력(좌)</th>
      <td>-0.127396</td>
      <td>0.037348</td>
      <td>0.095645</td>
      <td>-0.027205</td>
      <td>1.000000</td>
      <td>0.782525</td>
      <td>-0.070846</td>
      <td>0.011778</td>
      <td>-0.075470</td>
      <td>-0.016174</td>
      <td>...</td>
      <td>-0.070988</td>
      <td>-0.031262</td>
      <td>0.093129</td>
      <td>-0.000907</td>
      <td>0.004689</td>
      <td>-0.031116</td>
      <td>0.020102</td>
      <td>0.002466</td>
      <td>0.027782</td>
      <td>0.077466</td>
    </tr>
    <tr>
      <th>시력(우)</th>
      <td>-0.188795</td>
      <td>0.072104</td>
      <td>0.141252</td>
      <td>-0.000727</td>
      <td>0.782525</td>
      <td>1.000000</td>
      <td>-0.081140</td>
      <td>0.004906</td>
      <td>-0.076975</td>
      <td>0.001863</td>
      <td>...</td>
      <td>-0.030498</td>
      <td>-0.025723</td>
      <td>0.109446</td>
      <td>-0.001537</td>
      <td>0.039497</td>
      <td>-0.045625</td>
      <td>0.013817</td>
      <td>0.005675</td>
      <td>0.053504</td>
      <td>0.127111</td>
    </tr>
    <tr>
      <th>청력(좌)</th>
      <td>0.183535</td>
      <td>-0.006374</td>
      <td>-0.019221</td>
      <td>0.080931</td>
      <td>-0.070846</td>
      <td>-0.081140</td>
      <td>1.000000</td>
      <td>0.602775</td>
      <td>0.029199</td>
      <td>-0.038140</td>
      <td>...</td>
      <td>-0.126116</td>
      <td>-0.107053</td>
      <td>0.006606</td>
      <td>0.011492</td>
      <td>0.005684</td>
      <td>0.009670</td>
      <td>-0.007910</td>
      <td>-0.006114</td>
      <td>-0.023750</td>
      <td>-0.010153</td>
    </tr>
    <tr>
      <th>청력(우)</th>
      <td>0.195334</td>
      <td>-0.022580</td>
      <td>-0.039121</td>
      <td>0.096487</td>
      <td>0.011778</td>
      <td>0.004906</td>
      <td>0.602775</td>
      <td>1.000000</td>
      <td>0.039796</td>
      <td>-0.027337</td>
      <td>...</td>
      <td>-0.131223</td>
      <td>-0.105415</td>
      <td>-0.021564</td>
      <td>0.043040</td>
      <td>0.022896</td>
      <td>0.027041</td>
      <td>-0.010486</td>
      <td>0.006759</td>
      <td>-0.046579</td>
      <td>-0.054561</td>
    </tr>
    <tr>
      <th>수축기혈압</th>
      <td>0.349800</td>
      <td>0.202821</td>
      <td>-0.014566</td>
      <td>0.327536</td>
      <td>-0.075470</td>
      <td>-0.076975</td>
      <td>0.029199</td>
      <td>0.039796</td>
      <td>1.000000</td>
      <td>0.718995</td>
      <td>...</td>
      <td>-0.139903</td>
      <td>-0.027939</td>
      <td>0.123754</td>
      <td>0.022890</td>
      <td>0.058005</td>
      <td>0.134216</td>
      <td>0.147013</td>
      <td>0.151787</td>
      <td>0.069112</td>
      <td>-0.075946</td>
    </tr>
    <tr>
      <th>이완기혈압</th>
      <td>0.159607</td>
      <td>0.289494</td>
      <td>0.098946</td>
      <td>0.340540</td>
      <td>-0.016174</td>
      <td>0.001863</td>
      <td>-0.038140</td>
      <td>-0.027337</td>
      <td>0.718995</td>
      <td>1.000000</td>
      <td>...</td>
      <td>-0.057340</td>
      <td>0.082778</td>
      <td>0.232416</td>
      <td>0.006830</td>
      <td>0.037778</td>
      <td>0.122247</td>
      <td>0.180714</td>
      <td>0.166703</td>
      <td>0.152681</td>
      <td>0.040279</td>
    </tr>
    <tr>
      <th>식전혈당(공복혈당)</th>
      <td>0.206484</td>
      <td>0.136265</td>
      <td>0.019214</td>
      <td>0.221309</td>
      <td>-0.028642</td>
      <td>-0.016368</td>
      <td>0.018105</td>
      <td>0.021174</td>
      <td>0.164785</td>
      <td>0.142229</td>
      <td>...</td>
      <td>-0.217306</td>
      <td>-0.121702</td>
      <td>0.139489</td>
      <td>0.157652</td>
      <td>0.038795</td>
      <td>0.184680</td>
      <td>0.207667</td>
      <td>0.182517</td>
      <td>0.133554</td>
      <td>0.006212</td>
    </tr>
    <tr>
      <th>총콜레스테롤</th>
      <td>-0.077281</td>
      <td>-0.096693</td>
      <td>-0.016820</td>
      <td>-0.078285</td>
      <td>-0.056973</td>
      <td>-0.038872</td>
      <td>-0.119387</td>
      <td>-0.122461</td>
      <td>-0.011587</td>
      <td>0.116069</td>
      <td>...</td>
      <td>0.274266</td>
      <td>0.945943</td>
      <td>0.097754</td>
      <td>-0.108839</td>
      <td>-0.012347</td>
      <td>-0.054421</td>
      <td>-0.017856</td>
      <td>0.021746</td>
      <td>-0.017133</td>
      <td>0.121078</td>
    </tr>
    <tr>
      <th>트리글리세라이드</th>
      <td>-0.075039</td>
      <td>0.362322</td>
      <td>0.202382</td>
      <td>0.359888</td>
      <td>0.009455</td>
      <td>0.008060</td>
      <td>0.031522</td>
      <td>0.024972</td>
      <td>0.169232</td>
      <td>0.189047</td>
      <td>...</td>
      <td>-0.438453</td>
      <td>0.104399</td>
      <td>0.227156</td>
      <td>0.037112</td>
      <td>0.162543</td>
      <td>0.072826</td>
      <td>0.224462</td>
      <td>0.113394</td>
      <td>0.279714</td>
      <td>0.132334</td>
    </tr>
    <tr>
      <th>HDL콜레스테롤</th>
      <td>-0.008990</td>
      <td>-0.474402</td>
      <td>-0.263950</td>
      <td>-0.448829</td>
      <td>-0.070988</td>
      <td>-0.030498</td>
      <td>-0.126116</td>
      <td>-0.131223</td>
      <td>-0.139903</td>
      <td>-0.057340</td>
      <td>...</td>
      <td>1.000000</td>
      <td>0.070019</td>
      <td>-0.199439</td>
      <td>-0.061067</td>
      <td>-0.177129</td>
      <td>0.015201</td>
      <td>-0.190295</td>
      <td>0.079779</td>
      <td>-0.203105</td>
      <td>0.001492</td>
    </tr>
    <tr>
      <th>LDL콜레스테롤</th>
      <td>-0.069520</td>
      <td>-0.047627</td>
      <td>0.022036</td>
      <td>-0.041462</td>
      <td>-0.031262</td>
      <td>-0.025723</td>
      <td>-0.107053</td>
      <td>-0.105415</td>
      <td>-0.027939</td>
      <td>0.082778</td>
      <td>...</td>
      <td>0.070019</td>
      <td>1.000000</td>
      <td>0.105996</td>
      <td>-0.122924</td>
      <td>-0.020882</td>
      <td>-0.099739</td>
      <td>-0.025265</td>
      <td>-0.047366</td>
      <td>-0.018342</td>
      <td>0.109403</td>
    </tr>
    <tr>
      <th>혈색소</th>
      <td>-0.135059</td>
      <td>0.469982</td>
      <td>0.484071</td>
      <td>0.367066</td>
      <td>0.093129</td>
      <td>0.109446</td>
      <td>0.006606</td>
      <td>-0.021564</td>
      <td>0.123754</td>
      <td>0.232416</td>
      <td>...</td>
      <td>-0.199439</td>
      <td>0.105996</td>
      <td>1.000000</td>
      <td>0.004334</td>
      <td>0.134153</td>
      <td>0.216544</td>
      <td>0.333478</td>
      <td>0.251651</td>
      <td>0.428821</td>
      <td>0.241594</td>
    </tr>
    <tr>
      <th>요단백</th>
      <td>0.001206</td>
      <td>0.045226</td>
      <td>-0.014542</td>
      <td>0.081971</td>
      <td>-0.000907</td>
      <td>-0.001537</td>
      <td>0.011492</td>
      <td>0.043040</td>
      <td>0.022890</td>
      <td>0.006830</td>
      <td>...</td>
      <td>-0.061067</td>
      <td>-0.122924</td>
      <td>0.004334</td>
      <td>1.000000</td>
      <td>0.047256</td>
      <td>0.035156</td>
      <td>0.044127</td>
      <td>-0.007007</td>
      <td>0.036278</td>
      <td>0.008124</td>
    </tr>
    <tr>
      <th>혈청크레아티닌</th>
      <td>0.018989</td>
      <td>0.174937</td>
      <td>0.212614</td>
      <td>0.160110</td>
      <td>0.004689</td>
      <td>0.039497</td>
      <td>0.005684</td>
      <td>0.022896</td>
      <td>0.058005</td>
      <td>0.037778</td>
      <td>...</td>
      <td>-0.177129</td>
      <td>-0.020882</td>
      <td>0.134153</td>
      <td>0.047256</td>
      <td>1.000000</td>
      <td>0.028677</td>
      <td>0.063325</td>
      <td>0.021825</td>
      <td>0.126396</td>
      <td>0.026089</td>
    </tr>
    <tr>
      <th>혈청지오티(AST)</th>
      <td>0.083863</td>
      <td>0.219666</td>
      <td>0.052889</td>
      <td>0.280977</td>
      <td>-0.031116</td>
      <td>-0.045625</td>
      <td>0.009670</td>
      <td>0.027041</td>
      <td>0.134216</td>
      <td>0.122247</td>
      <td>...</td>
      <td>0.015201</td>
      <td>-0.099739</td>
      <td>0.216544</td>
      <td>0.035156</td>
      <td>0.028677</td>
      <td>1.000000</td>
      <td>0.769642</td>
      <td>0.451544</td>
      <td>0.092994</td>
      <td>0.023030</td>
    </tr>
    <tr>
      <th>혈청지피티(ALT)</th>
      <td>-0.049474</td>
      <td>0.414519</td>
      <td>0.176992</td>
      <td>0.401564</td>
      <td>0.020102</td>
      <td>0.013817</td>
      <td>-0.007910</td>
      <td>-0.010486</td>
      <td>0.147013</td>
      <td>0.180714</td>
      <td>...</td>
      <td>-0.190295</td>
      <td>-0.025265</td>
      <td>0.333478</td>
      <td>0.044127</td>
      <td>0.063325</td>
      <td>0.769642</td>
      <td>1.000000</td>
      <td>0.361555</td>
      <td>0.180274</td>
      <td>0.061475</td>
    </tr>
    <tr>
      <th>감마지티피</th>
      <td>0.016992</td>
      <td>0.176637</td>
      <td>0.110185</td>
      <td>0.206141</td>
      <td>0.002466</td>
      <td>0.005675</td>
      <td>-0.006114</td>
      <td>0.006759</td>
      <td>0.151787</td>
      <td>0.166703</td>
      <td>...</td>
      <td>0.079779</td>
      <td>-0.047366</td>
      <td>0.251651</td>
      <td>-0.007007</td>
      <td>0.021825</td>
      <td>0.451544</td>
      <td>0.361555</td>
      <td>1.000000</td>
      <td>0.216047</td>
      <td>0.135755</td>
    </tr>
    <tr>
      <th>흡연상태</th>
      <td>-0.100189</td>
      <td>0.322813</td>
      <td>0.379509</td>
      <td>0.240071</td>
      <td>0.027782</td>
      <td>0.053504</td>
      <td>-0.023750</td>
      <td>-0.046579</td>
      <td>0.069112</td>
      <td>0.152681</td>
      <td>...</td>
      <td>-0.203105</td>
      <td>-0.018342</td>
      <td>0.428821</td>
      <td>0.036278</td>
      <td>0.126396</td>
      <td>0.092994</td>
      <td>0.180274</td>
      <td>0.216047</td>
      <td>1.000000</td>
      <td>0.251431</td>
    </tr>
    <tr>
      <th>음주여부</th>
      <td>-0.328453</td>
      <td>0.249291</td>
      <td>0.369439</td>
      <td>0.069029</td>
      <td>0.077466</td>
      <td>0.127111</td>
      <td>-0.010153</td>
      <td>-0.054561</td>
      <td>-0.075946</td>
      <td>0.040279</td>
      <td>...</td>
      <td>0.001492</td>
      <td>0.109403</td>
      <td>0.241594</td>
      <td>0.008124</td>
      <td>0.026089</td>
      <td>0.023030</td>
      <td>0.061475</td>
      <td>0.135755</td>
      <td>0.251431</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
<p>23 rows × 23 columns</p>
</div>




```python
# 키에 대한 상관계수가 특정 수치 이상인 데이터를 봅니다. 
df_corr.loc[df_corr["신장(5cm단위)"] > 0.3, "신장(5cm단위)"]
```




    체중(5kg단위)    0.633472
    신장(5cm단위)    1.000000
    허리둘레         0.300283
    혈색소          0.484071
    흡연상태         0.379509
    음주여부         0.369439
    Name: 신장(5cm단위), dtype: float64




```python
# 음주여부에 대한 상관계수가 특정 수치 이상인 데이터를 봅니다, 

df_corr["음주여부"].sort_values()
```




    연령대코드(5세단위)   -0.328453
    수축기혈압         -0.075946
    청력(우)         -0.054561
    청력(좌)         -0.010153
    HDL콜레스테롤       0.001492
    식전혈당(공복혈당)     0.006212
    요단백            0.008124
    혈청지오티(AST)     0.023030
    혈청크레아티닌        0.026089
    이완기혈압          0.040279
    혈청지피티(ALT)     0.061475
    허리둘레           0.069029
    시력(좌)          0.077466
    LDL콜레스테롤       0.109403
    총콜레스테롤         0.121078
    시력(우)          0.127111
    트리글리세라이드       0.132334
    감마지티피          0.135755
    혈색소            0.241594
    체중(5kg단위)      0.249291
    흡연상태           0.251431
    신장(5cm단위)      0.369439
    음주여부           1.000000
    Name: 음주여부, dtype: float64




```python
# 혈색소에 대한 상관계수가 특정 수치 이상인 데이터를 봅니다. 
df_corr["혈색소"].sort_values(ascending = False).head(7)
```




    혈색소           1.000000
    신장(5cm단위)     0.484071
    체중(5kg단위)     0.469982
    흡연상태          0.428821
    허리둘레          0.367066
    혈청지피티(ALT)    0.333478
    감마지티피         0.251651
    Name: 혈색소, dtype: float64




```python
# 감마지티피에 대한 상관계수가 특정 수치 이상인 데이터를 봅니다. 
df_corr["감마지티피"].sort_values(ascending = False).head(7)
```




    감마지티피         1.000000
    혈청지오티(AST)    0.451544
    혈청지피티(ALT)    0.361555
    혈색소           0.251651
    흡연상태          0.216047
    허리둘레          0.206141
    식전혈당(공복혈당)    0.182517
    Name: 감마지티피, dtype: float64



## heatmap 
- [seaborn_heatmap](https://seaborn.pydata.org/examples/many_pairwise_correlations.html)


```python
# 위에서 구한 상관계수를 heatmap을 통해 표현해 봅니다. 
plt.figure(figsize = (20, 7))
sns.heatmap(df_corr, annot = True, fmt = ".2f", cmap = "Blues")
```




    <AxesSubplot:>




    
![png](/assets/images/EDA&Visualization/second/output_118_1.png)
    



```python
mask = np.triu(np.ones_like(df_corr, dtype=bool))

plt.figure(figsize = (20, 7))
sns.heatmap(df_corr, annot = True, fmt = ".2f", cmap = "Blues", mask = mask)
```




    <AxesSubplot:>




    
![png](/assets/images/EDA&Visualization/second/output_119_1.png)
    


- 결론적으로 음주 여부에 따라 건강검진 수치가 차이가 있고, 신장과 허리둘레의 크기는 체중과 상관관계가 있습니다. 
