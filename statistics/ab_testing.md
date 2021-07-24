# AB Testing

## AB Testing

Ref: [TowardsDataScience-blog](https://towardsdatascience.com/a-summary-of-udacity-a-b-testing-course-9ecc32dedbb1), [1p3a-bbs](https://www.1point3acres.com/bbs/thread-470867-1-1.html)

### Definition
A/B testing: A way to compare two versions of a single variable. [Wiki-AB_testing](https://en.wikipedia.org/wiki/A/B_testing)
    - English: typically, we test the response of the population to two versions of a product, the two versions **only have 1 difference**, all other conditions/design are same. Our purpose is to determine which one is better.

### Process

Ref: [TowardsDataScience](https://towardsdatascience.com/a-summary-of-udacity-a-b-testing-course-9ecc32dedbb1)

0. Brainstorm:
   - What is your goal/objective?
   - Do you have existing metric/target?
1. Choose the evaluation metric and invariant metric.
   - Evaluation metric is "what do you care about?"
   - Invariant metric is for sanity check, i.e. "what should be affected during the experiment"
   - Need to consider about sensitivity and robustness when choosing metric.
2. Choose: 
   - Significant level ($\alpha$)
   - Sensitivity ($1-\beta$, also called statistical power)
   - Practical significant level $d$ (also called minimum detectable effect)
3. Calculate the required sample size.
   - Required quantity: 
     - $p_0$: baseline conversion rate.
     - $d$: minimum detectable effect (practical significant).
     - $\alpha$: significant level
     - $1-\beta$: sensitivity
   - Need to consider about unit of diversion, population, duration.



## Background Knowledges

### Metric

#### Typical metrics in AB testing:
- Sum and counts
- Statistics of distribution (e.g. mean, median, percentiles)
- Probability and rates (e.g. click through probability/rates)
- Ratios: division between any two numbers.

#### Sensitivity and robustness

**Definition:**

- Sensitivity: Whether the metric is *sensitive* to the change of the data.
  - English: if a part of the data changes, how much the metric is affected. 
  - The higher the metric value changes, the higher sensitivity.
- Robustness: Whether the metric is robust (resistant) to the change of *outliers*.
  - English: if a part of the data become outliers, i.e. have extreme abnormal values, how much the metric is affected.
  - The lower the metric value changes, the higher robustness.

Generally, there should be a counter effect between sensitivity and robustness. 
  - This is because *outlier* can be viewed as a kind extreme large change of data. 
  - If the metric is easily affected by change of particular data, then it should have higher sensitivity but lower robustness.
  - If the metric is hardly affected by change of particular data, then it should have lower sensitivity but higher robustness.
- E.g. mean vs median
  - Typically:
    - mean: have higher sensitivity lower robustness .
    - median: median have lower sensitivity higher robustness.
  - Exp: data: $[1,1,2,3,3] \rightarrow [1,1,2,3, {\color{red}{93}}]$
    - mean: $2 \rightarrow \textcolor{red}{20}$
    - median: $2 \rightarrow 2$


Note (personal understanding): although mathematically outlier is a also a kind of change, in terms of the purpose,sensitivity aims to capture small changes in a relatively large part of samples. While the robustness aims to exclude the affect from very extreme individual samples. For example can can design a metric that ignore the outliers and then get average of "normal" samples, then, the sensitivity and robustness may be achieved at same time. (?)

Q: How to measure/observe the sensitivity and robustness?
- Use real experiment, 
  - i.e. change data in the dataset and observe the change of metric.
  - e.g.: "E.g. mean vs median" above.
- Use A/A test:
  - If the metric on the two identical groups of data are same, then it has high robustness and low sensitivity. Vice versa.
  - A/A test: two groups of test, noting changes. 
    - I.e. Test the response of the population to the same version of a product.
    - Can be achieved by randomly sample two group of data from the A version or B version. 
    - The purpose of A/A test is to examine the stability of statistics/metric. 
- Retrospective cohort analysis [Ref: wiki](https://en.wikipedia.org/wiki/Retrospective_cohort_study) (TBD???)
  - If sample with same metric are from an identical group, the metric is stable, i.e. it has high robustness and low sensitivity. (Else, the metric is sensitive to other unobservable factors.)
    - Looks a reverse of A/A test.
  - Retrospective cohort analysis: for samples with same experimental result, whether they have same experimental condition.
    - I.e. here, we analysis if same experimental results have a similar experimental conditions.
    - Cohort: group of individual share same **observable** properties.
    - A/A test can be viewed as prospective cohort analysis: for samples with same observable experimental condition, if they have same experimental result.
    <div  align="center"><img src=https://upload.wikimedia.org/wikipedia/commons/thumb/1/14/ExplainingCaseControlSJW-en.svg/462px-ExplainingCaseControlSJW-en.svg.png style = "zoom:60%"></div>
  
    - Ref:[1p3a-bbs](https://www.1point3acres.com/bbs/thread-529848-1-1.html), [github-blog](https://nancyyanyu.github.io/posts/17c5bb19/), [TowardsDataScience](https://towardsdatascience.com/what-i-learned-from-udacitys-course-on-a-b-testing-by-google-45f6d3297f42)
  

## Deep Dive: Sample size calculation

### DD-SSC.1. Basics knowledges

#### DD-SSC.1.1. SD vs SE: different kinds of "standard deviation"

- SD: standard deviation of the **population**
  - We have a sample set with N samples: $\mathcal{S} = \{x_1,\dots, x_N\}$
    - $SD:=$ the standard deviation of theses samples. 
  - $SD:=\sigma(x) = \sqrt{\frac{\sum^N_{i-1} (x_i - \mu)^2 }{N}} = \sqrt{Var(x)}$
    - $Var(x) := E[ (x-\bar{x})^2] = E(x^2) - [E(x)]^2$
    - $\mu(x)$ means the standard deviation of the x variable.
    - $E(x)$ means the expectation of the x variable.'
  - From the definition, we can see **SD is almost independent of sample size**. I.e. $E(SD) \perp N$ (TBD: any proof/ref?)
- SE: standard 'error' of the mean value. Also written as "SEM"
  - We have enormous sample sets (each has $N$ samples)  $\{\mathcal{S}_1,\mathcal{S}_2,\dots\}$ drawn from a same distribution.
    - For each sample set, we have a mean value: $\mu_1, \mu_2, \dots$
  - $SE:=$ the standard deviation of the these mean values
    - $SE:=\sigma(\mu)$
    - $\sigma(\mu)$ means the standard deviation of the $\mu$ variable.
    - $SE=\frac{SD}{\sqrt{N}}\propto\frac{1}{\sqrt{N}}$
- Ref: 
  - [Openanesthesia](https://www.openanesthesia.org/se_vs-_sd_calculation/): 'SD' vs. 'SE'
  - [Wiki-Standard_deviation](https://en.wikipedia.org/wiki/Standard_deviation#:~:text=The%20mean's%20standard%20error%20turns,root%20of%20the%20sample%20size.): $SE=\frac{SD}{\sqrt{N}}$



#### DD-SSC.1.2. Effect size

[Effect size (in ab test)](https://www.simplypsychology.org/effect-size.html) is the magnitude of the difference of population mean between control group and experimental group in terms of their pooled standard deviation.

More formal terminology for the "effect size" used in ab test is "[Cohen's d]( https://en.wikipedia.org/wiki/Effect_size#Cohen's_d)"


$$D_{ef} := \frac{\mu_{EG} - \mu_{CG}}{s_{pooled}}$$
where:
- $\mu_{EG}$: population mean of the experimental group.
- $\mu_{CG}$: population mean of the control group.

- $s_{pooled}$: the pooled standard deviation of the experimental and experimental group
  - $s_{pooled} = \sqrt{\frac{(N_{EG}-1)s^2_{EG} + (N_{CG}-1)s^2_{CG}}{N_{EG}+N_{CG} - 2}}$
    - $s_{EG}$: SD of the experimental group.
    - $s_{CG}$: SD of the control group.
    - $N_{EG}$: size of the experimental group.
    - $N_{CG}$: size of the control group.

**Notes**:
- The concept "effect size" is often used in [estimating sample size](https://en.wikipedia.org/wiki/Sample_size_determination#Estimating_sample_sizes)
- Since $\mu$ and SD ($E(\mu)$ and $E(SD)$) is almost independent of sample size, so the $D_{ef}$ should also **not affected by sample size**.

#### DD-SSC.1.2. G\*power app

The related topic to calculate sample size is "power analysis". "G\*power" is a widely used app for power analysis. An example of using G\*power to calculate sample size is [here: Splitmetrics](https://splitmetrics.com/blog/mobile-a-b-testing-sample-size/).

A turtorial for G\*power is [here: UND-Med](https://med.und.edu/daccota/_files/pdfs/berdc_resource_pdfs/sample_size_gpower_module.pdf).

A detailed explanation of the G\*power graph in t-test is [here: Wordpress-Blog](https://testview.wordpress.com/tag/hypothesis-test/). I.e.:

<div  align="center"><img src=https://testview.files.wordpress.com/2015/07/figure-2-review.jpg style = "zoom:60%"></div>

-	The red curve is called central distribution
- The blue curve is called non-central distribution


### DD-SSC.2. Mechanism of sample size calculation.

An [example](https://splitmetrics.com/blog/mobile-a-b-testing-sample-size/) of using G\*power to calculate sample size is as:

<div  align="center"><img src=https://splitmetrics.com/wp-content/uploads/2018/04/8-1-1.png style = "zoom:60%"></div>

We will explain why/how the sample size is calculated via such method.

#### DD-SSC.2.1. Explanation on the two curves

Notation:

- Curve related:
  - $\Delta \mu$: a variable denotes the difference of the population mean between the experimental group and the control group.
  - $\Delta \mu_{obs}$: the observed $\Delta \mu$, i.e., the $\Delta \mu$ we calculated with real data from the experimental and control group.
  - $E(\Delta \mu)$: the expectation of variable $\Delta \mu$.
- Hypothesis related
  - $d_{mde}$: practical significance, also called minimum detectable effect.
  - $p_2:=p_{EG}:=\mu_2$: the conversion rate from experimental group.
    - Is calculated as $p_2 = p_1 + d_{mde}$
  - $p_1:=p_{CG}:=\mu_1$: the (expected) conversion rate from control group.
  - $D_{ef}$: effect size

Red curve: the central distribution, i.e.,  the distribution of $\Delta \mu_{obs}$ under the null hypothesis. 
- $H_0 \Rightarrow E(\Delta\mu_{obs} = 0)$, that is why the center of red curve is at 0.

Blue curve: the non-central distribution, i.e.,  the distribution of $\Delta \mu_{obs}$ under a specific alternative hypothesis case $E(\Delta \mu) = D_{ef}$

- Detailed explanation:
  - Blue curve is often viewed as the distribution of $\Delta \mu_{obs}$ under the alternative hypothesis. 
  - But, different from null hypothesis: $H_0: p_{EG} = p_{CG} \Rightarrow \Delta \mu = 0$ case, if we want to quantitatively express $\Delta \mu$ from the alternative hypothesis $H_1: p_{EG} \not = p_{CG}$
  - So, to quantitatively represent $H_1$ we need to answer "How much difference can be viewed as $\not =$". 
  - $d_{mde} := p_2-p_1$ is the threshold we manually/subjectively set for the question, i.e. it is our expectation on the $\Delta \mu$.
- $D_{ef} := \frac{p_2-p_1}{s_{pooled}} = \frac{d_{mde}}{s_{pooled}}$
  - $s_{pooled}$ is the pooled SD for the control and experimental group.
  - It is the difference between the centers of the blue curve and the red curve.


#### DD-SSC.2.2. Mechanism for sample size calculation

<div  align="center"><img src=https://splitmetrics.com/wp-content/uploads/2018/04/8-1-1.png style = "zoom:60%"></div>

- Point 1: $N \uparrow \Rightarrow $ width of the two curves will shrinkage
  - $\because$ the two curves is the distribution about mean 
  - $\therefore$ the "standard deviation" of the two curve is the "standard deviation of mean (SE)" not "standard deviation of population (SD)". 
  - $\because$ $SE \propto \frac{1}{\sqrt{N}}$,
  - $\therefore$ when $N \uparrow$,  width of the curve will shrinkage 
- Point 2: The centers of the two curves (almost) not change with $N$
  - The distance between the centers of the two curves is $D_{ef} = \frac{d_{mde}}{s_{pooled} }$
  - $\because$
    - $d_{mde}$ is manually set by us, not change with $N$
    - $s_{pooled}$ is a kind of SD, also not change with $N$ (t is the pooled value of the population standard deviation (SD) of experimental and control group.)
  - $\therefore$ $D_{ef}$ not change with $N$
- Brainstorming: Graphical relations between $N, d_{mde}, \alpha, \beta$
  - Give an $N$ value, the shape (width) of the two curves is fixed.
  - Give a $d_{mde}$ value, the distance between two curves is fixed.
  - Then, from the red curve and $\alpha$ value, we can calculate the position of $z$
  - Bring $z$ to the blue curve, we can get the value of $\beta$
- An intuition algorithm:
  - From the brainstorming above, we can regard the $\beta$ as a function of $N, d_{mde}, \alpha$ as $\beta = f_{\beta}(N, d_{mde}, \alpha)$
  - When the $d_{mde}, \alpha$ is given (as $d_{mde0}, \alpha_0$), $\beta$ is a function solely of $N$ as $\beta = f_{(\beta|d_{mde0},\alpha_0)}(N)$
  - So, we can scan over different $N$ values, find the required size value $N_{req}$ that make $f_{(\beta|d_{mde0},\alpha_0)}(N_{req}) = \beta_0$ i.e. our input $\beta$ value.
  - Then, $N_{req}$ is the solution.
- Or, more directly, from the $\beta = f_{\beta}(N, d_{mde}, \alpha)$, we can know $N$ is a function of $\alpha, \beta, d_{mde}$
  - I.e. $N = f_N(\alpha, \beta, d_{mde})$



### Ref: 
- [TowardsDataScience](https://towardsdatascience.com/required-sample-size-for-a-b-testing-6f6608dd330a): equation of sample size.
- [Blog](https://jeffshow.com/caculate-abtest-required-sample-size.html): equation of sample size.
- [StakeExchange](https://stats.stackexchange.com/questions/392979/ab-test-sample-size-calculation-by-hand): equation of sample size.
- [Splitmetrics](https://splitmetrics.com/blog/mobile-a-b-testing-sample-size/): graph illustration of relation between $p_1, p_2, \alpha, \beta$ and the width of the distribution.

### Remaining Qs (TBD):

- How $SE=\frac{SD}{\sqrt{N}}\propto\frac{1}{\sqrt{N}}$ is get?
- What is central limit theory?