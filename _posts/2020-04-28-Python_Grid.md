---
title: "Grid Search를 통한 Hyper Parameter Tuning"
tags: [Python,TIL]
style: 
color:
description: Python으로 Grid Search를 활용하여 Hyper Parameter Tuning
---
### Grid Search란?
![grid-search](https://user-images.githubusercontent.com/57039464/80916465-d31fb180-8d93-11ea-9d0c-9ccf7ac0f83e.png){: width="125" height="125"} <br/>
모델에 해당되는 파라미터를 조합하여 최적의 parameter를 찾는 방법입니다. <br/>
조합이 많을수록 좋은 성능을 가진 파라미터를 찾아낼 수 있지만 시간 소요가 오래걸립니다. <br/>
제가 밑에 작성해놓은 Gri Search도 3~5시간 정도 걸립니다. <br/>
```python
import pandas as pd
import numpy as np
from sklearn.model_selection import StratifiedKFold,train_test_split, GridSearchCV
from sklearn.metrics import f1_score
from sklearn.ensemble import RandomForestClassifier
```

필요한 데이터를 불러옵니다


```python
df_City = pd.read_csv('../data/City Hotel PCA.csv',index_col=0)
df_Resort = pd.read_csv('../data/Resort Hotel PCA.csv',index_col=0)
```

필요없는 컬럼을 드랍시키고 타겟 label을 분리하고 train_test_split을 활용하여 <br/>
train data와 test data로 나눕니다


```python
City_label = df_City['is_canceled']
Resort_label = df_Resort['is_canceled']
df_City = df_City.drop(['reservation_status_date','arrival_date','is_canceled'],axis=1)
df_Resort = df_Resort.drop(['reservation_status_date','arrival_date','is_canceled'],axis=1)

City_train,City_test,City_train_label,City_test_label = train_test_split(df_City,City_label,test_size=0.3,random_state=42)
Resort_train,Resort_test,Resort_train_label,Resort_test_label = train_test_split(df_Resort,Resort_label,test_size=0.3,random_state=42)
```

Grid Search를 이용해 Random Forest의 하이퍼 파라미터 튜닝을 해줍니다.


```python
# parameter를 설정해줍니다.
param_list = {"n_estimators":list(range(1700,2000,100)),
              "max_depth": [10,11,12],
              "max_features": [11,12,13],
              "min_samples_split": [3]}
```

StratifiedKFold를 통해 cv를 설정해줍니다


```python
rf_model = RandomForestClassifier(random_state=42)
skf = StratifiedKFold(n_splits=5)
```

GridSearch를 변수에 할당해줍니다


```python
rf_grid_search = GridSearchCV(
    estimator = rf_model,
    param_grid = param_list,
    cv = skf,
    scoring = 'f1',
    n_jobs = -1,
    verbose= 3
)
```


```python
#train data를 학습시킵니다
```


```python
rf_grid_search.fit(City_train,City_train_label)
```

scoring으로 설정해준 f1 score를 통해 가장 높은 점수를 표시해줍니다


```python
rf_grid_search.best_score 
```

가장 좋은 성능을 보여준 parameter를 확인해줍니다


```python
rf_grid_search.best_params_
```

위에서 나온 parameter를 통해 train data를 학습시켜 줍니다


```python
rf = RandomForestClassifier(max_depth= 10, max_features= 11,
 min_samples_split= 3, n_estimators = 1700)
```

test data을 통해 예측합니다


```python
prediction = rf.predict(City_test)
```

정답값을 통해 f1_score을 계산해줍니다


```python
f1_score(City_test_label,prediction)
```
