# Misc Topics

## Concept differentiation

### Gradient vs Derivative

- Gradient: A vector, whose value on each axis takes the corresponding. 
  $$\nabla f(x) = (\partial_x f, \partial_y f, \partial_z f)$$
  - The gradient is perpendicular to the contour line.
  - The direction of the steepest slope or grade.
- Derivative: A scaler. The change speed of the function along a a specific axis/direction. Also called directional derivative.
  $$\partial_\mu f = \frac{\Delta f}{ \Delta x} = \nabla f \cdot \vec{1}_\mu$$
- Ref: [Oregon State U](https://math.oregonstate.edu/home/programs/undergrad/CalculusQuestStudyGuides/vcalc/grad/grad.html), [Wiki-Gradient](https://en.wikipedia.org/wiki/Gradient), [Wiki-Derivative](https://en.wikipedia.org/wiki/Derivative)



### ML vs Data Mining:

- Machine Learning: algorithms that can automatically learn with data, without being explicitly programmed.
- Data Mining: extract knowledge from unstructured data. Use a lot of techniques including ML.


### Parametric vs Non-parametric

- Parametric: the model has <font color="#0000dd">fixed</font> number of parameters.
  - During training, value of parameters change, number of parameters will not change.
  - E.g.  Linear Regression, Logistic Regression, Linear SVM
- Non-parametric: the model <font color="#0000dd">complexity varies with data</font>. I.e. the number of parameter will change with data and hyperparameter.
  - E.g. 
    - Tree models: model is deep when data is complex; model is shallow when data is simple
    - KNN: dataset itself is the model (the dataset will be memorized).
    - RBF kernel SVM: number of kernel parameter varies with data.
- Ref: [Quora](https://www.quora.com/Do-Support-Vector-Machines-come-under-parametric-or-non-parametric-models-and-why)


### Supervised learning, unsupervised learning and reinforcement learning.
  

### Generative vs Discriminative

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
    - Hidden Markov Models
    - Bayesian Network
    - Markov random fields
- Discriminative: directly fit the conditional probability $p(y|x)$, without know the joint distribution.
  - Why call it generative: the features space will be split. I.e. there is clear boundary in the feature space.
  - Algorithm:
    - <font color="#0000dd">Assume a function for $p(y|x)$</font>
    - Fit this function with data. (Typically, define a loss and reduce it.)
  - Examples:
    - SVM, Logistic, KNN, Neural Networks, Conditional Random Field, Tree model.

### Overfitting vs Underfitting

Ref: [TowardsDataScience](https://towardsdatascience.com/overfitting-vs-underfitting-a-complete-example-d05dd7e19765), [AnalyticsIndiaMagazine](https://analyticsindiamag.com/tackling-underfitting-and-overfitting-problems-in-data-science/)

- Overfitting:
  - Definition: model describes random error/noise instead of underlying relationship. When a model is excessively complex.
  - Behavior: low training error but a high testing error
  - Solution:
    - More data (Data augmentation)
    - Cross Validation
    - Less complex model
    - Regularization
    - Model Ensemble (Ensemble learning)
      - Ensemble different models to reduce variance, like bagging trees.
- Underfitting:
  - Behavior: high training error (and a high testing error).
  - Solution:
    - More complex model
    - Increase training time


### Ensemble learning

- Definition: multiple models (classifiers, experts) are strategically generated and combined. This process is known as ensemble learning.
- Why ensemble learning is used?
  - Ensemble learning is used to improve the classification, prediction, function approximation etc of a model. Ref: [Quora](https://www.quora.com/How-do-ensemble-methods-work-and-why-are-they-superior-to-individual-models)
- When to use ensemble learning?
  - Classifiers that are more accurate and <font color="#0000dd">**independent**</font> from each other.
- Two paradigms of ensemble methods:
  - Sequential ensemble learning, e.g. boosting tree
  - Parallel ensemble learning, e.g. bagging tree (Random Forest)
- What is the general principle of an ensemble method
  - combine the predictions of several models built with a given learning algorithm in order to improve robustness over a single model. 
- What is bagging and boosting in ensemble method?
  - Bagging: a method in ensemble for improving unstable estimation or classification schemes.
  - Boosting: boosting method are used sequentially to reduce the bias of the combined model. 
  - Boosting and Bagging both can reduce errors by reducing the variance term.
