# Data issues

## Data Leakage (TBD)

There is leakage when:

- A feature have certain/fixed/direct causal relation with label.
  - E.g. Visiting payment page (feature) $\leftrightarrow$ Make payment (label)


## Data Bias (TBD)

### Different types of data bias

- Selection bias: choice of data is not correct.
  - **Sampling bias**: non-random sample items from the pool. i.e. The sample NOT objectively represents the population (whole dataset).
  - Time interval bias: time interval/duration is not proper. E.g.:
    - Sampling frequency is too low to catch the peaks.
    - Sampling window is too short to represent the whole period.
  - Exposure bias: participants NOT give correct answer.
    - The patient is not willing to acknowledge their disease.
  - Attrition bias: lose participants. E.g.:
    - People cannot pass test give up.
    - People not interested in travel will not join in tourism survey.
  - Ref: [Wiki-Selection_bias](https://en.wikipedia.org/wiki/Selection_bias)
- Recall bias: people are easy to forget/exaggerate the past thing in their memory.
- Survivorship bias: entities that fails are not included in the dataset.
- Observer bias: research un-consciously project his expectation to the experiment.

Ref [data36](https://data36.com/statistical-bias-types-explained/), [TowardsDataScience](https://towardsdatascience.com/5-types-of-bias-how-to-eliminate-them-in-your-machine-learning-project-75959af9d3a0)

### How to correct data bias (TBD)

Ref: [TowardsDataScience](https://towardsdatascience.com/survey-d4f168791e57)