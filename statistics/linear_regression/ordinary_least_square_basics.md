# Ordinary least square (OLS)

Here, we will use the most simple 2D (x,y) case as example.



## 1. Definition

OLS is a method to fit the linear model.


OLS, "O" for ordinary, ordinate, i.e. the y-axis. I.e. it minimizes the squared error on y-axis.

$$\boldsymbol{\beta} =\underset{\beta}{\argmin} \left [ \sum^m_{i=1} (y_i - f(x_i))^2 \right ]$$

where $\mathrm{Var_{res}}(y) : = \frac{1}{m} \sum^m_{i=1} [y_i - f(x_i)]^2$ and $f(x) = \sum_j \beta^{(j)} x^{(j)}$

Illustration:

<div  align="center"><img src=https://www.cerebralmastication.com/wp-content/uploads/2010/09/OLS1-280x300.png style = "zoom:80%"></div>

## 2. Coefficients values

The format of our function is:
$$f(x) = \beta^{(0)} + \beta^{(1)}x$$

### 2.1. Summary

The values of coefficient is:

$$ \left\{
    \begin{aligned}
    &\beta^{(0)}= \bar{y} - \beta^{(1)} \bar{x} \\
    &\beta^{(1)} = \frac{\mathrm{Cov}(x,y) }{ \mathrm{Var}(x) } = r_{\mathrm{cov}}(x,y) \frac{\sigma(y)}{\sigma(x)} 
    \end{aligned}
    \right. $$

where: $r_{\mathrm{cov}}(x,y):= \frac{\mathrm{Cov}(x,y)}{\sigma(x)\sigma(y)} := r_{xy}$ is the correlation coefficient.

### 2.2 Deduction process

For OLS, the loss is the squared error on y direction (ordinary square): 
$$L_{OLS}(x):=\mathrm{Var_{res}} =  \frac{1}{m} \sum^m_{i=1} (y_i - f(x_i))^2$$


Minimization $\boldsymbol{\beta} = \underset{\boldsymbol{\beta}}{\argmin}L_{OLS}(x)$ can be solve with the partial derivative equation: $\partial_{\beta^{(i)}}L(x) =0$:

- $\beta^{(0)}$:
      $\begin{aligned}
        & \partial_{\beta^{(0)}}L_{OLS}(x) = 0 \\ 
        \Rightarrow & \sum^m_{i=1} 1\cdot[y_i - f(x_i)] = 0 \\ 
        \Rightarrow & \sum^m_{i=1} y_i - (\beta^{(0)} + \beta^{(1)}x_i) = 0 \\ 
        \Rightarrow & \beta^{(0)} = \frac{1}{m} \sum^m_{i=1}(y_i -\beta^{(1)}x_i)\\
        \Rightarrow & \beta^{(0)} = \bar{y} - \beta^{(1)} \bar{x}
      \end{aligned}$

- $\beta^{(1)}$:
    $\begin{aligned}
      & \partial_{\beta^{(0)}}L_{OLS}(x) = 0 \\ 
      \Rightarrow & \sum^m_{i=1} x_i[y_i - f(x_i)] = 0 \\ 
      \Rightarrow & \sum^m_{i=1} x_i \{ y_i -  [ \bar{y} - \beta^{(1)} \bar{x} + \beta^{(1)}x_i]  \}  = 0 \\ 
      \Rightarrow & \sum^m_{i=1} ( x_iy_i -  \bar{x}\bar{y} )= \beta^{(1)} \sum^m_{i=1} ( x_i^2 - \bar{x}^2 ) \\ 
      \Rightarrow & \sum^m_{i=1} [( x_i -  \bar{x})(y_i -\bar{y} )]= \beta^{(1)} \sum^m_{i=1} ( x_i- \bar{x} )^2  \\
      \Rightarrow & \beta^{(1)} = \frac{ \frac{1}{m} \sum^m_{i=1} [( x_i -  \bar{x})(y_i -\bar{y} )] }{ \frac{1}{m} \sum^m_{i=1} ( x_i- \bar{x} )^2 } = \frac{\mathrm{Cov}(x,y) }{ \mathrm{Var}(x) } = r_{\mathrm{cov}}(x,y) \frac{\sigma(y)}{\sigma(x)} \\
    \end{aligned}$
<!-- - Therefore  -->
  
## 3. Statistics under OLS

### 3.1. Residue error

$$\bar{\varepsilon} = 0$$

**proof:**

$\begin{aligned}
  \bar{\varepsilon} & = E(y_i) - E[f(x_i)] = \bar{y} - \beta^{(1)}\bar{x}-\beta^{(0)}\\
  & = \bar{y} - \beta^{(1)}\bar{x} - (\bar{y} - \beta^{(1)} \bar{x} )\\
  & =0
\end{aligned}$ 

### 3.2. Variances


#### 3.2.1. Expression of residual variance

$$\mathrm{Var_{res}}(y,\hat{y}):= \mathbb{E}(\varepsilon^2) = \mathrm{Var}(\varepsilon) $$


**proof:**

$\mathrm{Var_{res}}(y,\hat{y}) := \frac{1}{m} \sum^m_{i=1} \varepsilon^2_i \underset{OLS}{=} \sum_i (\varepsilon_i - \bar{\varepsilon})^2 = \mathrm{Var}(\varepsilon) $


#### 3.2.2. Relation between variances

$$\mathrm{Var_{res}}(y) = \mathrm{Var_{tot}}(y) - \mathrm{Var_{exp}}(y)$$

**proof:**

<!-- **Proof:** (use $f(x) = \beta^{(0)} + \beta^{(1)}x$ as example.) -->

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

Thus $\sum_i (y_i-\hat{y}_i)(\hat{y}_i - \bar{y}) =0$ and we have:

$$\mathrm{Var_{tot}}(y) = \mathrm{Var_{res}} + \mathrm{Var_{exp}}$$

#### 3.2.2. Variances and correlation


$$ \left\{
    \begin{aligned}
    &\mathrm{Var_{exp}}(y, \hat{y}) = r^2_{xy} \mathrm{Var}(y)\\
    &\mathrm{Var}(\varepsilon) = \mathrm{Var_{res}}(y, \hat{y}) =   (1-r^2_{xy})\mathrm{Var}(y)
    \end{aligned}
\right. $$

**Important deduction:** Under OLS: 

- the SSE (sum of squared error, euqals residual variance) is **irrelevant** to the scale of $x$.
- The SSE is only determined by correlation $r_{xy}$ and variance of $y$.
  - As $r_{xy}\rightarrow 1$, SSE $\mathrm{Var}(\varepsilon)\rightarrow0$.

**proof:**

$\begin{aligned}
  \mathrm{Var_{exp}}(y, \hat{y}) &= \sum_i (\hat{y}_i - \bar{y})^2 = \sum_i \left[( \beta^{(0)} + \beta^{(1)}x_i) - \bar{y} \right]^2\\
  & = \sum_i \left[ ( \bar{y} - \beta^{(1)}\bar{x} ) + \beta^{(1)}x_i - \bar{y} \right]^2\\
  & = \sum_i \left [ ( \bar{y} - \bar{y}) + \beta^{(1)}(x_i - \bar{x} )   \right ]^2 \\
  & = \left[\beta^{(1)} \right]^2 \sum_i \left[ (x_i - \bar{x} )   \right]^2 \\&=  [r_{\mathrm{cov}}(x,y) \frac{\sigma(y)}{\sigma(x)} ]^2 \mathrm{Var(x)} \\
  &= r^2_{xy} \frac{\mathrm{Var}(y)}{\mathrm{Var}(x)} \mathrm{Var}(x)\\
  &=r^2_{xy} \mathrm{Var}(y)
\end{aligned}$
<!-- =r^2_{xy} \mathrm{Var_{tot}}(y) -->

$\begin{aligned}
  \mathrm{Var_{res}} & = \mathrm{Var_{tot}}(y) -  \mathrm{Var_{exp}} (y)\\
  & = \mathrm{Var}(y)  - r^2_{xy} \mathrm{Var}(y)\\
  & = (1- r^2_{xy} )\mathrm{Var}(y)
\end{aligned}$

### 3.3.  R-squared score: $R^2$

$$R^2 = r_{x,y}^2$$

I.e. Under OLS, R-squared is the square of the correlation coefficient.

**proof:**

$R^2 :=  \frac{\mathrm{Var_{exp}}(y) }{\mathrm{Var_{tot}}(y) } = \frac{r^2_{xy} \mathrm{Var}(y) }{\mathrm{Var}(y)} = r^2_{xy}$





#### 3.4. Hypothesis related stats

$$\mathrm{SE}_{\beta^{(1)}} \sim \frac{1}{\sqrt{m}} \frac{\sigma(\varepsilon)}{\sigma(x)}\sim \frac{1}{\sqrt{m}} \sqrt{1-r^2_{xy}} \frac{\sigma(y)}{\sigma(x)}$$

$$z^{obs} = \frac{\beta^{(1)obs} - \beta_{H_0}}{\mathrm{SE}_{\beta^{(1)}}} = \sqrt{m} \frac{r_{xy}}{\sqrt{1-r^2_{xy}}}$$


**memorization tips for SE:** (not strict, just help to memorize)

- $\sigma(\varepsilon)$: higher error in y, the more inaccurate on $\beta^{(1)}$
- $\sigma(x)$: when x scale increase, the value of $\beta^{(1)}$ shrinks, so as the uncertainty on $\beta^{(1)}$.
- $\frac{1}{\sqrt{m}}$ similar to mean value, more sample, more precise we can estimate the parameter.
- $\frac{1}{\sqrt{m-2}}$ 2 points can define a line, so we need sacrifice 2 degree of freedom.

**proof:**

Since:

$$\mathrm{SE}_{\beta^{(1)}} = \frac{1}{\sqrt{m-2}}\sqrt{\frac{\frac{1}{m}\sum^m_{i=1} (y_i - f(x_i))^2 }{\frac{1}{m} (x_i-\bar{x})^2 }} = \frac{1}{\sqrt{m-2}}\sqrt{\frac{\mathrm{Var_{res} }(y,\hat{y})}{\mathrm{Var}(x)}}$$

For OLS, since $\mathrm{Var_{res} }(y,\hat{y}) = \sigma^2(\varepsilon) = (1-r^2_{xy})\sigma^2(y)$, thus

$$\mathrm{SE}_{\beta^{(1)}} \sim \frac{1}{\sqrt{m}} \frac{\sigma(\varepsilon)}{\sigma(x)}\sim \frac{1}{\sqrt{m}} \sqrt{1-r^2_{xy}} \frac{\sigma(y)}{\sigma(x)}$$



Thus, under OLS and $H_0:\beta^{(1)}=0$

$$z^{obs} = \frac{\beta^{(1)obs} - \beta_{H_0}}{\mathrm{SE}(\beta^{(1)})} = \frac{r_{xy} \frac{\sigma(y)}{\sigma(x)} }{\frac{1}{\sqrt{m}} \sqrt{1-r^2_{xy}} \frac{\sigma(y)}{\sigma(x)}} = \sqrt{m} \frac{r_{xy}}{\sqrt{1-r^2_{xy}}}$$


## 4. Statistical interpretation on OLS

If the **error is normally distributed**, the **least square estimator** is identical to the **MLE estimators** for **linear model**. Ref:[Wiki-Least_sqaures](https://en.wikipedia.org/wiki/Least_squares)

**Deduction**: [StackExchange](https://stats.stackexchange.com/questions/143705/maximum-likelihood-method-vs-least-squares-method)

- When assuming error is normally distributed, we have:
  $$y=w^T X +\epsilon \quad\text{ where }\epsilon\thicksim N(0,\sigma^2)$$
- The probability of one individual sample $(x_i,y_i)$ is:
  $$P(y|w, X)=\mathcal{N}(y|w^TX, \sigma^2I) = \frac{1}{ \sqrt{2\pi\sigma^2}}\exp(-\frac{(y_i-wx_i)^2)}{2\sigma^2})$$
- The probability for the whole dataset is:
  $$\begin{aligned}
  &L(y^{(1)},\dots,y^{(N)};w, X^{(1)},\dots,X^{(N)}) \\ =& \prod_{i=1}^N \mathcal{N}(y^{(i)}|w^TX^{(i)}, \sigma^2I) \\ =& 
  \frac{1}{(2\pi)^{\frac{N}{2}}\sigma^N}exp(-\frac{\sum_{i=1}^N(y_i-wx_i)^2}{2\sigma^2}).
  \end{aligned}$$
- $\because MSE = \frac{1}{N} \sum_{i=1}^N(y_i-wx_i)^2$
  - So, maximize the likelihood is equal to minimize the mean square error (MSE).


## 5. FAQs


### 5.1. Influence on fitting results

For OLS, how are the coefficient and t-score (or z-score) affected when the following cases happens?

#### 5.1.1 Data is duplicated

Not change:

- Statistics:
  - Population mean $\bar{x}$,$\bar{y}$
  - Population variance and SD $\mathrm{Var}(x), \sigma(x)$, $\mathrm{Var}(y),\sigma(y)$
  - Covariance $\mathrm{Cov}(x,y)$
  - Correlation $r_{xy} = \frac{\mathrm{Cov}(x,y)}{\sigma(x)\sigma(y)}$
  - R-sqaured: $R^2 = r^2_{xy}$
- OLS related:
  - Observed/expectation/mean $\boldsymbol{\beta}$ ($\beta^{(0)},\beta^{(1)}$)
    - Since $\beta$ are determined by Covariance, variance and mean value.

Change:

- Statistics:
  - $\mathrm{SE}_{\beta^{(1)}} \propto \frac{1}{\sqrt{m}}$ will decrease.
  - $z^{obs} = \frac{\beta^{(1)obs}-0 }{\mathrm{SE}_{\beta^{(1)}}} \propto \sqrt{m}$ increase.
    - observed mean $\beta^{(1)obs}$ not change, SE will change.


**Note: how to understand the change of SE and z-score here?**

Here $\mathrm{CI} = [\beta^{(1)obs} - \mathrm{SE}_{\beta^{(1)}}, \beta^{(1)obs} + \mathrm{SE}_{\beta^{(1)}}]$, 

$m\uparrow \Rightarrow \mathrm{SE} \downarrow$ means CI is smaller.

Generally, then the smaller CI brought by large amount of sample means the result is more precise.

But **here**, we duplicate the data, the smaller CI is just a mathematic trick; the $\beta^{(1)obs}$ doesn't change, we didn't take in more info. 

The smaller CI make that there is more chance 0 fall out of the CI, i.e. **more chance to reject null hypothesis**, more chance on **type-1 error**.

If we don't duplicate data but sample more data, we will have no such issue, i.e. we don't increase the chance of type-1 error. This is because, when sampling more data, $\beta^{(1)obs}$ will be more precise, i.e. the $\beta^{(1)obs}$ will not be the same, it will be closer to the true value $\beta^{(1)true}$.

**Proof:**


$\mathrm{Cov}_m(x,y) = \frac{1}{m}[\sum^m_{i=1} (x_i-\bar{x})(y_i-\bar{y})] = \frac{1}{2m}[2\cdot\sum^m_{i=1} (x_i-\bar{x})(y_i-\bar{y})]=\mathrm{Cov}_{2m}(x,y)$

$\mathrm{Var}_m(y)= \frac{1}{m} \sum^m_{i=1} ( y_i - \bar{y} )^2= \frac{1}{2m} 2\cdot \sum^m_{i=1} ( y_i - \bar{y} )^2 = \mathrm{Var}_{2m}(y)$

<!-- $\sigma(\varepsilon)= \sqrt{ \frac{1}{m} \sum^m_{i=1} \varepsilon_i^2 } = \sqrt{ \frac{1}{2m} 2 \cdot \sum^m_{i=1} \varepsilon_i^2 }$ -->

Similarly, $\mathrm{Var}_m(y)= \mathrm{Var}_{2m}(y)$. And thus $\sigma_m(y)= \sigma_{2m}(y)$, $\sigma_m(x)= \sigma_{2m}(x)$

Since correlation is determined by covariance and SD, $r_{xy} = \frac{\mathrm{Cov}_m(x,y)}{\sigma(x)\sigma(y)}$, $r_{xy}$ not change.



Since $\boldsymbol{\beta}$ can be written as function of variance and covariance:
  $$ \left\{
    \begin{aligned}
    &\beta^{(0)}= \bar{y} - \beta^{(1)} \bar{x} \\
    &\beta^{(1)} = \frac{\mathrm{Cov}(x,y) }{ \mathrm{Var}(x) } = r_{\mathrm{cov}}(x,y) \frac{\sigma(y)}{\sigma(x)} 
    \end{aligned}
    \right.
  $$
$\boldsymbol{\beta}$ will change when duplication happens.

  
Thus, coefficients $\beta^{(0)}$ and $\beta^{(1)}$ NOT change when duplicating data.

 $$R^2 = r_{x,y}^2 = \left[\frac{\mathrm{Cov}(x,y)}{\sigma(x)\sigma(y)}\right]^2 = \frac{\mathrm{Cov}^2(x,y)}{\mathrm{Var}(x)\mathrm{Var}(y)}$$

$\mathrm{Cov}(x,y)$, $\mathrm{Var}(x)$, $\mathrm{Var}(y)$ will not change, thus $R^2$ will not change.
  
  $$\mathrm{SE}_{\beta^{(1)}} \sim \frac{1}{\sqrt{m}} \frac{\sigma(\varepsilon)}{\sigma(x)}$$

Similar to above, variance doesn't change when duplicating;Population standard deviation doesn't change when duplicating.


Thus, $\mathrm{SE}_{\beta^{(1)}} \sim \frac{1}{\sqrt{m}}$ will drop. I.e. $\mathrm{SE}_{\beta^{(1)}, 2m} = \frac{1}{\sqrt{2}} \mathrm{SE}_{\beta^{(1)},m}$ 
  


$$z^{obs} = \frac{\beta^{(1)obs} - \beta_{H_0}}{\mathrm{SE}(\beta^{(1)})}$$

Since $\beta^{(1)obs}$ will not change, $\mathrm{SE}_{\beta^{(1)}, 2m} = \frac{1}{\sqrt{2}} \mathrm{SE}_{\beta^{(1)},m}$

Thus, $z^{obs}_{2m} = \sqrt{2} z^{obs}_m$, z (or t) score increases.

#### 5.2.2 Swith X and Y


**Assume**:

- Before switch, the function is: $y=\beta^{(0)}+\beta^{(1)}x$
- After switch, the function is: $x = \alpha^{(0)}+\alpha^{(1)}y$

**Summary**:

Not change:

- Statistics:
  - Population mean $\bar{x}$,$\bar{y}$
  - Population variance and SD $\mathrm{Var}(x), \sigma(x)$, $\mathrm{Var}(y),\sigma(y)$
  - Covariance $\mathrm{Cov}(x,y)$
  - Correlation $r_{xy} = \frac{\mathrm{Cov}(x,y)}{\sigma(x)\sigma(y)}$
  - R-sqaured: $R^2 = r^2_{xy}$

- OLS related:
  - $z^{obs} = \frac{\beta^{(1)obs}-0 }{\mathrm{SE}_{\beta^{(1)}}} = \sqrt{m} \frac{r_{xy}}{\sqrt{1-r^2_{xy}}}$

No specific pattern:

- OLS related:
  - Coefficients $\boldsymbol{\beta}, \boldsymbol{\alpha}$
  - SE of coefficients, e.g.: $\mathrm{SE}_{\beta^{(1)}}, \mathrm{SE}_{\alpha^{(1)}} $

**Understanding:**: the results is reasonable:

- Axis switching NOT affect statistics, like the covariance and variance, is because those statistics reflect the relation between x,y array, they are not related to axis position.
- OLS params is affected by switching axis is because: OLS minimize error on y-axis, after switching, it minimize error on x-axis. 
  - For TLS, the relative position of the line will not change, the slope will be inverse.

**proof:**

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


## Deep-dive-1. $R^2$, $Var_{tot/res/exp}$ and $r$

Ref:[Wiki-R_squared](https://en.wikipedia.org/wiki/Coefficient_of_determination), [Wiki-R_squared: explained_var](https://en.wikipedia.org/wiki/Coefficient_of_determination#As_explained_variance), [Wiki-R_squared: squared_corr](https://en.wikipedia.org/wiki/Coefficient_of_determination#As_squared_correlation_coefficient)

The formal definition of $R^2$ is as following:

$$\begin{aligned}
  R^{2}(y^*,\hat{y}) := 1 - \frac{Var_{res}(y^*,\hat{y})}{Var_{tot}(y^*)} = 1-\frac{\sum_i(y_i^*-\hat{y}_i)^2}{\sum_i(y_i^*-\bar{y}_i)^2}
\end{aligned}$$

$$\begin{cases}
  Var_{tot}(y^*) : = \frac{1}{m} \sum^m_{i=1}(y_i^*-\bar{y^*}_i)^2\\
  Var_{res}(y^*,\hat{y}) : = \frac{1}{m} \sum^m_{i=1}(y_i^*-\hat{y}_i)^2
\end{cases}$$

**For some cases like simple linear regression**, explained variance equal to the difference between total variance and residue variance:

$$Var_{exp}(y^*,\hat{y}) = Var_{tot}(y^*) -  Var_{res}(y^*,\hat{y}) $$

$$\begin{aligned}
  R^{2}(y^*,\hat{y}) = \frac{Var_{exp}(y^*,\hat{y})}{Var_{tot}(y^*)}
\end{aligned}$$


**For simple linear regression:**

$$\begin{aligned}
  R^{2}(y^*,\hat{y}) = r^2(y^*,\hat{y}))
\end{aligned}$$
where $r$ is correlation coefficient.


