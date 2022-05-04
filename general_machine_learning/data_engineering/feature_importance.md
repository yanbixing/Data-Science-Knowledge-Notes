# Feature importance

Feature importance can be obtained in a generally and model-specifically.

## General

- Check the "information gain" (mutual information)
  - Ref: [TowardsDataScience](https://towardsdatascience.com/how-to-find-feature-importances-for-blackbox-models-c418b694659d)
- Remove the features, observe its influence on model performance
- Check the "permutation importance"
  - Shuffle/permute the values in a column (feature), observe its influence on model performance.
  - Ref: [Sklearn-permutation_importance](
https://scikit-learn.org/stable/modules/generated/sklearn.inspection.permutation_importance.html#sklearn.inspection.permutation_importance), [Github: Sklearn-permutation_importance](https://github.com/scikit-learn/scikit-learn/blob/b194674c4/sklearn/inspection/_permutation_importance.py#L40)

## Model specific

- Logistic regression: weight
  - Standardize the feature
- Tree: number of splits