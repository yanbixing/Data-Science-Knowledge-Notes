# Linear Models

- What is "linear model"?
  - Outcome is determined by linear sum of **coefficients**.
    $$f(\vec{\theta}) = F( \boldsymbol{g}(\boldsymbol{x}) \cdot \boldsymbol{\theta} ) = F[\sum^N_{i=1)} \boldsymbol{g}_i(\boldsymbol{x})\theta_i]$$
    - "Linear or non-linear" model is Regardless form of feature, it is ok if there is product combination or power of feature ( like $x^{(i)}x^{(j)}$ or $(x^{(i)})^2$ ).
    - Kernel SVM may (usually, unless linear kernal) be non-lieaner 
  - Ref: [Ref: blog](https://sebastianraschka.com/faq/docs/logistic_regression_linear.html), [Wiki-Generalized_linear_model](https://en.wikipedia.org/wiki/Generalized_linear_model), [Wiki-Linear_Model](https://en.wikipedia.org/wiki/Linear_model), , [Ref: Wiki - Kernel_method](https://en.wikipedia.org/wiki/Kernel_method)


## Typical Linear Classifiers

Ref: [Wiki-Linear_classifier](https://en.wikipedia.org/wiki/Linear_classifier)

- Definition:
$$F(x) = f(\boldsymbol{\theta}^T\boldsymbol{x}) = f(\sum^{N+1}_{i=1} \boldsymbol{\theta}_i \boldsymbol{x}_i)$$

- $\Rightarrow$ Geometric property: partition the space with a linear **hyperplane**.
  - Proof: 
    - the separation interface should be $F(x) = f(\theta^T\boldsymbol{x}) = 0$
    - $\Rightarrow \theta^T\boldsymbol{x} = f^{-1}(0) := const$
    - $\theta^T\boldsymbol{x} = const$ should be a hyperplane.
      - Proof:
        - for two points $\boldsymbol{x}_{c1}, \boldsymbol{x}_{c2}$ on the hypersurface
        - we have $\theta^T(\boldsymbol{x}_{c2} - \boldsymbol{x}_{c1}) = \theta^T\boldsymbol{x}_{c2} - \theta^T\boldsymbol{x}_{c1} = 0$
          - i.e. vector $\theta$ is perpendicular to any vector formed from points on the hyper surface
        - The hypersurface should be a hyperplane.
  - Necessary but not sufficient.
    - A tree stump also split space with hyperplane, but it is not linear classifier. (Partition the space with boolean operation on feature.)
  - Typical linear classifiers:
    - Generative:
      - LDA
      - Naive Bayes
    - Discriminative:
      - Logistic Regression
      - SVM

#### Linear Regression

Let $x^{(N+1)} := 1$

$$f(x) = \theta^T\boldsymbol{x} = \sum^{N+1}_{i=1} \theta_i x_i$$

- Linear regression can be used as both regressor or classifier.
- Linear regression can also be used on categorical features, the input is exactly same with numerical feature, the value is binary, like $\boldsymbol{x} = [1,0,1,1,0,1]$ [Ref: Web doc](https://www.moresteam.com/whitepapers/download/dummy-variables.pdf)
- Training:
  - Target: minimize the cost function
    - Cost (loss) function: typically MSE, i.e. $l(\theta) = (y-f(\boldsymbol{x}))^2$
  - Method: gradient descent
- Regularization:
  - L1, L2: seeing above. Typically use L2.
  - Elastic Net: use L1 and L2 together.
- Assumption (when to use): linear relationship between features and label.
- How to handle non-linearity: add polynomial or interaction features.
- Refs [USF-Lecture](http://faculty.cas.usf.edu/mbrannick/regression/regma.htm), [Purdue-Lecture](https://www.stat.purdue.edu/~boli/stat512/lectures/topic3.pdf), [Wiki-Linear_regression](https://en.wikipedia.org/wiki/Linear_regression)

#### Logistic Regression
(see [./logistic_regression.md](./logistic_regression.md))
- Logistic regression is typically used as classifier.
- prediction (score) function: $h_\theta(x) = \frac{1}{1+\exp(\theta^T\boldsymbol{x})}$ 
- Loss function: 
  - cross entropy between label and corresponding probability
  - = negative log likelihood
  $$J(\boldsymbol{\theta}|\mathcal{S})= - \frac{1}{M}\sum^M_{i=1}[ y_i \log (h_\theta(\boldsymbol{x}_i)) + (1-y_i) \log (1-h_\theta(\boldsymbol{x}_i)) ]$$
- Optimization:
  - Minimize loss
  - =Maximum likelihood estimation (MLE)
- Method: gradient descent
  - Strictly convex, no need random weight initialization
- Comparison:
  - Compare to decision tree, LR is better on small data set [?why?]


#### SVM
(see [./support_vector_machine.md](./support_vector_machine.md))
- Why SVM?
  - The separation hyperplane is not unique.
  - Maximize geometric margin (hard-margin svm) provide unique solution
- Why to introduce hinge loss? (soft-margin svm)
  - Hard margin SVM have no solution when the data are not separable/mixed at the boundary
  - Soft SVM can tolerate misclassified/inconfident samples
- SVM vs LR
  - SVM
    - Stronger geometric interpretation
    - Easily extends to non-linear decision surface
    - Strong learning guarantees (???)
    - Focus on near-boundary points; very confident sample will not affect boundary, higher accuracy near boundary.
      - LR's decision plane will be affected by very confident samples.
    - More resistant to outliers than LR. 
      - LR is more sensitive to outliers, as the loss function diverge faster. [Ref: TowrdsDataScience](https://towardsdatascience.com/support-vector-machine-vs-logistic-regression-94cc2975433f)
  - LR
    - Stronger statistical representation 
      - output score can be interpreted as probability $p(\hat{y}=1|\boldsymbol{x})$
      - SVM CANNOT directly give probability [Ref: sklearn, svm](https://scikit-learn.org/stable/modules/svm.html)
    - Suitable for small data (why????)


<!-- ### Linear Regressor

- Prediction function: $f(\boldsymbol{x}) = \theta^T\boldsymbol{x}$
  - The $\theta^{(N+1)}$ is the intercept, corresponding $\boldsymbol{x}^{(N+1)}:= 1$
- Loss functions: MSE, 
- Training algorithm: gradient descent. -->



## L1&L2 Regularization: 
(see [../model_training/regularization.md](../model_training/regularization.md))
- Intuitive Mechanism of regularization: 
  - Model variance is: the variance of model output, i.e. $Var(\hat{y})$, given certain training data generation distribution $f$ (with noise $\varepsilon$) 
    - Model bias is: the deviation of the model average, i.e. $\mathbb{E}(\hat{y}) - f$
  - regularization $\uparrow$ $\Rightarrow$ weight scale $\downarrow$ $\Rightarrow$ $\Delta y \downarrow$ on same point $x$ $\Rightarrow$ Model variance $\downarrow$
- L1 vs. L2:
  - L2: 
    - Larger punish on larger weight (Shrinkage speed $\propto$ weight scale)
    - Effect: Keep the weights small but NOT 0.
    - For correlated feature: distribute weights to every feature (proportional to scale)
    - Choose L2 when a lot a feature are correlated with label. 
      - i.e.: Noise of data is small, most features are useful.
      - Then, weights will be distributed to different features, 
        - i.e. L2 will make the result stable, reduce variance.
  - L1: 
    - Equal shrinkage speed for all features
    - Effect: Tends to shrink small (noise/unrelated) features to zero.
    - Choose L1 when a lot a feature are correlated with label.
      - i.e.: Noise of data is large, a lot of features are useless, we want to do a dimension reduction
      - Then, weights of the noise features will be shrink to zero.
- Ref: [Github blog](https://github.com/ShuaiW/data-science-question-answer#l1-vs-l2-regularization)