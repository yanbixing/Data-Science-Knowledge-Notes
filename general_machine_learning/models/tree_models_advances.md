# Tree models advances

## Ref

[TowardsDataScience](https://towardsdatascience.com/catboost-vs-light-gbm-vs-xgboost-5f93620723db): CatBoost,Light GBM, XGBoost
[TowardsDataScience](https://towardsdatascience.com/lightgbm-vs-xgboost-which-algorithm-win-the-race-1ff7dd4917d): Light GBM vs XGBoost

## 1. Xgboost

### 1.0 Xgboost vs Gradient Boost

XGBoost is actually an engineering implementation (and plus) of Gradient Boost idea.
<!-- XGB use more regularized model formalizatioin to control overfitting, which gives better performance<font color="#dd0000"> (???) </font> -->

- Base learner:
  - GB: typically only use tree classifier.
  - XGB: besides tree classifier, there are other types of linear classifiers (linear booster)
- Regularization:
  - GB: no explicit regularization term.
  - XGB: have explicit regularization term 
    - linear classifiers: L1, L2 regularization
    - tree: number of leave nodes, IG (score) at leave nodes
- Order of derivative:
  - GB: pseudo residue, i.e. first-order derivative
  - XGB: both first- and second- order of derivative
- Feature (Column) Subsampling and parallelization
  - GB: use full features and only one sequence
  - XGB: use subset of features, can parallelize multiple GB sequences, further reduce variance.
- Missing value:
  - GB: No default strategy for missing value.
  - XGB: Has strategy (can learn) to deal with missing value.

Ref:[ZhiHu Blog1](https://zhuanlan.zhihu.com/p/42740654), [ZhiHu Blog2](https://zhuanlan.zhihu.com/p/81368182), [CSDN Blog1](https://blog.csdn.net/jamexfx/article/details/93780308), [CSDN Blog2](https://blog.csdn.net/qq_28031525/article/details/70207918)

Terminology:

- CART: Classification and Regression Trees, i.e. Decision Tree.


### 1.1. Details

XGBoost use pre-sort-based algorithm [analyticsindiamag](https://analyticsindiamag.com/comparing-the-gradient-boosting-decision-tree-packages-xgboost-vs-lightgbm/)



## 2. Light GBM

### 2.1. GOSS: Gradient Based One Side Sampling

- Ref:
  - Light GBM: Gradient-based on-side sampling
  - XGBoost: pre-sorted algorithm & Histogram-based algorithm



"LightGBM uses histogram-based algorithms" [analyticsindiamag](https://analyticsindiamag.com/comparing-the-gradient-boosting-decision-tree-packages-xgboost-vs-lightgbm/)



### 2.2 XGBoost vs LightGBM

- [analyticsvidhya](https://www.analyticsvidhya.com/blog/2017/06/which-algorithm-takes-the-crown-light-gbm-vs-xgboost/)
  - XGBoost: level wise algo
    - <div  align="center"><img src=https://cdn.analyticsvidhya.com/wp-content/uploads/2017/06/11194110/leaf.png style = "zoom:60%"></div>
  - LightGBM: leave wise algo
    - <div  align="center"><img src=https://cdn.analyticsvidhya.com/wp-content/uploads/2017/06/11194227/depth.png style = "zoom:60%"></div>
    - much better accuracy
    - Faster -> very fast, hence the word ‘Light’.
  - Advantage of LightGBM
    - Faster training speed and higher efficiency: Light GBM use histogram based algorithm i.e it buckets continuous feature values into discrete bins which fasten the training procedure.
    - Lower memory usage: Replaces continuous values to discrete bins which result in lower memory usage.
    - Better accuracy than any other boosting algorithm: It produces much more complex trees by following leaf wise split approach rather than a level-wise approach which is the main factor in achieving higher accuracy. However, it can sometimes lead to overfitting which can be avoided by setting the max_depth parameter.
    - Compatibility with Large Datasets: It is capable of performing equally good with large datasets with a significant reduction in training time as compared to XGBOOST.
    - Parallel learning supported.
  - Important param: similar to normal boost trees (pls refer to the link above.)

- [analyticsindiamag](https://analyticsindiamag.com/comparing-the-gradient-boosting-decision-tree-packages-xgboost-vs-lightgbm/)
  - Advantages of XGBoost are mentioned below
    - XGBoost is also known as the regularised version of GBM. This framework includes built-in L1 and L2 regularisation which means it can prevent a model from overfitting.
    - Traditionally, XGBoost id slower than lightGBM but it achieves faster training via Histogram binning.
    - It supports user-defined objective functions with classification, regression and ranking problems.
    - This framework utilises multiple CPU cores and performs parallel processing.
    - It allows the user to run cross-validation at each iteration dung the boosting process.
  - Advantages of lightGBM are mentioned below
    - LightGBM uses histogram-based algorithms which results in faster training efficiency.
    - Due to the use of discrete bins, it results in less memory usage.
    - It supports parallel as well as GPU learning.
    - It deals with large scale data with better accuracy.
    - Supports various metrics and applications.



## Cat Boost


## Boost vs XGboost vs lihghtboost vs Catboost

- [machinelearningmastery](https://machinelearningmastery.com/gradient-boosting-with-scikit-learn-xgboost-lightgbm-and-catboost/)