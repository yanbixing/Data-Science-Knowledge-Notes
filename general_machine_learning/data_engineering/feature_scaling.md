# Feature Scaling

<div  align="center"><img src=https://i.stack.imgur.com/kRbSk.png style = "zoom:100%"></div>

Ref: [StackExchange](https://stats.stackexchange.com/questions/244507/what-algorithms-need-feature-scaling-beside-from-svm), [DataSchool](http://www.dataschool.io/comparing-supervised-learning-algorithms/)

## Interview Q:

- 22, Should we rescale features before gradient descent? why?
  - Yes. In short, GD will converge more smoothly and quickly.
    - If not scaling, descend on small scale is quick, on large scale is slow, will oscillate inefficiently.
    <div  align="center"><img src=https://miro.medium.com/max/1160/1*iwY9jGjCq1REPr1ryXjjhA.png style = "zoom:80%"></div>
    
  - Ref:
    - [Blog](https://www.atoti.io/when-to-perform-a-feature-scaling/#:~:text=To%20ensure%20that%20the%20gradient,more%20quickly%20towards%20the%20minima.): description
    - [Medium](https://societyofai.medium.com/simplest-way-for-feature-scaling-in-gradient-descent-ae0aaa383039): figure
    - [Medium](https://rahul-saini.medium.com/feature-scaling-why-it-is-required-8a93df1af310): explain


## 0. Summary

- Typically, scaling is compulsory when there is an interaction between different features/weights:
  - there is regularization term
  - the model works based on distance
  - different weights share a same gradient descent speed?

## 1. Models need feature scaling

### 1.1. (Weight) Regularized models

After added a weight regularization terms, those regularized models require feature scaling. Or different features will have a different punishment force on weight.

### 1.2. SVM

Need feature scaling, because there is always a "regularization" term. ($d_m = \frac{2}{\|w\|}$ is the geometric margin, maximize $d_m$ $\Leftrightarrow$ minimize $\|w\|^2$)

So, the scale of the feature affect the margin size.


### 1.3. KNN & Most Clustering Algorithms

Need feature scaling, because it works on distance. The feature scale will affect the distance.

### 1.4. Neural Networks?

Need feature scaling, because features are usually combined with each other. (? I guess it is because we use similar step, standardization make the convergence faster.)

## 2. Models don's need feature scaling

### 2.1. Un-regularized Logistic Regression

Weight can vary to adapt to the scale of the feature.

### 2.2. Tree

Each split only works on a sole feature, and it only splits the data, no scale issue like multiplication is involved.

### 2.3. Naive Bayes

Similar to Tree model, each feature are considered independently, there is no interaction between different features, thus scaling will not affect the final result.



## When scaling/distribution matters? (To be organized.)

- For distance based models, scaling (and distribution) may affect the performance.
  - E.g. LinearRegression, LR, SVM, KNN, K-means
  - Ref:[TowardsDataScience](https://towardsdatascience.com/clearly-explained-what-why-and-how-of-feature-scaling-normalization-standardization-e9207042d971)
- For models have assumption on data distribution, data distribution may affect the model performance. E.g.
  - Naive bayes: usually assume the feature is normally distributed.
  - GMM: assume the cluster should follow normal distribution.
  - Linear regression: requires noise normally distributed.
    - Ref: [StackExchange](https://datascience.stackexchange.com/questions/20237/why-do-we-convert-skewed-data-into-a-normal-distribution), [StackExchange](https://datascience.stackexchange.com/questions/75652/which-supervised-machine-learning-algorithms-assume-normally-distributed-feature)
  - Ref: [StackExchange](https://datascience.stackexchange.com/questions/88965/feature-and-the-gaussian-distribution-classification)
- For non-distance based models or models have no assumption on data distribution, data distribution may be not important.
