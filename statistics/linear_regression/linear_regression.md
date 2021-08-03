# Linear Regression

## 0. Definition


- $\mathcal{S}$: the dataset contain $m$ samples.
  - i-th sample is denoted as $(\boldsymbol{x}_i, y_i)$.
  - j-th feature of the input of i-th sample is denoted as $\boldsymbol{x}^{(j)}$.
- $\hat{y_i} :=\hat{y}(x_i) := f(x_i)$: the estimated/prediction value according to estimator $f$.

## 1. Basics

### 1.0. Variances related to linear regression:

- (Total )Variance of y variable: 
  $$\mathrm{Var}(y):=\mathrm{Var_{tot}}(y) := \frac{1}{m} \sum^m_{i=1} ( y_i - \bar{y} )^2$$
- Explained variance of y variable: 
  $$\mathrm{Var_{exp}}(y) := \frac{1}{m} \sum^m_{i=1} ( \hat{y}_i - \bar{y} )^2$$
- Residual variance of y variable: 
  $$\mathrm{Var_{res}}(y) := \sum_i \varepsilon^2_i := \frac{1}{m} \sum^m_{i=1} ( \hat{y}_i - y_i )^2$$
  - $\varepsilon_i$: **residue** of prediction on i-th sample.



### 1.1. OLS: Ordinary least square

A method to fit the linear model.
$$\boldsymbol{\beta} =\underset{\beta}{\argmin} \left [ \sum^m_{i=1} (y_i - f(x_i))^2 \right ]$$

where $\mathrm{Var_{res}}(y) : = \frac{1}{m} \sum^m_{i=1} (y_i - f(x_i))^2$ and $f(x) = \sum_j \beta^{(j)} x^{(j)}$

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
    &\beta^{(0)}= \bar{y} - b^{(1)} \bar{x} \\
    &\beta^{(1)} = \frac{\mathrm{Cov}(x,y) }{ \mathrm{Var}(x) } = r_{\mathrm{cov}}(x,y) \frac{\sigma(y)}{\sigma(x)} 
    \end{aligned}
    \right.
  $$
  - where: $r_{\mathrm{cov}}(x,y):= \frac{\mathrm{Cov}(x,y)}{\sigma(x)\sigma(y)} := r_{xy}$ is the correlation coefficient.


### 1.2. R-squared score: $R^2$

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


