# AB Testing

## 1. AB Testing

- Ref: 
  - [TowardsDataScience-blog](https://towardsdatascience.com/a-summary-of-udacity-a-b-testing-course-9ecc32dedbb1), [1p3a-bbs](https://www.1point3acres.com/bbs/thread-470867-1-1.html): Summary of ab-testing.
  - [Medium-blog](https://productcoalition.com/start-here-statistics-for-a-b-testing-5f5c7e02ce1e): somebasic statistics for AB testing.

### 1.1. Definition
A/B testing: A way to compare two versions of a single variable. [Wiki-AB_testing](https://en.wikipedia.org/wiki/A/B_testing)
    - English: typically, we test the response of the population to two versions of a product, the two versions **only have 1 difference**, all other conditions/design are same. Our purpose is to determine which one is better.

### 1.2. Process

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
   - Practical significant level $d_{\min}$ (also called minimum detectable effect)
3. Calculate the required sample size. (Details see [sample_size_calculation.md](./sample_size_calculation.md))
   - Required quantity: 
     - $p_0$: baseline conversion rate.
     - $d_{\min}$: minimum detectable effect (practical significant).
     - $\alpha$: significant level
     - $1-\beta$: sensitivity
   - Need to consider about unit of diversion, population, duration.(?)
4. collect data for experimental group and control group. (?)
   - Duration: how long the experiment will run
   - Exposure: what fraction of user you want to expose to experimental group.
   - Learning effect: the result is not stable at first, as the user may not get used to it. After the user get used to it, then the result will be stable. 
     - How to detect? Run on small group of users to see their response.
5. Analyze the result and draw the conclusion.
   - Sanity check (?)
   - Analysis:
     - Not significant: try on different platform or different day. Maybe there is a bug; examine if there are subgroups, may be different user have different response. (Simpson's paradox?)
     - Significant:
       - If we ran multiple variants (metrics? or test-variable?) at a same time. There is always a chance one metric is significant (what is the name of such effect?) Ref:[TowardsDataScience](https://towardsdatascience.com/a-summary-of-udacity-a-b-testing-course-9ecc32dedbb1)
         - Solution: 
           - only run 1 variant at each each time
           - bootstrap?
   - Draw conclusion:
     - Analyze why? Is there any subgroup that is very sensitive?



<!-- ### 1.3. Result analysis -->



<!-- 
## Background Knowledges
 -->
