# Key statistics

## 1. Standard deviation

### 1.1. Standard deviation for EG/CG: SD

For group, the target in EG and CG is individual samples. (Not mean value.) So. the standard deviation we use to describe EG or CG is population standard deviation, i.e. SD.

Usually, the user action/response in AB test is binary (1,0), e.g., click or not, purchase or not, etc. This is called binomial trial or [Bernoulli_trial](https://en.wikipedia.org/wiki/Bernoulli_trial). The binary outcomes follow [Bernoulli_distribution](https://en.wikipedia.org/wiki/Bernoulli_distribution). See: [bernoulli_distribution.md](../probability_distributions/bernoulli_distribution.md).

So, we can use bernoulli distribution to calcuate statistics like SD. E.g.:

- In our AB test, 
  - The control group result is denoted as: $\{X^{CG}\}:= \{x_1^{CG},\dots,x_N^{CG}\} \underset{e.g.}{=} \{1,0,0,1,0,...\}$. 
    - The number of 1 is $n^{CG}_1$ the number of 0 is  $n^{CG}_0$
  - The control group result is denoted as: $\{X^{EG}\}$
    - The number of 1 is $n^{EG}_1$ the number of 0 is  $n^{EG}_0$
- Then
  - $E(X^{CG}) = p(X^{CG} = 1) = \frac{n^{CG}_1}{N} =: p_{CG}$
  - $s_{CG} = SD(X^{CG}) = \sqrt{p_{CG}(1-p_{CG})}$
- Note:
  - $SD \underset{\sim}{\perp} N$: SD is almost independent of N. See [standard_deviation.md](../basic_concepts/standard_deviation.md)


<!-- The mean for bernoulli distribution is 
The SD for Bernoulli_distribution is  $\sigma = \sqrt{p(1-p)}$ where $p$ is the probability of result = 1 appears.

E.g. In AB test -->

Ref: [Medium blog](https://productcoalition.com/start-here-statistics-for-a-b-testing-5f5c7e02ce1e)

(Note: Binominal distribution is a different concept. Don't misuse them. See [bernoulli_distribution.md](../probability_distributions/bernoulli_distribution.md) & [binominal_distribution.md](../probability_distributions/binominal_distribution.md))


### 1.2. Pooled standard deviation $s_{pooled}$

Pooled standard deviation is the standard deviation of multiple populations with different mean but **same standard deviation**. 

In ab test, $s_{pooled}$ is used to describe the population standard deviation the whole group combining experimental and experimental group. (I.e. SD when we considering EG and CG as a whole)
$$s_{pooled} = \sqrt{\frac{(N_{EG}-1)s^2_{EG} + (N_{CG}-1)s^2_{CG}}{N_{EG}+N_{CG} - 2}}$$
where:
- $s_{EG}$: SD of the experimental group.
- $s_{CG}$: SD of the control group.
- $N_{EG}$: size of the experimental group.
- $N_{CG}$: size of the control group.

Ref: [Wiki-Pooled_variance](https://en.wikipedia.org/wiki/Pooled_variance)



### 1.3. Standard deviation in Hypothesis: SE

For the hypothesis $H_0, H_1$ in ab testing, the quantity we care about is $\Delta \mu$ rather individual I.e.:

- $H_0 \Leftrightarrow E(\Delta\mu_{obs} = 0)$
- $H_1 \Leftrightarrow E(\Delta\mu_{obs} \not = 0)$

where:
- $\Delta \mu = p_{EG} - p_{CG}$: a variable denotes the difference of the population mean between the experimental group and the control group.
  - $\Delta \mu^{obs}= p^{obs}_1 - p^{obs}_0$: the observed $\Delta \mu$, i.e., the $\Delta \mu$ we calculated with real data from the experimental and control group.

where: 

- $p_{EG}:=p_{1}:=\mu_{EG}:=\mu_{1}$: (human-expected) conversion rate of experimental group.
  - $p^{obs}_{EG}:=p^{obs}_{1}:=\mu^{obs}_{EG}:=\mu^{obs}_{1}$: observed/calculated conversion rate of experimental group from the real data.
- $p_{CG}:=p_{0}:=\mu_{CG}:=\mu_{0}$: (human-expected) conversion rate of control group. (baseline conversion rate.)
  - $p^{obs}_{CG}:=p^{obs}_{0}:=\mu^{obs}_{CG}:=\mu^{obs}_{0}$: observed/calculated conversion rate of control group from the real data.

Thus, the "standard deviation" for $\Delta \mu$ is standard error for mean, i.e. SE.

$$SE = \frac{SD}{\sqrt{N}}$$

Note: $\because$ $SD \underset{\sim}{\perp} N$ $\therefore$ $SE\underset{\sim}{\propto} \frac{1}{\sqrt{N}}$