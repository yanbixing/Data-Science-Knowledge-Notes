# Naive Bayes

## 0. Notation in this page

- $\mathcal{S} = ( s^{(1)},...s^{(m)} )$ denotes the sample set with $m$ samples.
  - $s^{(i)}$ is usually an input-output pair $(x^{(i)},y^{(i)})$ in machine learning. 
  - input $x^{(i)}$ is usually an $n$-dim vector $(X^{(i)}_1, X^{(i)}_n)$
- $\theta$ denotes a parameter setting, $\theta \in \Theta$, $\Theta$ denotes the parameter space.
- $C_k$ denote the k-th class, usually the value of output variable $y$, there are $K$ classes in total.

## 1. Basics

### 1.1. Prediction function:

$$h(x) := \underset{k\in\{1,...,K\}}{\argmax}\left[p(C_k|X_1,...X_n)\right] = \underset{k\in\{1,...,K\}}{\argmax}\left[p(C_k)\prod^n_{i=1}p(X_i|C_k)\right]$$

where $C_k$ is the k-th class label, input $X = (X_1,...X_n)$ is an $n-\dim$ vector as and $X_i$ is the i-th feature.

This means, in the **training** process, we aim to get:
- $p(C_k)$ for all $k$ values. 
- $p(X_i|C_k)$ for all $(i,k)$ pairs.

### 1.2. Deduction

**"Naive"** assume **features are independent with each other**, i.e.:
$$p(X_i|X_1,...,X_{i-1},X_{i+1},...,X_n,C_k) = p(X_i|C_k)$$
Then:
$$\begin{aligned}
    & p(X_1,...X_n,C_k)\\
    = & p(X_1|x_2,....,X_n,C_k)p(X_2,...X_n,C_k) \\
    = & p(X_1|C_k)p(X_2,...X_n,C_k) \leftarrow \text{"Naive" assumption} \\
    & ...
    \\
    = & p(X_1|C_k)p(X_2|C_k)p(X_3,...X_n,C_k) \leftarrow \text{same above approach}\\
    & ...
    \\
     = & \prod^{n-1}_{i=1} p(X_i|C_k) \cdot p(X_n,C_k) = \prod^{n-1}_{i=1} p(X_i|C_k) \cdot p(X_n|C_k)p(C_k)
    \\
    = & p(C_k)\prod^n_{i=1} p(X_i|C_k)
\end{aligned}$$
Then:
$$ \begin{aligned}
    &\underset{k\in\{1,...,K\}}{\argmax}\left[p(C_k|X_1,...X_n)\right] \\ = & \underset{k\in\{1,...,K\}}{\argmax}\left[\frac{p(X_1,...X_n,C_k)}{p(X_1,...X_n)}\right] \leftarrow \text{Bayes rule} \\
    = & \underset{k\in\{1,...,K\}}{\argmax}\left[ p(X_1,...X_n,C_k) \right] \leftarrow p(X_1,...X_n) \text{ is independent on k} \\
    = & \underset{k\in\{1,...,K\}}{\argmax}\left[ p(C_k)\prod^n_{i=1} p(X_i|C_k) \right]
\end{aligned} $$


## 2. Application models

### 2.1 Why we need feature distribution assumption in real world application

In the **training** process, we aim to get:
- $p(C_k)$ for all $k$ values. 
  - Able to get with training sample, and typically, in the prediction, we don't expect to get predicted classes out of $k\in\{1,...,k\}$
- $p(X_i|C_k)$ for all $(i,k)$ pairs.
  - Assume $X_i \in [a,b]$, and in training samples $\mathcal{S}$, $X_i \in \{v_1,...,v_\mathcal{m}\}$
  - With training sample, we can get: 
  $$p(X_i = v_j|C_k ) = \frac{N(X_i = v_j|C_k )}{\sum^\mathcal{S}_{s = 1} N(X_i = v_s|C_k ) } $$
  - However, the problem is that for other values not appear in training set, i.e., $v_u \in [a,b]$ but $\notin\{v_1,...,v_m\}$, we cannot get $p(X_i = v_u|C_k )$ and thus cannot make prediction, this is un-applicable for real world use.
  - Thus,we need to assume a form (distribution) of $p(X_i |C_k )$ to make it have values for all $X_i \in [a,b]$

### 2.1 Gaussian Naive Bayes

Assume posterior follow Gaussian distribution:
$$p(X_i = v | C_k = k) = \frac{1}{\sqrt{2\pi\sigma^2_k}}e^{-\frac{(v-\mu_k)^2}{2\sigma^2_k}} $$
The parameters $\theta = (\mu,\sigma)$ is trained with MLE. Denote training set with $\mathcal{S} = \{(x^{(1)}, y^{(1)}),...(x^{(m)}, y^{(m)})\}$, we have:
$$\begin{aligned}
    (\mu_k, \sigma_k) & = \underset{\mu,\sigma}{\argmax}[p(\mathcal{S})]
    \\
    & = \underset{\mu,\sigma}{\argmax}[\log(p(\mathcal{\mathcal{S}})) ]
    \\
    & = \underset{\mu,\sigma}{\argmax}[\log(\prod^m_{s=1} p(x_s,y_s)) ]
    \\
    & = \underset{\mu,\sigma}{\argmax} \sum^m_{s=1} \log[p(x^{(s)},y^{(s)})] \leftarrow p(x,y) = p(x|y)p(y)
    \\& =  \underset{\mu,\sigma}{\argmax} \sum^m_{s=1} \{ \log[p(x^{(s)}|y^{(s)})] + \log[p(y^{(s)})]) \} \leftarrow \log[p(y^{(s)})] = const.
    \\& =  \underset{\mu,\sigma}{\argmax} \sum^m_{s=1} \{ \log[p(x^{(s)}|y^{(s)})] \leftarrow \underset{\theta}{\max} f(\theta) \Leftrightarrow \partial_\theta f(\theta) = 0
    \\
    \Rightarrow \mu_k &= AVG( \{x^{(s)}\}), \sigma_k = \sigma( \{x^{(s)}\} )
    \end{aligned}$$
Ref: [Toronto U Slides](https://www.cs.toronto.edu/~urtasun/courses/CSC411/tutorial4.pdf)


## 3. Deep Dive: 
### 3.1. What if the features are correlated?

The feature will be over-emphasized both in training and prediction process.

Ref: [Stackexchange](https://datascience.stackexchange.com/questions/9087/correlation-and-naive-bayes#:~:text=It%20is%20called%20Naive%20because,practice%20is%20rarely%20the%20case.&text=Correlation%20between%20features%20in%20Naive,will%20often%20say%20the%20same.)

Let's use a simple case as an example: 

- We have two different features $X_1, X_2$, this mode is denote as $p_0$. 
- Introduce a third feature $X_3$ identical to $X_1$ i.e. $X_3 = X_1$, this model is denoted as $p'$.

#### 3.1.1 In training process
**Conclusion:** The more $X_1$ is added, the more the model make decision only on $X_1$.

- Deduction:
$$\begin{aligned}
  \theta = & \underset{\theta}{\argmax}[p(\mathcal{S})] \leftarrow \text{regardless of form of $p$ or $\theta$}
  \\
  =& \underset{\theta}{\argmax} \sum^m_{s=1} \{ \log[p(x^{(s)}|y^{(s)})] \leftarrow \text{feature independent}
  \\
  =& \underset{\theta}{\argmax} \sum^m_{s=1} \sum^n_{i=1} \{ \log[p(X^{(s)}_i|y^{(s)})]
\end{aligned}$$
  - Denote $\partial_\theta \{ \sum^m_{s=1}\log[p(X^{(s)}_i|y^{(s)})]\}$ as $\Delta_i(\theta)$: the partial of i-th feature's log-posterior
    - For $p_0$: $\Delta_1(\theta)+\Delta_2(\theta) = 0$
    - For $p'$: $2\Delta_1(\theta)+\Delta_2(\theta) = 0$
    - ...
    - In extreme case, add $N\rightarrow \infty$ features identical to $X_1$: 
    $N\Delta_1(\theta)+\Delta_2(\theta) = 0$
    $\sim \Rightarrow N\Delta_1(\theta) \rightarrow 0\sim \Rightarrow \Delta_1(\theta) = 0$ 
    namely: $\partial_\theta \{ \sum^m_{s=1}\log[p(X^{(s)}_1|y^{(s)})] \} = 0$
    $\sim \Rightarrow \theta = \underset{\theta}{\argmax} \{ \sum^m_{s=1}\log[p(X^{(s)}_1|y^{(s)})]\}$
    namely: we only max likelihood based on $X_1$
    equally: the model only have one input feature $X_1$
    - **Conclusion:** The more $X_1$ is added, the more the model make prediction rely on $X_1$.

#### 3.1.2 In prediction process

**Conclusion:** Same conclusion with training process: the more $X_1$ is added, the more the model make decision only on $X_1$.

- Deduction:
Based on:
$$ h(x) = \underset{k\in\{1,...,K\}}{\argmax}\left[p(C_k)\prod^n_{i=1}p(X_i|C_k) \right] $$
  - For $p_0$: $h(x) = \underset{k\in\{1,...,K\}}{\argmax}\left[p(C_k) \cdot p(X_1|C_k) \cdot p(X_2|C_k) \right]$
  - For $p'$: $h(x) = \underset{k\in\{1,...,K\}}{\argmax}\left[p(C_k) \cdot p^2(X_1|C_k) \cdot p(X_2|C_k) \right]$
  - ...
  - In extreme case, add $N\rightarrow \infty$ features identical to $X_1$:
  $\begin{aligned}
    h(x) =& \underset{k\in\{1,...,K\}}{\argmax}\left[p(C_k) \cdot p^N(X_1|C_k) \cdot p(X_2|C_k) \right] \\
     \sim& \underset{k\in\{1,...,K\}}{\argmax}\left[ p(X_1|C_k) \right]
  \end{aligned}$ <br> Namely, the model makes decision only based on $X_1$

### 3.2 Effect of imbalanced dataset.

- Will class imbalance affect prediction? 
  - Yes. The model assign higher "probability score" to the majority.
- Should we balance affect prediction?
  - No. Do **NOT** intentionally balance the data! The data should draw as its natural distribution.

Because generative model fit the joint probability: $f(x,y)$. *Balancing data* is equal to change the f(x,y).
e.g. 
  - increase label A and decrease label B $\rightarrow$ $f'(x,A)>f_0(x,A)$, $f'(x,B)<f_0(x,B)$ 
  - For naive bayes, define $h_k(x)$ as the "probability score" as following: <br> 
  $$h(x) = \underset{k\in\{1,...,K\}}{\argmax}\left[p(C_k)\prod^n_{i=1}p(X_i|C_k) \right] : = \underset{k\in\{1,...,K\}}{\argmax}\left[ h_k(X) \right]$$ 
  When we intentionally duplicate A class, because the samples are duplicated, the $P(X|C_A)$ will remain unchanged; but P(A) will increase $\Rightarrow$ Given same X, $h'_A(X) > {h_0}_A(X)$. 
  i.e.
  - **Even the feature-label correlation doesn't change, the prob score can be tuned by proportion of labels.** Which is usually NOT reasonable.

Thus, for generative models, the training set are better to have a same join prob with test set (or application scenario.) 

Related Ref:
- [stackexchange](https://stats.stackexchange.com/questions/126438/balanced-datasets-in-naive-bayes): " training data should be generated by the true process"
- [quora](https://www.quora.com/Is-Naive-Bayes-able-to-handle-imbalanced-categorical-data-sets) "prior is misleading" / "not objective"

## 4. Related topics: 

### 4.1 MLE vs MAP

- Maximum Likelihood Estimation (MLE):
$$\begin{aligned}
\theta_{MLE} = \underset{\theta}{\argmax}[p(\mathcal{S}|\theta)]
\end{aligned}$$
where 
  - $p(\mathcal{S}|\theta)$ denotes a parametric model. 
  $$p(\mathcal{S}|\theta) = \prod^m_{i=1} p(s_i|\theta)$$
  - MLE is usually written as:
  $$\begin{aligned}
\theta_{MLE} =& \underset{\theta}{\argmax}[p(\mathcal{S}|\theta)]\\
=&\underset{\theta}{\argmax}\{\log[p(\mathcal{S}|\theta)]\}\\
=&\underset{\theta}{\argmax}\{\log[\prod^m_{i=1} p(s_i|\theta)]\}\\
=&\underset{\theta}{\argmax}\{\sum_i\log[p(s_i|\theta)]\}
\end{aligned}$$

- Maximum A Posteriori (MAP):
$$\begin{aligned}
  \theta_{MAP} &= \underset{\theta}{\argmax}[p(\theta|\mathcal{S})]
\end{aligned}$$
  - $P(\theta \vert \mathcal{S}) = \frac{P(\mathcal{S} \vert \theta) P(\theta)}{P(\mathcal{S})}$
  - MAP is usually written as:
  $$\begin{aligned}
  \theta_{MAP} &= \underset{\theta}{\argmax}[p(\theta|\mathcal{S})] \\
  & = \underset{\theta}{\argmax}[\frac{P(\mathcal{S} \vert \theta) P(\theta)}{P(\mathcal{S})}] \leftarrow P(\mathcal{S}) \text{is const to } \theta\\
  & = \underset{\theta}{\argmax}[P(\mathcal{S} \vert \theta) P(\theta)]\\
  & = \underset{\theta}{\argmax}\{ \log[ P(\mathcal{S} \vert \theta) P(\theta) ] \}\\
  & = \underset{\theta}{\argmax}\{ \log[ P(\mathcal{S} \vert \theta)]+\log[ P(\theta) ] \} \leftarrow \text{see MLE}\\
  & = \underset{\theta}{\argmax}\{ \sum_i\log[p(s_i|\theta)]+\log[ P(\theta) ] \}
\end{aligned}$$
- Insight - MLE vs MAP:
 If the prior $p(\theta)$ is invariant with $\theta$ (uniform prior):
 $\Rightarrow \theta_{MAP} = \theta_{MLE} = \underset{\theta}{\argmax}\{\sum_i\log[p(s_i|\theta)]\}$
 This means **MLE is a special case of MAP (uniform prior)**.

- Ref:[Github blog](https://wiseodd.github.io/techblog/2017/01/01/mle-vs-map/), [NYU slides](https://github.com/davidrosenberg/mlcourse/blob/gh-pages/Lectures/06b.MLE.pdf)

### 4.2 Generative model vs Discriminative model

Generative model:  model learns $P(X,Y)$ or $P(X|Y)$
Discriminative model: model learns $P(Y|X)$
Ref: [Wiki - Generative model](https://en.wikipedia.org/wiki/Generative_model)

Generative models model the distribution of individual classes.
Discriminative models learn the (hard or soft) boundary between classes.
Ref: [StakeExchange](https://stats.stackexchange.com/questions/12421/generative-vs-discriminative)

<!-- - Answer is **Yes**!
- From the equantion:
$$h(x) = \underset{k\in\{1,...,K\}}{\argmax}\left[p(C_k)\prod^n_{i=1}p(X_i|C_k)\right]$$
  - Total sample $N_\mathcal{S} = N$, 
  - sample $N_\mathcal{S}(C=k) := n_k$
  - Then:
  $\begin{aligned}
      N_\mathcal{S}(C=k, X=x_i) =& p_0(X=x_i|C=k)n_k \\
      =& \frac{p_0(X=x_i, C=k)}{p_0(C=k)}\cdot n_k \\
      =&\frac{p_0(C=k|X=x_i)p_0(X=x_i)}{p_0(C=k)}\cdot n_k
  \end{aligned}$
  - Then: 
  $N_\mathcal{S}(X=x_i ) = \sum_k N_\mathcal{S}(C=k, X=x_i) $
  - Sample prior:
  $\begin{aligned}
      p_\mathcal{S}(C = k) = \frac{n_k}{N}
  \end{aligned}$
  - Sample joint:
  $\begin{aligned}
      p_\mathcal{S}(C=k,X=x_i) =& \frac{N_\mathcal{S}(C=k, X=x_i)}{N} \\
      = & \frac{p_0(X=x_i, C=k)}{p_0(C=k)}\cdot\frac{n_k}{N} \\
      = & p_0(C=k, X=x_i)\cdot\frac{p_\mathcal{S}(C=k)}{p_0(C=k)}
  \end{aligned}$
  - Sample posterior:
  $\begin{aligned}
      p_\mathcal{S}(X=x_i|C = k) & = \frac{N_\mathcal{S}(C=k,X=x_i)}{N_\mathcal{S}(C=k)} \\
       = & \frac{p_0(X=x_i|C=k)n_k}{n_k} \\
      = &  p_0(X=x_i|C=k)
  \end{aligned}$
  - Sample posterior:
  $\begin{aligned}
      p_\mathcal{S}(C=k|X=x_i) &= \frac{N_\mathcal{S}(C=k,X=x_i)}{\sum_l N_\mathcal{S}(C = l,X=x_i) } \\
      & =  \frac{p_0(X=x_i|C=k)n_k}{\sum_s p_0(X=x_i|C=s)n_s} \\
      & = p_0(X=x_i|C=k) \frac{n_k}{N_\mathcal{S}(X=x_i )} \\
      & = p_0(X=x_i|C=k)\frac{p_\mathcal{S}(C = k)}{p_\mathcal{S}(X = x_i)} \\
      & \leftarrow \text{same with naive bayes} \because p_0(X=x_i|C=k) = p_\mathcal{S}(X=x_i|C=k) \text{}\\
      & = p_0(C=k|X=x_i)\cdot \frac{p_0(X=x_i)}{p_0(C=k)}\cdot\frac{p_\mathcal{S}(C = k)}{p_\mathcal{S}(X = x_i)}
  \end{aligned}$
  - Therefore:
  $$ \frac{p_\mathcal{S}(C=k_1|X=x_i) }{p_\mathcal{S}(C=k_2|X=x_i) } =\left[\frac{p_0(C=k_1|X=x_i)}{p_0(C=k_2|X=x_i)}\cdot \frac{p_0(C=k_2)}{p_0(C=k_1)} \right] \cdot \frac{p_\mathcal{S}(C = k_1)}{p_\mathcal{S}(C = k_2)}$$
  English: the prediction probability k-th class $C = k$ is proportional to the k-th class proportion $p_\mathcal{S}(C = k)$ in the sample set.

- Intuitive understanding: Naive bays is a generative model, i.e., we learn the P(x,y) and then the prediction function (posterior) P(y|x) is determined by bayes rule: $$P(y|x) = \frac{P(x,y)}{P(y)}$$
Thus, if joint distribution $P(x,y)$ changes, the prediction function may change.
  - sdf
$$P_\mathcal{S}(y|x) = \frac{P_\mathcal{S}(x,y)}{P_\mathcal{S}(x)}$$ -->

  <!-- Although seems a larger $C_k$ will increase the chance of $h(x_s) = k$ but it will reduce $p(X = x_s|C_k) = \frac{p(X, C_k)}{p(C_k)}$ as long as the joint probability holds $p(X, C_k)$

  - **Intuitively:** e.g. 
    - Assume, true joint distribution:
        P(x=0, c = 0) = 0.5
        P(x=0, c = 1) = 0
        P(x=1, c = 0) = 0.25
        P(x=1, c = 1) = 0.25
        <br>
        $P(C = 0 |x=0) = 1$, 
        $P(C = 1 |x=0) = 0$
        $P(C = 0 |x=1) = 0.5$, 
        $P(C = 1 |x=1) = 0.5$
        <br>
        $P(x = 0 |C=0) = 2/3$
        $P(x = 1 |C=0) = 1/3$
        $P(x = 0 |C=1) = 0$
        $P(x = 1 |C=1) = 1$     -->

