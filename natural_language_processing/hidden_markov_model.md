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

$

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

The algorithm to solve HMM is [Baum–Welch algorithm](https://en.wikipedia.org/wiki/Expectation%E2%80%93maximization_algorithm). It is a kind of [EM algorithm](https://en.wikipedia.org/wiki/Expectation%E2%80%93maximization_algorithm).

Typically, when applying HMM, 

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

Thus, the [EM algorithm](https://en.wikipedia.org/wiki/Expectation%E2%80%93maximization_algorithm) is as following:)


- E-step: update the probability distribution of $\{H_t\}$ according to current $\theta$
- M-step: update $\theta$ via maximize


E step: Update the prob distribution of the hidden states according to the parameters. And the update the expectation (E) of likelihood function w.r.t current hidden state distribution. (Regard the expectation of likelihood function as a function where the hidden states is fixed and the parameter is unknown variable.)
  - s
- M step: Update the value of parameter by maximizing (M) the expectation of likelihood function.




### POS Tagging

- Model:
  - Tag are hidden states
  - Words are observable states (outcomes).

## Deep Dive: EM Algorithm

Likelihood is a function of hidden states $H$ and parameters $\theta$: $p(X|H,\theta) = L(H,\theta)$
Hidden states have a probability distribution $p(H) := p(H|\theta) = P_{(H|\theta)}(\theta)$: when parameter $\theta$ changes, the probability distribution of hidden states will also change.
A temporary value is the expectation of Likelihood given the distribution of hidden states: $L_{p(H)}(\theta) = E[ p(X,H|\theta) ]= \sum_H p(X|H,\theta)p(H|\theta) $

EM algorithm will update (probability distribution of) hidden states according to current parameter and update parameter according to current hidden states distribution iteratively:

- E-step: Update $H_r$ according to $\theta_{r-1}$, get the updated $L_{p_r(H)}(\theta)$
- M-step: Update $\theta_r$ via $\theta_r = \underset{\theta}{\argmax} L_{p_r(H)}(\theta)$
