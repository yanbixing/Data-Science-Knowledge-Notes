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

Define:
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
           x_{m}&\dots&x_{m}^{(n)}
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
