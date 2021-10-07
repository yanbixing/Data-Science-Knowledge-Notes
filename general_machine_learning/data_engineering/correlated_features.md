# Correlated features

## 1. Basics

### 1.1. Concept differentiation: Correlation, collinearity and multi-collinearity

- Correlation: a statistic to describe the linearity (linear relation) between **two** variables.
  - VIF is also a statistic to describe the linearity, the difference is that VIC describe the linear relation between two **or more** variables.
- Collinearity: a problem that two or more independent variable (input features) have strong linear relation.
  - Identification method: 
    - correlation (collinearity between two variable)
    - VIF (collinearity between two or more variable)
- Multi-collinearity: a special case of collinearity: 3 or more independent variable have strong linear relation, even no pairs have strong linear relation.
  - E.g. when $x^{(1)} \sim a x^{(2)} + b x^{(3)}$ $r_{x^{(1)},ax^{(2)}+bx^{(3)}}\rightarrow 1$ but $r_{x^{(1)},x^{(2)}}, r_{x^{(1)},x^{(3)}}, r_{x^{(2)},x^{(3)}} \rightarrow 0$.
  - Identification method: VIF

ref: [Blog-QuantifyingHealth](https://quantifyinghealth.com/correlation-collinearity-multicollinearity/)

### 1.2. VIF:  Variance inflation factor 

Assume our fitting problem is:

$$y = \sum^n_{i=0} \beta^{(i)} x^{(i)}$$

VIF measures the linear relation between one independent variable with other variables, i.e.:

- whether one independent variable can be fitted with other variables, 
- through the R-squared score between the true value and fitting prediction:

$$\hat{x}^{(j)} = \sum_{i\neq j} \alpha^{(i)} x^{(i)}$$

$$\mathrm {VIF} _{j}:={\frac {1}{1-R_{j}^{2}}}$$

where $R_{i}^{2}$ is the the R-square score between $x^{(j)}$ and $\sum_{i\neq j} \alpha^{(i)} x^{(i)}$, 

$$\begin{aligned}
  R_{j}^{2} := R^2(x^{*(j)},\hat{x}^{(j)} )
\end{aligned}$$

**Analysis method:** the higher VIF the more collinearity.

- the higher (multi-)collinearity
- the better $x^{*(j)}$ can be fitted with other variable ($\hat{x}^{(j)} = \sum_{i\neq j} \alpha^{(i)} x^{(i)}$)
- the higher R-squared score $R^2(x^{*(j)},\hat{x}^{(j)} )$, the lower denominator $(1-R_j^2)$
- the higher $\mathrm {VIF} _{j}$


Ref: [Wiki-VIF](https://en.wikipedia.org/wiki/Variance_inflation_factor)

#### 1.2. Appendix: R-squared

From [linear-ols_basics, Sec DD1.](../statistics/../../statistics/linear_regression/ordinary_least_square_basics.md)

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


Ref:[Wiki-R_squared](https://en.wikipedia.org/wiki/Coefficient_of_determination#In_a_multiple_linear_model), [Wiki-R_squared: explained_var](https://en.wikipedia.org/wiki/Coefficient_of_determination#As_explained_variance), [Wiki-R_squared: squared_corr](https://en.wikipedia.org/wiki/Coefficient_of_determination#As_squared_correlation_coefficient)

## 2. Impact on different models

- The **variance/uncertainty in parameter is high**. E.g.:
  - $y^* = x^{(1)} + x^{(2)} + \varepsilon$ is the true generative function, $x_1$ and $x_2$ are two features with same value, $\varepsilon$ is noise.
  - $\hat{y} = \beta^{(1)} x^{(1)} + \beta^{(2)} x^{(2)}$ then $(\beta^{(1)}, \beta^{(2)})$ could take $(1,1),(2,0),(0,2),\dots$
- When the parameters can be assigned randomly, i.e. not stable, **small change in training data may cause large change in parameter**. (consistent with the statement "variance" is high.)
- Also, if coefficients is not stable, 
  - it will be **harder to interpret the model** and thus hard to detect problems in model.
  - it will be **harder to select the correct model**.
  - it will **weaken the performance/power of your model**.

Refs: [Blog-mintlab](https://blog.minitab.com/en/adventures-in-statistics-2/what-are-the-effects-of-multicollinearity-and-when-can-i-ignore-them), [Blog-StatisticsByJim](https://statisticsbyjim.com/regression/multicollinearity-in-regression-analysis)


## 2.1. Influence on Naive Bayes

- Conclusion: the more features correlated, the more importance/over-emphasis of the feature.
  - The naive bayes use the product of features. E.g. If two features are correlated, the effect is equal to the square of one feature.
  - In the training process, the correlated features will be preferred to be optimized.
  - In the prediction process, the correlated features give power (e.g. $x \rightarrow x^2$) influence.



[naive_bayes_model-Sec 3.1](../models/naive_bayes_model.md)

## 3. Solution methods:

[Blog-mintlab](https://blog.minitab.com/en/adventures-in-statistics-2/what-are-the-effects-of-multicollinearity-and-when-can-i-ignore-them)

- Remove correlated features
  - Drop features according to statistics like corr or VIF.
  - Regularization
- Combine correlated features
  - Manually add them together
  - Dimension reduction (like PCA))

### 3.1. Add Regularization

For identical feature, 
- L1 $\Re$ spreads weight arbitrarily. 
- L2 $\Re$ spreads weight evenly.

For linear correlated feature:

- L1 $\Re$ assign all weight to the feature with largest scale, all others' weights are zero. 
- L2 $\Re$ prefers variable with larger scale - spread weights **proportional** to scale.<br>i.e. larger scale get larger weights, smaller scale feature get smaller (proportional) weights but **not zero**.
- Note: both L1/L2 $\Re$ give more weight to larger scale, this is because regularization always punish on weight, and weights on larger scale feature are smaller, thus get preferred.

[Regularization-Sec 1.2.2.](../model_training/regularization.md)

