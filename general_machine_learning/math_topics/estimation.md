# MLE vs MAP

## 1. Maximum Likelihood Estimation (MLE):
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

## 2. Maximum A Posteriori (MAP):
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

## 3. MLE vs MAP:

- MLE: maximize the likelihood of sample set given parameter
  - I.e. MLE <mark style="background-color:yellow;"><font color="#0000dd">**only**</font></mark> consider the <mark style="background-color:yellow;"><font color="#0000dd">**likelihood of sample**</font></mark>
- MAP: maximize the <mark style="background-color:yellow;"><font color="#0000dd">**joint probability**</font></mark> of samples and parameter
  - I.e. MAP consider <mark style="background-color:yellow;"><font color="#0000dd">**both likelihood of sample** and **prior probability of parameter**</font></mark>.


- MLE = MAP when parameter is uniformly distributed. 
  - "uniform distribution": different parameter have same probability.
    - i.e. $P(\theta) = const$
    - English: "uniform" (distribution) is more formal expression. Ref: [StackExchange](https://physics.stackexchange.com/questions/436562/what-is-the-difference-between-uniform-distribution-and-even-distribution)
  - Proof: 
    $$\begin{aligned}
        \theta_{MAP} &= \underset{\theta}{\argmax}\{ \sum_i\log[p(s_i|\theta)]+\log[ P(\theta) ] \}\\
        &= \underset{\theta}{\argmax}\{ \sum_i\log[p(s_i|\theta)]+const ] \}\\
        &= \underset{\theta}{\argmax}\{ \sum_i\log[p(s_i|\theta)] \}\\
        &=\theta_{MLE}
    \end{aligned}$$

- This means **MLE is a special case of MAP**:
  - MLE = MAP under **uniform prior**. OR
  - MLE = MAP under **uniform parameter distribution**.


Ref:[Github blog](https://wiseodd.github.io/techblog/2017/01/01/mle-vs-map/), [NYU slides](https://github.com/davidrosenberg/mlcourse/blob/gh-pages/Lectures/06b.MLE.pdf)


## 4. Interview Qs

Typical MLE Q workflow:

- Get the expression of likelihood of observation w.r.t. the parameter.
- Use derivative = 0 to get the parameter maximize the likelihood.

### 4.1 Unbiased estimation of $p_H$ for tossing a coin

**Q**: When you toss a coin $N$ times, head appears $n_H$ times, what is the unbiased estimation of the probability that head appears?

**Solution:** 

Perspective 1: MLE understanding 

- The probability of such observation should follow binomial distribution, denote head prob as $p$, we can get the prob of such observation:
  - $ P(n_H; N, p) = \binom{N}{n_H} p^{n_H} (1-p)^{N-n_H}$
- Unbiased estimation can be get by MLE. 
  - $\hat{p} = \underset{p}{\argmax}P(n_H; N, p) = \underset{p}{\argmin} NNL$
  - $NNL = -\log P(n_H; N, p)  = - [n_H \log p + (N-n_H)\log(1-p) ] $
- Use derivative to solve, i.e. $\partial_p NNL =0$
  - $\frac{n_H}{p} - \frac{N-n_H}{1-p} \Rightarrow n_H - n_H p = Np-n_hp \Rightarrow p = \frac{n_H}{N}$
- Thus, $\hat{p} = \frac{n_H}{N}$$

Perspective 2: cross-entropy understanding

- Given:
  - the estimated prob distribution ($\hat{P}$) of head $\hat{P}_H := \hat{p}$ and tail $\hat{P}_T := 1 - \hat{p}$ 
  - the true observed prob distribution ($P^*$) of head $P^*_H = \frac{n_H}{N}$ and tail $P^*_T = \frac{N-n_H}{N}$
  - Definition of CE: $H(P,\hat{P}):=-\sum _{x\in {\mathcal {X}}}P^*(x)\,\log \hat{P}(x)$
- The CE for the observed result is:
  $\begin{aligned}
    H(P,\hat{P}) = & - (P^*_H\log\hat{P}_H  + P^*_N\log\hat{P}_N) \\
    =& -\left[ \frac{n_H}{N} \log \hat{p}+\frac{N-n_H}{N}\log(1-\hat{p}) \right]
  \end{aligned}$
- Minimize CE will give us the $\hat{p}$:
  - $\hat{p} = \underset{\hat{p}}{\argmin} CE$
  - Note: $CE$ is exactly $N\cdot NNL$ in the MLE understanding, so the the solution is same
- Thus, $\hat{p} = \frac{n_H}{N}$