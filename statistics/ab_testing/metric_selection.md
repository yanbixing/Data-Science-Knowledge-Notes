# Metric

## 1. Typical metrics in AB testing:

- Sum and counts
- Statistics of distribution (e.g. mean, median, percentiles)
- Probability and rates (e.g. click through probability/rates)
- Ratios: division between any two numbers.

## 2. Sensitivity and robustness

### Definition

- Sensitivity: Whether the metric is *sensitive* to the change of the data.
  - English: if a part of the data changes, how much the metric is affected. 
  - The higher the metric value changes, the higher sensitivity.
- Robustness: Whether the metric is robust (resistant) to the change of *outliers*.
  - English: if a part of the data become outliers, i.e. have extreme abnormal values, how much the metric is affected.
  - The lower the metric value changes, the higher robustness.

### Trade-off between sensitivity and robustness

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

### How to measure/observe the sensitivity and robustness?
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
  
