# Summary: Model training

## Strategy


- Linear regression: 
  - Minimize RMSE, convex
  - Algorithm: GD, math


- Logistic regression: 
  - cross-entropy, convex
  - GD


- SVM: 
  - maximize margin (W or W + hinge loss)
  - Algorithm:
    - GD: Use Lagrange multiplier and Lagrange dual technique to convert constrained optimization problem to an unconstrained optimazation problem
    - Coordinate descent or subgradient descent


- Tree: greedy, maximize information gain
  - Split feature at different interval, calculate info gain.


- HMM: 
  - MLE
  - EM algorithm


- K-means:
  - EM algorithm

- Naive Bayes:
  - Algo: directly calculate 
    - prior prob for all classes: $p(C_k)$ for all $k$ values. 
    - posterior prob for feature values given classes: $p(X_i|C_k)$ for all $(i,k)$ pairs.

- KNN:
  - Find k-nearest neighbour to make decision, no training process.


- DNN:
  - regression: RMSE
  - classification(multi-label): cross-entropy
  - GD