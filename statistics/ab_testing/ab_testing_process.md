# AB Testing

## AB Testing

Ref: [TowardsDataScience-blog](https://towardsdatascience.com/a-summary-of-udacity-a-b-testing-course-9ecc32dedbb1), [1p3a-bbs](https://www.1point3acres.com/bbs/thread-470867-1-1.html)

### Definition
A/B testing: A way to compare two versions of a single variable. [Wiki-AB_testing](https://en.wikipedia.org/wiki/A/B_testing)
    - English: typically, we test the response of the population to two versions of a product, the two versions **only have 1 difference**, all other conditions/design are same. Our purpose is to determine which one is better.

### Experimental design

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



### Result analysis




## Background Knowledges

