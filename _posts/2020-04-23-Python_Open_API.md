---
title: "아파트 실거래가 Open_API"
tags: [Python, TIL]
style:
color:
description: Python을 활용한 아파트 실거래가 Open API
---
Open_API에서 원하는 정보를 가져와서 데이터프레임화 하는 과정을 진행해봤습니다.<br/>
데이터_소스:[공공데이터 포털](https://www.data.go.kr/tcs/dss/selectApiDataDetailView.do?publicDataPk=15057511)      
위 사이트에서 활용신청을 하여 service_key를 받아야 진행 가능합니다.

### 아파트 실거래가 Open_API

```python
import requests
import pandas as pd
import numpy as np
from bs4 import BeautifulSoup
from urllib.request import urlopen
```


```python
def apt_info(ym,lawd_cd):
    API_KEY = '본인이 받은 서비스키 '
    url = 'http://openapi.molit.go.kr/OpenAPI_ToolInstallPackage/service/rest/RTMSOBJSvc/getRTMSDataSvcAptTradeDev'
    url=url+"?&serviceKey="+API_KEY+"&pageNo=1"+"&numOfRows=1000"+"&LAWD_CD="+lawd_cd+"&DEAL_YMD="+ym

 
    # webbrowser.open(url)

    resultXML = urlopen(url)

    result = resultXML.read()

    xmlsoup = BeautifulSoup(result,'lxml-xml')

    te = xmlsoup.findAll('item')

    sil = pd.DataFrame()
    count = 0
    #te를 돌면서 필요한 부분을 돌면서 필요한 정보를 추출합니다.
    for t in te:
        deal_price = t.find("거래금액").text
        build_y = t.find("건축년도").text
        dong = t.find("법정동").text
        apt_nm = t.find("아파트").text
        size = t.find("전용면적").text
        # jibun이 없는 경우도 있기 때문에 있으면 text를 가져오고 없으면 공백을 넣습니다.
        try:
            jibun=t.find("지번").text
        except:
            jibun=""
 
        floor=t.find("층").text
        #for문 안에서 임시적으로 정보를 담는 temp라는 데이터 프레임을 생성하고
        temp = pd.DataFrame(([[build_y,dong,apt_nm,deal_price,size,jibun,floor]]), columns=["건축년도","법정동","아파트이름","거래금액","면적","지번","층수"])
        #sill과 temp를 합쳐 temp에 있던 정보들을 sil에 저장한다고 생각하면 됩니다.
        sil=pd.concat([sil,temp])
        
    sil=sil.reset_index(drop=True)
    
    return sil
    
```


```python
df = apt_info('201909','11680')
```


```python
df['거래금액'] = df['거래금액'].str.replace(',','')
```


```python
df['거래금액'] = pd.to_numeric(df['거래금액'])
df['면적'] = pd.to_numeric(df['면적'])
```


```python
df1 = pd.DataFrame()
```


```python
a = df[['아파트이름','거래금액']].groupby('아파트이름').mean()
```


```python
b = df[['법정동','아파트이름','거래금액','면적']].groupby(['법정동','아파트이름'],as_index=False).sum()
```


```python
df1 = pd.concat([df1,a])
```


```python
df1.rename(columns={'거래금액':'평균거래금액'},inplace=True)
```


```python
df_final = pd.merge(b,df1,on='아파트이름',how='inner')
```


```python
df_final
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
      <th>법정동</th>
      <th>아파트이름</th>
      <th>거래금액</th>
      <th>면적</th>
      <th>평균거래금액</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>개포동</td>
      <td>개포주공 1단지</td>
      <td>2114000</td>
      <td>512.5100</td>
      <td>211400</td>
    </tr>
    <tr>
      <th>1</th>
      <td>개포동</td>
      <td>개포주공 3단지</td>
      <td>165000</td>
      <td>35.6400</td>
      <td>165000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>개포동</td>
      <td>개포주공 5단지</td>
      <td>195000</td>
      <td>83.1700</td>
      <td>195000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>개포동</td>
      <td>개포주공 6단지</td>
      <td>492600</td>
      <td>193.2800</td>
      <td>164200</td>
    </tr>
    <tr>
      <th>4</th>
      <td>개포동</td>
      <td>경남2차</td>
      <td>257000</td>
      <td>182.2000</td>
      <td>257000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>개포동</td>
      <td>래미안블레스티지</td>
      <td>487000</td>
      <td>198.9100</td>
      <td>243500</td>
    </tr>
    <tr>
      <th>6</th>
      <td>개포동</td>
      <td>삼익대청아파트</td>
      <td>222000</td>
      <td>99.5300</td>
      <td>111000</td>
    </tr>
    <tr>
      <th>7</th>
      <td>개포동</td>
      <td>새롬(1164-13)</td>
      <td>58000</td>
      <td>59.6700</td>
      <td>58000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>개포동</td>
      <td>성원대치2단지아파트</td>
      <td>351900</td>
      <td>132.7200</td>
      <td>87975</td>
    </tr>
    <tr>
      <th>9</th>
      <td>논현동</td>
      <td>마일스디오빌</td>
      <td>38700</td>
      <td>36.2900</td>
      <td>38700</td>
    </tr>
    <tr>
      <th>10</th>
      <td>논현동</td>
      <td>쌍용</td>
      <td>77000</td>
      <td>59.9100</td>
      <td>77000</td>
    </tr>
    <tr>
      <th>11</th>
      <td>논현동</td>
      <td>이산</td>
      <td>35000</td>
      <td>62.1000</td>
      <td>35000</td>
    </tr>
    <tr>
      <th>12</th>
      <td>대치동</td>
      <td>대치아이파크</td>
      <td>266000</td>
      <td>114.9700</td>
      <td>266000</td>
    </tr>
    <tr>
      <th>13</th>
      <td>대치동</td>
      <td>대치현대</td>
      <td>132000</td>
      <td>59.8200</td>
      <td>132000</td>
    </tr>
    <tr>
      <th>14</th>
      <td>대치동</td>
      <td>동양</td>
      <td>83500</td>
      <td>98.7300</td>
      <td>83500</td>
    </tr>
    <tr>
      <th>15</th>
      <td>대치동</td>
      <td>래미안대치팰리스</td>
      <td>279800</td>
      <td>84.9700</td>
      <td>279800</td>
    </tr>
    <tr>
      <th>16</th>
      <td>대치동</td>
      <td>아름빌(889-74)</td>
      <td>29000</td>
      <td>30.4600</td>
      <td>29000</td>
    </tr>
    <tr>
      <th>17</th>
      <td>대치동</td>
      <td>은마</td>
      <td>178000</td>
      <td>76.7900</td>
      <td>178000</td>
    </tr>
    <tr>
      <th>18</th>
      <td>대치동</td>
      <td>한티(933-0)</td>
      <td>101500</td>
      <td>116.2100</td>
      <td>101500</td>
    </tr>
    <tr>
      <th>19</th>
      <td>도곡동</td>
      <td>도곡쌍용예가</td>
      <td>185500</td>
      <td>112.1300</td>
      <td>185500</td>
    </tr>
    <tr>
      <th>20</th>
      <td>도곡동</td>
      <td>우성4</td>
      <td>269000</td>
      <td>152.7400</td>
      <td>269000</td>
    </tr>
    <tr>
      <th>21</th>
      <td>도곡동</td>
      <td>우성캐릭터199</td>
      <td>327500</td>
      <td>297.9100</td>
      <td>163750</td>
    </tr>
    <tr>
      <th>22</th>
      <td>도곡동</td>
      <td>타워팰리스2</td>
      <td>239000</td>
      <td>160.1700</td>
      <td>239000</td>
    </tr>
    <tr>
      <th>23</th>
      <td>도곡동</td>
      <td>타워팰리스3</td>
      <td>260000</td>
      <td>163.5670</td>
      <td>260000</td>
    </tr>
    <tr>
      <th>24</th>
      <td>도곡동</td>
      <td>한신엠비씨</td>
      <td>136500</td>
      <td>84.7400</td>
      <td>136500</td>
    </tr>
    <tr>
      <th>25</th>
      <td>삼성동</td>
      <td>LG선릉에클라트(A)</td>
      <td>34000</td>
      <td>36.1600</td>
      <td>34000</td>
    </tr>
    <tr>
      <th>26</th>
      <td>삼성동</td>
      <td>롯데캐슬프레미어</td>
      <td>514000</td>
      <td>315.2450</td>
      <td>257000</td>
    </tr>
    <tr>
      <th>27</th>
      <td>삼성동</td>
      <td>미켈란107</td>
      <td>133000</td>
      <td>83.0000</td>
      <td>133000</td>
    </tr>
    <tr>
      <th>28</th>
      <td>삼성동</td>
      <td>미켈란147</td>
      <td>139000</td>
      <td>83.6900</td>
      <td>139000</td>
    </tr>
    <tr>
      <th>29</th>
      <td>삼성동</td>
      <td>삼성동롯데아파트</td>
      <td>130500</td>
      <td>59.4000</td>
      <td>130500</td>
    </tr>
    <tr>
      <th>30</th>
      <td>삼성동</td>
      <td>삼성동힐스테이트 1단지</td>
      <td>309000</td>
      <td>115.6380</td>
      <td>154500</td>
    </tr>
    <tr>
      <th>31</th>
      <td>삼성동</td>
      <td>삼성파크</td>
      <td>153000</td>
      <td>107.3800</td>
      <td>153000</td>
    </tr>
    <tr>
      <th>32</th>
      <td>삼성동</td>
      <td>포스코트</td>
      <td>180000</td>
      <td>154.2400</td>
      <td>180000</td>
    </tr>
    <tr>
      <th>33</th>
      <td>세곡동</td>
      <td>강남엘에이치1단지</td>
      <td>292800</td>
      <td>194.5200</td>
      <td>97600</td>
    </tr>
    <tr>
      <th>34</th>
      <td>수서동</td>
      <td>강남데시앙포레</td>
      <td>165000</td>
      <td>101.8100</td>
      <td>165000</td>
    </tr>
    <tr>
      <th>35</th>
      <td>수서동</td>
      <td>까치마을</td>
      <td>136000</td>
      <td>68.8800</td>
      <td>68000</td>
    </tr>
    <tr>
      <th>36</th>
      <td>신사동</td>
      <td>로데오현대</td>
      <td>159000</td>
      <td>110.1600</td>
      <td>79500</td>
    </tr>
    <tr>
      <th>37</th>
      <td>압구정동</td>
      <td>한양6</td>
      <td>230000</td>
      <td>106.7100</td>
      <td>230000</td>
    </tr>
    <tr>
      <th>38</th>
      <td>압구정동</td>
      <td>현대14차(203,204,205,206동)</td>
      <td>250000</td>
      <td>84.9800</td>
      <td>250000</td>
    </tr>
    <tr>
      <th>39</th>
      <td>압구정동</td>
      <td>현대7차(73~77,82,85동)</td>
      <td>349000</td>
      <td>157.3600</td>
      <td>349000</td>
    </tr>
    <tr>
      <th>40</th>
      <td>압구정동</td>
      <td>현대8차(성수현대:91~95동)</td>
      <td>235000</td>
      <td>111.5000</td>
      <td>235000</td>
    </tr>
    <tr>
      <th>41</th>
      <td>역삼동</td>
      <td>e-편한세상</td>
      <td>362000</td>
      <td>144.5970</td>
      <td>181000</td>
    </tr>
    <tr>
      <th>42</th>
      <td>역삼동</td>
      <td>강남서해더블루</td>
      <td>91000</td>
      <td>84.8000</td>
      <td>91000</td>
    </tr>
    <tr>
      <th>43</th>
      <td>역삼동</td>
      <td>강남역우정에쉐르</td>
      <td>19800</td>
      <td>17.1300</td>
      <td>19800</td>
    </tr>
    <tr>
      <th>44</th>
      <td>역삼동</td>
      <td>개나리래미안</td>
      <td>198000</td>
      <td>129.8000</td>
      <td>198000</td>
    </tr>
    <tr>
      <th>45</th>
      <td>역삼동</td>
      <td>금호어울림</td>
      <td>264800</td>
      <td>217.8300</td>
      <td>132400</td>
    </tr>
    <tr>
      <th>46</th>
      <td>역삼동</td>
      <td>대우디오빌</td>
      <td>63000</td>
      <td>59.5050</td>
      <td>63000</td>
    </tr>
    <tr>
      <th>47</th>
      <td>역삼동</td>
      <td>로얄빌리지</td>
      <td>72000</td>
      <td>80.6400</td>
      <td>72000</td>
    </tr>
    <tr>
      <th>48</th>
      <td>역삼동</td>
      <td>역삼디오슈페리움</td>
      <td>61000</td>
      <td>46.9000</td>
      <td>61000</td>
    </tr>
    <tr>
      <th>49</th>
      <td>역삼동</td>
      <td>역삼푸르지오</td>
      <td>155000</td>
      <td>59.8848</td>
      <td>155000</td>
    </tr>
    <tr>
      <th>50</th>
      <td>역삼동</td>
      <td>탑팰리스</td>
      <td>85000</td>
      <td>84.9700</td>
      <td>85000</td>
    </tr>
    <tr>
      <th>51</th>
      <td>역삼동</td>
      <td>한화진넥스빌</td>
      <td>35000</td>
      <td>39.2000</td>
      <td>35000</td>
    </tr>
    <tr>
      <th>52</th>
      <td>일원동</td>
      <td>래미안 개포 루체하임</td>
      <td>390000</td>
      <td>161.9600</td>
      <td>195000</td>
    </tr>
    <tr>
      <th>53</th>
      <td>자곡동</td>
      <td>래미안강남힐즈</td>
      <td>147000</td>
      <td>101.9400</td>
      <td>147000</td>
    </tr>
    <tr>
      <th>54</th>
      <td>자곡동</td>
      <td>래미안포레</td>
      <td>136000</td>
      <td>101.5100</td>
      <td>136000</td>
    </tr>
    <tr>
      <th>55</th>
      <td>청담동</td>
      <td>청담스위트</td>
      <td>24000</td>
      <td>13.4250</td>
      <td>24000</td>
    </tr>
    <tr>
      <th>56</th>
      <td>청담동</td>
      <td>청담자이</td>
      <td>172000</td>
      <td>49.6390</td>
      <td>172000</td>
    </tr>
    <tr>
      <th>57</th>
      <td>청담동</td>
      <td>휴먼스타빌</td>
      <td>57000</td>
      <td>35.9050</td>
      <td>57000</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_final['단위금액'] = df_final['금액합계'] / df_final['면적합계']
```


```python
df_final = df_final.rename(columns={'거래금액':'금액합계','면적':'면적합계'})
```


```python
df_final
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
      <th>법정동</th>
      <th>아파트이름</th>
      <th>금액합계</th>
      <th>면적합계</th>
      <th>평균거래금액</th>
      <th>단위금액</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>개포동</td>
      <td>개포주공 1단지</td>
      <td>1668000</td>
      <td>412.3100</td>
      <td>208500</td>
      <td>4045.499745</td>
    </tr>
    <tr>
      <th>1</th>
      <td>개포동</td>
      <td>개포주공 3단지</td>
      <td>165000</td>
      <td>35.6400</td>
      <td>165000</td>
      <td>4629.629630</td>
    </tr>
    <tr>
      <th>2</th>
      <td>개포동</td>
      <td>개포주공 5단지</td>
      <td>195000</td>
      <td>83.1700</td>
      <td>195000</td>
      <td>2344.595407</td>
    </tr>
    <tr>
      <th>3</th>
      <td>개포동</td>
      <td>개포주공 6단지</td>
      <td>337300</td>
      <td>133.1500</td>
      <td>168650</td>
      <td>2533.233196</td>
    </tr>
    <tr>
      <th>4</th>
      <td>개포동</td>
      <td>경남2차</td>
      <td>257000</td>
      <td>182.2000</td>
      <td>257000</td>
      <td>1410.537870</td>
    </tr>
    <tr>
      <th>5</th>
      <td>개포동</td>
      <td>래미안블레스티지</td>
      <td>487000</td>
      <td>198.9100</td>
      <td>243500</td>
      <td>2448.343472</td>
    </tr>
    <tr>
      <th>6</th>
      <td>개포동</td>
      <td>삼익대청아파트</td>
      <td>222000</td>
      <td>99.5300</td>
      <td>111000</td>
      <td>2230.483271</td>
    </tr>
    <tr>
      <th>7</th>
      <td>개포동</td>
      <td>새롬(1164-13)</td>
      <td>58000</td>
      <td>59.6700</td>
      <td>58000</td>
      <td>972.012737</td>
    </tr>
    <tr>
      <th>8</th>
      <td>개포동</td>
      <td>성원대치2단지아파트</td>
      <td>351900</td>
      <td>132.7200</td>
      <td>87975</td>
      <td>2651.446655</td>
    </tr>
    <tr>
      <th>9</th>
      <td>논현동</td>
      <td>쌍용</td>
      <td>77000</td>
      <td>59.9100</td>
      <td>77000</td>
      <td>1285.261225</td>
    </tr>
    <tr>
      <th>10</th>
      <td>논현동</td>
      <td>이산</td>
      <td>35000</td>
      <td>62.1000</td>
      <td>35000</td>
      <td>563.607085</td>
    </tr>
    <tr>
      <th>11</th>
      <td>대치동</td>
      <td>대치아이파크</td>
      <td>266000</td>
      <td>114.9700</td>
      <td>266000</td>
      <td>2313.647038</td>
    </tr>
    <tr>
      <th>12</th>
      <td>대치동</td>
      <td>대치현대</td>
      <td>132000</td>
      <td>59.8200</td>
      <td>132000</td>
      <td>2206.619860</td>
    </tr>
    <tr>
      <th>13</th>
      <td>대치동</td>
      <td>동양</td>
      <td>83500</td>
      <td>98.7300</td>
      <td>83500</td>
      <td>845.740910</td>
    </tr>
    <tr>
      <th>14</th>
      <td>대치동</td>
      <td>아름빌(889-74)</td>
      <td>29000</td>
      <td>30.4600</td>
      <td>29000</td>
      <td>952.068286</td>
    </tr>
    <tr>
      <th>15</th>
      <td>대치동</td>
      <td>은마</td>
      <td>178000</td>
      <td>76.7900</td>
      <td>178000</td>
      <td>2318.010158</td>
    </tr>
    <tr>
      <th>16</th>
      <td>대치동</td>
      <td>한티(933-0)</td>
      <td>101500</td>
      <td>116.2100</td>
      <td>101500</td>
      <td>873.418811</td>
    </tr>
    <tr>
      <th>17</th>
      <td>도곡동</td>
      <td>도곡쌍용예가</td>
      <td>185500</td>
      <td>112.1300</td>
      <td>185500</td>
      <td>1654.329796</td>
    </tr>
    <tr>
      <th>18</th>
      <td>도곡동</td>
      <td>우성4</td>
      <td>269000</td>
      <td>152.7400</td>
      <td>269000</td>
      <td>1761.162760</td>
    </tr>
    <tr>
      <th>19</th>
      <td>도곡동</td>
      <td>우성캐릭터199</td>
      <td>327500</td>
      <td>297.9100</td>
      <td>163750</td>
      <td>1099.325300</td>
    </tr>
    <tr>
      <th>20</th>
      <td>도곡동</td>
      <td>타워팰리스2</td>
      <td>239000</td>
      <td>160.1700</td>
      <td>239000</td>
      <td>1492.164575</td>
    </tr>
    <tr>
      <th>21</th>
      <td>도곡동</td>
      <td>타워팰리스3</td>
      <td>260000</td>
      <td>163.5670</td>
      <td>260000</td>
      <td>1589.562687</td>
    </tr>
    <tr>
      <th>22</th>
      <td>도곡동</td>
      <td>한신엠비씨</td>
      <td>136500</td>
      <td>84.7400</td>
      <td>136500</td>
      <td>1610.809535</td>
    </tr>
    <tr>
      <th>23</th>
      <td>삼성동</td>
      <td>LG선릉에클라트(A)</td>
      <td>34000</td>
      <td>36.1600</td>
      <td>34000</td>
      <td>940.265487</td>
    </tr>
    <tr>
      <th>24</th>
      <td>삼성동</td>
      <td>롯데캐슬프레미어</td>
      <td>514000</td>
      <td>315.2450</td>
      <td>257000</td>
      <td>1630.477882</td>
    </tr>
    <tr>
      <th>25</th>
      <td>삼성동</td>
      <td>미켈란107</td>
      <td>133000</td>
      <td>83.0000</td>
      <td>133000</td>
      <td>1602.409639</td>
    </tr>
    <tr>
      <th>26</th>
      <td>삼성동</td>
      <td>미켈란147</td>
      <td>139000</td>
      <td>83.6900</td>
      <td>139000</td>
      <td>1660.891385</td>
    </tr>
    <tr>
      <th>27</th>
      <td>삼성동</td>
      <td>삼성동롯데아파트</td>
      <td>130500</td>
      <td>59.4000</td>
      <td>130500</td>
      <td>2196.969697</td>
    </tr>
    <tr>
      <th>28</th>
      <td>삼성동</td>
      <td>삼성동힐스테이트 1단지</td>
      <td>309000</td>
      <td>115.6380</td>
      <td>154500</td>
      <td>2672.131998</td>
    </tr>
    <tr>
      <th>29</th>
      <td>삼성동</td>
      <td>삼성파크</td>
      <td>153000</td>
      <td>107.3800</td>
      <td>153000</td>
      <td>1424.846340</td>
    </tr>
    <tr>
      <th>30</th>
      <td>삼성동</td>
      <td>포스코트</td>
      <td>180000</td>
      <td>154.2400</td>
      <td>180000</td>
      <td>1167.012448</td>
    </tr>
    <tr>
      <th>31</th>
      <td>세곡동</td>
      <td>강남엘에이치1단지</td>
      <td>196000</td>
      <td>134.6300</td>
      <td>98000</td>
      <td>1455.841937</td>
    </tr>
    <tr>
      <th>32</th>
      <td>수서동</td>
      <td>강남데시앙포레</td>
      <td>165000</td>
      <td>101.8100</td>
      <td>165000</td>
      <td>1620.665946</td>
    </tr>
    <tr>
      <th>33</th>
      <td>수서동</td>
      <td>까치마을</td>
      <td>136000</td>
      <td>68.8800</td>
      <td>68000</td>
      <td>1974.448316</td>
    </tr>
    <tr>
      <th>34</th>
      <td>신사동</td>
      <td>로데오현대</td>
      <td>159000</td>
      <td>110.1600</td>
      <td>79500</td>
      <td>1443.355120</td>
    </tr>
    <tr>
      <th>35</th>
      <td>압구정동</td>
      <td>현대7차(73~77,82,85동)</td>
      <td>349000</td>
      <td>157.3600</td>
      <td>349000</td>
      <td>2217.844433</td>
    </tr>
    <tr>
      <th>36</th>
      <td>역삼동</td>
      <td>e-편한세상</td>
      <td>362000</td>
      <td>144.5970</td>
      <td>181000</td>
      <td>2503.509755</td>
    </tr>
    <tr>
      <th>37</th>
      <td>역삼동</td>
      <td>강남역우정에쉐르</td>
      <td>19800</td>
      <td>17.1300</td>
      <td>19800</td>
      <td>1155.866900</td>
    </tr>
    <tr>
      <th>38</th>
      <td>역삼동</td>
      <td>개나리래미안</td>
      <td>198000</td>
      <td>129.8000</td>
      <td>198000</td>
      <td>1525.423729</td>
    </tr>
    <tr>
      <th>39</th>
      <td>역삼동</td>
      <td>금호어울림</td>
      <td>264800</td>
      <td>217.8300</td>
      <td>132400</td>
      <td>1215.626865</td>
    </tr>
    <tr>
      <th>40</th>
      <td>역삼동</td>
      <td>대우디오빌</td>
      <td>63000</td>
      <td>59.5050</td>
      <td>63000</td>
      <td>1058.734560</td>
    </tr>
    <tr>
      <th>41</th>
      <td>역삼동</td>
      <td>로얄빌리지</td>
      <td>72000</td>
      <td>80.6400</td>
      <td>72000</td>
      <td>892.857143</td>
    </tr>
    <tr>
      <th>42</th>
      <td>역삼동</td>
      <td>역삼푸르지오</td>
      <td>155000</td>
      <td>59.8848</td>
      <td>155000</td>
      <td>2588.302875</td>
    </tr>
    <tr>
      <th>43</th>
      <td>역삼동</td>
      <td>탑팰리스</td>
      <td>85000</td>
      <td>84.9700</td>
      <td>85000</td>
      <td>1000.353066</td>
    </tr>
    <tr>
      <th>44</th>
      <td>역삼동</td>
      <td>한화진넥스빌</td>
      <td>35000</td>
      <td>39.2000</td>
      <td>35000</td>
      <td>892.857143</td>
    </tr>
    <tr>
      <th>45</th>
      <td>일원동</td>
      <td>래미안 개포 루체하임</td>
      <td>390000</td>
      <td>161.9600</td>
      <td>195000</td>
      <td>2408.001976</td>
    </tr>
    <tr>
      <th>46</th>
      <td>자곡동</td>
      <td>래미안강남힐즈</td>
      <td>147000</td>
      <td>101.9400</td>
      <td>147000</td>
      <td>1442.024720</td>
    </tr>
    <tr>
      <th>47</th>
      <td>자곡동</td>
      <td>래미안포레</td>
      <td>136000</td>
      <td>101.5100</td>
      <td>136000</td>
      <td>1339.769481</td>
    </tr>
    <tr>
      <th>48</th>
      <td>청담동</td>
      <td>청담자이</td>
      <td>172000</td>
      <td>49.6390</td>
      <td>172000</td>
      <td>3465.017426</td>
    </tr>
  </tbody>
</table>
</div>


