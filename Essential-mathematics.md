# Essential mathematics

## Data and Sampling distribution

- Traditional statistics focused very much on the population, using theory based on strong assumptions about the population.
- Modern statistics has moved to the sampling, where such assumptions are not needed

### Size v.s. Quality

- In the era of big data, it is sometimes surprising that smaller is better.
  - Time and effort spent on random sampling not only reduces bias but also allows greater attention to data exploration and data quality. 
  - For example, missing data and outliers may contain useful information. 
  - 오히려 데이터가 적을때 아웃라이어 인식이 쉬울 수 있음.

#### When are massive amounts of data needed?

- The classic scenario for the value of big data is when the data is not only big but sparse as well.

### Regression to the Mean (평균 회귀)

*Regression to the mean* refers to a phenomenon involving successive measurements on a given variable: extreme observations tend to be followed by more central ones. Attaching special focus and meaning to the extreme value can lead to a form of selection bias.

- Regression to the mean is a consequence of a particular form of selection bias

## Central limit theorem

The central limit theorem receives a lot of attention in traditional statistics texts because it underlies the machinery of hypothesis tests and confidence intervals, which themselves consume half the space in such texts. Data scientists should be aware of this role; ***however, since formal hypothesis tests and confidence intervals play a small role in data science,*** and ***the bootstrap is available in any case, the central limit theorem is not so central in the practice of data science.***

### Standard Error

The standard error is a single metric that sums up the variability in the sampling distribution for a statistic. The standard error can be estimated using a statistic based on the standard deviation `s` of the sample values, and the sample size `n`:
$$
\text{Standard error} = SE = \frac{s}{\sqrt{n}}
$$

- As the sample size increases, the standard error decreases

### The Bootstrap

One easy and effective way to estimate the sampling distribution of a statistic, or of model parameters, ***is to draw additional samples, with replacement***, from the sample itself and recalculate the statistic or model for each resample.

- This procedure is called the ***bootstrap***
- ***It does not necessarily involve any assumptions*** about the data or the sample statistic being normally distributed.

> Bootstrap sample : A sample taken with replacement from an observed data set

- We simply replace each observation after each draw; that is, we sample with replacement. ***In this way we effectively create an infinite population*** in which the probability of an element being drawn remains unchanged from draw to draw. 

#### Resampling v.s. Bootstrapping

- In any case, the term bootstrap always implies sampling with replacement from an observed data set.

### Student t distribution

- 정규분포의 평균을 추정 할 때 사용 함.

## Statistical Experiments and Significance Testing

- Classical statistical inference
  - Formulate hypothesis -> Design experiment -> Collect data -> Inference/Conclusion

### A/B testing

An A/B test is an experiment with two groups to establish which of two treatments, products, procedures, or the like is superior. 

### Hypothesis tests

Hypothesis tests, also called *significance tests*

### One-Way v.s. Two-Way Hypothesis Tests

Often in an A/B test, you are testing a new option (say, B) against an established default option (A), and the presumption is that you will stick with the default option unless the new option proves itself definitively better.

- ***In such a case, you want a hypothesis test to protect you from being fooled by chance in the direction favoring B***. You don’t care about being fooled by chance in the other direction, because you would be sticking with A unless B proves definitively better.
- A가 좋다는 전제가 깔려있을 때 -> one-way direction test 필요
- So you want a directional alternative hypothesis (B is better than A).
  - ***In such a case, you use a one-way (or one-tail) hypothesis test.***
  - This means that extreme chance results in only one direction count toward the p-value

If you want a hypothesis test to protect you from being fooled by chance in either direction, the alternative hypothesis is bidirectional (A is different from B; could be bigger or smaller).

- In such a case, you use a two-way (or two-tail) hypothesis. This means that extreme chance results in either direction count toward the p-value.
- A와 B둘 중 어느게 좋은지 모를때 -> two-way direction test 필요
- A one-tail hypothesis test often fits the nature of A/B decision making, in which a decision is required and one option is typically assigned “default” status unless the other proves better.
- Software, however, including R and scipy in Python, typically provides a two-tail test in its default output, and many statisticians opt for the more conservative two-tail test just to avoid argument.

### Resampling

There are two main types of resampling procedures: the bootstrap and permutation tests.

- Bootstrap : sampling with replacement
- Permutation tests are used to test hypotheses, typically involving two or more groups, 

### Permutation Test

- ***Permute means to change the order of a set of values.*** 
  - The first step in a permutation test of a hypothesis is to combine the results from groups A and B (and, if used, C, D,...).
    - This is the logical embodiment(구체화하다) of the null hypothesis that the treatments to which the groups were exposed do not differ.
  - We then test that hypothesis by randomly drawing groups from this combined set and seeing how much they differ from one another. 

### t-test

There are numerous types of significance tests, depending on whether the data comprises count data or measured data,  how many samples there are, and what’s being measured. A very common one is the t-test, named after Student’s  t-distribution.

- In a resampling test, the scale of the data does not matter. You create the reference (null hypothesis) distribution from the data itself and use the test statistic as is.
- In the 1920s and 1930s, when statistical hypothesis testing was being developed, it was not feasible to randomly shuffle data thousands of times to do a resampling test.
- Statisticians found that a good approximation to the permutation (shuffled) distribution was the t-test, based on Gosset’s t-distribution.
- ***It is used for the very common two-sample comparison***—A/B  test—in which the data is numeric. But in order for the t-distribution to be used without regard to scale, ***a standardized form of the test statistic must be used***

## Regression and Prediction

Nowhere is the nexus between statistics and data science stronger than in the realm of prediction.

### Simple Linear Regression

Simple linear regression provides a model of the relationship between the magnitude of one variable and that of a second—for example, as X increases, Y also increases.

#### The Regression Equation

Simple linear regression estimates how much Y will change when X changes by a certain amount
$$
Y = b_0 + b_1X
$$

#### Fitted Values and Residuals

Important concepts in regression analysis are the fitted values (the predictions) and residuals (prediction errors).  

- Regression equation includes exploit error term

$$
Y_i = b_0 + b_1X_i + e_i
$$

- Fitted value (predicted values)

$$
\hat{Y}_i = \hat{b}_0 + \hat{b}_1X_i
$$

- Error term $e_i$ is

$$
\hat{e}_i = Y_i - \hat{Y}_i
$$

#### Least square

How is the model fit to the data?

- Residual sum of squares (RSS)

$$
\begin{align}
RSS &= \sum^n_{i=1}(Y_i - \hat{Y}_i)^2 \\
&= \sum^n_{i=1}(Y_i - \hat{b}_0 - \hat{b}_1X_i)^2
\end{align}
$$

- Regression models are typically fit by the method of least square

#### Prediction v.s. Explanation (Profiling)

With the advent of big data, regression is widely used to form a model to predict individual outcomes for new data (i.e., a predictive model) rather than explain data in hand. 

- A regression model that fits the data well is set up such that changes in X lead to changes in Y. However, by itself, ***the regression equation does not prove the direction of causation(인과)***.
- Conclusions about causation must come from a broader understanding about the relationship.
- Regression is used both for prediction and explanation

### Multiple Linear Regression

When there are multiple predictors, the equation is simply extended to accommodate them:
$$
Y = b_0 + b_1X_1 + b_2X_2 + \cdots + b_pX_p + e
$$

#### Assessing the Model

The most important performance metric from a data science perspective is *root mean squared error*, or *RMSE*.
$$
RMSE = \sqrt{\frac{\sum^n_{i=1}(y_i - \hat{y}^i)^2}{n}}
$$
This measures the overall accuracy of the model and is a basis for comparing it to other models (including models fit using machine learning techniques).

## Prediction Using Regression

### The danger of Extrapolation

***Regression models should not be used to extrapolate beyond the range of the data*** (leaving aside the use of regression for time series forecasting.). 

### Confidence and Prediction Interval

The t-statistics and p-values reported in regression output deal with this in a formal way, which is sometimes useful for variable selection. ***More useful metrics are confidence intervals***, which are uncertainty intervals placed around regression coefficients and predictions. 

## Classification

