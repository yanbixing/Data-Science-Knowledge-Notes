# Case Interview

## 1. Qualitative understanding on the task

### 1.1. Ask clarifying questions: understand the goal

Note:
- [1p3a-FB-Case-210912](https://www.1point3acres.com/bbs/thread-796776-1-1.html): short term goal and long term goal.

#### Type1. Decision-making/impact-estimation: understand which affected aspects to explore.


Typical question type：

- How to evaluate a plan/proposal.
- How to evaluate the impact of a product/incidence.

Clarifying questions:

- Ask what a aspects will be affected.
- Ask what is the motivation of such decision/plan/proposal.

Based on the answer

- If have clear goal/affected-aspects:
  - Proceed to Problem breaking down stage.
- If no clear goal/affected-aspects:
  - Explore possible affected aspects
    - E.g. 5C, Micro-Macro, internal-external, etc.
  - Then proceed to problem breaking down stage.


<!-- - If NO clear goal (impact of a plan/proposal, before-dev)
  - Focus on exploring different aspects of **business goals** (5C)
- Have clear goal (impact of a product, whether to launch, after-dev): 
  - Explore **different aspects** (scale vs. degree, quality vs. quality, pro vs. con, different channels) of such goal

explore impact (of the decision/problem) on different aspects

- Given: a single change
  - Explore: **different aspects/goals** and **corresponding metrics**
- Question type: -->
  


#### Type2. Problem (factor) diagnosis/identification: understand the procedures and subpopulations.


Typical question type：

- Factor exploration: explore possible factors
- Factor identification : confirm a factor is influencing
  - **Cohort analysis**: experiment on cohorts, isolate the influence of other factors

Clarifying Questions:

- Ask what is the backend-mechanism/sequential-steps
- Ask what is the channels/subpopulations

Based on the answer:

- If have clear procedures/channels:
  - Proceed to problem breaking down stage
- If no clear procedures/channels:
  - Explore according to possible features and experience.

  
#### Type3. Concept quantification: understand which descriptive aspects to explore.

Typical question type:

- How to describe/define XXX
- How to modeling/identify XXX

Clarifying questions：

- What aspect

- Give: a concept
  - Explore: different aspects can describe such concept
- Question type:
  - Definition: Explore diff aspects of target concept and corresponding metrics to describe the concept
  - Modeling: Explore one/multiple metrics to describe to concepts


### 1.2. Breaking down the problem -> quantify to metric


#### Type1. Decision-making/impact-estimation: breaking down affected aspects


1. **Impact =  Scale * Degree** (Most important)
   - Scale <-who and how many affected
     - target of population (user segmetation)
     - size of population -> number
     - Duration -> time
   - Degree (break down) <-> how much affected


2. Impact/Degree = Impact on aspect 1 + Impact on aspect2 + Impact on aspect3

   - 5C (Company, Customer, Competitor, Collaborator, Context) 
     - Company: revenue/cost/profit, growth rate
     - Customer: activity (time spent, engagement), number (growth rate/churn rate)
     - Collaborator/Competitor/Context: mkt share, growth rate
   - Micro - Macro (Specific product - overall products)

#### Type2: Problem (factor) diagnosis/identification: metric decomposition

1. Impact: Sudden or regular? Duration?
   - Sudden: may due to a specific event  (check competitor, overall mkt)
   - Regular: 
     - technical issue (decompose to check)
     - design issue (omit some periodical factors, add periodical/trend features)
     - seasonal components (holiday, overall mkt tendency)
2. Metric decomposition:
   - Metric = A/B : numerator/denominator
   - Metric = m1 * m2 * m3 * m4
   - Metric =  M1 + M2 + M3
   - Note:  factor identification: metric -> built cohorts

#### Type3. Concept qualification: descriptive aspects qualifications

1. Business goal $\to$ different aspects
   - Concept = concept on aspect1 + concept on aspect2 + concept on aspect3
     - 5C: Company, customer, competitor, collaborate, context
     - Micro-Macro: product itself, company/other products	
2. Aspect $\to$ metrics /features
   - Quality vs quality
   - Short-term vs long term

### 1.3. Possible problem in our metrics -> introduce more metrics (iterate the above analysis)

- Multi-metric: use those metrics together, get trade-off relation, set threshold, etc.
- Modify metric: One metrics composed from multi-metric
- Modeling: Score of quality learned by ML model, use the multi-metrics as features

## 2. Quantitative method

- Pre-dev: mkt/user-experience research
  - Survey
  - Focus group: gather opinion from smaller amount of representative users.
  - Expert evaluation (heuristic)
  - Historical data (user comments/feedbacks <- NLP tech)
- After-dev: experiment or modeling
  - Ab test
  - ML models


## 3. Note

- Rate: Clearly define numerator/denominator


## Ref:


- [1p3a](https://www.1point3acres.com/bbs/interview/facebook-data-science-489916.html) DS 完全准备贴
- [1p3a](https://www.1point3acres.com/bbs/forum.php?mod=viewthread&tid=330947&extra=page%3D1%26filter%3Dsortid%26sortid%3D311%26sortid%3D311): Production Interpretation 分类
- [1p3a](https://www.1point3acres.com/bbs/thread-462895-1-1.html): Production Interpretation Framework: 
- [1p3a](https://www.1point3acres.com/bbs/forum.php?mod=viewthread&tid=483072&extra=page%3D1%26filter%3Dsortid%26sortid%3D311%26sortid%3D311): DS面试准备经验
