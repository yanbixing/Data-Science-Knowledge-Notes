# Feature Engineering

## 1. Feature Engineering


### 1.1 General feature engineering techniques

- **Binning**: 
  - Assign different similar values to a same bin.
    - Works for both categorical and numerical data. 
    - Can effectively reduce the amount of distinct values in a feature.
- **Non-linear transformation** and **Multiplication combination**:
  - Take power(log,exponential)/combination of the feature.
    - Works for numerical features.
    - Equal to add "model variance" in the feature engineering way. Can make linear model fits better on complex data.
    - The idea is exactly same with kernel-svm: rise data to higher dimension.
- **Feature Extraction**:
  - Extract better feature from the raw data.
    - Works for complex objects, like text, image, webpage
    - E.g. Text embedding, extract time from string

### 1.2 Different categorical feature encoders.

#### 1.2.1 Label encoder

- Assign different integer labels to different categorical value. Typically, N values, $\{0,1,...N-1\}$, will be assigned to $N$ distinct categorical value.
- Suitable for ordered/sequential categorical features.
Ref: [Sklearn-LabelEncoder](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html)

#### 1.2.2 One-hot encoder

- Typically one-hot encoder: 
  - Use $N$-$\dim$ binary features to represent $N$ distinct values. The "1" i-th position denotes i-th category exists. As
    - Example, three categorical values A,B,C
      - A [001]
      - B [010]
      - C [100]
    - Understanding: i-th-to-right column stands for 'is i-th categorical value'.
  - Advantage:
    - Additive, the vectors can be added, means the items takes several categorical features. Dummy cannot.

- Dummy encoding: Also, we could use $(N-1)$-$\dim$ binary features to represent $N$ distinct values, as:
  - Example, three categorical values A,B,C
    - A $\rightarrow$ [00]
    - B $\rightarrow$ [01]
    - C $\rightarrow$ [10]
  - Understanding: the 1st-to-right column stands for 'is B', 2nd-to-right column stands for 'is C', A = 'is not B and is not C'

Ref: [StakeExchange](https://stats.stackexchange.com/questions/224051/one-hot-vs-dummy-encoding-in-scikit-learn), [Wiki-Dummy_variable](https://en.wikiversity.org/wiki/Dummy_variable_(statistics)), [TowardsDataScience](https://towardsdatascience.com/one-hot-encoding-multicollinearity-and-the-dummy-variable-trap-b5840be3c41a)


## 2. Missing value

### 2.1. Missing values

Missing value means a lot of values are unkown.

- Dropping
  - Too few, drop the rows (sample)
  - Too much, drop the feature (column)
- Filling
  - Forward/backfilling if there is a sequence relation
  - 0/Average/median/mode(majority) accordingly.
  - Train model to predict if it is related with other features.

### 2.2. Sparse value (TBD)

Sparse value means a lot of values are 0.

- Removing features
  - Manually
  - Automatically via model. E.g.
    - Lasso regularization
    - Reduce depth of tree models.
- Making it dense, with various linear/non-linear transformation
  - PCA
  - Feature hashing
  - Neural network (like word2vec)
- Making it dense, via model
  - Clustering like K-means

Ref: [KDnuggets Blog](https://www.kdnuggets.com/2021/01/sparse-features-machine-learning-models.html)
