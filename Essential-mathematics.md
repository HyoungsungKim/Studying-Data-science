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