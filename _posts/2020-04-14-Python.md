---
title: "이탈률 예측(Churn_Prediction) EDA"
tags:  [Python,TIL]
style:
color:
description: Churn prediction EDA using Python
---
참고: [Kaggle](https://www.kaggle.com/blastchar/telco-customer-churn) <br/>
Kaggle에 있는 Churn_Prediction EDA를 진행해 봤습니다. <br/>
### Exploratory Data Analysis <br/>
#### Library Import
```python
import pandas as pd   
import numpy as np    
import matplotlib.pyplot as plt 
import seaborn as sns
import time
import warnings
#warning 무시
warnings.filterwarnings("ignore")

sns.set()
#구 버전은 사용
%matplotlib inline
#글씨를 더 명확하게
%config InlineBackend.figure_formats = ['retina']
#한글 폰트 설정
plt.rc('font',family='Malgun Gothic')
```

#### Data 확인
```python
# 파일을 읽어와 df에 담습니다.
df = pd.read_csv('../data/telco-customer-churn.csv')
df.head()

# 데이터의 갯수를 봅니다.
df.shape

# 컬럼만 출력해 봅니다.
df.columns

# 전체 데이터의 결측치 수를 봅니다.
df.isnull().sum()

# 전체 데이터프레임의 unique 데이터수를 nunique()를 통해 세어봅니다.
df.nunique()

# info를 봅니다.
df.info()
```

#### Missing & Duplicated Value
```python
# 결측치가 있다면 dropna 를 통해 제거합니다. how='all' 옵션을 사용합니다.
# 중복데이터가 있다면 제거합니다.
print(df.shape)
df = df.dropna(how='all') # remove samples with null fields
df = df[~df.duplicated()] # remove duplicates
print(df.shape)

# TotalCharges 의 타입을 봅니다.
#한글이나 공백이 있으면 이 방식으로 데이터를 불러 올 수 없음
df.TotalCharges
df.loc[df.TotalCharges == " ", "TotalCharges"].value_counts()
df['TotalCdropnacharges'] = df["TotalCharges"].replace(" ",np.nan) # 공백으로 되어 있는 요금을 결측치로 변경합니다. 
df = df.dropna(how = 'any')
df['TotalCharges'] = df['TotalCharges'].astype(float)

df.info()
```
#### 수치형을 범주형으로 변환
```python
# replace values for SeniorCitizen as a categorical feature
# SeniorCitizen 의 값이 1일 때 "Yes"로 0일 때 "No" 로 변경합니다.
df['SeniorCitizen'] = df['SeniorCitizen'].apply(lambda x: "Yes" if x == 1 else "No")

num_cols = ['tenure', 'MonthlyCharges', 'TotalCharges']
df[num_cols].describe()
```
#### 시각화
```python
sns.countplot(data=df,x="Churn")
```
![churn](https://user-images.githubusercontent.com/57039464/79299476-1d450e00-7f1f-11ea-801c-f8d60d1e4d2b.png)

```python
# 'tenure', 'MonthlyCharges', 'TotalCharges' 를 이탈률에 따라 pairplot 으로 그립니다.
sns.pairplot(df[['tenure', 'MonthlyCharges', 'TotalCharges' ,'Churn']],hue='Churn')
```
![pair](https://user-images.githubusercontent.com/57039464/79230482-a0268400-7e9f-11ea-8cca-f1cdc11a7230.png)

```python
# lmplot 으로 기간(tenure) 에 따른 MonthlyCharges 를 그리고 이탈률별로 나눠 그려봅니다.
sns.lmplot(data=df,x="tenure",y='MonthlyCharges',hue="Churn",col="Churn")
```
![tenure](https://user-images.githubusercontent.com/57039464/79299545-4b2a5280-7f1f-11ea-90a3-7a283d5667d0.png)

```python
# lmplot 으로 MonthlyCharges 에 따른 TotalCharges 를 그리고 이탈률별로 나눠 그려봅니다.
sns.lmplot(data=df,x='MonthlyCharges',y='TotalCharges',hue='Churn',col="Churn",palette="seismic")
```
![Monthly](https://user-images.githubusercontent.com/57039464/79299569-5c735f00-7f1f-11ea-9c06-9b21758b8c9d.png)

```python
# 수치변수 간의 상관관계를 그려봅니다. 이때 cmap="seismic" 으로 색상을 설정합니다.
corr = df.corr()
sns.heatmap(corr, cmap="seismic",annot=True,vmax= 1, vmin=-1)
```
![corr](https://user-images.githubusercontent.com/57039464/79299585-6b5a1180-7f1f-11ea-94e6-61c3b67e17ec.png)

```python
# 수치데이터를 histogram 으로 그려봅니다.
h = df.hist(figsize=(15, 5))
```
![hist](https://user-images.githubusercontent.com/57039464/79299601-76ad3d00-7f1f-11ea-84b5-2526da8f0d3e.png)
