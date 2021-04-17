# Loss

## 1. L1 and L2 Loss

### 1.1. Form

- Loss function:
  - L1 loss: $l(y,h) = \sum^m_{i=1}(y_i - h(x_i))^2$
    - Also called MSE, Mean Square Error
  - L2 loss: $l(y,h) = \sum^m_{i=1}|y_i - h(x_i)|$
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
  - L2 aims to find the mean
  - L1 aims to find the median
  - Ref:?
  - E.g. In a sample set, we have two points with same $x$ value: $(x_1, 1)$ and (x_1, 0)$
    - L1 (MAE) has the same loss between $h(x_i)\in[0,1]$
    - L2 (MSE) has the smallest loss at $h(x_i) = 0.5$.
    - Ref: [Quora](https://www.quora.com/How-would-a-model-change-if-we-minimized-absolute-error-instead-of-squared-error-What-about-the-other-way-around)

#### 1.2.3. Application (when to choose what?)

- When you have many outliers:
L1 is better. Since it is insensitive to outlier.
- When you have few outliers (i.e. you think the 'outlier' points is not outlier and want to fit them.):
L2 is better, since it will consider more about outliers.
- Ref: [Quora](https://www.quora.com/How-would-a-model-change-if-we-minimized-absolute-error-instead-of-squared-error-What-about-the-other-way-around)


## 2. Deep Dive: difference bettween L1/L2 as loss v.s. regularization

### 2.1. L1 and L2 as loss:

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

### 2.2. L1 and L2 as regularization:

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