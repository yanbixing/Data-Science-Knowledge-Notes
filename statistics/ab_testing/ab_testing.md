# AB Testing

## 1. AB Testing

- Ref: 
  - [Udacity-Course](https://classroom.udacity.com/courses/ud257): Google's ab-test course.
  - [TowardsDataScience-blog](https://towardsdatascience.com/a-summary-of-udacity-a-b-testing-course-9ecc32dedbb1), [1p3a-bbs](https://www.1point3acres.com/bbs/thread-470867-1-1.html): Summary of ab-testing.
  - [Medium-blog](https://productcoalition.com/start-here-statistics-for-a-b-testing-5f5c7e02ce1e): somebasic statistics for AB testing.
  - [TowardsDataScience-blog](https://towardsdatascience.com/what-i-learned-from-udacitys-course-on-a-b-testing-by-google-45f6d3297f42): Summary of ab-testing.
  <!-- - [Optimizesmart-blog](https://www.optimizesmart.com/understanding-ab-testing-statistics-to-get-real-lift-in-conversions/): some good questions/understandings on ab-testing. -->

### 1.1. Definitions

- A/B testing: A way to compare two versions of a single variable. [Wiki-AB_testing](https://en.wikipedia.org/wiki/A/B_testing)
  - English: typically, we test the response of the population to two versions of a product, the two versions **only have 1 difference**, all other conditions/design are same. Our purpose is to determine which one is better.


- Variant: version, variation, variable value.
  - E.g. There are exactly **2 variants**, i.e. A and B, in ab-testing.
  - [Variable](https://en.wikipedia.org/wiki/Variable_(mathematics)): a symbol that can change value.
    - There are only **1 variable** in ab-testing. (E.g. Color of button, design of the webpage, etc.)
    - "variants" vs "variable": e.g. $x$-axis is a variable, the values $x_1, x_2, \dots$ on $x$-axis are variants for the $x$ variable.


- Control vs Experimental Group: Except the condition we want to change, all other conditions are same.


### 1.2. Brainstorm: what need to be considered and why?

#### 1.2.1. Objective

**Q to consider**: What do you care about? I.e. What is your goal/objective, do you have existing metric/target? 

**Purpose**: Used to design evaluation metric.

**Technique**: we can split/break the process into different channel (parallel)/steps (sequential) to gain deeper insights on the process.


#### 1.2.2. Unit (of diversion & analysis)(TBD, not fully understand)

**Q to consider**: What is experiment **unit**.

- Unit of diversion: the experimental unit, the individual subject in your experiment .
  - E.g.: Per user ID, per cookies, per device, per IP. 
- Unit of analysis: the denominator of the evaluation metric.
  - Could be same with unit of diversion, could also be different from the unit of diversion.
  - Different unit of analysis will result in different variability of metric.
- Variability of the metric = the variance of the metric.
  - Analytical variability: the variance calculated under certain assumption, i.e. the variance we used to estimate the required sample (different metric have different distribution assumption.)
  - Empirical variability: the variance calculated with real experimental data.
    - E.g. CI determined from empirical variability: Plot the data and determine the CI from the plot. 
      <div  align="center"><img src=https://miro.medium.com/max/1400/1*6uaJHE1bHZJxb3oFlkFVMA.png style = "zoom:40%"></div>
  - If the empirical variability is different from analytical variability, it may be due to the unit of diversion is different from unit of analysis.
  

**Purpose**: Used to design evaluation metric.

<!-- **Why important**: The more close to the individual object in our goal, the less variability the unit is, the better the unit.

- "variability": the uncertainty of the unit to the true individual target we want to analysis.
   - E.g. We want to analyze individual customer; we only have user id, visit cookie, visit device, visit IP; A customer could have different id, cookie, device, ip. 
   - If according our intuition or research, usually a user usually have only 1 id but several cookie/device/ip. 
   - Then, user id would have the least variability, i.e., a user id can-represent/is-more-close-to an individual user. 哪里来的？好像不对。-->

**Technique**: 

- Different unit/metric-definition serve different purpose. 
Thus, we need to consider unit according to our purpose. E.g.
  - ${\text{Click-through-rate (CTR)}}= \frac{\text{Number of Clicks (on button)}}{\text{Number of Views (on page)}}$
    - Characterize whether the design of of the button can attract user to click. 
    - Usually used to compare buttons on a same page.
  - ${\text{Click-through-probability(CTP)}}= \frac{\text{Number of Customer who click}}{\text{Number of Customer who view}}$
    - Check if we can keep/attract our customers. 
    - Usually used to compare different webpages.

- Different unit may result in different **sample size.**
Thus, we need to consider unit for proper **duration**. 
  - Different unit will have different distribution, having different click-through-prop ($p_0$) and different variance, thus result in different variability of metric.
  - Different variability of metric result in different sample size requirement.
  - Thus, change unit of diversion to unit of analysis may reduce the required sample size.
    - However, such change may not make sense intuitively.
  

**Ref**: 
  - [Udacity-Lesson4-Lec17](https://classroom.udacity.com/courses/ud257/lessons/4001558669/concepts/39700990240923): variability and sample size
  - [Udacity-Lesson4-Lec17](https://classroom.udacity.com/courses/ud257/lessons/4001558669/concepts/39700990250923): change unit of diversion to unit of analysis may reduce required sample size.
  - [Udacity-Lesson3-Lec22](https://classroom.udacity.com/courses/ud257/lessons/4028708543/concepts/39546791350923): definition of variability.
  - [Udacity-Lesson40Lec8](https://classroom.udacity.com/courses/ud257/lessons/4001558669/concepts/39700990110923), [Blog](https://www.bellodata.com/archives/2332): definition of unit of analysis and diversion
  - [TowardsDataScience-blog](https://towardsdatascience.com/a-summary-of-udacity-a-b-testing-course-9ecc32dedbb1), [TowardsDataScience-blog](https://towardsdatascience.com/what-i-learned-from-udacitys-course-on-a-b-testing-by-google-45f6d3297f42): Summary of unit of analysis vs diversion
  - [Blog](http://napitupulu-jon.appspot.com/posts/variability-abtesting-udacity.html): variability of metric.
  - [Medium-blog](https://regularization.medium.com/udacity-a-b-testing-lesson-3-choosing-and-characterizing-metrics-edd261db8879): analytical vs empirical variability.
  - [Blog](https://qianqianshan.com/post/a-b-testing/),[Blog](https://arjan-hada.github.io/A/B-testing.html), [Blog](https://bangdasun.github.io/2019/08/08/56-ab-test-learning-notes-3/): The empirical variability of the metric may then not be consistent with the analytical variance if the unit of analysis is different with unit of diversion.



#### 1.2.3. Target population

**Q to consider**: who are our target population? 

**Purpose**: Avoid noise or inaccuracy. I.e. If our change aims to certain kinds of user, we don't want to expose it to other users. Or the result may be inaccurate.

**Technique**: 
- Filtration: Avoid unwanted data according to you goal.
  - E.g. we want to analyze buyer rather than non-buyer visitors, then, we would like to exclude the visitor who didn't buy the product.
- Subpopulation (study)
  - The contribution from different subpopulation may be different
  - Used to analyze the result (maybe we should separate different sub-populations to discuss/do-experiment.)

#### 1.2.4. Duration

**Q to consider**:  What is the **duration** of the experiment?

**Purpose:** Avoid noise or inaccuracy. I.e. Our assumption on individual sample is i.i.d. Thus:

 - If the duration is too long, the  <mark style="background-color:yellow;"><font color="#0000dd">data may shift</font></mark> during the experiment.
 - If the duration is too short, then the <mark style="background-color:yellow;"><font color="#0000dd">samples may not cover/represent the whole population</font></mark> (different kinds of users) comprehensively.
   - Then, we would have <mark style="background-color:yellow;"><font color="#0000dd">sample bias.</font></mark>
 - Typically the duration is 1-2 weeks, less than 4 weeks.
   - Ref: [Invespcro-blog](https://www.invespcro.com/blog/how-long-should-you-run-an-ab-test-for/)
   <!-- - Used/considered when setting parameters to calculate sample size. -->

**Technique:** Duration

- Duration is proportional to sample size, surely can be changed by changing required sample size.
  - By tuning parameter like $\alpha, \beta, d_{\min}$
  - By changing unit of diversion, as described above.
- Duration can also be changed by changing traffic.
  - **Traffic (exposure)**: the rate we expose experimental (B) version to users.
    - By setting different traffic, we can control the duration of the experiment.
    - Why we need limit traffic: experimental version may damage to user/website, we want to control its influence.
    - When need/needn't to limit traffic: 
      - Need to limit traffic: if new/different content/condition is added, like adding new external links on the page, using slower backend algorithm.
      - Needn't to limit traffic: the change is subtle, will not affect user's experience, like changing the button color, change ranking order, etc.
        - Ref: [Udacity-Lesson4-Lec22](https://classroom.udacity.com/courses/ud257/lessons/4001558669/concepts/40222885720923)
    - Ref: [Udacity-Lesson4-Lec20](https://classroom.udacity.com/courses/ud257/lessons/4001558669/concepts/39700990290923)


### 1.2. AB test process




     


 - Any potential influential factors/bugs, any possible discrepancy between designer and user
   - Can be verified by designing invariant metric 
   - Can used to analyze result.

#### 1.2.1. Design


Ref: [TowardsDataScience](https://towardsdatascience.com/a-summary-of-udacity-a-b-testing-course-9ecc32dedbb1)

1. Choose the evaluation metric and invariant metric.
   - Evaluation metric: your target metric or design one based on your goal.
   - Invariant metric is for sanity check, i.e. "what should be affected during the experiment"
   - When choosing metric, need to consider: 
     - Sensitivity and robustness. 
       - E.g. median or mean?
     - Duration:
       - Use the short term proxy for metrics need long term. 
2. Choose: 
   - Significant level ($\alpha$)
   - Sensitivity ($1-\beta$, also called statistical power)
   - Practical significant level $d_{\min}$ (also called minimum detectable effect)
3. Calculate the required sample size. (Details see [sample_size_calculation.md](./sample_size_calculation.md))
   - Required quantity: 
     - $p_0$: baseline conversion rate.
     - $d_{\min}$: minimum detectable effect (practical significance level).
     - $\alpha$: significance level
     - $1-\beta$: sensitivity (statistical power)
   - Need to consider about unit of diversion, population, duration.(?)
4. Collect data for experimental group and control group. (?)
   - Duration: how long the experiment will run
   - Exposure: what fraction of user you want to expose to experimental group.
   - Learning effect (see below.)
5. Analyze the result and draw the conclusion.
   - Sanity check (?)
   - Analysis:
     - Not significant: try on different platform or different day. Maybe there is a bug; examine if there are subgroups, may be different user have different response. (Simpson's paradox?)
     - Significant:
       - If we ran multiple variants (metrics? or test-variable?) at a same time. There is always a chance one metric is significant (what is the name of such effect?) Ref:[TowardsDataScience](https://towardsdatascience.com/a-summary-of-udacity-a-b-testing-course-9ecc32dedbb1)
         - Solution: 
           - only run 1 variant at each each time
           - bootstrap? Like random draw subgroup and check the whether the result is still statistically/practically significant?
   - Draw conclusion:
     - Analyze why? Is there any subgroup that is very sensitive?



## 2. A/B testing deep dives


### 1.0. Basics

- Each individual observation in A/B testing follows Bernoulli distribution: each individual test give independent binary outcome. See: [bernoulli_distribution.md](../probability_distributions/bernoulli_distribution.md).
  - We run A/B testing multiple rounds, the results of each round follow binominal distribution. See: [binominal_distribution.md](../probability_distributions/binominal_distribution.md). 
- The testing should be released to the whole/general population, not a particular subpopulation/cohort. I.e. the test people should represent the whole population.
- The conclusion applies to the specific tested change, cannot be generalized. I.e. we can determine if current change can improve the metric, we cannot generalize the conclusion to similar changes. I.e. if this change works, it doesn't means other changes can also work.
  - E.g. I want to test whether the items I recommend is complete. (Incorrect)
  - Explanation: With A/B testing, I can only know whether a specific item should be recommended, we cannot know if there are other items should be recommended.
- What we test is metric, i.e. we need specific metric to describe the target fact we want to test.


### 2.1. Situations cannot use A/B testing? 

Ref: [Udacity-Abtesting-Lesson1-Lecture3](https://classroom.udacity.com/courses/ud257/lessons/4018018619/concepts/40043986740923), [TowardsDataScience](https://towardsdatascience.com/a-summary-of-udacity-a-b-testing-course-9ecc32dedbb1)

<!-- 0. The duration is too long or too short.
   - Reason: 
     - If the duration is too long, <mark style="background-color:yellow;"><font color="#0000dd">the user property/behavior may change over time.</font></mark>
     - If the duration is too short, <mark style="background-color:yellow;"><font color="#0000dd">the responses we collect may not represent the whole population.</font></mark> -->
1. Changes that bring <mark style="background-color:yellow;"><font color="#0000dd">**new experience**</font></mark>:
   - I.e. For A/B test, the change should be insensible.
   - Large impact changes: brand name changing, logo changing
     - Learning effect: it may takes time for user to <mark style="background-color:yellow;"><font color="#0000dd">**adapt**</font></mark> to the changes. Specifically: 
       - Change aversion: people don't like new things will resist to use new services at the beginning. 
       - Novelty effect: people like new things will overact to try new services at the beginning. 
     - How to detect? Run on small group of users to see their response. The result is not stable at first, as the user may not get used to it. After the user get used to it, then the result will be stable.
     - Ref: [Udacity-Lesson4-Lec23](https://classroom.udacity.com/courses/ud257/lessons/4001558669/concepts/39700990310923)
   - New services: adding premium service.
     - <mark style="background-color:yellow;"><font color="#0000dd">**No baseline**</font></mark>: A/B testing is to test the metrics of version A with version B. You may have a baseline metric value.
2. Changes that have **long term effect**. 
   - I.e. For A/B test, we should be able to get user's response instantly. 
   - Reason: The responses we collect <mark style="background-color:yellow;"><font color="#0000dd">may not represent the whole population.</font></mark>
   - Eg. Two colors of house-rental promotion letter.
     - Explaination: People don't buy house often. The effect of our change can only be gather after a long time. I.e. It is <mark style="background-color:yellow;"><font color="#0000dd">**hard to comprehensively collect data**</font></mark> within the experimental period.
  <!-- - Long term effect. E.g. the user activity is infrequent, or it takes user long time to adapt
  - Cannot comprehensively collect data. -->

- Then, what should we do when we cannot use A/B testing?
   - Retrospective analysis: use historical results (like logs) to analysis the user behavior.
     - Retrospective vs prospective:
       - Prospective examine the **difference on result values** under different feature values.
       - Retrospective examine the **difference on feature values** under different result values. 
       - Ref: [theclassroom](https://www.theclassroom.com/ethical-issues-research-design-8084192.html), [Wiki-Retrospective_cohort_analysis](https://en.wikipedia.org/wiki/Retrospective_cohort_study)
   - User experience research: gather opinion from user.
     - Survey: gather opinion from a lot of users.
     - Focus group: gather opinion from smaller amount of representative users.
     - Ref: [ResearchGate-Paper](https://www.researchgate.net/profile/Arnold-Vermeeren/publication/221248254_User_experience_evaluation_methods_Current_state_and_development_needs/links/09e4150c5d003a175b000000/User-experience-evaluation-methods-Current-state-and-development-needs.pdf)
   - Heuristic/expert evaluation: ask expert to evaluate the new feature based on their experience. Ref: [YouTube](https://www.youtube.com/watch?v=TQ5jY1kKUNk), [Wiki](https://en.wikipedia.org/wiki/Heuristic_evaluation) 
   - Ref: [Udacity-Abtesting-Lesson1-Lecture6](https://classroom.udacity.com/courses/ud257/lessons/4018018619/concepts/40043986810923)


**Further Q:** Some example questions suitable (CAN) for ab-testing?

- Whether the latency can affect user behavior? (infrastructure A vs B)
- Which backend-infrastructure/algorithm/ranking/color is better?
  - Caution: we can know which specific version is better. But we generalize the conclusion, like "the faster algorithm, the better" is NOT correct.

### 2.2. Overpower and underpower
<!-- - Overpower: 
  - When $N_{gather}$ is larger than the designed $N_{required}$, the chance of type-II error will be smaller ($\beta_{gather}<\beta_{required}$), sensitivity (statistical power) will be larger $(1-\beta)_{gather}>(1-\beta)_{required}$, that why it calls overpower.
    - This is because $N\uparrow\beta\downarrow$
  - $N\uparrow \rightarrow SE\downarrow$, when $N\rightarrow\infty\Rightarrow \mathrm{SE}\rightarrow 0$, from the graph below, we know **everything will be statistically significant.**
    - I.e. the previous insignificant result (accept $H_0$, label=0, negative) becomes significant (reject $H_0$, label=1, positive)
    - This means we will make type-1 error.
  - In summary, overpowered test will decrease chance of type-2 error but increase chance of type-1 error.
  - Ref: [Optimizesmart](https://www.optimizesmart.com/understanding-ab-testing-statistics-to-get-real-lift-in-conversions/#a20)
  <div  align="center"><img src=./result_analysis_asset/statistically_and_practically_significant_2.jpeg style = "zoom:25%"></div>

- Underpower:
  - Contrary to overpower case. Underpowered test will decrease chance of type-1 error but increase chance of type-2 error.
  - Ref: [Optimizesmart](https://www.optimizesmart.com/understanding-ab-testing-statistics-to-get-real-lift-in-conversions/#a19) 感觉这个不太对，如果overpower +H_0 is true,  then obs value的distribution是要小于原H_0的CI，应该会less chance to have type-1 error.-->



- Overpower: the test is set with too large power $(1-\beta)$, larger than necessary power.
  - I.e. $\beta$ is set too small, need gather too much data. 
  - Con: It wastes the time and resources. For medical/bio tests involving animal or human, it is unethical. 
  - Note: overpower do **NO harm to result**. The result is **more precise**, it is just a matter of wasting.
  - Ref: [Quality-progress](http://rube.asq.org/quality-progress/2015/07/statistics-roundtable/the-significance-of-power.html), [StackExchange](https://stats.stackexchange.com/questions/9225/what-does-it-mean-for-a-study-to-be-over-powered), 
- Underpower: the test is set with too small power $(1-\beta)$, smaller than necessary power.
  - I.e. $\beta$ is set too large, gather too few sample. L
  - Con: larger chance to get type-2 error.
  - Note: Sample size has no influence on type-1 error prob. So both overpower and underpower is irrelavant to type-1 error.
  - Ref: [Quality-progress](http://rube.asq.org/quality-progress/2015/07/statistics-roundtable/the-significance-of-power.html), [StackeExchange](https://stats.stackexchange.com/questions/176384/do-underpowered-studies-have-increased-likelihood-of-false-positives)
- Note: the statements here [Optimizesmart-blog](https://www.optimizesmart.com/understanding-ab-testing-statistics-to-get-real-lift-in-conversions/) should be incorrect.
  - Overpower should means we the power is too large at designing stage, not meaning we collect more data than expected.
  - Even we collect more data than required, if $H_0$ is correct, the distribution of calculated $\Delta\mu^{obs}$ should be smaller/narrower than our design, thus, have lower chance to reject $H_0$ when it is true, i.e. lower type-1 error chance, not larger. 
    - Note: the criteria of CI should be determined by $N_{required}$ in original design, not the $N_{gathered}$. The webpage cited above seems make mistake on this point.
  - TBD Refs: [Medium-blog](https://medium.com/@ignazioziano/aint-no-such-thing-as-overpowered-studies-myths-in-the-lab-part-i-5deadc6d2e60), [StackExchange](https://stats.stackexchange.com/questions/176384/do-underpowered-studies-have-increased-likelihood-of-false-positives)


### 2.3. Multi-metric 

It is ok to test multiple metrics in a same round of experiment. There are some points need to be careful about.


#### 2.3.1. Sample size of the experiment

For different metrics, we usually set different significance level ($\alpha$), sensitivity ($1-\beta$) and minimum detectable effect $d_{\min}$. Thus, <font color="#0000dd">different metrics usually require different sample size.</font>

Use the <mark style="background-color:yellow;"><font color="#0000dd">**largest/maximum**</mark></font> required sample size in the experiment.

Because: overpower will not do no harm to the experiment. (No affect on the chance of type-1 error, decrease the chance of type-2 error.) But underpower will increase the chance of type-2 error.
- Note: also, the calculated "sample size" is actually <mark style="background-color:yellow;"><font color="#0000dd">"minimum required sample size"</mark></font>
  - Ref: [sample_size_calculator](https://www.calculator.net/sample-size-calculator.html), [Dummies-blog](https://www.dummies.com/education/math/statistics/how-to-determine-the-minimum-size-needed-for-a-statistical-sample/)


Ref: [Amazon-blog](https://rstudio-pubs-static.s3.amazonaws.com/347758_9da9522d18a8455fb810c48b11ff9824.html), [Github-blog](https://github.com/baumanab/udacity_ABTesting), [Sample-size-calculator](https://www.calculator.net/sample-size-calculator.html), [Dummies-blog](https://www.dummies.com/education/math/statistics/how-to-determine-the-minimum-size-needed-for-a-statistical-sample/)

#### 2.3.2 Problem: Increased false positive rate (TBD, solution)

If you test too many metrics, there will always be false positive. (I.e. False practically or statistically significant result.)

**Mechanism:** 

- Let's say the false positive rate for one metric is 5%.
- Then among 100 metrics, the chance of at least 1 false positive result is: $1-(1-\alpha)^{N} = 1-0.95^{100} = 0.994$
- I.e. The chance we have false positive result is increased when we do multi-metric testing.


**Solution:** (TBD)

- Bootstrap (?)
- Run experiment multiple times to see if the result is stable.
- There are some correction method: like Bonferoni correction.

Note: Also, there will always be false negative, the mechanism is same. Just because positive result is usually more important in application, we strengthen false positive here.


#### 2.3.3 Problem: Too long duration

Different metric settings may require different amount of data. It equivalently means different metric settings need different duration. 

However, if the duration difference is too large, it may violate the our original plan on the metric.

For example, for one experiment, we just want to gather data in one day, because user behavior will change day by day. For another experiment, we want to gather data for months.

Then, it is unsuitable to run the two experiment together. The user behavior change will affect the result of the first metric.

**Solution:** Drop the very different metric. Only do experiment on metrics require similar duration/sample-size. 
(I.e. Don't put together metrics that have very different requirements. Do experiment separately.)


### Other FAQs

- What to do if I don't trust the results? (? TBD)
  - Solution1: Run a separate experiment (Repeat).
  - Solution2: Use stricter criteria/parameter-setting.
    - If the result supports $H_0$. 
      - "You don't believe it" means you believe there is a type-1 error
      - Then you can set smaller $\alpha$ to reduce the chance of type-1 error.
      - Mechanism: increase the confidence interval by reducing $\alpha$
    - If the reuslt reject $H_0$
      - "You don't believe it" means you believe there is a type-2 error.
      - You can set a smaller $\beta$ to reduce the expected chance of type-2 error.
      - Mechanism: reduce the chance of type-2 error by increasing sample size.)
  - Solution3: Attack it with analysis, e.g.:
    - correlation doesn't means causality.

- What if I don't have/use control group? (TBD) 
  - E.g. Red color button (control) are forbidden now. We want to test effect of blue button (experimental).
  - Solution1: Use a version similar to the control group.
    - E.g. Run ab-testing on pink button vs blue button.
    - Cons: the "similar" is hard to define/measure. I.e. the alternative control group is still different from true control group.
    - Pros: But the user group are exactly same.
  - Solution2: retrospective, i.e. use the past data as the control.
    - E.g. Run experiment on blue button and compare it with red button's historical data.
    - Cons: the user group may "shift", i.e. user group at different time amy have different properties.
    - Pros: the past data are true control group.

- The purpose of A/A test?
  - Examine potential problems in the A/B-testing plan.
  - I.e. If the conclusions are different (or the metric values are significantly different) for the two same tests:
    - The metric may be not robust (stable) enough.
    - There may be problems in setting of parameters $\alpha, \beta, d_{min}$ and/or sample size $N$.
    - There may be other problems in the experiment process.

- How to split the samples to experimental/control group? (TBD)
  - Usually Equally split.
  
- Correlation vs causality (TBD)
  - The a/b test only give whether there is correlation between the result and condition.
  - However, there is correlation between the result and condition doesn't necessarily mean there is a causality between the result and condition.
  - E.g. Test result: User who response to website error churn less
    - Correlation conclusion: there is a relation between user's error-response-rate and churn-rate. 
      - This conclusion is acceptable, since it is reasonable assume the user like our product cares more about it.
    - Causality conclusion: website error make user to stay. (So we should increase the website error?)
      - This causality conclusion is absurd.
    - Note: customer churn = customer leave.

- Two phases of A/B testing: (TBD)
  - Exploration: use test samples to decide whether we need to make change.
  - Exploitation: launch the changes and observe the effect.


<!-- - Z-score: AB-testing use [Z-test](https://en.wikipedia.org/wiki/Z-test). AB test can use different types of statistical test. see: https://stats.stackexchange.com/questions/178854/a-b-tests-z-test-vs-t-test-vs-chi-square-vs-fisher-exact-test-->

## Related knowledges

- A/B/n vs. multi-variate:
  - A/B/n testing: **multiple versions** of an object are compared against each. The purpose is to determine which has the highest conversion rate. [Optimizely](https://www.optimizely.com/optimization-glossary/abn-testing/)
    - The additional "n" in the name means the *number of versions/variants/variable-values is larger than 2*.
    - Still there is a **single variable**.
    - AB test can be viewed as a special case (2 variants/versions) of the A/B/n test.
  - Multi-variate testing: **multiple variables** in an object are tested together. Each of the variable can have multiple variants. The test usually iterate over all the possible combinations of variants for different variables, each combination is an individual version.
    - Purpose: 
      - Same as AB or ABn: find the best version.
      - Explore the influence of different variable.
      - Explore interaction between different variables, like, if there are 1+1>2 effect.
    - variate = variable
    - A/B/n test can be viewed as a special case (single variable) of the multivariate test.
    - Ref: 
      - [YouTube](https://www.youtube.com/watch?v=fdrnAkLyMxo): ab vs abn vs multivariate.
      - [Optimizely](https://www.optimizely.com/optimization-glossary/abn-testing/): abn vs multivariate


## Notes


- Different views of relation between quantities.
  - $\beta$, $N$ relation:
    - Regarding $\beta, \alpha,  d_{min}, p_{EG}$ as input variable, N can be viewed as a function of these variable, i.e. $N = f_N(\beta, \alpha , d_{min}, p_{EG})$
    - Alternatively, regarding $\beta,\alpha,  d_{min}, p_{EG}$ as input variable, we can also regard $\beta$ as a function of these variable, i.e. $\beta = f_\beta(N, \alpha , d_{min}, p_{EG})$
  - $\alpha$, $\mathrm{CI}$ relation
    - Also, $\mathrm{CI}$ is directly related with $\alpha$, i.e., $CI$ can be determined by $\alpha$, $\alpha$ can also be determined by setting $\mathrm{CI}$
  - type-1/2 error and $\alpha$/$\beta$ relation:
    - $\alpha$ and $\beta$ can be a variable to be set for sample size calculation.
    - $\alpha$ and $\beta$ can also be viewed as the chance of type-1/type-2 error given different sample size.
- "Version"/test-object for A/B testing:
  - The version (test target/object) for A/B testing **can** be "large" or "small", both are ok, e.g.
    - if the version indicates the whole webpage, then, we are comparing two different versions of page. E.g. [YouTube](https://www.youtube.com/watch?v=fdrnAkLyMxo)
    - if the version indicates the color of button on a webpage, then, we are comparing two version of the button on the webpage. E.g. [TowardsDataScience-blog](https://towardsdatascience.com/a-summary-of-udacity-a-b-testing-course-9ecc32dedbb1)
  - It is just, typically, the version is small, like we usually test elements one by one, rather than the whole page. If we force to test the whole page, mathematically, it is also no problem.

