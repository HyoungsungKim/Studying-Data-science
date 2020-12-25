# Boosting, Bagging and Stacking

Reference : https://stats.stackexchange.com/questions/18891/bagging-boosting-and-stacking-in-machine-learning/187700

All three are so-called "meta-algorithm": approaches to combine several machine learning techniques into one predictive model in order to

- Bagging : decrease the variance
- Boosting : decrease the bias
- Stacking : Improve predictive force

## Bagging (Bootstrap Aggregating)

- ***It decrease the variance*** of your prediction by generating additional data for training from your original dataset.
  - Suitable for high variance low bias models
- ***Parallel ensemble*** : Each model is build independently
  - 여러개의 모델을 독립적으로 학습시켜서 결과를 합침 (e.g., random forest)
- By increasing the size of training set you cannot improve the model predictive force, but just decrease the variance.

## Boosting

- ***It decrease bias***
  - Suitable for low variance high bias models
- ***Sequential ensemble*** : Try to add new models that do well where previous model lack.
  - 이전 모델에서 잘 수행 하지 못했던 걸 다음 모델에서 수정해서 다시 테스트 함 (e.g., gradient boosting)
- It is two-step approaches.
  - First, uses subsets of the original data to produce a series of averagely performing models
  - Second, boosts their performance by combining them together using particular cost function.

## Stacking

It is similar to boosting

- Apply several models to your data such as first step of boosting
- The difference is you don't have just an empirical formula for your weight function, rather you introduce a meta-level and use another model/approach to estimate the input together.
- In other word, to determine what models perform well and what model badly perform