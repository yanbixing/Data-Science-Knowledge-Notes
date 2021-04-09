# Regularization

## 1. L1 and L2 Regularization

### 1.1. Form

Loss function:
$$ L = l(y,h) + \Re(h)$$


L1 $\Re$ (Lasso): $\Re(h(x;w)) =  \lambda\sum_i|w_i|$
L2 $\Re$ (Ridge): $\Re(h(x;w))=  \lambda\sum_i w^2_i$ 

### 1.2 property

#### 1.2.1 Similarity

- Imposing a penalty on weight, shrink its scale, equals:
  - Implicit dimension reduction 
  - Feature selection
  - Reduce effective number of parameter.
  - Reduce the model complexity.

- Pros:
  - Lower model variance = prevent overfitting
  - Easier to interpret model
  - Better scalability

#### 1.2.2 Difference

- Generally
  - L1 $\Re$ has more zero weight. 
    - = eliminate the features = feature selection / dimension reduction
  - L2 $\Re$ tends to make weight small but non-zero.
- For identical features ($x_i = x_j$)
  - L1 $\Re$ spreads weight arbitrarily. 
    - e.g.($|0.9|+|0.1| = |0.5|+|0.5|$) 
  - L2 $\Re$ spreads weight evenly. 
    - e.g.($0.5^2+0.5^2 < 0.9^2 + 0.1^2$)
- For linearly related features. ($x_i = c x_j$, e.g. c=3 )
  - L1 $\Re$ assign all weight to the feature with largest scale, all others' weights are zero. ( feature scale$\uparrow$, $|w| \downarrow$, $\Re \downarrow$ )
    - e.g. $w_ix_{m(i)}+w_jx_{m(j)} = y_m \sim\rightarrow 3w_i+w_j = 1 $
    $min(|w_i|+|w_j|) \sim min(1-2w_i)\rightarrow\frac{1}{3}$ when $(w_i,w_j) = (\frac{1}{3},0)$
  - L2 $\Re$ prefers variable with larger scale - spread weights **proportional** to scale.
  i.e. larger scale get larger weights, smaller scale feature get smaller (proportional) weights but not zero.
    - e.g.$cw_i+w_j = 1, c>1$, 
    $min(w_i^2 + w_j^2) = min(w_i^2+(1-cw_i)^2)\rightarrow$ $w_i = \frac{c}{c^2+1}, w_j = \frac{1}{c^2+1}$

  - For correlated feature (TBD)

#### 1.2.3 Application (when to choose what?)

- When features are **correlated**:
**L2** $\Re$ is better. 
Explanation: In the extreme case “linearly related”: 
  - **L2** will distribute weights among different features 
  $\sim\rightarrow$ the result is an **weighted average/vote of all correlated positions**
  $\sim\rightarrow$ even some position are missing/inaccurate, the result is stable.
  $\sim\rightarrow$ less model variance?
  - L1 will assign weight to the largest scale feature.
  $\sim\rightarrow$ the result is solely dependent on that feature 
  $\sim\rightarrow$ the result is not stable.
  $\sim\rightarrow$ higher model variance?

- When features is not correlated or data is **noisy**:
**L1** $\Re$ may be better. 
Explanation: **L1** can **remove useless/noise feature**. But L2 will still keep them.
**Note**: usually weight can be also viewed as feature importance. Reduce weight = reduce reliance on the feature = reduce feature importance.


### 1.3 Understanding

#### 1.3.1 Gradient perspective:

$$(w_i)_{t+1} = (w_i)_t-\alpha\partial_{w_i}L(w)|_{w=w_t}$$
Assume w>0 for simplicity:

- L1 $\Re$ contribution: $-\partial_{w_i}\sum_j |w_j| = -1$
- L2 $\Re$ contribution: $-\partial_{w_i}\sum_j w_j^2 = -w_i$
This means:
- L1 same punishment (shrinkage speed) for all weights as long as it's not zero. So, there are more zero weight.
- L2 larger weight shrink faster, smaller weight shrink slower. So more weights may be small but not zero.

#### 1.3.2 Graph illustration:

<div  align="center"><img src="https://github.com/ShuaiW/data-science-question-answer/blob/master/assets/l1l2.png?raw=true" style = "zoom:50%"></div>

**Figure:** Different axis represent different weight terms. The objective function is quadratic, thus its contours are ellipsoids. The black solid square / circle are the contours of L1 / L2 regularization. The intersection of the contours is the solution of minimum total loss. 

- For L1, the minimum-loss-solution is more likely to appear on the axis, i.e. more zero weight.
- For L2, the minimum-loss-solution is more likely to appear at non-axis area, i.e. small but non-zero.

Ref: Similar figure can be found at [Foundations of Machine Learning, Sec 11.3.4, P286](https://cs.nyu.edu/~mohri/mlbook/)

## 2. Deep dive: How regularization works?

This and the following parts includes knowledge to explain how regularization works.

### 2.1 Bias-variance decomposition: what is "bias" and "variance"

- 'Noises' always exist in real word data. i.e. even for same feature values $x$, we usually observe different label value $y$. Thus, the function between label and feature can be represented as:
$$\begin{aligned}
y  =& f(x)+\varepsilon \\ : =& f+\varepsilon
\end{aligned}$$
where: 
  - $\varepsilon$ has $E(\varepsilon) = \bar{\varepsilon} = 0$ and $Var(\varepsilon) = E(\varepsilon - E(\varepsilon)) = \bar{\varepsilon^2} - \bar{\varepsilon}^2 = \sigma^2$.
  - f(x) stands for the "real" function between $x$ and $y$.

- With machine learning algorithm $\mathcal{A}$, we can find $h_\mathcal{S}$ through minimizing **empirical risk** (square error here):
$$\begin{aligned}
  \hat{R}_\mathcal{S}(h_\mathcal{S}) : =& (y-h_\mathcal{S})^2 \\
   =& (f - h_\mathcal{S})^2 + 2\varepsilon(f-h_\mathcal{S})+\varepsilon^2
\end{aligned}$$
  - Risk is another name of "loss"
- Then, **the expectation of empirical risk over different sample set $\mathcal{S}$** equals:
$$\begin{aligned}
  \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}[\hat{R}_\mathcal{S}(h_\mathcal{S})] = & \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}[(f - h_\mathcal{S})^2 + 2\varepsilon(f-h_\mathcal{S})+\varepsilon^2] \\
  = & \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}[(f - h_\mathcal{S})^2] + 
  \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}[2\varepsilon(f-h_\mathcal{S})]+
  \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}[\varepsilon^2] \\ 
  = & \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(f^2-2fh_\mathcal{S}+h^2_\mathcal{S}) + 2\bar{\varepsilon}\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(f-h_\mathcal{S})+\bar{\varepsilon^2} \\ 
  = & [f^2-2f\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h_\mathcal{S})+\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h^2_\mathcal{S})] + 0 + \sigma^2 \\
  = & \{ f^2-2f\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h_\mathcal{S}) + [\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h_\mathcal{S})]^2 \} +\\ 
  & + \{  \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h^2_\mathcal{S})- [\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h_\mathcal{S})]^2  \} +\sigma^2\\
  = & [\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h_\mathcal{S})-f]^2 + \{  \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h^2_\mathcal{S})- [\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h_\mathcal{S})]^2  \} +\sigma^2\\ := & [\underset{\mathcal{S}\sim\mathcal{D}^m}{Bias}(h_\mathcal{S})]^2 +\underset{\mathcal{S}\sim\mathcal{D}^m}{Var}(h_\mathcal{S}) +IrreducibleError
\end{aligned}$$ where:$$\underset{\mathcal{S}\sim\mathcal{D}^m}{Bias}(h_\mathcal{S}):=\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h_\mathcal{S})-f$$ and $$ \underset{\mathcal{S}\sim\mathcal{D}^m}{Var}(h_\mathcal{S}) = \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h^2_\mathcal{S})- [\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}(h_\mathcal{S})]^2$$ <br> 
**Note**: The expectation $\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}[\hat{R}_\mathcal{S}(h_\mathcal{S})]$ is different from **generalization loss**, generalization/empirical loss is in terms of one certain function. While here in the expectation $\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}[\hat{R}_\mathcal{S}(h_\mathcal{S})]$, $\mathcal{S}$ is a "variable", different $\mathcal{S}$ give different optimized function $h_\mathcal{S}$. <br> In sum, the decomposition require two necessary conditions: (1) The $h_\mathcal{S}$ must vary with $\mathcal{S}$, ($\mathcal{S}$ change, $\rightarrow h_\mathcal{S}$ change, i.e., prediction value on same point changes.). If not, $h_\mathcal{S}$ is independent with $\mathcal{S}$, the deduction process (expectation) doesn't stand. (2) The risk should be squared error. If not, the deduction process doesn't stand. <br> 
**Personal Understanding:** The **bias** and **variance** here actually is a metric to measure the performance of learning **algorithm** $\mathcal{A}$ and **hypothesis class** $\mathcal{H}$, since $\mathcal{H} \overset{\mathcal{A},  \mathcal{S}\in\mathcal{D}}{\rightarrow} h_\mathcal{S}$, i.e. $h_\mathcal{S}$ is found through $\mathcal{A}$).
Ref: [Wiki-Bias-variance tradeoff](https://en.wikipedia.org/wiki/Bias%E2%80%93variance_tradeoff), [The elements in statistical learning, Sec 7.3, e.q.7.9, P223](TBD) <br> 
**Conflict**: In [The elements in statistical learning, Sec 2.9, P37](TBD) the expectation of empirical error is generalization error (also called EPE, expected prediction error.) <br>
**Personal Clarification:** both generalization error and empirical error measure the error of a particular function. However, in the above deduction process, the function varies with the sample set. Thus, it is NOT **generalization error**.
<!-- **Reconcile**: In the above function, we didn't restrict the form/algorithm of empirical risk, whatever form (e.g. training loss, prediction loss) of empirical risk we use, the above relation stands. If we define the empirical loss as prediction loss, then, the **expectation on empirical risk (prediction loss) over different sample set** is the **generalization risk** -->
### 2.2 Model complexity and generalization risk

#### 2.2.1 Model complexity metrics

- Empirical Rademacher complexity:
  
  $$Rad_\mathcal{S}(\mathcal{G}) 
  = \underset{\mathbf{ \sigma }}{\mathbb{E}}[\underset{g\in\mathcal{G}}{\sup(\frac{1}{m}\sum^m_{i=1} \sigma_i g(z_i))}] 
  = \underset{\mathbf{ \sigma }}{\mathbb{E}}[\underset{g\in\mathcal{G}}{\sup(\frac{\mathbf{\sigma}\cdot\mathbf{g}_\mathcal{S}}{m})}]$$
  where:
  - $\mathcal{G}$ is a family of functions mapping from $\mathcal{Z}$ to range $[a,b]$
  - $\mathcal{S}$ is a dataset with $m$ samples $\mathcal{S} = (z_1,...,z_m)$ where $z_i \in \mathcal{Z}$.
  - $\mathcal{\sigma} = (\sigma_1,...,\sigma_m)$ is sampled from uniform i.i.d of binary values {-1, +1}
  - Rademacher complexity:
    $$ Rad(\mathcal{G}) = \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}} [Rad_\mathcal{S}(\mathcal{G})]$$
  - For many models, the Rademacher complexity is infinite. (Thus, hard to compare different models.) Ref: [On the Bias-Variance Tradeoff, Sec 2.3, P7](https://arxiv.org/abs/1912.08286)

- VC-dimension:
  $$ VC\dim(\mathcal{H}) = \max\{ m : \Pi_\mathcal{H}(m) = 2^m \} $$
  - Shattering: A set $S$ with $m$ points can be shattered by a hypothesis set $\mathcal{H}$ when $\mathcal{H}$ can realize all dichotomies of $S$
  Ref: [Foundations of Machine Learning, Sec 3.3, P36](https://cs.nyu.edu/~mohri/mlbook/)
  - Growth function $\Pi_\mathcal{H} (m): \mathbb{N} \rightarrow \mathbb{N}$ is defined as:
  $$\forall m \in \mathbb{N}, \Pi_\mathcal{H} (m)= \underset{x_i \in X}{\max}|\{(h(x_1),...,h(x_m)):h\in\mathcal{H}\}|$$
  i.e. $\Pi_\mathcal{H} (m)$ is the maximum number of distinct ways in which m points can be classified with hypothesis $\mathcal{H}$. Each one of these distinct classification is call a **dichotomy**.
  Ref: [Foundations of Machine Learning, Sec 3.2, P34](https://cs.nyu.edu/~mohri/mlbook/)
  - Good examples: see [Wiki](https://en.wikipedia.org/wiki/Vapnik%E2%80%93Chervonenkis_dimension).
  - Purely combinatorial.
  - VC dimension seems only defined on binary classification cases? [Wiki](https://en.wikipedia.org/wiki/Vapnik%E2%80%93Chervonenkis_dimension).
  - For many models, VC dimension is roughly proportional to the number of parameters $p$. (**Note:** $m$ is number of samples not number of parameters. Don't confuse it with number of parameters.)
  Ref: [On the Bias-Variance Tradeoff, Sec 2.3, P7](https://arxiv.org/abs/1912.08286)
  

#### 2.2.2 Generalization risk and model complexity

$$ R(h) \leq \hat{R}_\mathcal{S}(h) + PosCorrF(\frac{VC\dim(\mathcal{H})}{m})$$

- **personal notation:** $PosCorrF$ here denotes some function is roughly positively correlated with model complexity and negatively correlated with number of samples ($m$) in the sample set $\mathcal{S}$.

- **personal understanding:** The generalization (true) risk is capped by the empirical risk (risk observed with $\mathcal{S}$) plus a value positively correlated with model complexity and negatively correlated with number of samples. This makes sense as it is consistent with our intuition:
  - Complexer model may result in higher generalization risk.
  - More samples make empirical risk approximate generalization risk.

#### 2.2.3 Bias-variance tradeoff ?

The **trade-off** relation is **never** mathematically proved. [On the Bias-Variance Tradeoff, Ch.4, P15](https://arxiv.org/abs/1912.08286) The concept is just an intuitive understanding:

- From the above, we can conclude: 
  - Expectation of empirical risk with $\mathcal{A}, \mathcal{H}$ can be decomposed into bias- and variance- related risk.
    - **Personal Intuition:** $\sim \rightarrow$ with the same expectation of empirical risk, a higher model variance lead to lower bias
  - The generalization risk capping (top limit) of a specific $h$ rough increase with model complexity. 
    - **Personal Intuition:**  $\sim\rightarrow$ with the same empirical risk, the higher number of param, the higher model complexity, the higher generalization risk $\Rightarrow$ Overfitting.
  - Thus, if we approximately regard $\underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}[\hat{R}_\mathcal{S}(h_\mathcal{S})]$ to $\hat{R}_\mathcal{S}(h)$ as same thing. We can intuitive understand the well known bias-variance "tradeoff": 
    - Higher complexity $\overset{same \hat{R}_\mathcal{S}(h)}{\Rightarrow}$ higher generalization risk <br> $\sim \rightarrow$ model-overfitting <br> $\sim \rightarrow$ higher variance $\overset{same \underset{\mathcal{S}\sim\mathcal{D}^m}{\mathbb{E}}[\hat{R}_\mathcal{S}(h_\mathcal{S})]}{\Rightarrow}$ lower bias 
    - I.e. complexer model will cause overfitting (intuitive not strict) which will result in higher model variance and lower bias (intuitive not strict)
    - I.e. "the bias-variance can be tuned through model complexity".
  - Actually, the bias and variance can decrease at the same time. [On the Bias-Variance Tradeoff, Ch.4, P15](https://arxiv.org/abs/1912.08286)
- Appendix - "Model variance" (Intuitive) 
If we approximate the model variance with in-sample error (may be training error? not generalization error), we can regard that the model variance is proportional to number of parameters:
$$Var_{in-sample} \sim \sigma^2\frac{p}{m}$$ 
where $p$ is number of parameters and $m$ is number of training samples, $\sigma^2$ is the **Irreducible Error**, i.e. the variance of noise $\varepsilon$ in the bias-variance decomposition section.
**Note**: the in-sample error is not test error, so there is sti, pls check the ref *The element in statistical learning*.
Ref: [Nonlinear System Identification, Sec 7.2, P181](TBD), [The elements in statistical learning, Sec 7.3, P224, eq.7.12](TBD)
  

### 2.3 Regularization and bias-variance "tradeoff"

Since the trade-off is just an intuitive understanding, the regularization of the bias-variance "tradeoff" is also just an **intuitive** understanding. 



- Regularization makes model behave as though it possesses fewer parameters than it really is. Ref: [Nonlinear System Identification, Sec 7.5, P207](TBD)

- Intuitive understanding:
  - Since: larger weight tends to cause sharper shape of function. <br> Ref: [machinelearningmastery](https://machinelearningmastery.com/weight-regularization-to-reduce-overfitting-of-deep-learning-models/)
  - Thus: model complexity can be expressed with a function of weights. e.g. in the form of L1 and L2 regularization. <br> Ref: [Machine Learning Crush Course](https://developers.google.com/machine-learning/crash-course/regularization-for-simplicity/l2-regularization), [TowardsDataScience](https://towardsdatascience.com/l1-and-l2-regularization-explained-874c3b03f668)



## Appendix - Concept Differentiation

- Risk vs Empirical Risk:
  - Notation: 
    - $\mathcal{D}$: An **unknown joint distribution** that generates $(x,y)$ pairs.
      - $\mathcal{D}^m$: a sample set with m i.i.d samples ${d_1,...,d_m}$ where $d_i \in \mathcal{d}$


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
    - $\Re$ (latex: \Re) for regularization.


  - <br> Ref: [On the Bias-Variance Tradeoff, P17](https://arxiv.org/abs/1912.08286) 

- Estimation error and approximation error
  - Risk decomposition:$$ \begin{aligned}
    R(h) =& (R(h) - \underset{ h' \in \mathcal{H} }{\inf}R(h')) +(\underset{ h' \in \mathcal{H} }{\inf}R(h')- R^*) + R^* \\
    :=& EstimationRisk + ApproximationRisk + BayesRisk
  \end{aligned}
   $$ **personal understanding:** bayes risk is the $\varepsilon (\sigma^2)$ in the bias-variance decomposition.
  - **persoonal understanding:** The estimation-approximation decomposition targets a specific hypothesis $h$, while the bias-variance tradeoff targets an algorithm $\mathcal{A}$ and a hypothesis set $\mathcal{H}$
  - Ref: [Foundations of Machine Learning, Sec 4.1, P61](https://cs.nyu.edu/~mohri/mlbook/)

## Appendix-Expectation Rules

- $E(X+Y) = E(X) + E(Y)$
- Typically, $E(XY) \neq E(X)E(Y)$
  - If $X$, $Y$ are independent, $E(XY) = E(X)E(Y)$
- Ref:[wiki: expectation](https://en.wikipedia.org/wiki/Expected_value)
## TBD questions:

- what does math mark ";" like ";D" means? parameterized? Ref: [Wiki-Bias-variance tradeoff](https://en.wikipedia.org/wiki/Bias%E2%80%93variance_tradeoff)
