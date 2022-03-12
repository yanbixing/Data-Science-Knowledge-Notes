# Loss

## 1. Classification Losses

### 1.1. Cross Entropy



Also see [entropy_related_statistics.md](../math_topics/entropy_related_statistics.md)

Evaluate two distributions on a same dataset. 

$$H(P,Q):=-\sum _{x\in {\mathcal {X}}}P(x)\,\log Q(x)$$ 

- Cross Entropy measures: evaluate the uncertainty of using an estimated distribution $Q$ to identify events with true distribution $P$.
  - i.e. Expected number of bits needed to identify a $\boldsymbol{x}$ from $\mathcal{X}$, with the estimated distribution $Q$ rather than ground true distribution $P$.
- $P$ and $Q$ defined on the same probability space $\mathcal{X}$
- Ref: [Wiki-Cross_Entropy](https://en.wikipedia.org/wiki/Cross_entropy)

Application Note:

- Usually used as the **loss for classification task** (works for both binary and **multi-label** classification). Ref: [StackExchange](https://stats.stackexchange.com/questions/207794/what-loss-function-for-multi-class-multi-label-classification-tasks-in-neural-n)

- In deep learning, to keep the result sum to 1, usually we use a use a **softmax layer** to normalize the output. Ref [StackExchange](https://stats.stackexchange.com/questions/207794/what-loss-function-for-multi-class-multi-label-classification-tasks-in-neural-n)

#### 1.1.1 Application: cross entropy is log-loss (Deduction TBD)


Cross entropy is also called log-loss, logistic-loss. Ref:[sklearn-log_loss](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.log_loss.html)


Negative log likelihood (NLL):
$$NLL = - \sum_i \log p(\hat{y}_i = y_i)$$

Cross entropy ($k$ denotes the label of k-th class):
$$CE = - \sum_k p(y_i = k)\log p(\hat{y}_i = y_i)$$

**Interpretation of minimize CE**: 

- Minimize CE = Minimize NLL = maximize the likelihood = MLE estimation, 
  
**prof**:

- $M:=$ number of samples in $\mathcal{S}$
- $m_k$ denote the number of samples with k-th class

  $$\begin{aligned}
    NLL &= - \sum_i \log p(\hat{y}_i = y_i)\\
    &= - \sum_k m_k \log p(\hat{y}_i = y_i) (?TBD) \\
    &= - M \sum_k p(y_i = k)\log p(\hat{y}_i = y_i)
    &= CE
  \end{aligned}$$

<!-- $$\begin{aligned}
      NLL(\mathcal{S}|\theta) & = - \sum^M_{i=1} \log[ p(\hat{y}_i = y_i|\boldsymbol{x}_i) ]\\
      & \underset{LR}{=} - \sum^M_{i=1} H_i(P_{(Y| S)}, P_{(\hat{y} | S)})\\
      & = M\cdot J(\theta)
  \end{aligned}$$ -->




## 2. Regression Losses: L2 Loss (MSE) and L1 Loss (MAE) 

### 1.1. Form

- Loss function:
  - L2 loss: $l(y,h) = \frac{1}{m} \sum^m_{i=1}(y_i - h(x_i))^2$
    - Also called MSE, Mean Square Error
  - L1 loss: $l(y,h) = \frac{1}{m} \sum^m_{i=1}|y_i - h(x_i)|$
    - Also called MAE, Mean Absolute Error

### 1.2. Property

### 1.2.1. Similarity: 

- both average the error from each sample (no weight)
- Range from 0 to $+\infty$
- Lower value means better fitting.

### 1.2.2. Difference:

- Effect from outlier:
  - L2 is heavily affected by outliers, since outliers have larger contribution to loss.
  - L1 is insensitive to outliers.
- Continuity
  - L2 is differentiable.
  - L1 is not.
- Prediction value:
  - L2 optimization give the mean value of output distribution.
    - Ref: [StackExchange](https://stats.stackexchange.com/questions/428169/can-someone-give-the-intuition-behind-mean-absolute-error-and-the-median)
    - One method to solve it is partial? (TBD) : 
      - $\sum_i\partial_x(\hat{y}-y_i)^2 = 0 \Rightarrow \hat{y} = \frac{1}{n}\sum_i y_i$
  - L1 optimization give the median value of output distribution.
    - Ref: [Wiki](https://en.wikipedia.org/wiki/Mean_absolute_error#Optimality_property), [StackOverflow](https://math.stackexchange.com/questions/113270/the-median-minimizes-the-sum-of-absolute-deviations-the-ell-1-norm/113336#113336)
  - E.g. In a sample set, we have two points with same $x$ value: $(x_1, 1)$ and $(x_1, 0)$
    - L1 (MAE) has the same loss between $h(x_i)\in[0,1]$
    - L2 (MSE) has the smallest loss at $h(x_i) = 0.5$.
    - Ref: [Quora](https://www.quora.com/How-would-a-model-change-if-we-minimized-absolute-error-instead-of-squared-error-What-about-the-other-way-around)

#### 1.2.3. Application (when to choose what?)

- When you have many outliers:
L1 is better. Since it is insensitive to outlier.
- When you have few outliers (i.e. you think the 'outlier' points is not outlier and want to fit them.):
L2 is better, since it will consider more about outliers.
- Ref: [Quora](https://www.quora.com/How-would-a-model-change-if-we-minimized-absolute-error-instead-of-squared-error-What-about-the-other-way-around)


### 1.3. Difference bettween L1/L2 as loss v.s. regularization

#### 1.3.1. L1 and L2 as loss:

- Keypoint: Loss is about punishment on outliers
  - L2: 
    - Larger punishment on outliers $\rightarrow$ affect more by outliers $\rightarrow$ less robust.
    - Take mean value $\rightarrow$ always one solution and thus stable.
  - L1:
    - Less punishment on outliers: $\rightarrow$ affect less by outliers $\rightarrow$ more robust
    - Take median value $\rightarrow$ may be multiple solutions and thus unstable.
  - Terminology: 
    - **robust** $\sim$ tolerance on outliers.
- Summary:
  <div  align="center"><img src=http://www.chioka.in/wp-content/uploads/2013/12/L1-vs-L2-properties-loss-function.png style = "zoom:100%"></div>

#### 1.3.2. L1 and L2 as regularization:

- Keypoint: Regularization is about punishment on weight values.
  - L2: 
    - Grad descend speed proportional to weight scale 
    $\rightarrow$ The optimized weight usually is samll but non-zero (non-sparse)(less sparse)
        - $\rightarrow$ higher training speed (more efficient)
    - For highly correlated features, L2 tends to distribute weights among different features, more stable production. (weight value proportional to feature scale)
      - $\rightarrow$ stabler output for dataset with correlated features.
  - L1: 
    - Constant grad descend speed regardless weight value 
    $\rightarrow$ The optimized weight has more zero (sparse)
      - $\rightarrow$ Good for noisy data (feature selection)
      - $\rightarrow$ Slower training speed than L2.
    - For highly correlated features, L1 tends to only assign weight to feature with largest scale, and all others tends to 0.
      - $\rightarrow$ dimension reduction for dataset with correlated features. 
  <div  align="center"><img src=http://www.chioka.in/wp-content/uploads/2013/12/L1-vs-L2-properties-regularization.png style = "zoom:100%"></div>

- Ref: [Blog](http://www.chioka.in/differences-between-l1-and-l2-as-loss-function-and-regularization/)


