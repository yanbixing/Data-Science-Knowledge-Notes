# Training issues

#### Overfitting and Underfitting

Ref: [EliteDataScience](https://elitedatascience.com/overfitting-in-machine-learning),[TowardsDataScience](https://towardsdatascience.com/overfitting-vs-underfitting-a-complete-example-d05dd7e19765), [AnalyticsIndiaMagazine](https://analyticsindiamag.com/tackling-underfitting-and-overfitting-problems-in-data-science/)

- Overfitting:
  - Definition: model describes random error/noise instead of underlying relationship. When a model is excessively complex.
  - Behavior: low training error but a high testing error
  - Solution:
    - More data (Data augmentation)
    - Cross Validation
    - Less complex model
    - Regularization
    - Model Ensemble (Ensemble learning)
      - Ensemble different models to reduce variance, like bagging trees.
    - Early stopping
    - Feature elimination (delete noise features.)
- Underfitting:
  - Behavior: high training error (and a high testing error).
  - Solution:
    - More complex model
    - Increase training time
