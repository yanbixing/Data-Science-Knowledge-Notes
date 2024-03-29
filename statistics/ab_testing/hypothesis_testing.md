# Hypothesis testing

Ref: [StatTrek](https://stattrek.com/hypothesis-test/hypothesis-testing.aspx)

## 0. Interview Qs

### 0.1 How to test a coin is fair

- Solution: Hypothesis testing
  - Ref: 
    - [Wiki](https://en.wikipedia.org/wiki/Checking_whether_a_coin_is_fair) (TBD)
    - [Zhihu](https://zhuanlan.zhihu.com/p/89065805),
    - [CSDN](https://blog.csdn.net/Tonywu2018/article/details/83901831)
- MyAns:
- Advanced version: determine a dice is fair or not.
  - Ref: [1p3a](https://www.1point3acres.com/bbs/thread-676967-1-1.html): M-Itv.
  - MyAns: Similar idea, use binomial/one-vs-all way, check whether each different faces have a 1/6 prob 


## 1. Two types of hypothesis

Normally, there are two kinds of hypothesis:

- Null hypothesis ($H_0$): the hypothesis we want to reject.
  - Normally it claims the incidence is purely random and not affected by other factors.
  - e.g. in AB test: "the experiment condition we change cannot affect the experiment result"

- Alternative hypothesis ($H_1$ or $H_a$): the hypothesis we want to accept.
  - Normally it claims the incidence would be affected by other factors.
  - e.g. in AB test: "the experiment condition we change can affect the experiment result"


Relation between null and alternative hypothesis:

- Alternative hypothesis = **NOT** "null hypotheses" 
  - The idea of AB test is to prove $H_1$ by rejecting $H_0$
- I.e. The union of null and alternative hypothesis is the complete set. 
- I.e. The alternative hypothesis is the [complement set](https://en.wikipedia.org/wiki/Complement_(set_theory)) of the alternative hypothesis.

Example: toss a coin, observe the probability of head up $p(H)$.

- Null hypothesis: $p(H) = 0.5$
- Alternative hypothesis: $p(H) \neq 0.5$

## 2. Type 1 and type 2 error

### 2.1. Definition:

  - Type 1 error: reject null hypothesis when it is true.
  - Type 2 error ($\beta$, sensitivity): accept null hypothesis when it is false.
    - null hypothesis is false = alternative hypothesis is true. 

#### 2.1.1. Probability representation:

  - Type 1 error: $\alpha = p(\text{reject } H_0| H_0 \text{ is true})$, also called **significant level**.
    - English: if the null hypothesis is correct, the probability we observe a value more extreme than the experiment result.
  - Type 2 error:$\beta = p(\text{accept } H_0| H_1 \text{ is true})$
    - English: if the alternative hypothesis is correct, the probability we observe a value more extreme than the experiment result.
    <div  align="center"><img src=https://dp8v87cz8a7qa.cloudfront.net/45396/5bd20d03240611540492547.png style = "zoom:50%"></div>

#### 2.1.2. Metric representation:

Definition:

- Ground true P := "$H_1$ is true"; Ground true N := "$H_0$ is true"
- Label P := $H_1$ is accepted; Label N := $H_0$ is accepted

Representation

- $\alpha = FPR = \frac{FP}{N}= \frac{FP}{FP+TN}$ 
  - reject $H_0$: predicted label is P
  - $H_0$ is true: true label is N 
  - $\therefore$ $\alpha = \frac{FP}{GroundN} = FPR$
- $\beta = FNR = \frac{FN}{P}= \frac{FN}{TP+FN}$
  - Accept $H_0$: predicted label is N
  - $H_1$ is true: true label is P
  - $\therefore$ $\beta = \frac{FN}{GroundP} = FNR$
  
Note:

- $\mathrm{Sensitivity} := 1 - \beta = 1- \frac{FN}{TP+FN} = \frac{TP}{TP+FN} = TPR = \mathrm{Recall}$
- Sensitivity is also called "power" or "statistical power".


Ref: [Wiki-FPR&FNR](https://en.wikipedia.org/wiki/False_positives_and_false_negatives#False_positive_and_false_negative_rates), [OptimizeSmart](https://www.optimizesmart.com/understanding-ab-testing-statistics-to-get-real-lift-in-conversions/)

### 2.2. Effect of sample size

Ref: [Udacity-Ab_test](https://classroom.udacity.com/courses/ud257/lessons/4018018619/concepts/40043987050923)

**Conclusion:**

- Chance of type-1 error is NOT affected by the sample size.
- Chance of type-2 error decreases as we increase the sample size.
  - I.e. when the sample size is small. The result hypothesis may have a higher chance to be the type-2 error.

**Explanation:**

Typically, what we test is the mean of the population. Thus, the standard deviation of is SE, which will be affected by sample size $N$ (i.e. $\mathrm{SE} \propto \frac{1}{\sqrt{N}}$), thus the probability of type-1 and type-2 error ($\alpha$ and $\beta$) may be affected by the sample size.

It doesn't matter to set:
- $H_0$: $\mu = 0$
- $H_1$: $\mu \neq 0$

Then we can visualize the effect of sample size on $\alpha$ and $\beta$ as:
- (a) $\alpha$ for small $N$
- (b) $\alpha$ for large $N$, when $N$ increases, the shape of the pdf will be sharper.
- (c) $\beta$ for small $N$
- (d) $\beta$ for large $N$

<div  align="center"><img src=./hypothesis_testing_asset/type_1_and_type_2_error_vs_sample_size.jpeg style = "zoom:50%"></div>

- Under $H_0$, we have an expected distribution for the observed $\mu$
  - And from the given certain significant level $\alpha_0$, we can determine the confidence interval.
  - We $\mu^{obs}$ fall outside of the confidence interval under $H_0$, we can reject $H_0$, i.e. we say $H_1$ is statistically significant.
- "$H_0$ is true" means the ground true value of $\mu$ is 0. Then, the distribution of $\mu^{obs}$ is exactly the the expected distribution for the observed $\mu$ of $H_0$
  - Type 1 error is the case that $\mu^{obs}$ fall outside of the confidence interval under $H_0$
  - Then, the probability of type-1 error ($\alpha$) is the part of distribution of $\mu^{obs}$ outside of the confidence interval of $H_0$ , as the orange area shown in (a).
  - This orange area is exactly $\alpha_0$, it is independent of width of the curve, i.e. it is the sample size, as shown in (a) and (b).
- "$H_0$ is false" means the ground true value of $\mu$ is not 0. Then, the distribution of $\mu^{obs}$ will deviates from the the expected distribution for the observed $\mu$ under $H_0$.
  - Type 2 error is the case that that $\mu^{obs}$ fall inside the confidence interval of $H_0$
  - Then, the probability of type-2 error ($\beta$) is the intersection between the distribution of $\mu^{obs}$ and the confidence interval, as the purple area shown in (c).
  - This purple area in heavily affected by width of the curve, sharper curve (larger $N$) will result in smaller chance of type-2 error. 

<!-- null hypothesis expect the observed value to be zero -->

**Application:**

When the sample size is small, the chance to make type-1 error is small, as long as you set the confidence interval is large, as in figure (a). However, the chance of type-2 error is high, as in figure (c). To reduce the chance of type-2 error, you need to increase the sample size. I.e.:

- To reduce chance of type-1 error, we reduce the set alpha-value (increase the confidence interval.)
- But this measure will increase the chance of type-2 error.
  
- Then, to reduce the chance of type-2 error, we increase sample size.
  

<div  align="center"><img src=./hypothesis_testing_asset/type_1_vs_type_2_error.jpeg style = "zoom:30%"></div>

**Trade-off relation:** from the above description, we can conclude:

- Under fixed $N$, there is a trade-off between $\alpha$ and $\beta$, .
  - I.e.: $\alpha \downarrow$, $\beta \uparrow$
- Under fixed $\alpha$, there is a trade-off between $\beta$ and $N$.
  - I.e.: $N \uparrow$, $\beta \downarrow$

Note: $\alpha \downarrow = \mathrm{CI} \uparrow$

## 3. P-value

### 3.1. Definition and understanding

[p-value](https://en.wikipedia.org/wiki/P-value): under the null hypothesis, the probability that we obtain results more extreme than current observation.

- Not strict but intuitive understandings:
  - p-value is  "the strength that the evidence supports null hypothesis"
- This means:
  - The larger the p-value, the more we should accept null hypothesis.
  - The smaller the p-value, the more we should reject null hypothesis.
- This is because:
  - The larger p-value, more possible to observe current result, the more possible that null hypothesis is correct.
  - The smaller p-value, the more extreme/impossible to observe current result, the more possible that null hypothesis is wrong.

<div  align="center"><img src=https://scientistseessquirrel.files.wordpress.com/2015/02/p-value_in_statistical_significance_testing-svg.png?w=640 style = "zoom:60%"></div>


Region of acceptance: the range that support the null hypothesis.

**two-tail and one-tail**: E.g. observation is $x_0$, p-value is 5%.

- One-tail: the probability we observe $x>x_0$ is 5%
- Two-tail: the probability we observe ($x>x_0$ or $x<-x_0$) is 5%
  - I.e. the probability we observe $x>x_0$ is 2.5%


### Concept differentiation: P-val vs $\alpha$

Ref: [Blog](https://blog.minitab.com/en/adventures-in-statistics-2/understanding-hypothesis-tests-significance-levels-alpha-and-p-values-in-statistics)

The idea/process is similar, the usage/purpose is different

P-val: given distribution and a observation, we want to get p-val, i.e.,the prob that a observation is extreme than current observation.

<div  align="center"><img src=https://blog.minitab.com/hubfs/Imported_Blog_Media/sig_level_05.png style = "zoom:60%"></div>

$\alpha$: given distribution and $\alpha$, we want to get the confidence interval, i.e. the range we expect the observation to fall into.

<div  align="center"><img src=https://blog.minitab.com/hubfs/Imported_Blog_Media/sig_level_05.png style = "zoom:60%"></div>
