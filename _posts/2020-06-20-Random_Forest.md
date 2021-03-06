---
title: "Random Forest 개념"
tags: [Python,Machine Learning, Blog]
style:
color:
description: Random Forest 개념 간단 정리
---
참고:[옥수별](https://m.blog.naver.com/samsjang/220979751089)

### Random Forest

#### 사용 이유
- 의사결정트리는 학습 데이터에 따라 매우 달라지기 때문에 일반적으로 사용하기 어렵다.
- 위 같은 이유 때문에 학습 결과 역시 성능과 변동의 폭이 크다는 단점이 있다.
- 랜덤 포레스트는 여러 개의 랜덤한 의사결정트리를 생성하여 예측한다.

#### 원리
- 데이터 셋에서 무작위로 중복을 허용해서 n개 선택한다.
- 선택한 n개의 데이터 샘플에서 데이터 특성값(feature)을 중복 허용없이 d개 선택한다.
- 이를 활용하여 의사결정트리를 학습하고 생성한다.
- 1~3단계를 n번 반복한다.
- 1~4단계를 통해 생성된 k개의 의사결정트리를 활용하여 예측하고, 예측된 결과의 평균 또는 가장 많이 등장한 예측 결과를 선택하여 최종 예측값으로 결정한다.
- 1단계에서 랜덤으로 중복을 허용하여 선택한 n개의 데이터를 선택하는 과정을 부트스트랩(bootstrap)이라 한다.
- 부스트랩으로 추출된 n개의 데이터를 부트스트랩 샘플이라 부른다.
- 2단계에서 d값으로 주어지는 값은 데이터의 전체 특성의 개수의 제곱은즈로 주어진다.
- 5단계에서 여러 개의 의사결정트리로부터 나온 예측 결과들의 평균이나 다수의 예측 결과를 활용하는 방법을 앙상블 기법이라 한다.

