---
title: "Python dfply"
tags: [Python, TIL]
style:
color:
description: R dply function using Python dfply
---

### dfply

R의 dplyr과 비슷한 기능을 Python에서도 사용가능 한 함수가 dfply다.

*참고: <https://towardsdatascience.com/dplyr-style-data-manipulation-with-pipes-in-python-380dcb137000> <br/>
          <https://github.com/allenakinkunle/dplyr-style-data-manipulation-in-python>

dfply는 Python 3 버전 이상에서만 작동한다고 합니다


```python
#dfply는 밑에 방법으로 설치합니다
!pip install dfply
```

    Collecting dfply
      Downloading dfply-0.3.3-py3-none-any.whl (612 kB)
    Requirement already satisfied: pandas in c:\users\lg\anaconda3\lib\site-packages (from dfply) (1.0.1)
    Requirement already satisfied: numpy in c:\users\lg\anaconda3\lib\site-packages (from dfply) (1.18.1)
    Requirement already satisfied: pytz>=2017.2 in c:\users\lg\anaconda3\lib\site-packages (from pandas->dfply) (2019.3)
    Requirement already satisfied: python-dateutil>=2.6.1 in c:\users\lg\anaconda3\lib\site-packages (from pandas->dfply) (2.8.1)
    Requirement already satisfied: six>=1.5 in c:\users\lg\anaconda3\lib\site-packages (from python-dateutil>=2.6.1->pandas->dfply) (1.14.0)
    Installing collected packages: dfply
    Successfully installed dfply-0.3.3
    


```python
# 필요한 라이브러리를 로드합니다.
import pandas as pd
from dfply import * #*는 모두를 의미합니다
```


```python
# pandas를 활용하면 url에서 직접 데이터를 가져올 수 있습니다.
url = "https://raw.githubusercontent.com/allenakinkunle/dplyr-style-data-manipulation-in-python/master/nycflights13.csv"
```


```python
# df라는 변수에 데이터를 담아줍시다.
df = pd.read_csv(url)
```


```python
# head를 이용해 데이터를 미리보기 합시다
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
      <th>year</th>
      <th>month</th>
      <th>day</th>
      <th>dep_time</th>
      <th>sched_dep_time</th>
      <th>dep_delay</th>
      <th>arr_time</th>
      <th>sched_arr_time</th>
      <th>arr_delay</th>
      <th>carrier</th>
      <th>flight</th>
      <th>tailnum</th>
      <th>origin</th>
      <th>dest</th>
      <th>air_time</th>
      <th>distance</th>
      <th>hour</th>
      <th>minute</th>
      <th>time_hour</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>517.0</td>
      <td>515</td>
      <td>2.0</td>
      <td>830.0</td>
      <td>819</td>
      <td>11.0</td>
      <td>UA</td>
      <td>1545</td>
      <td>N14228</td>
      <td>EWR</td>
      <td>IAH</td>
      <td>227.0</td>
      <td>1400</td>
      <td>5</td>
      <td>15</td>
      <td>2013-01-01 05:00:00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>533.0</td>
      <td>529</td>
      <td>4.0</td>
      <td>850.0</td>
      <td>830</td>
      <td>20.0</td>
      <td>UA</td>
      <td>1714</td>
      <td>N24211</td>
      <td>LGA</td>
      <td>IAH</td>
      <td>227.0</td>
      <td>1416</td>
      <td>5</td>
      <td>29</td>
      <td>2013-01-01 05:00:00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>542.0</td>
      <td>540</td>
      <td>2.0</td>
      <td>923.0</td>
      <td>850</td>
      <td>33.0</td>
      <td>AA</td>
      <td>1141</td>
      <td>N619AA</td>
      <td>JFK</td>
      <td>MIA</td>
      <td>160.0</td>
      <td>1089</td>
      <td>5</td>
      <td>40</td>
      <td>2013-01-01 05:00:00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>544.0</td>
      <td>545</td>
      <td>-1.0</td>
      <td>1004.0</td>
      <td>1022</td>
      <td>-18.0</td>
      <td>B6</td>
      <td>725</td>
      <td>N804JB</td>
      <td>JFK</td>
      <td>BQN</td>
      <td>183.0</td>
      <td>1576</td>
      <td>5</td>
      <td>45</td>
      <td>2013-01-01 05:00:00</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>554.0</td>
      <td>600</td>
      <td>-6.0</td>
      <td>812.0</td>
      <td>837</td>
      <td>-25.0</td>
      <td>DL</td>
      <td>461</td>
      <td>N668DN</td>
      <td>LGA</td>
      <td>ATL</td>
      <td>116.0</td>
      <td>762</td>
      <td>6</td>
      <td>0</td>
      <td>2013-01-01 06:00:00</td>
    </tr>
  </tbody>
</table>
</div>



pipe는 dplyr의 가장 강력한 무기라고 생각합니다.<br/>
R에서는 %>%로 여러 함수를 연결 가능합니다.<br/>
dfply의 경우 >>가 파이프 연산자 역활을 합니다.


```python
# drop과 select 모두 dfply에 포함되어 있는 기능입니다.
# select 값은 인수로 전달된 열을 반환하고 drop은 반환하지 않습니다.
(df >>
select(X.origin,X.dest,X.hour))
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
      <th>origin</th>
      <th>dest</th>
      <th>hour</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>EWR</td>
      <td>IAH</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>LGA</td>
      <td>IAH</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>JFK</td>
      <td>MIA</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>JFK</td>
      <td>BQN</td>
      <td>5</td>
    </tr>
    <tr>
      <th>4</th>
      <td>LGA</td>
      <td>ATL</td>
      <td>6</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>336771</th>
      <td>JFK</td>
      <td>DCA</td>
      <td>14</td>
    </tr>
    <tr>
      <th>336772</th>
      <td>LGA</td>
      <td>SYR</td>
      <td>22</td>
    </tr>
    <tr>
      <th>336773</th>
      <td>LGA</td>
      <td>BNA</td>
      <td>12</td>
    </tr>
    <tr>
      <th>336774</th>
      <td>LGA</td>
      <td>CLE</td>
      <td>11</td>
    </tr>
    <tr>
      <th>336775</th>
      <td>LGA</td>
      <td>RDU</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
<p>336776 rows × 3 columns</p>
</div>




```python
(df >> 
drop(X.year,X.month,X.day))
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
      <th>dep_time</th>
      <th>sched_dep_time</th>
      <th>dep_delay</th>
      <th>arr_time</th>
      <th>sched_arr_time</th>
      <th>arr_delay</th>
      <th>carrier</th>
      <th>flight</th>
      <th>tailnum</th>
      <th>origin</th>
      <th>dest</th>
      <th>air_time</th>
      <th>distance</th>
      <th>hour</th>
      <th>minute</th>
      <th>time_hour</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>517.0</td>
      <td>515</td>
      <td>2.0</td>
      <td>830.0</td>
      <td>819</td>
      <td>11.0</td>
      <td>UA</td>
      <td>1545</td>
      <td>N14228</td>
      <td>EWR</td>
      <td>IAH</td>
      <td>227.0</td>
      <td>1400</td>
      <td>5</td>
      <td>15</td>
      <td>2013-01-01 05:00:00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>533.0</td>
      <td>529</td>
      <td>4.0</td>
      <td>850.0</td>
      <td>830</td>
      <td>20.0</td>
      <td>UA</td>
      <td>1714</td>
      <td>N24211</td>
      <td>LGA</td>
      <td>IAH</td>
      <td>227.0</td>
      <td>1416</td>
      <td>5</td>
      <td>29</td>
      <td>2013-01-01 05:00:00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>542.0</td>
      <td>540</td>
      <td>2.0</td>
      <td>923.0</td>
      <td>850</td>
      <td>33.0</td>
      <td>AA</td>
      <td>1141</td>
      <td>N619AA</td>
      <td>JFK</td>
      <td>MIA</td>
      <td>160.0</td>
      <td>1089</td>
      <td>5</td>
      <td>40</td>
      <td>2013-01-01 05:00:00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>544.0</td>
      <td>545</td>
      <td>-1.0</td>
      <td>1004.0</td>
      <td>1022</td>
      <td>-18.0</td>
      <td>B6</td>
      <td>725</td>
      <td>N804JB</td>
      <td>JFK</td>
      <td>BQN</td>
      <td>183.0</td>
      <td>1576</td>
      <td>5</td>
      <td>45</td>
      <td>2013-01-01 05:00:00</td>
    </tr>
    <tr>
      <th>4</th>
      <td>554.0</td>
      <td>600</td>
      <td>-6.0</td>
      <td>812.0</td>
      <td>837</td>
      <td>-25.0</td>
      <td>DL</td>
      <td>461</td>
      <td>N668DN</td>
      <td>LGA</td>
      <td>ATL</td>
      <td>116.0</td>
      <td>762</td>
      <td>6</td>
      <td>0</td>
      <td>2013-01-01 06:00:00</td>
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
    </tr>
    <tr>
      <th>336771</th>
      <td>NaN</td>
      <td>1455</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1634</td>
      <td>NaN</td>
      <td>9E</td>
      <td>3393</td>
      <td>NaN</td>
      <td>JFK</td>
      <td>DCA</td>
      <td>NaN</td>
      <td>213</td>
      <td>14</td>
      <td>55</td>
      <td>2013-09-30 14:00:00</td>
    </tr>
    <tr>
      <th>336772</th>
      <td>NaN</td>
      <td>2200</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2312</td>
      <td>NaN</td>
      <td>9E</td>
      <td>3525</td>
      <td>NaN</td>
      <td>LGA</td>
      <td>SYR</td>
      <td>NaN</td>
      <td>198</td>
      <td>22</td>
      <td>0</td>
      <td>2013-09-30 22:00:00</td>
    </tr>
    <tr>
      <th>336773</th>
      <td>NaN</td>
      <td>1210</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1330</td>
      <td>NaN</td>
      <td>MQ</td>
      <td>3461</td>
      <td>N535MQ</td>
      <td>LGA</td>
      <td>BNA</td>
      <td>NaN</td>
      <td>764</td>
      <td>12</td>
      <td>10</td>
      <td>2013-09-30 12:00:00</td>
    </tr>
    <tr>
      <th>336774</th>
      <td>NaN</td>
      <td>1159</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1344</td>
      <td>NaN</td>
      <td>MQ</td>
      <td>3572</td>
      <td>N511MQ</td>
      <td>LGA</td>
      <td>CLE</td>
      <td>NaN</td>
      <td>419</td>
      <td>11</td>
      <td>59</td>
      <td>2013-09-30 11:00:00</td>
    </tr>
    <tr>
      <th>336775</th>
      <td>NaN</td>
      <td>840</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1020</td>
      <td>NaN</td>
      <td>MQ</td>
      <td>3531</td>
      <td>N839MQ</td>
      <td>LGA</td>
      <td>RDU</td>
      <td>NaN</td>
      <td>431</td>
      <td>8</td>
      <td>40</td>
      <td>2013-09-30 08:00:00</td>
    </tr>
  </tbody>
</table>
<p>336776 rows × 16 columns</p>
</div>




```python
# select 문에서도 ~를 사용하면 인수로 전달된 값을 반환하지 않습니다.
(df >>
select(~X.hour,~X.minute))
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
      <th>year</th>
      <th>month</th>
      <th>day</th>
      <th>dep_time</th>
      <th>sched_dep_time</th>
      <th>dep_delay</th>
      <th>arr_time</th>
      <th>sched_arr_time</th>
      <th>arr_delay</th>
      <th>carrier</th>
      <th>flight</th>
      <th>tailnum</th>
      <th>origin</th>
      <th>dest</th>
      <th>air_time</th>
      <th>distance</th>
      <th>time_hour</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>517.0</td>
      <td>515</td>
      <td>2.0</td>
      <td>830.0</td>
      <td>819</td>
      <td>11.0</td>
      <td>UA</td>
      <td>1545</td>
      <td>N14228</td>
      <td>EWR</td>
      <td>IAH</td>
      <td>227.0</td>
      <td>1400</td>
      <td>2013-01-01 05:00:00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>533.0</td>
      <td>529</td>
      <td>4.0</td>
      <td>850.0</td>
      <td>830</td>
      <td>20.0</td>
      <td>UA</td>
      <td>1714</td>
      <td>N24211</td>
      <td>LGA</td>
      <td>IAH</td>
      <td>227.0</td>
      <td>1416</td>
      <td>2013-01-01 05:00:00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>542.0</td>
      <td>540</td>
      <td>2.0</td>
      <td>923.0</td>
      <td>850</td>
      <td>33.0</td>
      <td>AA</td>
      <td>1141</td>
      <td>N619AA</td>
      <td>JFK</td>
      <td>MIA</td>
      <td>160.0</td>
      <td>1089</td>
      <td>2013-01-01 05:00:00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>544.0</td>
      <td>545</td>
      <td>-1.0</td>
      <td>1004.0</td>
      <td>1022</td>
      <td>-18.0</td>
      <td>B6</td>
      <td>725</td>
      <td>N804JB</td>
      <td>JFK</td>
      <td>BQN</td>
      <td>183.0</td>
      <td>1576</td>
      <td>2013-01-01 05:00:00</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>554.0</td>
      <td>600</td>
      <td>-6.0</td>
      <td>812.0</td>
      <td>837</td>
      <td>-25.0</td>
      <td>DL</td>
      <td>461</td>
      <td>N668DN</td>
      <td>LGA</td>
      <td>ATL</td>
      <td>116.0</td>
      <td>762</td>
      <td>2013-01-01 06:00:00</td>
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
      <th>336771</th>
      <td>2013</td>
      <td>9</td>
      <td>30</td>
      <td>NaN</td>
      <td>1455</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1634</td>
      <td>NaN</td>
      <td>9E</td>
      <td>3393</td>
      <td>NaN</td>
      <td>JFK</td>
      <td>DCA</td>
      <td>NaN</td>
      <td>213</td>
      <td>2013-09-30 14:00:00</td>
    </tr>
    <tr>
      <th>336772</th>
      <td>2013</td>
      <td>9</td>
      <td>30</td>
      <td>NaN</td>
      <td>2200</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2312</td>
      <td>NaN</td>
      <td>9E</td>
      <td>3525</td>
      <td>NaN</td>
      <td>LGA</td>
      <td>SYR</td>
      <td>NaN</td>
      <td>198</td>
      <td>2013-09-30 22:00:00</td>
    </tr>
    <tr>
      <th>336773</th>
      <td>2013</td>
      <td>9</td>
      <td>30</td>
      <td>NaN</td>
      <td>1210</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1330</td>
      <td>NaN</td>
      <td>MQ</td>
      <td>3461</td>
      <td>N535MQ</td>
      <td>LGA</td>
      <td>BNA</td>
      <td>NaN</td>
      <td>764</td>
      <td>2013-09-30 12:00:00</td>
    </tr>
    <tr>
      <th>336774</th>
      <td>2013</td>
      <td>9</td>
      <td>30</td>
      <td>NaN</td>
      <td>1159</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1344</td>
      <td>NaN</td>
      <td>MQ</td>
      <td>3572</td>
      <td>N511MQ</td>
      <td>LGA</td>
      <td>CLE</td>
      <td>NaN</td>
      <td>419</td>
      <td>2013-09-30 11:00:00</td>
    </tr>
    <tr>
      <th>336775</th>
      <td>2013</td>
      <td>9</td>
      <td>30</td>
      <td>NaN</td>
      <td>840</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1020</td>
      <td>NaN</td>
      <td>MQ</td>
      <td>3531</td>
      <td>N839MQ</td>
      <td>LGA</td>
      <td>RDU</td>
      <td>NaN</td>
      <td>431</td>
      <td>2013-09-30 08:00:00</td>
    </tr>
  </tbody>
</table>
<p>336776 rows × 17 columns</p>
</div>




```python
# mask()를 통해 결과를 필터링 할 수 있습니다.
(df >>
mask(X.month == 1, X.day == 1, X.origin == 'JFK', X.hour > 10))
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
      <th>year</th>
      <th>month</th>
      <th>day</th>
      <th>dep_time</th>
      <th>sched_dep_time</th>
      <th>dep_delay</th>
      <th>arr_time</th>
      <th>sched_arr_time</th>
      <th>arr_delay</th>
      <th>carrier</th>
      <th>flight</th>
      <th>tailnum</th>
      <th>origin</th>
      <th>dest</th>
      <th>air_time</th>
      <th>distance</th>
      <th>hour</th>
      <th>minute</th>
      <th>time_hour</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>151</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>848.0</td>
      <td>1835</td>
      <td>853.0</td>
      <td>1001.0</td>
      <td>1950</td>
      <td>851.0</td>
      <td>MQ</td>
      <td>3944</td>
      <td>N942MQ</td>
      <td>JFK</td>
      <td>BWI</td>
      <td>41.0</td>
      <td>184</td>
      <td>18</td>
      <td>35</td>
      <td>2013-01-01 18:00:00</td>
    </tr>
    <tr>
      <th>258</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>1059.0</td>
      <td>1100</td>
      <td>-1.0</td>
      <td>1210.0</td>
      <td>1215</td>
      <td>-5.0</td>
      <td>MQ</td>
      <td>3792</td>
      <td>N509MQ</td>
      <td>JFK</td>
      <td>DCA</td>
      <td>50.0</td>
      <td>213</td>
      <td>11</td>
      <td>0</td>
      <td>2013-01-01 11:00:00</td>
    </tr>
    <tr>
      <th>265</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>1111.0</td>
      <td>1115</td>
      <td>-4.0</td>
      <td>1222.0</td>
      <td>1226</td>
      <td>-4.0</td>
      <td>B6</td>
      <td>24</td>
      <td>N279JB</td>
      <td>JFK</td>
      <td>BTV</td>
      <td>52.0</td>
      <td>266</td>
      <td>11</td>
      <td>15</td>
      <td>2013-01-01 11:00:00</td>
    </tr>
    <tr>
      <th>266</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>1112.0</td>
      <td>1100</td>
      <td>12.0</td>
      <td>1440.0</td>
      <td>1438</td>
      <td>2.0</td>
      <td>UA</td>
      <td>285</td>
      <td>N517UA</td>
      <td>JFK</td>
      <td>SFO</td>
      <td>364.0</td>
      <td>2586</td>
      <td>11</td>
      <td>0</td>
      <td>2013-01-01 11:00:00</td>
    </tr>
    <tr>
      <th>272</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>1124.0</td>
      <td>1100</td>
      <td>24.0</td>
      <td>1435.0</td>
      <td>1431</td>
      <td>4.0</td>
      <td>B6</td>
      <td>641</td>
      <td>N590JB</td>
      <td>JFK</td>
      <td>SFO</td>
      <td>349.0</td>
      <td>2586</td>
      <td>11</td>
      <td>0</td>
      <td>2013-01-01 11:00:00</td>
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
    </tr>
    <tr>
      <th>832</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>2326.0</td>
      <td>2130</td>
      <td>116.0</td>
      <td>131.0</td>
      <td>18</td>
      <td>73.0</td>
      <td>B6</td>
      <td>199</td>
      <td>N594JB</td>
      <td>JFK</td>
      <td>LAS</td>
      <td>290.0</td>
      <td>2248</td>
      <td>21</td>
      <td>30</td>
      <td>2013-01-01 21:00:00</td>
    </tr>
    <tr>
      <th>833</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>2327.0</td>
      <td>2250</td>
      <td>37.0</td>
      <td>32.0</td>
      <td>2359</td>
      <td>33.0</td>
      <td>B6</td>
      <td>22</td>
      <td>N639JB</td>
      <td>JFK</td>
      <td>SYR</td>
      <td>45.0</td>
      <td>209</td>
      <td>22</td>
      <td>50</td>
      <td>2013-01-01 22:00:00</td>
    </tr>
    <tr>
      <th>835</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>2353.0</td>
      <td>2359</td>
      <td>-6.0</td>
      <td>425.0</td>
      <td>445</td>
      <td>-20.0</td>
      <td>B6</td>
      <td>739</td>
      <td>N591JB</td>
      <td>JFK</td>
      <td>PSE</td>
      <td>195.0</td>
      <td>1617</td>
      <td>23</td>
      <td>59</td>
      <td>2013-01-01 23:00:00</td>
    </tr>
    <tr>
      <th>836</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>2353.0</td>
      <td>2359</td>
      <td>-6.0</td>
      <td>418.0</td>
      <td>442</td>
      <td>-24.0</td>
      <td>B6</td>
      <td>707</td>
      <td>N794JB</td>
      <td>JFK</td>
      <td>SJU</td>
      <td>185.0</td>
      <td>1598</td>
      <td>23</td>
      <td>59</td>
      <td>2013-01-01 23:00:00</td>
    </tr>
    <tr>
      <th>837</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>2356.0</td>
      <td>2359</td>
      <td>-3.0</td>
      <td>425.0</td>
      <td>437</td>
      <td>-12.0</td>
      <td>B6</td>
      <td>727</td>
      <td>N588JB</td>
      <td>JFK</td>
      <td>BQN</td>
      <td>186.0</td>
      <td>1576</td>
      <td>23</td>
      <td>59</td>
      <td>2013-01-01 23:00:00</td>
    </tr>
  </tbody>
</table>
<p>213 rows × 19 columns</p>
</div>




```python
# arrange()를 통해 결과를 정렬 할 수 있습니다.
# 밑의 코드 같은 경우 distance로 먼저 정렬하고 그 다음에 hours로 정렬합니다.
(df >>
arrange(X.distance, X.hour))
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
      <th>year</th>
      <th>month</th>
      <th>day</th>
      <th>dep_time</th>
      <th>sched_dep_time</th>
      <th>dep_delay</th>
      <th>arr_time</th>
      <th>sched_arr_time</th>
      <th>arr_delay</th>
      <th>carrier</th>
      <th>flight</th>
      <th>tailnum</th>
      <th>origin</th>
      <th>dest</th>
      <th>air_time</th>
      <th>distance</th>
      <th>hour</th>
      <th>minute</th>
      <th>time_hour</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>275945</th>
      <td>2013</td>
      <td>7</td>
      <td>27</td>
      <td>NaN</td>
      <td>106</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>245</td>
      <td>NaN</td>
      <td>US</td>
      <td>1632</td>
      <td>NaN</td>
      <td>EWR</td>
      <td>LGA</td>
      <td>NaN</td>
      <td>17</td>
      <td>1</td>
      <td>6</td>
      <td>2013-07-27 01:00:00</td>
    </tr>
    <tr>
      <th>3083</th>
      <td>2013</td>
      <td>1</td>
      <td>4</td>
      <td>1240.0</td>
      <td>1200</td>
      <td>40.0</td>
      <td>1333.0</td>
      <td>1306</td>
      <td>27.0</td>
      <td>EV</td>
      <td>4193</td>
      <td>N14972</td>
      <td>EWR</td>
      <td>PHL</td>
      <td>30.0</td>
      <td>80</td>
      <td>12</td>
      <td>0</td>
      <td>2013-01-04 12:00:00</td>
    </tr>
    <tr>
      <th>3901</th>
      <td>2013</td>
      <td>1</td>
      <td>5</td>
      <td>1155.0</td>
      <td>1200</td>
      <td>-5.0</td>
      <td>1241.0</td>
      <td>1306</td>
      <td>-25.0</td>
      <td>EV</td>
      <td>4193</td>
      <td>N14902</td>
      <td>EWR</td>
      <td>PHL</td>
      <td>29.0</td>
      <td>80</td>
      <td>12</td>
      <td>0</td>
      <td>2013-01-05 12:00:00</td>
    </tr>
    <tr>
      <th>3426</th>
      <td>2013</td>
      <td>1</td>
      <td>4</td>
      <td>1829.0</td>
      <td>1615</td>
      <td>134.0</td>
      <td>1937.0</td>
      <td>1721</td>
      <td>136.0</td>
      <td>EV</td>
      <td>4502</td>
      <td>N15983</td>
      <td>EWR</td>
      <td>PHL</td>
      <td>28.0</td>
      <td>80</td>
      <td>16</td>
      <td>15</td>
      <td>2013-01-04 16:00:00</td>
    </tr>
    <tr>
      <th>10235</th>
      <td>2013</td>
      <td>1</td>
      <td>12</td>
      <td>1613.0</td>
      <td>1617</td>
      <td>-4.0</td>
      <td>1708.0</td>
      <td>1722</td>
      <td>-14.0</td>
      <td>EV</td>
      <td>4616</td>
      <td>N11150</td>
      <td>EWR</td>
      <td>PHL</td>
      <td>36.0</td>
      <td>80</td>
      <td>16</td>
      <td>17</td>
      <td>2013-01-12 16:00:00</td>
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
    </tr>
    <tr>
      <th>331506</th>
      <td>2013</td>
      <td>9</td>
      <td>25</td>
      <td>1001.0</td>
      <td>1000</td>
      <td>1.0</td>
      <td>1508.0</td>
      <td>1445</td>
      <td>23.0</td>
      <td>HA</td>
      <td>51</td>
      <td>N389HA</td>
      <td>JFK</td>
      <td>HNL</td>
      <td>636.0</td>
      <td>4983</td>
      <td>10</td>
      <td>0</td>
      <td>2013-09-25 10:00:00</td>
    </tr>
    <tr>
      <th>333478</th>
      <td>2013</td>
      <td>9</td>
      <td>27</td>
      <td>951.0</td>
      <td>1000</td>
      <td>-9.0</td>
      <td>1442.0</td>
      <td>1445</td>
      <td>-3.0</td>
      <td>HA</td>
      <td>51</td>
      <td>N390HA</td>
      <td>JFK</td>
      <td>HNL</td>
      <td>629.0</td>
      <td>4983</td>
      <td>10</td>
      <td>0</td>
      <td>2013-09-27 10:00:00</td>
    </tr>
    <tr>
      <th>334406</th>
      <td>2013</td>
      <td>9</td>
      <td>28</td>
      <td>955.0</td>
      <td>1000</td>
      <td>-5.0</td>
      <td>1412.0</td>
      <td>1445</td>
      <td>-33.0</td>
      <td>HA</td>
      <td>51</td>
      <td>N391HA</td>
      <td>JFK</td>
      <td>HNL</td>
      <td>584.0</td>
      <td>4983</td>
      <td>10</td>
      <td>0</td>
      <td>2013-09-28 10:00:00</td>
    </tr>
    <tr>
      <th>335095</th>
      <td>2013</td>
      <td>9</td>
      <td>29</td>
      <td>957.0</td>
      <td>1000</td>
      <td>-3.0</td>
      <td>1405.0</td>
      <td>1445</td>
      <td>-40.0</td>
      <td>HA</td>
      <td>51</td>
      <td>N384HA</td>
      <td>JFK</td>
      <td>HNL</td>
      <td>580.0</td>
      <td>4983</td>
      <td>10</td>
      <td>0</td>
      <td>2013-09-29 10:00:00</td>
    </tr>
    <tr>
      <th>336081</th>
      <td>2013</td>
      <td>9</td>
      <td>30</td>
      <td>959.0</td>
      <td>1000</td>
      <td>-1.0</td>
      <td>1438.0</td>
      <td>1445</td>
      <td>-7.0</td>
      <td>HA</td>
      <td>51</td>
      <td>N392HA</td>
      <td>JFK</td>
      <td>HNL</td>
      <td>603.0</td>
      <td>4983</td>
      <td>10</td>
      <td>0</td>
      <td>2013-09-30 10:00:00</td>
    </tr>
  </tbody>
</table>
<p>336776 rows × 19 columns</p>
</div>




```python
# 내림차순으로 보고싶다면 ascending =False를 해주면 됩니다
((df >>
 arrange(X.distance,X.hour, ascending=False)))
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
      <th>year</th>
      <th>month</th>
      <th>day</th>
      <th>dep_time</th>
      <th>sched_dep_time</th>
      <th>dep_delay</th>
      <th>arr_time</th>
      <th>sched_arr_time</th>
      <th>arr_delay</th>
      <th>carrier</th>
      <th>flight</th>
      <th>tailnum</th>
      <th>origin</th>
      <th>dest</th>
      <th>air_time</th>
      <th>distance</th>
      <th>hour</th>
      <th>minute</th>
      <th>time_hour</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>28259</th>
      <td>2013</td>
      <td>10</td>
      <td>2</td>
      <td>951.0</td>
      <td>1000</td>
      <td>-9.0</td>
      <td>1438.0</td>
      <td>1450</td>
      <td>-12.0</td>
      <td>HA</td>
      <td>51</td>
      <td>N381HA</td>
      <td>JFK</td>
      <td>HNL</td>
      <td>623.0</td>
      <td>4983</td>
      <td>10</td>
      <td>0</td>
      <td>2013-10-02 10:00:00</td>
    </tr>
    <tr>
      <th>30229</th>
      <td>2013</td>
      <td>10</td>
      <td>4</td>
      <td>954.0</td>
      <td>1000</td>
      <td>-6.0</td>
      <td>1438.0</td>
      <td>1450</td>
      <td>-12.0</td>
      <td>HA</td>
      <td>51</td>
      <td>N380HA</td>
      <td>JFK</td>
      <td>HNL</td>
      <td>618.0</td>
      <td>4983</td>
      <td>10</td>
      <td>0</td>
      <td>2013-10-04 10:00:00</td>
    </tr>
    <tr>
      <th>31157</th>
      <td>2013</td>
      <td>10</td>
      <td>5</td>
      <td>1002.0</td>
      <td>1000</td>
      <td>2.0</td>
      <td>1418.0</td>
      <td>1450</td>
      <td>-32.0</td>
      <td>HA</td>
      <td>51</td>
      <td>N384HA</td>
      <td>JFK</td>
      <td>HNL</td>
      <td>593.0</td>
      <td>4983</td>
      <td>10</td>
      <td>0</td>
      <td>2013-10-05 10:00:00</td>
    </tr>
    <tr>
      <th>31850</th>
      <td>2013</td>
      <td>10</td>
      <td>6</td>
      <td>958.0</td>
      <td>1000</td>
      <td>-2.0</td>
      <td>1415.0</td>
      <td>1450</td>
      <td>-35.0</td>
      <td>HA</td>
      <td>51</td>
      <td>N389HA</td>
      <td>JFK</td>
      <td>HNL</td>
      <td>601.0</td>
      <td>4983</td>
      <td>10</td>
      <td>0</td>
      <td>2013-10-06 10:00:00</td>
    </tr>
    <tr>
      <th>32842</th>
      <td>2013</td>
      <td>10</td>
      <td>7</td>
      <td>957.0</td>
      <td>1000</td>
      <td>-3.0</td>
      <td>1504.0</td>
      <td>1450</td>
      <td>14.0</td>
      <td>HA</td>
      <td>51</td>
      <td>N390HA</td>
      <td>JFK</td>
      <td>HNL</td>
      <td>642.0</td>
      <td>4983</td>
      <td>10</td>
      <td>0</td>
      <td>2013-10-07 10:00:00</td>
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
    </tr>
    <tr>
      <th>112694</th>
      <td>2013</td>
      <td>2</td>
      <td>2</td>
      <td>1610.0</td>
      <td>1617</td>
      <td>-7.0</td>
      <td>1702.0</td>
      <td>1722</td>
      <td>-20.0</td>
      <td>EV</td>
      <td>4616</td>
      <td>N18120</td>
      <td>EWR</td>
      <td>PHL</td>
      <td>33.0</td>
      <td>80</td>
      <td>16</td>
      <td>17</td>
      <td>2013-02-02 16:00:00</td>
    </tr>
    <tr>
      <th>118425</th>
      <td>2013</td>
      <td>2</td>
      <td>9</td>
      <td>1619.0</td>
      <td>1617</td>
      <td>2.0</td>
      <td>1706.0</td>
      <td>1722</td>
      <td>-16.0</td>
      <td>EV</td>
      <td>4616</td>
      <td>N10575</td>
      <td>EWR</td>
      <td>PHL</td>
      <td>25.0</td>
      <td>80</td>
      <td>16</td>
      <td>17</td>
      <td>2013-02-09 16:00:00</td>
    </tr>
    <tr>
      <th>3083</th>
      <td>2013</td>
      <td>1</td>
      <td>4</td>
      <td>1240.0</td>
      <td>1200</td>
      <td>40.0</td>
      <td>1333.0</td>
      <td>1306</td>
      <td>27.0</td>
      <td>EV</td>
      <td>4193</td>
      <td>N14972</td>
      <td>EWR</td>
      <td>PHL</td>
      <td>30.0</td>
      <td>80</td>
      <td>12</td>
      <td>0</td>
      <td>2013-01-04 12:00:00</td>
    </tr>
    <tr>
      <th>3901</th>
      <td>2013</td>
      <td>1</td>
      <td>5</td>
      <td>1155.0</td>
      <td>1200</td>
      <td>-5.0</td>
      <td>1241.0</td>
      <td>1306</td>
      <td>-25.0</td>
      <td>EV</td>
      <td>4193</td>
      <td>N14902</td>
      <td>EWR</td>
      <td>PHL</td>
      <td>29.0</td>
      <td>80</td>
      <td>12</td>
      <td>0</td>
      <td>2013-01-05 12:00:00</td>
    </tr>
    <tr>
      <th>275945</th>
      <td>2013</td>
      <td>7</td>
      <td>27</td>
      <td>NaN</td>
      <td>106</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>245</td>
      <td>NaN</td>
      <td>US</td>
      <td>1632</td>
      <td>NaN</td>
      <td>EWR</td>
      <td>LGA</td>
      <td>NaN</td>
      <td>17</td>
      <td>1</td>
      <td>6</td>
      <td>2013-07-27 01:00:00</td>
    </tr>
  </tbody>
</table>
<p>336776 rows × 19 columns</p>
</div>




```python
# group by를 통해 데이터를 컬럼에 따라 묶을 수 있습니다.
# 밑의 경우는 집약함수를 설정을 안했기 때문에 데이터 그대로 반환됩니다.
(df >> 
group_by(X.origin))
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
      <th>year</th>
      <th>month</th>
      <th>day</th>
      <th>dep_time</th>
      <th>sched_dep_time</th>
      <th>dep_delay</th>
      <th>arr_time</th>
      <th>sched_arr_time</th>
      <th>arr_delay</th>
      <th>carrier</th>
      <th>flight</th>
      <th>tailnum</th>
      <th>origin</th>
      <th>dest</th>
      <th>air_time</th>
      <th>distance</th>
      <th>hour</th>
      <th>minute</th>
      <th>time_hour</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>517.0</td>
      <td>515</td>
      <td>2.0</td>
      <td>830.0</td>
      <td>819</td>
      <td>11.0</td>
      <td>UA</td>
      <td>1545</td>
      <td>N14228</td>
      <td>EWR</td>
      <td>IAH</td>
      <td>227.0</td>
      <td>1400</td>
      <td>5</td>
      <td>15</td>
      <td>2013-01-01 05:00:00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>533.0</td>
      <td>529</td>
      <td>4.0</td>
      <td>850.0</td>
      <td>830</td>
      <td>20.0</td>
      <td>UA</td>
      <td>1714</td>
      <td>N24211</td>
      <td>LGA</td>
      <td>IAH</td>
      <td>227.0</td>
      <td>1416</td>
      <td>5</td>
      <td>29</td>
      <td>2013-01-01 05:00:00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>542.0</td>
      <td>540</td>
      <td>2.0</td>
      <td>923.0</td>
      <td>850</td>
      <td>33.0</td>
      <td>AA</td>
      <td>1141</td>
      <td>N619AA</td>
      <td>JFK</td>
      <td>MIA</td>
      <td>160.0</td>
      <td>1089</td>
      <td>5</td>
      <td>40</td>
      <td>2013-01-01 05:00:00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>544.0</td>
      <td>545</td>
      <td>-1.0</td>
      <td>1004.0</td>
      <td>1022</td>
      <td>-18.0</td>
      <td>B6</td>
      <td>725</td>
      <td>N804JB</td>
      <td>JFK</td>
      <td>BQN</td>
      <td>183.0</td>
      <td>1576</td>
      <td>5</td>
      <td>45</td>
      <td>2013-01-01 05:00:00</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>554.0</td>
      <td>600</td>
      <td>-6.0</td>
      <td>812.0</td>
      <td>837</td>
      <td>-25.0</td>
      <td>DL</td>
      <td>461</td>
      <td>N668DN</td>
      <td>LGA</td>
      <td>ATL</td>
      <td>116.0</td>
      <td>762</td>
      <td>6</td>
      <td>0</td>
      <td>2013-01-01 06:00:00</td>
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
    </tr>
    <tr>
      <th>336771</th>
      <td>2013</td>
      <td>9</td>
      <td>30</td>
      <td>NaN</td>
      <td>1455</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1634</td>
      <td>NaN</td>
      <td>9E</td>
      <td>3393</td>
      <td>NaN</td>
      <td>JFK</td>
      <td>DCA</td>
      <td>NaN</td>
      <td>213</td>
      <td>14</td>
      <td>55</td>
      <td>2013-09-30 14:00:00</td>
    </tr>
    <tr>
      <th>336772</th>
      <td>2013</td>
      <td>9</td>
      <td>30</td>
      <td>NaN</td>
      <td>2200</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2312</td>
      <td>NaN</td>
      <td>9E</td>
      <td>3525</td>
      <td>NaN</td>
      <td>LGA</td>
      <td>SYR</td>
      <td>NaN</td>
      <td>198</td>
      <td>22</td>
      <td>0</td>
      <td>2013-09-30 22:00:00</td>
    </tr>
    <tr>
      <th>336773</th>
      <td>2013</td>
      <td>9</td>
      <td>30</td>
      <td>NaN</td>
      <td>1210</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1330</td>
      <td>NaN</td>
      <td>MQ</td>
      <td>3461</td>
      <td>N535MQ</td>
      <td>LGA</td>
      <td>BNA</td>
      <td>NaN</td>
      <td>764</td>
      <td>12</td>
      <td>10</td>
      <td>2013-09-30 12:00:00</td>
    </tr>
    <tr>
      <th>336774</th>
      <td>2013</td>
      <td>9</td>
      <td>30</td>
      <td>NaN</td>
      <td>1159</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1344</td>
      <td>NaN</td>
      <td>MQ</td>
      <td>3572</td>
      <td>N511MQ</td>
      <td>LGA</td>
      <td>CLE</td>
      <td>NaN</td>
      <td>419</td>
      <td>11</td>
      <td>59</td>
      <td>2013-09-30 11:00:00</td>
    </tr>
    <tr>
      <th>336775</th>
      <td>2013</td>
      <td>9</td>
      <td>30</td>
      <td>NaN</td>
      <td>840</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1020</td>
      <td>NaN</td>
      <td>MQ</td>
      <td>3531</td>
      <td>N839MQ</td>
      <td>LGA</td>
      <td>RDU</td>
      <td>NaN</td>
      <td>431</td>
      <td>8</td>
      <td>40</td>
      <td>2013-09-30 08:00:00</td>
    </tr>
  </tbody>
</table>
<p>336776 rows × 19 columns</p>
</div>




```python
#mean값으로 묶으면 origin이 group화 되서 반환됩니다.
(df >> 
group_by(X.origin) >>
summarize(mean_distance = X.distance.mean()))
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
      <th>origin</th>
      <th>mean_distance</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>EWR</td>
      <td>1056.742790</td>
    </tr>
    <tr>
      <th>1</th>
      <td>JFK</td>
      <td>1266.249077</td>
    </tr>
    <tr>
      <th>2</th>
      <td>LGA</td>
      <td>779.835671</td>
    </tr>
  </tbody>
</table>
</div>




```python
# dfply를 사용하면 다음 단계를 위해 변수를 저장할 필요가 없습니다.
(df >>
       mask(X.hour >10) >>
       mutate(speed = X.distance / (X.air_time * 60)) >>
             group_by(X.origin) >>
             summarize(mean_speed = X.speed.mean()) >>
             arrange(X.mean_speed, ascending=False))
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
      <th>origin</th>
      <th>mean_speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>EWR</td>
      <td>0.109777</td>
    </tr>
    <tr>
      <th>1</th>
      <td>JFK</td>
      <td>0.109427</td>
    </tr>
    <tr>
      <th>2</th>
      <td>LGA</td>
      <td>0.107362</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 위의 결과를 재사용을 위해 변수에 담을 수도 있습니다. 
df2 = (df >>
       mask(X.hour >10) >>
       mutate(speed = X.distance / (X.air_time * 60)) >>
             group_by(X.origin) >>
             summarize(mean_speed = X.speed.mean()) >>
             arrange(X.mean_speed, ascending=False))
```


```python
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
      <th>origin</th>
      <th>mean_speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>EWR</td>
      <td>0.109777</td>
    </tr>
    <tr>
      <th>1</th>
      <td>JFK</td>
      <td>0.109427</td>
    </tr>
    <tr>
      <th>2</th>
      <td>LGA</td>
      <td>0.107362</td>
    </tr>
  </tbody>
</table>
</div>




```python
# dfply를 사용하지 않는 다면 밑의 코드처럼 작성할 수 있습니다.
# 이 경우 새로운 변수를 지정해 줘야 합니다.
df.loc[df['hour'] > 10, 'speed'] = df['distance'] / (df['air_time'] * 60)
result = df.groupby('origin', as_index = False)['speed'].mean()
result.sort_values('speed', ascending = False)
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
      <th>origin</th>
      <th>speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>EWR</td>
      <td>0.109777</td>
    </tr>
    <tr>
      <th>1</th>
      <td>JFK</td>
      <td>0.109427</td>
    </tr>
    <tr>
      <th>2</th>
      <td>LGA</td>
      <td>0.107362</td>
    </tr>
  </tbody>
</table>
</div>



개인적으로 R의 강력한 기능이였던 dplyr을 Python에서 사용할 수 있어 편리하다고 생각합니다.
