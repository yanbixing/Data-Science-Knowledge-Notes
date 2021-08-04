# Linear Regression

## 0. Definition


- $\mathcal{S}$: the dataset contain $m$ samples.
  - i-th sample is denoted as $(\boldsymbol{x}_i, y_i)$.
  - j-th feature of the input of i-th sample is denoted as $\boldsymbol{x}^{(j)}$.
- $\hat{y_i} :=\hat{y}(x_i) := f(x_i)$: the estimated/prediction value according to estimator $f$.
- $\sigma(y):=\mathrm{SD}(y):=\sqrt{\mathrm{Var}(y)}$: population standard deviation.

### 0.1. Frequent equations

- Covariance of (x,y)
$$\begin{aligned} \mathrm{Cov}(x,y) &= E[ (x-\bar{x})(y-\bar{y}) ]\\
&= E(xy - \bar{x}y-x\bar{y}+\bar{x}\bar{y})\\
&= E(xy) - E(x)E(y) - E(x)E(y) + E(x)E(y) \\
& = E(xy) - E(x)E(y)
\end{aligned}$$
- Correlation coefficient of (x,y)
$$\begin{aligned} \mathrm{r_{cov}}(x,y) &:= \frac{\mathrm{Cov}(x,y)}{\sigma(x)\sigma(y)} \\ &= \frac{[ \frac{1}{m}\sum^m_{i=1}(x_i-\bar{x})(y_i-\bar{y}) ]}{ \sqrt{\ \frac{1}{m}\sum^m_{i=1}(x_i-\bar{x})^2} \sqrt{ \frac{1}{m}\sum^m_{i=1}(y_i-\bar{y})^2} }
\end{aligned}$$


## 1. Basics

### 1.0. Variances related to linear regression:

- (Total )Variance of y variable: 
  $$\mathrm{Var}(y):=\mathrm{Var_{tot}}(y) := \frac{1}{m} \sum^m_{i=1} ( y_i - \bar{y} )^2$$
- Explained variance of y variable: 
  $$\mathrm{Var_{exp}}(y,\hat{y}) := \frac{1}{m} \sum^m_{i=1} ( \hat{y}_i - \bar{y} )^2$$
- Residual variance of y variable: 
  $$\mathrm{Var_{res}}(y,\hat{y}) := \frac{1}{m} \sum^m_{i=1} \varepsilon^2_i := \frac{1}{m} \sum^m_{i=1} ( \hat{y}_i - y_i )^2$$
  - $\varepsilon_i := y_i - f(x_i)$: **residue** of prediction on i-th sample.
  - $\mathrm{Var_{res}}$ is also call "Residual error"
    - $\sum^m_{i=1} \varepsilon^2_i$ is called "sum squared error" (SSE)
  

### 1.1. OLS: Ordinary least square

A method to fit the linear model.
$$\boldsymbol{\beta} =\underset{\beta}{\argmin} \left [ \sum^m_{i=1} (y_i - f(x_i))^2 \right ]$$

where $\mathrm{Var_{res}}(y) : = \frac{1}{m} \sum^m_{i=1} [y_i - f(x_i)]^2$ and $f(x) = \sum_j \beta^{(j)} x^{(j)}$

E.g.: 

- For $f(x) = \beta^{(0)} + \beta^{(1)}x$
- $L_{OLS}(x):=\mathrm{Var_{res}} =  \frac{1}{m} \sum^m_{i=1} (y_i - f(x_i))^2$
- Minimization can be solve with $\partial_{\beta^{(i)}}L(x) =0$:
  - $\beta^{(0)}$ deduction:
      $\begin{aligned}
        & \partial_{\beta^{(0)}}L_{OLS}(x) = 0 \\ 
        \Rightarrow & \sum^m_{i=1} 1\cdot[y_i - f(x_i)] = 0 \\ 
        \Rightarrow & \sum^m_{i=1} y_i - (\beta^{(0)} + \beta^{(1)}x_i) = 0 \\ 
        \Rightarrow & \beta^{(0)} = \frac{1}{m} \sum^m_{i=1}(y_i -\beta^{(1)}x_i)\\
        \Rightarrow & \beta^{(0)} = \bar{y} - \beta^{(1)} \bar{x}
      \end{aligned}$

  - $\beta^{(1)}$ deduction:
      $\begin{aligned}
        & \partial_{\beta^{(0)}}L_{OLS}(x) = 0 \\ 
        \Rightarrow & \sum^m_{i=1} x_i[y_i - f(x_i)] = 0 \\ 
        \Rightarrow & \sum^m_{i=1} x_i \{ y_i -  [ \bar{y} - \beta^{(1)} \bar{x} + \beta^{(1)}x_i]  \}  = 0 \\ 
        \Rightarrow & \sum^m_{i=1} ( x_iy_i -  \bar{x}\bar{y} )= \beta^{(1)} \sum^m_{i=1} ( x_i^2 - \bar{x}^2 ) \\ 
        \Rightarrow & \sum^m_{i=1} [( x_i -  \bar{x})(y_i -\bar{y} )]= \beta^{(1)} \sum^m_{i=1} ( x_i- \bar{x} )^2  \\
        \Rightarrow & \beta^{(1)} = \frac{ \frac{1}{m} \sum^m_{i=1} [( x_i -  \bar{x})(y_i -\bar{y} )] }{ \frac{1}{m} \sum^m_{i=1} ( x_i- \bar{x} )^2 } = \frac{\mathrm{Cov}(x,y) }{ \mathrm{Var}(x) } = r_{\mathrm{cov}}(x,y) \frac{\sigma(y)}{\sigma(x)} \\
      \end{aligned}$
- Therefore 
  $$ \left\{
    \begin{aligned}
    &\beta^{(0)}= \bar{y} - \beta^{(1)} \bar{x} \\
    &\beta^{(1)} = \frac{\mathrm{Cov}(x,y) }{ \mathrm{Var}(x) } = r_{\mathrm{cov}}(x,y) \frac{\sigma(y)}{\sigma(x)} 
    \end{aligned}
    \right.
  $$
  - where: $r_{\mathrm{cov}}(x,y):= \frac{\mathrm{Cov}(x,y)}{\sigma(x)\sigma(y)} := r_{xy}$ is the correlation coefficient.


for OLS:

$\begin{aligned}
  \bar{\varepsilon} & = E(y_i) - E[f(x_i)] = \bar{y} - \beta^{(1)}\bar{x}-\beta^{(0)}\\
  & = \bar{y} - \beta^{(1)}\bar{x} - (\bar{y} - \beta^{(1)} \bar{x} )\\
  & =0
\end{aligned}$ 

$\mathrm{Var_{res}}(y) := \sum_i \varepsilon^2_i \underset{OLS}{=} \sum_i (\varepsilon_i - \bar{\varepsilon})^2 = \mathrm{Var}(\varepsilon) $

### 1.2. R-squared score: $R^2$

R-squared is also called "coefficient of determination" Ref: [Stattrek](https://stattrek.com/regression/linear-regression.aspx?Tutorial=AP)

#### 1.2.1. Definition:

The R-squared score is defined as:

$$R^2 := \frac{\mathrm{Var_{exp}}(y) }{\mathrm{Var_{tot}}(y) } = \frac{\sum^m_{i=1} ( \hat{y}_i - \bar{y} )^2}{\sum^m_{i=1} ( y_i - \bar{y} )^2 } \in [0,1]$$

**The higher R-squared** score, **the better** the estimator $f$ is. Intuitively, you can understand it as "the better variance we can explain, the less uncertainty remaining in data."

#### 1.2.2.  $R^2$ and OLS



1. For OLS fitting: $R^2 = r_{x,y}^2$, i.e. R-squared is the square of the correlation coefficient.

**Proof:** (use $f(x) = \beta^{(0)} + \beta^{(1)}x$ as example.)

$\begin{aligned}
  \mathrm{Var_{exp}}(y) &= \sum_i (\hat{y}_i - \bar{y})^2 = \sum_i \left[( \beta^{(0)} + \beta^{(1)}x_i) - \bar{y} \right]^2\\
  & = \sum_i \left[ ( \bar{y} - \beta^{(1)}\bar{x} ) + \beta^{(1)}x_i - \bar{y} \right]^2\\
  & = \sum_i \left [ ( \bar{y} - \bar{y}) + \beta^{(1)}(x_i - \bar{x} )   \right ]^2 \\
  & = \left[\beta^{(1)} \right]^2 \sum_i \left[ (x_i - \bar{x} )   \right]^2 =  [r_{\mathrm{cov}}(x,y) \frac{\sigma(y)}{\sigma(x)} ]^2 \mathrm{Var(x)} \\
  &=r^2_{xy} \mathrm{Var}(y)=r^2_{xy} \mathrm{Var_{tot}}(y)
\end{aligned}$

Thus $R^2 :=  \frac{\mathrm{Var_{exp}}(y) }{\mathrm{Var_{tot}}(y) } = r^2_{xy}$


2. For OLS fitting: $\mathrm{Var_{res}}(y) = \mathrm{Var_{tot}}(y) - \mathrm{Var_{exp}}(y)$


**Proof:** (use $f(x) = \beta^{(0)} + \beta^{(1)}x$ as example.)

$\begin{aligned}
  \mathrm{Var_{tot}}(y) & = \sum_i (y_i - \bar{y})^2=\sum_i [ (y_i -\hat{y}_i) + (\hat{y_i} - \bar{y})]^2  \\
  & = \sum_i [ (y_i - \hat{y}_i)^2 + (\hat{y}_i - \bar{y})^2 - 2(y_i-\hat{y}_i)(\hat{y}_i - \bar{y}) ] \\
  & = \mathrm{Var_{res}} + \mathrm{Var_{exp}} - 2 \sum_i [(y_i-\hat{y}_i)(\hat{y}_i - \bar{y}) ]
\end{aligned}$

We can prove $\sum_i (y_i-\hat{y}_i)(\hat{y}_i - \bar{y}) =0$ as following:

$\begin{aligned}
  \sum_i (y_i-\hat{y}_i)(\hat{y}_i - \bar{y}) &= \sum_i \hat{y}_i (y_i-\hat{y}_i) + \sum_i \bar{y} (y_i-\hat{y}_i)\\
  & = \left[\beta^{(1)}\sum_ix_i(y_i - \hat{y}_i) + \beta^{(0)}\sum_i(y_i - \hat{y}_i) \right] + \bar{y} \sum_i(y_i-\hat{y}_i)\\
  & = \beta^{(1)}\sum_ix_i(y_i - \hat{y}_i) + (\beta^{(0)}+\bar{y})\sum_i(y_i - \hat{y}_i)
\end{aligned}$

- Since $\partial_{\beta^{(0)}}L_{OLS}(x) = 0 \Rightarrow 0 = \sum^m_{i=1}  1\cdot[y_i - f(x_i)]  = \sum_i (y_i-\hat{y}_i)$
  - Therefore: $(\beta^{(0)}+\bar{y})\sum_i (y_i-\hat{y}_i) = (\beta^{(0)}+\bar{y}) \cdot 0 = 0$
- Since $\partial_{\beta^{(1)}}L_{OLS}(x) = 0 \Rightarrow 0= \sum^m_{i=1} x_i[y_i - f(x_i)]  = \sum_i x_i(y_i-\hat{y}_i)$
  - Therefore: $ \beta^{(1)}\sum_ix_i(y_i - \hat{y}_i) = \beta^{(1)} \cdot 0 = 0$
- Thus $\sum_i (y_i-\hat{y}_i)(\hat{y}_i - \bar{y}) =0$

Thus, we proved:
$$\mathrm{Var_{tot}}(y) = \mathrm{Var_{res}} + \mathrm{Var_{exp}}$$

3. For OLS fitting: the residual variance (sum of squared error, residual error) $\mathrm{Var_{res}}:= \sum_i \varepsilon^2_i$ is irrelevant to the scale of $x$.

**Proof:**

$\begin{aligned}
  \mathrm{Var_{res}} & = \mathrm{Var_{tot}}(y) -  \mathrm{Var_{exp}} (y)\\
  & = \mathrm{Var}(y)  - r^2_{xy} \mathrm{Var}(y)\\
  & = (1- r^2_{xy} )\mathrm{Var}(y)
\end{aligned}$

- Therefore: the residual error is only related to coorelation coefficient $r_{xy}$ and the variance of y $\mathrm{Var}(y)$.

- The more $r_{xy}\rightarrow 1$ the smaller the fitting residual error.

<font color="#0000dd">**Need to remember:**</font> <mark style="background-color:yellow;"><font color="#0000dd">$\mathrm{Var_{res}}= (1- r^2_{xy} )\mathrm{Var}(y)$</font></mark>

### 1.3. TLS: total least square

Ref: [Blog](https://cerebralmastication.com/2010/09/principal-component-analysis-pca-vs-ordinary-least-squares-ols-a-visual-explination/)

OLS, "O" for ordinary, ordinate, i.e. the y-axis. I.e. it minimizes the squared error on y-axis.

  <div  align="center"><img src=https://www.cerebralmastication.com/wp-content/uploads/2010/09/OLS1-280x300.png style = "zoom:80%"></div>


Total least square, also called "orthogonal regression", minimize the sum squared distance between the points and the line.

  <div  align="center"><img src=https://www.cerebralmastication.com/wp-content/uploads/2010/09/pca-280x300.png style = "zoom:80%"></div>


PCA: maximize the variance on the direction of principal component. The direction of the principal component is the direction of the TLS. 

#### Why PCA = TLS?

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




## 2. Topics

### 2.1. Hypothesis and regression

In hypothesis testing area, linear regression is often used to identify whether the dependent variable y is related to feature x. If the slope is zero, we will regard x and y as irrelevant.

I.e.  Denote the best estimator for (x,y) as $f(x) = \beta^{(0)} + \beta^{(1)}x$, then:

- $H_0: \beta^{(1)} = 0$
- $H_1: \beta^{(1)} \not = 0$

The mean value of $\beta^{(1)}$ is the fitted $\beta^{(1)obs}$ from OLS or other methods.

The SE of $\beta^{(1)}$ (SEC: standard error of coefficients) is deducted from the residual error.

the $\mathrm{CI}_{(\beta^{(1)})} = [\beta^{(1)obs} - z\cdot \mathrm{SE}_{\beta^{(1)}}, \beta^{(1)obs} + z\cdot \mathrm{SE}_{\beta^{(1)}}]$

If $0 \in \mathrm{CI}_{(\beta^{(1)})}$, accept $H_0$, x,y is irrelevant; else, reject $H_0$, x,y is relevant.

- Alternatively, you can also use the calculate the z-score of the $\beta^{(1)obs}$: $z^{obs} = \frac{\beta^{(1)obs} - \beta_{H_0}}{\mathrm{SE}(\beta^{(1)})}$, calculated the p-value, and compare it with $\alpha$. 
  - $\mathrm{p-val} (\beta^{(1)obs})<\alpha \Rightarrow$ Reject $H_0$ (i.e. the obs value fall out of CI under $H_0$)
  - $\mathrm{p-val} (\beta^{(1)obs})>\alpha \Rightarrow$ Accept $H_0$(i.e. the obs value fall in the CI under $H_0$)

**Note:** fomula of SEC

Ref: [StackExchange](https://stats.stackexchange.com/questions/85943/how-to-derive-the-standard-error-of-linear-regression-coefficient), [Wiki-Linear_regression](https://en.wikipedia.org/wiki/Simple_linear_regression#Normality_assumption)


The SE of can be deducted from the residual error: 


$$\mathrm{SE}_{\beta^{(1)}} = \frac{1}{\sqrt{m-2}}\sqrt{\frac{\frac{1}{m}\sum^m_{i=1} (y_i - f(x_i))^2 }{\frac{1}{m} (x_i-\bar{x})^2 }} = \frac{1}{\sqrt{m-2}}\sqrt{\frac{\mathrm{Var_{res} }(y,\hat{y})}{\mathrm{Var}(x)}}$$

For OLS $\mathrm{Var_{res} }(y,\hat{y}) = \mathrm{Var}(\varepsilon) := \sigma^2(\varepsilon)$, thus

$$\mathrm{SE}_{\beta^{(1)}} \sim \frac{1}{\sqrt{m}} \frac{\sigma(\varepsilon)}{\sigma(x)}$$

and $\mathrm{Var_{res} }(y,\hat{y}) = \mathrm{Var_{tot}}(y) -  \mathrm{Var_{exp}}(y,\hat{y}) = (1-r^2_{xy})\mathrm{Var}(y)$
 
$$\mathrm{SE}_{\beta^{(1)}} \sim \frac{1}{\sqrt{m}} \sqrt{1-r^2_{xy}} \frac{\sigma(y)}{\sigma(x)}$$

Thus, under OLS and $H_0:\beta^{(1)}=0$

$$z^{obs} = \frac{\beta^{(1)obs} - \beta_{H_0}}{\mathrm{SE}(\beta^{(1)})} = \frac{r_{xy} \frac{\sigma(y)}{\sigma(x)} }{\frac{1}{\sqrt{m}} \sqrt{1-r^2_{xy}} \frac{\sigma(y)}{\sigma(x)}} = \sqrt{m} \frac{r_{xy}}{\sqrt{1-r^2_{xy}}}$$

**Memorization tips:** (not strict, just help to memorize)

- $\sigma(\varepsilon)$: higher error in y, the more inaccurate on $\beta^{(1)}$
- $\sigma(x)$: when x scale increase, the value of $\beta^{(1)}$ shrinks, so as the uncertainty on $\beta^{(1)}$.
- $\frac{1}{\sqrt{m}}$ similar to mean value, more sample, more precise we can estimate the parameter.
- $\frac{1}{\sqrt{m-2}}$ 2 points can define a line, so we need sacrifice 2 degree of freedom.


### 2.2. Geometric understanding of correlation

The correlation coefficient can be understood as the $\cos$ value of the angle between the two $m$-dim vectors constructed from the mean-subtracted x-coordinates and y-coordinates respectively.

- Note: Mean subtraction is a translation operation; translation operation will not change the covariance and variance, so it will not change correlation.

I.e. 

- Assume we have $m$ samples, the mean-subtracted data is denoted as $\mathcal{S} = \{(x_1, y_1), \dots, (x_m, y_m) \}$, denotes $\boldsymbol{v}_X = (x_1, \dots, x_m)$, $\boldsymbol{v}_Y = (y_1, \dots, y_m)$
- Then, we have: $r_{xy} = \cos(\boldsymbol{v}_X, \boldsymbol{v}_Y)$
  

**proof:**

- Note: after mean subtruction $\bar{x}=0$, $\bar{y}=0$

$\begin{aligned} r_{xy} & = \frac{Cov(x,y)}{\sigma(x)\sigma(y)} = \frac{\frac{1}{m} \sum^m_{i=1} (x_i-\bar{x})(y_i-\bar{y}) }{ \sqrt{\frac{1}{m} \sum^m_{i=1} (x_i-\bar{x})^2} \sqrt{\frac{1}{m} \sum^m_{i=1} (y_i-\bar{y})^2} }
\\ & = \frac{\sum^m_{i=1} (x_i-\bar{x})(y_i-\bar{y}) }{ \sqrt{ \sum^m_{i=1} (x_i-\bar{x})^2} \sqrt{ \sum^m_{i=1} (y_i-\bar{y})^2} }
\\ & \underset{0 \text{ mean} }{=} \frac{\sum^m_{i=1} x_iy_i }{ \sqrt{ \sum^m_{i=1} x^2_i} \sqrt{ \sum^m_{i=1} y^2_i} } = \frac{\boldsymbol{v}_X \cdot \boldsymbol{v}_Y}{ \|\boldsymbol{v}_X\| \|\boldsymbol{v}_Y\| } = \cos(\boldsymbol{v}_X , \boldsymbol{v}_Y)
\end{aligned}$

**proof:** translation doesn't change Covariance and Variance.
- Note: $c$ is a constant, $x$ is a variable, then
  - $\overline{x+c} = E(x+c) = E(x)+c = \bar{x}
+c $

$\begin{aligned}
  \mathrm{Cov}(x+c,y) &= \frac{1}{m}\sum^m_{i=1} [(x_i+c) - \overline{x+c} \text{ } ](y_i - \bar{y})\\
  &= \frac{1}{m}\sum^m_{i=1} [(x_i+c) - (\bar{x}+c) ](y_i - \bar{y})\\
  &= \frac{1}{m}\sum^m_{i=1} (x_i- \bar{x}) (y_i - \bar{y}) = \mathrm{Cov}(x,y)
\end{aligned}$

$\begin{aligned}
  \mathrm{Var}(x+c) &= \frac{1}{m}\sum^m_{i=1} [(x_i+c) - \overline{x+c} \text{ } ]^2\\
  &= \frac{1}{m}\sum^m_{i=1} [(x_i+c) - (\bar{x}+c) ]^2\\
  &= \frac{1}{m}\sum^m_{i=1} (x_i- \bar{x})^2 = \mathrm{Var}(x)
\end{aligned}$

Ref: [ResearchGate](https://www.researchgate.net/publication/256374947_Geometric_interpretation_of_a_correlation)

#### Application Examples

Q: We have a data in three dimension $\mathcal{X} = \{(x_1,y_1,z_1), \dots, (x_m,y_m,z_m)\}$. Given $r_{xy} = \alpha, r_{yz} = \beta$, solve the range of $r_{xz}$.
A: Use the geometric understand, 
Denotes $X:=(x_1,\dots, x_m)$,$Y:=(y_1,\dots, y_m)$, $Z:=(z_1,\dots, z_m)$ 
And denote the angle between $A, B$ as $\theta_{AB}$, then: $\theta_{XY} = \arccos \alpha$, $\theta_{YZ} = \arccos \beta$
Then, $\theta_{XZ}\in [\theta_{XY} - \theta_{YZ},\theta_{XY} + \theta_{YZ} ]$

<div  align="center"><img src=http://i.imgur.com/XRpz35T.png style = "zoom:60%"></div>

Thus $r_{xz} = \cos\theta_{XZ} \in [ \cos(\arccos\alpha+\arccos\beta), \cos(\arccos\alpha-\arccos\beta) ]$
Ref: [Blog](http://jakewestfall.org/blog/index.php/2013/09/17/geometric-argument-for-constraints-on-corrxz-given-corrxy-and-corryz/)


## 3. FAQ


### 3.1. Influence on fitting results

How does the coefficient and t-score (or z-score) changes when the following cases happens?

#### 3.1.1 Duplication of data

For OLS:

  $$ \left\{
    \begin{aligned}
    &\beta^{(0)}= \bar{y} - \beta^{(1)} \bar{x} \\
    &\beta^{(1)} = \frac{\mathrm{Cov}(x,y) }{ \mathrm{Var}(x) } = r_{\mathrm{cov}}(x,y) \frac{\sigma(y)}{\sigma(x)} 
    \end{aligned}
    \right.
  $$

- Since: covariance and variance doesn't change when duplicating data:
  - $\mathrm{Cov}(x,y) = \frac{1}{m}[\sum^m_{i=1} (x_i-\bar{x})(y_i-\bar{y})] = \frac{1}{2m}[2\cdot\sum^m_{i=1} (x_i-\bar{x})(y_i-\bar{y})]$
  - $\mathrm{Var}(x)= \frac{1}{m} \sum^m_{i=1} ( y_i - \bar{y} )^2= \frac{1}{2m} 2\cdot \sum^m_{i=1} ( y_i - \bar{y} )^2$
- Thus, coefficients $\beta^{(0)}$ and $\beta^{(1)}$ NOT change when duplicating data.

 $$R^2 = r_{x,y}^2 = \left[\frac{\mathrm{Cov}(x,y)}{\sigma(x)\sigma(y)}\right]^2 = \frac{\mathrm{Cov}^2(x,y)}{\mathrm{Var}(x)\mathrm{Var}(y)}$$

- $\mathrm{Cov}(x,y)$, $\mathrm{Var}(x)$, $\mathrm{Var}(y)$ will not change, thus $R^2$ will not change.
  
  $$\mathrm{SE}_{\beta^{(1)}} \sim \frac{1}{\sqrt{m}} \frac{\sigma(\varepsilon)}{\sigma(x)}$$

- Similar to above, variance doesn't change when duplicating;Population standard deviation doesn't change when duplicating.
  - $\sigma(\varepsilon)= \sqrt{ \frac{1}{m} \sum^m_{i=1} \varepsilon_i^2 } = \sqrt{ \frac{1}{2m} 2 \cdot \sum^m_{i=1} \varepsilon_i^2 }$
- Thus, $\mathrm{SE}_{\beta^{(1)}} \sim \frac{1}{\sqrt{m}}$ will drop. I.e. $\mathrm{SE}_{\beta^{(1)}, 2m} = \frac{1}{\sqrt{2}} \mathrm{SE}_{\beta^{(1)},m}$ 
  - Generally, smaller CI means the result is more precise.
  - But **here** since we just duplicate the data, the $\beta^{(1)obs}$ doesn't change; when CI is smaller, there is more chance 0 fall out of the CI, i.e. **more chance to reject null hypothesis**, more chance on **type-1 error**.
  - However, if we don't duplicate data but sample more data, we will have no such issue, i.e. we don't increase the chance of type-1 error. This is because when sampling more data, the $\beta^{(1)obs}$ may also change: be closer to the true value.


$$z^{obs} = \frac{\beta^{(1)obs} - \beta_{H_0}}{\mathrm{SE}(\beta^{(1)})}$$

- $\beta^{(1)obs}$ will not change, $\mathrm{SE}_{\beta^{(1)}, 2m} = \frac{1}{\sqrt{2}} \mathrm{SE}_{\beta^{(1)},m}$
- Thus, $z^{obs}_{2m} = \sqrt{2} z^{obs}_m$, z (or t) score increases.

#### 3.2.2 Swith X and Y


Assume:

- Before switch, the function is: $y=\beta^{(0)}+\beta^{(1)}x$
- After switch, the function is: $x = \alpha^{(0)}+\alpha^{(1)}y$

Note: the covariance and variance are not affected by the switch, this is reasonable, as swithing will not affect the relation X,Y. It will affect OLS params is because OLS minimize error on y-axis, after switching, it minimize error on x-axis. For TLS, the relative position of the line will not change, the slope will be inverse.

$\mathrm{Cov}(x,y) = E[ (x-\bar{x})(y-\bar{y}) ]$ 
$\mathrm{Var}(x) = E[ (x-\bar{x})^2]$ 

- Since $\mathrm{Cov}$ and $\mathrm{Var}$ not change, correlation coefficient $r_{xy}$ and $R^2$ score will not change. 
  - The understanding is same, they are metric describing relation between array x and y, not related to axis position.

Under OLS, there is not special relation, the slope is not inverse.

- $\beta^{(1)} = \frac{\mathrm{Cov}(x,y) }{ \mathrm{Var}(x) } = r_{xy}\frac{\sigma(y)}{\sigma(x)}$
- $\alpha^{(1)} = \frac{\mathrm{Cov}(x,y) }{ \mathrm{Var}(y) } = r_{xy}\frac{\sigma(x)}{\sigma(y)}$

Since for OLS, $\mathrm{SE}_{\beta^{(1)}} \sim \frac{1}{\sqrt{m}} \sqrt{1-r^2_{xy}} \frac{\sigma(y)}{\sigma(x)}$, there is no special relation.

- $\mathrm{SE}_{\beta^{(1)}} \sim \frac{1}{\sqrt{m}} \sqrt{1-r^2_{xy}} \frac{\sigma(y)}{\sigma(x)}$
- $\mathrm{SE}_{\alpha^{(1)}} \sim \frac{1}{\sqrt{m}} \sqrt{1-r^2_{xy}} \frac{\sigma(x)}{\sigma(y)}$

For t (or z) score, interestingly, they are same.

- $z_{\beta^{(1)}}^{obs} = \frac{\beta^{(1)obs} - \beta_{H_0}}{\mathrm{SE}(\beta^{(1)})} = \frac{r_{xy} \frac{\sigma(y)}{\sigma(x)} }{\frac{1}{\sqrt{m}} \sqrt{1-r^2_{xy}} \frac{\sigma(y)}{\sigma(x)}} = \sqrt{m} \frac{r_{xy}}{\sqrt{1-r^2_{xy}}}$
- $z_{\alpha^{(1)}}^{obs} = \frac{\alpha^{(1)obs} - \alpha_{H_0}}{\mathrm{SE}(\alpha^{(1)})} = \frac{r_{xy} \frac{\sigma(x)}{\sigma(y)} }{\frac{1}{\sqrt{m}} \sqrt{1-r^2_{xy}} \frac{\sigma(x)}{\sigma(y)}} = \sqrt{m} \frac{r_{xy}}{\sqrt{1-r^2_{xy}}}$

Ref: [YouTube](https://youtu.be/oXBlG8vIZg8), [StackExchange](https://stats.stackexchange.com/questions/22718/what-is-the-difference-between-linear-regression-on-y-with-x-and-x-with-y/22721), [Wiki-t_test](https://en.wikipedia.org/wiki/Student%27s_t-test)

## Deep Dive: Assumptions in linear regression

Ref: [Duke_U](http://people.duke.edu/~rnau/testing.htm)

1. **Linearity** and **additivity**: 
   - I.e.:
     - The expected values of $y$ is a a straight-line function of $\boldsymbol{x}$
     - The slope is a constant, independent on $\boldsymbol{x}$
     - The effect (contribution) of different independent variable $x^{(i)}$ on the dependent variable $y$ is additive.
   - I.e. personal understand: 
     - the relation between the expectation of y and x satisfy the form $\bar{y}(x) = \sum_i k^{(i)} x^{(i)}$
   - If violate:
     - Error is high and your fitted function is not generalized. 
       - E.g. the function fitted with x=[0,1] have much higher error in x=[1,2].
   - Check:
     - Plot error (residue) vs x, fit with non-linear function.
   - Fix:
     - Introduce non-linear transformation of the features.

2. **Statistical independence** of errors: the errors on different $\boldsymbol{x}$ values are independent with each other.
   - I.e. personal understanding: 
     - There is **no autocorrelation** in the data.
   - If violate:
     - Cannot further improve the model (reduce error) without considering the seasonality and stationarity.
   - Check:
     - Plot error (residue) vs x, check autocorrelation.
   - Fix:
     - Introduce seasonality and stationarity with time series model like ARIMA.
  
3. [Homoscedasticity](https://en.wikipedia.org/wiki/Homoscedasticity): the **variance of error is constant**, not change with $\boldsymbol{x}$ or $y$.
   - If violate:
     - The confidence interval is not accurate.
   - Check
     - Plot error vs x and y, see if there is any pattern.
   - Fix:
     - Non-linearly transform the data to make it constant.
     - Adjust the predictor, introduce other components like time series models.

4. The error is **normally distributed**.
   - If violate:
     - The confidence interval is not accurate.
   - Check:
     - Plot the distribution of error, check if it is a normal distribution.
   - Fix:
     - Non-linear transformation of features.
   
In sum, if those assumptions are violated, the error will be high and the confidence interval is not accurate.
The detection method is usually plot and observe.
The solution is introduce non-linearity by non-linear transformation of feature or introduce non-linear models.


## Deep Dive: Confidence interval vs prediction interval (TBD)

Ref: [YouTube-CI_vs_PI](https://youtu.be/o0UESA3UZss), [StakeExchange](https://stats.stackexchange.com/questions/16493/difference-between-confidence-intervals-and-prediction-intervals)

- Prediction interval (PI): **Given an estimator** function $f_{est}(x)$, the 90\% PI is the $2w_{PI}(x)$ width area centered with $f_{est}(x)$ (i.e. $[f_{est}(x)-w_{PI}(x), f_{est} +w_{PI}(x)]$) where **90% of data will fall into**.
  - $2w_{PI}(x)$ means the width of the PI area varies with x.
    <div  align="center"><img src=./linear_regression_asset/prediction_interval.png style = "zoom:20%"></div>


- Confidence interval (CI): **Given the data**, the 90\% PI is the $2w_{CI}(x)$ area centered with the observed best estimator (i.e. $[f^{*}_{obs}(x)-w_{CI}(x), f^*_{obs} +w_{CI}(x)]$) that where the **true best estimator** have 90% percent chance to fall into.
  - $2w_{CI}(x)$ means the width of the CI area varies with x.
  

    <div  align="center"><img src=./linear_regression_asset/confidence_interval.png style = "zoom:20%"></div>


- The exact equation of CI and PI is given by:
    <div  align="center"><img src=./linear_regression_asset/equation_for_ci_and_pi.png style = "zoom:20%"></div>

  
Note: CI for estimator seems different from CI for parameter. Eg. the 95% CI for slope $\beta_1$ is just $[\beta^{obs}_1\pm 1.96\cdot\mathrm{SE}_{\beta_1}]$ , 90% CI for intercept $\beta_0$ is just $[\beta^{obs}_0\pm 1.96\cdot\mathrm{SE}_{\beta_0}]$. Ref: [Econometrics](https://www.econometrics-with-r.org/5-2-cifrc.html), [Gatech_slides](https://www2.isye.gatech.edu/~yxie77/isye2028/lecture12.pdf), 


