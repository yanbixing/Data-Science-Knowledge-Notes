# Correlated features

## Naive Bayes

- Conclusion: the more features correlated, the more importance/over-emphasis of the feature.
  - The naive bayes use the product of features. E.g. If two features are correlated, the effect is equal to the square of one feature.
  - In the training process, the correlated features will be preferred to be optimized.
  - In the prediction process, the correlated features give power (e.g. $x \rightarrow x^2$) influence.



[Naive Bayes-Sec 3.1](../models/naive_bayes_model.md)

## Regularization L1 and L2 

For identical feature, 
- L1 $\Re$ spreads weight arbitrarily. 
- L2 $\Re$ spreads weight evenly.

For linear correlated feature:

- L1 $\Re$ assign all weight to the feature with largest scale, all others' weights are zero. 
- L2 $\Re$ prefers variable with larger scale - spread weights **proportional** to scale.<br>i.e. larger scale get larger weights, smaller scale feature get smaller (proportional) weights but **not zero**.
- Note: both L1/L2 $\Re$ give more weight to larger scale, this is because regularization always punish on weight, and weights on larger scale feature are smaller, thus get preferred.

[Regularization-Sec 1.2.2.](../model_training/regularization.md)