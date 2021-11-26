# Matrix Factorization

Matrix factorization, i.e., decompose matrix into to production of a few factor matrix, also called matrix decomposition.

## 0. Notation

- All vectors are column vector.
- Typically, we denote different types of matrix as:
  - $\mathbf{X}$: regular $m \times n$ matrix
  - $\mathbf{A}$: a symmetric matrix $m \times m$


## 1. Matrix factorization methods

Terminology: "matrix factorization" = "matrix decomposition", i.e. transform a matrix to a product of multiple matrices ("matrix factors"). [Ref: Wiki](https://en.wikipedia.org/wiki/Matrix_decomposition)

### 1.1. Non-negative matrix factorization (NMF)

#### 1.1.1. NMF

$$\mathbf{X}_{m \times n} \approx \mathbf{W}_{m \times k} \mathbf{H}_{k \times n}$$

- e.g. (The "AB" in the following figure refer to our $\mathbf{WH}$)
  <div  align="center"><img src=https://miro.medium.com/max/1838/1*Cdk8UXkHqkLxfPEFTNEU4A.jpeg style = "zoom:20%"></div>
  
  <!-- <div  align="center"><img src=https://predictivehacks.com/wp-content/uploads/2020/08/NMF.png style = "zoom:50%"></div> -->



Ref: [Wiki-NMF](https://en.wikipedia.org/wiki/Non-negative_matrix_factorization)

#### 1.1.2. NMF vs. SVD

- Sign of value:
  - NMF: only positive values, so NMF is typically used in RecSys, because the rating should always be positive.
  - SVD: no requirements on sign of value. Can be either positive or negative.
  - Ref: [Analyticsvidhya](https://discuss.analyticsvidhya.com/t/how-is-svd-different-from-other-matrix-factorization-techniques-like-non-negative-matrix-factorization/67519/2), [Quora](https://www.quora.com/What-is-the-difference-between-non-negative-matrix-factorization-and-singular-value-decomposition)



### 1.2. Singular value decomposition (SVD)

### 1.2.1. SVD

<!-- - 
- $\mathbf{U}$, $\mathbf{V}$ is [unitary matrix](https://en.wikipedia.org/wiki/Unitary_matrix): $U^*U=I$, $V^*V=I$
- $\mathbf{\Sigma}$ is a [Rectangular diagonal matrix](https://en.wikipedia.org/wiki/Diagonal_matrix#Rectangular_diagonal_matrices) -->
<!-- $$\begin{aligned}
    m_{ij} &= \sum_x u_{ix} (\sum_y\sigma_{xy}v_{yj}) \\ & = \sum_x u_{ix}\sigma_{xx}v_{xj} \leftarrow \Sigma {\text{ is diagonal}}
    \\ & = \vec{u}_i \cdot \vec{v}_j?
\end{aligned}$$ -->


<!-- SVD aims to decompose a $m \times n$ matrix. -->
$$\mathbf{X}_{m \times n} =\mathbf{U}_{m \times m} \mathbf{\Sigma}_{m \times n} (\mathbf{V}_{n \times n})^{T}$$
- $\mathbf{U}$, $\mathbf{V}$ is [unitary matrix](https://en.wikipedia.org/wiki/Unitary_matrix): $U^TU=I$, $V^TV=I$
  - $\mathbf{U}$, $\mathbf{V}$  is called left-, right- singular matrix
  - The columns $\mathbf{u}_i$, $\mathbf{v}_i$ in $\mathbf{U} = [\mathbf{u}_1...\mathbf{u}_m]$, $\mathbf{V}=[\mathbf{v}_1...\mathbf{v}_n]$ is called left-, right- singular vector.
- $\mathbf{\Sigma}$ is a [rectangular diagonal matrix](https://en.wikipedia.org/wiki/Diagonal_matrix#Rectangular_diagonal_matrices)
- e.g. (The "A" in the following figure refer to our $\mathbf{X}$)

    <div  align="center">
    <img src=https://img-blog.csdnimg.cn/20191212111401278.png style = "zoom:80%">
    </div>


Ref: [CSDN-blog-basics](https://blog.csdn.net/weixin_42575020/article/details/103504792), [CSDN-blog](https://blog.csdn.net/u010087338/article/details/114576013), [Zhihu-blog](https://zhuanlan.zhihu.com/p/36546367), [ScienceNet-Blog](http://blog.sciencenet.cn/blog-696950-699432.html) , [Zhihu-blog](https://zhuanlan.zhihu.com/p/36538284),[Wiki-SVD](https://en.wikipedia.org/wiki/Singular_value_decomposition)


### 1.2.1. Truncated SVD
$$\mathbf{X}_{m \times n} \approx \mathbf{U}_{m \times k} \mathbf{\Sigma}_{k \times k} (\mathbf{V}_{n \times k})^{T}$$

- Typically, in real application, we will order the singular values in descending order and then truncate it.
  - I.e. We keep the first $k$ principle components ($k$ axes that have largest variance.).
- e.g. (The "A" in the following figure refer to our $\mathbf{X}$)
  <!-- <div  align="center"><img src=https://www.researchgate.net/profile/Naji-Albatayneh/publication/322539357/figure/fig2/AS:583730137669638@1516183518861/Matrix-decomposition-using-truncated-SVD.png style = "zoom:100%"></div> -->
  <div  align="center"><img src=https://cdn.analyticsvidhya.com/wp-content/uploads/2018/09/Screenshot_7.png style = "zoom:80%"></div>

   
## 2. Matrix factorization applications


### 2.1. Principle component analysis (PCA)





#### 2.1.0. Definitions

Principle component: the direction has the largest variance.

**Note**: For PCA, we should first subtract the mean. Ref: [Wiki-PCA](https://en.wikipedia.org/wiki/Principal_component_analysis),[StackeExchange](https://stats.stackexchange.com/questions/94103/pca-on-non-centered-data), [Standford-U-stats](https://www.stat.cmu.edu/~cshalizi/uADA/12/lectures/ch18.pdf)

#### 2.1.1. Calculation of principle components from definition

From definition, The principle components can be calculated via maximize the variance in a direction.

The variance of dataset $\mathbf{X}$ on the direction of $\mathbf{w}$ is:

$$\begin{aligned}
Var_\mathbf{w}(\mathbf{X})& = \sum_{i=1}^m (\mathbf{x}_i\cdot\mathbf{w} - \mathbf{\bar{x}}\cdot\mathbf{w})^2 \\
& =  \sum_{i=1}^m [  (\mathbf{x}_i\cdot\mathbf{w})^2 ] \leftarrow \underset{\text{preprocess: subtract mean }} { \mathbf{\bar{x}}=0}  \\
& =  (\mathbf{X}\mathbf{w})^T(\mathbf{X}\mathbf{w})\\
& =  \mathbf{w}^T\mathbf{X}^T\mathbf{X}\mathbf{w}
\end{aligned} $$

The first component is calculated with:

$$\begin{aligned}
\mathbf{w}_1 & = \underset{\|\mathbf{w}\|=1}{\argmax} [ Var_\mathbf{w}(\mathbf{X})] \\
& =  \underset{\|\mathbf{w}\|=1}{\argmax}( \mathbf{w}^T\mathbf{X}^T\mathbf{X}\mathbf{w} )
\end{aligned} $$

The remaining components can be calculated iteratively with:

$$\mathbf {\hat {X}} _{k}=\mathbf {X} -\sum _{s=1}^{k-1}\mathbf {X} \mathbf {w} _{(s)}\mathbf {w} _{(s)}^{\mathsf {T}}$$

$$\mathbf {w} _{(k)}=\mathop {\operatorname {arg\,max} } _{\left\|\mathbf {w} \right\|=1}\left\{\left\|\mathbf {\hat {X}} _{k}\mathbf {w} \right\|^{2}\right\}=\arg \max \left\{{\tfrac {\mathbf {w} ^{\mathsf {T}}\mathbf {\hat {X}} _{k}^{\mathsf {T}}\mathbf {\hat {X}} _{k}\mathbf {w} }{\mathbf {w} ^{T}\mathbf {w} }}\right\}$$

Note: 
- The coordinate value of i-th sample $\mathbf{x}_i$ on j-th principle components ($\mathbf{w}_j$) is $x_{i,\mathbf{w}_j} = \mathbf{x}_i\cdot\mathbf{w}_j$. 
- Thus column vector formed from j-th principle component coordinate value of all samples $[x_{1,\mathbf{w}_j},..., x_{m,\mathbf{w}_j}]^T = \mathbf{X}\cdot \mathbf{w}_j$
- Thus, if we denote $\mathbf{W} = [\mathbf{w}_1,...\mathbf{w}_k]$, the coordinate values of samples in the coordinate system of principle components is $\mathbf{X_\mathbf{W}} = \mathbf{X}\mathbf{W}$



#### 2.1.2. Principle components and SVD

We can get principle components with SVD, the principle components are vectors in the right singular vector $\mathbf{V}$, 

Proof:

1. We can prove that, the solution for $\underset{\|\mathbf{w}\|=1}{\argmax}( \mathbf{w}^T\mathbf{X}^T\mathbf{X}\mathbf{w})$ is the eigenvector of $\mathbf{X}^T\mathbf{X}$
   - i.e. the principle components of $\mathbf{X}$, i.e. $\mathbf{W}$,are eigenvectors of $\mathbf{X}^T\mathbf{X}$
2. From Section DD-3, we can know, for SVD $\mathbf {X} =\mathbf {U\Sigma V^{T}}$, $\mathbf{V}=[\mathbf{v}_1, \mathbf{v}_2...]$ are exactly the eigenvectors of $\mathbf{X}^T\mathbf{X}=\mathbf{V}\mathbf{\Sigma}^T\mathbf{\Sigma}\mathbf{V}^T=\mathbf{V}\mathbf{\Lambda}\mathbf{V}^T$
3. Thus, we can know the principle components $\mathbf{W}$ is exactly the right singular vector $\mathbf{V}$ in SVD, i.e.: $\mathbf{W} = \mathbf{V}$

Notes:

- New coordinates values $\mathbf{X_{V}} = \mathbf{X}\mathbf{V}$
 - Also, coordinates values can be calculated as: $\mathbf{X_{V}} = \mathbf{X} = \mathbf {U\Sigma V^{T}}\mathbf{V} =  \mathbf {U\Sigma}$
- The order of principle components can be determined with the eigenvalues of $\mathbf{X}^T\mathbf{X}$, i.e. $\mathbf{\Lambda} = \mathbf{\Sigma}^T\mathbf{\Sigma}$
- This is because then, the i-th eigenvalue $\lambda_i$ in $\mathbf{\Lambda}$ is exactly the variance on the principle component $\mathbf{v}_i$, proof:
  - Since the principle components are orthonormal, i.e.: 
   $$\mathbf{v}_i\cdot\mathbf{v}_j = \begin{cases} 1 & i=j \\ 0 & i \neq j \end{cases}$$
  - Then, the variance on i-th principle components $\mathbf{v}_i$ is: 
   $$\begin{aligned}
       Var_\mathbf{v_i}(\mathbf{X}) & = \mathbf{v_j}^T\mathbf{X}^T\mathbf{X}\mathbf{v_i} 
       \\ &= \mathbf{v_i}^T\mathbf{V}\mathbf{\Lambda}\mathbf{V}^T\mathbf{v_i} \\ &= [0,...0,\underset{ \text{i-th position} }{1},0,...,0] \mathbf{\Lambda} [0,...0,\underset{ \text{i-th position} }{1},0,...,0]^T
       \\ &=\lambda_i
   \end{aligned}$$


<!-- - For PCA,
  - From [Standford-U-stats-Chp18.1.2](https://www.stat.cmu.edu/~cshalizi/uADA/12/lectures/ch18.pdf) or [Wiki-PCA](https://en.wikipedia.org/wiki/Principal_component_analysis), we can know the solution should be an eigenvector of $\mathbf{X}^T\mathbf{X}$.
  - Since $\mathbf{X}^T\mathbf{X}$ must be a symmetric matrix, should be able to be decomposed in the form: "$\mathbf{A} = \mathbf{U} \mathbf{\Lambda}\mathbf{U}^T$" where:
    - $\mathbf{U} = [u_1, u_2, u_3...]$ is a symmetric orthonormal matrix composed from eigenvectors.
    - $\mathbf{\Lambda} = \begin{bmatrix} \lambda_{1} & & \\
          & \ddots & \\
          & & \lambda_{m}
          \end{bmatrix}$ is a diagonal matrix with eigenvalues.

- For SVD,
  - $\mathbf {X} =\mathbf {U\Sigma V^{T}}$
  - Then $\mathbf{X}^T\mathbf{X} = \mathbf{V}^T \Sigma^T \Sigma \mathbf{V}^{T} = \mathbf{V} \mathbf{\Lambda} \mathbf{V}^{T}$ i.e. $\mathbf{V} \mathbf{\Lambda} \mathbf{V}^{T}$ is exactly the decomposition of $\mathbf{X}^T\mathbf{X}$
  - I.e. the orthonormal vectors in $\mathbf{V}$ is exactly the principal components. -->

<!-- - How to get the new coordinates in the principle-components-axis?
  - New coordinates values $\mathbf{X_{V}} = \mathbf{X}\mathbf{V} $
    - From $\mathbf {X} =\mathbf {U\Sigma V^{T}}$, 
- How to determine the order of those principle components?
  - Use the eigenvalue. Since the principle components are normalized, the value of the eigenvalue equal the variance on the principle component. -->


- Other Ref: [StackExchange](https://stats.stackexchange.com/questions/134282/relationship-between-svd-and-pca-how-to-use-svd-to-perform-pca)
- Other Ref - probabilistic interpretation: if we regard $\boldsymbol{X}_{m\times n} = [p(d_i, t_{j})]$ as a bag of word (term $t$) representation, then $\boldsymbol{X_V}_{m\times k} = \boldsymbol{XV} = [p(d_i, \tau_{z})]$ is a "bag of topics" representation. see "$\boldsymbol{A_V}$" in [latent_semantic_analysis.md](../../natural_language_processing/latent_semantic_analysis.md)

## Deep Dive 1: SVD-related linear algebra basics


### DD1.1. Eigenvalue and eigenvector


This part answer what is eignevector and its understanding. 
Ref: [Wiki-Eigenvalues_and_eigenvectors](https://en.wikipedia.org/wiki/Eigenvalues_and_eigenvectors)

- $\mathbf{X}\mathbf{u} = \lambda\mathbf{u}$ 
  - $\mathbf{u}$ is called eigenvector
  - $\lambda$ is called eigenvalue
  - How to interpret eigenvalue and eigenvector: 
    - After transformation, the points on eigenvector direction $\mathbf{u}$ will still be on the direction of eigenvector $\mathbf{u}$
    - $\mathbf{X}\mathbf{v} = \mathbf{X}\mathbf{(u_1 + u_2 + u_3 +...}) = \lambda_1\mathbf{u_1}+\lambda_2 \mathbf{u_2} + \lambda_3 \mathbf{u_3} +... $
    - Ref: [Zhihu](https://www.zhihu.com/question/400720726)
- For [real symmetric matrix](https://en.wikipedia.org/wiki/Symmetric_matrix#Real_symmetric_matrices), the eigenvectors corresponding to different eigenvalues are orthogonal. Ref: [wiki-對稱矩陣#實對稱矩陣](https://zh.wikipedia.org/wiki/%E5%B0%8D%E7%A8%B1%E7%9F%A9%E9%99%A3#%E5%AF%A6%E5%B0%8D%E7%A8%B1%E7%9F%A9%E9%99%A3)
- For other matrices, the eigenvectors are not necessarily orthogonal.


### DD1.2. Symmetric matrix and eigenvector

Then, how to use the eigenvector? 

For a symetric matrix, the matrix usually can be decomposed as:
- $\mathbf{A} = \mathbf{V} \mathbf{\Lambda}\mathbf{V}^T$ where: 
  - $\mathbf{V} = [v_1, v_2, v_3...]$ is a symetric orthonmal matrix composed from eigenvectors.
  - $\mathbf{\Lambda} = \begin{bmatrix} \lambda_{1} & & \\
        & \ddots & \\
        & & \lambda_{m}
        \end{bmatrix}$ is a diagonal matrix with eigenvalues.
- How to solve symmetric matrix? Find its eigenvectors and eigen

### DD1.3. SVD and symmetric matrix

<!-- - Similar to this decomposition, SVD aims to decompose a $m \times n$ matrix.
  - $\mathbf{X}_{m \times n} =\mathbf{U}_{m \times m} \mathbf{\Sigma}_{m \times n} (\mathbf{V}_{n \times n})^{T}$
    - $\mathbf{U}$, $\mathbf{V}$ is [unitary matrix](https://en.wikipedia.org/wiki/Unitary_matrix): $U^*U=I$, $V^*V=I$
    - $\mathbf{\Sigma}$ is a [Rectangular diagonal matrix](https://en.wikipedia.org/wiki/Diagonal_matrix#Rectangular_diagonal_matrices)
    - e.g. (The "A" in the following figure refer to our $\mathbf{X}$)

    <div  align="center">
    <img src=https://img-blog.csdnimg.cn/20191212111401278.png style = "zoom:80%">
    </div> -->

With SVD, we know:

$$\mathbf {X} =\mathbf {U\Sigma V^{*}}$$

Then how to solve out $\mathbf{U}$, $\mathbf{V}$? Using the property of symmetric matrix:

- Construct a matrix $\mathbf{A'} =\mathbf{X}^T\mathbf{X}$, we can prove the matrix is symmetric: 
   - $A'_{ij} = \sum_k (X^T)_{ik}X_{kj} = \sum_k X_{ki}X_{kj} = \sum_k (X^T)_{jk}X_{ki} = A'_{ji}$
- With SVD, we can express $\mathbf{A'}$ as: $\mathbf{A'} = \mathbf{X}^T\mathbf{X} = \mathbf{V}\mathbf{\Sigma}^T\mathbf{\Sigma}\mathbf{V}^T$
- Denote $\mathbf{\Lambda'} = \mathbf{\Sigma}^T\mathbf{\Sigma}$,   we have $\mathbf{A'} = \mathbf{V} \mathbf{\Lambda'} \mathbf{V}^T$ 
  - $\mathbf{\Lambda'} $ is diagonal, proof:
    - $\because \Sigma$ is diagonal, i.e. $\Sigma_{ij} = \begin{cases} 1 & i=j\\0 & i \neq j \end{cases}$
    - $\therefore$ $\Sigma_{ki}\Sigma_{kj} = \begin{cases} 1 & i=j=k\\0 & otherwise \end{cases}$
    - $\therefore \Lambda'_{ij} = \sum_k (\Sigma^T)_{ik}\Sigma_{kj}= \sum_k \Sigma_{ki}\Sigma_{kj} =  \begin{cases} 1 & i=j\\0 & i \neq j \end{cases}$
- Given "symmetric matrix $\mathbf{A}$ can be eigen-decomposed as $\mathbf{A}=\mathbf{U} \mathbf{\Lambda}\mathbf{U}^T$, where $\mathbf{\Lambda}$ is a *diagonal* matrix of eigenvalues and $\mathbf{U}$ is a matrix of eigenvectors." We could know:
  - Here, $\mathbf{A'} = \mathbf{V} \mathbf{\Lambda'} \mathbf{V}^T$ could be an eigendecomposition
- Thus, $\mathbf{V} = [\mathbf{v}_1,\mathbf{v_2},....]$, where $\mathbf{v_i}$ is the eigenvector of the symmetric matrix $\mathbf{A} =\mathbf{X}^T\mathbf{X} $

Similarly, construct $\mathbf{A'} =\mathbf{X}\mathbf{X}^T =  = \mathbf{U}\mathbf{\Sigma}\mathbf{\Sigma}^T\mathbf{U}^T$, and then we can know $\mathbf{U} = [\mathbf{u}_1,\mathbf{u_2},....]$ where $\mathbf{u_i}$ is the eigenvector of the symmetric matrix $\mathbf{A'} =\mathbf{X}\mathbf{X}^T $
- Ref: [CSDN-blog](https://blog.csdn.net/weixin_42575020/article/details/103504792), [Wiki-SVD](https://en.wikipedia.org/wiki/Singular_value_decomposition)

## Deep Dive 2: Properties of real symmetric matrix

Ref: [CSDN-blog](https://blog.csdn.net/Banach_I/article/details/51078451)

### DD2.1. All eigenvalues are real number.

Proof: Ref: [CSDN-blog](https://blog.csdn.net/Banach_I/article/details/51078451)

### DD2.2. The eigenvectors with different eigenvalues are orthogonal

Ref: [CSDN-blog](https://blog.csdn.net/Banach_I/article/details/51078451)

**Proof:**

Let: $\mathbf{A}\mathbf{u_1} = \lambda_1\mathbf{u_1}, \mathbf{A}\mathbf{u_2} = \lambda_2\mathbf{u_2}$

$\because$ symetric matrix is always square, i.e. num_row = num_col
$\therefore$ we could construct such product: $\xi = \mathbf{u_2}^T A \mathbf{u_1} = \mathbf{u_2}^T \lambda_1 \mathbf{u_1} = \lambda_1\mathbf{u_2}^T \mathbf{u_1}$

$\because A=A^T$ (symmetric)
$\therefore\xi = \mathbf{u_2}^T A \mathbf{u_1} = \mathbf{u_2}^T A^T \mathbf{u_1} = [\mathbf{A}\mathbf{u_2}]^T \mathbf{u_1}= [\lambda_2\mathbf{u_2}]^T \mathbf{u_1}= \lambda_2\mathbf{u_2}^T \mathbf{u_1}$

$\begin{aligned}
\therefore & \lambda_1\mathbf{u_2}^T \mathbf{u_1}  = \lambda_1\mathbf{u_2}^T \mathbf{u_1}\\
\Rightarrow & (\lambda_1 - \lambda_2)\mathbf{u_2}^T \mathbf{u_1}=0\\
\Rightarrow & \lambda_1 - \lambda_2 = 0 {\text{ or }} \mathbf{u_2}^T \mathbf{u_1} = 0
\end{aligned}$
Proved.

<!-- $\mathbf{u_1}^T A \mathbf{u_2} = \mathbf{u_1}^T \lambda_2 \mathbf{u_2} = \lambda_2\mathbf{u_1}^T \mathbf{u_2}$ -->

### DD2.3. Eigen-decomposition A = $U\Sigma U^T$

For any diagonalizable matrix, the matrix can be eigen-decomposed:

- $\mathbf{D}=\mathbf{Q}\mathbf{\Lambda}\mathbf{Q}^{-1}$
  - $\mathbf{Q}$ is the square $n \times n$ matrix whose i-th column is the eigenvector $q_i$ of $\mathbf{A}$, 
  - $\Lambda$ is the diagonal matrix whose diagonal elements are the corresponding eigenvalues $\lambda_i$

Ref: [Wiki-Eigendecomposition_of_a_matrix](https://en.wikipedia.org/wiki/Eigendecomposition_of_a_matrix)

For real-symmetric matrix:

- $\mathbf{A} = \mathbf{V} \mathbf{\Lambda}\mathbf{V}^T$ where: 
  - $\mathbf{V} = [\mathbf{v}_1, \mathbf{v}_2, \mathbf{v}_3...]$ is a symetric orthonmal matrix composed from eigenvectors.
  - $\mathbf{\Lambda} = \begin{bmatrix} \lambda_{1} & & \\
        & \ddots & \\
        & & \lambda_{m}
        \end{bmatrix}$ is a diagonal matrix with eigenvalues.

**Proof:** Ref: [CSDN-blog](https://blog.csdn.net/Banach_I/article/details/51078451)
<!-- It doesn't matter we normalize all the eigenvector, i.e., $\mathbf{u}_i^T\mathbf{u}_i = 1$ And use these vectors to construct $\mathbf{U} = [\mathbf{u_1},..,\mathbf{u_m}]$

Plus that A is symmetric, these eigenvectors are orthogonal. We have:
$\mathbf{u_j}^T\mathbf{u_i} = \begin{cases}
1 & i=j\\0 & i \neq j  
\end{cases}$

Therefore, we have $\left\{ \begin{aligned} \mathbf{U}^T\mathbf{A}\mathbf{U}= \mathbf{\Sigma} \\ U^TU = I \end{aligned} \right.$ -->

### DD2.4. There are and only are k linear-independent eigenvectors.
Proof: Ref: [CSDN-blog](https://blog.csdn.net/Banach_I/article/details/51078451)


## Deep Dive 3. Solve SVD/PCA

### 3.1. Power iteration

#### 3.1.1 Power iteration to get 1 principle axis $v_1$

- Given diagonalizable matrix $\boldsymbol{A}$ (for PCA $ \boldsymbol{A}= \boldsymbol{X}^T\boldsymbol{X}$), power iteration will return the largest eigenvalue $\lambda_1$ and the corresponding eigenvector $\boldsymbol{v}_1$.


- Algorithm:
  - Power iteration starts with random vector $b_0$, 
  - iteration the following until eigenvalue $\mu _{{k}}$ converge:
    - $\boldsymbol{b}_{k+1}={\frac {A\boldsymbol{b}_{k}}{\|A\boldsymbol{b}_{k}\|}}$
    - $\mu _{{k}}={\frac  {\boldsymbol{b}_{{k}}^{{*}}A\boldsymbol{b}_{{k}}}{\boldsymbol{b}_{{k}}^{{*}}\boldsymbol{b}_{{k}}}}$
  - then $\boldsymbol{v}_1 = \boldsymbol{b}_k$, and $\lambda_1 = \mu_k$
    - Note: The eigen value is the diagonal element in $\boldsymbol{\Lambda}$, the eigen vector is the column vector of $\boldsymbol{V}$.

- Ref:
  - [Wiki-Power_iteration](https://en.m.wikipedia.org/wiki/Power_iteration): definition
  - [TowardsDataScience](https://towardsdatascience.com/simple-svd-algorithms-13291ad2eef2): implementation

#### 3.1.2. Algorithm to get all principle axes $V$

- Algorithm
  - Get the first principle axis (eigenvector) $\boldsymbol{v}_1$ and diagonal element (eigenvalue) $\lambda_1$ of matrix $\boldsymbol{A_1}:=\boldsymbol{A} = \boldsymbol{X^T}\boldsymbol{X}:=\boldsymbol{X}^T_1\boldsymbol{X}_1$
  - Iterate the following process:
    - $[\boldsymbol{X}_{k+1}]_{m\times n} = [\boldsymbol{X}_{k}]_{m\times n} - [[\boldsymbol{X}_{k}]_{m\times n}[\boldsymbol{v}_k]_{n\times 1}]_{m \times 1}[\boldsymbol{v}_k^T]_{1\times n}$ 
      - $[[\boldsymbol{X}_{k}]_{m\times n}[\boldsymbol{v}_k]_{n\times 1}]_{m \times 1}$ is the project value of different i-th sample $\boldsymbol{x}_{(k),i}$ on the principle component $\boldsymbol{v}_k$
      - i.e. $\boldsymbol{x}_{(k+1),i} = \boldsymbol{x}_{(k),i} - v_{(k),i} \boldsymbol{v}^T_k$
        - where $v_{(k),i} := \boldsymbol{x}_{(k),i}\boldsymbol{v}_k$ is the projection value (coordinate value in the new $\boldsymbol{V}$ coordinates system.)
        - Note: $\boldsymbol{x}$ is row vector, $\boldsymbol{v}$ is col vector
  - Get the $\boldsymbol{V}$ and $\boldsymbol{\Lambda}$:
    - $\mathbf{V} = [\mathbf{v}_1, \mathbf{v}_2, \mathbf{v}_3...]$ is a symetric orthonmal matrix composed from eigenvectors.
    - $\mathbf{\Lambda} = \begin{bmatrix} \lambda_{1} & & \\
          & \ddots & \\
          & & \lambda_{m}
          \end{bmatrix}$ is a diagonal matrix with eigenvalues.

- Ref:
  - [Stanford-Slides](http://theory.stanford.edu/~tim/s15/l/l8.pdf):application


## Interview Qs:

### Included in above:

- What is PCA
  - Usually used to reduce dimension. Project to data to different principle components.
  - Principle components is the direction have largest variance.

- Algo to get the first component.
  - Power iteration:
  

- Algo to get all components.
  - Use power iteration to get the direction of the 1st principle component.
  - Subtract the samples' projection on the first component.
  - Use power iteration to get the direction of 2nd principle component.
  - ...iterate the process until k.

### Not included in above:

- Why do we maximize variance during PCA? (Intuitive understanding.)
  - Variance is a metric of the "variability" of your data. Other direction are more flat than the principle component. So the principle component is the direction we keep the most info.
    - Ref[StackOverflow](https://stackoverflow.com/questions/12395542/why-do-we-maximize-variance-during-principal-component-analysis)
  - My understanding: we often use $R^2 = \frac{Var_{explained}}{Var_{total}}$ to measure the quality of fitting, the more variance our fitting line take (the less residue variance), the more comprehensive info/pattern in data our fitting line can get. So the variance is proportional the the amount of info. The principle axis have the largest amount of info.