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