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




## Deep Dive 1: Feature relation statistics

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


## Deep Dive 2: "Big P, Little N" (p>>n) problem

"Big P, Little N" (p>>n) problem refers the case that: 

- number of features >> number of data points

Ref: [MLMastery - blog](https://machinelearningmastery.com/how-to-handle-big-p-little-n-p-n-in-machine-learning/)

### DD2.1. Definition

- predictors := features, typically the columns in a dataset
  - number of predictors
- n := number of data, typically the rows in a dataset 
  - number of samples
- Ref: [MLMastery - blog](https://machinelearningmastery.com/how-to-handle-big-p-little-n-p-n-in-machine-learning/)


### DD2.2. Influence

The model (solution of coefficients) does <mark style="background-color:yellow;"><font color="#0000dd">**NOT** have a **unique** solution</font></mark>. Certainly, can be called <mark style="background-color:yellow;"><font color="#0000dd">"overfitting"</font></mark>, because the model is too flexible and there is a high variance error.

- E.g. 3 dimension and there are only two data points
  - Model: $y = ax_1 + bx_2+cx_3$
  - Data: $(x_1,x_2,x_3,y) = [(1,1,1,1),(1,2,1,3)]$
  - Result in:
    - Any (a,b,c) satisfy $b=2, a+c=-1$ works, there $\infty$ number of solutions. 
    - The model will result in high variance error, i.e. overfit.

- Ref: 
  - [StackExchange](https://stats.stackexchange.com/questions/223486/modelling-with-more-variables-than-data-points): "no unique solution"
  - [Quora](https://www.quora.com/Is-it-possible-to-train-a-machine-learning-model-if-there-are-more-features-than-samples-in-the-data-set): example
  - [StackExchange](https://datascience.stackexchange.com/questions/11390/any-rules-of-thumb-on-number-of-features-versus-number-of-instances-small-da): some thumb-of-rules of model complexity, TBD

### DD2.3. Solution

Of course the solution will be:

- increase data
- reduce number of features (feature selection)
  - feature elimination: best subset, step wise selection
  - feature combination: PCA/SVD
  - "soft" selection: regularization
    - For clustering algorithm (in "curse of dimensionality" problem), the regularization can be like AIC/BIC, the punishment on number of centroids.
- Ref: 
  - [Medium](https://medium.com/@jennifer.zzz/more-features-than-data-points-in-linear-regression-5bcabba6883e): typical solutions.
  - [Analyticsindiamag - Blog](https://analyticsindiamag.com/curse-of-dimensionality-and-what-beginners-should-do-to-overcome-it/): this post seems confuse COD vs "P>>N"? Anyway, they are similar.


### DD2.4. Similar concept: "Curse of Dimensionality"

A similar problem to "P>>N" problem is called "Curse of Dimensionality". Ref: 

**Curse of Dimensionality:** As dimension increases, the space volume increase exponentially, data become more sparse (dissimilar, hard to use clustering).

- English: for **high dimensional** data, **everything is separated** (dissimilar) **hard to apply clustering**.
- Solution: solution on COD is exactly similar to "P>>N", i.e., dimension reduction.

Difference:

- "Large P, Little N" problem: focus on the unstability/overfitting of general mode.
- "Curse of Dimensionality" focus on sparsity of data and clustering problem. I.e.:


- Ref:
  - [TowardsDataScience](https://towardsdatascience.com/the-curse-of-dimensionality-50dc6e49aa1e): COD bad for clustering
  - [Wiki-COD](https://en.wikipedia.org/wiki/Curse_of_dimensionality): increase volume.
  - [Analyticsindiamag - Blog](https://analyticsindiamag.com/curse-of-dimensionality-and-what-beginners-should-do-to-overcome-it/): Increase volume.
  - [DeepAI-Post](https://deepai.org/machine-learning-glossary-and-terms/curse-of-dimensionality): visualization of COD on clustering.
  - [StackExchange](https://stats.stackexchange.com/questions/10423/number-of-features-vs-number-of-observations): this post seems confuse COD vs "P>>N"? Anyway, they are similar.

