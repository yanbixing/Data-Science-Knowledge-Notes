## Hypothesis testing

Ref: [StatTrek](https://stattrek.com/hypothesis-test/hypothesis-testing.aspx)

## Two types of hypothesis

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

## Type 1 and type 2 error

- Definition:
  - Type 1 error: reject null hypothesis when it is true.
  - Type 2 error ($\beta$, sensitivity): accept null hypothesis when it is false.
    - null hypothesis is false = alternative hypothesis is true. 
- Probability representation:
  - Type 1 error: $\alpha = p(\text{reject } H_0| H_0 \text{ is true})$, also called **significant level**
    - English: if the null hypothesis is correct, the probability we observe a value more extreme than the experiment result.
  - Type 2 error:$\beta = p(\text{reject } H_0| H_1 \text{ is true})$
    - English: if the alternative hypothesis is correct, the probability we observe a value more extreme than the experiment result.
    <div  align="center"><img src=https://dp8v87cz8a7qa.cloudfront.net/45396/5bd20d03240611540492547.png style = "zoom:50%"></div>
- PN interpretation:
  - Ground true P := $H_1$ is true; Ground true N := $H_0$ is true
  - Label P := $H_1$ is accepted; Label N := $H_0$ is accepted
  - $\alpha = FPR = \frac{FP}{FP+TN}$
  - $\beta = TPR = \frac{TP}{TP+FN}$
  





## P-value

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