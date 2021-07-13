# Imbalanced data

- Useful Refs:
  - [MachineLearningMastery-Blog](https://machinelearningmastery.com/tactics-to-combat-imbalanced-classes-in-your-machine-learning-dataset/)
  - [Github-Blog](https://shiring.github.io/machine_learning/2017/04/02/unbalanced)
  - [TowardsDataScience-Blog](https://towardsdatascience.com/methods-for-dealing-with-imbalanced-data-5b761be45a18)

- How to understand imbalanced data mathematically? (Personal understanding)
  - From balanced binary-label data, we randomly remove points from one label, making it sparse.


## 1. Metrics

The imbalance usually make the common evaluation metric, like accuracy or precision, is not a good/fair metric anymore.

Thus, using proper metric may alleviate the effect on data imbalance on evaluation part.

### 1.1. Mechanism

- For most common metrics, like, $precision = \frac{TP}{PredictP}=\frac{TP}{TP+FP}$, 
  - the PredictedP not only varies from model-to-model, but also capped by the number of ground true P & N
  - Thus, for different test sets with different ground true P/N ratio, the metric has different values.
  - This means, the metric value not only depends on the real performance of the model, but also also affected by data imbalance. 
  - In other words: such metric not only reflect the model performance but also reflect the imbalance of the dataset. Thus it is not a fair metric to compare different models.

- However, for base-rate-invariant metric, like, $recall (TPR) = \frac{TP}{Ground P} = \frac{TP}{TP+FN}$
  - The GroundP not varies from model-to-model.
  - For different model the denominator is always same, thus is fair metric to compare different models.
  - This means, the base-rate-invariant metric only reflect the performance of the model, independent of the ground true P/N ratio of the test set.
  - Thus, it is a fair metric for doing model comparison across different datasets.
    - It should not change when dataset have different P/N ratio (base-rate)
    - That is why it is called "base-rate invariant"

- E.g: Duplicate positive sample c times in *test* set.
  - $GroundP' = c \times GroundP_0$, $TP'=c \times TP_0$, $FN'=c \times FN_0$
  - $GroundN' =  GroundN_0$, $TN'=TN_0$, $FP'=FP_0$
  - $precision' = \frac{TP'}{PredictedP'} = \frac{TP'}{TP'+FP'} = \frac{c \times TP_0}{c \times TP_0+FP_0} $
    $=\frac{TP_0}{ TP_0+FP_0/c} \geq \frac{TP_0}{ TP_0+FP_0} = precision_0$
  - $recall' = \frac{TP'}{GroundP'} = \frac{TP'}{TP'+FN'} = \frac{c \times TP_0}{c \times TP_0+c \times FN_0} = recall_0$

- **Note:** for a trained model, the "constant"/"inherent"/invariant metric is P(f(x)|y), i.e. metrics with ground true P or ground true N as denominator.

### 1.2. Solutions:


- Recall, FPR (Single base-rate-invariant metrics)
  - Ref: [evaluation_metrics.md](../model_training/evaluation_metrics.md)
  - Summary:
    - PredictedP varies with model.
    - GroundP invariant with model. (GroundP,GroundN is called base-rate.)
    - When dataset is imbalanced, for different models, the denominator of "base-rate invariant" metric is same/constant
    - Thus, "base-rate invariant" metric is more objective when compare the performance of different models when the dataset is imbalanced. 
  - Pro&Cons:
    - Pro (v.s. precision/accuracy): comparable for different models (constant denominator, more fair/objective)
    - Con (v.s. AUC): not comprehensive, e.g. recall has no FP TN info.

- AUC (ensemble of multiple base-rate-invariant metric)
  - ROC, TPR ($\frac{TP}{Ground P} = \frac{TP}{TP+FN} $) as y-axis vs FPR ($\frac{FP}{Ground N} = \frac{FP}{FP+TN}$) as x-axis
  - Pro: 
    - Has all $TP,FP,TN,FN$ info
      - v.s. Recall: recall has no FP, TN info.
      - v.s. F1 (precision-recall framework)
        - precision-recall has no TN info
        - $precision = \frac{TP}{PredictP}=\frac{TP}{TP+FP}$
        - $recall = \frac{TP}{Ground P} = \frac{TP}{TP+FN} = TPR$

- F1: (ensemble of multiple metrics)
  - Pro: Although F1 is not base-rate invariant, it is more comprehensive and intuitive than single precision or recall.
  - Con: Has no TN info.

## 2. Data

To solve the imbalance, of course one way is to balance the dataset.

- Modify the dataset
  - Oversampling, Downsampling
    - Give more weight to the sample during training, this is similar to over-/down-sampling
  - Synthetic (like SMOTE): generate new data according to existing data.
  
- Decompose to smaller equally distributed subcategory
  - This method only works when there are possible sub-population/sub-groups in the majority
  - Then, this method change the problem from binary-classification to multi-classification, i.e. (minority + majority) -> [label1+(label2+label3+...)]

## 3. Model

When the data is sparse, it may cause different types of problem:

- The amount/density of data in different class is different
- The boundary may be complex and hard to separate.

Different models/techniques can be used to solve these problems.

### 3.1. SVM

Some model may prefer majority, the boundary will be **affected by the density/amount of data** (e.g. Logistic regression), which make it unsuitable for imbalanced data. SVM wins in this point.

- SVM focus on the **boundary points**, affected less by data sparsity of the minority
- If imbalance (sparsity of minority) is so heavy that the boundary points is heavily affected, the SVM will also be affected, 
  - the boundary will be pushed towards minority, i.e., the model favor majority.
  - Solution: we can use class-weight SVM to increase the weight of the minority points, this is equivalent to oversampling.

### 3.2. Trees

When the data is sparse, the boundary may be very irregular, thus **hard to fit** (e.g. Logistic regression, Linear regression), require very complex/high-variance. Tree models win in this point

- Decision tree split space with if-else condition, thus **easier** to control **non-linearity/complexity/variance** than other classifiers, can **force the data to be separated**.
  - The complexity/variance of tree model can solely tuned by depth, while other models require trick feature engineering.
- Deeper understanding:
  - Decision tree rely on entropy. So, for bad separability data, imbalance (sparsity of minority data.) will still affect the model. 
  - However, it is because the variance of the model is high, so the data is usually separable for the model (**force** the data to be separated.), thus, the model are less affected.

### 3.3. Less complex model/regularization

Sometimes, the irregular boundary caused by data sparsity is not true. I.e. the model may overfit. When this happens, we will need less complex models or regularization techniques.



## 4. Strategy

We can always change our perspective. E.g. Treat the minority as anomaly data, thus, we can apply anomaly detection techniques.

### 4.1. Anomaly Detection

- Cluster-based: clusters (majority) vs. noise(minority) 
- Density-based: high-density (majority) vs. low-density (minority)
- Bayesian Network: high-probability (majority)  vs. low-probability (minority)
  - Introduction: [Bayesian Network](https://en.wikipedia.org/wiki/Bayesian_network) is a inference model/classifier. [Naive Bayes](https://stackoverflow.com/questions/12298150/what-is-the-difference-between-a-bayesian-network-and-a-naive-bayes-classifier) can be understand as a most simple case.
  - How it works: given a sample, the model will give its occurrence probability. Normal sample will have a high one while abnormal sample will have low one.
- Auto-encoder: low reconstruction error (majority) vs. high reconstruction error (minority)
  - Introduction: 
    - Auto-encoder is a model
      -  first project the input $x$ to lower dimension, as representation/embedding $v$, with encoder model
      -  then restore the representation back to the original dimension, as $x'$, with decoder model.
      -  like $x\underset{encoder}{\rightarrow}v\underset{decoder}{\rightarrow} x'$
    - Application: the $v$ is usually used as embedding of the input.
    - Evaluation: 
      - Reconstruct error: the difference between x and x', the lower, the better.
        - Because the better x can be restored, the more info is enclosed in $v$, the better the representation/encoding.
  - How it work on anomaly detection?
    - If the reconstruct error is very high, it is abnormal.
      - Because The auto-encoder should work better on frequent/normal samples
      - So the normal/frequent sample should have low reconstruct error.
  - Ref: [TowardsDataScience](https://towardsdatascience.com/how-to-use-machine-learning-for-anomaly-detection-and-condition-monitoring-6742f82900d7)

### 4.1. Change detection
  
  Usually used on time series (sequential) data, similar to anomaly detection, but focus on the different between neighbouring points.

  Ref: [Wiki-Change_detection](https://en.wikipedia.org/wiki/Change_detection)