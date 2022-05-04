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
  - GB: **NO regularization** on leaf score ("leaf weights") in the **loss function.**
    - Only have typically tree regularization methods: number of leave nodes, IG (score) at leave nodes
  - XGB: Introduce explicit L1&L2 regularization term on **leaf score ('leaf weights')** to the **loss function**
    - Also have typical regularization methods like 
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


## Draft: Un organized contents

		https://campus.datacamp.com/courses/extreme-gradient-boosting-with-xgboost/regression-with-xgboost?ex=7
			XGBoost Characteristics:
				Base Learning, not only tree, but also linear classifiers
					(GBM only tree)
				Has, regularization, on "leaf score"/"leaf nodes"
					(GBM no regularization)
		
		https://blog.csdn.net/weixin_35710893/article/details/112900925
			Xgboos regularization (正则化)
				1, 与正常树相同: max_depth, min_sample, min_gain
				2, L1 & L2 regularization on "leaf weights"
				3, Xgboost have column sampling, can use emsemble to prevent overfitting (parallel computing)
				
			Xgboost vs Light GBM
				1)xgBoosting采用预排序，在迭代之前，对结点的特征做预排序，遍历选择最优分割点，数据量大时，贪心法耗时，LightGBM方法采用histogram算法，占用的内存低，数据分割的复杂度更低；
				
				2)xgBoosting采用level-wise生成决策树，同时分裂同一层的叶子，从而进行多线程优化，不容易过拟合，但很多叶子节点的分裂增益较低，没必要进行跟进一步的分裂，这就带来了不必要的开销；LightGBM采用深度优化，leaf-wise生长策略，每次从当前叶子中选择增益最大的结点进行分裂，循环迭代，但会生长出更深的决策树，产生过拟合，因此引入了一个阈值进行限制，防止过拟合
				
			Leaf Weight:
			https://datascience.stackexchange.com/questions/73317/what-is-the-intuitive-meaning-of-leaf-weight-in-xgboost
				LWeight is a value calculated from first and secondary derivative.
				其他:
					https://discuss.xgboost.ai/t/what-does-leaf-weight-mean/1587
					https://stats.stackexchange.com/questions/265940/xgboost-weights-in-classifcation-tree-0-or-1
				
				https://towardsdatascience.com/de-mystifying-xgboost-part-i-f37c5e64ec8e
				
					https://towardsdatascience.com/de-mystifying-xgboost-part-ii-175252dcdbc5
					Leaf score
					
				
				https://stats.stackexchange.com/questions/265940/xgboost-weights-in-classifcation-tree-0-or-1
					这条里面回答关于 "leaf weight"的理解似乎更正确, 就是 leaf 的 output value
						I.e. sample 落入该leaf时, tree 给出的value, 由training时该leaf node中的sample来决定
					" 𝑦𝑖^yi^ is from your boosting trees and equal to the sum of the leaf weights."
				个人理解 
					对于一般classification tree, 当sample 落入一个leaf node时，根据majority vote, 我们会决定这个leaf node是 0 or 1, binary
						不同的，对于regression tree，我们输出的avg value (numerical)
					XGB  更像 regression tree 
						每个node输出是 numerical value, 叫 "weight"
						但这个weight 在 [0,1] 范围内
							似乎叫leaf score更合适?
					当一个 leaf weight =0 or =1 时，这个leaf 过于确定 sample 的 label 是不好的,  所以要punish leaf weight, i.e. 我们不想让final eval  rely on perticular leaf node too much.
					
					
				个人理解 - 联系GB (似乎不对)
					1, the final learning is still a weighted sum of different learner
					
					2, the weight is determined by loss 
						
					
					
					
					3, Loss is the first order derivative  (minize it.)
					
					So, we can see the weight is related to the first order direvative
					
					XGBoost weight not only determined by first order derivative, but also determined by second order derivative
