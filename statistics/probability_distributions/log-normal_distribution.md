# Log-normal distribution

Summary: If $\ln(X) \sim \mathcal{N}(\mu,\sigma^2)$, log of variable $X$ follow normal distribution, then, the variable $X$ follow log-normal distribution.

## 1. Basics

Let $Z\sim \mathcal{N}(0,1)$ i.e. standard normal distribution ($\mu = 0, \sigma^2=1 $)

$X = e^{\mu + \sigma Z}$

Note as: $\ln(X) \sim \mathcal{N}(\mu,\sigma^2)$

Ref: [Wiki](https://en.wikipedia.org/wiki/Log-normal_distribution)

## 2. TBD

## 3. Applications:

### 3.1 Why stock price is log normal

**Key point summary**:

- Stock price have compound interests
- unit time interests is usally in normal distribution

**Deduction:**

Discrete expression for stock price: 

- $\lambda := \frac{\Delta r}{\Delta t}$ is the unit-time interest rate.
- The stock price after n times compounding is:
$\begin{aligned}
    X(n\Delta t) =& (1+\Delta r)^n X(0) \\
    =& (1+\frac{\Delta r}{\Delta t}\Delta t)^n X(0) \\
    =& (1+\lambda \Delta t)^n X(0)
\end{aligned}$

Continuous experssion for stock price: 
- $T:=n \Delta t$
- $X(T) = \underset{n\to\infty}{\lim} (1+\frac{\lambda T}{n})^nX(0) = e^{\lambda T} X(0)$
- i.e. $X(T)  = e^{\lambda T + \ln X(0)}$
- or $\ln X(T)  = \lambda T + \ln X(0)$

If the interest rate $\lambda \sim \mathcal{N}(\mu_\lambda, \sigma_\lambda^2)$
- Let $Z = (\frac{\lambda - \mu_\lambda }{ \sigma_\lambda}) \sim \mathcal{N}(0,1)$
- $X(T)  = e^{[\mu_\lambda T + \ln X(0)] + [\sigma_\lambda T] Z  }$
- $\ln X(T) \sim \mathcal{N}(  [\mu_\lambda T + \ln X(0)], (\sigma_\lambda T)^2 )$

Note:

- $X\sim \mathcal{N}(\mu,\sigma^2)$
- $f(x)=\frac {1}{\sigma {\sqrt {2\pi }}}e^{-{\frac {1}{2}}\left({\frac {x-\mu }{\sigma }}\right)^{2}}$
- If $z = \frac{x-\mu}{\sigma}$, $f(z) = \frac {1}{ {\sqrt {2\pi }}}e^{-{\frac {1}{2}}\left({z}\right)^{2}}= \frac {1}{1 {\sqrt {2\pi }}}e^{-{\frac {1}{2}}\left({\frac {z-0 }{1 }}\right)^{2}}$
- Then $Z = \frac{X-\mu}{\sigma} \sim \mathcal{N}(0,1)$

Ref: [FinanceZacks-Blog](https://finance.zacks.com/stock-prices-considered-lognormal-10606.html), [investopedia-blog](
https://www.investopedia.com/articles/investing/102014/lognormal-and-normal-distribution.asp)