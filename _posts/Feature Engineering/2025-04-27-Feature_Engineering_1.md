---
title: "[Feature Engineering] 최적의 자취방 구하기"

categories: Feature_Engineering

tags: [python, data, Feature_Engineering]
---


# 부동산 데이터: 최적의 자취방 구하기

✅ 최적의 자취방을 구하기 위한 노력

지방 자취러로서 취업에 성공하게 된다면.. 출근에 용이하고 최상의 집 조건을 가진 집에서 생활하고 싶습니다. 

그래서 서울특별시 관악구 봉천동의 부동산 데이터를 통해 원하는 집을 찾아 방문할 예정입니다. 

✅ 원하는 집의 조건 

1. 보증금 3000만원 이하

2. 월세는 저렴할수록 좋음

3. 지하, 반지하, 꼭대기층은 선호하지 않음

4. 전용면적이 클수록 좋음

5. 북향은 선호하지 않음

6. 연식이 오래되지 않을수록 좋음

7. 지하철 역에서 가까울수록 좋음

## 크롤링


```python
from user_agent import generate_user_agent, generate_navigator
user_agent = generate_user_agent()
user_agent
```

```python
import requests
import numpy as np
import pandas as pd
from tqdm.notebook import tqdm
import time
```

```python
article_list = []
for i in tqdm(range(1, 101)):
    try:
        url = f'https://m.land.naver.com/cluster/ajax/articleList?itemId=&mapKey=&lgeo=&showR0=&rletTpCd=OPST%3AVL%3AOR&tradTpCd=B2&z=12&lat=37.481021&lon=126.951601&btm=37.3398975&lft=126.6762562&top=37.6218785&rgt=127.2269458&totCnt=8360&cortarNo=1162000000&sort=rank&page={i}'

        user_agent = generate_user_agent()
        headers = {'User-Agent':user_agent}

        res = requests.get(url, headers=headers)
        time.sleep(1)

        article_json = res.json()
        article_body = article_json['body']
        article_list.append(article_body)
    except:
        break
```

✅ 서울특별시 관악구 봉천동에 있는 매물을 네이버 부동산을 통해 크롤링합니다. 


```python
article_list1 = [j for i in article_list for j in i]
```

```python
data = pd.DataFrame(article_list1)
```


```python
data = data[['atclNo','rletTpNm','flrInfo','rentPrc','hanPrc','spc1','spc2','direction','atclCfmYmd','repImgUrl','lat','lng','atclFetrDesc','tagList']]
data.columns = ['물건번호','구분','층수(물건층/전체층)','월세','보증금','계약면적(m2)','전용면적(m2)','방향','확인일자','이미지','위도','경도','설명','태그']
data
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
      <th>물건번호</th>
      <th>구분</th>
      <th>층수(물건층/전체층)</th>
      <th>월세</th>
      <th>보증금</th>
      <th>계약면적(m2)</th>
      <th>전용면적(m2)</th>
      <th>방향</th>
      <th>확인일자</th>
      <th>이미지</th>
      <th>위도</th>
      <th>경도</th>
      <th>설명</th>
      <th>태그</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2521833244</td>
      <td>오피스텔</td>
      <td>11/16</td>
      <td>105</td>
      <td>500</td>
      <td>29</td>
      <td>19.53</td>
      <td>동향</td>
      <td>25.04.25.</td>
      <td>/20250425_173/1745570410430YI84v_JPEG/677b2f58...</td>
      <td>37.485593</td>
      <td>126.930410</td>
      <td>신림역 초역세권에 위치한 신축오피스텔 1.5룸 입니다.</td>
      <td>[2년이내, 역세권, 소형평수]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2521602111</td>
      <td>원룸</td>
      <td>1/4</td>
      <td>40</td>
      <td>2,000</td>
      <td>16</td>
      <td>16.5</td>
      <td>서향</td>
      <td>25.04.25.</td>
      <td>/20250425_7/17455719623224Hbh2_JPEG/d6b4274e8c...</td>
      <td>37.477661</td>
      <td>126.965002</td>
      <td>위치 좋은 분리형 풀옵션 원룸</td>
      <td>[25년이내, 역세권, 1층]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2521833099</td>
      <td>오피스텔</td>
      <td>11/16</td>
      <td>100</td>
      <td>1,000</td>
      <td>29</td>
      <td>19.53</td>
      <td>동향</td>
      <td>25.04.25.</td>
      <td>/20250425_287/1745570406570jBbFn_JPEG/5e41441b...</td>
      <td>37.485593</td>
      <td>126.930410</td>
      <td>신림역 초역세권에 위치한 신축 풀옵션 프리미엄 오피스텔 1.5룸 입니다.</td>
      <td>[2년이내, 역세권, 소형평수]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2521599464</td>
      <td>원룸</td>
      <td>3/5</td>
      <td>50</td>
      <td>1,000</td>
      <td>16</td>
      <td>16.5</td>
      <td>서향</td>
      <td>25.04.25.</td>
      <td>/20250425_2/1745571851447tu01B_JPEG/05c35bef5b...</td>
      <td>37.474452</td>
      <td>126.964377</td>
      <td>지하철 역 가까운 낙성대역 원룸</td>
      <td>[25년이내, 역세권, 화장실한개]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2521701387</td>
      <td>원룸</td>
      <td>2/6</td>
      <td>60</td>
      <td>3,000</td>
      <td>25</td>
      <td>24</td>
      <td>남서향</td>
      <td>25.04.25.</td>
      <td>/20250425_137/1745570287407vl8lt_JPEG/58d20793...</td>
      <td>37.482340</td>
      <td>126.932285</td>
      <td>1.5룸 리모델링 채광좋고 깔끔해요 큰길가 위치굿 밤길안전</td>
      <td>[25년이내, 융자금적은, 역세권]</td>
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
    </tr>
    <tr>
      <th>1995</th>
      <td>2521789960</td>
      <td>원룸</td>
      <td>저/3</td>
      <td>40</td>
      <td>1,000</td>
      <td>23</td>
      <td>23.14</td>
      <td>남동향</td>
      <td>25.04.23.</td>
      <td>/20250423_99/1745391603834Q9ntL_JPEG/6808893ba...</td>
      <td>37.476291</td>
      <td>126.962862</td>
      <td>설명이필요없는깔끔한원룸와따 이집은 솔루션이 필요없어유</td>
      <td>[10년이내, 역세권, 소형평수]</td>
    </tr>
    <tr>
      <th>1996</th>
      <td>2521789379</td>
      <td>빌라</td>
      <td>중/5</td>
      <td>40</td>
      <td>1억</td>
      <td>70</td>
      <td>48.03</td>
      <td>남동향</td>
      <td>25.04.23.</td>
      <td>/20250423_67/1745390593023RVDqU_JPEG/batch_Kak...</td>
      <td>37.484023</td>
      <td>126.951473</td>
      <td>보금자리 올리모델링 운동장만한 역세권 큰 1.5룸입니다.</td>
      <td>[25년이내, 역세권, 화장실한개]</td>
    </tr>
    <tr>
      <th>1997</th>
      <td>2521787876</td>
      <td>빌라</td>
      <td>중/4</td>
      <td>20</td>
      <td>1억 9,000</td>
      <td>49</td>
      <td>46.28</td>
      <td>남서향</td>
      <td>25.04.23.</td>
      <td>/20250423_174/1745390687089So1RK_JPEG/68088714...</td>
      <td>37.472863</td>
      <td>126.970566</td>
      <td>핵꿀매물낙성대역 사이즈 크고 대출 가능한 풀옵션 넓은 투룸</td>
      <td>[15년이내, 화장실한개, 소형평수]</td>
    </tr>
    <tr>
      <th>1998</th>
      <td>2521780575</td>
      <td>원룸</td>
      <td>2/5</td>
      <td>50</td>
      <td>1,000</td>
      <td>-</td>
      <td>23.14</td>
      <td>남향</td>
      <td>25.04.23.</td>
      <td>/20250423_262/174538878407443SUa_JPEG/KakaoTal...</td>
      <td>37.475296</td>
      <td>126.972970</td>
      <td>사당역 근처 준신축 원룸입니다</td>
      <td>[15년이내, 융자금적은, 화장실한개]</td>
    </tr>
    <tr>
      <th>1999</th>
      <td>2521779947</td>
      <td>원룸</td>
      <td>7/8</td>
      <td>45</td>
      <td>1,000</td>
      <td>-</td>
      <td>16.5</td>
      <td>동향</td>
      <td>25.04.23.</td>
      <td>/20250423_234/1745388365054usPt5_JPEG/17453875...</td>
      <td>37.484832</td>
      <td>126.954119</td>
      <td>서울대입구역 역세권 가성비 좋은 원룸</td>
      <td>[15년이내, 융자금없는, 역세권]</td>
    </tr>
  </tbody>
</table>
<p>2000 rows × 14 columns</p>
</div>




```python
data.to_excel('data/관악구_봉천동.xlsx')
```

## 내가 원하는 자취방은?

✅ 최적의 자취방 구하기
    
    - 내가 원하는 조건은?
        
        1. 보증금 3000만원 이하
        
        2. 월세는 저렴할수록 좋음
        
        3. 지하, 반지하, 꼭대기층은 선호하지 않음
        
        4. 전용면적이 클수록 좋음
        
        5. 북향은 선호하지 않음
        
        6. 연식이 오래되지 않을수록 좋음
        
        7. 지하철 역에서 가까울수록 좋음

## 데이터 전처리


```python
import plotly.express as px
import folium

import warnings
warnings.filterwarnings(action='ignore')
```


```python
data.head()
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
      <th>물건번호</th>
      <th>구분</th>
      <th>층수(물건층/전체층)</th>
      <th>월세</th>
      <th>보증금</th>
      <th>계약면적(m2)</th>
      <th>전용면적(m2)</th>
      <th>방향</th>
      <th>확인일자</th>
      <th>이미지</th>
      <th>위도</th>
      <th>경도</th>
      <th>설명</th>
      <th>태그</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2521833244</td>
      <td>오피스텔</td>
      <td>11/16</td>
      <td>105</td>
      <td>500</td>
      <td>29</td>
      <td>19.53</td>
      <td>동향</td>
      <td>25.04.25.</td>
      <td>/20250425_173/1745570410430YI84v_JPEG/677b2f58...</td>
      <td>37.485593</td>
      <td>126.930410</td>
      <td>신림역 초역세권에 위치한 신축오피스텔 1.5룸 입니다.</td>
      <td>[2년이내, 역세권, 소형평수]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2521602111</td>
      <td>원룸</td>
      <td>1/4</td>
      <td>40</td>
      <td>2,000</td>
      <td>16</td>
      <td>16.5</td>
      <td>서향</td>
      <td>25.04.25.</td>
      <td>/20250425_7/17455719623224Hbh2_JPEG/d6b4274e8c...</td>
      <td>37.477661</td>
      <td>126.965002</td>
      <td>위치 좋은 분리형 풀옵션 원룸</td>
      <td>[25년이내, 역세권, 1층]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2521833099</td>
      <td>오피스텔</td>
      <td>11/16</td>
      <td>100</td>
      <td>1,000</td>
      <td>29</td>
      <td>19.53</td>
      <td>동향</td>
      <td>25.04.25.</td>
      <td>/20250425_287/1745570406570jBbFn_JPEG/5e41441b...</td>
      <td>37.485593</td>
      <td>126.930410</td>
      <td>신림역 초역세권에 위치한 신축 풀옵션 프리미엄 오피스텔 1.5룸 입니다.</td>
      <td>[2년이내, 역세권, 소형평수]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2521599464</td>
      <td>원룸</td>
      <td>3/5</td>
      <td>50</td>
      <td>1,000</td>
      <td>16</td>
      <td>16.5</td>
      <td>서향</td>
      <td>25.04.25.</td>
      <td>/20250425_2/1745571851447tu01B_JPEG/05c35bef5b...</td>
      <td>37.474452</td>
      <td>126.964377</td>
      <td>지하철 역 가까운 낙성대역 원룸</td>
      <td>[25년이내, 역세권, 화장실한개]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2521701387</td>
      <td>원룸</td>
      <td>2/6</td>
      <td>60</td>
      <td>3,000</td>
      <td>25</td>
      <td>24</td>
      <td>남서향</td>
      <td>25.04.25.</td>
      <td>/20250425_137/1745570287407vl8lt_JPEG/58d20793...</td>
      <td>37.482340</td>
      <td>126.932285</td>
      <td>1.5룸 리모델링 채광좋고 깔끔해요 큰길가 위치굿 밤길안전</td>
      <td>[25년이내, 융자금적은, 역세권]</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.columns 
```




    Index(['물건번호', '구분', '층수(물건층/전체층)', '월세', '보증금', '계약면적(m2)', '전용면적(m2)', '방향',
           '확인일자', '이미지', '위도', '경도', '설명', '태그'],
          dtype='object')


🔍 Data info 확인

```python
data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 2000 entries, 0 to 1999
    Data columns (total 14 columns):
     #   Column       Non-Null Count  Dtype  
    ---  ------       --------------  -----  
     0   물건번호         2000 non-null   object 
     1   구분           2000 non-null   object 
     2   층수(물건층/전체층)  2000 non-null   object 
     3   월세           2000 non-null   int64  
     4   보증금          1989 non-null   object 
     5   계약면적(m2)     2000 non-null   object 
     6   전용면적(m2)     2000 non-null   object 
     7   방향           2000 non-null   object 
     8   확인일자         2000 non-null   object 
     9   이미지          1950 non-null   object 
     10  위도           2000 non-null   float64
     11  경도           2000 non-null   float64
     12  설명           1802 non-null   object 
     13  태그           2000 non-null   object 
    dtypes: float64(2), int64(1), object(11)
    memory usage: 218.9+ KB
    

✅ data 결정

1. 월세가 0원인 경우는 제외하겠습니다. 

2. 보증금 값이 null값인 경우는 제외하겠습니다. 

3. 보증금이 3000만원 이하의 매물을 찾고 있기 때문에 일단 int 값으로 변환할 때, '억'소리 나는 매물은 제외하겠습니다. 

4. '물건층' 컬럼과 '전체층' 컬럼을 분리하여 1층, 꼭대기층 유무 컬럼을 만들고 확인합니다. 

5. '물건층'에 '저', '중', '고' 로 기재되어 있는 사항은 제외하겠습니다. 

6. 비선호층여부를 새로운 column으로 설정합니다. 

7. 보증금 3000만원 이하 & 지하, 반지하, 꼭대기층 선호하지 않음 & 북향은 선호하지 않음 으로 filter 합니다. 

8. 태그 column에 연식 정보가 포함되어 있는 부분의 데이터만 확인합니다. 

9. 위도, 경도를 이용하여 역까지의 거리를 확인합니다. 

10. 최종 매물을 결정합니다. 

---

### 월세 0원인 경우 제외

```python
data = data.query('월세 > 0')
```

### 보증금 컬럼에 null값이 있는 행 삭제제

```python
data = data.dropna(subset=['보증금'])
```

🔍 보증금의 unique 값 확인

```python
data['보증금'].unique()
```

    array(['500', '2,000', '1,000', '3,000', '3,400', '5,000', '300', '200',
           '100', '2억 1,500', '5,500', '4,000', '6,000', '50', '30', '400',
           '7,000', '1억 2,000', '1억 5,000', '2억 4,500', '6,600', '2억 2,500',
           '1억 3,500', '2억 9,000', '2억 7,000', '1억 2,500', '4억 3,000',
           '3,500', '3,700', '1억', '8,000', '2억 2,000', '1억 300', '1억 3,000',
           '1,500', '8,500', '3억', '1억 7,000', '2억 3,000', '95', '600', '2억',
           '3억 3,000', '7,500', '2억 5,000', '1억 6,000', '9,500', '9,000',
           '35', '2억 1,000', '1억 700', '8,400', '7,400', '1억 8,000',
           '1억 9,000', '2억 8,000', '1억 5,600', '7,990', '9,300', '1,200',
           '1억 1,000', '1억 4,000', '2억 160', '3억 4,000', '1억 2,100',
           '4억 2,000', '3억 6,000', '6,500', '20', '70', '3억 1,000'],
          dtype=object)

### '억'소리 나는 매물 제외외

```python
data = data.query('~보증금.str.contains("억")')
```

🔍 보증금의 unique 값 확인

```python
data['보증금'].unique()
```

    array(['500', '2,000', '1,000', '3,000', '3,400', '5,000', '300', '200',
           '100', '5,500', '4,000', '6,000', '50', '30', '400', '7,000',
           '6,600', '3,500', '3,700', '8,000', '1,500', '8,500', '95', '600',
           '7,500', '9,500', '9,000', '35', '8,400', '7,400', '7,990',
           '9,300', '1,200', '6,500', '20', '70'], dtype=object)

✅ int 값으로 변환 위해 쉼표 제거

```python
data['보증금'] = data['보증금'].str.replace(',','').astype(int)
data['보증금'].unique()
```

    array([ 500, 2000, 1000, 3000, 3400, 5000,  300,  200,  100, 5500, 4000,
           6000,   50,   30,  400, 7000, 6600, 3500, 3700, 8000, 1500, 8500,
             95,  600, 7500, 9500, 9000,   35, 8400, 7400, 7990, 9300, 1200,
           6500,   20,   70])



### 물건층과 전체층 분리 후 1층과 꼭대기층 유무 컬럼 만들기

```python
data[['물건층','전체층']] = data['층수(물건층/전체층)'].str.split('/', expand=True)
```

```python
data['물건층'].unique()
```

    array(['11', '1', '3', '2', '6', '고', 'B1', '5', '4', '저', '중', '9', '12',
           '7', '10', '15', '14', '8', '19', '13', '17'], dtype=object)

✅ 지하이거나 1층이거나 꼭대기 층이면 새로운 column = 비선호층여부에 Boolean 형태로 추가

```python
def floor_info(target, total):
    try:
        if target in ['B1','B2']: #지하이면
            return 'y'
        elif int(target) == 1 or int(target)/int(total) == 1: #1층이거나 꼭대기층이면
            return 'y'
        else:
            return 'n'
    except ValueError:
        return 'n'
```

### '저', '중', '고' 인 물건층은 삭제

```python
data = data[~data['물건층'].isin(['저', '중', '고'])].copy()
```

### 비선호층 여부 새로운 column으로 지정

```python
data['비선호층여부'] = data.apply(lambda x: floor_info(x['물건층'], x['전체층']), axis=1)
```

### 일부 데이터 Filter

✅ 보증금 3000만원 이하

✅ 지하, 반지하, 꼭대기층은 선호하지 않음

✅ 북향은 선호하지 않음

```python
data_filtered = data.query('300 <= 보증금 <= 3000 and 비선호층여부 == "n" and 전체층 != "1" and ~방향.str.contains("북")')
```

🔍 data_filtered 값 확인인

```python
data_filtered
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
      <th>물건번호</th>
      <th>구분</th>
      <th>층수(물건층/전체층)</th>
      <th>월세</th>
      <th>보증금</th>
      <th>계약면적(m2)</th>
      <th>전용면적(m2)</th>
      <th>방향</th>
      <th>확인일자</th>
      <th>이미지</th>
      <th>위도</th>
      <th>경도</th>
      <th>설명</th>
      <th>태그</th>
      <th>물건층</th>
      <th>전체층</th>
      <th>비선호층여부</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2521833244</td>
      <td>오피스텔</td>
      <td>11/16</td>
      <td>105</td>
      <td>500</td>
      <td>29</td>
      <td>19.53</td>
      <td>동향</td>
      <td>25.04.25.</td>
      <td>/20250425_173/1745570410430YI84v_JPEG/677b2f58...</td>
      <td>37.485593</td>
      <td>126.930410</td>
      <td>신림역 초역세권에 위치한 신축오피스텔 1.5룸 입니다.</td>
      <td>[2년이내, 역세권, 소형평수]</td>
      <td>11</td>
      <td>16</td>
      <td>n</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2521833099</td>
      <td>오피스텔</td>
      <td>11/16</td>
      <td>100</td>
      <td>1000</td>
      <td>29</td>
      <td>19.53</td>
      <td>동향</td>
      <td>25.04.25.</td>
      <td>/20250425_287/1745570406570jBbFn_JPEG/5e41441b...</td>
      <td>37.485593</td>
      <td>126.930410</td>
      <td>신림역 초역세권에 위치한 신축 풀옵션 프리미엄 오피스텔 1.5룸 입니다.</td>
      <td>[2년이내, 역세권, 소형평수]</td>
      <td>11</td>
      <td>16</td>
      <td>n</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2521599464</td>
      <td>원룸</td>
      <td>3/5</td>
      <td>50</td>
      <td>1000</td>
      <td>16</td>
      <td>16.5</td>
      <td>서향</td>
      <td>25.04.25.</td>
      <td>/20250425_2/1745571851447tu01B_JPEG/05c35bef5b...</td>
      <td>37.474452</td>
      <td>126.964377</td>
      <td>지하철 역 가까운 낙성대역 원룸</td>
      <td>[25년이내, 역세권, 화장실한개]</td>
      <td>3</td>
      <td>5</td>
      <td>n</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2521701387</td>
      <td>원룸</td>
      <td>2/6</td>
      <td>60</td>
      <td>3000</td>
      <td>25</td>
      <td>24</td>
      <td>남서향</td>
      <td>25.04.25.</td>
      <td>/20250425_137/1745570287407vl8lt_JPEG/58d20793...</td>
      <td>37.482340</td>
      <td>126.932285</td>
      <td>1.5룸 리모델링 채광좋고 깔끔해요 큰길가 위치굿 밤길안전</td>
      <td>[25년이내, 융자금적은, 역세권]</td>
      <td>2</td>
      <td>6</td>
      <td>n</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2521104570</td>
      <td>원룸</td>
      <td>6/9</td>
      <td>110</td>
      <td>3000</td>
      <td>37</td>
      <td>23.65</td>
      <td>동향</td>
      <td>25.04.25.</td>
      <td>/20250425_45/17455719202059YTe8_JPEG/7fb178f5f...</td>
      <td>37.485410</td>
      <td>126.925207</td>
      <td>NaN</td>
      <td>[4년이내, 역세권, 복층]</td>
      <td>6</td>
      <td>9</td>
      <td>n</td>
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
    </tr>
    <tr>
      <th>1986</th>
      <td>2521792852</td>
      <td>빌라</td>
      <td>7/8</td>
      <td>135</td>
      <td>500</td>
      <td>38</td>
      <td>26.59</td>
      <td>동향</td>
      <td>25.04.23.</td>
      <td>/20250423_199/1745391632176gKUki_JPEG/17453902...</td>
      <td>37.485537</td>
      <td>126.928089</td>
      <td>단기가능, 보증금조절가능, 신림역 초역세권</td>
      <td>[10년이내, 역세권, 화장실한개]</td>
      <td>7</td>
      <td>8</td>
      <td>n</td>
    </tr>
    <tr>
      <th>1988</th>
      <td>2521791561</td>
      <td>원룸</td>
      <td>2/4</td>
      <td>35</td>
      <td>500</td>
      <td>23</td>
      <td>23.14</td>
      <td>남동향</td>
      <td>25.04.23.</td>
      <td>/20250423_61/1745390959162jeIIX_JPEG/68088a41a...</td>
      <td>37.482718</td>
      <td>126.939435</td>
      <td>NaN</td>
      <td>[25년이상, 역세권, 화장실한개]</td>
      <td>2</td>
      <td>4</td>
      <td>n</td>
    </tr>
    <tr>
      <th>1990</th>
      <td>2521765720</td>
      <td>원룸</td>
      <td>4/7</td>
      <td>35</td>
      <td>300</td>
      <td>19</td>
      <td>18.18</td>
      <td>남서향</td>
      <td>25.04.23.</td>
      <td>/20250423_129/1745385935394sSJRq_JPEG/6735a775...</td>
      <td>37.480130</td>
      <td>126.946794</td>
      <td>서울대입구역 역세권가성비 최고채광 눈부신 방</td>
      <td>[25년이내, 화장실한개, 소형평수]</td>
      <td>4</td>
      <td>7</td>
      <td>n</td>
    </tr>
    <tr>
      <th>1998</th>
      <td>2521780575</td>
      <td>원룸</td>
      <td>2/5</td>
      <td>50</td>
      <td>1000</td>
      <td>-</td>
      <td>23.14</td>
      <td>남향</td>
      <td>25.04.23.</td>
      <td>/20250423_262/174538878407443SUa_JPEG/KakaoTal...</td>
      <td>37.475296</td>
      <td>126.972970</td>
      <td>사당역 근처 준신축 원룸입니다</td>
      <td>[15년이내, 융자금적은, 화장실한개]</td>
      <td>2</td>
      <td>5</td>
      <td>n</td>
    </tr>
    <tr>
      <th>1999</th>
      <td>2521779947</td>
      <td>원룸</td>
      <td>7/8</td>
      <td>45</td>
      <td>1000</td>
      <td>-</td>
      <td>16.5</td>
      <td>동향</td>
      <td>25.04.23.</td>
      <td>/20250423_234/1745388365054usPt5_JPEG/17453875...</td>
      <td>37.484832</td>
      <td>126.954119</td>
      <td>서울대입구역 역세권 가성비 좋은 원룸</td>
      <td>[15년이내, 융자금없는, 역세권]</td>
      <td>7</td>
      <td>8</td>
      <td>n</td>
    </tr>
  </tbody>
</table>
<p>564 rows × 17 columns</p>
</div>

🔍 data_filtered의 info 확인

```python
data_filtered.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 564 entries, 0 to 1999
    Data columns (total 17 columns):
     #   Column       Non-Null Count  Dtype  
    ---  ------       --------------  -----  
     0   물건번호         564 non-null    object 
     1   구분           564 non-null    object 
     2   층수(물건층/전체층)  564 non-null    object 
     3   월세           564 non-null    int64  
     4   보증금          564 non-null    int32  
     5   계약면적(m2)     564 non-null    object 
     6   전용면적(m2)     564 non-null    object 
     7   방향           564 non-null    object 
     8   확인일자         564 non-null    object 
     9   이미지          557 non-null    object 
     10  위도           564 non-null    float64
     11  경도           564 non-null    float64
     12  설명           492 non-null    object 
     13  태그           564 non-null    object 
     14  물건층          564 non-null    object 
     15  전체층          564 non-null    object 
     16  비선호층여부       564 non-null    object 
    dtypes: float64(2), int32(1), int64(1), object(13)
    memory usage: 77.1+ KB
    
### 태그 컬럼 정리

✅ '태그' 열의 NaN 값을 빈 문자열로 대체하고, 모든 값을 문자열로 변환

✅ 태그 column에 들어 있는 문자열을 처리하여, 그 값을 새로운 컬럼으로 분할하는 작업을 수행합니다. 

```python
data_filtered['태그'] = data_filtered['태그'].fillna('').astype(str)

data_filtered[['tag1', 'tag2', 'tag3', 'tag4']] = data_filtered['태그'].str.replace("\'|\[|\]", "").str.split(', ', expand=True)
```

🔍 data_filtered의 데이터 형태 확인

```python
data_filtered.head()
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
      <th>물건번호</th>
      <th>구분</th>
      <th>층수(물건층/전체층)</th>
      <th>월세</th>
      <th>보증금</th>
      <th>계약면적(m2)</th>
      <th>전용면적(m2)</th>
      <th>방향</th>
      <th>확인일자</th>
      <th>이미지</th>
      <th>...</th>
      <th>경도</th>
      <th>설명</th>
      <th>태그</th>
      <th>물건층</th>
      <th>전체층</th>
      <th>비선호층여부</th>
      <th>tag1</th>
      <th>tag2</th>
      <th>tag3</th>
      <th>tag4</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2521833244</td>
      <td>오피스텔</td>
      <td>11/16</td>
      <td>105</td>
      <td>500</td>
      <td>29</td>
      <td>19.53</td>
      <td>동향</td>
      <td>25.04.25.</td>
      <td>/20250425_173/1745570410430YI84v_JPEG/677b2f58...</td>
      <td>...</td>
      <td>126.930410</td>
      <td>신림역 초역세권에 위치한 신축오피스텔 1.5룸 입니다.</td>
      <td>['2년이내', '역세권', '소형평수']</td>
      <td>11</td>
      <td>16</td>
      <td>n</td>
      <td>2년이내</td>
      <td>역세권</td>
      <td>소형평수</td>
      <td>None</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2521833099</td>
      <td>오피스텔</td>
      <td>11/16</td>
      <td>100</td>
      <td>1000</td>
      <td>29</td>
      <td>19.53</td>
      <td>동향</td>
      <td>25.04.25.</td>
      <td>/20250425_287/1745570406570jBbFn_JPEG/5e41441b...</td>
      <td>...</td>
      <td>126.930410</td>
      <td>신림역 초역세권에 위치한 신축 풀옵션 프리미엄 오피스텔 1.5룸 입니다.</td>
      <td>['2년이내', '역세권', '소형평수']</td>
      <td>11</td>
      <td>16</td>
      <td>n</td>
      <td>2년이내</td>
      <td>역세권</td>
      <td>소형평수</td>
      <td>None</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2521599464</td>
      <td>원룸</td>
      <td>3/5</td>
      <td>50</td>
      <td>1000</td>
      <td>16</td>
      <td>16.5</td>
      <td>서향</td>
      <td>25.04.25.</td>
      <td>/20250425_2/1745571851447tu01B_JPEG/05c35bef5b...</td>
      <td>...</td>
      <td>126.964377</td>
      <td>지하철 역 가까운 낙성대역 원룸</td>
      <td>['25년이내', '역세권', '화장실한개']</td>
      <td>3</td>
      <td>5</td>
      <td>n</td>
      <td>25년이내</td>
      <td>역세권</td>
      <td>화장실한개</td>
      <td>None</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2521701387</td>
      <td>원룸</td>
      <td>2/6</td>
      <td>60</td>
      <td>3000</td>
      <td>25</td>
      <td>24</td>
      <td>남서향</td>
      <td>25.04.25.</td>
      <td>/20250425_137/1745570287407vl8lt_JPEG/58d20793...</td>
      <td>...</td>
      <td>126.932285</td>
      <td>1.5룸 리모델링 채광좋고 깔끔해요 큰길가 위치굿 밤길안전</td>
      <td>['25년이내', '융자금적은', '역세권']</td>
      <td>2</td>
      <td>6</td>
      <td>n</td>
      <td>25년이내</td>
      <td>융자금적은</td>
      <td>역세권</td>
      <td>None</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2521104570</td>
      <td>원룸</td>
      <td>6/9</td>
      <td>110</td>
      <td>3000</td>
      <td>37</td>
      <td>23.65</td>
      <td>동향</td>
      <td>25.04.25.</td>
      <td>/20250425_45/17455719202059YTe8_JPEG/7fb178f5f...</td>
      <td>...</td>
      <td>126.925207</td>
      <td>NaN</td>
      <td>['4년이내', '역세권', '복층']</td>
      <td>6</td>
      <td>9</td>
      <td>n</td>
      <td>4년이내</td>
      <td>역세권</td>
      <td>복층</td>
      <td>None</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>

### 연식 정보가 있는 데이터만 필터링 

✅ 연식 정보가 없으면 제거거

```python
data_filtered = data_filtered.query('tag1.str.contains("년")')
```

```python
data_filtered['연식'] = [int(i[0]) for i in data_filtered['tag1'].str.split('년')]
data_filtered
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
      <th>물건번호</th>
      <th>구분</th>
      <th>층수(물건층/전체층)</th>
      <th>월세</th>
      <th>보증금</th>
      <th>계약면적(m2)</th>
      <th>전용면적(m2)</th>
      <th>방향</th>
      <th>확인일자</th>
      <th>이미지</th>
      <th>...</th>
      <th>설명</th>
      <th>태그</th>
      <th>물건층</th>
      <th>전체층</th>
      <th>비선호층여부</th>
      <th>tag1</th>
      <th>tag2</th>
      <th>tag3</th>
      <th>tag4</th>
      <th>연식</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2521833244</td>
      <td>오피스텔</td>
      <td>11/16</td>
      <td>105</td>
      <td>500</td>
      <td>29</td>
      <td>19.53</td>
      <td>동향</td>
      <td>25.04.25.</td>
      <td>/20250425_173/1745570410430YI84v_JPEG/677b2f58...</td>
      <td>...</td>
      <td>신림역 초역세권에 위치한 신축오피스텔 1.5룸 입니다.</td>
      <td>['2년이내', '역세권', '소형평수']</td>
      <td>11</td>
      <td>16</td>
      <td>n</td>
      <td>2년이내</td>
      <td>역세권</td>
      <td>소형평수</td>
      <td>None</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2521833099</td>
      <td>오피스텔</td>
      <td>11/16</td>
      <td>100</td>
      <td>1000</td>
      <td>29</td>
      <td>19.53</td>
      <td>동향</td>
      <td>25.04.25.</td>
      <td>/20250425_287/1745570406570jBbFn_JPEG/5e41441b...</td>
      <td>...</td>
      <td>신림역 초역세권에 위치한 신축 풀옵션 프리미엄 오피스텔 1.5룸 입니다.</td>
      <td>['2년이내', '역세권', '소형평수']</td>
      <td>11</td>
      <td>16</td>
      <td>n</td>
      <td>2년이내</td>
      <td>역세권</td>
      <td>소형평수</td>
      <td>None</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2521599464</td>
      <td>원룸</td>
      <td>3/5</td>
      <td>50</td>
      <td>1000</td>
      <td>16</td>
      <td>16.5</td>
      <td>서향</td>
      <td>25.04.25.</td>
      <td>/20250425_2/1745571851447tu01B_JPEG/05c35bef5b...</td>
      <td>...</td>
      <td>지하철 역 가까운 낙성대역 원룸</td>
      <td>['25년이내', '역세권', '화장실한개']</td>
      <td>3</td>
      <td>5</td>
      <td>n</td>
      <td>25년이내</td>
      <td>역세권</td>
      <td>화장실한개</td>
      <td>None</td>
      <td>25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2521701387</td>
      <td>원룸</td>
      <td>2/6</td>
      <td>60</td>
      <td>3000</td>
      <td>25</td>
      <td>24</td>
      <td>남서향</td>
      <td>25.04.25.</td>
      <td>/20250425_137/1745570287407vl8lt_JPEG/58d20793...</td>
      <td>...</td>
      <td>1.5룸 리모델링 채광좋고 깔끔해요 큰길가 위치굿 밤길안전</td>
      <td>['25년이내', '융자금적은', '역세권']</td>
      <td>2</td>
      <td>6</td>
      <td>n</td>
      <td>25년이내</td>
      <td>융자금적은</td>
      <td>역세권</td>
      <td>None</td>
      <td>25</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2521104570</td>
      <td>원룸</td>
      <td>6/9</td>
      <td>110</td>
      <td>3000</td>
      <td>37</td>
      <td>23.65</td>
      <td>동향</td>
      <td>25.04.25.</td>
      <td>/20250425_45/17455719202059YTe8_JPEG/7fb178f5f...</td>
      <td>...</td>
      <td>NaN</td>
      <td>['4년이내', '역세권', '복층']</td>
      <td>6</td>
      <td>9</td>
      <td>n</td>
      <td>4년이내</td>
      <td>역세권</td>
      <td>복층</td>
      <td>None</td>
      <td>4</td>
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
      <th>1986</th>
      <td>2521792852</td>
      <td>빌라</td>
      <td>7/8</td>
      <td>135</td>
      <td>500</td>
      <td>38</td>
      <td>26.59</td>
      <td>동향</td>
      <td>25.04.23.</td>
      <td>/20250423_199/1745391632176gKUki_JPEG/17453902...</td>
      <td>...</td>
      <td>단기가능, 보증금조절가능, 신림역 초역세권</td>
      <td>['10년이내', '역세권', '화장실한개']</td>
      <td>7</td>
      <td>8</td>
      <td>n</td>
      <td>10년이내</td>
      <td>역세권</td>
      <td>화장실한개</td>
      <td>None</td>
      <td>10</td>
    </tr>
    <tr>
      <th>1988</th>
      <td>2521791561</td>
      <td>원룸</td>
      <td>2/4</td>
      <td>35</td>
      <td>500</td>
      <td>23</td>
      <td>23.14</td>
      <td>남동향</td>
      <td>25.04.23.</td>
      <td>/20250423_61/1745390959162jeIIX_JPEG/68088a41a...</td>
      <td>...</td>
      <td>NaN</td>
      <td>['25년이상', '역세권', '화장실한개']</td>
      <td>2</td>
      <td>4</td>
      <td>n</td>
      <td>25년이상</td>
      <td>역세권</td>
      <td>화장실한개</td>
      <td>None</td>
      <td>25</td>
    </tr>
    <tr>
      <th>1990</th>
      <td>2521765720</td>
      <td>원룸</td>
      <td>4/7</td>
      <td>35</td>
      <td>300</td>
      <td>19</td>
      <td>18.18</td>
      <td>남서향</td>
      <td>25.04.23.</td>
      <td>/20250423_129/1745385935394sSJRq_JPEG/6735a775...</td>
      <td>...</td>
      <td>서울대입구역 역세권가성비 최고채광 눈부신 방</td>
      <td>['25년이내', '화장실한개', '소형평수']</td>
      <td>4</td>
      <td>7</td>
      <td>n</td>
      <td>25년이내</td>
      <td>화장실한개</td>
      <td>소형평수</td>
      <td>None</td>
      <td>25</td>
    </tr>
    <tr>
      <th>1998</th>
      <td>2521780575</td>
      <td>원룸</td>
      <td>2/5</td>
      <td>50</td>
      <td>1000</td>
      <td>-</td>
      <td>23.14</td>
      <td>남향</td>
      <td>25.04.23.</td>
      <td>/20250423_262/174538878407443SUa_JPEG/KakaoTal...</td>
      <td>...</td>
      <td>사당역 근처 준신축 원룸입니다</td>
      <td>['15년이내', '융자금적은', '화장실한개']</td>
      <td>2</td>
      <td>5</td>
      <td>n</td>
      <td>15년이내</td>
      <td>융자금적은</td>
      <td>화장실한개</td>
      <td>None</td>
      <td>15</td>
    </tr>
    <tr>
      <th>1999</th>
      <td>2521779947</td>
      <td>원룸</td>
      <td>7/8</td>
      <td>45</td>
      <td>1000</td>
      <td>-</td>
      <td>16.5</td>
      <td>동향</td>
      <td>25.04.23.</td>
      <td>/20250423_234/1745388365054usPt5_JPEG/17453875...</td>
      <td>...</td>
      <td>서울대입구역 역세권 가성비 좋은 원룸</td>
      <td>['15년이내', '융자금없는', '역세권']</td>
      <td>7</td>
      <td>8</td>
      <td>n</td>
      <td>15년이내</td>
      <td>융자금없는</td>
      <td>역세권</td>
      <td>None</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
<p>564 rows × 22 columns</p>
</div>

### 필요한 컬럼만 남기기

```python
data_filtered.columns
```


    Index(['물건번호', '구분', '층수(물건층/전체층)', '월세', '보증금', '계약면적(m2)', '전용면적(m2)', '방향',
           '확인일자', '이미지', '위도', '경도', '설명', '태그', '물건층', '전체층', '비선호층여부', 'tag1',
           'tag2', 'tag3', 'tag4', '연식'],
          dtype='object')


```python
data_filtered = data_filtered[['물건번호', '월세','보증금','전용면적(m2)','방향','위도','경도','물건층','전체층','연식']]
data_filtered.head()
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
      <th>물건번호</th>
      <th>월세</th>
      <th>보증금</th>
      <th>전용면적(m2)</th>
      <th>방향</th>
      <th>위도</th>
      <th>경도</th>
      <th>물건층</th>
      <th>전체층</th>
      <th>연식</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2521833244</td>
      <td>105</td>
      <td>500</td>
      <td>19.53</td>
      <td>동향</td>
      <td>37.485593</td>
      <td>126.930410</td>
      <td>11</td>
      <td>16</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2521833099</td>
      <td>100</td>
      <td>1000</td>
      <td>19.53</td>
      <td>동향</td>
      <td>37.485593</td>
      <td>126.930410</td>
      <td>11</td>
      <td>16</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2521599464</td>
      <td>50</td>
      <td>1000</td>
      <td>16.5</td>
      <td>서향</td>
      <td>37.474452</td>
      <td>126.964377</td>
      <td>3</td>
      <td>5</td>
      <td>25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2521701387</td>
      <td>60</td>
      <td>3000</td>
      <td>24</td>
      <td>남서향</td>
      <td>37.482340</td>
      <td>126.932285</td>
      <td>2</td>
      <td>6</td>
      <td>25</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2521104570</td>
      <td>110</td>
      <td>3000</td>
      <td>23.65</td>
      <td>동향</td>
      <td>37.485410</td>
      <td>126.925207</td>
      <td>6</td>
      <td>9</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>




```python
data_filtered_sorted = data_filtered.sort_values(by='월세', ascending=True)
data_filtered_sorted.head()
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
      <th>물건번호</th>
      <th>월세</th>
      <th>보증금</th>
      <th>전용면적(m2)</th>
      <th>방향</th>
      <th>위도</th>
      <th>경도</th>
      <th>물건층</th>
      <th>전체층</th>
      <th>연식</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1007</th>
      <td>2522404469</td>
      <td>15</td>
      <td>3000</td>
      <td>19.84</td>
      <td>남동향</td>
      <td>37.486893</td>
      <td>126.927902</td>
      <td>6</td>
      <td>10</td>
      <td>10</td>
    </tr>
    <tr>
      <th>836</th>
      <td>2522411528</td>
      <td>16</td>
      <td>3000</td>
      <td>19.84</td>
      <td>남향</td>
      <td>37.468225</td>
      <td>126.939859</td>
      <td>2</td>
      <td>4</td>
      <td>15</td>
    </tr>
    <tr>
      <th>489</th>
      <td>2521557867</td>
      <td>20</td>
      <td>3000</td>
      <td>19.84</td>
      <td>남동향</td>
      <td>37.484678</td>
      <td>126.954118</td>
      <td>4</td>
      <td>9</td>
      <td>15</td>
    </tr>
    <tr>
      <th>1064</th>
      <td>2522316428</td>
      <td>25</td>
      <td>2000</td>
      <td>23.1</td>
      <td>남서향</td>
      <td>37.472444</td>
      <td>126.936188</td>
      <td>2</td>
      <td>4</td>
      <td>25</td>
    </tr>
    <tr>
      <th>247</th>
      <td>2520847802</td>
      <td>25</td>
      <td>300</td>
      <td>16.5</td>
      <td>남향</td>
      <td>37.489236</td>
      <td>126.936250</td>
      <td>2</td>
      <td>3</td>
      <td>25</td>
    </tr>
  </tbody>
</table>
</div>



### 위도, 경도를 이용하여 역까지의 거리 재기


```python
coordinate = pd.read_csv('data/서울시역사마스터정보.csv', encoding='cp949')
coordinate = coordinate.query('호선 == "2호선"')
station_list = ['신대방', '신림', '봉천', '서울대입구(관악구청)', '낙성대', '사당']
coordinate.query('역사명 in @station_list')
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
      <th>역사_ID</th>
      <th>역사명</th>
      <th>호선</th>
      <th>위도</th>
      <th>경도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>727</th>
      <td>231</td>
      <td>신대방</td>
      <td>2호선</td>
      <td>37.487462</td>
      <td>126.913149</td>
    </tr>
    <tr>
      <th>728</th>
      <td>230</td>
      <td>신림</td>
      <td>2호선</td>
      <td>37.484201</td>
      <td>126.929715</td>
    </tr>
    <tr>
      <th>729</th>
      <td>229</td>
      <td>봉천</td>
      <td>2호선</td>
      <td>37.482362</td>
      <td>126.941892</td>
    </tr>
    <tr>
      <th>730</th>
      <td>228</td>
      <td>서울대입구(관악구청)</td>
      <td>2호선</td>
      <td>37.481247</td>
      <td>126.952739</td>
    </tr>
    <tr>
      <th>731</th>
      <td>227</td>
      <td>낙성대</td>
      <td>2호선</td>
      <td>37.476930</td>
      <td>126.963693</td>
    </tr>
    <tr>
      <th>732</th>
      <td>226</td>
      <td>사당</td>
      <td>2호선</td>
      <td>37.476538</td>
      <td>126.981544</td>
    </tr>
  </tbody>
</table>
</div>




```python
from haversine import haversine

haversine((37.487462,126.913149), (37.484201,126.929715), unit = 'm')
```




    1505.9854014218404




```python
def distance(station_name, lat, long):
    station_lat = coordinate.query(f'역사명 == "{station_name}"')['위도'].values[0]
    station_long = coordinate.query(f'역사명 == "{station_name}"')['경도'].values[0]

    distance = haversine((station_lat, station_long), (lat, long), unit = 'm')

    return distance
```


```python
for s in station_list:
    data_filtered[s] = data_filtered.apply(lambda x: distance(s, x['위도'], x['경도']), axis=1)
```


```python
data_filtered.head()
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
      <th>물건번호</th>
      <th>월세</th>
      <th>보증금</th>
      <th>전용면적(m2)</th>
      <th>방향</th>
      <th>위도</th>
      <th>경도</th>
      <th>물건층</th>
      <th>전체층</th>
      <th>연식</th>
      <th>신대방</th>
      <th>신림</th>
      <th>봉천</th>
      <th>서울대입구(관악구청)</th>
      <th>낙성대</th>
      <th>사당</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2521833244</td>
      <td>105</td>
      <td>500</td>
      <td>19.53</td>
      <td>동향</td>
      <td>37.485593</td>
      <td>126.930410</td>
      <td>11</td>
      <td>16</td>
      <td>2</td>
      <td>1537.102307</td>
      <td>166.488689</td>
      <td>1074.940774</td>
      <td>2028.634506</td>
      <td>3090.805147</td>
      <td>4623.009206</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2521833099</td>
      <td>100</td>
      <td>1000</td>
      <td>19.53</td>
      <td>동향</td>
      <td>37.485593</td>
      <td>126.930410</td>
      <td>11</td>
      <td>16</td>
      <td>2</td>
      <td>1537.102307</td>
      <td>166.488689</td>
      <td>1074.940774</td>
      <td>2028.634506</td>
      <td>3090.805147</td>
      <td>4623.009206</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2521599464</td>
      <td>50</td>
      <td>1000</td>
      <td>16.5</td>
      <td>서향</td>
      <td>37.474452</td>
      <td>126.964377</td>
      <td>3</td>
      <td>5</td>
      <td>25</td>
      <td>4746.176620</td>
      <td>3245.045916</td>
      <td>2170.344483</td>
      <td>1274.975315</td>
      <td>282.075176</td>
      <td>1532.572482</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2521701387</td>
      <td>60</td>
      <td>3000</td>
      <td>24</td>
      <td>남서향</td>
      <td>37.482340</td>
      <td>126.932285</td>
      <td>2</td>
      <td>6</td>
      <td>25</td>
      <td>1781.931451</td>
      <td>306.994506</td>
      <td>847.704413</td>
      <td>1808.917607</td>
      <td>2836.009990</td>
      <td>4394.294402</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2521104570</td>
      <td>110</td>
      <td>3000</td>
      <td>23.65</td>
      <td>동향</td>
      <td>37.485410</td>
      <td>126.925207</td>
      <td>6</td>
      <td>9</td>
      <td>4</td>
      <td>1088.106080</td>
      <td>419.866894</td>
      <td>1510.726470</td>
      <td>2473.041956</td>
      <td>3524.453302</td>
      <td>5068.088810</td>
    </tr>
  </tbody>
</table>
</div>


✅ 역까지 최소 거리를 data_filtered에 추가하기기

```python
data_filtered['역까지최소거리'] = data_filtered.apply(lambda x: min([x['신대방'], x['신림'], x['봉천'], x['서울대입구(관악구청)'], x['낙성대'], x['사당']]), axis=1)
data_filtered.head()
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
      <th>물건번호</th>
      <th>월세</th>
      <th>보증금</th>
      <th>전용면적(m2)</th>
      <th>방향</th>
      <th>위도</th>
      <th>경도</th>
      <th>물건층</th>
      <th>전체층</th>
      <th>연식</th>
      <th>신대방</th>
      <th>신림</th>
      <th>봉천</th>
      <th>서울대입구(관악구청)</th>
      <th>낙성대</th>
      <th>사당</th>
      <th>역까지최소거리</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2521833244</td>
      <td>105</td>
      <td>500</td>
      <td>19.53</td>
      <td>동향</td>
      <td>37.485593</td>
      <td>126.930410</td>
      <td>11</td>
      <td>16</td>
      <td>2</td>
      <td>1537.102307</td>
      <td>166.488689</td>
      <td>1074.940774</td>
      <td>2028.634506</td>
      <td>3090.805147</td>
      <td>4623.009206</td>
      <td>166.488689</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2521833099</td>
      <td>100</td>
      <td>1000</td>
      <td>19.53</td>
      <td>동향</td>
      <td>37.485593</td>
      <td>126.930410</td>
      <td>11</td>
      <td>16</td>
      <td>2</td>
      <td>1537.102307</td>
      <td>166.488689</td>
      <td>1074.940774</td>
      <td>2028.634506</td>
      <td>3090.805147</td>
      <td>4623.009206</td>
      <td>166.488689</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2521599464</td>
      <td>50</td>
      <td>1000</td>
      <td>16.5</td>
      <td>서향</td>
      <td>37.474452</td>
      <td>126.964377</td>
      <td>3</td>
      <td>5</td>
      <td>25</td>
      <td>4746.176620</td>
      <td>3245.045916</td>
      <td>2170.344483</td>
      <td>1274.975315</td>
      <td>282.075176</td>
      <td>1532.572482</td>
      <td>282.075176</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2521701387</td>
      <td>60</td>
      <td>3000</td>
      <td>24</td>
      <td>남서향</td>
      <td>37.482340</td>
      <td>126.932285</td>
      <td>2</td>
      <td>6</td>
      <td>25</td>
      <td>1781.931451</td>
      <td>306.994506</td>
      <td>847.704413</td>
      <td>1808.917607</td>
      <td>2836.009990</td>
      <td>4394.294402</td>
      <td>306.994506</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2521104570</td>
      <td>110</td>
      <td>3000</td>
      <td>23.65</td>
      <td>동향</td>
      <td>37.485410</td>
      <td>126.925207</td>
      <td>6</td>
      <td>9</td>
      <td>4</td>
      <td>1088.106080</td>
      <td>419.866894</td>
      <td>1510.726470</td>
      <td>2473.041956</td>
      <td>3524.453302</td>
      <td>5068.088810</td>
      <td>419.866894</td>
    </tr>
  </tbody>
</table>
</div>




```python
data_filtered.drop(station_list, axis=1, inplace=True)
data_filtered.head()
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
      <th>물건번호</th>
      <th>월세</th>
      <th>보증금</th>
      <th>전용면적(m2)</th>
      <th>방향</th>
      <th>위도</th>
      <th>경도</th>
      <th>물건층</th>
      <th>전체층</th>
      <th>연식</th>
      <th>역까지최소거리</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2521833244</td>
      <td>105</td>
      <td>500</td>
      <td>19.53</td>
      <td>동향</td>
      <td>37.485593</td>
      <td>126.930410</td>
      <td>11</td>
      <td>16</td>
      <td>2</td>
      <td>166.488689</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2521833099</td>
      <td>100</td>
      <td>1000</td>
      <td>19.53</td>
      <td>동향</td>
      <td>37.485593</td>
      <td>126.930410</td>
      <td>11</td>
      <td>16</td>
      <td>2</td>
      <td>166.488689</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2521599464</td>
      <td>50</td>
      <td>1000</td>
      <td>16.5</td>
      <td>서향</td>
      <td>37.474452</td>
      <td>126.964377</td>
      <td>3</td>
      <td>5</td>
      <td>25</td>
      <td>282.075176</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2521701387</td>
      <td>60</td>
      <td>3000</td>
      <td>24</td>
      <td>남서향</td>
      <td>37.482340</td>
      <td>126.932285</td>
      <td>2</td>
      <td>6</td>
      <td>25</td>
      <td>306.994506</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2521104570</td>
      <td>110</td>
      <td>3000</td>
      <td>23.65</td>
      <td>동향</td>
      <td>37.485410</td>
      <td>126.925207</td>
      <td>6</td>
      <td>9</td>
      <td>4</td>
      <td>419.866894</td>
    </tr>
  </tbody>
</table>
</div>



## 분석

### 각 항목 분포 확인하기


```python
for x in ['월세','보증금','전용면적(m2)','연식','역까지최소거리']:
    fig = px.box(data_frame = data_filtered, x=x, width=700, height=400)
    fig.show()
```

![PNG](/assets/images/Feature_Engineering/1/1.png)

![PNG](/assets/images/Feature_Engineering/1/2.png)

![PNG](/assets/images/Feature_Engineering/1/3.png)




- 내가 원하는 조건은?
    - 보증금 3000만원 이하 ✅
    - 월세는 저렴할수록 좋음
    - 지하, 반지하, 꼭대기층은 선호하지 않음 ✅
    - 전용면적이 클수록 좋음
    - 북향은 선호하지 않음 ✅
    - 연식이 오래되지 않을수록 좋음
    - 지하철 역에서 가까울수록 좋음



### 월세, 전용면적, 연식, 지하철역까지의 거리 점수 매기기

🔍 data_filtered의 info 확인

```python
data_filtered.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 564 entries, 0 to 1999
    Data columns (total 11 columns):
     #   Column    Non-Null Count  Dtype  
    ---  ------    --------------  -----  
     0   물건번호      564 non-null    object 
     1   월세        564 non-null    int64  
     2   보증금       564 non-null    int32  
     3   전용면적(m2)  564 non-null    object 
     4   방향        564 non-null    object 
     5   위도        564 non-null    float64
     6   경도        564 non-null    float64
     7   물건층       564 non-null    object 
     8   전체층       564 non-null    object 
     9   연식        564 non-null    int64  
     10  역까지최소거리   564 non-null    float64
    dtypes: float64(3), int32(1), int64(2), object(5)
    memory usage: 50.7+ KB
    
✅ 내가 원하는 자취방의 조건 등급을 설정합니다. 

```python
data_filtered['전용면적(m2)'] = pd.to_numeric(data_filtered['전용면적(m2)'], errors='coerce')
```


```python
data_filtered['월세_등급'] = pd.qcut(data_filtered['월세'], 5, labels=[1,2,3,4,5])
data_filtered['전용면적_등급'] = pd.qcut(data_filtered['전용면적(m2)'], 5, labels=[5,4,3,2,1])
data_filtered['연식_등급'] = pd.qcut(data_filtered['연식'].rank(method='first'), 5, labels=[1,2,3,4,5])
data_filtered['역까지최소거리_등급'] = pd.qcut(data_filtered['역까지최소거리'], 5, labels=[1,2,3,4,5])
```

🔍 data_filtered의 데이터 형태 확인

```python
data_filtered
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
      <th>물건번호</th>
      <th>월세</th>
      <th>보증금</th>
      <th>전용면적(m2)</th>
      <th>방향</th>
      <th>위도</th>
      <th>경도</th>
      <th>물건층</th>
      <th>전체층</th>
      <th>연식</th>
      <th>역까지최소거리</th>
      <th>월세_등급</th>
      <th>전용면적_등급</th>
      <th>연식_등급</th>
      <th>역까지최소거리_등급</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2521833244</td>
      <td>105</td>
      <td>500</td>
      <td>19.53</td>
      <td>동향</td>
      <td>37.485593</td>
      <td>126.930410</td>
      <td>11</td>
      <td>16</td>
      <td>2</td>
      <td>166.488689</td>
      <td>5</td>
      <td>4</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2521833099</td>
      <td>100</td>
      <td>1000</td>
      <td>19.53</td>
      <td>동향</td>
      <td>37.485593</td>
      <td>126.930410</td>
      <td>11</td>
      <td>16</td>
      <td>2</td>
      <td>166.488689</td>
      <td>5</td>
      <td>4</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2521599464</td>
      <td>50</td>
      <td>1000</td>
      <td>16.50</td>
      <td>서향</td>
      <td>37.474452</td>
      <td>126.964377</td>
      <td>3</td>
      <td>5</td>
      <td>25</td>
      <td>282.075176</td>
      <td>3</td>
      <td>5</td>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2521701387</td>
      <td>60</td>
      <td>3000</td>
      <td>24.00</td>
      <td>남서향</td>
      <td>37.482340</td>
      <td>126.932285</td>
      <td>2</td>
      <td>6</td>
      <td>25</td>
      <td>306.994506</td>
      <td>4</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2521104570</td>
      <td>110</td>
      <td>3000</td>
      <td>23.65</td>
      <td>동향</td>
      <td>37.485410</td>
      <td>126.925207</td>
      <td>6</td>
      <td>9</td>
      <td>4</td>
      <td>419.866894</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
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
    </tr>
    <tr>
      <th>1986</th>
      <td>2521792852</td>
      <td>135</td>
      <td>500</td>
      <td>26.59</td>
      <td>동향</td>
      <td>37.485537</td>
      <td>126.928089</td>
      <td>7</td>
      <td>8</td>
      <td>10</td>
      <td>206.525251</td>
      <td>5</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1988</th>
      <td>2521791561</td>
      <td>35</td>
      <td>500</td>
      <td>23.14</td>
      <td>남동향</td>
      <td>37.482718</td>
      <td>126.939435</td>
      <td>2</td>
      <td>4</td>
      <td>25</td>
      <td>220.384134</td>
      <td>1</td>
      <td>2</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1990</th>
      <td>2521765720</td>
      <td>35</td>
      <td>300</td>
      <td>18.18</td>
      <td>남서향</td>
      <td>37.480130</td>
      <td>126.946794</td>
      <td>4</td>
      <td>7</td>
      <td>25</td>
      <td>498.693289</td>
      <td>1</td>
      <td>4</td>
      <td>5</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1998</th>
      <td>2521780575</td>
      <td>50</td>
      <td>1000</td>
      <td>23.14</td>
      <td>남향</td>
      <td>37.475296</td>
      <td>126.972970</td>
      <td>2</td>
      <td>5</td>
      <td>15</td>
      <td>769.117081</td>
      <td>3</td>
      <td>2</td>
      <td>3</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1999</th>
      <td>2521779947</td>
      <td>45</td>
      <td>1000</td>
      <td>16.50</td>
      <td>동향</td>
      <td>37.484832</td>
      <td>126.954119</td>
      <td>7</td>
      <td>8</td>
      <td>15</td>
      <td>416.817201</td>
      <td>2</td>
      <td>5</td>
      <td>3</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
<p>564 rows × 15 columns</p>
</div>

### 최종 매물 결정하기

```python
data_filtered_final = data_filtered.query('월세_등급 <= 3 and 전용면적_등급 <= 1 and 연식_등급 <= 2 and 역까지최소거리_등급 <= 2')
data_filtered_final
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
      <th>물건번호</th>
      <th>월세</th>
      <th>보증금</th>
      <th>전용면적(m2)</th>
      <th>방향</th>
      <th>위도</th>
      <th>경도</th>
      <th>물건층</th>
      <th>전체층</th>
      <th>연식</th>
      <th>역까지최소거리</th>
      <th>월세_등급</th>
      <th>전용면적_등급</th>
      <th>연식_등급</th>
      <th>역까지최소거리_등급</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1178</th>
      <td>2522096126</td>
      <td>45</td>
      <td>1000</td>
      <td>29.05</td>
      <td>남향</td>
      <td>37.487152</td>
      <td>126.930633</td>
      <td>6</td>
      <td>7</td>
      <td>10</td>
      <td>337.985911</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1374</th>
      <td>2522216246</td>
      <td>42</td>
      <td>500</td>
      <td>26.45</td>
      <td>남동향</td>
      <td>37.482455</td>
      <td>126.955225</td>
      <td>2</td>
      <td>5</td>
      <td>2</td>
      <td>257.219698</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1974</th>
      <td>2521795532</td>
      <td>45</td>
      <td>1000</td>
      <td>24.10</td>
      <td>남향</td>
      <td>37.478044</td>
      <td>126.965745</td>
      <td>2</td>
      <td>3</td>
      <td>10</td>
      <td>219.391335</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



### 최종 매물 리스트 시각화


```python
f = folium.Figure(width=700, height=500)
m = folium.Map(location=[37.486313, 126.935378], zoom_start=14).add_to(f)

for idx in data_filtered_final.index:
    lat = data_filtered_final.loc[idx, '위도']
    long = data_filtered_final.loc[idx, '경도']
    num = data_filtered_final.loc[idx, '물건번호']

    folium.Marker([lat, long]
                  , popup=f"<a href=https://m.land.naver.com/article/info/{num}>링크</a>"
                  ).add_to(m)
m
```

![PNG](/assets/images/Feature_Engineering/1/4.png)
