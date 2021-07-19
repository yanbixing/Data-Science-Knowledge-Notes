# Hidden Markov Model

## 0. Notation

- Values:
  - $\mathcal{S} = \{\dots,s_{(y)i},\dots\}$: the dataset containing $m$ sequences of observable states.
  - $s_{(y)i} = \{...,y_{(i)t},...\}$: the i-th sequence in the dataset. A set of observable states at each time.
    - $s_{(y)i}(t) := y_{(i)t}$ the $t$-th observable value of the $i$-th sequence. 
    - $\mathcal{Y}$: the set of all possible states (values) of observable states.
    - $n_{\mathcal{Y}}$: The size (number of unique possible values) of $\mathcal{Y}$
  - $s_{(h)i} = \{...,h_{(i)t},...\}$: a set of hidden states at each time. "$s$" for "sequence".
    - $s_{(h)i}(t) :=h_{(i)t}$: $t$-th hidden state value of the the $i$-th sequence. 
    - $\mathcal{H}$: the set of all possible states (values) of hidden states.
    - $n_{\mathcal{H}}$: The size (number of unique possible states) of $\mathcal{H}$
    
- Model variable name:
  - $Y_t$: the t-th position of observable states in the model.
    - $\{Y_t\}$: denotes the sequence of all positions of observable states in the model.
  - $H_t$: the t-th position of hidden states in the model.
    - $\{H_t\}$: denotes the sequence of all positions of hidden states in the model.

## 1. Introduction

For the model

- $\{H_t\}, \{Y_t\}$ are discrete-time [stochastic (random) process](https://en.wikipedia.org/wiki/Stochastic_process), $n \geq 1$.
  - **Stochastic process** is a sequence of events in which the outcome at any stage depends on some probability. Ref: [OSU-Math](https://people.math.osu.edu/husen.1/teaching/571/markov_1.pdf)
  - "Personal understanding" on the term **"stochastic process"**: "stochastic" = "random", but the random here doesn't necessarily means white-noise/unpredictable. It just means "not certain/definite", the behavior usually still follow some pattern, i.e. probability distribution.
- $\{H_t\}$ is a Markov process, cannot be observed directly (can only be inferred).
  - Markov process definition: 
    - The outcome of $t$-th state only dependent on its previous state (i.e. $(t-1)$-th) 
    - The transition matrix is constant (not varies with time.)
    - Ref: [OSU-Math](https://people.math.osu.edu/husen.1/teaching/571/markov_1.pdf)
  - i.e. $p(H_t = i) = \sum_{j\in\mathcal{H}} p(H_t=i|H_{t-1}=j)p(H_{t-1}=j)$
    - $i \in \mathcal{H},j\in \mathcal{H}$
    - Transition matrix $\boldsymbol{A}$: $A_{ij} = p(H_t=i|H_{t-1}=j)$
- The observable state at time $t$, i.e. $y_t$, is only dependent on the hidden states on time $t$, i.e. $h_t$
  - i.e. $p(Y_t = a) = \sum_{i\in\mathcal{H}} p(Y_t = a|H_t=i)p(H_t=i)$
    - $i\in \mathcal{H}$, $a\in \mathcal{Y}$
    - Emission(Output) matrix  $\boldsymbol{B}$: $B_{ai} = p(Y_t = a|H_t=i)$


## 2. Training method.

The algorithm to solve HMM is [Baum–Welch algorithm](https://en.wikipedia.org/wiki/Baum-Welch_algorithm). It is a kind of [EM algorithm](https://en.wikipedia.org/wiki/Expectation-maximization_algorithm).

### 2.1. Understanding of the variables.

- The observable states sequence $s_{(y)}$ is the dataset we have. It is known.
- The hidden state sequence $s_{(h)}$ is the target (label) we want to predict. Unknown.
- The transition matrix $\boldsymbol{A}$ and outcome matrix $\boldsymbol{B}$ is the parameters we need to fit. It is unknown.
  - The dimension of $\boldsymbol{A}$ is $n_\mathcal{H} \times n_\mathcal{H}$
  - The dimension of $\boldsymbol{B}$ is $n_\mathcal{Y} \times n_\mathcal{H}$
- Besides, the first hidden state $h_1$ cannot be obtained via transition, so we need to fit another parameter: the initial state distribution $\boldsymbol{\pi}$: $\pi(i) = p(h_1 = i)$, $i\in \mathcal{H}$.
  - The dimension of $\boldsymbol{\pi}$ is $n_\mathcal{H}$ (i.e. a 1-dim vector).

In sum, we have two different types of variables to model during training:
- The intermediate variable: the hidden states $s_{(h)i}$ $\forall i$
- The parameters: $\theta = \{ \boldsymbol{A}, \boldsymbol{B}, \boldsymbol{\pi} \}$

<!-- Thus, the [EM algorithm](https://en.wikipedia.org/wiki/Expectation-maximization_algorithm) is as following: -->

### 2.2. Summary of BW algorithm.

- E-step: update the probability of different hidden states $s_{(h)}$.
  - Compute two auxiliary quantities $\alpha, \beta$ via forward, backward process.
    - The two auxiliary quantity will be used to compute another axuliary quantities $\gamma, \xi$ which will be used to update $\theta$
  - Compute the auxiliary quantity $\gamma$, which could represent the the probability hidden states given $\theta^{(r)}$

- M-step: update the parameter $\theta$.
  - Compute another auxiliary quantity $\xi$ for update $\theta$
  - Update $\theta$ ($\theta^{(r)} \rightarrow \theta^{(r+1)}$) with the auxiliary quantities $\gamma, \xi$


<!-- E step: Update the prob distribution of the hidden states according to the parameters. And the update the expectation (E) of likelihood function w.r.t current hidden state distribution. (Regard the expectation of likelihood function as a function where the hidden states is fixed and the parameter is unknown variable.)
  - s
- M step: Update the value of parameter by maximizing (M) the expectation of likelihood function. -->




## 3. Application POS Tagging

- Model:
  - Tag are hidden states
  - Words are observable states (outcomes).

## Deep Dive 1: BW Algorithm

BW algorithm is a special case of EM algorithm for HMM. Ref: [Wiki-BW_algorithm](https://en.wikipedia.org/wiki/Baum%E2%80%93Welch_algorithm)


### E-step: 

In the E-step, we aim to update the probability distribution of $\{H_t\}$ according to current $\theta$. 
- Difference vs EM: Don't need to get $P_{(s_{(y)}|\theta)}(\theta)$

First, we calculated two useful auxiliary quantities $\alpha$ via forward process:

- $\alpha _{i}(t):=p(Y_{1}=y_{1},\ldots ,Y_{t}=y_{t},H_{t}=i\mid \theta )$
  - $\alpha _{i}(1)=\pi _{i}b_{i}(y_{1})$
  - $ \alpha _{i}(t+1)=b_{i}(y_{t+1})\sum _{j=1}^{N}\alpha _{j}(t)a_{ji}$

and $\beta$ backward process:

- $\beta _{i}(t):=p(Y_{t+1}=y_{t+1},\ldots ,Y_{T}=y_{T}\mid H_{t}=i,\theta )$
  - $\beta _{i}(T)=1$
  - $\beta _{i}(t)=\sum _{j=1}^{N}\beta _{j}(t+1)a_{ij}b_{j}(y_{t+1})$
  
Then, with the two kinds of quantities, we can update an auxiliary quantity $\gamma$:
- $\gamma _{i}(t):=p(X_{t}=i\mid Y,\theta )={\frac {P(X_{t}=i,Y\mid \theta )}{P(Y\mid \theta )}}={\frac {\alpha _{i}(t)\beta _{i}(t)}{\sum _{j=1}^{N}\alpha _{j}(t)\beta _{j}(t)}}$
  - $\gamma$ directly represents probability distribution of the hidden states
  - $\gamma$ is needed when updating $\theta$

### M-step: 

In M step, we aims to update the parameter.

- Difference vs EM: Don't need to use $\theta^{(r+1)} =\underset{\theta}{\argmax}P_{(s_{(y)}|\theta)}(\theta)$, $\theta$ can be directly calculated from the two auxiliary quantities $\gamma$ and $\xi$.

First, get another auxiliary quantity $\xi$ used when updating $\theta$:
- $\xi _{ij}(t)=P(X_{t}=i,X_{t+1}=j\mid Y,\theta )={\frac {P(X_{t}=i,X_{t+1}=j,Y\mid \theta )}{P(Y\mid \theta )}}={\frac {\alpha _{i}(t)a_{ij}\beta _{j}(t+1)b_{j}(y_{t+1})}{\sum _{k=1}^{N}\sum _{w=1}^{N}\alpha _{k}(t)a_{kw}\beta _{w}(t+1)b_{w}(y_{t+1})}}$
  
Then, we can update $\theta$ with the two auxiliary quantities $\gamma$ and $\xi$:

- $\pi _{i}^{*}=\gamma _{i}(1)$
- $a_{ij}^{*}={\frac {\sum _{t=1}^{T-1}\xi _{ij}(t)}{\sum _{t=1}^{T-1}\gamma _{i}(t)}}$
- $b_{i}^{*}(v_{k})={\frac {\sum _{t=1}^{T}1_{y_{t}=v_{k}}\gamma _{i}(t)}{\sum _{t=1}^{T}\gamma _{i}(t)}}$ where:
  - $1_{y_{t}=v_{k}} :={\begin{cases}1&{\text{if }}y_{t}=v_{k},\\0&{\text{otherwise}}\end{cases}}$

### Summary:

- E-step: update the probability of different hidden states $s_{(h)}$.
  - Compute two auxiliary quantities $\alpha, \beta$ via forward, backward process.
    - The two auxiliary quantity will be used to compute another axuliary quantities $\gamma, \xi$ which will be used to update $\theta$
  - Compute the auxiliary quantity $\gamma$, which could represent the the probability hidden states given $\theta^{(r)}$

- M-step: update the parameter $\theta$.
  - Compute another auxiliary quantity $\xi$ for update $\theta$
  - Update $\theta$ ($\theta^{(r)} \rightarrow \theta^{(r+1)}$) with the auxiliary quantities $\gamma, \xi$

## Deep Dive 2: EM Algorithm


Also see: [ML-K-means](../general_machine_learning/models/k-means.md)




<!-- ### Notes: 

- The likelihood $p(s_{(y)}|\theta)$ is an expectation of likelihood (prob) $p(s_{(y)}|s_{(h)},\theta)$ over different $p(s_{(h)}|\theta)$, i.e.:
  - $p(s_y|\theta) = \sum_{s_{(h)}} p(s_y|s_{(h)},\theta) p(s_{(h)}|\theta)$
    - $p(s_y|s_{(h)},\theta)$ is a function of $(s_{(h)},\theta)$: $P_{(s_{(y)}|s_{(h),\theta})}(s_{(h)},\theta)$
    - For a certain case $s'_{(h)}, P_{(s_{(y)}|s_{(h),\theta})}(s_{(h)},\theta)$
    - To get the specific form of $p(s_y|\theta)$ as a function of $\theta$: $P_{(s_{(y)}|\theta)}(\theta)$, we need to know $p(s_{(h)}|\theta)$xs -->



### Application scenario

EM algorithm is usually applied on optimization problem with latent(unobserved) variables. (Latent variable is an intermediate quantity that is not parameter.)

Usually the problem is expressed as: get the optimal parameter via $\theta^* = \underset{\theta}{\argmax} p(s_y|\theta)$, where:

$$p(s_y|\theta) = \sum_{s_{(h)}} p(s_y|s_{(h)},\theta) p(s_{(h)}|\theta)$$

<!-- To solve the problem, we need to express $p(s_y|\theta)$ an explicit function only dependent on $\theta$: $P_{(s_{(y)}|\theta)}(\theta)$ -->


### Why EM?

Then, why EM rather than directly solve it?

We have a latent variable, thus, as shown in the decomposition, we have two underlying probability distribution to consider: $p(s_y|s_{(h)},\theta)$, $p(s_{(h)}|\theta)$.

This complexity usually make $p(s_y|\theta)$ non-convex, thus hard to solve. Ref: [StackExchange](https://stats.stackexchange.com/questions/94559/why-is-optimizing-a-mixture-of-gaussian-directly-computationally-hard)

However, with EM algorithm, in the parameter optimization (M) step: The distribution of hidden states $s_{(h)}$ is fixed, independent of $\theta$. This makes:

- The distribution $p(s_{(h)}|\theta)$ is "fixed", i.e. $p(s_{(h)}|\theta) \rightarrow w_{s_{(h)}}$ 
- Thue distribution $p(s_y|s_{(h)},\theta)$ becomes "simpler", i.e. $p(s_y|s_{(h)},\theta) \rightarrow F_{s_{(h)}}(\theta)$

And usually, when $s_{(h)}$ is fixed, the probability distribution $p(s_y|s_{(h)},\theta)$, i.e. $F_{s_{(h)}}(\theta)$ is usually convex w.r.t $\theta$. Its weighted expectation $P_{(s_{(y)}|\theta)}(\theta)= \sum_{s_{(h)}} F_{s_{(h)}}(\theta)w_{s_{(h)}} $ is also usually convex., much easier to solve. Ref: [Columbia U](http://www.columbia.edu/~mh2078/MachineLearningORFE/EM_Algorithm.pdf)


- Other refs: 
  - [StackExchange: Cite PRML](https://stats.stackexchange.com/questions/326343/why-use-em-algorithm-instead-of-just-plain-old-ml-for-mixture-model)
  - [StackExchange: hard/easy, qualitatively](https://stats.stackexchange.com/questions/64962/motivation-of-expectation-maximization-algorithm)

<!-- - $p(s_y|s_{(h)},\theta)$ is a probability distribution of $(s_{(h)},\theta)$: $P_{(s_{(y)}|s_{(h),\theta})}(s_{(h)},\theta)$
- $p(s_{(h)}|\theta)$ is a probability distribution of $(s_{(h)},\theta)$

So, our question will be how to collapse $p(s_y|\theta) = \sum_{s_{(h)}} p(s_y|s_{(h)},\theta) p(s_{(h)}|\theta)$ to a univariate function $P_{(s_{(y)}|\theta)}(\theta)$. -->

### Idea in EM Algorithm:

The EM algorithm understand the optimization problem in such ways:

- $p(s_y|s_{(h)},\theta)$ can be understood as a function whose explicit form is dependent on $s_{(h)}$: $F_{s_{(h)}}(\theta)$
  - Similarly, write $p(s_{(h)}|\theta)$ as $w_{s_{(h)}} (\theta)$
- Then, the expression of $p(s_y|\theta)$ can be written like a weighed sum (expectation) form: $p(s_y|\theta)= F_{s_{(h)1}}(\theta)w_{s_{(h)1}}(\theta) + F_{s_{(h)2}}(\theta)w_{s_{(h)2}}(\theta) +\dots$
  - Note, the weight is dependent on $\theta$, so it is not really a weighted sum.
  
However, the function is still hard to solve. (Why? TBD)

The EM algorithm simplify this problem in such way:


- In E-step, 
  - we give a fixed distribution of $s_{(h)}$
    - I.e. $\forall s_{(h)}$ $p(s_{(h)}|\theta)$ is a constant, independent of variable $\theta$, 
      - $p(s_{(h)}|\theta)$ is calculated from $r$-th time parameter $\theta^{(r)}$, but in the optimization process ($\argmax$) of $(r+1)$-th time, we treat the $\theta$ in $p(s_{(h)}|\theta)$ as a constant $\theta^{(r)}$ rather than a variable $\theta$.
    - I.e. we can write $p(s_{(h)}|\theta)$ as $w_{s_{(h)}}$
  - Then, the expression of $p(s_y|\theta)$ can be written in a weighed sum (expectation) form:
    - $P_{(s_{(y)}|\theta)}(\theta)= F_{s_{(h)1}}(\theta)w_{s_{(h)1}} + F_{s_{(h)2}}(\theta)w_{s_{(h)2}} +\dots$
- In M-step,
  - We can use $\underset{\theta}{\argmax} P_{(s_{(y)}|\theta)}(\theta)$ 
    - The $\theta$ calculated here is $\theta^{(r+1)}$
  
  
  <!-- - we have: $p(s_y|\theta)= F_{s_{(h)1}}(\theta)w_{s_{(h)1}}(\theta) + F_{s_{(h)2}}(\theta)w_{s_{(h)2}}(\theta) +\dots$ -->
<!-- - However, this function is hard to solve. (Why?)
  - If we can freeze $w_{s_{(h)}} (\theta)$ to a constant only dependent on $s_{(h)}$ i.e. $w_{s_{(h)}}$ , $p(s_y|\theta)$ can be viewed as a weighted sum (expectation) of the different forms:
  - $P_{(s_{(y)}|\theta)}(\theta) = F_{s_{(h)1}}(\theta)w_{s_{(h)1}} + F_{s_{(h)2}}(\theta)w_{s_{(h)2}} +\dots$
- And then  -->


**Notes**:
- Even given certain observation $s_{(y)}$ and parameter $\theta$, hidden states case $s_{(h)}$ are not definite, it will follow a distribution, i.e. different hidden states case $s_{(h)}$ have different probability.

### EM Algorithm:

- At time $r$, get distribution of hidden states: $P^{(r)}_{(s_{(h)}| \theta)}(s_{(h)})$:
  - Given certain observation $s_{(y)}$ and parameter $\theta^{(r)}$, the probability of a hidden state case $s_{(h)}$ follows: 
  $$\begin{aligned}
    &P^{(r)}(s_{(h)}) := p(s_{(h)}) {\text{ at time }} r
    \\&= P^{(r)}_{(s_{(h)}| \theta)}(s_{(h)}) := p(s_h|\theta^{(r)})
    \\&= P^{(r)}_{(s_{(h)}|s_{(y)}, \theta)}(s_{(h)}) := p(s_h|s_y,\theta^{(r)})
  \end{aligned}$$
  - This is easy to understand:
    - "At time r, given certain observation $s_y$ and parameter $\theta$" means: at time $r$, we take $p(s_y)=1, p(\theta_r)=1$
    - Then, under such setting, at time $r$: $p(s_h) = p(s_h|\theta_r) = p(s_h|s_y,\theta_r)$
- For time $r+1$, to get $\theta_{r+1}$:
  - We need to construct a optimization function of $\theta$
  - $P^{(r+1)}_{(s_{(y)}|\theta)}(\theta) := \sum_{s_{(h)}} P_{(s_{(y)}|s_{(h),\theta})}(s_{(h)},\theta)P^{(r)}_{(s_{(h)}| \theta)}(s_{(h)})$ 
    - All above (get $P^{(r)}_{(s_{(h)}| \theta)}(s_{(h)})$ + get $P^{(r+1)}_{(s_{(y)}|\theta)}(\theta)$ ) is called **E-step**.
    - I.e. Get expression of likelihood w.r.t. $\theta$. This expression is an expectation (E) of a likelihood function over certain $s_{(h)}$ distribution.
  - $\theta_{r+1} = \underset{\theta}{\argmax} P^{(r+1)}_{(s_{(y)}|\theta)}(\theta)$
    - This $\argmax$ process is call $M$ process.
    - I.e. we maximize (M) the likelihood (w.r.t to $\theta$)

### EM Algorithm Summary

- E-step: we get expression of likelihood $p(s_y|\theta)$ as the an **expectation** of likelihood considering hidden states $p(s_y|s_{(h)},\theta)$ 
  - First, we update the probability distribution of different hidden states cases, i.e. $P^{(r)}_{(s_{(h)}| \theta)}(s_{(h)})$, with current $\theta$ value, i.e. $\theta^{(r)}$.
  - Second, update the likelihood function $P^{(r+1)}_{(s_{(y)}|\theta)}(\theta)$ with the updated distribution.
- M-step: we update the parameter $\theta$ with the updated likelihood function as $\theta^{(r+1)} = \underset{\theta}{\argmax} P^{(r+1)}_{(s_{(y)}|\theta)}(\theta)$


<!-- ### wgt EM

Ref: [StackExchange](https://stats.stackexchange.com/questions/62940/why-is-the-expectation-maximization-algorithm-used),
- For normal ML, we need to think about $p(s_y|s_{(h)},\theta)$ and $p(s_{(h)}|\theta)$ together. 
- For EM, we just need to $p(s_y|s_{(h)},\theta)$ and $p(s_{(h)}|\theta)$  iteratively -->


<!-- - [StackExchange](https://stats.stackexchange.com/questions/94559/why-is-optimizing-a-mixture-of-gaussian-directly-computationally-hard): The $p(s_y|\theta)$ is usually not convex, hard to solve.
- [Columbia U](http://www.columbia.edu/~mh2078/MachineLearningORFE/EM_Algorithm.pdf): The is usually convex, easy to solve

- Other refs: 
  - [StackExchange: Cite PRML](https://stats.stackexchange.com/questions/326343/why-use-em-algorithm-instead-of-just-plain-old-ml-for-mixture-model)
  - [StackExchange: hard/easy, qualitatively](https://stats.stackexchange.com/questions/64962/motivation-of-expectation-maximization-algorithm) -->

 <!-- matrix $\{ \boldsymbol{A}, \boldsymbol{\pi} \}$ -->

<!-- sfadsfadfadsf

- Some understanding: 
  - , , it have a probability distribution $P_r(s_{(h)}) = P_{(s_{(h)}|s_{(y)}, \theta_r)}(s_{(h)})$.
    - i.e. Each different hidden states case $s_{(h)r}$ have a certain probability, i.e. $P_{(s_{(h)}|s_{(y)}, \theta_r)}(s_{(h)r}) = p(s_{(h)r}|s_{(y)},\theta_r)$
      - Then, under such setting, $p(s_h) = p(s_h|\theta_r) = p(s_h|s_y,\theta_r) = P_{(s_{(h)}|s_{(y)}, \theta_r)}(s_{(h)}) = P_{(s_{(h)}| \theta_r)}(s_{(h)}) = P_r(s_{(h)})$
  - Since different hidden states cases give different likelihood expression, then the expectation of likelihood is a summarization of all different $s_h$ cases
    - $P_{(s_{(y)}|\theta)}(\theta) := \sum_{s_{(h)}} P_{(s_{(y)}|s_{(h),\theta})}(s_{(h)},\theta)P_{(s_{(h)}| \theta_r)}(s_{(h)})$
    - $p(s_y|\theta) = \sum_{s_y} p(s_y|s_{(h)},\theta) p(s_{(h)}|\theta) $

- Likelihood is a function of hidden states $H$ and parameters $\theta$: $p(X|H,\theta) = L(H,\theta)$
Hidden states have a probability distribution $p(H) := p(H|\theta) = P_{(H|\theta)}(\theta)$: when parameter $\theta$ changes, the probability distribution of hidden states will also change.
A temporary value is the expectation of Likelihood given the distribution of hidden states: $L_{p(H)}(\theta) = E[ p(X,H|\theta) ]= \sum_H p(X|H,\theta)p(H|\theta) $

EM algorithm will update (probability distribution of) hidden states according to current parameter and update parameter according to current hidden states distribution iteratively:

- E-step: Update $H_r$ according to $\theta_{r-1}$, get the updated $L_{p_r(H)}(\theta)$
- M-step: Update $\theta_r$ via $\theta_r = \underset{\theta}{\argmax} L_{p_r(H)}(\theta)$



 -->
