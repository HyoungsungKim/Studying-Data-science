# Introduction

## 예측 작업의 기본단계

1. 문제 정의
2. 정보 수집
   - 어떤 데이터를 사용할 것인가에 대한 고민
   - e.g., 과거의 데이터를 그대로 사용 할 수 있을것인가?
3. EDA (Exploratory data analysis)
   - 그래프로 나타내는 것으로 시작
     - 일괄된 패턴이 존재하는가?
     - 의미 있는 추세가 존재하는가?
     - 계절성이 중요한가?
     - 경기 순환이 존재한다는 증거가 있는가?
4. 모델 선택
   - Regression
   - Exponential smoothing
   - Box-Jenkins ARIMA
   - Dynamic regression
   - hierarchical 예측
   - Neural network
   - vector auto-regression
5. 예측 모델 사용하고 평가하기

## 예측이란?

정보 (I)가 주어졌을때 시간 t에 대한 $y_t|I$의 확률 분포.

- 여기서 $y_t$의 평균을 예측이라 하고 $\hat{y_t}$라 함.

