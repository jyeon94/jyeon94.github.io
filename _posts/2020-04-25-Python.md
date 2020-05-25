---
title: "pd.cut을 이용한 label 부여"
tags: [Python,TIL]
style: 
color:
description: pandas cut을 이용하여 데이터 분할 및 label 부여
---

```python
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.preprocessing import LabelEncoder
```


```python
plt.rc('font',family='Malgun Gothic')
```


```python
df = pd.read_csv('../data/final_data.csv',encoding='cp949')
```


```python
labels = ['좋음','보통','나쁨','매우나쁨']
```


```python
SO2_bins = [(0,0.02),(0.021,0.05),(0.051,0.15),(0.151,1)]
CO_bins = [(0,2),(2.01,9),(9.01,15),(15.01,50)]
O3_bins = [(0,0.03),(0.031,0.09),(0.091,0.15),(0.151,0.6)]
NO2_bins = [(0,0.03),(0.031,0.06),(0.061,0.2),(0.201,2)]
PM10_bins = [(0,30),(31,80),(81,150),(151,600)]
PM25_bins = [(0,15),(16,35),(36,75),(76,500)]
```


```python
#both는 bins의 양끝값을 모두 포함합니다.
#대기오염을 좋음 부터 매우 나쁨까지 label을 부여하기 위한 함수입니다.
def condition(series,new_name,bins,labels,closed='both'):
   #tuple로 interval을 만들어 줍니다
    _bins = pd.IntervalIndex.from_tuples(bins,closed='both')
    d = dict(zip(_bins,labels))
    #dictionary와 map 함수를 통해 기존 컬럼의 값들을 label로  변경해서 새로운 컬럼에 할당합니다.
    df[new_name] =pd.cut(series,_bins,include_lowest=True,).map(d)
    return df[new_name]
```


```python
# NUll값 발생이유 -> 자리수가 안 맞음
# 해결을 위해 SO2,O3,NO2컬럼을 소수 세번째 자리까지 반올림
# PM10,PM25경우 일의 자리까지 반올림
# 함수 다시 적용
```


```python
df[['SO2_round','O3_round','NO2_round']] = df[['SO2','O3','NO2']].round(3)
df[['PM10_round','PM25_round']] = df[['PM10','PM25']].round(0).astype('int')
```


```python
condition(df['SO2_round'],'SO2_condition',SO2_bins,labels)
condition(df['CO'],'CO_condition',CO_bins,labels)
condition(df['O3_round'],'O3_condition',O3_bins,labels)
condition(df['NO2_round'],'NO2_condition',NO2_bins,labels)
condition(df['PM10_round'],'PM10_condition',PM10_bins,labels)
condition(df['PM25_round'],'PM25_condition',PM25_bins,labels)
```




    0        보통
    1        보통
    2        보통
    3        보통
    4        보통
             ..
    32860    좋음
    32861    보통
    32862    보통
    32863    보통
    32864    보통
    Name: PM25_condition, Length: 32865, dtype: category
    Categories (4, object): [좋음 < 보통 < 나쁨 < 매우나쁨]




```python
df.columns
```




    Index(['판매일시', '파종일시', '도_num', '연도', 'SO2', 'CO', 'O3', 'NO2', 'PM10', 'PM25',
           '합계 일조시간(hr)', '품종명', '도', '마켓명', '가격', 'SO2_condition', 'CO_condition',
           'O3_condition', 'NO2_condition', 'PM10_condition', 'PM25_condition'],
          dtype='object')




```python
(df[['SO2_condition', 'CO_condition','O3_condition', 'NO2_condition', 'PM10_condition', 'PM25_condition']]) 
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
      <th>SO2_condition</th>
      <th>CO_condition</th>
      <th>O3_condition</th>
      <th>NO2_condition</th>
      <th>PM10_condition</th>
      <th>PM25_condition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>보통</td>
      <td>보통</td>
    </tr>
    <tr>
      <th>1</th>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>보통</td>
      <td>보통</td>
      <td>보통</td>
    </tr>
    <tr>
      <th>2</th>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>보통</td>
      <td>보통</td>
    </tr>
    <tr>
      <th>3</th>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>보통</td>
      <td>보통</td>
      <td>보통</td>
    </tr>
    <tr>
      <th>4</th>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>보통</td>
      <td>보통</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>32860</th>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
    </tr>
    <tr>
      <th>32861</th>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>보통</td>
    </tr>
    <tr>
      <th>32862</th>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>보통</td>
      <td>보통</td>
    </tr>
    <tr>
      <th>32863</th>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>보통</td>
    </tr>
    <tr>
      <th>32864</th>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>보통</td>
    </tr>
  </tbody>
</table>
<p>32865 rows × 6 columns</p>
</div>




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
      <th>판매일시</th>
      <th>파종일시</th>
      <th>도_num</th>
      <th>연도</th>
      <th>SO2</th>
      <th>CO</th>
      <th>O3</th>
      <th>NO2</th>
      <th>PM10</th>
      <th>PM25</th>
      <th>...</th>
      <th>품종명</th>
      <th>도</th>
      <th>마켓명</th>
      <th>가격</th>
      <th>SO2_condition</th>
      <th>CO_condition</th>
      <th>O3_condition</th>
      <th>NO2_condition</th>
      <th>PM10_condition</th>
      <th>PM25_condition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2013-05-01</td>
      <td>2013-01-01</td>
      <td>0</td>
      <td>2013</td>
      <td>0.007216</td>
      <td>0.770076</td>
      <td>0.020583</td>
      <td>0.018420</td>
      <td>45.132576</td>
      <td>31.543635</td>
      <td>...</td>
      <td>토마토(10kg)</td>
      <td>강원도</td>
      <td>가락도매</td>
      <td>-1.0</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>보통</td>
      <td>보통</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2013-05-01</td>
      <td>2013-01-01</td>
      <td>1</td>
      <td>2013</td>
      <td>0.006340</td>
      <td>0.649199</td>
      <td>0.013143</td>
      <td>0.031923</td>
      <td>44.532051</td>
      <td>31.543635</td>
      <td>...</td>
      <td>토마토(10kg)</td>
      <td>경기도</td>
      <td>가락도매</td>
      <td>-1.0</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>보통</td>
      <td>보통</td>
      <td>보통</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2013-05-01</td>
      <td>2013-01-01</td>
      <td>2</td>
      <td>2013</td>
      <td>0.007477</td>
      <td>0.713492</td>
      <td>0.013913</td>
      <td>0.030154</td>
      <td>39.626984</td>
      <td>31.543635</td>
      <td>...</td>
      <td>토마토(10kg)</td>
      <td>인천광역시</td>
      <td>가락도매</td>
      <td>-1.0</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>보통</td>
      <td>보통</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2013-05-01</td>
      <td>2013-01-01</td>
      <td>3</td>
      <td>2013</td>
      <td>0.005741</td>
      <td>0.658125</td>
      <td>0.008458</td>
      <td>0.043541</td>
      <td>38.334375</td>
      <td>31.543635</td>
      <td>...</td>
      <td>토마토(10kg)</td>
      <td>서울특별시</td>
      <td>가락도매</td>
      <td>-1.0</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>보통</td>
      <td>보통</td>
      <td>보통</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2013-05-01</td>
      <td>2013-01-01</td>
      <td>4</td>
      <td>2013</td>
      <td>0.006363</td>
      <td>0.739583</td>
      <td>0.015653</td>
      <td>0.017980</td>
      <td>48.280093</td>
      <td>31.543635</td>
      <td>...</td>
      <td>토마토(10kg)</td>
      <td>경상북도</td>
      <td>북부도매</td>
      <td>-1.0</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>보통</td>
      <td>보통</td>
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
      <th>32860</th>
      <td>2019-04-30</td>
      <td>2018-12-31</td>
      <td>10</td>
      <td>2018</td>
      <td>0.003241</td>
      <td>0.502206</td>
      <td>0.018193</td>
      <td>0.023311</td>
      <td>22.787681</td>
      <td>12.066176</td>
      <td>...</td>
      <td>토마토(10kg)</td>
      <td>울산광역시</td>
      <td>엄궁도매</td>
      <td>22000.0</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
    </tr>
    <tr>
      <th>32861</th>
      <td>2019-04-30</td>
      <td>2018-12-31</td>
      <td>11</td>
      <td>2018</td>
      <td>0.003150</td>
      <td>0.475868</td>
      <td>0.017313</td>
      <td>0.022639</td>
      <td>29.648866</td>
      <td>19.518839</td>
      <td>...</td>
      <td>토마토(10kg)</td>
      <td>경상남도</td>
      <td>엄궁도매</td>
      <td>22000.0</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>보통</td>
    </tr>
    <tr>
      <th>32862</th>
      <td>2019-04-30</td>
      <td>2018-12-31</td>
      <td>12</td>
      <td>2018</td>
      <td>0.003204</td>
      <td>0.605093</td>
      <td>0.015468</td>
      <td>0.027125</td>
      <td>34.759259</td>
      <td>22.728704</td>
      <td>...</td>
      <td>토마토(10kg)</td>
      <td>광주광역시</td>
      <td>각화도매</td>
      <td>21000.0</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>보통</td>
      <td>보통</td>
    </tr>
    <tr>
      <th>32863</th>
      <td>2019-04-30</td>
      <td>2018-12-31</td>
      <td>13</td>
      <td>2018</td>
      <td>0.004119</td>
      <td>0.373418</td>
      <td>0.014718</td>
      <td>0.027532</td>
      <td>28.525591</td>
      <td>15.960829</td>
      <td>...</td>
      <td>토마토(10kg)</td>
      <td>부산광역시</td>
      <td>엄궁도매</td>
      <td>22000.0</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>보통</td>
    </tr>
    <tr>
      <th>32864</th>
      <td>2019-04-30</td>
      <td>2018-12-31</td>
      <td>14</td>
      <td>2018</td>
      <td>0.005988</td>
      <td>0.594565</td>
      <td>0.018808</td>
      <td>0.015774</td>
      <td>25.805264</td>
      <td>20.055719</td>
      <td>...</td>
      <td>토마토(10kg)</td>
      <td>전라남도</td>
      <td>각화도매</td>
      <td>22000.0</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>좋음</td>
      <td>보통</td>
    </tr>
  </tbody>
</table>
<p>32865 rows × 21 columns</p>
</div>




```python
df.isnull().sum
```




    <bound method DataFrame.sum of         판매일시   파종일시  도_num     연도    SO2     CO     O3    NO2   PM10   PM25  \
    0      False  False  False  False  False  False  False  False  False  False   
    1      False  False  False  False  False  False  False  False  False  False   
    2      False  False  False  False  False  False  False  False  False  False   
    3      False  False  False  False  False  False  False  False  False  False   
    4      False  False  False  False  False  False  False  False  False  False   
    ...      ...    ...    ...    ...    ...    ...    ...    ...    ...    ...   
    32860  False  False  False  False  False  False  False  False  False  False   
    32861  False  False  False  False  False  False  False  False  False  False   
    32862  False  False  False  False  False  False  False  False  False  False   
    32863  False  False  False  False  False  False  False  False  False  False   
    32864  False  False  False  False  False  False  False  False  False  False   
    
           ...  O3_round  NO2_round  PM10_round  PM25_round  SO2_condition  \
    0      ...     False      False       False       False          False   
    1      ...     False      False       False       False          False   
    2      ...     False      False       False       False          False   
    3      ...     False      False       False       False          False   
    4      ...     False      False       False       False          False   
    ...    ...       ...        ...         ...         ...            ...   
    32860  ...     False      False       False       False          False   
    32861  ...     False      False       False       False          False   
    32862  ...     False      False       False       False          False   
    32863  ...     False      False       False       False          False   
    32864  ...     False      False       False       False          False   
    
           CO_condition  O3_condition  NO2_condition  PM10_condition  \
    0             False         False          False           False   
    1             False         False          False           False   
    2             False         False          False           False   
    3             False         False          False           False   
    4             False         False          False           False   
    ...             ...           ...            ...             ...   
    32860         False         False          False           False   
    32861         False         False          False           False   
    32862         False         False          False           False   
    32863         False         False          False           False   
    32864         False         False          False           False   
    
           PM25_condition  
    0               False  
    1               False  
    2               False  
    3               False  
    4               False  
    ...               ...  
    32860           False  
    32861           False  
    32862           False  
    32863           False  
    32864           False  
    
    [32865 rows x 26 columns]>




```python
df.drop(['SO2_round','O3_round','NO2_round','PM10_round','PM25_round'],axis=1,inplace=True)
```
