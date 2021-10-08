# Exponential distribution

Ref: [Wiki-Exponential_distribution](https://en.wikipedia.org/wiki/Exponential_distribution)

## 1. Basics

- $X\sim Exp(\lambda )$: random variable $X$ follow exponential distribution.

- The pdf of exponential distribution is:
  $$f(x;\lambda )={\begin{cases}\lambda e^{-\lambda x}&x\geq 0,\\0&x<0.\end{cases}}$$
  <div  align="center"><img src=https://upload.wikimedia.org/wikipedia/commons/thumb/0/02/Exponential_probability_density.svg/600px-Exponential_probability_density.svg.png style = "zoom:50%"></div> 

 - The cdf (cumulative distribution function) is
  $$F(x;\lambda) = \begin{cases}
  1-e^{-\lambda x} & x \ge 0, \\
  0 & x < 0.
  \end{cases}$$
  <div  align="center"><img src=https://upload.wikimedia.org/wikipedia/commons/thumb/b/ba/Exponential_cdf.svg/600px-Exponential_cdf.svg.png style = "zoom:50%"></div> 


## 2. Key statistics



- mean: $\mathbb{E}(X) = \frac{1}{\lambda}$
- variance: $\mathrm{Var}(X) = \frac{1}{\lambda^2}$
  - SD: $\sigma(X) = \frac{1}{\lambda}$
- median: $\mathrm{mid}(X) = \frac{\ln 2}{ \lambda}$
- mode: $\mathrm{mod}(X) = 0$

## 3. Example question

Ref: [1p3a](https://www.1point3acres.com/bbs/thread-479597-1-1.html)

There is an auction. A and B bids for 1 item. Each A and B will give an offer price $q_A$ and $q_B$ for the item ($q$ for "quote"). 

As A：

- You know your offer price.
- You knows B's offer price only when you wins the bidding, 
  - I.e. your price is higher than B's
  - I.e. $q_A > q_B$

Assume the B's offer price follow the exponential distribution. As A, how to estimate the parameter $\lambda$? 

(Assume the each time A or B gives an offer price is an independent incidence, i.e. there is no relation between A and B and different rounds.)

**Solution:**



To solve the problem, we have to use MLE.

Ideally, $p(\mathcal{S}_B) = p(q_{B,1})p(q_{B,2})\dots=\prod^N_i p(q_{B,i})$, where $p(q_{B,i}) \propto f_\lambda(q_{B,i}) = \lambda e^{-\lambda q_{B,i} }$ 

The difficulty here is sometimes we don't know $q_{B,i}$. So we need to change our strategy:

$$p(\mathcal{S}) = p[(q_{B,1},q_{A,1})]p[(q_{B,2},q_{A,2})]\dots=\prod^N_i p[(q_{B,i},q_{A,i})]$$


When $q_A>q_B$ the probability is simple, we can directly give: $ p[(q_{B,i},q_{A,i})] \propto f_\lambda(q_{B,i}) = \lambda e^{-\lambda q_{B,i} }$ 


When $q_A<q_B$, we don't know $q_{B,i}$, however, we could know the probability of such case: the probability B give higher price than A's when A's is known:  $p(q_{B,i}>q_{A,i}|q_{A,i}) = \int^\infty_{q=q_{A,i} } f_\lambda(q) = 1-F_\lambda(q_{A,i}) = e^{-\lambda q_{A,i} }$

In sum


 $$p[(q_{B,i},q_{A,i})]  {\begin{cases}\propto \lambda e^{-\lambda q_{B,i} } & q_{A,i}>q_{B,i},\\ = e^{-\lambda q_{A,i}}& q_{A,i}<q_{B,i}.\end{cases}}$$

Bring the above to $p(\mathcal{S})$, we can get $p(\mathcal{S}) \propto L(\lambda) $, where $L(\lambda) $ denotes the function of $\lambda$ proportional to the likelihood of our data. Then $\lambda$ can be solved by MLE:

$$\lambda = \underset{\lambda}{\argmin}p(\mathcal{S}) = \underset{\lambda}{\argmin}L(\lambda)$$

<!-- $p(\text{know } q_B|q_a) = p(q_A >q_B)$
$p(\text{don't know } q_B|q_a) = p(q_A < q_B)$ -->
