# Sample size calculation

## 1. Preparation knowledges

### 1.1. Effect size

[Effect size (in ab test)](https://www.simplypsychology.org/effect-size.html) is the magnitude of the difference of population mean between control group and experimental group in terms of their pooled standard deviation.

More formal terminology for the "effect size" used in ab test is "[Cohen's d]( https://en.wikipedia.org/wiki/Effect_size#Cohen's_d)"


$$D_{ef} := \frac{\mu_{EG} - \mu_{CG}}{s_{pooled}}$$
where:
- $\mu_{EG}$: population mean of the experimental group.
- $\mu_{CG}$: population mean of the control group.

- $s_{pooled}$: the pooled standard deviation of the experimental and experimental group. Can be calculated from $s_{EG}$ and $s_{CG}$.
  <!-- - $s_{pooled} = \sqrt{\frac{(N_{EG}-1)s^2_{EG} + (N_{CG}-1)s^2_{CG}}{N_{EG}+N_{CG} - 2}}$
    - $s_{EG}$: SD of the experimental group.
    - $s_{CG}$: SD of the control group.
    - $N_{EG}$: size of the experimental group.
    - $N_{CG}$: size of the control group. -->

**Notes**:
- The concept "effect size" is often used in [estimating sample size](https://en.wikipedia.org/wiki/Sample_size_determination#Estimating_sample_sizes)
- Since $\mu$ and SD ($E(\mu)$ and $E(SD)$) is almost independent of sample size, so the $D_{ef}$ should also **not affected by sample size**.

### 1.2. G\*power app

The related topic to calculate sample size is "power analysis". "G\*power" is a widely used app for power analysis. An example of using G\*power to calculate sample size is [here: Splitmetrics](https://splitmetrics.com/blog/mobile-a-b-testing-sample-size/).

A turtorial for G\*power is [here: UND-Med](https://med.und.edu/daccota/_files/pdfs/berdc_resource_pdfs/sample_size_gpower_module.pdf).

A detailed explanation of the G\*power graph in t-test is [here: Wordpress-Blog](https://testview.wordpress.com/tag/hypothesis-test/). I.e.:

<div  align="center"><img src=https://testview.files.wordpress.com/2015/07/figure-2-review.jpg style = "zoom:60%"></div>

-	The red curve is called central distribution
- The blue curve is called non-central distribution


## 2. Mechanism of sample size calculation.

An [example](https://splitmetrics.com/blog/mobile-a-b-testing-sample-size/) of using G\*power to calculate sample size is as:

<div  align="center"><img src=https://splitmetrics.com/wp-content/uploads/2018/04/8-1-1.png style = "zoom:60%"></div>

We will explain why/how the sample size is calculated via such method.

### 2.1. Explanation on the two curves

Notation:

- Curve related:
  - $\Delta \mu$: a variable denotes the difference of the population mean between the experimental group and the control group.
  - $\Delta \mu_{obs}$: the observed $\Delta \mu$, i.e., the $\Delta \mu$ we calculated with real data from the experimental and control group.
  - $E(\Delta \mu)$: the expectation of variable $\Delta \mu$.
- Hypothesis related
  - $d_{\min}$: practical significance, also called minimum detectable effect.
  - $p_2:=p_{EG}:=\mu_2$: the conversion rate from experimental group.
    - Is calculated as $p_2 = p_1 + d_{\min}$
  - $p_1:=p_{CG}:=\mu_1$: the (expected) conversion rate from control group.
  - $D_{ef}$: effect size

Red curve: the central distribution, i.e.,  the distribution of $\Delta \mu_{obs}$ under the null hypothesis. 
- $H_0 \Rightarrow E(\Delta\mu_{obs} = 0)$, that is why the center of red curve is at 0.

Blue curve: the non-central distribution, i.e.,  the distribution of $\Delta \mu_{obs}$ under a specific alternative hypothesis case $E(\Delta \mu) = D_{ef}$

- Detailed explanation:
  - Blue curve is often viewed as the distribution of $\Delta \mu_{obs}$ under the alternative hypothesis. 
  - But, different from null hypothesis: $H_0: p_{EG} = p_{CG} \Rightarrow \Delta \mu = 0$ case, if we want to quantitatively express $\Delta \mu$ from the alternative hypothesis $H_1: p_{EG} \not = p_{CG}$
  - So, to quantitatively represent $H_1$ we need to answer "How much difference can be viewed as $\not =$". 
  - $d_{\min} := p_2-p_1$ is the threshold we manually/subjectively set for the question, i.e. it is our expectation on the $\Delta \mu$.
- $D_{ef} := \frac{p_2-p_1}{s_{pooled}} = \frac{d_{\min}}{s_{pooled}}$
  - $s_{pooled}$ is the pooled SD for the control and experimental group.
  - It is the difference between the centers of the blue curve and the red curve.


### 2.2. Mechanism for sample size calculation

<div  align="center"><img src=https://splitmetrics.com/wp-content/uploads/2018/04/8-1-1.png style = "zoom:60%"></div>

- Point 1: $N \uparrow \Rightarrow$ width of the two curves will shrinkage
  - $\because$ the two curves is the distribution about mean 
  - $\therefore$ the "standard deviation" of the two curve is the "standard deviation of mean (SE)" not "standard deviation of population (SD)". 
  - $\because$ $SE \propto \frac{1}{\sqrt{N}}$,
  - $\therefore$ when $N \uparrow$,  width of the curve will shrinkage 
- Point 2: The centers of the two curves (almost) not change with $N$
  - The distance between the centers of the two curves is $D_{ef} = \frac{d_{\min}}{s_{pooled} }$
  - $\because$
    - $d_{\min}$ is manually set by us, not change with $N$
    - $s_{pooled}$ is a kind of SD, also not change with $N$ (t is the pooled value of the population standard deviation (SD) of experimental and control group.)
  - $\therefore$ $D_{ef}$ not change with $N$
- Brainstorming: Graphical relations between $N, d_{\min}, \alpha, \beta$
  - Give an $N$ value, the shape (width) of the two curves is fixed.
  - Give a $d_{\min}$ value, the distance between two curves is fixed.
  - Then, from the red curve and $\alpha$ value, we can calculate the position of $z$
  - Bring $z$ to the blue curve, we can get the value of $\beta$
- An intuition algorithm:
  - From the brainstorming above, we can regard the $\beta$ as a function of $N, d_{\min}, \alpha$ as $\beta = f_{\beta}(N, d_{\min}, \alpha)$
  - When the $d_{\min}, \alpha$ is given (as $d_{mde0}, \alpha_0$), $\beta$ is a function solely of $N$ as $\beta = f_{(\beta|d_{mde0},\alpha_0)}(N)$
  - So, we can scan over different $N$ values, find the required size value $N_{req}$ that make $f_{(\beta|d_{mde0},\alpha_0)}(N_{req}) = \beta_0$ i.e. our input $\beta$ value.
  - Then, $N_{req}$ is the solution.
- Or, more directly, from the $\beta = f_{\beta}(N, d_{\min}, \alpha)$, we can know $N$ is a function of $\alpha, \beta, d_{\min}$
  - I.e. $N = f_N(\alpha, \beta, d_{\min})$



### Ref: 
- [TowardsDataScience](https://towardsdatascience.com/required-sample-size-for-a-b-testing-6f6608dd330a): equation of sample size.
- [Blog](https://jeffshow.com/caculate-abtest-required-sample-size.html): equation of sample size.
- [StakeExchange](https://stats.stackexchange.com/questions/392979/ab-test-sample-size-calculation-by-hand): equation of sample size.
- [Splitmetrics](https://splitmetrics.com/blog/mobile-a-b-testing-sample-size/): graph illustration of relation between $p_1, p_2, \alpha, \beta$ and the width of the distribution.
- [Ucla](https://stats.idre.ucla.edu/other/gpower/two-independent-proportions-power-analysis/): power analysis.

### Remaining Qs (TBD):

- How $SE=\frac{SD}{\sqrt{N}}\propto\frac{1}{\sqrt{N}}$ is get?
- What is central limit theory?