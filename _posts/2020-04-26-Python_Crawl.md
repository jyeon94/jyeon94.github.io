---
title: "크롤링을 이용한 따릉이 데이터 수집"
tags: [Python,TIL]
style: 
color:
description: Python을 활용한 따릉이 데이터 크롤링
---
손쉬운 방법으로 따릉이 데이터 수집이 가능합니다. <br/>
데이터: [따릉이](https://www.bikeseoul.com/app/station/moveStationSearchView.do?currentPageNo=1)   
```
from bs4 import BeautifulSoup as bs
import pandas as pd
import numpy as np
import time
import re
from selenium import webdriver
```


```
base_url= 'https://www.bikeseoul.com/app/station/moveStationSearchView.do?currentPageNo=1'
driver = webdriver.Chrome(executable_path='E:/chromedriver_win32_1/chromedriver.exe')
```


```
driver.get(base_url)
```


```
df_list = []
for i in range(1,312):
    time.sleep(2)
    base_url = 'https://www.bikeseoul.com/app/station/moveStationSearchView.do?currentPageNo=%d'%i
    df_list.append(pd.read_html(base_url)[0])
```


```
df = pd.concat(df_list)
```


```
df['대여소 번호'] = df['대여소 이름'].str.split('.').str.get(0).str.replace(r'[ㄱ-ㅣ가-힣+]','')
```


```
df['대여소 이름'] = df['대여소 이름'].str.replace(r'[0-9+\.]','')
```


```
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
      <th>대여소 이름</th>
      <th>상태정보</th>
      <th>거치대수</th>
      <th>대여가능거치대수</th>
      <th>위치</th>
      <th>대여소 번호</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>광진교 남단 사거리(디지털프라자앞)</td>
      <td>운영중</td>
      <td>15</td>
      <td>0</td>
      <td>서울특별시 강동구 올림픽로 674 로석빌딩</td>
      <td>1001</td>
    </tr>
    <tr>
      <th>1</th>
      <td>해공공원(천호동)</td>
      <td>운영중</td>
      <td>10</td>
      <td>3</td>
      <td>서울특별시 강동구 올림픽로 702 해공도서관</td>
      <td>1002</td>
    </tr>
    <tr>
      <th>2</th>
      <td>해공도서관앞</td>
      <td>운영중</td>
      <td>20</td>
      <td>3</td>
      <td>서울특별시 강동구 올림픽로 702 해공도서관</td>
      <td>1003</td>
    </tr>
    <tr>
      <th>3</th>
      <td>삼성광나루아파트 버스정류장</td>
      <td>운영중</td>
      <td>10</td>
      <td>9</td>
      <td>서울특별시 강동구 올림픽로 812</td>
      <td>1004</td>
    </tr>
    <tr>
      <th>4</th>
      <td>롯데캐슬 동앞</td>
      <td>운영중</td>
      <td>15</td>
      <td>0</td>
      <td>서울특별시 강동구 고덕로 131 강동 롯데캐슬퍼스트아파트</td>
      <td>1006</td>
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
      <th>0</th>
      <td>서울혁신파크</td>
      <td>운영중</td>
      <td>13</td>
      <td>1</td>
      <td>서울특별시 은평구 통일로 684 서울혁신파크2</td>
      <td>967</td>
    </tr>
    <tr>
      <th>1</th>
      <td>은평뉴타운 상림마을 단지</td>
      <td>운영중</td>
      <td>7</td>
      <td>0</td>
      <td>서울특별시 은평구 진관4로 48-50 은평뉴타운 상림마을 13단지</td>
      <td>968</td>
    </tr>
    <tr>
      <th>2</th>
      <td>은평 지웰테라스</td>
      <td>운영중</td>
      <td>9</td>
      <td>4</td>
      <td>서울특별시 은평구 진관4로 78-38 은평 지웰테라스</td>
      <td>969</td>
    </tr>
    <tr>
      <th>3</th>
      <td>역촌 센트레빌 아파트</td>
      <td>운영중</td>
      <td>10</td>
      <td>0</td>
      <td>서울특별시 은평구 갈현로3나길 23 역촌 센트레빌 아파트 107동 인근</td>
      <td>971</td>
    </tr>
    <tr>
      <th>4</th>
      <td>수색역</td>
      <td>운영중</td>
      <td>20</td>
      <td>2</td>
      <td>서울특별시 은평구 수색로 261 수색역</td>
      <td>972</td>
    </tr>
  </tbody>
</table>
<p>1555 rows × 6 columns</p>
</div>


