# Model comparison (Model selection?)

## 1. Model comparison

- LR (Logistic Regression) vs SVM
  - SVM is more fair on **boundary**.
      <div  align="center"><img src=https://cdn-images-1.medium.com/max/800/1*ru8OP1r2755a4B42AQEcnA.png style = "zoom:80%"></div>

    - SVM focuses on near-boundary points, not affected by high-confidence predictions (far-from-boundary points).
      - LR or linear regression may be affected by high-confidence predictions.
      - This also means, compare to LR, SVM is less affected by imbalanced data.
    - SVM have unique solution. 
      - Logistic regression or linear regression may have multiple optimized boundaries
    - **Detailed Explanation**: For LR or linear regression, we can have two strategy (kinds of metric) to get tune the parameter 
      - Most common strategy: optimize metrics based on the final binary result, like precision or recall.  Then, e.g., for linear-separable data, as long as the boundary stands in the gap, separate the data well, the score will be 100%. Thus, there will be multiple "optimized" boundaries.
      - Less common: optimize metrics based on the score given by the model, like likelihood. Then, the far-from-boundary points has non-zero contribution to the score and thus will affect the position of boundary.
  - SVM can be used for not-linear separable data, with kernel. LR doesn't.
  - LR has probability interpretation. SVM doesn't.
  - LR is strictly convex. SVM doesn't
  - Refs: [Medium blog](https://medium.com/@george.drakos62/support-vector-machine-vs-logistic-regression-94cc2975433f), [StackExchange](https://stats.stackexchange.com/questions/95340/comparing-svm-and-logistic-regression), [Blog-GDCoder](https://gdcoder.com/support-vector-machine-vs-logistic-regression/)

- LR (Logistic Regression) vs Linear regression.
  - The loss is different.

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

#### 2.2.3.Random Forest (TBD)

Bagging + subset of features (column random sampling)

##### 2.2.3.1. Pros

- Further decorrelates trees (relative to bagged trees)
  - important when dealing with mulitple features which may be correlated
- reduced variance (relative to regular trees)

##### 2.2.3.2. Cons

Not as easy to visually interpret

#### 2.2.4. Boosted Trees (TBD)

Similar to bagging, but learns sequentially and builds off previous trees

##### 2.2.4.1. Pros

- Somewhat more interpretable than bagged trees/random forest as the user can define the size of each tree resulting in a collection of stumps (1 level) which can be viewed as an additive model
- Can easily handle qualitative (categorical) features

##### 2.2.4.2. Cons

Unlike bagging and random forests, can overfit if number of trees is too large.


### 2.4. Naive Bayes  (TBD)

#### 2.4.1. Pro: 

- Model is simple
  - need less training data
- Convergent quicker than discriminative model.
- Model is simple, need less training data
- Computationally fast
- Simple to implement
- Works well with high dimensions
- 

#### 2.4.2. Cons:

- Relies on independence assumption and will perform badly if this assumption is not met
- Cannot learn interactions between features. But:
  - converge quicker than discriminative models like logistic regression
  - need less training data.

### 2.5. Neural Network (TBD)