# Regularization

## 1. L1 and L2

TBD.

## 2. Bias-Variance Tradeoff

TBD.

### 2.1 How does "bias-variance tradeoff" come from ?

- 'Noises' always exist in real word data. i.e. even for same feature values $x$, we usually observe different label value $y$. Thus, the function between label and feature can be represented as:
$$y=f(x)+\varepsilon$$
where: 
  - $\varepsilon$ has $E(\varepsilon) = \bar{\varepsilon} = 0 $ and $Var(\varepsilon) = E(\varepsilon - E(\varepsilon)) = \bar{\varepsilon^2} - \bar{\varepsilon}^2 = \sigma^2$.
  - f(x) stands for the "real" function between $x$ and $y$.

- In machine learning, we aims to find $\hat{f}(x;D)$ that minimize ?(Training?) Risk:
$$(y-\hat{f}(x;D))^2 = (f(x) - \hat{f}(x))^2$$
  - Risk is another name of "loss"


Temp Q:
what is the ? risk name?
what does math mark ";" like ";D" means? parameterized?

