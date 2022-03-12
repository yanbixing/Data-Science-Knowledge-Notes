# OLS - Advanced

## 1. Solutions

### 1.1. Matrix solution

####  1.1.0. Problem review

For single variate OLS, our optimization problem is:

$$\begin{aligned}
    \boldsymbol{\beta} & =\underset{\boldsymbol{\beta} }{\argmin} \left [ R(\boldsymbol{\beta}) \right ]\\
    R(\boldsymbol{\beta}) &:= \sum^m_{i=1} (y_i - f(x_i))^2
\end{aligned}$$

where $f(x) = \sum^n_{j=1} \beta^{(j)} x^{(j)} +\beta^{(0)} = \sum^n_{j=0} \beta^{(j)} x^{(j)}$ , $\beta^{0} : =1$

####  1.1.1. Matrix representation

Denote:
- $\begin{aligned}
    \boldsymbol{Y}_{m\times 1} &= \begin{bmatrix}
           y_{1} \\
           \vdots \\
           y_{m}
         \end{bmatrix}
  \end{aligned}$

- $\begin{aligned}
    \boldsymbol{X}_{m \times (n+1)} &= \begin{bmatrix}
           x_{1} \\
           \vdots \\
           x_{m}
         \end{bmatrix}= \begin{bmatrix}
           x_{1}^{(0)}&\dots&x_{1}^{(n)} \\
           \vdots&\dotsb&\vdots \\
           x_{m}^{(0)}&\dots&x_{m}^{(n)}
         \end{bmatrix}
  \end{aligned}$

- $\begin{aligned}
    \boldsymbol{\beta}_{(n+1)\times 1} &= \begin{bmatrix}
           \beta^{(0)}  \\
           \vdots \\
           \beta^{(n)} 
         \end{bmatrix}
  \end{aligned}$

Then:

$$\begin{aligned}
    R(\boldsymbol{\beta}) & =  (\boldsymbol{Y} - \boldsymbol{X}\boldsymbol{\beta})^T(\boldsymbol{Y} - \boldsymbol{X}\boldsymbol{\beta})\\
     \boldsymbol{\beta} & =\underset{\boldsymbol{\beta} }{\argmin} \left [ R(\boldsymbol{\beta}) \right ]
\end{aligned}$$

So:

$$\begin{aligned}
    &\frac{\partial R(\boldsymbol{\beta}) }{\partial \beta}= - 2 \boldsymbol{X}^T(\boldsymbol{Y} - \boldsymbol{X}\boldsymbol{\beta}) = 0\\
    \Rightarrow & \boldsymbol{X}^T\boldsymbol{X}\boldsymbol{\beta} - \boldsymbol{X}^T\boldsymbol{Y} = 0\\
    \Rightarrow & \boldsymbol{\beta} = (\boldsymbol{X}^T\boldsymbol{X})^{-1}\boldsymbol{X}^T\boldsymbol{Y}
\end{aligned}$$

Further deduction: [PurdueU-stat512](https://www.stat.purdue.edu/~boli/stat512/lectures/topic3.pdf), [PSU-stat462](https://online.stat.psu.edu/stat462/node/132/)

#### 1.1.1 What if $\boldsymbol{X}^T\boldsymbol{X}$ NOT invertible?

When $\boldsymbol{X}^T\boldsymbol{X}$ is invertible,this means the the solution (on coefficient $\boldsymbol{\beta}$) is unique (there is only one solution.)

When $\boldsymbol{X}^T\boldsymbol{X}$ is NOT invertible, this means the solution is not unique, usually happens when number of sample $M$ < num features $N$.

- Solution:
  - Regularization (L1/L2)
    - $\boldsymbol{\beta}^* = \underset{\boldsymbol{\beta}}{\argmin} \left[ R(\boldsymbol{\beta}) + \lambda \|\beta\|^2 \right]$
    - I.e. Add punishment on weight to make the solution unique. L1 also works.
  - Pseudo inversion
    - $\boldsymbol{\beta}^* = \underset{\boldsymbol{\beta} \in \mathcal{B} }{\argmin} \| \boldsymbol{\beta}\|^2 $ where $ \mathcal{B} = \{ \text{ all } \boldsymbol{\beta}' = \underset{\boldsymbol{\beta}}{\argmin} R(\boldsymbol{\beta}) \} $
    - I.e. Choose the solution have least size among all solutions.
  - Other non-academic method:
    - Feature elimination
    - PCA dimension reduction
    - Ref: [1p3a](https://www.1point3acres.com/bbs/thread-520357-1-1.html): make sense, the problem happens when $N>M$, i.e. weight is more free than sample can limit, so reduce dimension of feature which reduce the degree of freedom of solution may also work.

- Ref: 
  - [Berkeley-slide](https://www.stat.berkeley.edu/~aditya/resources/LectureFOUR.pdf): not invertible means no unique solution
    - [UCLA-slide](http://web.cs.ucla.edu/~chohsieh/teaching/CS260_Winter2019/notes_linearregression.pdf): not invertible usually $M<N$
  - [CMU-slide](https://www.cs.cmu.edu/~epxing/Class/10715/lectures/lecture2-LR.pdf): solution: regularization and statistical interpretation.
    - [Princeton-slide](https://www.cs.princeton.edu/~bee/courses/scribe/lec_09_11_2013.pdf): Matrix representation of solution of Ridge regression, generally invertible.
  - [UCLA-slide](http://web.cs.ucla.edu/~chohsieh/teaching/CS260_Winter2019/notes_linearregression.pdf): solution: pseudo inverse, detailed process, employ SVD technique.
    - [MathOverflow](https://mathoverflow.net/questions/384919/is-the-pseudoinverse-the-same-as-least-squares-with-regularization): pseudo inverse looks similar to ridge regression but different.
  
  

### 1.2. Gradient descent solution

In the above matrix representation, we found there is an matrix inverse. For high dimension matrix, inverse is hard to solve. So we can use gradient descent.


$R(\boldsymbol{\beta}) = \sum^m_{i=1} (y_i - f(x_i))^2 = \sum^m_{i=1} (y_i - \sum^n_{j=0} \beta^{(j)} x_i^{(j)})^2 $

Thus:

$\begin{aligned}
    \mathrm{grad}(\beta^{(r)}) &= \frac{\partial R(\boldsymbol{\beta})}{\partial \beta^{(r)}} \\
    &= \sum^m_{i=1} 2(y_i - \sum^n_{j=0} \beta^{(j)} x^{(j)})(-x_i^{(r)}) \\
    &=  -2\sum^m_{i=1} x_i^{(r)}(y_i - \sum^n_{j=0} \beta^{(j)} x^{(j)}) \\
\end{aligned}$

I.e. In matrix representation:

$$\nabla R(\boldsymbol{\beta}) := \left[ \mathbf{grad( \boldsymbol{\beta})} \right]_{(n+1) \times 1}  = - 2 \left [ \boldsymbol{X}^T \right ]_{(n+1) \times m} \left [ \boldsymbol{Y} - \boldsymbol{X}\boldsymbol{\beta} \right ]_{m\times 1}$$

And the GD update process is like:

$$\boldsymbol{\beta}_{(t+1)} = \boldsymbol{\beta}_{(t)} - \alpha \left. \mathbf{grad( \boldsymbol{\beta})} \right |_{\boldsymbol{\beta} = \boldsymbol{\beta}_{(t)}} $$

Note: both $\boldsymbol{\beta}$ and $\mathbf{grad( \boldsymbol{\beta})}$ are $[(n+1) \times 1]$-dim matrix, i.e. ($n+1$-dim vector)
