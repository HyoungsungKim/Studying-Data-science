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

### Regression 2

#### From MSE and MAE to MSPE and MAPE

MSE의 문제점

- Shop 1 : Predicted : 9, sold : 10 -> MSE = 1
- Shop 2 : Predicted : 999, sold : 1000 -> MSE = 1
  - 두가지 경우 모두 MSE는 1이지만 shop 1같은 경우 오차가 10%임
  - MSE는 오차의 크기를 잡아내지 못함

MAE

- Shop 1 : Predicted : 9, sold : 10, MAE = 1
- Shop 2 : Predicted : 900, sold : 1000, MAE = 10000
  - 같은 10%오차인데 단위가 크면 error값이 매우 크게 차이남.

Relative metric

- Shop 1 : Predicted : 9, sold : 10, relative_metric = 1
- Shop 2 : Predicted : 900, sold : 1000, relative_metric = 1
  - 같은 10%의 오차로 잡아냄

Mean square prediction error (MSPE)
$$
\frac{100\%}{N}\sum^N_{i=1}(\frac{y_i - \hat{y}_i}{y_i})^2
$$
Mean absolute prediction error (MSAE)
$$
\frac{100\%}{N}\sum^N_{i=1}|\frac{y_i - \hat{y}_i}{y_i}|
$$

- MSE는 estimator로, MSPE는 predictor로 사용 함
  - Estimator : parameter를 추정하기 위해 값을 구하는 경우
    - e.g., sample의 평균을 이용하여 모집단의 평균을 estimate (또 다른 parameter를 추정)
  - Predictor : 데이터를 이용하여 데이터에 없는 값을 구하는 경우 
    - e.g., 부모의 키를 이용하여 자식의 키를 계산(값을 추정)

Root mean square logarithm error (RMSLE)
$$
RMSLE = \sqrt{\frac{1}{N}\sum^N_{i=1}(log(y_i + 1) - log(\hat{y}_i + 1))^2}
$$

### Classification

Overview

- Accuracy
- Logarithmic loss
- Area under ROC curve
- (Quadratic weighted) Kappa

#### Accuracy score

$$
Accurancy = \frac{1}{N}\sum^N_{i=1}[\hat{y}_i=y_i]
$$

- How frequently our class prediction is correct
- Best constant
  - Predict the most frequent class

#### Logarithmic loss (log loss)

Binary
$$
Logloss = -\frac{1}{N}\sum^N_{i=1}y_ilog(\hat{y}_i) + (1-y_i)log(1-\hat{y}_i)
$$
Multiclass
$$
Logloss = -\frac{1}{N}\sum^N_{i=1}\sum^L_{i=1}y_{il}log(\hat{y}_{il})
$$
In practice
$$
Logloss = -\frac{1}{N}\sum^N_{i=1}\sum^N_{l=1}y_{il}log(Min(Max(\hat{y}_{il}, 10^{-15}), 1-10^{-15}))
$$

- Clipped to be not from 0 to 1
- Best constant
  - The most frequent class

#### Area under curve - Receiver operating characteristic (AUC-ROC)

- Only for binary tasks
- Depends only on ordering of the predictions, not on absolute values

ROC curve

- ROC curve는 모든 분류 임계값에서 분류 모델의 성능을 보여주는 그래프임.
- 두개의 매개변수 사용
  - True-positive ratio (TPR)
    - $TPR = \frac{TP}{TP + TN}$
  - False-positive ratio (FRP)
    - $FPR = \frac{FP}{FP + TN}$
- ROC curve는 다양한 분류 임계값의 TPR 및  FPR을 나타냄.
- 분류 임계값을 낮추면 더 많은 항목이 양성으로 분류되므로 거짓양성과 참양성 모두 증가 함.
- ROC curve의 점을 계산하기 위해 분류 임계값이 다른 로지스틱 회귀 모형을 여러번 평가 할 수 있지만 비효율적임
  - 효율적인 정렬 기반 알고리즘인 Area under curve (AUC) 사용
  - AUC는 ROC curve의 아래 영역을 의미함
    - AUC의 범위는 0 ~ 1
    - AUC는 척도 불변임
      - AUC는 절대값이 아니라 예측이 얼마나 잘 평가 되는지 측정 함
      - AUC는 분류 임계값 불변임.
        - AUC는 어떤 분류 임게값이 선택되었는지와 상관없이 모델의 예측 품질을 측정 함.

$$
AUC = \frac{\text{# correctly ordered pairs}}{\text{total number of pairs}}
$$

### General approaches for metrics optimization

- What is loss and what is metric?
- General approaches to metric optimization

#### Loss and Metric

- Target metric : What we want to optimize
  - The metric or target metric is a function which we want to use to evaluate the quality of our model. 
  - e.g., for a classification, we may want to maximize accuracy of our predictions
  - How frequently the model outputs the correct label
  - But no one really knows how to optimize accuracy efficiently
  - *Instead, people come up with the proxy loss functions*
- Optimization loss : What model optimizes
  - e.g., logarithm loss
- Sometimes we are lucky and the model can optimize our target metric directly.
  - For example, for mean square error metric, most libraries can optimize it from the outset, from the box.
  - So the loss function is the same as the target metric
- And sometimes we want to optimize metrics that are really hard or even impossible to optimize directly.
  - In this case, we usually set the model to optimize a loss that is different to a target metric, but after a model is trained, we use hacks and heuristics to negate the discrepancy and adjust the model to better fit the target metric. 

#### Approaches for target metric optimization

- Just run the right model!
  - MSE, Log loss
- Preporcess train and optimize another metric
  - MSPE, MAPE, RMSLE
  - Directly optimize 할 수 없는 경우 (e.g., XGBoost) custom loss 사용
- optimize another metric, post process predictions
  - Accuracy, Kappa
- Write custom loss function
  - Any, if you can
- Optimize another metric, use early stopping
  - Any

### Regression metrics optimization

Regression metric

- MSE, (R)MSE, R-squared
- MAE
- (R)MSPE, MAPE
- (R)MSLE

#### RMSE, MSE, R-squared (L2 loss)

How do you optimize them?

- Just fit the right model!
  - tree based : boosting, Random forest
  - linear model : stochastic gradient descent
  - Neural networks

#### MAE (L1 loss)

How do you optimize them?

- Just fit the fight model
  - Tree based : random forest
  - Neural network

### Classification metrics optimization

Classification metric

- Logloss
- Accuracy
- AUC
- (Quadratic weighted) Kappa

#### Logloss

- How do you optimize it?
  - Just run the right model
    - Tree model : XGboost (부스팅 계열만 사용 가능)
    - Linear models : Stochastic gradient descent
    - Neural network

#### Accuracy

- How do you optimize it?
  - Fit any metric and tune threshold
  - or just optimize logloss
    - Tree based : boosting
    - Neural network

#### Pointwise vs pairwise

- 우리가 일반적으로 사용하는 loss 계산 방식이 pointwise임

  - Pointwise : compute each object individually
    - And sum or average it to get a total loss
    - We want  $y_0 = 0$, $y_1 = 1$ 
  - Pairwise : 순서 고려해야 할 때?
    - Recall that AUC is the probability of a pair of the objects to be ***ordered in the right way***
    - We want  $y_1 > y_2$
    -  A pairwise loss takes predictions and labels for a pair of objects and computes their loss. 
      - Ideally, the loss would be zero when the ordering is correct and greater than zero when the ordering is incorrect.
    - But in practice, different loss functions can be used. For example, we can use logloss.

  $$
  Logloss = -\frac{1}{N_1N_2}\sum^{N_1}_{j:y_j=1}\sum^{N_0}_{i:y_i=0}log(prob(\hat{y}_j - \hat{y}_i))
  $$

  

  

  

  

