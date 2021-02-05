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