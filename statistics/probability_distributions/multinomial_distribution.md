# Multinomial distribution

## 1. Basics

Seems not particular notation.

The probability mass function follow combination rule.

Doing $n$ times of trials; for each trial there $k$ possible outcomes, with $p_k$ as corresponding probability. $\forall i \in [1 \to k]$, denote the $i$-th outcome appears exactly $x_i$ times, the possibility for such outcome sequence is:

$${\displaystyle {\begin{aligned}f(x_{1},\ldots ,x_{k};n,p_{1},\ldots ,p_{k})&{}=\Pr(X_{1}=x_{1}{\text{ and }}\dots {\text{ and }}X_{k}=x_{k})\\&{}={\begin{cases}{\displaystyle {n! \over x_{1}!\cdots x_{k}!}p_{1}^{x_{1}}\times \cdots \times p_{k}^{x_{k}}},\quad &{\text{when }}\sum _{i=1}^{k}x_{i}=n\\\\0&{\text{otherwise,}}\end{cases}}\end{aligned}}}$$

Ref: [Wiki](https://en.wikipedia.org/wiki/Multinomial_distribution)

Note: the above possibility is the possibility we know the frequency (times of appearance) of every value $X_i, \forall i \in [1\to k]$. If we just care about the appearance of a particular value, then, follow the "one vs all" idea, it is just a binomial distribution. And actually, binomial distribution is the a special case of multinomial distribution when $k=2$.

## 2. Typical statistics

- $E(X_i) = np_i$
- $\mathrm{Var}(X_i) = np_i(1-p_i)$
- $\mathrm{Cov}(X_i,X_j) = -np_ip_j$

Note: the mean and variance is exactly same with binomial case. Since for a certain category $i$, the number of outcome is just two: $i$ appears, $i$ not appears.

## 3. Applications:

Describe the result of rolling a dice/die.