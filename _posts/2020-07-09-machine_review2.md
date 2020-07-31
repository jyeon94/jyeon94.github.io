---
title: "R 머신러닝 데이터 분석2"
tags: [R, TIL]
style:
color:
description: 나성호 강사님 강의안 복습
---
### 지도학습 프로세스

#### 프로세스
1. 문제선정
2. 데이터셋 준비
3. 데이터셋 분할
4. 모형 적합
5. 최종모형 선택

#### 데이터셋 분할 방법
- 데이터셋 분할 방법은 크게 자료분할과 k-겹 교차검증의 2가지로 나뉜다
- 데이터셋의 크기가 큰 경우, 자료분할(Hold-out Validation)을 주로 사용한다
   - 전체 데이터셋을 훈련용, 검증용, 시험용 데이터셋으로 나눈다
   - 데이터를 나눌 때 단순임의추출방식을 사용하지만 만약 데이터셋이 계층 분리가 된다면 층화추출 방식(stratified sampling)을 사용한다
   - 층화추출법이란 모집단을 먼저 중복되지 않도록 층으로 나눈 다음 각 층에서 표본을 추출하는 방법이다
- 데이터셋의 크기가 작은 경우, k겹-교차검증(k-Cross Validation)이 쓰인다
   - 전체 데이터셋을 훈련용 데이터셋과 시험용 데이터셋으로 나눈다
   - 그후 다시 훈련용 데이터셋을 k개의 그룹으로 나눈 다음, k-1개 그룹으로 모형을 적합하고 나머지 그룹으로 추정하는 것을 k번 반복한다
   - k번의 반복으로 확보한 추정값의 평균을 구하면 해당 모형의 성능 지표가 된다.

### 회귀모형의 성능평가

#### 성능 평가 기준
- 회귀모형은 목표변수가 연속형 숫자이므로, 실제값과 추정값의 차이인 오차를 계산하여 모형의 성능을 평가할 수 있다
   - 오차(Error) = 실제값(Actual Value) - 추정값(Predicted Value)
- 모형 전체의 오차 크기를 계산할때 개별 오차를 단순 합계하면 결국 0이 되므로 부호를 제거하기 위해서 일반적으로 제곱 또는 절대값을 취한다
- 회귀모형의 성능을 평가하는 지표는 여러 개이지만, 대체로 비슷한 결과를 얻게 되므로 한 가지 지표를 정하여 사용하면 된다

#### 회귀모형의 서능 평가 지표 (4가지)
- MSE (Mean Squared Error): 오차 제곱합을 전체 건수로 나눈 평균이다
	- MSE = $$1/n \sum_{i=1}^n(yi-\hat{yi})^2$$
- RMSE (Root Mean Squared Error): MSE의 양의 제곱근. 오차와 척도를 맞춘것
	-  RMSE = $$\sqrt{1/n \sum_{i=1}^n(yi-\hat{yi})^2}$$
- MAE(Mean Absolute Error): 오차의 절대값을 더한 후 건수로 나눈 평균
	-  MAE = $$1/n\sum_{i=1}^n|yi-\hat{yi}|$$
- MAPE(Mean Absolute Percentage Error): 오차의 절대값을 실제값으로 나눈 비율의 평균
	- MAPE = $$1/n\sum_{i=1}^n|yi-\hat{yi}|/|yi|$$

분류모형의 성능평가는 내용이 많으므로 다음 포스트에 올리겠습니다.