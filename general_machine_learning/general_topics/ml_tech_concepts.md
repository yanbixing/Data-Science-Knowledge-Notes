# ML Tech Concepts

## Introduction


#### Ensemble learning

- Definition: multiple models (classifiers, experts) are strategically generated and combined. This process is known as ensemble learning.
- Why ensemble learning is used?
  - Ensemble learning is used to improve the classification, prediction, function approximation etc of a model. Ref: [Quora](https://www.quora.com/How-do-ensemble-methods-work-and-why-are-they-superior-to-individual-models)
- When to use ensemble learning?
  - Classifiers that are more accurate and <font color="#0000dd">**independent**</font> from each other.
- Two paradigms of ensemble methods:
  - Sequential ensemble learning, e.g. boosting tree
  - Parallel ensemble learning, e.g. bagging tree (Random Forest)
- What is the general principle of an ensemble method
  - combine the predictions of several models built with a given learning algorithm in order to improve robustness over a single model. 
- What is bagging and boosting in ensemble method?
  - Bagging: a method in ensemble for improving unstable estimation or classification schemes.
  - Boosting: boosting method are used sequentially to reduce the bias of the combined model. 
  - Boosting and Bagging both can reduce errors by reducing the variance term.

#### Cross-validation for time series

Ref: [StackExchange]( https://stats.stackexchange.com/questions/14099/using-k-fold-cross-validation-for-time-series-model-selection)

  <div  align="center"><img src=https://i.stack.imgur.com/fXZ6k.png style = "zoom:50%"></div>

- Algorithm:
  - fold 1 : training [1], test [2]
  - fold 2 : training [1 2], test [3]
  - fold 3 : training [1 2 3], test [4]
  - fold 4 : training [1 2 3 4], test [5]

#### ML selections (TBD)

- Algorithm selection: [Model pro/cons](../models/summary_model_comparision.md)
  - Simpler model:
    - Eg: Decision Tree, Naive Bayes, KNN, Logistic Regression, Linear Regression, Basic SVM
  - Complex model:
    - Eg: Random forest, Kernel SVM, Neural Networks
- Feature selection: [feature_selection](../data_engineering/feature_selection.md)
- Hyper parameter selection: ???


## Differentiation


#### Regularization L1 vs. L2 

For identical feature, 
- L1 $\Re$ spreads weight arbitrarily. 
- L2 $\Re$ spreads weight evenly.

For linear correlated feature:

- L1 $\Re$ assign all weight to the feature with largest scale, all others' weights are zero. 
- L2 $\Re$ prefers variable with larger scale - spread weights **proportional** to scale.<br>i.e. larger scale get larger weights, smaller scale feature get smaller (proportional) weights but **not zero**.
- Note: both L1/L2 $\Re$ give more weight to larger scale, this is because regularization always punish on weight, and weights on larger scale feature are smaller, thus get preferred.

[Regularization-Sec 1.2.2.](../model_training/regularization.md)






