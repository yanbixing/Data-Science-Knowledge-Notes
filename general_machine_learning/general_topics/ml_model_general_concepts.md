# ML Model Concepts (General)

## Differentiation

### Parametric vs Non-parametric Model

- Parametric: the model has <font color="#0000dd">fixed</font> number of parameters.
  - During training, value of parameters change, number of parameters will not change.
  - E.g.  Linear Regression, Logistic Regression, Linear SVM
- Non-parametric: the model <font color="#0000dd">complexity varies with data</font>. I.e. the number of parameter will change with data and hyperparameter.
  - E.g. 
    - Tree models: model is deep when data is complex; model is shallow when data is simple
    - KNN: dataset itself is the model (the dataset will be memorized).
    - RBF kernel SVM: number of kernel parameter varies with data.
- Ref: [Quora](https://www.quora.com/Do-Support-Vector-Machines-come-under-parametric-or-non-parametric-models-and-why)

### Generative vs Discriminative Model

Both generative/discriminative purpose is to get $p(y|x)$, generative get it indirectly via joint distribution (that is why it is called generative: ), discriminative directly fit the p(y|x).

- Generative: calculate $p(y|x)$ from the joint distribution $p(x,y)$ 
  - Idea: $p(y|x) = \frac{p(x,y)}{p(x)} = \frac{p(x|y)p(y)}{p(x)}$
  - Why call it generative: you can generate samples with the joint distribution.
  - Algorithm: 
    - <font color="#0000dd">Assume a function for $p(x|y)$</font> (distribution of feature given label.)
    - Fit this function with data. (Typically, with MLE)
    - Get joint distribution via $p(x,y) = p(x|y)p(y)$
    - Get the prediction function via $p(y|x) = \frac{p(x,y)}{p(x)}$
  - Examples:
    - Naive Bayes
    - Hidden Markov Models (HMM)
    - Bayesian Network (BN)
    - Markov random fields (MRF)
- Discriminative: directly fit the conditional probability $p(y|x)$, without know the joint distribution.
  - Why call it generative: the features space will be split. I.e. there is clear boundary in the feature space.
  - Algorithm:
    - <font color="#0000dd">Assume a function for $p(y|x)$</font>
    - Fit this function with data. (Typically, define a loss and reduce it.)
  - Examples:
    - SVM, Logistic Regression, KNN, Traditional Neural Networks, Conditional Random Field (CRF), Tree model.
- Ref: [Medium-blog](https://medium.com/@mlengineer/generative-and-discriminative-models-af5637a66a3)

