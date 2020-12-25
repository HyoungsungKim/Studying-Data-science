# Week 01

## Recap of main ML algorithms

- Linear
- Tree based
- K-nearest neighbors (KNN)
- Neural networks

***NO free lunch theorem:***  모든 task에 대해서 완벽하게 동작하는 알고리즘은 없음

- 따라서 task에 맞춰 적절한 알고리즘을 선택하는 것이 중요함.
- The most powerful methods are GBDT of tree based and neural network
  - But you should not underestimate other methods

### Linear Model

- Logistic regression
- Support vector machine (SVM)
  - 데이터 경계의 마진을 최대화 하는 알고리즘

### Tree based

- Decision tree
- Random forest
- Gradient boost decision tree (GBDT)
  - Very powerful for tabular data
  - Thus, it is used as default model

Tree based model is not good to capture linear dependencies. Because it needs a lot of split.

- Liner 모델 사용할때 decision이 1번이면 될걸 tree based model은 여러번 나눠야 할 경우가 있음

#### Gradient boosting

- 회귀 분석 또는 분류를 할 때 사용 할 수 있는 예측 모형
  - 앙상블 방법론 중 부스팅 계열에 속함
- Tabular format 데이터에 좋은 성능을 보여줌
- 머신러닝 알고리즘 중 가장 좋은 성능을 보임

### KNN

레이블이 없는 데이터를 인접 데이터의 레이블로 정의 함.

- It is heavy rely on how to measure point "closeness" 

### Neural network

- It generates smooth non-linear decision boundary

## Feature preprocessing and generation

### overview

- Feature preprocessing is often necessary
- Feature generation is powerful technique
- Preprocessing and generation pipelines depend on a model type

Feature preprocessing and generation is another hyper-parameters that need to optimizes

### Features

- Numeric
- Categoric
- Ordinal
- Datetime
- Coordinates

### Numeric features

- Preprocessing
  - Tree based model
  - Non-tree based model (e.g., linear)
- Feature generation

#### Preprocessing: Scaling

1. MinMaxScaler

   - Normalize all x axis of data to [0, 1]

   $$
   X = \frac{X - X.min()}{X.max() - X.min()}
   $$

   

2. StandardScaler

   - Standardize all x axis of data with mean = 0 and std = 1

   $$
   X = \frac{X - X.mean()}{X.std()}
   $$

- Scaling은 non-tree based model에서는 MinMax scaling이나 Standard scaling이나 차이가 없음
- 주로 tree based model에서 효과적
- KNN에서는 중요한 feature에 가중치를 크게 주는게 좋음
  - 예를 들어 확률적으로 많이 발생하는 데이터에 가중치 줘서 레이블 없는 데이터에 가까워지게 (?)

#### Preprocessing: Outliers

- 모든 데이터를 포함해서 distribution을 그리는 것 보다, outlier를 제외한 99% 값들로 distribution을 그렸을 때 insight를 얻을 수 있는 경우도 있음

#### Preprocessing: Rank

- Outliers를 outlier가 아닌 데이터에 가깝게 이동시킴 (Scaling과 다름)
  - Scaling은 모든 데이터에 가중치 줌

#### Preprocessing: ETC

데이터의 형태를 변형 함: 예를 들어 최저값과 최대값의 차이가 클경우 log를 씀 

Non-linear 방법 쓰는듯? sqrt(a + b) != sqrt(a) + sqrt(b)

1. Log transform:
   - np.log(1 + x)
2. Raising to the power < 1:
   - np.sqrt(x + 2/3)

#### Feature generation

기존 feature들을 조합하여 새로운 feature를 만듬

- Utilize prior knowledge
- Exploratory data analysis

e.g.) price of real estate + area of real estate = price per m^2 of real estate

#### Conclusion

1. Numeric feature preprocessing is different for tree and non-tree models:
   - Tree-based models does not depend on scaling
   - Non-tree based models hugely depend on scaling
2. The most often used preprocessing are:
   -  MinMaxScaler
   - StandardScaler
   - Rank - Sets spaces between sorted values to be equal
   - np.log(1+x) and np.sqrt(1 + x)
3. Feature generation
   - Prior knowledge
   - Exploratory data analysis

### Categorical and ordinal feature

- Make a data as numerical or boolean data such as one-hot vector
  - Class of flight seat [economy, business, first] -> [0, 1, 2]
  - Male/female [male, female] -> [0, 1]

### Handling missing values

It is different depend on data 

- NaN -> -999, 0, or etc
- Mean, median
- Reconstruct value 

## Feature extraction from text

There are two ways

- Apply bag of words
- Embedding (word2 vector)

### Bag of words

- Count the number of occurrences for each word, and place this value in the appropriate column
- 단어의 빈도수에 대한 분석
- Term frequency
  - tf = 1 / x.sum(axois=1)[:,None]
  - x = x * tf
- Inverse document frequency
  - 한 단어가 전체 문서에서 얼마나 공통적으로 많이 등장하는지 나타냄
  - 만약 tf(빈도수)가 전체 문서에서 많이 나온다면 그 단어는 중요도가 떨어지는 단어임
  - 따라서 tf가 클수록 가중치를 낮추기 위해 tf의 역수를 사용 함.
  - 전체 문서수가 증가 할 때 데이터가 증가 할 수 있기 때문에 log를 사용 함.
  - idf = np.log(x.shape[0] / (x >0).sim(0))
  - x = x * idf
  - ***Scale down appropriate features***
- tf = # of target / # of samples
- idf = log(# of samples / # of targets)
- idf는 tf의 역수에 log 취한것
- tf-idf -> 빈도 수 뿐만 아니라 동시에 다른 문서에서는 잘 나타나지 않는 것 까지 고려함.
  - 값이 크면 특정 문서에서만 등장 함
  - 작으면 전체 문서에서 등장 함

### Text preprocessing

- Stemming : 비슷한 의미인 단어를 처리함 (형태소 분석)
- Lemmatization : 비슷한 의미와 품사까지 고려 해서 처리함
- Stopwords : 중요하지 않은 단어는 모델에서 고려하지 않음

### Conclusion

Pipe line of Bag of word (BoW)

1. preprocessing : Stemmingm, Lemmatization, Stopwords
2. N-grams can help to use local context
3. Post processing : TF-IDF

## Word2Vec

- Text를 벡터로 표현 함
  - Feature끼리 연산이 가능해짐 (e.g., queen - woman + man = king)
  - For example,
    - Queen : [1 1 0]
    - Woman : [0 1 0]
    - Man : [0 0 1]
    - King : [1 0 1]

### BoW and Word2Vec comparison

1. Bag of Words
   - Very large vectors but it has nice benefit
   - Meaning of each value in vector is known
   - Bow produces one number (a word count)
2. Word2Vec
   - Represent word as a vector (one vector per word)
   - Relatively small vectors
   - Values in vectors can be interpreted only in some cases
   - The words with similar meaning often have similar embedding

***Usually BoW and Word2Vec give a quite different result***

- These can be used together