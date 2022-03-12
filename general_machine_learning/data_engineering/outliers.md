# Outliers

## 0. Basics: Relation between loss and data location

### 0.1. Linear Regression

For linear regression, 
    $$\begin{aligned}
        f(x) &= \sum^n_{j=0} \beta^{(j)} x^{(j)}
        \\
        L_{OLS}(x)&:=\mathrm{Var_{res}} =  \frac{1}{m} \sum^m_{i=1} (y_i - f(x_i))^2
    \end{aligned}$$

- $L_{OLS}(x) \propto x^2$

### 0.2. Logistic Regression


For logistic regression, the cross-entropy loss:
    $$\begin{aligned}
        h_\theta(\boldsymbol{x}) &= \frac{1}{1+\exp(\boldsymbol{\theta}^T\boldsymbol{x})}\\
        J(\theta|\mathcal{S})&= - \frac{1}{M}\sum^M_{i=1}[ y_i \log (h_\theta(\boldsymbol{x}_i)) + (1-y_i) \log (1-h_\theta(\boldsymbol{x}_i)) ]
    \end{aligned}$$

- $x\rightarrow 0, h_\theta(\boldsymbol{x}) \rightarrow \text{const}, J(\theta|\mathcal{S})\rightarrow \text{const i.e. }  x^0$

- $x\rightarrow \infty, h_\theta(\boldsymbol{x}) \rightarrow \exp(\boldsymbol{\theta}^T\boldsymbol{x}), J(\theta|\mathcal{S})\rightarrow \boldsymbol{\theta}^T\boldsymbol{x} \propto x^1$


### 0.3. SVM

The hinge loss for SVM is like:

$$\begin{aligned}
    f(\boldsymbol{x}) &= \boldsymbol{w}^T\boldsymbol{x} - b \\
    l_H(x,y) &= \max(0,1-y f(x))
\end{aligned}$$

- $l_H \rightarrow x^1$

### 0.4. Tree

Tree use mutual info, it only cares about the "mixing" of different labels, not care about "the position of the data"/"distance between data points and boundary". So Tree is least affected by "deep" outliers.

$$\underset{X_i \in \mathcal{X_i}}{\mathbb{E}}[ IG(Y,X_i) ] = MI(Y;X_i) = E(Y) - E(Y|X_i)$$

- $IG \perp x$


## 1. Robustness of different models

### 1.1.  Logistic Regression vs Linear Regression



  <div  align="center"><img src=https://qph.fs.quoracdn.net/main-qimg-2ed9bd4ae4c45c6939572de3f5ac07de style = "zoom:60%"></div>

- The square loss is more sensitive to the change of point's location $x$.
- Therefore, the square loss (Linear regression) is more affected by outliers.


Ref: [Quora](https://www.quora.com/Why-is-logistic-regression-considered-robust-to-outliers-compared-to-a-least-square-method): cross-entropy-loss vs RMSE, i.e. Logistic regression vs linear regression.

### 1.2.  Logistic Regression vs SVM


  <div  align="center"><img src=https://cdn-images-1.medium.com/max/800/1*d1MLhL9veCEr5zKJj-vSWw.png style = "zoom:60%"></div>


For both logistic regression and SVM: 

- Although when $x \rightarrow \infty$, both loss function are $\propto x^1$
  - LR: $J(\theta|\mathcal{S})\rightarrow x^1$ 
  - SVM: $l_H \rightarrow x^1$
- But when $x \rightarrow 0$, LR is $\propto x^0$
  - LR: $J(\theta|\mathcal{S})\rightarrow x^0$
  - SVM: $l_H \rightarrow x^1$
- This means SVM have no acceleration while LR have positive acceleration, i.e. LR is up-bending, i.e **LR diverge faster that hinge loss**.
  - LR: $\frac{\partial^2 J}{\partial x^2} > 0$
  - SVM: $\frac{\partial^2 l_H}{\partial x^2} = 0$
- LR is more sensitive to the position change of the data.
- So, LR is more affected by "deep" outliers.


**Note**: However, SVM is also not robust to outlier is when the ourlier near to margin, especially for hard margin SVM. Ref: [lsd-pdf](https://lstat.kuleuven.be/research/lsd/lsd2008/presentationslsd3/Debruyne.pdf), [Personal-blog](https://www.mihaileric.com/posts/support-vector-machines/)
      <div  align="center"><img src=https://www.mihaileric.com/static/linearSeparator1-af5113984a755c11f57170b4893cb5cc-a3db2.png style = "zoom:20%"><img src=https://www.mihaileric.com/static/svmOutlierDiagram-dbef9a2e874968a10f9a8c1b82338375-a3db2.png style = "zoom:20%"></div>



Ref and other reasons:
- [TowardsDataScience](https://towardsdatascience.com/support-vector-machines-intuitive-understanding-part-1-3fb049df4ba1), [GDCoder](https://gdcoder.com/support-vector-machine-vs-logistic-regression/), [Github-blog](https://nancyyanyu.github.io/posts/c8f688ba/): LR diverge faster than SVM.
- [Github-blog](https://cpatdowling.github.io/notebooks/classification_2): SVM can use non-linear kernels.
- [StackExchange](https://stats.stackexchange.com/questions/443351/svm-vs-logistic-regression): experience, no reason.
- [GeekForGeek](https://www.geeksforgeeks.org/differentiate-between-support-vector-machine-and-logistic-regression/): SVM don't care about correctly predicted outliers, since SVM only consider points on the boundary. While LR want to maximize to probability of all points. (Same reason that SVM suffer less from imbalance than LR, as it only consider points in-the-margin/near-the-boundary)




### 1.2.  Tree vs. LR/SVM/Linear-Regression

Tree use mutual info, it only cares about the "mixing" of different labels, NOT affected by "the position of the data"/"distance between data points and boundary". So Tree is least affected by "deep" outliers.


## 2. Outlier detection

Approaches are similar to anomaly detection. See: [imbalanced_data.md](./imbalanced_data.md)

- Clustering-based: noise/far-distance.
- Density-based: low density.
- Probability-based: low prob.
- Autoencoder: high reconstruction error.

Ref: 
- [Sklearn](https://scikit-learn.org/stable/modules/outlier_detection.html):"Outlier detection and novelty detection are both used for anomaly detection"

- [TowardsDataScience](https://towardsdatascience.com/a-brief-overview-of-outlier-detection-techniques-1e0b2c19e561)

  - Z-score
  - Clustering
  - Isolation Forest (TBD)

