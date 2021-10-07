# Convex optimization

## 1. Basics

### 1.1. Convex optimization vs Non-convex optimization

- Convex optimization have one global minimum (local minimum is global minimum)
  - Can use gradient descent to get optimal point.
- Non-convex: have multiple local minimums
  - Need to use some randomization or momentum tech to avoid local minimum

Ref: [Wiki-Convex_optimization](https://en.wikipedia.org/wiki/Convex_optimization)


## 2. Convexity of different models

### 2.1. Linear Regression
  
- RMSE loss: convex
  - Ref: [Github_blog](https://jermwatt.github.io/machine_learning_refined/notes/5_Linear_regression/5_2_Least.html)


### 2.2. Logistic Regression

- Cross-entropy loss: convex
  - RMSE non-convex
  - Ref: [Coursera_ML](https://www.coursera.org/learn/machine-learning/lecture/1XG8G/cost-functio)


### 2.3. SVM

- SVM is convex

  <div  align="center"><img src=https://qph.fs.quoracdn.net/main-qimg-44688a484f93019cb9ca387f4f093089 style = "zoom:60%"></div>

Ref: [Quora](https://www.quora.com/Why-is-SVM-convex-optimization), [paper](http://www.lib.kobe-u.ac.jp/repository/90000231.pdf), [CMU-Slides](https://www.cs.cmu.edu/~epxing/Class/10701-08s/recitation/svm.pdf)

Note: prove method Hessian: [SVM-Hessian](https://www.svm-tutorial.com/2016/09/unconstrained-minimization/) 

### 2.4 K-means

- Should be Non-convex.
  - Why? Different random-initialization state give different result
  - How to prove? (TBD)
  - Some refs: [StackExchange](https://math.stackexchange.com/questions/463453/how-to-see-that-k-means-objective-is-non-convex), [Github-blog](https://wei2624.github.io/MachineLearning/usv_kmeans/)


### 2.5 Decision Trees

- Should be Non-convex
  - Why? Algorithm is greedy, usually cannot get global optimal.
    - Ref: [paper](https://arxiv.org/abs/1511.04056)
  - How to prove? (TBD)

### 2.6 Deep Neural Network

- Should be Non-convex
  - Why? A lot of method are proposed to overcome/avoid local minimum.
  - How to prove? (TBD)



## Appendix: Hessian matrix

Hessian matrix is usually used judge the convexity of the problem.

- Optimization problem to be convex, its hessian matrix must be positive definite.
- Ref: [ResearchGate](https://www.researchgate.net/post/Identify-if-optimization-problem-is-convex-or-non-convex), [Wiki-Hessian_matrix](https://en.wikipedia.org/wiki/Hessian_matrix)

A hessian matrix is like (Ref: [Wiki-Hessian_matrix](https://en.wikipedia.org/wiki/Hessian_matrix)):

$${\displaystyle \mathbf {H} _{f}={\begin{bmatrix}{\dfrac {\partial ^{2}f}{\partial x_{1}^{2}}}&{\dfrac {\partial ^{2}f}{\partial x_{1}\,\partial x_{2}}}&\cdots &{\dfrac {\partial ^{2}f}{\partial x_{1}\,\partial x_{n}}}\\[2.2ex]{\dfrac {\partial ^{2}f}{\partial x_{2}\,\partial x_{1}}}&{\dfrac {\partial ^{2}f}{\partial x_{2}^{2}}}&\cdots &{\dfrac {\partial ^{2}f}{\partial x_{2}\,\partial x_{n}}}\\[2.2ex]\vdots &\vdots &\ddots &\vdots \\[2.2ex]{\dfrac {\partial ^{2}f}{\partial x_{n}\,\partial x_{1}}}&{\dfrac {\partial ^{2}f}{\partial x_{n}\,\partial x_{2}}}&\cdots &{\dfrac {\partial ^{2}f}{\partial x_{n}^{2}}}\end{bmatrix}}}$$