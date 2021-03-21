# Regularization

## 1. L1 and L2

TBD.

## 2. Bias-Variance Tradeoff

TBD.

### 2.1 How does "bias-variance tradeoff" come from ?

#### 2.1.1 Bias-variance decomposition

- 'Noises' always exist in real word data. i.e. even for same feature values $x$, we usually observe different label value $y$. Thus, the function between label and feature can be represented as:
$$\begin{aligned}
y&=f(x)+\varepsilon \\&:= f+\varepsilon
\end{aligned}$$
where: 
  - $\varepsilon$ has $E(\varepsilon) = \bar{\varepsilon} = 0$ and $Var(\varepsilon) = E(\varepsilon - E(\varepsilon)) = \bar{\varepsilon^2} - \bar{\varepsilon}^2 = \sigma^2$.
  - f(x) stands for the "real" function between $x$ and $y$.

- With machine learning algorithm $\mathcal{A}$, we can find $h_\mathcal{S}$ through minimizing **empirical risk** (square error here):
$$\begin{aligned}
  \hat{R}_\mathcal{S}(h_\mathcal{S}) &:= (y-h_\mathcal{S})^2 \\
  &= (f - h_\mathcal{S})^2 + 2\varepsilon(f-h_\mathcal{S})+\varepsilon^2
\end{aligned}$$
  - Risk is another name of "loss"
- Then, **the expectation of empirical risk over different sample set $\mathcal{S}$** equals:
$$\begin{aligned}
  \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}[\hat{R}_\mathcal{S}(h_\mathcal{S})] = & \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}[(f - h_\mathcal{S})^2 + 2\varepsilon(f-h_\mathcal{S})+\varepsilon^2] \\
  = &\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}[(f - h_\mathcal{S})^2] + 
  \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}[2\varepsilon(f-h_\mathcal{S})]+
  \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}[\varepsilon^2] \\ 
  = &\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(f^2-2fh_\mathcal{S}+h^2_\mathcal{S}) + 2\bar{\varepsilon}\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(f-h_\mathcal{S})+\bar{\varepsilon^2} \\ 
  = &[f^2-2f\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h_\mathcal{S})+\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h^2_\mathcal{S})] + 0 + \sigma^2 \\
  =& \{ f^2-2f\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h_\mathcal{S}) + [\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h_\mathcal{S})]^2 \} +\\ 
  &+ \{  \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h^2_\mathcal{S})- [\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h_\mathcal{S})]^2  \} +\sigma^2\\
  =& [\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h_\mathcal{S})-f]^2 + \{  \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h^2_\mathcal{S})- [\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h_\mathcal{S})]^2  \} +\sigma^2\\:= & [\underset{\mathcal{S}\sim\mathcal{D}^m}{Bias}(h_\mathcal{S})]^2 +\underset{\mathcal{S}\sim\mathcal{D}^m}{Var}(h_\mathcal{S}) +\sigma^2
\end{aligned}$$ where:$$\underset{\mathcal{S}\sim\mathcal{D}^m}{Bias}(h_\mathcal{S}):=\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h_\mathcal{S})-f$$ and $$ \underset{\mathcal{S}\sim\mathcal{D}^m}{Var}(h_\mathcal{S}) = \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h^2_\mathcal{S})- [\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h_\mathcal{S})]^2$$ <br> **Note**: The expectation $\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}[\hat{R}_\mathcal{S}(h_\mathcal{S})]$ is different from **generalization loss**, generalization/empirical loss is in terms of one certain function. While here in the expectation $\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}[\hat{R}_\mathcal{S}(h_\mathcal{S})]$, $\mathcal{S}$ is a "variable", different $\mathcal{S}$ give different optimized function $h_\mathcal{S}$. <br> **Personal Understanding:** The **bias** and **variance** here actually is a metric to measure the performance of learning **algorithm** $\mathcal{A}$ and **hypothesis class** $\mathcal{H}$, since $\mathcal{H} \overset{\mathcal{A},  \mathcal{S}\in\mathcal{D}}{\rightarrow} h_\mathcal{S}$, i.e. $h_\mathcal{S}$ is found through $\mathcal{A}$).
Ref: [Wiki-Bias-variance tradeoff](https://en.wikipedia.org/wiki/Bias%E2%80%93variance_tradeoff)
#### 2.1.2 Bias-variance tradeoff?

- Conclusion
- Model Complexity:
  - Ra

Temp Q:

what does math mark ";" like ";D" means? parameterized?

bias-variance tradeoff vs. Approximation-Estimation tradeoff


## 3. Appendix - Concept Differentiation

- Risk vs Empirical Risk:
  - Notation: 
    - $\mathcal{D}$: An **unknown joint distribution** that generates $(x,y)$ pairs.
    - $\mathcal{S}$: A finite **training set** with $m$ i.i.d samples $(x_1,...,x_m)$ from $\mathcal{D}$.
    - $h$: A **hypothesis** that chosen from **hypothesis class** $\mathcal{H}$ <br> English: $\mathcal{H}$ is a finite set of functions, $h \in \mathcal{H}$, and $h_\mathcal{S}$ denotes a function that satisfies $\hat{R}_\mathcal{S}(h_\mathcal{S}) = 0$ given by **algorithm** $\mathcal{A}$<br> Ref:[Foundations of Machine Learning, Sec 2.2, P15](https://cs.nyu.edu/~mohri/mlbook/) <br> Consistent with definition of $h_\mathcal{S}$ which is given by $\mathcal{A}$ in [On the Bias-Variance Tradeoff, Sec 2.1, P5](https://arxiv.org/abs/1912.08286) 
    - $\hat{R}_\mathcal{S}(h)$ denotes the **empirical risk** or **empirical error**. <br> $R(h)$ denotes the **generalization risk** or **generalization error**. <br> Ref: [Foundations of Machine Learning, Sec 2.1, P10](https://cs.nyu.edu/~mohri/mlbook/) <br> $\hat{R}(h)$ also denotes the empirical risk, see: [On the Bias-Variance Tradeoff, Sec 2.1, P5](https://arxiv.org/abs/1912.08286) and the older version of *Foundations of Machine Learning (2012)*
    - $\hat{R}_\mathcal{S}(h)$ and $R(h)$ satisfy:
    $$  \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}[\hat{R}_\mathcal{S}(h)]=R(h) $$
    i.e. expectation of empirical error on an i.i.d $\mathcal{S}$ equals generalization error. <br> Ref:  [Foundations of Machine Learning, Sec 2.1, P11](https://cs.nyu.edu/~mohri/mlbook/)
  - <br>Ref: [On the Bias-Variance Tradeoff, P17](https://arxiv.org/abs/1912.08286) 

##4. Appendix-Expectation Rules
- $E(X+Y) = E(X) + E(Y)$
- Typically, $E(XY) \neq E(X)E(Y)$
  - If $X$, $Y$ are independent, $E(XY) = E(X)E(Y)$

Ref:[wiki: expectation](https://en.wikipedia.org/wiki/Expected_value)


