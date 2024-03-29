# Model comparison (Model selection?) (TBD)

感觉是不是可以归结为几个不同的方面 (TBD)

- interpretable?
  - probability interpretation?
  - Model easy to interpret?
- Resources consumption?
  - time complexity?
    - Easy to train or not?
  - Scaling?
    - Whether can find to large data?
- Accuracy?
  - Boundary complexity?
    - Can fit non-linear boundary or not?
- Stability/Robust
  - Able to handle imbalanced data or missing values?



## 1. Model details

### 1.1. Linear models

- SVM vs LR (Logistic Regression)
  - SVM is more fair on **boundary**: There are infinity hyperplanes to separate the classes, SVM have the most proper from the intuition/geometric understanding.
      <div  align="center"><img src=https://cdn-images-1.medium.com/max/800/1*ru8OP1r2755a4B42AQEcnA.png style = "zoom:80%"></div>

    - SVM focuses on near-boundary points, not affected by high-confidence predictions (far-from-boundary points).
      - LR or linear regression may be affected by high-confidence predictions.
      - This also means, compare to LR, SVM is less affected by imbalanced data.
    <!-- - SVM have unique boundary solution
      - Logistic regression or linear regression may have multiple optimized boundaries. -->
    - **Detailed Explanation**: For LR or linear regression, we can have two strategy (kinds of metric) to get tune the parameter 
      - Most common strategy: optimize metrics based on the final binary result, like precision or recall.  Then, e.g., for linear-separable data, as long as the boundary stands in the gap, separate the data well, the score will be 100%. Thus, there will be multiple "optimized" boundaries.
      - Less common: optimize metrics based on the score given by the model, like likelihood. Then, the far-from-boundary points has non-zero contribution to the score and thus will affect the position of boundary.
  - SVM can be used for not-linear separable data, with kernel. LR doesn't.
  - LR has probability interpretation. SVM doesn't.
  - LR is strictly convex. SVM is convex, not strictly convex. 
    - See:[math_topics/convex_optimization.md](../math_topics/convex_optimization.md)
  - Refs: [Medium blog](https://medium.com/@george.drakos62/support-vector-machine-vs-logistic-regression-94cc2975433f), [StackExchange](https://stats.stackexchange.com/questions/95340/comparing-svm-and-logistic-regression), [Blog-GDCoder](https://gdcoder.com/support-vector-machine-vs-logistic-regression/)

- LR (Logistic Regression) vs Linear regression.
  - The loss is different.



### 1.2. Clustering methods

- K-means:
  - 
- DBSCAN:
  - 
- GMM



## 2. Pro/Con Summary

### 2.1. Linear Models

#### 2.1.1. Linear Regression (TBD)


#### 2.1.2. Logistic Regression

##### 2.1.2.1. Pro

- Low model variance. 
  - Low chance for overfitting.
  - Since The model is too simple.
- Provide probability interpretation.
- works well with diagonal (not-parallel-to-feature) decision boundary.
  - Trees only splits the feature, i.e. the boundary is parallel to feature axis.
- Note: LR can also use "kernel" to produce non-linear boundary, as Kernel LR (KLR), i.e. replace $\boldsymbol{\theta}^T\boldsymbol{x}$ with $f(x)$, but this kernel is different from the kernel in SVM, 
  - SVM specify to replace inner product, 
  - Ref [Standford U](https://web.stanford.edu/~hastie/Papers/svmtalk.pdf)

##### 2.1.2.2 Con
- High bias. Hard to fit data with non-linear boundary.

#### 2.1.3. SVM (TBD)

##### 2.1.3.1. Pros

- Performs similarly to logistic regression when linear separation
- Performs well with non-linear boundary depending on the kernel used
- Handle high dimensional data well

##### 2.1.3.2. Cons

- Susceptible to overfitting/training issues depending on kernel

### 2.2. Tree models

Ref: [Medium-blog](https://medium.datadriveninvestor.com/random-forest-pros-and-cons-c1c42fb64f04)

General pros vs other models:

- Can be used for both classification or regression tasks.
  - While logistic/SVM can only handle classification.
- Naturally can be used for multi-label classification
  - While logistic/SVM need to use one-vs-all.
- Can be used on both categorical and numerical feature.
- Implicitly perform feature selection, i.e. not split on non-important feature.
- Not influenced by outliers far from boundary ('deep' outliers, my terminology)
  - SVM and logistic regression will give huge punishment to those 'deep' outliers, the boundary will be dragged towards those outliers to reduce punishment/loss.
  - Tree only cares about entropy/'degree-of-mixing', 'deep' outliers will have same influence with 'shallow' outliers.
- Tree are good at handle complex non-linear relationship
  - non-kernel/simple svm or logistic model only handle linear-separable data.
- Tree are easily to balance variance-tradeoff (by controls the depth/min_samples), no need to re-train, can just prune or grow, as each step is greedy. 
  - SVM or logistic when change model (regularization, input features), usually need to re-train.

General cons vs other models:

- More expensive to handle tilt boundary. Since it only split features, i.e. the boundary is perpendicular/vertical to axis.


#### 2.2.1. Decision tree


##### 2.2.1.1. Pros

- Easy to interpret (visualize) and implement (only if-else)
- Can handle both numerical and categorical data
- Performs well on large dataset.
  - Why? Fast? Can have Complex boundary.
  - Complexity of prediction = depth of the tree
- No need feature engineering
  - Can handle categorical/text features as is  (?)
  - Automatically handle non-linearities and interactions need very deep model.

##### 2.2.1.2. Cons:

- Prone to overfitting (Depth)
- Not global optimum
  - The algorithm is greedy, information gain is maximized locally not globally.
- Unstable
  - Sensitive to where it/how the nodes are split. Small changes in variable and sample may result in different tree structure.
- Not good at imbalanced data? (Why?)
- Not good at diagonal decision boundary

#### 2.2.2. Bagged Trees (TBD)

Train multiple trees on different bootstrap sample set.

##### 2.2.2.1. Pros

- reduces variance in comparison to regular decision trees
- Can provide variable importance measures
  - classification: Gini index
  - regression: RSS
- Can easily handle qualitative (categorical) features
- Out of bag (OOB) estimates can be used for model validation

##### 2.2.2.2. Cons

- Not as easy to visually interpret.
- Not reduce variance if the features are correlated

#### 2.2.3.Random Forest

Ref: [Medium blog](https://medium.datadriveninvestor.com/random-forest-pros-and-cons-c1c42fb64f04)

Bagging + subset of features (column random sampling)

##### 2.2.3.1. Pros

- Further decorrelates trees (relative to bagged trees)
  - important when dealing with multiple features which may be correlated
- reduced variance (relative to regular trees)



##### 2.2.3.2. Cons

- Not as easy to visually interpret
- More like "black box", have little control on model training process (random sample random feature).

Other general cons:

- Computationally intensive for large datasets (A lot of different models)


#### 2.2.4. Boosted Trees (TBD)

Similar to bagging, but learns sequentially and builds off previous trees

##### 2.2.4.1. Pros

- Somewhat more interpretable than bagged trees/random forest as the user can define the size of each tree resulting in a collection of stumps (1 level) which can be viewed as an additive model
- Can easily handle qualitative (categorical) features

##### 2.2.4.2. Cons

Unlike bagging and random forests, can overfit if number of trees is too large.

##### 2.2.4.3. Comparison

- Compared to Bagging or RF: more suitable to fit stable data.
  - Noisy data is easy to have high variance error (model fit too much on detail/noise), so should focus on reduce variance 
  - Stable data is easy to have high bias error (mode is not complex enough to fit the shape), so should focus on reduce bias.
- Sequentially trained, slower.
  - Bagging can be parallelized, much faster.


### 2.4. Naive Bayes  (TBD)

#### 2.4.1. Pro: 

- Model is simple
  - need less training data
- Convergent quicker than discriminative model.
- Model is simple, need less training data
- Computationally fast
- Simple to implement
- Works well with high dimensions

#### 2.4.2. Cons:

- Relies on independence assumption and will perform badly if this assumption is not met
- Cannot learn interactions between features. But:
  - converge quicker than discriminative models like logistic regression
  - need less training data.
- Not perform well on imbalanced data, because the predicted probability is proportional to the prior probability of a label.
  - Ref: [StackOverflow](https://stackoverflow.com/questions/42791002/is-naive-bayes-sensitive-to-the-number-of-training-observations)

### 2.5. Neural Network (TBD)


## 3. Model comparison (TBD)

### 3.1. Classifers

- SVM vs logistic
  - SVM boundary more fair, more generalized. 
  - SVM's boundary only affected by near boundary data.
  - While logistic affected by points far from boundary, sensitive to imbalance.
  - SVM can be used for non-linear (Kernel); LR hard.
LR have probabilistic interpretation, SVM not.

- Tree vs SVM/logistic
  - Tree both regression and classification, SVM/logistic only regression
  - Tree can naturally handle multi-class (node majority), SVM/logistic need to use one-vs-all
  - Tree easy to handle non-linear data, SVM/logsitic need kernal or feature engineering
    - Tree no need feature engineering, can direct split on space.
    - However, SVM/logistic can handle tilt boundary, tree is more expensive, since tree split features, boundary perpendicular to axis
  - Tree just care about mixing status of sample on both sides, don't care about the distance (especially misclassified sample/outlier) to the boundary, while distance to boundary affect logistic and slightly affect 

- Bagging/Boosting vs DT:
  - Harder to interpret than DT, more like black-box
  - More accurate, can handle more complex data.
    - DT easy to overfit, while Bagging/Boosting not. (reduce variance / start from low var.)
  - DT unstable, sensitive to node splits, while Bagging/Boosting not (ensemble)

- Bagging vs Boosting:
  - Bagging: 
    - Can be parallelized, faster
      - The ensemble is independent.
    - reduce variance, better for noisy data
      - Noisy data should reduce variance, or model will fit noise.
    - The training set (samples) is different for each batch
  - Boosting: 
    - Sequentially grow, slower
      - The ensemble is not independent.
    - reduce bias, better for confident data.
      - For no noise data, should reduce bias, or model cannot be complex enough to fit the data.
    - The training set (samples) is same for each batch, but sample weight/label-value may change
      - AdaBoost: The weight change
      - GradientBoost: The label value changes.

- RF vs Bagging: further decorrelate the tree with column sampling
- Ada vs Gradient boosting:
  - Ada reweigh sample according to loss
  - Gradient directly train on the sample on the pseudo residue calculated from loss.
    - Pseudo residue = negative gradient of loss
    - Usually more efficient because directly train on pseudo residue.
    - Note: 
      - for classification task, we can also use the idea, understand label as probability,
      - The probability of true label is 1, the other label is 0, and the output we give is the probability of the sample


		
- Naïve bayes
  - Pro: 
    - Easy to understand, 
    - Simple
    - Fast
    - Computational cheap
  - Cons: 
    - Assume feature is independent, which is not true
      - Cannot learn interaction between features
    - Sensitive to imbalance (there is a class prior in bayes theorem)
      - Although not proportional to, because there is an argmax learning process, but do affect the prediction

### 3.2. Clustering

- GMM vs K-means
  - GMM an advanced version of k-means
  - Can give probability interpretation, not just one label but prob for all labels.
  - Not spherical, eclipse, can fit more complex data.
- DBSCAN vs K-means
  - Pro:
    - Arbitrary shape
    - No need determine K
    - Not affected by initialization
    - Usually faster
  - Cons:
    - No centroid concept
    - Sensitive to scale
    - Take more storage space (need to remember all points like KNN)

