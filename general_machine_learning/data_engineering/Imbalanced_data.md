# Imbalanced data

- Useful Refs:
  - [MachineLearningMastery-Blog](https://machinelearningmastery.com/tactics-to-combat-imbalanced-classes-in-your-machine-learning-dataset/)
  - [Github-Blog](https://shiring.github.io/machine_learning/2017/04/02/unbalanced)
  - [TowardsDataScience-Blog](https://towardsdatascience.com/methods-for-dealing-with-imbalanced-data-5b761be45a18)

- How to understand imbalanced data mathematically? (Personal understanding)
  - From balanced binary-label data, we randomly remove points from one label, making it sparse.


## 1. Data

To solve the imbalance, of course one way is to balance the dataset.

- Modify the dataset
  - Oversampling, Downsampling, reweighing
    - Reweighing: Give more weight to the sample during training, this is similar to over-/down-sampling
  - Synthetic (like SMOTE): generate new data according to existing data.
  
- Decompose to smaller equally distributed subcategory
  - This method only works when there are possible sub-population/sub-groups in the majority
  - Then, this method change the problem from binary-classification to multi-classification, i.e. (minority + majority) -> [label1+(label2+label3+...)]

## 2. Metrics

The imbalance usually make the common evaluation metric, like accuracy or precision, is not a good/fair metric anymore.

Thus, using proper metric may alleviate the effect on data imbalance on evaluation part.

### 2.1. Mechanism

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

### 2.2. Solutions:


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

- Easy to **force the data to be separated**: Decision tree split space with if-else condition, thus **easier** to control **non-linearity/complexity/variance** than other classifiers, can **force the data to be separated**.
  - The complexity/variance of tree model can solely tuned by depth, while other models require trick feature engineering.
  - Deeper understanding:
    - Decision tree rely on entropy. So, for bad separability data, imbalance (sparsity of minority data.) will still affect the model. 
    - However, it is because the variance of the model is high, so the data is usually separable for the model (**force** the data to be separated.), thus, the model are less affected.

- **Less affected by misclassified 'far/deep' outliers**: Loss is entropy, only care about how mixed the sample is; do **NOT consider/affected-by sample's distance to boundary**, so "deep" outliers (misclassification far from boundary) contribute equally to "shallow" ones, will not significant affect the boundary. 
  - However, regression/SVM, deep outlier will drag the boundary towards the outlier, which usually bad.

### 3.3. Less complex model/regularization

Sometimes, the irregular boundary caused by data sparsity is not true. I.e. the model may overfit. When this happens, we will need less complex models or regularization techniques.



## 4. Strategy

We can always change our perspective. E.g. Treat the minority as anomaly data, thus, we can apply anomaly detection techniques.

### 4.1. Anomaly Detection

Ref: [Wiki-Anomaly_detection](https://en.wikipedia.org/wiki/Anomaly_detection)

- Cluster-based: clusters (majority) vs. noise(minority) 
- Density-based: high-density (majority) vs. low-density (minority)
  - KNN (density $\propto \frac{1}{\text{Area of k NN points}}$) or other method.
- Bayesian Network: high-probability (majority)  vs. low-probability (minority)
  - Introduction: [Bayesian Network](https://en.wikipedia.org/wiki/Bayesian_network) is a inference model/classifier. [Naive Bayes](https://stackoverflow.com/questions/12298150/what-is-the-difference-between-a-bayesian-network-and-a-naive-bayes-classifier) can be understand as a most simple case. (Naive Bay and other generative models can give the probability of (X,Y) pairs.)
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



## Deep Dive 1. Mechanism of SMOTE

Ref: [MLMastery](https://machinelearningmastery.com/smote-oversampling-for-imbalanced-classification/), [CSDN-blog](https://blog.csdn.net/Frank_LJiang/article/details/104427978)

- Select a random minority point.
- Find the k (typically k = 5) neighbours of the point and select one of the neighbours randomly.
- Selected a random point between the point and its neighbour as the synthetic example. (Linearly interpolation)


## Deep Dive 2. Model recalibration

- Ref:
  - [Educative-Ads-Training_data_collection](https://www.educative.io/courses/grokking-the-machine-learning-interview/xomqq0RVlmJ)
  - [Medium](https://nojeshua.medium.com/model-calibration-when-subsampling-negatives-548eb8790223): model calibration when subsampling negative
  - [TowardsDataScience](https://towardsdatascience.com/how-to-calibrate-undersampled-model-scores-8f3319c1ea5b): Visualization of density change when subsampling (?TBD)
  - [PersonalBlog](https://andrewpwheeler.com/2020/07/04/adjusting-predicted-probabilities-for-sampling/): Predicted prob vs actual prob after balance data (?TBD)

### DD2.1. Distribution change introduce by data balancing

For imbalance data, we often do try to upsampling the data so that model have fair performance on both negative and positive labels. 

However, when you down sampling the minority of upsampling the majority, the probability distribution is changed.

E.g.

Assume we have 1 LR model: $\hat{y} = h(x)$ and the LR model can "perfectly" fit the data. I.e. $\hat{y} := \hat{p} := h(x) = p^*$

With original model, the $f_0(x)=0.5$ decision boundary is at $x=3$
```
########################################################################

+       +       +       +       +       +      +     +   [ x = 4 ], p=1

+        -        +       -      +     -      +     -    [ x = 3 ], p=0.5 *

  -       -        +        -       +      -      -      [ x = 2 ], p~0.3

-     -      -    -      +    -     -     -    -    -    [ x = 1 ], p=0.1

  -    -   -   -   -   -    -    -    -   -   -   -   -  [ x = 0 ], p=0

-------------------------------------------------------- [ x =-1 ], p=0

########################################################################
```

After we upsampling positive sample 3 times, the $f_1(x)=0.5$ decision boundary is at $x=2$

```
########################################################################

+++     +++     +++      +++      +++     +++     +++    [ x = 4 ], p=1

+++      -      +++       -       +++      -      +++    [ x = 3 ], p =0.75

  -        -      +++       -      +++     -       -     [ x = 2 ], p~0.5 *

  -      -      -    -     +++    -     -     -     -    [ x = 1 ], P~0.3

-    -   -   -   -   -    -    -    -   -   -    -    -  [ x = 0 ], p=0

-------------------------------------------------------- [ x =-1 ], p=0
		
########################################################################
```

### DD2.2. Problem for probability interpretation


Then $f_0$ and $f_1$ will give different score, e.g.:

- $f_0(x=2.5) < 0.5 $
- $f_1(x=2.5) > 0.5 $

If you assign same threshold to both model, then, the prediction will be different.

#### DD2.1.1 Score interpretation

If you simply understand the model output as "score", i.e. use the score interpretation to process the model output. That is fine, because you can easily tune the threshold to make the model same. 

E.g. If you assign 0.75 threshold to $f_1$. Then both $f_0$ and $f_1$ give same prediction.

- $f_0(x=2.5) < 0.5  \Rightarrow 0$
- $f_1(x=2.5) = 0.5 < 0.75 \Rightarrow 0$

**Personal understanding: (TBD)** For model only have score interpretation, like SVM, the score after data balancing no need further processing.

#### DD2.1.2. Probability  interpretation

However, if you understand the output as probability (LR, NN, Naive Bayes, Tree, etc.), i.e. you need to use the probability interpretation. Such change from data balancing is problematic.

This is obvious, when the true probability $f_0(2)=\hat{\Pr}(x=1)=0.3$, the model after data balancing give $f_1(2)=\hat{\Pr}(x=1)=0.5$, inconsistent with the probability/behavior of true data.


### DD2.3. Model recalibration

Let's say you increase the size of positive sampling to k times.

$$p^* =\frac{p_1}{k(1-p_1)+p_1} $$

**Deduction:**

- $p^* = \frac{N_+}{N_++N_-} = \frac{1}{\frac{N_-}{N_+}+1}$
  - $\Rightarrow \frac{N_-}{N_+} = \frac{1}{p^*} - 1$
- $p_1 = \frac{kN_+}{kN_++N_-} = \frac{1}{\frac{1}{k}\frac{N_-}{N_+}+1}$
  - $\Rightarrow \frac{N_-}{N_+} = (\frac{1}{p_1} - 1)k$

Therefore:

$$\begin{aligned}
  \frac{1}{p^*} - 1 &= (\frac{1}{p_1} - 1)k\\
  \frac{1}{p^*} &= (\frac{1}{p_1} - 1)k + 1\\
  p^* &= \frac{1}{(\frac{1}{p_1} - 1)k + 1}\\
  p^* &= \frac{p_1}{(1 - p_1)k + p_1}\\
\end{aligned}$$
