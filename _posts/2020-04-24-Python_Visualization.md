---
title: "Python 시각화"
tags: [Python,TIL]
style: 
color:
description: Python을 활용한 프로젝트 시각화
---
현재 교육과정에서 진행중인 프로젝트의 시각화를 해봤습니다.<br/>
사용데이터: [Kaggle](https://www.kaggle.com/jessemostipak/hotel-booking-demand)   
```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np

from IPython.display import set_matplotlib_formats

set_matplotlib_formats('retina')
```


```python
plt.rc('font',family='Malgun Gothic')
```


```python
df_City = pd.read_csv('../data/City Hotel.csv')
df_Resort = pd.read_csv('../data/Resort Hotel.csv')
```


```python
length = lambda x: x == 1
length_2 = lambda x: x == 0
```


```python
df_City_1 = df_City[df_City['is_canceled'].apply(length)]
df_City_0 = df_City[df_City['is_canceled'].apply(length_2)]
df_Resort_1 = df_Resort[df_Resort['is_canceled'].apply(length)]
df_Resort_0 = df_Resort[df_Resort['is_canceled'].apply(length_2)]
```


```python
plt.figure(figsize=(15,8))
ax_1 = sns.distplot(df_City_0['lead_time'],label='is_canceled_0',hist=False)
ax_2 = sns.distplot(df_City_1['lead_time'],hist=False,label='is_canceled_1')
plt.axvline(x=71,color='red')
```




    <matplotlib.lines.Line2D at 0x1c1b1504f08>




<img width="883" alt="output_5_1" src="https://user-images.githubusercontent.com/57039464/80309958-1e294a00-8813-11ea-98f7-4a45079b04d7.png">



```python
plt.figure(figsize=(15,8))
sns.distplot(df_Resort_0['lead_time'],hist=False,label='is_canceled_0')
sns.distplot(df_Resort_1['lead_time'],hist=False,label='is_canceled_1')
plt.axvline(x=39,color='red')
```




    <matplotlib.lines.Line2D at 0x1c1b10b4248>




<img width="883" alt="output_6_1" src="https://user-images.githubusercontent.com/57039464/80309962-1f5a7700-8813-11ea-95d3-44f0295e7145.png">



```python
sns.pairplot(df_Resort[['agent','days_in_waiting_list','adr','is_canceled']],hue='is_canceled',diag_kws={'bw':10})
```




    <seaborn.axisgrid.PairGrid at 0x2eeb8836208>




<img width="613" alt="output_7_1" src="https://user-images.githubusercontent.com/57039464/80309963-1ff30d80-8813-11ea-9f93-47afb1b3967f.png">



```python
sns.pairplot(df_City[['agent','days_in_waiting_list','adr','is_canceled']],hue='is_canceled',diag_kws={'bw':10})
```




    <seaborn.axisgrid.PairGrid at 0x2eeba7498c8>




<img width="619" alt="output_8_1" src="https://user-images.githubusercontent.com/57039464/80309964-208ba400-8813-11ea-910b-fd7e5a0b5f7b.png">



```python
parking_spaces_City = list(df_City['required_car_parking_spaces'].unique())
parking_spaces_Resort = list(df_Resort['required_car_parking_spaces'].unique())
```


```python
for i in parking_spaces_City:
    globals()['df_City_Park{}'.format(i)] = df_City[df_City['required_car_parking_spaces']==i]
    print(i)
```


```python
figure, axes = plt.subplots(nrows = 1, ncols = 4, figsize = (20,5))
figure.set_size_inches(15,5)
for i,j in enumerate(parking_spaces_City):
     sns.countplot(data= globals()['df_City_Park{}'.format(j)] ,x='required_car_parking_spaces',hue='is_canceled',ax=axes[i])
```


<img width="904" alt="output_11_0" src="https://user-images.githubusercontent.com/57039464/80309965-21243a80-8813-11ea-86ce-2c90ca59b81e.png">



```python
for i in parking_spaces_Resort:
    globals()['df_Resort_Park{}'.format(i)] = df_Resort[df_Resort['required_car_parking_spaces']==i]
    print(i)
```

    0
    1
    2
    8
    3
    


```python
figure, axes = plt.subplots(nrows = 1, ncols = 5, figsize = (20,5))
figure.set_size_inches(15,5)
for i,j in enumerate(parking_spaces_Resort):
     sns.countplot(data= globals()['df_Resort_Park{}'.format(j)] ,x='required_car_parking_spaces',hue='is_canceled',ax=axes[i])
```


<img width="905" alt="output_13_0" src="https://user-images.githubusercontent.com/57039464/80309967-21243a80-8813-11ea-90c7-d0ccbdb876cc.png">


```python
plt.figure(figsize=(10,5))
ax = sns.countplot(data=df_Resort, x= 'total_of_special_requests',hue='is_canceled',palette=(sns.color_palette("Set1", n_colors=8, desat=.5)))
for p in ax.patches:
    left, bottom, width, height = p.get_bbox().bounds
    ax.annotate("%.2f"%(height/df_Resort.shape[0]),(left+width/1.9, height*1.015), ha='center',size=11.5)

```


<img width="621" alt="output_14_0" src="https://user-images.githubusercontent.com/57039464/80309968-21bcd100-8813-11ea-83bd-979138756bec.png">



```python
plt.figure(figsize=(10,5))
ax = sns.countplot(data=df_City,x= 'total_of_special_requests',hue='is_canceled',palette=(sns.color_palette("Set1", n_colors=8, desat=.5)))
for p in ax.patches:
    left, bottom, width, height = p.get_bbox().bounds
    ax.annotate("%.2f"%(height/df_City.shape[0]),(left+width/1.9, height*1.011), ha='center',size=11.5)
```


<img width="621" alt="output_15_0" src="https://user-images.githubusercontent.com/57039464/80309970-22556780-8813-11ea-92a5-d62c1b2dc5ac.png">


