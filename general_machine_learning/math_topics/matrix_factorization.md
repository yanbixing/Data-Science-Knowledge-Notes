# Matrix Factorization

Matrix factorization, i.e., decompose matrix into to production of a few factor matrix, also called matrix decomposition.

## 0. Notation

- All vectors are column vector.
- Typically, we denote different types of matrix as:
  - $\mathbf{X}$ regular $m \times n$ matrix
  - $\mathbf{A}$ a symmetric matrix $m \times m$

## 1. Non-negative matrix factorization (NMF)




## 2. Singular value decomposition :SVD (TBD)

Terminology: "matrix factorization" = "matrix decomposition", i.e. transform a matrix to a product of multiple matrices. [Ref: Wiki](https://en.wikipedia.org/wiki/Matrix_decomposition)


$$\mathbf {X} =\mathbf {U\Sigma V^{*}}$$
- [Wiki-SVD](https://en.wikipedia.org/wiki/Singular_value_decomposition)
- $\mathbf{U}$, $\mathbf{V}$ is [unitary matrix](https://en.wikipedia.org/wiki/Unitary_matrix): $U^*U=I$, $V^*V=I$
- $\mathbf{\Sigma}$ is a [Rectangular diagonal matrix](https://en.wikipedia.org/wiki/Diagonal_matrix#Rectangular_diagonal_matrices)
<!-- $$\begin{aligned}
    m_{ij} &= \sum_x u_{ix} (\sum_y\sigma_{xy}v_{yj}) \\ & = \sum_x u_{ix}\sigma_{xx}v_{xj} \leftarrow \Sigma {\text{ is diagonal}}
    \\ & = \vec{u}_i \cdot \vec{v}_j?
\end{aligned}$$ -->


Ref: [CSDN-blog-basics](https://blog.csdn.net/weixin_42575020/article/details/103504792), [CSDN-blog](https://blog.csdn.net/u010087338/article/details/114576013), [Zhihu-blog](https://zhuanlan.zhihu.com/p/36546367), [ScienceNet-Blog](http://blog.sciencenet.cn/blog-696950-699432.html) , [Zhihu-blog](https://zhuanlan.zhihu.com/p/36538284)




## 3. Principle component analysis (PCA)


## Deep Dive: SVD-related linear algebra basics


### DD-1. Eigenvalue and eigenvector


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
- For other matrices, the eigenvectors are not necessarily orthogonal: 


### DD-2. Symmetric matrix and eigenvector

Then, how to use the eigenvector? 

For a symetric matrix, the matrix usually can be decomposed as:
- $\mathbf{A} = \mathbf{U} \mathbf{\Lambda}\mathbf{U}^T$ where: 
  - $\mathbf{U} = [u_1, u_2, u_3...]$ is a symetric orthonmal matrix composed from eigenvectors.
  - $\mathbf{\Lambda} = \begin{bmatrix} \lambda_{1} & & \\
        & \ddots & \\
        & & \lambda_{m}
        \end{bmatrix}$ is a diagonal matrix with eigenvalues.

### DD-3. SVD and symmetric matrix

- Similar to this decomposition, SVD aims to decompose a $m \times n$ matrix.
  - $\mathbf{X}_{m \times n} =\mathbf{U}_{m \times m} \mathbf{\Sigma}_{m \times n} (\mathbf{V}_{n \times n})^{T}$
    - $\mathbf{U}$, $\mathbf{V}$ is [unitary matrix](https://en.wikipedia.org/wiki/Unitary_matrix): $U^*U=I$, $V^*V=I$
    - $\mathbf{\Sigma}$ is a [Rectangular diagonal matrix](https://en.wikipedia.org/wiki/Diagonal_matrix#Rectangular_diagonal_matrices)
    - e.g. (The "A" in the following figure refer to our $\mathbf{X}$)

    <div  align="center">
    <img src=https://img-blog.csdnimg.cn/20191212111401278.png style = "zoom:80%">
    </div>

- Then how to solve out $\mathbf{U}$, $\mathbf{V}$? Using the property of symmetric matrix:
  - Construct a symmetric matrix $\mathbf{A} =\mathbf{X}^T\mathbf{X}$
    - $A_{ij} = \sum_k (X^T)_{ik}X_{kj} = \sum_k X_{ki}X_{kj} = \sum_k (X^T)_{jk}X_{ki} = A_{ji} $
  - From the SVD, we have $\mathbf{A} = \mathbf{X}^T\mathbf{X} = \mathbf{V}\mathbf{\Sigma}^T\mathbf{\Sigma}\mathbf{V}^T$
    - denote $\mathbf{\Lambda} = \mathbf{\Sigma}^T\mathbf{\Sigma}$, $\mathbf{\Lambda} $ is diagonal, proof:
      - $\because \Sigma$ is diagonal, i.e. $\Sigma_{ij} = \begin{cases} 1 & i=j\\0 & i \neq j \end{cases}$
      - $\therefore$ $\Sigma_{ki}\Sigma_{kj} = \begin{cases} 1 & i=j=k\\0 & otherwise \end{cases}$
      - $\therefore \Lambda_{ij} = \sum_k (\Sigma^T)_{ik}\Sigma_{kj}= \sum_k \Sigma_{ki}\Sigma_{kj} =  \begin{cases} 1 & i=j\\0 & i \neq j \end{cases}$, 
  - We have $\mathbf{A} = \mathbf{V} \mathbf{\Lambda} \mathbf{V}^T$
    - Remember that "for symmetric matrix $\mathbf{A} = \mathbf{U} \mathbf{\Lambda}\mathbf{U}^T$..."
    - We could know the here, $\mathbf{V} = [\mathbf{v}_1,\mathbf{v_2},....]$, where:
      - $\mathbf{v_i}$ is the eigenvector of the symmetric matrix $\mathbf{A} =\mathbf{X}^T\mathbf{X} $
  - Similarly, construct $\mathbf{B} =\mathbf{X}\mathbf{X}^T =  = \mathbf{U}\mathbf{\Sigma}\mathbf{\Sigma}^T\mathbf{U}^T$
    - We could know $\mathbf{U} = [\mathbf{u}_1,\mathbf{u_2},....]$, where:
      - $\mathbf{u_i}$ is the eigenvector of the symmetric matrix $\mathbf{B} =\mathbf{X}\mathbf{X}^T $
- Ref: [CSDN-blog](https://blog.csdn.net/weixin_42575020/article/details/103504792)

## Deep Dive: Real symmetric matrix

### Properties

Ref: [CSDN-blog](https://blog.csdn.net/Banach_I/article/details/51078451)

#### 1. The eigenvector corresponding to different eigenvalue are orthogonal

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

#### 2. Eigendecomposition A = $U\Sigma U^T$



For any diagonalizable matrix, the matrix can be eigen-decomposed:

- $\mathbf{D}=\mathbf{Q}\mathbf{\Lambda}\mathbf{Q}^{-1}$
  - $\mathbf{Q}$ is the square $n \times n$ matrix whose i-th column is the eigenvector $q_i$ of $\mathbf{A}$, 
  - $\Lambda$ is the diagonal matrix whose diagonal elements are the corresponding eigenvalues $\lambda_i$

Ref: [Wiki-Eigendecomposition_of_a_matrix](https://en.wikipedia.org/wiki/Eigendecomposition_of_a_matrix)

For real-symmetric matrix:

- $\mathbf{A} = \mathbf{U} \mathbf{\Lambda}\mathbf{U}^T$ where: 
  - $\mathbf{U} = [\mathbf{u}_1, \mathbf{u}_2, \mathbf{u}_3...]$ is a symetric orthonmal matrix composed from eigenvectors.
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