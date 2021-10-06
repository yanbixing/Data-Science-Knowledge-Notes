# Linear Regression Basics

## 0. Definition


- $\mathcal{S}$: the dataset contain $m$ samples.
  - i-th sample is denoted as $(\boldsymbol{x}_i, y_i)$.
  - j-th feature of the input of i-th sample is denoted as $\boldsymbol{x}^{(j)}$.
- $\hat{y_i} :=\hat{y}(x_i) := f(x_i)$: the estimated/prediction value according to estimator $f$.
- $\sigma(y):=\mathrm{SD}(y):=\sqrt{\mathrm{Var}(y)}$: population standard deviation.

### 0.1. Frequent equations

- Covariance of (x,y)
$$\begin{aligned} \mathrm{Cov}(x,y) &= \mathbb{E}[ (x-\bar{x})(y-\bar{y}) ]\\
&= \mathbb{E}(xy - \bar{x}y-x\bar{y}+\bar{x}\bar{y})\\
&= \mathbb{E}(xy) - \mathbb{E}(x)\mathbb{E}(y) - \mathbb{E}(x)\mathbb{E}(y) + \mathbb{E}(x)\mathbb{E}(y) \\
& = \mathbb{E}(xy) - \mathbb{E}(x)E(y)
\end{aligned}$$
- Correlation coefficient of (x,y)
$$\begin{aligned} \mathrm{r_{cov}}(x,y) &:= \frac{\mathrm{Cov}(x,y)}{\sigma(x)\sigma(y)} \\ &= \frac{[ \frac{1}{m}\sum^m_{i=1}(x_i-\bar{x})(y_i-\bar{y}) ]}{ \sqrt{\ \frac{1}{m}\sum^m_{i=1}(x_i-\bar{x})^2} \sqrt{ \frac{1}{m}\sum^m_{i=1}(y_i-\bar{y})^2} }
\end{aligned}$$


## 1. Basics: stats related to linear regression

### 1.1. Variances

- (Total )Variance of y variable: 
  $$\mathrm{Var}(y):=\mathrm{Var_{tot}}(y) := \frac{1}{m} \sum^m_{i=1} ( y_i - \bar{y} )^2$$
- Explained variance of y variable: 
  $$\mathrm{Var_{exp}}(y,\hat{y}) := \frac{1}{m} \sum^m_{i=1} ( \hat{y}_i - \bar{y} )^2$$
- Residual variance of y variable: 
  $$\mathrm{Var_{res}}(y,\hat{y}) := \frac{1}{m} \sum^m_{i=1} \varepsilon^2_i := \frac{1}{m} \sum^m_{i=1} ( \hat{y}_i - y_i )^2$$
  - $\varepsilon_i := y_i - f(x_i)$: **residue** of prediction on i-th sample.
  - $\mathrm{Var_{res}}$ is also call "Residual error"
    - $\sum^m_{i=1} \varepsilon^2_i$ is called "sum squared error" (SSE)
  


### 1.2. R-squared score: $R^2$

R-squared is also called "coefficient of determination" Ref: [Stattrek](https://stattrek.com/regression/linear-regression.aspx?Tutorial=AP)

#### 1.2.1. Definition:

The R-squared score is defined as:

$$R^2 := \frac{\mathrm{Var_{exp}}(y) }{\mathrm{Var_{tot}}(y) } = \frac{\sum^m_{i=1} ( \hat{y}_i - \bar{y} )^2}{\sum^m_{i=1} ( y_i - \bar{y} )^2 } \in [0,1]$$

**The higher R-squared** score, **the better** the estimator $f$ is. Intuitively, you can understand it as "the better variance we can explain, the less uncertainty remaining in data."




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

**Note:** formula of SEC

Ref: [StackExchange](https://stats.stackexchange.com/questions/85943/how-to-derive-the-standard-error-of-linear-regression-coefficient), [Wiki-Linear_regression](https://en.wikipedia.org/wiki/Simple_linear_regression#Normality_assumption)


The SE of can be deducted from the residual error: 


$$\mathrm{SE}_{\beta^{(1)}} = \frac{1}{\sqrt{m-2}}\sqrt{\frac{\frac{1}{m}\sum^m_{i=1} (y_i - f(x_i))^2 }{\frac{1}{m} (x_i-\bar{x})^2 }} = \frac{1}{\sqrt{m-2}}\sqrt{\frac{\mathrm{Var_{res} }(y,\hat{y})}{\mathrm{Var}(x)}}$$



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
     - Different kinds of hypothesis testing, Ref: [Wiki-Normality_test](https://en.wikipedia.org/wiki/Normality_test)
       - Most methods check [kurtosis](https://en.wikipedia.org/wiki/Kurtosis) and [skewness](https://en.wikipedia.org/wiki/Skewness).
         - E.g. [D'Agostino's K-squared test](https://en.wikipedia.org/wiki/D%27Agostino%27s_K-squared_test), [Jarque–Bera test](https://en.wikipedia.org/wiki/Jarque%E2%80%93Bera_test)
       - [kurtosis](https://en.wikipedia.org/wiki/Kurtosis) [kəˈtəʊsɪs]:
         - ${\displaystyle \operatorname {Kurt} [X]=\operatorname {E} \left[\left({\frac {X-\mu }{\sigma }}\right)^{4}\right]={\frac {\operatorname {E} \left[(X-\mu )^{4}\right]}{\left(\operatorname {E} \left[(X-\mu )^{2}\right]\right)^{2}}}={\frac {\mu _{4}}{\sigma ^{4}}},}$
         - Describe the "sharpness","peakedness" of the peak. 
           - Normal distribution, Kurt = 3
       - [skewness](https://en.wikipedia.org/wiki/Skewness) ['skjuːnɪs]:
         - ${\displaystyle {\tilde {\mu }}_{3}=\operatorname {E} \left[\left({\frac {X-\mu }{\sigma }}\right)^{3}\right]={\frac {\mu _{3}}{\sigma ^{3}}}={\frac {\operatorname {E} \left[(X-\mu )^{3}\right]}{(\operatorname {E} \left[(X-\mu )^{2}\right])^{3/2}}}={\frac {\kappa _{3}}{\kappa _{2}^{3/2}}}}$
         - Describe the symmetry of the shape, i.e. whether the shape is tilt [tɪlt], i.e. the length difference between different tail.
           - Normal distribution, skewness = 0
         - Positive skew = right skew = right tail is longer.
           - Mean>median>mode
           - Understanding: 
             - long tail in right, the sample in right have larger weight, will drag the mean to right. 
             - The original center point (median) will be sparser so the height will drop, the left side density (height) will increase, the right side will decrease, so the median will right shift compare to mode.
         - Negative skew = left skew = left tail is longer.
           - Mean< median < mode
         - <div  align="center"><img src=https://upload.wikimedia.org/wikipedia/commons/c/cc/Relationship_between_mean_and_median_under_different_skewness.png style = "zoom:40%"></div>
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


