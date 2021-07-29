# central limit theorem (CLT)

- $\{X_1,X_2,\dots,X_n\}$ are random samples from an i.i.d. (identical independent distribution.) Denote its variable name as X, i.e. $X\sim$ the i.i.d.
- $\mu^{true}:=E(X)$, i.e. the true mean value of the distribution.
- $\mathrm{SD}^2:=Var(X)$, i.e. the true variance of the distribution.
- $\mu^{obs}:= \frac{1}{N}\sum^N_{i=1} X_i$,i.e. the mean value calculated from the sample.

Then, when n is very large, the distribution of $\mu^{obs}$ is a normal distribution around $\mu^{true}$, with the standard error $\mathrm{SE} = \frac{SD}{\sqrt{n}}$. 

(English: No matter what the $X$'s distribution is, its sample mean is always normal distribution.)

I.e. More formally the variable $z(\mu^{obs}):=\lim_{n\rightarrow\infty}\frac{\mu^{obs}-\mu^{true}}{\frac{SD}{\sqrt{n}}}$ follow the standard normal distribution (the normal distribution with mean=0 and standard deviation=1). Ref: [Wiki-Normal_distribution](https://en.wikipedia.org/wiki/Normal_distribution)


Ref: [Wiki-CLT](https://en.wikipedia.org/wiki/Central_limit_theorem)