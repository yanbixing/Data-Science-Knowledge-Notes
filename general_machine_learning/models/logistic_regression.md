# Logistic Regression

## 0. Notations

- $s=(\mathbf{x},y)$ the input of a sample
  - $\mathbf{x}$ (or $\vec{x}$): an $(N+1)$-dim vector with $N$ features $\mathbf{x}^{(i)} :=X^{(i)}$ and an extra dimension with constant value 1 (so the $\theta_{N+1}$ corresponds to the "intercept".)
    - $\mathbf{x}^{(i)} :=X^{(i)}$ denotes the i-th feature.
  - $y$ (or $y^*$):  label of the sample, binary variable: $y \in \{0,1\}$
- $\theta$ is an $(N+1)$-dim vector contains the "weights" and the "intercept" of the features.
- $\mathcal{S}$ denotes the training set with $M$ samples $\{...s_i...\}$
- $v$: an independent binary variable: $v \in \{0,1\}$ 
- $\log := \log_2$

## 1.  Basics

### 1.1. Prediction (score) function: 
$$h_\theta(\mathbf{x}) = \frac{1}{1+\exp(\mathbf{\theta}^T\mathbf{x})}$$
- Probability interpretation: 
  - $p(\hat{y}=1|\mathbf{x}):= \hat{p}(1|\mathbf{x}) = h_\theta(\mathbf{x})$
  - $p(\hat{y}=0|\mathbf{x}):= \hat{p}(0|\mathbf{x})  = 1-h_\theta(\mathbf{x})$
  - The above two cases can be summarized as:<p>
  $$p(\hat{y}=v|\mathbf{x}) = v \cdot h_\theta(\mathbf{x}) + (1-v) \cdot (1-h_\theta(\mathbf{x}))$$

### 1.2. Training

Minimize loss function with gradient descent.

### 1.2.1. Loss function

Average of sample's cross entropy on the true label distribution and predicted label distribution:
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

- **Note:** The <mark style="background-color:yellow;">cross entropy loss</mark> (w/ or w/o regularization) is <mark style="background-color:yellow;">**strictly convex**</mark>.
  - No need random initialization
  - Could always go to the minimum with gradient descent.
  - Ref: [Google Search](https://www.google.com/search?q=l2+regularized+logistic+regression+convex%3F&oq=l2+reg&aqs=chrome.1.69i57j35i39j0l6.5290j1j3&sourceid=chrome&ie=UTF-8), [MIT - Doc](http://people.csail.mit.edu/jrennie/writing/convexLR.pdf)


### 1.2.2. Gradient Descent

Denote $J(\theta):=J(\theta|\mathcal{S})$

Object:
$$\text{minimize } J(\theta)$$
Algorithm - Gradient Descent, i.e.:

$$\mathbf{\theta}^{(i)}_{t+1} = \mathbf{\theta}^{(i)}_{t} - \alpha \frac{\partial J(\theta_t)}{\partial \mathbf{\theta}_t^{(i)}} $$

where $\alpha$ is the step length:

$$\frac{\partial J(\theta)}{\partial \mathbf{\theta}^{(i)}} = \frac{1}{M}\sum^M_{m=1} [h_\theta(\mathbf{x}_m) - y^*_m]\cdot\mathbf{x}^{(i)}_m$$

Note: $\theta^{(N+1)} := b$, $\mathbf{x}^{(N+1)}_m := 1$

Note: Follow the direction of the gradient, the function value changes has a same sign with the gradient.

Ref: [StackExchange](https://math.stackexchange.com/questions/477207/derivative-of-cost-function-for-logistic-regression)


<!-- ## Properties

- 	用cross-entropy 的 logistic regression是 strictly convex ，所以不需要random initialization
	Regularized LR is still strictly convex的 -->


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
  - [ [Ref: ./tree_models.md](./tree_models.md) ]

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

But <mark style="background-color:yellow;">in the case of Logistic regression</mark>, the <mark style="background-color:yellow;">"NNL loss"</mark> is <mark style="background-color:yellow;">equivalent</mark> to the <mark style="background-color:yellow;">"cross entropy loss"</mark>.

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

### Un-Summarized Qs:

- Why not use MSE loss?
  - MSE loss is **not convex** 
  <div  align="center"><img src=./logistic_regression_asset/mse_non-convex.png style = "zoom:10%"></div>

  - A lot of local minimum (due to the non-linearity of loss on weight.)
  - Ref: [Coursera_ML](https://www.coursera.org/learn/machine-learning/lecture/1XG8G/cost-functio)
- Feature importance of LR? 
  - The weight $\theta$ corresponds to the feature importance.
    - Explain: the larger weight, the results are more sensitive to the corresponding features. The model is more complex ,i.e., has larger variance.
    - Further understanding: Also, that is the reason we use weight regularization to tune model complexity.
- Feature scaling will on LR?
  - Yes, for regularized LR. 
    - Because regularization term punish large weight. 
    - So, when trying to maintain a same feature-weight product
      - feature scale $\uparrow$, weight scale $\downarrow$, punishment $\downarrow$
    - Namely, larger scale features are less regularized by regularization term, thus more prone to overfit.
  - No, for un-regularized LR
    - Because the prediction (and loss) function is dependent on the product of weight and feature.
    - When feature has a larger scale, 
    - A same feature-weight product can be maintained by weight in smaller scale
      - e.g. 100 = 10 * 10 = 1 * 100
    - Thus, changing the scale will not affect the final results for un-regularized LR.
  - Ref: [Datacamp](https://www.datacamp.com/community/tutorials/preprocessing-in-data-science-part-2-centering-scaling-and-logistic-regression), [StackExchange (more comprehensive)](https://stats.stackexchange.com/questions/244507/what-algorithms-need-feature-scaling-beside-from-svm)

## Backup Refs
[Zhihu](https://zhuanlan.zhihu.com/p/34670728), [Github page](https://ml-cheatsheet.readthedocs.io/en/latest/logistic_regression.html) 