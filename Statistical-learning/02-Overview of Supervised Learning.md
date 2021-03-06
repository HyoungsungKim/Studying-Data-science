# 2 Overview of Supervised Learning

## 2.4 Statistical Decision Theory

We first consider the case of a quantitative output, and place ourselves in the world of random variables and probability spaces.

- We seek a function $f(x)$ for predicting $Y$ given values of the input $X$.
- This theory requires a loss function $L(Y, f(X))$ for penalizing errors in prediction
  - The most common and convenient is squared error loss: $L(Y, f(X)) = (Y-f(X))^2$
  - This leads us to criterion for choosing f,

$$
EPE(f) = E(Y- f(X))^2
$$

The solution is 
$$
f(x) = E(Y|X=x)
$$

- EPE를 최소화하는 경우(Linear model에서)

The nearest-neighbor methods attempt to directly implement this recipe using the training data.
$$
\hat{f}(x) = Ave(y_i|x_i \in N_k(x))
$$

- $N_k(x)$ is the neighborhood containing the k points in T closet to x.
- Settling for nearest neighborhood as a surrogate for conditioning will fail us miserably.
  - ***The convergence above still holds, but the rate of convergence decreases as the dimension increases***

***How does linear regression fit into this framework?***

-  The simplest explanation is that one assumes that the regression function f(x) is approximately linear in its arguments:

$$
f(x) \approx x^T\beta
$$

Plugging this linear model for f(x) into EPE and differentiating we can solve for $\beta$ theoretically:
$$
\beta = [E(XX^T)]^{-1}E(XY)
$$

- Least squares assumes f(x) is well *approximated by a globally linear function.*
- k-nearest neighbors assumes f(x) is well *approximated by a locally constant function.*

## 2.5 Local Methods in High Dimensions

We have examined two learning techniques for prediction so far:

- The stable but biased ***linear model***
- The less stable but apparently less biased class of ***k-nearest-neighbor estimates***

It would seem that with a reasonably large set of training data, we could always approximate the theoretically optimal conditional expectation by k-nearest-neighbor averaging, since we should be able to find a fairly large neighborhood of observations close to any x and average them.

- ***This approach and our intuition breaks down in high dimensions***, and the phenomenon is commonly referred to as ***the curse of dimensionality***.
- There are many manifestations of this problem

Denote the training set by $T$. We can compute the expected prediction error at $x_0$ for our procedure, averaging over all such samples of size 1000.

- Since the problem is deterministic, this is the mean square error (MSE) for estimating f(0):
  - We use the 1-nearest-neighbor rule to predict $y_0$ at the test-point $x_0$.

$$
\begin{align}
MSE(x_0) &= E_T[f(x_0) - \hat{y}]^2 \\
&= E_T[\hat{y}_0-E_T(\hat{y}_0)] + [E_T(\hat{y_0} - f(x_0)]^2 \\
&= Var_T(\hat{y}_0) + bias^2(\hat{y}_0)

\end{align}
$$

- Bias-variance decomposition.
  - We have broken down the MSE into two components that will become familiar as we proceed: variance and squared bias.
  - Such a decomposition is always possible and often useful, and is known as the ***bias–variance decomposition.*** 

## 2.6 Statistical models, Supervised Learning and Function Approximation

The class of nearest-neighbor methods can be views as direct estimates of this condition; expectation, but we have seen that they can fail in at least two ways:

- If the dimension of the input space is high, the nearest neighbors need not be close to the target point, and can result in large errors;
- If special structure is known to exists, this can be used to reduce both the bias and the variance of the estimates
- Nearest-neighbor은 고차원에서 좋지 않음

### 2.6.1 A Statistical Model for the Joint Distribution Pr(X, Y)

Suppose in fact that our data arose from a statistical model (Additive error model)
$$
Y = f(X) + \epsilon
$$

- Where the random error $\epsilon$ has $E(\epsilon) = 0$ and is independent of X.
- The assumption in equation that the errors are independent and identically distributed is not strictly necessary, but seems to be at the back of our mind when we average squared errors uniformly in our EPE(expected prediction error) criterion

For most systems the input–output pairs (X, Y) will not have a deterministic relationship Y=f(X).

- Generally there will be other unmeasured variables that also contribute to Y, including measurement error.
- The additive model assumes that we can capture all these departures from a deterministic relationship via the error $\epsilon$.
- In general the conditional distribution Pr(Y|X) can depend on X in complicated ways, but the additive error model precludes these.

So far we have concentrated on the quantitative response. ***Additive error models are typically not used for qualitative outputs G***; in this case the target function p(X) is the conditional density Pr(G|X), and this is modeled directly. 

### 2.6.2 Supervised Learning

### 2.6.3 Function Approximation

Although somewhat less glamorous than the learning paradigm, ***treating supervised learning as a problem in function approximation*** encourages the geometrical concepts of Euclidean spaces and mathematical concepts of probabilistic inference to be applied to the problem. This is the approach taken in this book.

Many of the approximations we will encounter have associated a set of parameters $\theta$ that can be modified to suit the data at hand. For example,the linear model $f(x) =x^T\beta$ has $\theta=\beta$. Another class of useful approximators can be expressed as linear basis expansions
$$
f_\theta(x) = \sum^K_{k=1}h_k(x)\theta_k
$$

- Where $h_k$ are a suitable set of functions or transformations of the input vector x.

We also encounter nonlinear expansions, such as the sigmoid transformation common to neural network models,
$$
h_k(x) = \frac{1}{1 + exp)-x^T\beta_k}
$$
We can use least squares to estimate the parameters $\theta$ in $f_\theta$ as we did for the linear model, by minimizing the residual sum-of-squares
$$
RSS(\theta) = \sum^N_{i=1}(y_i-f_\theta(x_i))^2
$$
as a function of $\theta$.

- While least squares is generally very convenient, it is not the only criterion used and in some cases would not make much sense.
- A more general principle for estimation is maximum likelihood estimation.

$$
L(\theta) = \sum^N_{i=1}log(Pr_\theta(y_i))
$$

- The principle of maximum likelihood assumes that the most reasonable values for $\theta$ are ***those for which the probability of the observed sample is largest***. 

The log-likelihood (also referred to as the cross-entropy) is 
$$
L(\theta) = \sum^N_{i=1}logp_{g_i, \theta}(x_i)
$$
and when maximized it delivers values of $\theta$ that best conform with the data in this likelihood sense.

## 2.7 Structured Regression Models

We have seen that although nearest-neighbor and other local methods focus directly on estimating the function at a point, ***they face problems in high dimensions***.

- They may also be inappropriate even in low dimensions in cases where more structured approaches can make more efficient use of the data.
- This section introduces classes of such structured approaches.
- Nearest neighbor and other local methods : 고차원에서 성능 안좋음, 저차원에서는 더 좋은 방법 존재 함

### 2.7.1 Difficulty of the Problem

Consider the RSS criterion for an arbitrary function f,
$$
RSS(f) = \sum^N_{i=1}(y_i-f(x_i))^2
$$
If the sample size N were sufficiently large such that repeats were guaranteed and densely arranged, it would seem that these solutions might all tend to the limiting conditional expectation