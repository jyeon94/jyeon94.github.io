---
title: "R 머신러닝 데이터 분석3"
tags: [R, TIL]
style:
color:
description: 나성호 강사님 강의안 복습
---

### 분류모형의 성능평가

#### 분류모형의 성능 평가 지표

- 분류모형은 목표변수가 범주형이므로, 데이터셋의 실제값과 분류모형의 추정값이 서로 같거나 다름으로 성능을 평가할 수 있다
- 정확도(Accuracy)나 오분류율(Missclassification)등으로 분류모형의 성능을 측정하는 대신 다양한 지표를 활용한다
  - 오분류율은 '1-정확도'이다
- 해결해야 하는 문제의 내용과 데이터셋이 보유하고 있는 특징에 따라 참고해야 할 성능 평가 지표가 다를 수 있다

#### 분류모형의 성능평가 기준(3가지)

- 혼동행렬(Confusion Matrix)은 실제값(Actual Value) 및 추정값(Predicted Value)으로 그린 2X2 행렬이다
- 정확도, 민감도(재현율), 정밀도, 1-특이도 등 여러 지표를 한번에 확인할 수 있다
- F1점수는 혼동행렬의 여러 지표 중 민감도(Sensitivity)와 정밀도(Precision)의 조화평균을 의미한다
- 둘 중 하나의 지표만 우수한 경우 F-1점수는 낮아진다.
  - 조화평균이란 평균속려의 의미를 가집니다. 갈때 akm/s 되돌아 올때 bkm/s의 속력으로 이동할때 평균속력은 a와 b의 조화평균이다
  - 공식은 2ab/a+b 이다
- ROC 곡선은 Receiver Operating Characteristic의 약자로 x축은 '1-특이도', y축은 민감도로 그린 그래프다
- ROC 곡선 아랜의 면적을 AUC라고 하며, AUC의 크기로 모형의 분류 성능을 측정한다

##### 혼동행렬(Confusion Matrix)

<img width="166" alt="주석 2020-07-13 115357" src="https://user-images.githubusercontent.com/57039464/87266220-af00ef00-c4ff-11ea-8a85-6d250a84cf66.png">

Positive는 분석가가 관심을 갖는 범주로 지정한다 ex) '대출연체', '고객 이탈'

- TP: 모형이 긍정이라고 예측했고 실제값과 같은 건수
- FP: 모형이 긍정이라고 예측했지만 실제값과 다른 건수
- FN: 모형이 부정이라고 예측했지만 실제값과 다른 건수 -> 의료에선 최대한 줄어야함(암인데 암이 아니라고 판정)
- TN: 모형이 부정이라고 예측했고 실제값과 같은 건수

##### 혼동행렬 (계속)

- P = TP + FN
- N = FP + TN
- T = P + N = TP + FN + FP + TN

##### 혼동행렬로 계산되는 지표

- 정확도: 실제값과 추정값이 서로 같은 건수를 전체 합으로 나눈 것
  - Accuracy = (TP + TN) / (P + N)
- 민감도: 실제값이 긍정인 것 중에서 분류모형이 맞춘 비율
  - Sensitivity = TP / P
- 정밀도: 분류모형이 긍정이라고 한 것 중에서 실제값도 긍정인 비율
  - Precision = TP / (TP + FP)
- 특이도: 실제값이 부정인 것 중에서 모형이 맞춘 비율
  - Specificity = TN / N
- 1- 특이도: 실제값이 부정인 것 중에서 분류모형이 긍정으로 오분류한 비율
  - 거짓긍정비율(False Positive Rate, FPR)
    - False Positive Rate = FP / N
    - 1-TN/N = (N-TN)/N = (FP+TN-TN)/N = FP/N

##### F1 점수

- F1 점수는 "민감도"와 "정밀도"의 조화평균을 의미한다
  - F1 Score = 2TP / (2TP + FP + FN)
- 분류모형의 성능을 비교할 때 민감도와 정밀도를 동시에 고려한다면 어떤 모형의 성능이 더 우수한지 판단하기 어렵지만 F1 점수로 환산하면 비교할 수 있다

##### ROC 곡선

![Receiver-operating-characteristic-ROC-curve-of-circulating-free-DNA-between-cancer](https://user-images.githubusercontent.com/57039464/87269914-95fd3b80-c509-11ea-8f6d-138bdeed43f9.png)

- ROC 곡선은 X축에 1-특이도(FPR)를 기준으로 놓고 분류모형의 성능을 그린 그래프이다
- 분류모형이 성능이 우수할수록 "민감도"와 "특이도가" 동시에 높다
  - 정밀도가 높다면 FP가 0에 가깝다는 의미가 되므로 1-특이도도 낮아진다
  - ROC 곡선이 왼쪽 상단 모서리에 가까울 수록 좋은 모형이라고 할 수 있다

##### AUC (Area Under Cover)

- AUC는 ROC 곡선 아래에 해당하는 면적이다. AUC가 클수록 성능이 좋은 모형이다
- AUC는 이론상으로 0~1의 값을 갖는다
  - AUC가 0.5미만인 모형은 가치가 없다
  - 분류모형이 추정값을 랜덤하게 반환하면 ROC 곡선은 빨간색 점선처럼 그려지는데 이 모형의 AUC는 0.5가 되기 때문이다