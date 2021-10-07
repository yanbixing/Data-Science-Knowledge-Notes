# Binominal Distribution

Ref: [Wiki-Binominal_distribution](https://en.wikipedia.org/wiki/Binomial_distribution)

## Binominal Distribution Basics

- Binominal distribution describe, doing $n$ times bernoulli trials, the **possibility** that there are exactly **$X$ times success**. (In each bernoulli trial, the possibility of 1 is p.)
- $X \sim B(n, p)$ means variable X follow binominal distribution. The total times of the bernoulli trial is $n$, the possibility of 1 (success) in each time of bernoulli trial is $p$.
- The possibility mass function for binominal distribution is like: 
    <div  align="center"><img src=http://probabilitycourse.com/images/chapter3/binomial(n=10,p=0.3)%20color.png style = "zoom:30%"></div> 

  - The x axis is the number of success $X$ among n trials .
  - The y axis is the possibility that there are exactly X times of success.
  - Ref: [Probabilitycourse](https://www.probabilitycourse.com/chapter3/3_1_5_special_discrete_distr.php)


- Note:
  - When n=1, binominal distribution is Bernoulli distribution. I.e. $B(1, p) = Bernoulli(p)$


## Binominal distribution statistics

$X \sim B(n, p)$: a random variable $X$ follow binominal distribution.

- Mean: $E(X) = np$
- Variance: $Var(X) = npq$
