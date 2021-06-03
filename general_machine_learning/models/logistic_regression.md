# Logistic Regression

## Basics

### 0.0 Notations

- $s=(\mathbf{x},y)$ the input of a sample
  - $\mathbf{x}$ (or $\vec{x}$): an $(N+1)$-dim vector with $N$ features $\mathbf{x}^{(i)} :=X^{(i)}$ and an extra dimension with constant value 1 (so the $\theta_{N+1}$ corresponds to the "intercept".)
    - $\mathbf{x}^{(i)} :=X^{(i)}$ denotes the i-th feature.
  - $y$ (or $y^*$):  label of the sample, binary variable: $y \in \{0,1\}$
- $\theta$ is an $(N+1)$-dim vector contains the "weights" and the "intercept" of the features.
- $\mathcal{S}$ denotes the training set with $M$ samples $\{...s_i...\}$
- $v$: an independent binary variable: $v \in \{0,1\}$ 
- $\log := \log_2$

### 0.1  Basics

- Prediction (score) function: $h_\theta(\mathbf{x}) = \frac{1}{1+\exp(\mathbf{\theta}^T\mathbf{x})}$
  - Probability interpretation: 
    - $p(\hat{y}=1|\mathbf{x}):= \hat{p}(1|\mathbf{x}) = h_\theta(\mathbf{x})$
    - $p(\hat{y}=0|\mathbf{x}):= \hat{p}(0|\mathbf{x})  = 1-h_\theta(\mathbf{x})$
    - The above two cases can be summarized as:<p>
    $$p(\hat{y}=v|\mathbf{x}) = v \cdot h_\theta(\mathbf{x}) + (1-v) \cdot (1-h_\theta(\mathbf{x}))$$
- Loss function: Average of sample's cross entropy on the true label distribution and predicted label distribution.
  - loss function: 
  $$J(\theta|\mathcal{S})= - \frac{1}{M}\sum^M_{i=1}[ y_i \log (h_\theta(\mathbf{x}_i)) + (1-y_i) \log (1-h_\theta(\mathbf{x}_i)) ]$$
  - Deduction:
    - The probability distribution of sample's true label:<p>
    $$P_i^*(v) :=p(y_i^* = v| s_i )=\left\{ \begin{array}{ll} 1 & v=y_i\\ 0 & v\not = y_i \end{array} \right.$$
    - The probability distribution of sample's predicted label:<p>
    $$\hat{P}_i(v) := p(\hat{y}_i=v| s_i )$$
    - sample's cross_entropy: <p>
    $$\begin{aligned}
    H_i(P_i^*(v), \hat{P}_i(v)) &= - \sum_{v\in{0,1}} P_i^*(v) \log(\hat{P}_i(v))\\
    & = -[ P_i^*(v=0) \log(\hat{P}_i(v=0)) + P_i^*(v=1) \log(\hat{P}_i(v=1))] \\
    & = 1 \cdot \log(\hat{P}_i(v=y_i)) \\
    & = \log[ p(\hat{y}_i=y^*_i| s_i ) ] \\
    & = \log[y^*_i \cdot h_\theta(\mathbf{x}_i) + (1-y^*_i) \cdot (1-h_\theta(\mathbf{x}_i))]
    \end{aligned} $$
    - Loss function take the average of the sample's cross entropy:<p>
    $$J(\theta|\mathcal{S})= \frac{1}{M} \sum^M_{i=1} H_i(P_i^*(v), \hat{P}_i(v)) $$
  - The cross entropy loss is strictly convex
    - No need random initialization
    - Ref: [Google Search](https://www.google.com/search?q=l2+regularized+logistic+regression+convex%3F&oq=l2+reg&aqs=chrome.1.69i57j35i39j0l6.5290j1j3&sourceid=chrome&ie=UTF-8), [MIT - Doc](http://people.csail.mit.edu/jrennie/writing/convexLR.pdf)

## Properties

- 	用cross-entropy 的 logistic regression是 strictly convex ，所以不需要random initialization
	Regularized LR is still strictly convex的


<!-- - $\{y^*_1,...,y^*_M\}$ and corresponding predicted score $\{ \hat{p}(y^*_1|\mathbf{x}_1),...\hat{p}(y^*_M|\mathbf{x}_M) \}$. -->
  <!-- - Define: 
    - $\hat{p}(x,y) = \left\{ \begin{array}{ll} h_\theta(x) & y=1\\ 1-h_\theta(x) & y=0 \end{array} \right.$
    - $\hat{p}(x,y) = \left\{ \begin{array}{ll} h_\theta(x) & y=1\\ 1-h_\theta(x) & y=0 \end{array} \right.$
    - $p(y=1|x,y)$
  - $H(p^*,\hat{p})$
  $$l(\mathbf{w},b|\mathcal{S})=$$
  - $\left\{ \begin{array}{ll} p(\hat{y}=1| s_i ) = h_\theta(x) \\ p(\hat{y}=0| s_i )  = 1-h_\theta(x) \end{array} \right.$
  - $p(y^*=1| s_i )  = \left\{ \begin{array}{ll} 1 & y=1\\ 0 & y=0 \end{array} \right.$
  - $p(y^*=0| s_i )  = \left\{ \begin{array}{ll} 0 & y=1\\ 1 & y=0 \end{array} \right.$ -->

## Deep Dive: Cross Entropy, Log-Likelihood and LR

### Differentiation: Entropy, Cross Entropy, DL-Divergence and Log-Likelihood

- Entropy evaluate one distribution:
  $$H(p) = - \sum_{x\in\mathcal{X}} p(x)\log p(x)$$
  - [ [Ref: tree_model.md](./tree_model.md) ]

- Cross Entropy evaluate two distributions on a same dataset: 
  $$H(p,q)=-\sum _{x\in {\mathcal {X}}}p(x)\,\log q(x)$$ 
  - [ [Ref: Wiki-Cross_Entropy](https://en.wikipedia.org/wiki/Cross_entropy) ]

- KL Divergence:
  $$D_{\mathrm{KL} }(p\|q) = H(p,q) - H(p)$$
  - KL Divergence is the difference value between cross entropy and entropy, that is why it is also called "relative entropy." 
  - [ [Ref: Wiki-KL_Divergence](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence#Cross_entropy) ]

- Negative log likelihood:
  $$NLL(p(\mathcal{S})) = -\log(p(\mathcal{S})) $$
  - Likelihood is discussed in the perspective of likelihood of (appearance of) dataset $\mathcal{S}$
  - Ref: [Wiki-MLE](https://en.wikipedia.org/wiki/Maximum_likelihood_estimation)

### Cross Entropy and NNL in logistic regression.

Typically, cross entropy and NNL are not comparable as they conceptually different. (CE consider two distribution; NLL consider the probability of a dataset.)

But in definition of Logistic regression, the "NNL loss" is equivalent to the "cross entropy loss".

- Definition:
  - "NNL loss": NNL of the whole dataset
  - "cross entropy loss": average of sample's cross entropy between true label distribution and predicted label distribution

**Proof:**

- Likelihood of the dataset given parameter $\theta$ is:
  $$p(S|\theta) = \prod^M_{i=1} p(\hat{y} = y_i|\mathbf{x}_i)$$
- Then:
  $$\begin{aligned}
      NLL(S|\theta) & = - \sum^M_{i=1} \log[ p(\hat{y} = y_i|\mathbf{x}_i) ]\\
      & = - \sum^M_{i=1} \log[ p(\hat{y} = y_i|s_i) ] \\
      & \underset{LR}{=} - \sum^M_{i=1} H_i(P_i^*(v), \hat{P}_i(v))\\
      & = M\cdot J(\theta)
  \end{aligned}$$
  - Minimize NLL = MLE
  - Thus, also, **in LR**, the training strategy "minimize the average cross-entropy loss" can also be viewed as MLE

Backup refs: [StackExchange](https://stats.stackexchange.com/questions/198038/cross-entropy-or-log-likelihood-in-output-layer), [Quora](https://www.quora.com/What-are-the-differences-between-maximum-likelihood-and-cross-entropy-as-a-loss-function)


## FAQ:

## Backup Refs
[Zhihu](https://zhuanlan.zhihu.com/p/34670728), [Github page](https://ml-cheatsheet.readthedocs.io/en/latest/logistic_regression.html) 