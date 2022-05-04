

### Can boosting be used on linear classifiers?
  - No.
  - Boosting is a ensemble/sum of weak learner
  - sun of linear classifier is still linear, no help.

Proof:
    Linear model 定义: f(x) = g(WX)
    Ie. Func f is a function of linear combination of different features
    
    Sum of linear function is still linear function as:
    
    F(x) = A*a(WX) + Bb(WX) + Cc(WX) 
        = G(WX)
					
So, the decision boundary is still linear

Other refs: 
Can LR boosted like tree?
No, because sum of linear model is still linear model.
(Why)
https://stats.stackexchange.com/questions/329066/boosting-a-logistic-regression-model

Linear model (such as logistic regression) is not good for boosting. The reason is if you add two linear models together, the result is another linear model. On the other hand, adding two decision stumps or trees, will have a more complicated and interesting model (not a tree any more.)

https://stats.stackexchange.com/questions/230388/how-does-linear-base-learner-works-in-boosting-and-how-does-it-works-in-the-xgb
In most cases, we may not use linear learner as a base learner. The reason is simple: adding multiple linear models together will still be a linear model.
它证明的压根不对，狗屁不通, 对 linear classifier理解不对。

https://stats.stackexchange.com/questions/186966/gradient-boosting-for-linear-regression-why-does-it-not-work

https://stats.stackexchange.com/questions/93569/why-is-logistic-regression-a-linear-classifier
Logistic is linear



#### How to know you have enough training data?how to know how much needed

Ans: Learning curve
https://towardsdatascience.com/how-do-you-know-you-have-enough-training-data-ad9b1fd679ee

Note:
    Learning curve is metric vs sample size
    Training curve is metric vs training time/epoch
    不要搞混
    https://stats.stackexchange.com/questions/476339/learning-curve-vs-training-loss-curve
        The learning curve gives you an idea of how the model benefits from being incrementally fed more and more data observations, therefore focusing on inputs external to the model, thereby quantifying the marginal benefit of each new data point.
        The training curve gives you an idea of how the model benefits from having its bias-variance trade-off managed while cycling its algorithm back from start to finish repeatedly, therefore, focusing on processes or parameter calibration inputs internal to the model, likely while leaving the number of data observations unchanged.
					
