# Feature Engineering

## 0. feature kinds and summary (TBD) 

- Categorical feature
  - Sparse features: features that is very sparse with typical methods like one-hot encoding, bag of word
    - Typical types: id, text, purchase history (user-item matrix)
    - Methods:
      - PCA
      - HashEncoder
      - Mean (Target) Encoding
      - Embedding
      - Extract keywords
      - Convert to binary/dense with experience/stats/model. E.g.
        - Summarize purchase history to histogram of purchase quantity on different category/company
        - Sentiment analysis: convert comments to positive/negative feedback.
  - Dense features: features that is dense with typical methods.
    - One-hot encoding.
    - Label encoding
  - (TBD) Ref on dense features???
- Numerical features
  - numerical features are dense

- Ref: 
  - [Educative-Grokking_ML_Interview-RecSys-Feature_Engineering](https://www.educative.io/courses/grokking-the-machine-learning-interview/m7yvJPANx19)
  - [StackExchange](https://stats.stackexchange.com/questions/266996/what-do-the-terms-dense-and-sparse-mean-in-the-context-of-neural-networks): Seems dense/sparse feature := the features that, if converted to a vector, the vector is dense/sparse. Like high cardinality feature is sparse feature.
  - [Quora](https://quoraengineering.quora.com/Unifying-dense-and-sparse-features-for-neural-networks): The different on implementation in NN for dense/sparse feature. For dense feature, normal FC is fine. For sparse, usually need an embedding layer, i.e. squeeze it to a smaller dimension, and the weight will be the embedding.


## 1. Methods

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

Ref [Analyticsvidhya-blog](https://www.analyticsvidhya.com/blog/2020/08/types-of-categorical-data-encoding/)

#### 1.2.1 Label encoder

##### 1.2.1.1. Algorithm

Assign different integer labels to different categorical value. Typically, N values, $\{0,1,...N-1\}$, will be assigned to $N$ distinct categorical value.

Ref: [Sklearn-LabelEncoder](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html)


##### 1.2.1.2. Properties

- Suitable for **ordered/sequential** categorical features.
 - E.g. ['small', 'medium', 'large']
 - Also called "Ordinal Encoding"

#### 1.2.2 One-hot encoder

##### 1.2.2.1. Algorithm

- Typically one-hot encoder: 
  - Use $N$-$\dim$ binary features to represent $N$ distinct values. The "1" i-th position denotes i-th category exists. As
    - Example, three categorical values A,B,C
      - A [001]
      - B [010]
      - C [100]
    - Understanding: i-th-to-right column stands for 'is i-th categorical value'.
  - Advantage compared to Dummy:
    - Additive, the vectors can be added, means the items takes several categorical features. Dummy cannot.

- Dummy encoding: Also, we could use $(N-1)$-$\dim$ binary features to represent $N$ distinct values, as:
  - Example, three categorical values A,B,C
    - A $\rightarrow$ [00]
    - B $\rightarrow$ [01]
    - C $\rightarrow$ [10]
  - Understanding: the 1st-to-right column stands for 'is B', 2nd-to-right column stands for 'is C', A = 'is not B and is not C'
  - Compared to no-dropping one-hot
    - Dummy encoding is NOT additive. 
      - Because the 1st dimension is unknown (cannot be inferred from other dimension) then.

Ref: [StakeExchange](https://stats.stackexchange.com/questions/224051/one-hot-vs-dummy-encoding-in-scikit-learn), [Wiki-Dummy_variable](https://en.wikiversity.org/wiki/Dummy_variable_(statistics)), [TowardsDataScience](https://towardsdatascience.com/one-hot-encoding-multicollinearity-and-the-dummy-variable-trap-b5840be3c41a)

##### 1.2.2.2. Properties

- Suitable for un-ordered categorical feature.
  - E.g. ['cat', 'dog', 'rabbit']

- Drawback: space complexity. Especially when there are multiple categorical features.
  - One-hot encoding only care about features one by one. Each feature take "number of its unique values" dimension
  - It doesn't consider: the uniqueness of the combination of features, or in another word, the uniqueness of samples.
  - It makes the matrix extremely **sparse**.

#### 1.2.3. Hash encoder (Feature hashing)

##### 1.2.3.1 Algorithm

Use hash function to map one or multiple features to a fixed-dimension vector. The size can be tuned.

##### 1.2.3.2 Properties

- The dimension of output is tunable.
- Feature Hasher can work on multiple features at one time. [Sklearn-FeatureHasher](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.FeatureHasher.html)
- Output:
  - In the most simple case (only 1 category feature per sample), the output is similar to one-hot, i.e. binary and only one '1' in each row, like [0,1,0,0]
    - But the dimension is usually smaller than one-hot encoding.
  - the output can also be non-binary, row, like [-1,-2,3,0]. Then, 
    - -2 or 3 means there are multiple i.e. 2 or 3 same features in a sample.
    - "-" also originates from a hash function to reduce collision.
    - Ref: [Wiki-Feature_hashing](https://en.wikipedia.org/wiki/Feature_hashing)


##### 1.2.3.3. Understanding

- Why hash function could be used here? (personal understanding):
  - Problem of one-hot is the too large dimension.
    - So our aim is to reduce dimension.
  - However, reduce dimension will certainly lose information.
    - So we want to lose info as less as possible.
  - Coincidently, considering good hash function also tends to minimize information loss 
    - collision = information loss in terms of the dataset.
  - So hash function should be a good tool for encoding.


- How the encoding looks like? (personal understanding): 
  - Hash function map arbitrary-size input (feature) to **fixed-size** values (hash values)
    - I.e. the dimension of index(output) values is fixed.
    - I.e. we need to assign the index values to different feature values.
  - Since there will usually be information loss (collisions).
    - One-hot is equivalent transformation, fewer dimension certainly lead to information loss.
    - Hash try to **minimize the collisions**, i.e. Hash minimize information loss in perspective of the dataset.
    - So the problem become how to assign hash values to different feature values.
  - The collision should be minimized with such assignment method/distribution:
    - **High frequency** feature values will be assigned with **distinct/unique** hash values.
    - **Low frequent** feature values will be **grouped** together and assigned to same hash values.
  - Unsolved Q: Can we use entropy to mathematically express such idea?



<!-- - Why it works (personal understanding):
  - The purpose of hash encoder over one-hot is to reduce dimension.
  - Reducing dimension will always loss information. However, we want to loss information as less as possible.
  -  good hash also seek to minimize information loss (minimize collision)
  - Hash encoder "minimize information loss" in such thought:
    - The more collision, the more information loss.
    - Thus, wish hash function, we try to minimize collision, i.e.:
      - High frequent samples are assigned to unique values and low frequent values are assigned to same values.
      - High frequent samples has low collision chance and low frequent values has higher collision chance.
      - The information loss in high frequent values is low and in low frequent values is high.
  - So the information loss over the dataset is minimized. -->
  

##### 1.2.3.4. Extension

- Binary encoding = one-hot encoding + hash encoding

##### 1.2.3.5. Differentiation: FeatureHasher vs. HashingVectorizer
- FeatureHasher: encode one- or multi-column features to vector. 
  - i.e. the input of FeatureHasher is categorical feature(s), 
    - the feature could be either text or numerical.
    - the text will be considered as a whole, i.e., different text will be considered as different categorical values.
  - Ref: [Sklearn-FeatureHasher](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.FeatureHasher.html)
- HashingVectorizer: encode text to vector.
  - i.e. the input of HashingVectorizer is text documents.
    - the feature is only one column of long text.
    - the text will be broken into word level (bag of words), and then vectorize it with feature hasher. 
      -  i.e. Hashing vectorizer $\approx$ bag-of-word + Feature Hasher
  - Ref: [Sklearn-HashingVectorizer](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.FeatureHasher.html)

#### 1.2.4. Mean (target) encoder

##### 1.2.4.1. Algorithm

Calculate the mean of label/target value for each distinct value in feature, use the mean value as the encoding for that distinct value.

Ref: [TowardsDataScience](https://towardsdatascience.com/why-you-should-try-mean-encoding-17057262cd0)


<div  align="center">
<img src=https://miro.medium.com/max/688/1*x0Oi4VHNk4zOrn9bmjjtuw.png style = "zoom:60%"> <img src=https://miro.medium.com/max/680/1*6qNsNbcvT40YUbo_QpvGtA.png style = "zoom:60%"></div>

##### 1.2.4.1. Properties

- Especially good for "high cardinality categorical feature"
  - cardinality of a feature: the number of distinct/unique values in a features
  - high cardinality feature: feature that has many distinct/unique values.
  - E.g. name of country/city, zip code, street-number, phone-number, product series-number, etc.
  - Ref: [Zhihu](https://zhuanlan.zhihu.com/p/26308272), [Relevant paper](http://helios.mm.di.uoa.gr/~rouvas/ssi/sigkdd/sigkdd.vol3.1/barreca.pdf)
- Pro vs Con:
  - Pro: feature encoding is linearly correlated (mean value) with the label, namely, feature is useful, not noise.
  - Cons: Easy to overfit, especially when there are very few sample for each distinct feature value.
    - Because the model may solely dependent on this feature.
    - How to solve?
      - Add random noise.
      - Use cross-validation.
      - Smoothing it with feature global (column) mean.
      - Ref: [Github blog](https://necromuralist.github.io/kaggle-competitions/posts/mean-encoding/#org95524d8)


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
    - E.g. Mixed effect model: not sure if it is a classical/typical method.(TBD):
      - Ref: [Walmart](https://www.1point3acres.com/bbs/thread-682274-1-1.html), [ResearchGate](https://www.researchgate.net/post/How-are-missing-data-handled-in-linear-mixed-effects-models), [christophe-pdf](http://christophe.genolini.free.fr/recherche/atelierATelecharger/atelier205_1_HJG.pdf)

### 2.2. Sparse value (TBD)

Sparse value means a lot of values are 0.

- Removing noisy/unimportant features
  - Manually
  - Automatically via model. E.g.
    - Lasso regularization
    - Reduce depth of tree models.
- Making it dense
  - With various linear/non-linear transformation
    - PCA
    - Feature hashing
    - Neural network (like word2vec)
  - With modeling
    - Clustering like K-means
      - high-dim data will be collapsed to a 1-dim label (centroid)

Ref: [KDnuggets-Blog](https://www.kdnuggets.com/2021/01/sparse-features-machine-learning-models.html)

Why sparse is good for deep learning? (TBD)

Ref: [Quora](https://www.quora.com/What-are-the-advantages-of-using-sparse-representation-in-machine-learning-especially-in-deep-learning-models)

- less memory
- compressible
- less computational resources
- less model complexity, more stable.



## Deep Dive: Hashing

- Hash function: A hash function is any function that can be used to <font color="#0000dd"> map data of arbitrary size (key) to **fixed-size** values (hash value)</font>. The process is called *Hashing*. [Wiki-Hash_function](https://en.wikipedia.org/wiki/Hash_function)
  - Collision: different keys are mapped to a same hash value.
  - A good hash function should satisfy:
    - fast to compute
    - minimize duplication of output values (collisions)
- Hash table: A structure can map key to values. Also called hash map.[Wiki-Hash_table](https://en.wikipedia.org/wiki/Hash_table)
  - Mechanism: $ \text{key} \underset{ \text{hash function} }{\rightarrow} {\text{index}} \rightarrow {\text{value}}$
    - Each value is stored in a bucket
    - Each bucket corresponds to an index
    - Key can be translated to index, via hash function, to retrieve the stored value.
    <div  align="center"><img src=https://upload.wikimedia.org/wikipedia/commons/thumb/7/7d/Hash_table_3_1_1_0_1_0_0_SP.svg/640px-Hash_table_3_1_1_0_1_0_0_SP.svg.png style = "zoom:40%"></div>
  - E.g. Python dict is a hash map.
  - Hash function is the way to create the index of the bucket.
