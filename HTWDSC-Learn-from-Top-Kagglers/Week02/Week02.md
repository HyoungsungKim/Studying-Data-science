# Week 02 Exploratory data analysis (EDA)

- Data science가 새로운 데이터를 받았을때 가장 먼저 해야 할 일
  - To know that are the must important things from data
  - Exploration perspective we need to pay attention

## What is Exploratory Data Analysis (EDA)

- Better understand the data
  - Always first thing you have to do
- Build intuition about the data
- Generative hypotheses
- Find insight
- Visualization
  - One of the main EDA tools is visualization

Do EDA first! Do not immediately dig into modeling

## Building intuition about the data

- Get domain knowledge
  - It helps to deeper understand the problem
- Understand how the data was generated
  - It is crucial to understand the generation process to set up a proper validation scheme
- Check if the data is intuitive
  - And agrees with domain knowledge

## Exploring Anonymized Data

개인 정보 보호를 목적으로 삭제 또는 변조 된 데이터

- What can we do with it?

### Anonymized data

- 내용 유출을 막기 위해 hash 같은 것으로 암호화 되어 있는 경우
- For anonymized data,
  - Guess the meaning of the columns
  - Guess the type of the columns
- Explore feature relations
  - Find relations between pairs
  - Find feature group

### Conclusion

- Two things to do with anonymized feature
  - Try to decode features
    - Guess the true meaning of the feature
  - Guess the feature type
    - Each type needs its own preprocessing

## Visualization

Explore individual features

- Histogram
- plots
- statistics (e.g., mean, var, std...)
- Value count (Count the number of distinct feature values)

### Explore feature relations

- Scatter
  - We can effectively use scatter plots to check if the train data distribution in the train & test data are same
- Correlation pairs
- Groups
  - Correlation plot + clustering

## Dataset cleansing and other things to check

- Dataset cleansing
  - Constant feature
  - Duplicated feature
- Other things to check
  - Duplicated rows
  - check if dataset is shuffled

### Duplicated and constant feature

- 예를 들어 2019년에 대한 데이터라는걸 알 경우 2019년을 나타내는 column은 메모리 낭비임
- Column의 모든 값이 같은 값일 때
- 중복된 column이 존재 할 경우
  - 실수인건지 의도된건지 파악 할 필요 있음
- Check data is shuffled

Read -> Overview -> Cleaning (remove constant features)

## Validation strategies

- Hold out
- k-fold
- Leave-one-out
- Stratification
  - Data를 population에서 뽑을때 원본 population의 분포를 유지 함.

### Hold out

- Simple data split which divides data into two parts

### K-fold

- 데이터를 두개로 나눈 set을 여러개 만듬
- 이 set에서 나온 결과를 합쳐서 사용

### Leave-one-out

- 데이터 하나만 뺴둠
  - k-fold에서 데이터의 개수가 k와 같을 경우
- Special case of k-fold (When k is equal to the # of samples)
- This methods can be helpful if we have too little data and just enough to re-train

## Data splitting strategies

Different approaches to validation

- Previous and next target value
- Time-based trend

두가지 경우로 나누고 접근 방법을 다르게 해야 함.

### Splitting data into train and validation

1. Random, row-wise
   - Usually it means that rows are independent
   - Dependencies가 존재하는 경우 다른 방법 사용해야 함.
2. Time-wise
   - e.g., 지난주 방문자수를 이용해서 이번주 방문자수를 예측 할 경우
3. By id
   - Based on user history

## Problems occurring during validation

Train set에서 얻은 결과와 Test set에서 얻은 결과에 차이가 발생 할 경우

### Validation stage

Because of different score and optimal parameters

- Too little data
- Too diverse and inconsistent data

We should do extensive validation

- Average score from different k-fold split
- Tune model on one split, evaluate score on the other

### Submission stage

- Leader board score is consistently higher/lower than validation score
- Leader board score is not correlated with validation score at all

Train set과 test set의 일관성이 없을 경우 이런 문제 발생

- Validation set을 test set과 같은 분포 (유사하게) 만듬

### For validation

- Choose correct splitter strategy for train/validation split
- Make sure the same distributions in validation and testing

### Conclusion

- If we have big dispersion of score on validation score, we should extensive validate
  - Average score from different k-fold split
  - Tune model on one split, evaluate score on the other
- If submissions score do not match local validation score, we should
  - Check if we have too little data in public leader board
  - Check if we over-fitted
  - Check if we choose correct splitting strategy
  - Check if train/test have different distributions

## Data leakage

Train data에서 사용했던게 test data에 없는 경우

(Data from outside is used to create the model)

### Leak in times series

- Split should be done on time
  - In real life, we don't have information from future
  - In competition, first thing to look; train/public/private data split, is it on time?
- Even when split by time, features may contain information about future.
  - User history in CTR tasks
  - Weather

Leakage를 exploit해서 prediction 성능을 올릴수도 있음

- Feature generation 같은건가...?