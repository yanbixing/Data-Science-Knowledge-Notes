# Linear Model

- What is linear model?
  - Outcome is determined by linear sum of **coefficients**.
    $$f(\vec{\theta}) = F( \vec{g}(\vec{x}) \cdot \vec{\theta} ) = F[\sum^N_{i=1)} g_i(\vec{x})\theta_i]$$
    - "Linear or non-linear" model is Regardless form of feature, it is ok if there is product combination or power of feature ( like $x^{(i)}x^{(j)}$ or $(x^{(i)})^2$ ).
    - Kernel SVM may (usually, unless linear kernal) be non-lieaner 
  - Ref: [Wiki-Linear_Model](https://en.wikipedia.org/wiki/Linear_model), [Ref: blog](https://sebastianraschka.com/faq/docs/logistic_regression_linear.html), [Ref: Wiki - Kernel_method](https://en.wikipedia.org/wiki/Kernel_method)


## Topics' summary for linear models

- L1&L2 Regularization: (see [../model_training/regularization.md](../model_training/regularization.md))
  - Intuitive Mechanism of regularization: 
    - Model variance $\approx\propto$ to output variance $\Delta y \downarrow$ for certain variance on input $\Delta x$
    - regularization $\uparrow$ $\Rightarrow$ weight $\downarrow$ $\Rightarrow$ $\Delta y \downarrow$ for same variance on input $\Delta x$ $\Rightarrow$ Model variance $\downarrow$
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