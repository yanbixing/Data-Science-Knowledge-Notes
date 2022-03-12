# Statistical test


## 1. T test

t-stats: how much times of SE does the observed value deviates from the hypothesis value. (t-score $\uparrow$, deviation $\uparrow$, chance the hypothesis is incorrect $\uparrow$)

$$t = \frac{ | \mu^{obs} - \mu^{hypo} |}{SE(\mu)} = \frac{ | \bar{X} - \mu^{hypo} |}{ \frac{SD(X)}{\sqrt{N}} }$$

**One-sample vs two-sample**:

- one-sample test: test whether an unknown population mean is different from a specific value.
- two-sample test: test whether two unknown population means are same or not.


**t-score & p-value**: t-score $\uparrow$, p-val $\downarrow$. Explanation:

- p-val: the change that we observe a value extreme than the observed value under the distribution of current hypothesis.
- t-score: how much the observed value deviates from the hypothesis.
- Thus, the higher t-score, the larger the observed value deviates from the center of the distribution, the smaller the p-value is.
  


Ref:

- [MBAlib](https://wiki.mbalib.com/wiki/T%E6%A3%80%E9%AA%8C): definition of t-stats.
- [JMP](https://www.jmp.com/en_us/statistics-knowledge-portal/t-test/one-sample-t-test.html),[JMP](https://www.jmp.com/en_us/statistics-knowledge-portal/t-test/two-sample-t-test.html): one-sample and two-sample test


### 1.2. T test vs Z test

Mathematic form of Z-stats is same with t-stats:

$$Z = \frac{ | \mu^{obs} - \mu^{hypo} |}{SE(\mu)} = \frac{ | \bar{X} - \mu^{hypo} |}{ \frac{SD(X)}{\sqrt{N}} }$$

**Similarity**:

- Both of them are often used to test mean value of a population.
- Both of them required the mean value follows normal distribution.

**Difference**

- Z-test: 
  - test is used when sample size is large (n>30)
  - or the variance is known.

- T-test:
  - sample size is small (n<30) and population variance is unknown

<div  align="center"><img src=https://miro.medium.com/max/1400/1*XEtoA1-DkbXRdraMi4MxKQ.png style = "zoom:40%"></div>


Ref: 
- [Wiki-Z_test](https://en.wikipedia.org/wiki/Z-test): definition of z-test
- [TowardsDataScience-blog](https://towardsdatascience.com/statistical-tests-when-to-use-which-704557554740): z vs t vs chi test.
- [Bloomingtontutors](https://bloomingtontutors.com/blog/when-to-use-the-z-test-versus-t-test), [Oxford-Slides-P18](http://www.stats.ox.ac.uk/~massa/Lecture%2010.pdf): Z or t test.
- [Medium-blog](https://medium.com/dataseries/hypothesis-testing-in-machine-learning-what-for-and-why-ad6ddf3d7af2): Z or t test Figure.

### 1.3 t-distribution vs z-distribution (TBD)

Ref: [JMP-Webpage](https://www.jmp.com/en_us/statistics-knowledge-portal/t-test/t-distribution.html)

Some Key Points:

- z-distribution is just normal distribution
  - Ref: [JMP-Webpage](https://www.jmp.com/en_us/statistics-knowledge-portal/t-test/t-distribution.html), [Quora](https://www.quora.com/What-is-the-difference-between-the-z-distribution-and-the-Normal-distribution)

t vs z

Difference:

- The standard normal or z-distribution assumes that you know the population standard deviation. 
  - The t-distribution is based on the sample standard deviation. I.e. for t-distribution, you don't know population standard deviation.
- The normal distribution assumes that the population standard deviation is known.
  - The t-distribution does not make this assumption.
  - The t-distribution is defined by the degrees of freedom. These are related to the sample size.
  - As the sample size increases, the t-distribution becomes more similar to a normal distribution.
- The t-distribution is most useful for small sample sizes, when the population standard deviation is not known, or both.

Similarity:

- Like the normal distribution, the t-distribution has a smooth shape.
- Like the normal distribution, the t-distribution is symmetric. If you think about folding it in half at the mean, each side will be the same.
- Like a standard normal distribution (or z-distribution), the t-distribution has a mean of zero.

## 2. 

### 2.1. Chi-square independence test (TBD)

Also a test can use to check whether two distribution (dataset) are same or not.


### 2.2. KS (Kolmogorov–Smirnov test ) test (TBD)

A test can use to check whether two distribution (dataset) are same or not.

KL-divergence vs KS distance/test: https://stats.stackexchange.com/questions/9311/kullback-leibler-vs-kolmogorov-smirnov-distance


## Deep dive: 1/N vs 1/(n-1) - population vs sample

### Unbiased estimation of variance

We sample $n$ samples from a population with size $N$. Then, define:
- population mean ($\mu$) / variance ($\sigma^2$): the mean and variance of the whole population
  - population mean/var can be viewed as the ground true mean/var of the population.
  - population mean/var can be viewed as the parameter to generate the population
  $$\begin{cases}
    \mu = \frac{\sum^N_i X_i}{N}\\
    \sigma^2 = \frac{\sum^N_i (X_i-\mu)^2 }{N}
  \end{cases}$$

- sample mean ($\bar{X}$)/variance($s$): the estimation of population mean and variance with samples from the population.
  - divisor 1/n will underestimate the variance; divisor 1/(n-1) is an unbiased estimation of population variance.
  $$\begin{cases}
    \bar{X} = \frac{\sum^n_i X_i}{n}\\
    s^2 = \frac{\sum^n_i (X_i-\bar{X})^2 }{n-1}
  \end{cases}$$

Ref: 
- [KhanAcademy](https://www.khanacademy.org/math/ap-statistics/summarizing-quantitative-data-ap/more-standard-deviation/v/review-and-intuition-why-we-divide-by-n-1-for-the-unbiased-sample-variance): population vs sample mean/var
- [Wiki-Bias_of_an_estimator](https://en.wikipedia.org/wiki/Bias_of_an_estimator#Sample_variance): why 1/(n-1) is UNbiased for variance.

### Unbiased estimation of standard deviation

Although divisor is unbiased for estimation of variance, but it is biased for estimation of standard deviation, i.e.:
$$E(s^2) = \sigma^2$$
but:
$$E(s) \not = \sigma$$

 To have unbiased estimation of standard deviation, we need to correct the divisor according to sample size; in real application, for convenience, we usually take $k(n) = 1.5$

 $$s_{\hat{\sigma}} = \sqrt{\frac{\sum^n_i (X_i-\bar{X})^2 }{n-k(n)}} \sim \sqrt{\frac{\sum^n_i (X_i-\bar{X})^2 }{n-1.5}}$$ 

Ref: 
- [Wiki-Unbiased_estimation_of_standard_deviation](https://en.wikipedia.org/wiki/Unbiased_estimation_of_standard_deviation): why 1/(n-1) is BIASED for standard deviation.
- [KhanAcademy](https://www.khanacademy.org/math/ap-statistics/summarizing-quantitative-data-ap/measuring-spread-quantitative/v/sample-standard-deviation-and-bias)


