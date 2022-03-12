# Model_issues

## Overfitting

Ref: [EliteDataScience](https://elitedatascience.com/overfitting-in-machine-learning)

- Regularization
- Reduce model complexity
  - Less structural complexity: less layer, less tree depth
  - Less feature: Dimension reduction, feature elimination
- Data augmentation.
- Early stopping (less training epoch)
- Model ensemble (like tree - > bagging tree)
- Cross validation
  - Personal understanding: If for all k-folds, the training error > test error, it is safe to think the model is more complex than what data needs. (TBD)