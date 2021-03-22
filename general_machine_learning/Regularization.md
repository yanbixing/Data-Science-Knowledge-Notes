# Regularization

## 1. L1 and L2

L1 and L2 actually consider model complexity as a function of weights.
Ref: [Machine Learning Crush Course](https://developers.google.com/machine-learning/crash-course/regularization-for-simplicity/l2-regularization), [TowardsDataScience](https://towardsdatascience.com/l1-and-l2-regularization-explained-874c3b03f668)

Larger weight tends to cause sharper shape of function. Ref: [webpage](https://machinelearningmastery.com/weight-regularization-to-reduce-overfitting-of-deep-learning-models/)




## 2. Bias-Variance Tradeoff

This and the following parts includes knowledge to explain why regularization works.

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
  =& [\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h_\mathcal{S})-f]^2 + \{  \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h^2_\mathcal{S})- [\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h_\mathcal{S})]^2  \} +\sigma^2\\ := & [\underset{\mathcal{S}\sim\mathcal{D}^m}{Bias}(h_\mathcal{S})]^2 +\underset{\mathcal{S}\sim\mathcal{D}^m}{Var}(h_\mathcal{S}) +\sigma^2
\end{aligned}$$ where:$$\underset{\mathcal{S}\sim\mathcal{D}^m}{Bias}(h_\mathcal{S}):=\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h_\mathcal{S})-f$$ and $$ \underset{\mathcal{S}\sim\mathcal{D}^m}{Var}(h_\mathcal{S}) = \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h^2_\mathcal{S})- [\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h_\mathcal{S})]^2$$ <br> 
**Note**: The expectation $\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}[\hat{R}_\mathcal{S}(h_\mathcal{S})]$ is different from **generalization loss**, generalization/empirical loss is in terms of one certain function. While here in the expectation $\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}[\hat{R}_\mathcal{S}(h_\mathcal{S})]$, $\mathcal{S}$ is a "variable", different $\mathcal{S}$ give different optimized function $h_\mathcal{S}$. <br> In sum, the decomposition require two necessary conditions: (1) The $h_\mathcal{S}$ must vary with $\mathcal{S}$, ($\mathcal{S}$ change, $\rightarrow h_\mathcal{S}$ change, i.e., prediction value on same point changes.). If not, $h_\mathcal{S}$ is independent with $\mathcal{S}$, the deduction process (expectation) doesn't stand. (2) The risk should be squared error. If not, the deduction process doesn't stand.<br> 
**Personal Understanding:** The **bias** and **variance** here actually is a metric to measure the performance of learning **algorithm** $\mathcal{A}$ and **hypothesis class** $\mathcal{H}$, since $\mathcal{H} \overset{\mathcal{A},  \mathcal{S}\in\mathcal{D}}{\rightarrow} h_\mathcal{S}$, i.e. $h_\mathcal{S}$ is found through $\mathcal{A}$).
Ref: [Wiki-Bias-variance tradeoff](https://en.wikipedia.org/wiki/Bias%E2%80%93variance_tradeoff) <br> 
**Conflict**: In [The elements in statistical learning, Sec 2.9, P37](TBD) the expectation of empirical error is generalization error (also called EPE, expected prediction error.) <br>
**Personal Clarification:** both generalization error and empirical error measure the error of a particular function. However, in the above deduction process, the function varies with the sample set. Thus, it is NOT **generalization error**.
<!-- **Reconcile**: In the above function, we didn't restrict the form/algorithm of empirical risk, whatever form (e.g. training loss, prediction loss) of empirical risk we use, the above relation stands. If we define the empirical loss as prediction loss, then, the **expectation on empirical risk (prediction loss) over different sample set** is the **generalization risk** -->

#### 2.1.2 Bias-variance tradeoff?

- Conclusion
- Model Complexity:
  - Rademacher complexity:
  $$\hat{R}$$

Temp Q:

what does math mark ";" like ";D" means? parameterized?

bias-variance tradeoff vs. Approximation-Estimation tradeoff

## 3. Why regularization can reduce model variance?

Model variance: $Var\sim\sigma^2\frac{n}{N}$ where n is number of parameters and N is number of training samples, $\sigma$ is the noise variance.
Ref: [Nonlinear System Identification, Sec 7.2, P181 ](TBD) 

Regularization makes model behave as though it possesses fewer parameters than it really is. Ref: [Nonlinear System Identification, Sec 7.5, P207 ](TBD)


## Appendix - Concept Differentiation

- Risk vs Empirical Risk:
  - Notation: 
    - $\mathcal{D}$: An **unknown joint distribution** that generates $(x,y)$ pairs.


    - $\mathcal{S}$: A finite **sample set** with $m$ i.i.d samples $(x_1,...,x_m)$ from $\mathcal{D}$.


    - $h$: A **hypothesis** that chosen from **hypothesis class** $\mathcal{H}$ <br> English: $\mathcal{H}$ is a finite set of functions, $h \in \mathcal{H}$, and $h_\mathcal{S}$ denotes a function that satisfies $\hat{R}_\mathcal{S}(h_\mathcal{S}) = 0$ given by **algorithm** $\mathcal{A}$<br> Ref:[Foundations of Machine Learning, Sec 2.2, P15](https://cs.nyu.edu/~mohri/mlbook/) <br> Consistent with definition of $h_\mathcal{S}$ which is given by $\mathcal{A}$ in [On the Bias-Variance Tradeoff, Sec 2.1, P5](https://arxiv.org/abs/1912.08286)


    - $\hat{R}_\mathcal{S}(h)$ denotes the **empirical risk** or **empirical error**. i.e. The calculated error of a particular function $h$ (not a set of functions) based on sample set $\mathcal{S} $. <br> $R(h)$ denotes the **generalization risk** or **generalization error**. i.e. The average loss of a particular function $h$ (not a set of functions) over the whole space. <br> Ref: [Foundations of Machine Learning, Sec 2.1, P10](https://cs.nyu.edu/~mohri/mlbook/), [Wiki-Generalization error](https://en.wikipedia.org/wiki/Generalization_error) <br> $\hat{R}(h)$ also denotes the empirical risk, see: [On the Bias-Variance Tradeoff, Sec 2.1, P5](https://arxiv.org/abs/1912.08286) and the older version of *Foundations of Machine Learning (2012)* <br> 
    **Personal understanding**: the exact way to calculate the risk is **NOT** defined/ stipulated/ restricted. You can use the training loss (like, train the model with the whole $\mathcal{S}$ and report the loss) you can also use the **prediction loss** (like, split the $\mathcal{S}$ to training and test, report the test loss). 
    <!-- If we use the **prediction loss**, we should have something like:
    $$R(h\in\mathcal{H}|\mathcal{A}) = \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}[\hat{R}_\mathcal{S}(h_\mathcal{S})]$$
    which reconcile the 'conflicted' description between the above **"EPE"** in [The elements in statistical learning, Sec 2.9, P37](TBD) and the relation below in [Foundations of Machine Learning, Sec 2.1, P11](https://cs.nyu.edu/~mohri/mlbook/). -->


    - $\hat{R}_\mathcal{S}(h)$ and $R(h)$ satisfy:
    $$  \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}[\hat{R}_\mathcal{S}(h)]=R(h) $$
    i.e. expectation of empirical error on an i.i.d $\mathcal{S}$ equals generalization error. (This stands for all forms of risk.) <br> Ref:  [Foundations of Machine Learning, Sec 2.1, P11](https://cs.nyu.edu/~mohri/mlbook/)


  - <br>Ref: [On the Bias-Variance Tradeoff, P17](https://arxiv.org/abs/1912.08286) 

- Estimation error and approximation error
  - Risk decomposition:$$ \begin{aligned}
    R(h) =& (R(h) - \underset{ h' \in \mathcal{H} }{\inf}R(h')) +(\underset{ h' \in \mathcal{H} }{\inf}R(h')- R^*) + R^* \\
    :=& EstimationRisk + ApproximationRisk + BayesRisk
  \end{aligned}
   $$ **personal understanding:** bayes risk is the $\varepsilon (\sigma^2)$ in the bias-variance decomposition.

  - Ref: [Foundations of Machine Learning, Sec 4.1, P61](https://cs.nyu.edu/~mohri/mlbook/)

## Appendix-Expectation Rules
- $E(X+Y) = E(X) + E(Y)$
- Typically, $E(XY) \neq E(X)E(Y)$
  - If $X$, $Y$ are independent, $E(XY) = E(X)E(Y)$

Ref:[wiki: expectation](https://en.wikipedia.org/wiki/Expected_value)


