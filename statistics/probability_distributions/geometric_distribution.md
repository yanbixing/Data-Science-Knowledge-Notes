# Geometric distribution

Ref: [Wiki-Geometric_distribution](https://en.wikipedia.org/wiki/Geometric_distribution)

## 1. Basics

- Geometric distribution describes, if we keep repeating a Bernoulli trail, the probability we got our first success result at $X$.
  - Bernoulli trial: binary outcome, with $p$ probability to get positive/success result for each trial)




- $X\sim Geo(p)$: random variable $X$ follows geometric distribution.
    $$P(X = k) = (1-p)^{(k-1)}p$$
  - Explanation: the first $(k-1)$ times fails, so there is a $(1-p)^{(k-1)}$ term, and the k-th time succeed, so there is a $p$ term.
    <div  align="center"><img src=https://www.storyofmathematics.com/wp-content/uploads/2021/03/Figure_1-300x225.png style = "zoom:100%"></div> 
  - This tendency is intuitive, since $(1-p)<1$, so $k\uparrow (1-p)^k \downarrow$


## 2. Key statistics



- **mean**: $\mathbb{E}(X) = \frac{1}{p}$
  - This is intuitive:
    - Define n = the expect number of trials to get 1 success
    - Then we should have $np=1$, i.e. $n=1/p$
- variance: $\mathrm{Var}(X) = \frac{1-p}{p^2}$
  - SD: $\sigma(X) = \frac{\sqrt{1-p}}{p}$
- median: $\mathrm{mid}(X) = {\frac {-1}{\log _{2}(1-p)}}$
- mode: $\mathrm{mod}(X) = 1$


## Example questions

[1p3a-Q2](https://www.1point3acres.com/bbs/interview/facebook-data-science-300457.html)