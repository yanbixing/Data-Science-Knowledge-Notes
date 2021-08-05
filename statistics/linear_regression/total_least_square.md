# TLS: total least square

Ref: [Blog](https://cerebralmastication.com/2010/09/principal-component-analysis-pca-vs-ordinary-least-squares-ols-a-visual-explination/)


## 1. Definition

Total least square, also called "orthogonal regression", minimize the sum squared distance between the points and the line.

  <div  align="center"><img src=https://www.cerebralmastication.com/wp-content/uploads/2010/09/pca-280x300.png style = "zoom:80%"></div>


### 1.1. TLS vs OLS:

OLS, "O" for ordinary, ordinate, i.e. the y-axis. I.e. it minimizes the squared error on y-axis.

  <div  align="center"><img src=https://www.cerebralmastication.com/wp-content/uploads/2010/09/OLS1-280x300.png style = "zoom:80%"></div>


## 2. TLS and PCA

PCA: maximize the variance on the direction of principal component. The direction of the principal component is the direction of the TLS. 

### 2.1. Why PCA = TLS?

Intuitive proof (not strict.):
Note: for PCA we need mean subtraction. I.e. make the center of the data to (0,0). 
Note: for simplicity, we assume the line always pass (0,0), i.e. it rotates around the origin point, the only thing is to fit is the slope. (That is why the proof is not strict, just intuitive.)

<div  align="center"><img src=./linear_regression_asset/TLS_and_PCA.jpeg style = "zoom:18%"></div>

For PCA, denote the direction of the line as $\boldsymbol{w}$
$\begin{aligned}
  \boldsymbol{w} & = \underset{\boldsymbol{w}}{\argmax} \left[(\boldsymbol{X}\boldsymbol{w})^2\right] = \underset{\boldsymbol{w}}{\argmax}\left[\sum_i (d^{\parallel}_i)^2 \right] 
\end{aligned}$

And for TLS, denote the direction of the line as $\boldsymbol{u}$
$\begin{aligned}
\boldsymbol{u} &= \underset{\boldsymbol{u}}{\argmin}\left[\sum_i (d^{\perp}_i)^2 \right]
  \\& = \underset{\boldsymbol{u}}{\argmin}\left\{\sum_i [d^2_i - (d^{\parallel}_i)^2 ] \right\}\\
   &= \underset{\boldsymbol{u}}{\argmax}\left[\sum_i (d^{\parallel}_i)^2 \right]
\end{aligned}$
This means TLS maximize the variance of the projections of data on the direction of the fitting line. 

The proof is not strict. But the slope direction calculated from TLS is exactly the principal component direction calculated from PCA. The difference between the two tasks is: for PCA, we only care about direction, but for TLS, we need the line function, we also need the intercept.
  

- Ref: 
  - [NYU-pdf](https://cs.nyu.edu/~fergus/teaching/comp_photo/readings/leastSquares.pdf): Similar to above thinking, interpreting TLS as minimize the variance on the perpendicular direction.
  - [Blog](https://cerebralmastication.com/2010/09/principal-component-analysis-pca-vs-ordinary-least-squares-ols-a-visual-explination/): claim PCA = TLS
  - [StackExchange](https://stats.stackexchange.com/a/2700): Claim PCA = TLS, and give a further ref



PCA and SVD see: [matrix_factorization.md](../../general_machine_learning/math_topics/matrix_factorization.md)