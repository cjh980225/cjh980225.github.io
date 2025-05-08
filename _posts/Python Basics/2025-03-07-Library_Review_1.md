---
title: "[Library_Review] Pandas library" 


categories: Python_Basics


tag: [python, library, library_review, pandas]
---

Pandas Library

* [10 minutes to pandas 문서 참고](https://pandas.pydata.org/docs/user_guide/10min.html)


```python
import numpy as np 
import pandas as pd 
```


```python
# 현재 사용하고 있는 python version
import sys

sys.version
```




    '3.9.12 (main, Apr  4 2022, 05:22:27) [MSC v.1916 64 bit (AMD64)]'




```python
# 현재 사용하고 있는 pandas version
pd.__version__
```




    '1.4.4'




```python
# 현재 사용하고 있는 numpy version
np.__version__
```




    '1.21.5'



---

# Basic data structures in pandas 
* `Series` : 벡터 / 일차원 레이블 배열
* `DataFrame` : 이차원 배열 / 행렬 

---

# Ojbect creation 


```python
s = pd.Series([1, 3, 5, np.nan, 6, 8]) # Series ->  벡터 
s
```




    0    1.0
    1    3.0
    2    5.0
    3    NaN
    4    6.0
    5    8.0
    dtype: float64



* datetime 인덱스를 사용하여 Numpy 배열을 전달하고, 레이블에 지정된 열을 사용하여 다음을 만듭니다.


```python
dates = pd.date_range("20130101", periods = 6) # DataFrame -> 행렬
dates
```




    DatetimeIndex(['2013-01-01', '2013-01-02', '2013-01-03', '2013-01-04',
                   '2013-01-05', '2013-01-06'],
                  dtype='datetime64[ns]', freq='D')



* DataFrame 키가 열 레이블이고 값이 열 값인 객체 사전을 전달하여 생성합니다. 


```python
df = pd.DataFrame(np.random.randn(6, 4), index = dates, columns = list("ABCD"))
df
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-01</th>
      <td>0.570644</td>
      <td>-0.759579</td>
      <td>-0.360142</td>
      <td>-1.724012</td>
    </tr>
    <tr>
      <th>2013-01-02</th>
      <td>0.297219</td>
      <td>0.382312</td>
      <td>0.822667</td>
      <td>0.314014</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>0.834170</td>
      <td>0.076217</td>
      <td>-0.812529</td>
      <td>-1.911790</td>
    </tr>
    <tr>
      <th>2013-01-04</th>
      <td>-0.022074</td>
      <td>-0.967295</td>
      <td>-0.576004</td>
      <td>0.746524</td>
    </tr>
    <tr>
      <th>2013-01-05</th>
      <td>0.701261</td>
      <td>0.355139</td>
      <td>-0.485506</td>
      <td>0.780679</td>
    </tr>
    <tr>
      <th>2013-01-06</th>
      <td>1.609882</td>
      <td>0.022514</td>
      <td>0.026405</td>
      <td>-0.007853</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2 = pd.DataFrame(
    {
        "A" : 1.0, 
        "B" : pd.Timestamp("20130102"), 
        "C" : pd.Series(1, index = list(range(4)), dtype = "float32"), 
        "D" : np.array([3] * 4, dtype = "int32"), 
        "E" : pd.Categorical(["test", "train", "test", "train"]), 
        "F" : "foo", 
    }
)
df2
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>E</th>
      <th>F</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>2013-01-02</td>
      <td>1.0</td>
      <td>3</td>
      <td>test</td>
      <td>foo</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.0</td>
      <td>2013-01-02</td>
      <td>1.0</td>
      <td>3</td>
      <td>train</td>
      <td>foo</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1.0</td>
      <td>2013-01-02</td>
      <td>1.0</td>
      <td>3</td>
      <td>test</td>
      <td>foo</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1.0</td>
      <td>2013-01-02</td>
      <td>1.0</td>
      <td>3</td>
      <td>train</td>
      <td>foo</td>
    </tr>
  </tbody>
</table>
</div>



* 결과의 열에는 서로 다른 dtype이 DataFrame에 있습니다. 


```python
df2.dtypes
```




    A           float64
    B    datetime64[ns]
    C           float32
    D             int32
    E          category
    F            object
    dtype: object



---

# Viewing data 

* DataFrame.head()를 이용하여 data의 상위 5열을 확인할 수 있습니다. 


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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-01</th>
      <td>0.570644</td>
      <td>-0.759579</td>
      <td>-0.360142</td>
      <td>-1.724012</td>
    </tr>
    <tr>
      <th>2013-01-02</th>
      <td>0.297219</td>
      <td>0.382312</td>
      <td>0.822667</td>
      <td>0.314014</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>0.834170</td>
      <td>0.076217</td>
      <td>-0.812529</td>
      <td>-1.911790</td>
    </tr>
    <tr>
      <th>2013-01-04</th>
      <td>-0.022074</td>
      <td>-0.967295</td>
      <td>-0.576004</td>
      <td>0.746524</td>
    </tr>
    <tr>
      <th>2013-01-05</th>
      <td>0.701261</td>
      <td>0.355139</td>
      <td>-0.485506</td>
      <td>0.780679</td>
    </tr>
  </tbody>
</table>
</div>



* DataFrame.tail()을 이용하여 data의 하위 열을 확인할 수 있습니다.
* 괄호 안에 숫자만큼 하위 열의 갯수를 지정할 수 있습니다. 


```python
df.tail(3)
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-04</th>
      <td>-0.022074</td>
      <td>-0.967295</td>
      <td>-0.576004</td>
      <td>0.746524</td>
    </tr>
    <tr>
      <th>2013-01-05</th>
      <td>0.701261</td>
      <td>0.355139</td>
      <td>-0.485506</td>
      <td>0.780679</td>
    </tr>
    <tr>
      <th>2013-01-06</th>
      <td>1.609882</td>
      <td>0.022514</td>
      <td>0.026405</td>
      <td>-0.007853</td>
    </tr>
  </tbody>
</table>
</div>



* DataFrame.index를 통해 index = row = (행 이름)을 확인할 수 있습니다. 


```python
df.index 
```




    DatetimeIndex(['2013-01-01', '2013-01-02', '2013-01-03', '2013-01-04',
                   '2013-01-05', '2013-01-06'],
                  dtype='datetime64[ns]', freq='D')



* DataFrame.columns를 통해 columns = (열 이름)을 확인할 수 있습니다. 


```python
df.columns
```




    Index(['A', 'B', 'C', 'D'], dtype='object')



* DataFrame.to_numpy()를 통해 index나 열 레이블 없이 기본 데이터의 Numpy 표현을 확인할 수 있습니다. 


```python
df.to_numpy()
```




    array([[ 0.57064351, -0.75957894, -0.36014189, -1.72401249],
           [ 0.29721918,  0.38231153,  0.82266738,  0.31401374],
           [ 0.83416968,  0.07621652, -0.81252874, -1.91179028],
           [-0.02207392, -0.9672946 , -0.57600396,  0.74652411],
           [ 0.70126115,  0.35513892, -0.48550626,  0.78067905],
           [ 1.60988213,  0.02251429,  0.02640489, -0.00785282]])



* Numpy -> 배열 전체에 대해 하나의 dtype을 가집니다.
* pandas DataFrame는 열(column)당 하나의 dtype을 가집니다. 

* DataFrame.describe()를 통해 간단한 통계 요약을 확인할 수 있습니다. 


```python
df.describe()
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>6.000000</td>
      <td>6.000000</td>
      <td>6.000000</td>
      <td>6.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.665184</td>
      <td>-0.148449</td>
      <td>-0.230851</td>
      <td>-0.300406</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.554792</td>
      <td>0.576021</td>
      <td>0.585684</td>
      <td>1.212528</td>
    </tr>
    <tr>
      <th>min</th>
      <td>-0.022074</td>
      <td>-0.967295</td>
      <td>-0.812529</td>
      <td>-1.911790</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.365575</td>
      <td>-0.564056</td>
      <td>-0.553380</td>
      <td>-1.294973</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.635952</td>
      <td>0.049365</td>
      <td>-0.422824</td>
      <td>0.153080</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.800943</td>
      <td>0.285408</td>
      <td>-0.070232</td>
      <td>0.638397</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1.609882</td>
      <td>0.382312</td>
      <td>0.822667</td>
      <td>0.780679</td>
    </tr>
  </tbody>
</table>
</div>



* DataFrame.T 를 통해 데이터프레임의 전치를 확인할 수 있습니다. 


```python
df.T
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
      <th>2013-01-01</th>
      <th>2013-01-02</th>
      <th>2013-01-03</th>
      <th>2013-01-04</th>
      <th>2013-01-05</th>
      <th>2013-01-06</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>A</th>
      <td>0.570644</td>
      <td>0.297219</td>
      <td>0.834170</td>
      <td>-0.022074</td>
      <td>0.701261</td>
      <td>1.609882</td>
    </tr>
    <tr>
      <th>B</th>
      <td>-0.759579</td>
      <td>0.382312</td>
      <td>0.076217</td>
      <td>-0.967295</td>
      <td>0.355139</td>
      <td>0.022514</td>
    </tr>
    <tr>
      <th>C</th>
      <td>-0.360142</td>
      <td>0.822667</td>
      <td>-0.812529</td>
      <td>-0.576004</td>
      <td>-0.485506</td>
      <td>0.026405</td>
    </tr>
    <tr>
      <th>D</th>
      <td>-1.724012</td>
      <td>0.314014</td>
      <td>-1.911790</td>
      <td>0.746524</td>
      <td>0.780679</td>
      <td>-0.007853</td>
    </tr>
  </tbody>
</table>
</div>



* DataFrame.sort_index()를 이용하여 지정된 축에 따라 정렬합니다. 
* axis = 0 -> columns / axis = 1 -> row 입니다. / default -> axis = 0 
* ascending = False -> 내림차순 / ascending = True -> 오름차순 입니다. / default -> ascending = True


```python
df.sort_index(axis = 1, ascending = False)
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
      <th>D</th>
      <th>C</th>
      <th>B</th>
      <th>A</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-01</th>
      <td>-1.724012</td>
      <td>-0.360142</td>
      <td>-0.759579</td>
      <td>0.570644</td>
    </tr>
    <tr>
      <th>2013-01-02</th>
      <td>0.314014</td>
      <td>0.822667</td>
      <td>0.382312</td>
      <td>0.297219</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>-1.911790</td>
      <td>-0.812529</td>
      <td>0.076217</td>
      <td>0.834170</td>
    </tr>
    <tr>
      <th>2013-01-04</th>
      <td>0.746524</td>
      <td>-0.576004</td>
      <td>-0.967295</td>
      <td>-0.022074</td>
    </tr>
    <tr>
      <th>2013-01-05</th>
      <td>0.780679</td>
      <td>-0.485506</td>
      <td>0.355139</td>
      <td>0.701261</td>
    </tr>
    <tr>
      <th>2013-01-06</th>
      <td>-0.007853</td>
      <td>0.026405</td>
      <td>0.022514</td>
      <td>1.609882</td>
    </tr>
  </tbody>
</table>
</div>



* DataFrame.sort_values()를 이용하여 column을 지정하고, 지정된 값에 따라 정렬합니다. 
* default -> axis = 0 / axis = 1 지정 후 row 값 지정도 가능합니다. 


```python
df.sort_values(by = "B")
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-04</th>
      <td>-0.022074</td>
      <td>-0.967295</td>
      <td>-0.576004</td>
      <td>0.746524</td>
    </tr>
    <tr>
      <th>2013-01-01</th>
      <td>0.570644</td>
      <td>-0.759579</td>
      <td>-0.360142</td>
      <td>-1.724012</td>
    </tr>
    <tr>
      <th>2013-01-06</th>
      <td>1.609882</td>
      <td>0.022514</td>
      <td>0.026405</td>
      <td>-0.007853</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>0.834170</td>
      <td>0.076217</td>
      <td>-0.812529</td>
      <td>-1.911790</td>
    </tr>
    <tr>
      <th>2013-01-05</th>
      <td>0.701261</td>
      <td>0.355139</td>
      <td>-0.485506</td>
      <td>0.780679</td>
    </tr>
    <tr>
      <th>2013-01-02</th>
      <td>0.297219</td>
      <td>0.382312</td>
      <td>0.822667</td>
      <td>0.314014</td>
    </tr>
  </tbody>
</table>
</div>



## Summary Lession 3 

|문법|내용|
|-|-|
|df.head()|상위 다섯개 행 출력 / 괄호 안 숫자를 통해 행 출력 개수 지정 가능|
|df.tail()|하위 다섯개 행 출력 / 괄호 안 숫자를 통해 행 출력 개수 지정 가능|
|df.index|DataFrame의 index = row = (행 이름) 확인|
|df.columns|DataFrame의 columns = (열이름) 확인|
|df.to_numpy|DataFrame의 기본 데이터의 Numpy 표현 확인|
|df.describe()|DataFrame의 간단한 통계 요약 확인|
|df.T|DataFrame의 전치 확인|
|df.sort_index(axis = 0 or 1)|axis = (0, 열), (1, 행) 기준으로 정렬|
|df.sort_values(by = "B")|by 값에 따라 정렬|

---

# Selection

## Getitem([])

* DataFrame의 단일 레이블을 선택하면 열이 전달되고, Series로 출력됩니다. 


```python
df["A"]
```




    2013-01-01    0.570644
    2013-01-02    0.297219
    2013-01-03    0.834170
    2013-01-04   -0.022074
    2013-01-05    0.701261
    2013-01-06    1.609882
    Freq: D, Name: A, dtype: float64



* DataFrame의 슬라이스를 전달하면 그것에 일치하는 행이 선택됩니다. 


```python
df[0:3]
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-01</th>
      <td>0.570644</td>
      <td>-0.759579</td>
      <td>-0.360142</td>
      <td>-1.724012</td>
    </tr>
    <tr>
      <th>2013-01-02</th>
      <td>0.297219</td>
      <td>0.382312</td>
      <td>0.822667</td>
      <td>0.314014</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>0.834170</td>
      <td>0.076217</td>
      <td>-0.812529</td>
      <td>-1.911790</td>
    </tr>
  </tbody>
</table>
</div>




```python
df["20130102":"20130104"]
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-02</th>
      <td>0.297219</td>
      <td>0.382312</td>
      <td>0.822667</td>
      <td>0.314014</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>0.834170</td>
      <td>0.076217</td>
      <td>-0.812529</td>
      <td>-1.911790</td>
    </tr>
    <tr>
      <th>2013-01-04</th>
      <td>-0.022074</td>
      <td>-0.967295</td>
      <td>-0.576004</td>
      <td>0.746524</td>
    </tr>
  </tbody>
</table>
</div>



## Selection by label 

* label과 일치하는 행 선택


```python
df.loc[dates[0]]
```




    A    0.570644
    B   -0.759579
    C   -0.360142
    D   -1.724012
    Name: 2013-01-01 00:00:00, dtype: float64



* 선택된 열 레이블이 있는 모든 행 선택


```python
df.loc[:, ["A", "B"]]
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
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-01</th>
      <td>0.570644</td>
      <td>-0.759579</td>
    </tr>
    <tr>
      <th>2013-01-02</th>
      <td>0.297219</td>
      <td>0.382312</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>0.834170</td>
      <td>0.076217</td>
    </tr>
    <tr>
      <th>2013-01-04</th>
      <td>-0.022074</td>
      <td>-0.967295</td>
    </tr>
    <tr>
      <th>2013-01-05</th>
      <td>0.701261</td>
      <td>0.355139</td>
    </tr>
    <tr>
      <th>2013-01-06</th>
      <td>1.609882</td>
      <td>0.022514</td>
    </tr>
  </tbody>
</table>
</div>



* 라벨 슬라이싱의 경우 두 종점이 모두 포함됩니다. 


```python
df.loc["20130102":"20130104", ["A", "B"]]
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
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-02</th>
      <td>0.297219</td>
      <td>0.382312</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>0.834170</td>
      <td>0.076217</td>
    </tr>
    <tr>
      <th>2013-01-04</th>
      <td>-0.022074</td>
      <td>-0.967295</td>
    </tr>
  </tbody>
</table>
</div>



* 단일 행, 열을 선택하면 스칼라가 반환됩니다. 


```python
df.loc[dates[0], "A"]
```




    0.5706435136569595



* 스칼라에 빠르게 접근하려면 dataFrame.at[]을 사용합니다. 


```python
df.at[dates[0], "A"]
```




    0.5706435136569595



## Selection by position

* 정수 슬라이스는 DataFrame.iloc[] 를 사용합니다. 
* 전달된 정수의 위치를 통해 선택됩니다. 


```python
df.iloc[3]
```




    A   -0.022074
    B   -0.967295
    C   -0.576004
    D    0.746524
    Name: 2013-01-04 00:00:00, dtype: float64



* 정수 슬라이스는 Numpy / Python과 유사하게 작동합니다. 


```python
df.iloc[3:5, 0:2]
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
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-04</th>
      <td>-0.022074</td>
      <td>-0.967295</td>
    </tr>
    <tr>
      <th>2013-01-05</th>
      <td>0.701261</td>
      <td>0.355139</td>
    </tr>
  </tbody>
</table>
</div>



* 정수 위치 목록으로도 슬라이싱 가능합니다. 


```python
df.iloc[[1, 2, 4], [0, 2]]
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
      <th>A</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-02</th>
      <td>0.297219</td>
      <td>0.822667</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>0.834170</td>
      <td>-0.812529</td>
    </tr>
    <tr>
      <th>2013-01-05</th>
      <td>0.701261</td>
      <td>-0.485506</td>
    </tr>
  </tbody>
</table>
</div>



* 열을 명시적으로 슬라이싱 하는 경우 


```python
df.iloc[:, 1:3]
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
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-01</th>
      <td>-0.759579</td>
      <td>-0.360142</td>
    </tr>
    <tr>
      <th>2013-01-02</th>
      <td>0.382312</td>
      <td>0.822667</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>0.076217</td>
      <td>-0.812529</td>
    </tr>
    <tr>
      <th>2013-01-04</th>
      <td>-0.967295</td>
      <td>-0.576004</td>
    </tr>
    <tr>
      <th>2013-01-05</th>
      <td>0.355139</td>
      <td>-0.485506</td>
    </tr>
    <tr>
      <th>2013-01-06</th>
      <td>0.022514</td>
      <td>0.026405</td>
    </tr>
  </tbody>
</table>
</div>



* 단일 행, 열을 선택하면 스칼라가 반환됩니다.


```python
df.iloc[1, 1]
```




    0.3823115346346552



* 스칼라에 빠르게 접근하려면 dataFrame.iat[]을 사용합니다.


```python
df.iat[1, 1]
```




    0.3823115346346552



## Boolean indexing

* DataFrame.A가 0보다 큰 행을 선택하세요. 


```python
df[df["A"] > 0]
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-01</th>
      <td>0.570644</td>
      <td>-0.759579</td>
      <td>-0.360142</td>
      <td>-1.724012</td>
    </tr>
    <tr>
      <th>2013-01-02</th>
      <td>0.297219</td>
      <td>0.382312</td>
      <td>0.822667</td>
      <td>0.314014</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>0.834170</td>
      <td>0.076217</td>
      <td>-0.812529</td>
      <td>-1.911790</td>
    </tr>
    <tr>
      <th>2013-01-05</th>
      <td>0.701261</td>
      <td>0.355139</td>
      <td>-0.485506</td>
      <td>0.780679</td>
    </tr>
    <tr>
      <th>2013-01-06</th>
      <td>1.609882</td>
      <td>0.022514</td>
      <td>0.026405</td>
      <td>-0.007853</td>
    </tr>
  </tbody>
</table>
</div>



* DataFrame 조건이 충족되는 값을 선택할 수 있습니다. 


```python
df[df > 0]
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-01</th>
      <td>0.570644</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2013-01-02</th>
      <td>0.297219</td>
      <td>0.382312</td>
      <td>0.822667</td>
      <td>0.314014</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>0.834170</td>
      <td>0.076217</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2013-01-04</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.746524</td>
    </tr>
    <tr>
      <th>2013-01-05</th>
      <td>0.701261</td>
      <td>0.355139</td>
      <td>NaN</td>
      <td>0.780679</td>
    </tr>
    <tr>
      <th>2013-01-06</th>
      <td>1.609882</td>
      <td>0.022514</td>
      <td>0.026405</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



* isin() 필터링 사용 방법은 다음과 같습니다. 


```python
df2 = df.copy()
df2["E"] = ["one", "one", "two", "three", "four", "three"]
df2
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-01</th>
      <td>0.570644</td>
      <td>-0.759579</td>
      <td>-0.360142</td>
      <td>-1.724012</td>
      <td>one</td>
    </tr>
    <tr>
      <th>2013-01-02</th>
      <td>0.297219</td>
      <td>0.382312</td>
      <td>0.822667</td>
      <td>0.314014</td>
      <td>one</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>0.834170</td>
      <td>0.076217</td>
      <td>-0.812529</td>
      <td>-1.911790</td>
      <td>two</td>
    </tr>
    <tr>
      <th>2013-01-04</th>
      <td>-0.022074</td>
      <td>-0.967295</td>
      <td>-0.576004</td>
      <td>0.746524</td>
      <td>three</td>
    </tr>
    <tr>
      <th>2013-01-05</th>
      <td>0.701261</td>
      <td>0.355139</td>
      <td>-0.485506</td>
      <td>0.780679</td>
      <td>four</td>
    </tr>
    <tr>
      <th>2013-01-06</th>
      <td>1.609882</td>
      <td>0.022514</td>
      <td>0.026405</td>
      <td>-0.007853</td>
      <td>three</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2[df2["E"].isin(["two", "four"])]
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-03</th>
      <td>0.834170</td>
      <td>0.076217</td>
      <td>-0.812529</td>
      <td>-1.911790</td>
      <td>two</td>
    </tr>
    <tr>
      <th>2013-01-05</th>
      <td>0.701261</td>
      <td>0.355139</td>
      <td>-0.485506</td>
      <td>0.780679</td>
      <td>four</td>
    </tr>
  </tbody>
</table>
</div>



## Setting

* 새 열을 설정하면 인덱스에 따라 데이터가 자동으로 정렬됩니다. 


```python
s1 = pd.Series([1, 2, 3, 4, 5, 6], index=pd.date_range("20130102", periods=6))
s1
```




    2013-01-02    1
    2013-01-03    2
    2013-01-04    3
    2013-01-05    4
    2013-01-06    5
    2013-01-07    6
    Freq: D, dtype: int64




```python
df["F"] = s1
```

* 라벨로 값 설정


```python
df.at[dates[0], "A"] = 0
```

* 위치별 값 설정


```python
df.iat[0, 1] = 0
```

* NumPy 배열로 할당하여 설정


```python
df.loc[:, "D"] = np.array([5] * len(df))
```

* 결과


```python
df
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>F</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-01</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>-0.360142</td>
      <td>5</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2013-01-02</th>
      <td>0.297219</td>
      <td>0.382312</td>
      <td>0.822667</td>
      <td>5</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>0.834170</td>
      <td>0.076217</td>
      <td>-0.812529</td>
      <td>5</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>2013-01-04</th>
      <td>-0.022074</td>
      <td>-0.967295</td>
      <td>-0.576004</td>
      <td>5</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>2013-01-05</th>
      <td>0.701261</td>
      <td>0.355139</td>
      <td>-0.485506</td>
      <td>5</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>2013-01-06</th>
      <td>1.609882</td>
      <td>0.022514</td>
      <td>0.026405</td>
      <td>5</td>
      <td>5.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2 = df.copy()
df2[df2 > 0] = -df2
df2
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>F</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-01</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>-0.360142</td>
      <td>-5</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2013-01-02</th>
      <td>-0.297219</td>
      <td>-0.382312</td>
      <td>-0.822667</td>
      <td>-5</td>
      <td>-1.0</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>-0.834170</td>
      <td>-0.076217</td>
      <td>-0.812529</td>
      <td>-5</td>
      <td>-2.0</td>
    </tr>
    <tr>
      <th>2013-01-04</th>
      <td>-0.022074</td>
      <td>-0.967295</td>
      <td>-0.576004</td>
      <td>-5</td>
      <td>-3.0</td>
    </tr>
    <tr>
      <th>2013-01-05</th>
      <td>-0.701261</td>
      <td>-0.355139</td>
      <td>-0.485506</td>
      <td>-5</td>
      <td>-4.0</td>
    </tr>
    <tr>
      <th>2013-01-06</th>
      <td>-1.609882</td>
      <td>-0.022514</td>
      <td>-0.026405</td>
      <td>-5</td>
      <td>-5.0</td>
    </tr>
  </tbody>
</table>
</div>



## Summary Lession 4

### Lession 4.1

|문법|내용|
|-|-|
|df["A"]|DataFrame의 단일 열 전달 (Series)|
|df[0:3]|DataFrame의 [a, b - 1] 행 데이터 전달 (DataFrame)|
|df["20130102":"20130104"]| DataFrame의 일치하는 행 데이터 전달 (DataFrame)|

### Lession 4.2 

* 라벨 슬라이싱은 loc 사용 

|문법|내용|
|-|-|
|df.loc[dates[0]]|레이블과 일치하는 행 선택|
|df.loc[:, ["A", "B"]]|loc[행, 열], : 는 모든 영역 선택|
|df.loc["20130102":"20130104", ["A", "B"]]|라벨 슬라이싱은 두 종점이 모두 포함|
|df.loc[dates[0], "A"]|단일 행, 열을 선택하면 스칼라가 반환|
|df.at[dates[0], "A"]|스칼라에 더 빠르게 접근하는 방법|

### Lession 4.3 

* 정수 슬라이싱은 iloc 사용 

|문법|내용|
|-|-|
|df.iloc[3]|레이블과 일치하는 행 선택|
|df.iloc[3:5, 0:2]|iloc[행, 열], [a, b - 1], [c, d - 1]의 데이터 전달 (DataFrame)|
|df.iloc[[1, 2, 4], [0, 2]]|정수 위치 목록으로 슬라이싱|
|df.iloc[:, 1:3]|열을 명시적으로 슬라이싱|
|df.iloc[1, 1]|단일 행, 열을 선택하면 스칼라가 반환|
|df.iat[1, 1]|스칼라에 더 빠르게 접근하는 방법|

### Lession 4.4

|문법|내용|
|-|-|
|df[df["A"] > 0]|DataFrame의 A 열이 양수인 값만 DataFrame으로 출력|
|df[df > 0]|DataFrame의 기본 값이 양수인 값만 DataFrame으로 출력 / 조건에 충족하지 않은 값은 NAN으로 출력|
|df2[df2["E"].isin(["two", "four"])]|E열에 "two"와 "four"이 있는 조건이 충족되는 값만 DataFrame으로 출력|

### Lession 4.5 

|문법|내용|
|-|-|
|df.at[dates[0], "A"] = 0|라벨로 값 설정|
|df.iat[0, 1] = 0|정수 값으로 값 설정|
|df.loc[:, "D"] = np.array([5] * len(df))|Numpy 배열로 할당하여 설정|
|df[df > 0] = -df|Boolean 인덱싱으로 값 설정|

---

# Missing data

* NumPy 데이터의 `np.nan`는 누락된 데이터를 나타냅니다. 
* 기본적으로 계산에 포함되지 않습니다. 


```python
df1 = df.reindex(index = dates[0:4], columns = list(df.columns) + ["E"])
df1.loc[dates[0] : dates[1], "E"] = 1
df1
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>F</th>
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-01</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>-0.360142</td>
      <td>5</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2013-01-02</th>
      <td>0.297219</td>
      <td>0.382312</td>
      <td>0.822667</td>
      <td>5</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>0.834170</td>
      <td>0.076217</td>
      <td>-0.812529</td>
      <td>5</td>
      <td>2.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2013-01-04</th>
      <td>-0.022074</td>
      <td>-0.967295</td>
      <td>-0.576004</td>
      <td>5</td>
      <td>3.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



* `DataFrame.dropna()` 데이터가 누락된 모든 행을 삭제합니다. 


```python
df1.dropna(how = 'any')
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>F</th>
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-02</th>
      <td>0.297219</td>
      <td>0.382312</td>
      <td>0.822667</td>
      <td>5</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>



* `DataFrame.fillna()` 누락된 데이터를 채웁니다. 


```python
df1.fillna(value = 5)
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>F</th>
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-01</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>-0.360142</td>
      <td>5</td>
      <td>5.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2013-01-02</th>
      <td>0.297219</td>
      <td>0.382312</td>
      <td>0.822667</td>
      <td>5</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>0.834170</td>
      <td>0.076217</td>
      <td>-0.812529</td>
      <td>5</td>
      <td>2.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>2013-01-04</th>
      <td>-0.022074</td>
      <td>-0.967295</td>
      <td>-0.576004</td>
      <td>5</td>
      <td>3.0</td>
      <td>5.0</td>
    </tr>
  </tbody>
</table>
</div>



* `isna()` 값이 다음과 같은 Boolean 값을 출력합니다. 
* NaN = True 


```python
pd.isna(df1)
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>F</th>
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-01</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-01-02</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2013-01-04</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>



## Summary Lession 5

|문법|내용|
|-|-|
|DataFrame.dropna()|데이터가 누락된 모든 행 삭제|
|DataFrame.fillna()|누락된 데이터 채우기|
|DataFrame.isna(df1)|Boolean 값으로 출력 / NaN = True|

---

# Operations 

## Stats

* `DataFrame.mean()` 각 열의 평균값을 계산합니다. 


```python
df.mean()
```




    A    0.570076
    B   -0.021852
    C   -0.230851
    D    5.000000
    F    3.000000
    dtype: float64



* `DataFrame.mean(axis = 1)` 각 행의 평균값을 계산합니다. 


```python
df.mean(axis=1)
```




    2013-01-01    1.159965
    2013-01-02    1.500440
    2013-01-03    1.419571
    2013-01-04    1.286926
    2013-01-05    1.914179
    2013-01-06    2.331760
    Freq: D, dtype: float64



* `shift(n)` n의 수만큼 행이 아래로 내려갑니다. 
* n < 0 이라면 반대로 행이 위로 올라갑니다. 
* 비워진 행은 NaN 으로 기입됩니다. 


```python
s = pd.Series([1, 3, 5, np.nan, 6, 8], index = dates).shift(2)
s
```




    2013-01-01    NaN
    2013-01-02    NaN
    2013-01-03    1.0
    2013-01-04    3.0
    2013-01-05    5.0
    2013-01-06    NaN
    Freq: D, dtype: float64



* `sub` axis 기준으로 뺄셈의 의미입니다. 
* `df.sub(s, axis="index")` 는 df - s 인데 index 기준으로 뺄셈을 하라는 의미입니다. 


```python
df.sub(s, axis="index")
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>F</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-01</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2013-01-02</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>-0.165830</td>
      <td>-0.923783</td>
      <td>-1.812529</td>
      <td>4.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2013-01-04</th>
      <td>-3.022074</td>
      <td>-3.967295</td>
      <td>-3.576004</td>
      <td>2.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2013-01-05</th>
      <td>-4.298739</td>
      <td>-4.644861</td>
      <td>-5.485506</td>
      <td>0.0</td>
      <td>-1.0</td>
    </tr>
    <tr>
      <th>2013-01-06</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



## User defined functions 



* `DataFrame.agg()` Series 형태의 계산을 할 때 유용 
* `DataFrame.transform()` DataFrame의 각 data 계산을 할 때 유용 

* `transform()` vs `apply()`
    * `apply()`는 DataFrame 전체 열의 값들을 조작하면서 값 계산 가능
    * `transform()`은 전체 DataFrame을 한번에 연산할 수 없음
    * 따라서 상황에 따라 적절한 함수 사용이 필요합니다. 


```python
df.agg(lambda x: np.mean(x) * 5.6)
```




    A     3.192428
    B    -0.122372
    C    -1.292768
    D    28.000000
    F    16.800000
    dtype: float64




```python
df.transform(lambda x: x * 101.2)
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>F</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-01</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>-36.446359</td>
      <td>506.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2013-01-02</th>
      <td>30.078581</td>
      <td>38.689927</td>
      <td>83.253938</td>
      <td>506.0</td>
      <td>101.2</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>84.417972</td>
      <td>7.713112</td>
      <td>-82.227909</td>
      <td>506.0</td>
      <td>202.4</td>
    </tr>
    <tr>
      <th>2013-01-04</th>
      <td>-2.233881</td>
      <td>-97.890213</td>
      <td>-58.291601</td>
      <td>506.0</td>
      <td>303.6</td>
    </tr>
    <tr>
      <th>2013-01-05</th>
      <td>70.967628</td>
      <td>35.940059</td>
      <td>-49.133233</td>
      <td>506.0</td>
      <td>404.8</td>
    </tr>
    <tr>
      <th>2013-01-06</th>
      <td>162.920071</td>
      <td>2.278447</td>
      <td>2.672174</td>
      <td>506.0</td>
      <td>506.0</td>
    </tr>
  </tbody>
</table>
</div>



## Value Counts 


```python
s = pd.Series(np.random.randint(0, 7, size=10))
s
```




    0    6
    1    5
    2    2
    3    3
    4    0
    5    5
    6    3
    7    5
    8    1
    9    3
    dtype: int32



* `value_counts()` 각 데이터의 갯수를 카운트 합니다. 
* Boolean 값에서 True = 1 인 경우를 이용하여 NaN의 갯수를 카운트 하기 용이합니다. 


```python
s.value_counts()
```




    5    3
    3    3
    6    1
    2    1
    0    1
    1    1
    dtype: int64



## String Method 

* `lower()` 문자열을 소문자로 출력해줍니다. 
* Series 에서는 `str.lower()` 을 사용해야 합니다. 


```python
s = pd.Series(["A", "B", "C", "Aaba", "Baca", np.nan, "CABA", "dog", "cat"])
s.str.lower()
```




    0       a
    1       b
    2       c
    3    aaba
    4    baca
    5     NaN
    6    caba
    7     dog
    8     cat
    dtype: object



## Summary Lession 6

### Lession 6.1

|문법|내용|
|-|-|
|df.mean()|각 열의 평균값|
|df.mean(axis=1)|각 행의 평균값|
|shift(n)|행을 아래 or 위로 조정|
|df.sub(s, axis="index")|axis 기준으로 뺄셈|

### Lession 6.2

|문법|내용|
|-|-|
|DataFrame.agg()|Series 형태의 계산 수행 가능|
|DataFrame.transform()|DataFrame 형태의 전체 data 계산 가능| 

### Lession 6.3

|문법|내용|
|-|-|
|value_counts()|데이터의 갯수 출력|

### Lession 6.4

|문법|내용|
|-|-|
|lower()|소문자 출력|
|str.lower()|Series 형태의 배열 소문자 출력|

---

# Merge

## Concat

* `concat()` 행별로 pandas 객체를 연결합니다. 


```python
df = pd.DataFrame(np.random.randn(10, 4))
df
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
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-1.183298</td>
      <td>1.555068</td>
      <td>-1.895312</td>
      <td>-0.260293</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.790440</td>
      <td>1.131910</td>
      <td>-1.748850</td>
      <td>-0.330198</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2.506277</td>
      <td>0.457065</td>
      <td>-0.271042</td>
      <td>-0.521011</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.793096</td>
      <td>-1.263110</td>
      <td>0.217098</td>
      <td>-0.349861</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-1.038063</td>
      <td>-0.237038</td>
      <td>0.323379</td>
      <td>-0.059623</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.027864</td>
      <td>0.083783</td>
      <td>1.023926</td>
      <td>-1.099976</td>
    </tr>
    <tr>
      <th>6</th>
      <td>0.247469</td>
      <td>-0.393058</td>
      <td>-0.937385</td>
      <td>-0.881090</td>
    </tr>
    <tr>
      <th>7</th>
      <td>0.977895</td>
      <td>-0.388735</td>
      <td>0.875781</td>
      <td>-0.362118</td>
    </tr>
    <tr>
      <th>8</th>
      <td>-0.253913</td>
      <td>-1.670859</td>
      <td>-1.032215</td>
      <td>-1.564824</td>
    </tr>
    <tr>
      <th>9</th>
      <td>-1.377641</td>
      <td>1.040353</td>
      <td>0.152415</td>
      <td>3.028259</td>
    </tr>
  </tbody>
</table>
</div>




```python
pieces = [df[:3], df[3:7], df[7:]]
pieces
```




    [          0         1         2         3
     0 -1.183298  1.555068 -1.895312 -0.260293
     1  0.790440  1.131910 -1.748850 -0.330198
     2  2.506277  0.457065 -0.271042 -0.521011,
               0         1         2         3
     3  0.793096 -1.263110  0.217098 -0.349861
     4 -1.038063 -0.237038  0.323379 -0.059623
     5  0.027864  0.083783  1.023926 -1.099976
     6  0.247469 -0.393058 -0.937385 -0.881090,
               0         1         2         3
     7  0.977895 -0.388735  0.875781 -0.362118
     8 -0.253913 -1.670859 -1.032215 -1.564824
     9 -1.377641  1.040353  0.152415  3.028259]




```python
pd.concat(pieces)
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
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-1.183298</td>
      <td>1.555068</td>
      <td>-1.895312</td>
      <td>-0.260293</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.790440</td>
      <td>1.131910</td>
      <td>-1.748850</td>
      <td>-0.330198</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2.506277</td>
      <td>0.457065</td>
      <td>-0.271042</td>
      <td>-0.521011</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.793096</td>
      <td>-1.263110</td>
      <td>0.217098</td>
      <td>-0.349861</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-1.038063</td>
      <td>-0.237038</td>
      <td>0.323379</td>
      <td>-0.059623</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.027864</td>
      <td>0.083783</td>
      <td>1.023926</td>
      <td>-1.099976</td>
    </tr>
    <tr>
      <th>6</th>
      <td>0.247469</td>
      <td>-0.393058</td>
      <td>-0.937385</td>
      <td>-0.881090</td>
    </tr>
    <tr>
      <th>7</th>
      <td>0.977895</td>
      <td>-0.388735</td>
      <td>0.875781</td>
      <td>-0.362118</td>
    </tr>
    <tr>
      <th>8</th>
      <td>-0.253913</td>
      <td>-1.670859</td>
      <td>-1.032215</td>
      <td>-1.564824</td>
    </tr>
    <tr>
      <th>9</th>
      <td>-1.377641</td>
      <td>1.040353</td>
      <td>0.152415</td>
      <td>3.028259</td>
    </tr>
  </tbody>
</table>
</div>



## Join

* `merge()` 동일한 열(columns) 이름을 기준으로 여러개의 DataFrame을 하나의 DataFrame으로 만들어줍니다. 
* on 값으로 열의 기준을 나타냅니다. 


```python
left = pd.DataFrame({"key": ["foo", "foo"], "lval": [1, 2]})
left
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
      <th>key</th>
      <th>lval</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>foo</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>foo</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
right = pd.DataFrame({"key": ["foo", "foo"], "rval": [4, 5]})
right
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
      <th>key</th>
      <th>rval</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>foo</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>foo</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge(left, right, on="key")
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
      <th>key</th>
      <th>lval</th>
      <th>rval</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>foo</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>foo</td>
      <td>1</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>foo</td>
      <td>2</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>foo</td>
      <td>2</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
left = pd.DataFrame({"key": ["foo", "bar"], "lval": [1, 2]})
left 
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
      <th>key</th>
      <th>lval</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>foo</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>bar</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
right = pd.DataFrame({"key": ["foo", "bar"], "rval": [4, 5]})
right
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
      <th>key</th>
      <th>rval</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>foo</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>bar</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge(left, right, on="key")
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
      <th>key</th>
      <th>lval</th>
      <th>rval</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>foo</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>bar</td>
      <td>2</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



## Summary Lession 7

### Lession 7.1

|문법|내용|
|-|-|
|concat()|행 연결| 

### Lession 7.2

|문법|내용|
|-|-|
|merge()|열 연결|

---

# Grouping

* `Splitting` 기준에 따라 데이터를 그룹으로 분할
* `Applying` 각 그룹에 독립적으로 함수 적용
* `Combining` 결과를 데이터 구조로 결합


```python
df = pd.DataFrame(
    {
        "A": ["foo", "bar", "foo", "bar", "foo", "bar", "foo", "foo"],
        "B": ["one", "one", "two", "three", "two", "two", "one", "three"],
        "C": np.random.randn(8),
        "D": np.random.randn(8),
    }
)

df
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>foo</td>
      <td>one</td>
      <td>-0.292383</td>
      <td>-0.421345</td>
    </tr>
    <tr>
      <th>1</th>
      <td>bar</td>
      <td>one</td>
      <td>-0.860906</td>
      <td>-0.017166</td>
    </tr>
    <tr>
      <th>2</th>
      <td>foo</td>
      <td>two</td>
      <td>-1.086769</td>
      <td>-1.552017</td>
    </tr>
    <tr>
      <th>3</th>
      <td>bar</td>
      <td>three</td>
      <td>-0.727884</td>
      <td>-1.439300</td>
    </tr>
    <tr>
      <th>4</th>
      <td>foo</td>
      <td>two</td>
      <td>1.162965</td>
      <td>0.766341</td>
    </tr>
    <tr>
      <th>5</th>
      <td>bar</td>
      <td>two</td>
      <td>-0.488100</td>
      <td>0.834470</td>
    </tr>
    <tr>
      <th>6</th>
      <td>foo</td>
      <td>one</td>
      <td>0.934683</td>
      <td>-0.908139</td>
    </tr>
    <tr>
      <th>7</th>
      <td>foo</td>
      <td>three</td>
      <td>0.587568</td>
      <td>0.305971</td>
    </tr>
  </tbody>
</table>
</div>



* `DataFrame.groupby.sum()` 
* `df.groupby("A")[["C", "D"]].sum()`
    * A를 동일한 기준으로 결합한 후 C, D 를 sum(덧셈) 하는 것


```python
df.groupby("A")[["C", "D"]].sum()
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
      <th>C</th>
      <th>D</th>
    </tr>
    <tr>
      <th>A</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>bar</th>
      <td>-2.076890</td>
      <td>-0.621996</td>
    </tr>
    <tr>
      <th>foo</th>
      <td>1.306063</td>
      <td>-1.809189</td>
    </tr>
  </tbody>
</table>
</div>



* `df.groupby(["A", "B"]).sum()`
    * A, B를 동일한 기준으로 결합한 후 이외의 열들을 sum(덧셈)하는 것


```python
df.groupby(["A", "B"]).sum()
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
      <th>C</th>
      <th>D</th>
    </tr>
    <tr>
      <th>A</th>
      <th>B</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" valign="top">bar</th>
      <th>one</th>
      <td>-0.860906</td>
      <td>-0.017166</td>
    </tr>
    <tr>
      <th>three</th>
      <td>-0.727884</td>
      <td>-1.439300</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-0.488100</td>
      <td>0.834470</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">foo</th>
      <th>one</th>
      <td>0.642300</td>
      <td>-1.329484</td>
    </tr>
    <tr>
      <th>three</th>
      <td>0.587568</td>
      <td>0.305971</td>
    </tr>
    <tr>
      <th>two</th>
      <td>0.076196</td>
      <td>-0.785676</td>
    </tr>
  </tbody>
</table>
</div>



## Summary Lession 8

|문법|내용|
|-|-|
|DataFrame.groupby.sum()| 기준 설정 -> 계산 값 설정 -> 어떤 계산을 수행할 지 설정|

---

# Reshaping

## Stack 


```python
arrays = [
   ["bar", "bar", "baz", "baz", "foo", "foo", "qux", "qux"],
   ["one", "two", "one", "two", "one", "two", "one", "two"],
]

index = pd.MultiIndex.from_arrays(arrays, names=["first", "second"])

df = pd.DataFrame(np.random.randn(8, 2), index=index, columns=["A", "B"])

df2 = df[:4]

df2
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
      <th>A</th>
      <th>B</th>
    </tr>
    <tr>
      <th>first</th>
      <th>second</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">bar</th>
      <th>one</th>
      <td>-0.044480</td>
      <td>0.968086</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-0.487256</td>
      <td>-0.083276</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">baz</th>
      <th>one</th>
      <td>-0.154178</td>
      <td>0.508106</td>
    </tr>
    <tr>
      <th>two</th>
      <td>2.212253</td>
      <td>-0.253072</td>
    </tr>
  </tbody>
</table>
</div>



* `stack` DataFrame의 차원을 축소하는 개념 
    * pandas 2.0.0 이상에서 DataFrame.stack(future_unstack = True)를 사용하면 데이터 프레임 형태 유지
    * pandas 2.0.0 이하에서는 DataFrame.stack()을 하면 Series로 변환이 되어 DataFrame.stack().to_frame()을 사용해서 DataFrame 형태로 유지할 수 있음 


```python
stacked = df2.stack().to_frame()
stacked
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
      <th></th>
      <th>0</th>
    </tr>
    <tr>
      <th>first</th>
      <th>second</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="4" valign="top">bar</th>
      <th rowspan="2" valign="top">one</th>
      <th>A</th>
      <td>-0.044480</td>
    </tr>
    <tr>
      <th>B</th>
      <td>0.968086</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">two</th>
      <th>A</th>
      <td>-0.487256</td>
    </tr>
    <tr>
      <th>B</th>
      <td>-0.083276</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">baz</th>
      <th rowspan="2" valign="top">one</th>
      <th>A</th>
      <td>-0.154178</td>
    </tr>
    <tr>
      <th>B</th>
      <td>0.508106</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">two</th>
      <th>A</th>
      <td>2.212253</td>
    </tr>
    <tr>
      <th>B</th>
      <td>-0.253072</td>
    </tr>
  </tbody>
</table>
</div>



* `unstack()` "스택된" DataFrame 이나 Series의 경우 마지막 수준으로 돌려주는 역할을 수행합니다. 
* 모든 인덱스를 다 풀어서 원래 형태로 변환 
* 기본적으로 `unstack(-1)`, 즉 가장 안쪽 인덱스부터 풀어줌 
* 모든 계층을 다 풀어서 완전히 원래의 DataFrame 형태로 복원됨


```python
stacked.unstack()
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
      <th></th>
      <th colspan="2" halign="left">0</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
    <tr>
      <th>first</th>
      <th>second</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">bar</th>
      <th>one</th>
      <td>-0.044480</td>
      <td>0.968086</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-0.487256</td>
      <td>-0.083276</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">baz</th>
      <th>one</th>
      <td>-0.154178</td>
      <td>0.508106</td>
    </tr>
    <tr>
      <th>two</th>
      <td>2.212253</td>
      <td>-0.253072</td>
    </tr>
  </tbody>
</table>
</div>



* `unstack(1)` 두 번재 인덱스(second)를 컬럼으로 변환 
    * second(one, two)를 컬럼으로 변경하고, first(bar, baz)는 인덱스로 유지 
    * 결과적으로 "A", "B"를 가진 DataFrame이 됨 


```python
stacked.unstack(1)
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
      <th></th>
      <th colspan="2" halign="left">0</th>
    </tr>
    <tr>
      <th></th>
      <th>second</th>
      <th>one</th>
      <th>two</th>
    </tr>
    <tr>
      <th>first</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">bar</th>
      <th>A</th>
      <td>-0.044480</td>
      <td>-0.487256</td>
    </tr>
    <tr>
      <th>B</th>
      <td>0.968086</td>
      <td>-0.083276</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">baz</th>
      <th>A</th>
      <td>-0.154178</td>
      <td>2.212253</td>
    </tr>
    <tr>
      <th>B</th>
      <td>0.508106</td>
      <td>-0.253072</td>
    </tr>
  </tbody>
</table>
</div>



* `unstack(0)` 첫 번째 인덱스(first)를 컬럼으로 변환
    * first(bar, baz)를 컬럼으로 변경하고, second(one, two)는 인덱스로 유지 
    * 즉, "A"와 "B"가 각각 first(bar, baz)에 대해 정리됨


```python
stacked.unstack(0)
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
      <th></th>
      <th colspan="2" halign="left">0</th>
    </tr>
    <tr>
      <th></th>
      <th>first</th>
      <th>bar</th>
      <th>baz</th>
    </tr>
    <tr>
      <th>second</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">one</th>
      <th>A</th>
      <td>-0.044480</td>
      <td>-0.154178</td>
    </tr>
    <tr>
      <th>B</th>
      <td>0.968086</td>
      <td>0.508106</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">two</th>
      <th>A</th>
      <td>-0.487256</td>
      <td>2.212253</td>
    </tr>
    <tr>
      <th>B</th>
      <td>-0.083276</td>
      <td>-0.253072</td>
    </tr>
  </tbody>
</table>
</div>



## Pivot tables 


```python
df = pd.DataFrame(
    {
        "A": ["one", "one", "two", "three"] * 3,
        "B": ["A", "B", "C"] * 4,
        "C": ["foo", "foo", "foo", "bar", "bar", "bar"] * 2,
        "D": np.random.randn(12),
        "E": np.random.randn(12),
    }
)

df
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>one</td>
      <td>A</td>
      <td>foo</td>
      <td>-0.364209</td>
      <td>0.219147</td>
    </tr>
    <tr>
      <th>1</th>
      <td>one</td>
      <td>B</td>
      <td>foo</td>
      <td>0.191238</td>
      <td>0.364982</td>
    </tr>
    <tr>
      <th>2</th>
      <td>two</td>
      <td>C</td>
      <td>foo</td>
      <td>1.345826</td>
      <td>0.276045</td>
    </tr>
    <tr>
      <th>3</th>
      <td>three</td>
      <td>A</td>
      <td>bar</td>
      <td>2.090953</td>
      <td>1.129050</td>
    </tr>
    <tr>
      <th>4</th>
      <td>one</td>
      <td>B</td>
      <td>bar</td>
      <td>-0.342486</td>
      <td>-0.603012</td>
    </tr>
    <tr>
      <th>5</th>
      <td>one</td>
      <td>C</td>
      <td>bar</td>
      <td>-0.030710</td>
      <td>1.293112</td>
    </tr>
    <tr>
      <th>6</th>
      <td>two</td>
      <td>A</td>
      <td>foo</td>
      <td>-0.078546</td>
      <td>0.397122</td>
    </tr>
    <tr>
      <th>7</th>
      <td>three</td>
      <td>B</td>
      <td>foo</td>
      <td>-0.158317</td>
      <td>0.050471</td>
    </tr>
    <tr>
      <th>8</th>
      <td>one</td>
      <td>C</td>
      <td>foo</td>
      <td>-2.106966</td>
      <td>0.962997</td>
    </tr>
    <tr>
      <th>9</th>
      <td>one</td>
      <td>A</td>
      <td>bar</td>
      <td>0.010819</td>
      <td>-0.717967</td>
    </tr>
    <tr>
      <th>10</th>
      <td>two</td>
      <td>B</td>
      <td>bar</td>
      <td>-2.041610</td>
      <td>-1.919767</td>
    </tr>
    <tr>
      <th>11</th>
      <td>three</td>
      <td>C</td>
      <td>bar</td>
      <td>0.252937</td>
      <td>-0.892070</td>
    </tr>
  </tbody>
</table>
</div>



* `pivot_table()` : `values``index``columns`를 지정하여 DataFrame 만들 수 있습니다. 


```python
pd.pivot_table(df, values="D", index=["A", "B"], columns=["C"])
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
      <th>C</th>
      <th>bar</th>
      <th>foo</th>
    </tr>
    <tr>
      <th>A</th>
      <th>B</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" valign="top">one</th>
      <th>A</th>
      <td>0.010819</td>
      <td>-0.364209</td>
    </tr>
    <tr>
      <th>B</th>
      <td>-0.342486</td>
      <td>0.191238</td>
    </tr>
    <tr>
      <th>C</th>
      <td>-0.030710</td>
      <td>-2.106966</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">three</th>
      <th>A</th>
      <td>2.090953</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>B</th>
      <td>NaN</td>
      <td>-0.158317</td>
    </tr>
    <tr>
      <th>C</th>
      <td>0.252937</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">two</th>
      <th>A</th>
      <td>NaN</td>
      <td>-0.078546</td>
    </tr>
    <tr>
      <th>B</th>
      <td>-2.041610</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>C</th>
      <td>NaN</td>
      <td>1.345826</td>
    </tr>
  </tbody>
</table>
</div>



## Summary Lession 9

### Lession 9.1

|문법|내용|
|-|-|
|stack()|컬럼을 인덱스로 보내는 역할|
|stack().to_frame|pandas < 2.0.0 에서 데이터프레임으로 출력하는 방법|
|stack(future_unstack = True)|pandas >= 2.0.0 에서 데이터프레임으로 출력하는 방법| 
|unstack()|가능한 모든 인덱스를 풀어서 원래 형태로 복원|
|unstack(0)|첫 번째 인덱스를 컬럼으로 변환|
|unstack(1)|두 번째 인덱스를 컬럼으로 변환|

### Lession 9.2

`pivot_table` 


pd.pivot_table(data, 
               values=None, 
               index=None, 
               columns=None, 
               aggfunc="mean", 
               fill_value=None, 
               margins=False, 
               margins_name="All", 
               observed=False)
               
               
|매개변수|설명|
|-|-|
|data|피벗 테이블을 만들 DataFrame|
|values|요약할 수치형 컬럼|
|index|행(rows)으로 사용할 컬럼|
|columns|열(columns)으로 사용할 컬럼|
|aggfunc|적용할 집계함수 (기본값 : 평균 `mean`)|
|fill_value|`NaN`값을 채울 값|
|margins|총합(Total) 행/열 추가 여부(`True`이면 전체 합산)|
|margins_name|`margins = True`일 때 "총합"의 이름 (기본값 : `"ALL"`)|
|observed|범주형 컬럼에 대해 실제로 등장한 값만 출력할지 여부|

---

# Time series


```python
rng = pd.date_range("1/1/2012", periods=100, freq="s")
ts = pd.Series(np.random.randint(0, 500, len(rng)), index=rng)
ts.resample("5Min").sum()
```




    2012-01-01    24855
    Freq: 5T, dtype: int32



* `Series.tz_localize()` 시계열을 시간대에 맞게 현지화합니다. 


```python
rng = pd.date_range("3/6/2012 00:00", periods=5, freq="D")
ts = pd.Series(np.random.randn(len(rng)), rng)
ts
```




    2012-03-06    0.437337
    2012-03-07    1.018264
    2012-03-08   -0.012883
    2012-03-09   -0.416875
    2012-03-10    0.937738
    Freq: D, dtype: float64




```python
ts_utc = ts.tz_localize("UTC")
ts_utc
```




    2012-03-06 00:00:00+00:00    0.437337
    2012-03-07 00:00:00+00:00    1.018264
    2012-03-08 00:00:00+00:00   -0.012883
    2012-03-09 00:00:00+00:00   -0.416875
    2012-03-10 00:00:00+00:00    0.937738
    Freq: D, dtype: float64



* `Series.tz_convert()` 시간대 인식 시계열을 다른 시간대로 변환합니다. 


```python
ts_utc.tz_convert("US/Eastern")
```




    2012-03-05 19:00:00-05:00    0.437337
    2012-03-06 19:00:00-05:00    1.018264
    2012-03-07 19:00:00-05:00   -0.012883
    2012-03-08 19:00:00-05:00   -0.416875
    2012-03-09 19:00:00-05:00    0.937738
    Freq: D, dtype: float64



* `BusinessDay` 시계열에 고정되지 않은 기간 추가 


```python
rng
```




    DatetimeIndex(['2012-03-06', '2012-03-07', '2012-03-08', '2012-03-09',
                   '2012-03-10'],
                  dtype='datetime64[ns]', freq='D')




```python
rng + pd.offsets.BusinessDay(5)
```




    DatetimeIndex(['2012-03-13', '2012-03-14', '2012-03-15', '2012-03-16',
                   '2012-03-16'],
                  dtype='datetime64[ns]', freq=None)



## Summary Lession 10

|문법|내용|
|-|-|
|Series.tz_localize()|시계열을 시간대에 맞게 현지화| 
|Series.tz_convert()|시간대 인식 시계열을 다른 시간대로 변환|
|BusinessDay|시계열에 고정되지 않은 기간 추가|

---

# Categoricals 


```python
df = pd.DataFrame(
    {"id": [1, 2, 3, 4, 5, 6], "raw_grade": ["a", "b", "b", "a", "a", "e"]}
)
df
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
      <th>id</th>
      <th>raw_grade</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>a</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>b</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>b</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>a</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>a</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>e</td>
    </tr>
  </tbody>
</table>
</div>



* `astype("category")` 범주형 데이터 변환


```python
df["grade"] = df["raw_grade"].astype("category")
df["grade"]
```




    0    a
    1    b
    2    b
    3    a
    4    a
    5    e
    Name: grade, dtype: category
    Categories (3, object): ['a', 'b', 'e']




```python
new_categories = ["very good", "good", "very bad"]
df["grade"] = df["grade"].cat.rename_categories(new_categories)
```

* `Series.cat()` 새 카테고리를 반환하는 method 


```python
df["grade"] = df["grade"].cat.set_categories(
    ["very bad", "bad", "medium", "good", "very good"]
)

df["grade"]
```




    0    very good
    1         good
    2         good
    3    very good
    4    very good
    5     very bad
    Name: grade, dtype: category
    Categories (5, object): ['very bad', 'bad', 'medium', 'good', 'very good']



* 정렬은 범주별 순서에 따라 이루어집니다. (어휘 순서 X)


```python
df.sort_values(by="grade")
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
      <th>id</th>
      <th>raw_grade</th>
      <th>grade</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>e</td>
      <td>very bad</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>b</td>
      <td>good</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>b</td>
      <td>good</td>
    </tr>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>a</td>
      <td>very good</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>a</td>
      <td>very good</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>a</td>
      <td>very good</td>
    </tr>
  </tbody>
</table>
</div>



* `observed` default값은 False 입니다. True 설정을 하게 되면 빈 범주 표시가 되지 않습니다. 


```python
df.groupby("grade", observed = True).size()
```




    grade
    very good    3
    good         2
    very bad     1
    dtype: int64




```python
df.groupby("grade", observed = False).size()
```




    grade
    very bad     1
    bad          0
    medium       0
    good         2
    very good    3
    dtype: int64



---

# Plotting 


```python
import matplotlib.pyplot as plt 
plt.close("all")
```


```python
ts = pd.Series(np.random.randn(1000), index=pd.date_range("1/1/2000", periods=1000))

ts = ts.cumsum()

ts.plot();
```


    
![png](/assets/images/Library/third/output_182_0.png)
    



```python
df = pd.DataFrame(
    np.random.randn(1000, 4), index=ts.index, columns=["A", "B", "C", "D"])

df = df.cumsum()

plt.figure();

df.plot();

plt.legend(loc='best');
```


    <Figure size 432x288 with 0 Axes>



    
![png](/assets/images/Library/third/output_183_1.png)
    


---

# Importing and exporting data

## CSV

* `DataFrame.to_csv()`


```python
df = pd.DataFrame(np.random.randint(0, 5, (10, 5)))

df.to_csv("foo.csv")
```


```python
pd.read_csv("foo.csv")
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
      <th>Unnamed: 0</th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>4</td>
      <td>3</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>0</td>
      <td>1</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>0</td>
      <td>0</td>
      <td>4</td>
      <td>3</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>3</td>
      <td>2</td>
      <td>4</td>
      <td>4</td>
      <td>3</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>1</td>
      <td>2</td>
      <td>4</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>2</td>
      <td>3</td>
      <td>4</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>



## Excel

* `DataFrame.to_excel()`


```python
df.to_excel("foo.xlsx", sheet_name="Sheet1")
```


```python
pd.read_excel("foo.xlsx", "Sheet1", index_col=None, na_values=["NA"])
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
      <th>Unnamed: 0</th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>4</td>
      <td>3</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>0</td>
      <td>1</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>0</td>
      <td>0</td>
      <td>4</td>
      <td>3</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>3</td>
      <td>2</td>
      <td>4</td>
      <td>4</td>
      <td>3</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>1</td>
      <td>2</td>
      <td>4</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>2</td>
      <td>3</td>
      <td>4</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>


