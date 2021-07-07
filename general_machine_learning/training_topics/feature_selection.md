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

## 4. Dimension Reduction Algos

SVD

PCA

## 5. Regularization

Regularization can reduce the effective number of features, is a kind of implicit feature selection.


## Deep Dive SVD (TBD)

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


### Eigenvalue, eigenvector

Ref: [Wiki-Eigenvalues_and_eigenvectors](https://en.wikipedia.org/wiki/Eigenvalues_and_eigenvectors)

- $\mathbf{A}\mathbf{u} = \lambda\mathbf{u}$ 
  - $\mathbf{u}$ is called eigenvector
  - $\lambda$ is called eigenvalue
  - How to interpret eigenvalue and eigenvector: 
    - After transformation, the points on eigenvector direction $\mathbf{u}$ will still be on the direction of eigenvector $\mathbf{u}$
    - $\mathbf{A}\mathbf{v} = \mathbf{A}\mathbf{(u_1 + u_2 + u_3 +...}) = \lambda_1\mathbf{u_1}+\lambda_2 \mathbf{u_2} + \lambda_3 \mathbf{u_3} +... $
    - Ref: [Zhihu](https://www.zhihu.com/question/400720726)
- 对实对称矩阵，不同特征值对应的特征向量互相正交
  - 其他举证，特征向量不一定有正交性质
