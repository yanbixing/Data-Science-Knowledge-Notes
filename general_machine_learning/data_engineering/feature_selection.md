# Feature selection

## 1. Naive (heuristics)

Use background knowledge to select/engineer features.

## 2. Best subset selection

Choose the best/top k features from the total n features. The "best"/"top" is in terms of certain statistics.

- Information gain (mutual information) of the feature.
- Correlation between the feature and the label.

Note: the k value can be determined via elbow method:
<div  align="center"><img src=./feature_selection_asset/best_subset_selection_elbow_method.png style = "zoom:20%"></div>


## 3. Stepwise Selection

Incrementally add/subtract features until model performance stabilizes.

- Algorithm (Forward):
  - Add each feature to current subset
  - Find the best and add it to the current subset 
  - Stop will meet some criteria

Backward: similar to the above process, start from full features and reduce one by one. Also call recursive feature elimination. 

Ref [Coursera](https://www.coursera.org/lecture/ml-regression/complexity-of-the-greedy-forward-stepwise-algorithm-RRah3)

## 4. Dimension Reduction Algos (?)

- SVD: SVD will extract the latent factors in row/column of X.
  - $\mathbf{X}_{m \times n} =\mathbf{U}_{m \times k} \mathbf{\Sigma}_{k \times k} (\mathbf{V}_{n \times k})^{T}$
    - $\mathbf{U}_{m \times k}$: m sample with k elements, corresponds to the <mark style="background-color:yellow;">latent factors of each **row**</mark> in $\mathbf{X}$. $\mathbf{U}$ is usually orthonormal (orthogonal+normalized).
    - $\mathbf{\Sigma}_{k \times k}$: a diagonal matrix that hold singular values in descending order. The singular values is the weight that <mark style="background-color:yellow;"> how much the **latent factor** **contributes to the matrix** $\mathbf{X}_{m \times n}$ </mark>.
    - $\mathbf{V}_{n \times k}$: is a matrix of singular vectors, each row corresponds to the <mark style="background-color:yellow;"> latent factor of each **column** </mark> in $\mathbf{X}$. $\mathbf{V}$ is orthonormal.
  - A dimension-reduced matrix can be created via $\mathbf{X'}_{m \times k} = \mathbf{X}_{m \times n} \mathbf{V}_{n \times k}$.

- PCA: PCA will keep the direction with greatest variance axes as principle components (axis).
  - I.e. PCA will eliminate the direction with smallest variance.
  - PCA technique can use SVD to get principle components.

## 5. Regularization

Regularization can reduce the effective number of features, is a kind of implicit feature selection. (It softly restrict the scope of the weight.)

Typically useful when dimension is very high, hard to reduce it manually (rely on metric) or determine exact dimension we want.




## Deep Dive: Feature relation statistics

- Correlation coefficient:
  - Pro: 
    - give positive/negative dependence
    - intuitive (easy to understand)
  - Con:
    - just measure linear relationship, cannot give non-linear info.
    - Naturally works on numerical data, hard to work on categorical data.
- Mutual information (information gain):
  - Pro:
    - Give relation between feature even regardless linear or non-linear
    - Work naturally with categorical data.
    - Note: the behind mechanism of above advantage is because mutual info it is probability metric, so don'care about linear/non-linear, numerical/categorical
  - Cons:
    - Not so intuitive as correlation
    - Usually need additional engineering (binning/grouping) for continuos(numerical) input.
  - Ref: [Entropy_related_stats Sec 2.1. Mutual_info](../math_topics/entropy_related_statistics.md)