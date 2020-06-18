---
title: "LOL 승패예측"
tags: [Python, TIL]
style:
color:
description: RIOT API를 활용한 데이터 분석
---
### 승패예측

변수에 대한 설명은 밑의 블로그를 참고해주세요 <br/>
참고:[me뇽](https://shinminyong.tistory.com/12?category=839096)  

A팀의 특성만으로도 이겼을 때와 졌을 때의 특성을 파악할 수 있기 때문에 team1의 데이터로만 분석 진행


```python
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.preprocessing import LabelEncoder
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score
from sklearn.metrics import f1_score
from sklearn.inspection import permutation_importance
```

#### 데이터 결합


```python
team1 = pd.read_csv("team1.csv",encoding='cp949',index_col=0)
match = pd.read_csv("match_data.csv",encoding='cp949')
```


```python
match = match[~match['gameDuration'].isnull()]
```


```python
#index는 drop한걸 반영안 하기 때문에 reset_index를 해줘야 concat할때 null값이 발생하지 않는다
match = match.reset_index(drop=True)
```


```python
data_team = pd.concat([team1,match['gameDuration']],axis=1)
```


```python
data_team = data_team.dropna(axis=0)
```


```python
#타겟 데이터인 win을 제외한 나머지 데이터
data_team2 = data_team[list(data_team.columns)[2:]].copy()
```

#### 레이블링


```python
#first로 시작하는 컬럼들은 T/F로 되어 있기 때문에 0,1로 인코딩을 해줘야 한다
for i in range(0,6):
    le = LabelEncoder()
    y = list(data_team2.iloc[:,i])
    
    le.fit(y)
    y2 = le.transform(y)
    
    data_team2.iloc[:,i] = y2
```


```python
#종속변수 라벨링
dict_winner2 = {"Win" : 0, "Fail": 1}
data_label = np.array(data_team['win'].map(dict_winner2).tolist())
```


```python
X_train,X_test,y_train,y_test = train_test_split(data_team2,data_label,test_size=0.25,stratify=data_label,random_state=123456)
```

#### 예측


```python
rf = RandomForestClassifier(n_estimators=1000,oob_score=True) #oob 점수는 oob샘플에서 올바르게 예측 된 행수로 계산됩니다
rf.fit(X_train,y_train)
```




    RandomForestClassifier(bootstrap=True, ccp_alpha=0.0, class_weight=None,
                           criterion='gini', max_depth=None, max_features='auto',
                           max_leaf_nodes=None, max_samples=None,
                           min_impurity_decrease=0.0, min_impurity_split=None,
                           min_samples_leaf=1, min_samples_split=2,
                           min_weight_fraction_leaf=0.0, n_estimators=1000,
                           n_jobs=None, oob_score=True, random_state=None,
                           verbose=0, warm_start=False)




```python
predicted = rf.predict(X_test)
accuracy = accuracy_score(y_test, predicted)
f1_score = f1_score(y_test, predicted)
```


```python
print(f'Out-of-bag score estimate: {rf.oob_score_:.3}')
print(f'Mean accuracy score: {accuracy:.3}')
print(f'f1_score: {f1_score:.3}')
```

    Out-of-bag score estimate: 0.966
    Mean accuracy score: 0.963
    f1_score: 0.963
    

상당히 높은 정확도를 보이는것을 알 수 있습니다


```python
model = RandomForestClassifier(n_estimators=1000,oob_score=True).fit(X_train,y_train)
```


```python
# feature importances와는 다른게 permuation feature importancce는 특정 feature를 사용하지 않을 때, 이것이 성능 손실에
# 얼마만큼의 영향을 주는지 파약하여 feature의 중요도를 판단합니다
```


```python
r = permutation_importance(model,X_test,y_test,n_repeats=30,random_state=42,n_jobs=-1)
```


```python
sorted_idx = r.importances_mean.argsort()
```


```python
fig, ax = plt.subplots(figsize=(15,10))
ax.boxplot(r.importances[sorted_idx].T,
          vert=False, labels=X_test.columns[sorted_idx])
ax.set_title("Permuation Importances (test set)")
```




    Text(0.5, 1.0, 'Permuation Importances (test set)')




![output_24_1](https://user-images.githubusercontent.com/57039464/85030463-726bfd00-b1b8-11ea-9366-23ee0b57d022.png)



towerkills와 gameDuration이 영향을 제일 많이 끼치는것을 알 수 있습니다
