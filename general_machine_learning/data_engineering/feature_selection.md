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


## 6. Deep Dive: SVD (TBD)

Terminology: "matrix factorization" = "matrix decomposition", i.e. transform a matrix to a product of multiple matrices. [Ref: Wiki](https://en.wikipedia.org/wiki/Matrix_decomposition)


$$\mathbf {M} =\mathbf {U\Sigma V^{*}}$$
- [Wiki-SVD](https://en.wikipedia.org/wiki/Singular_value_decomposition)
- $\mathbf{U}$, $\mathbf{V}$ is [unitary matrix](https://en.wikipedia.org/wiki/Unitary_matrix): $U^*U=I$
- $\mathbf{\Sigma}$ is a [Rectangular diagonal matrix](https://en.wikipedia.org/wiki/Diagonal_matrix#Rectangular_diagonal_matrices)
<!-- $$\begin{aligned}
    m_{ij} &= \sum_x u_{ix} (\sum_y\sigma_{xy}v_{yj}) \\ & = \sum_x u_{ix}\sigma_{xx}v_{xj} \leftarrow \Sigma {\text{ is diagonal}}
    \\ & = \vec{u}_i \cdot \vec{v}_j?
\end{aligned}$$ -->


Ref: [CSDN-blog-basics](https://blog.csdn.net/weixin_42575020/article/details/103504792), [CSDN-blog](https://blog.csdn.net/u010087338/article/details/114576013), [Zhihu-blog](https://zhuanlan.zhihu.com/p/36546367), [ScienceNet-Blog](http://blog.sciencenet.cn/blog-696950-699432.html) , [Zhihu-blog](https://zhuanlan.zhihu.com/p/36538284)

### 6.1. Understanding

### 6.2. Eigenvalue, eigenvector

Ref: [Wiki-Eigenvalues_and_eigenvectors](https://en.wikipedia.org/wiki/Eigenvalues_and_eigenvectors)

- $\mathbf{A}\mathbf{u} = \lambda\mathbf{u}$ 
  - $\mathbf{u}$ is called eigenvector
  - $\lambda$ is called eigenvalue
  - How to interpret eigenvalue and eigenvector: 
    - After transformation, the points on eigenvector direction $\mathbf{u}$ will still be on the direction of eigenvector $\mathbf{u}$
    - $\mathbf{A}\mathbf{v} = \mathbf{A}\mathbf{(u_1 + u_2 + u_3 +...}) = \lambda_1\mathbf{u_1}+\lambda_2 \mathbf{u_2} + \lambda_3 \mathbf{u_3} +... $
    - Ref: [Zhihu](https://www.zhihu.com/question/400720726)
- For [real symmetric matrix](https://en.wikipedia.org/wiki/Symmetric_matrix#Real_symmetric_matrices), the eigenvectors corresponding to different eigenvalues are orthogonal. Ref: [wiki-對稱矩陣#實對稱矩陣](https://zh.wikipedia.org/wiki/%E5%B0%8D%E7%A8%B1%E7%9F%A9%E9%99%A3#%E5%AF%A6%E5%B0%8D%E7%A8%B1%E7%9F%A9%E9%99%A3)
  - For other matrices, the eigenvectors are not necessarily orthogonal.


## 7. Deep Dive: Feature relation statistics

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