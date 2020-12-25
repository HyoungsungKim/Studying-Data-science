# Visualization of time series data

## 시계열 패턴

### 추세 (Trend)

- 데이터가 장기적으로 증가하거나 감소 할 때

### 계절성 (Seasonality)

- 특정 요일이나 날에 반복적인 패턴이 나타나는 경우

### 주기성 (Cycle)

- 경기 순환과 관련되어 고정된 빈도가 아닌 형태로 증가나 감소하는 모습
- 계절성은 일정 시간을 기준으로 반복되지만 주기성은 그렇지 않음
  - 주기성은 더 변동성이 큼

## 계절성 그래프

- Polar coordinate (극좌표계)로 나타내면 좋음

## 산점도

- 시계열 사이의 관계를 살필 때 쓸모 있음.

### 상관 (Correlation)

- 상관 계수 (correlation coefficient)는 선형관계의 강도만 측정하기 때문에 종종 오해로 이어질 수 있음.
  - 값에만 의존하지 말고 직접 그려보는 것이 중요함.
  - 예를 들어 데이터가 편향되어 수치상으로는 높은 correlation을 가진것 처럼 보일 수도 있음

## 자기 상관 (Autocorrelation)

$$
r_k = \frac{\sum^T_{t=k+1}(y_t - \bar{y})(y_{t-k} - \bar{y})}{\sum^T_{t=1}(y_t - \bar{y})^2}
$$



- $\bar{y}$ : 평균
- 상관값이 두 변수 사이의 선형 관계의 크기를 측정하는 것처럼, 자기상관(autocorrelation)은 시계열의 *시차 값(lagged values)* 사이의 선형 관계를 측정함
  - 상관 : 변수와 변수 사이( $corr(a, b)$ )
  - 자기 상관 : 시간에 따라 달라지는 변수를 시간에 대한 상관을 계산 ($autocorr(y_t, y_{t+k})$)
- 계절성 같은 특징을 찾기 좋은듯

### 백색 잡음 (White noise)

- Autocorrelation이 없는 시계열
  - 정확하게 말하면 모든 자기 상관의 95%가 $\pm 2 / \sqrt{T}$, T는 시계열의 길이