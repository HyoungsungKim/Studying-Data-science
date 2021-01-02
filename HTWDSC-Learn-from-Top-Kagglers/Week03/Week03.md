# Week 03

- Metric optimization
- Mean encodings

## Metric optimization

### Lesson overview

- Metric
  - Why there are so many 
    - We have to use different metrics for different problems
  - Why should we care about them in competitions
    - It is how the competitors are ranked
- Loss v.s. Metric
- Review the most important metrics
  - For regression tasks and classification
  - Discuss baseline solutions for their optimization
- Optimization techniques for the metric

#### Metrics

- It is very important to understand how metric works and how to optimize it efficiently
  - Competition의 평가 방법에 맞는 metric을 사용해야 함.
- If your model is scored with some metric, you got the best results by optimizing exactly that metric.
  - The biggest problem is that some metrics cannot be optimized efficiently
- Test data와 train data가 다를 경우 -> metric optimization 필요함

### Regression

Regression metric

- Mean square error (MSE), Root mean square error (RMSE), R-squared
- Mean absolute error (MAE)
- (Root) mean square percentage error (MSPE), (Root) mean absolute percentage error (RMAPE)
- (Root) mean square least error (RMSLE)

#### Mean square error

$$
MSE = \frac{1}{N}\sum^N_{i=1}(y_i-\hat{y_i})^2
$$

- In data science, people use it when they don't have any specific preference for the solution to their problem, or when they don't know other metric.

- Best constant for simplest baseline model

  - Set $\hat{y}_i$ as mean value of $y$


#### Root mean square error

$$
RMSE = \sqrt{\frac{1}{N}\sum^N_{i=1}(y_i-\hat{y}_i)^2} = \sqrt{MSE}
$$

- RMSE and MSE are similar in terms of their minimizers
- If we have two set of predictions, A and B, MSE(A) > MSE(B)  <=> RMSE(A) > RMSE(B)
  - 만약 metric으로 RMSE 썼다면, MSE도 사용 가능 함.
  - MSE가 다루기 쉬워서 RMSE보다는 MSE 선호하는 사람들이 많음
- RMSE와 MSE는 Gradient는다름
  - Loss minimization을 Gradient based method로 사용 할 경우 interchange 할 수 없음

#### R-square

$$
R^2 = 1 - \frac{MSE}{\frac{1}{N}\sum^N_{i=1}(y - \bar{y})^2} = 1 - \frac{\frac{1}{N}\sum^N_{i=1}(y_i-\hat{y}_i)^2}{\frac{1}{N}\sum^N_{i=1}(y - \bar{y})^2}
$$

- RMSE나 MSE는 모델이 얼마나 좋은지 판단하기 힘듬
  - Numeric한 값이기 때문에 normalized된 값이 필요함.
- $R^2$는 normalized되어 있음.
  - Perfect하면 1, 매우 안좋으면 0
  - MSE가 0이면 $R^2$는 1
  - MSE가 optimized되면 $R^2$도 할 수 있음.

#### Mean absolute error

$$
MAE = \frac{1}{N}\sum^N_{i=1}|y_i - \hat{y}_i|
$$

- It penalizes huge errors that not as bad as MSE does
- It is not sensitive to outliers as MSE does
- MAE is used widely in finance
  - 10 \$ error is two times large error than 5\$ error in MAE
  - In MSE, 10\$ error is four times large error than 5\$ error

#### MSE vs MAE

- Do you have an outliers in the data?
  - Use MAE
- Are you sure they are outliers?
  - Use MAE
- Or they are just unexpected values we should still care about?
  - Use MSE
- MAE is more robust than MSE

#### Conclusion

- MSE, RMSE, R-square
  - They are the same from optimization perspective
- MAE
  - Robust to outliers

