# Batch Normalization

Ref: [Youtube](https://www.youtube.com/watch?v=-5hESl-Lj-4), [CSDN](https://blog.csdn.net/u010698086/article/details/78045532), [Zhihu](https://zhuanlan.zhihu.com/p/24810318),[Zhihu](https://www.zhihu.com/question/38102762)


- What problem the BN targets?
  - When the network is non-linear, large value will be suppressed.
    - Before the network is well trained, the weight may be very large, then the network will be insensitive to large input. (saturate) 
    - However, in the ground truth, the output may be sensitive to the large input. In this case, change in weight*feature cannot efficiently passed to the output, make the training slow.
      - I.e. large change in weight*feature result in small change in output. The apparent conclusion then is output is not sensitive to the feature. But this conclusion is actually false/incorrect.
    - <div  align="center"><img src=./techniques_asset/batch_normalization_fig_1.png style = "zoom:30%"></div> 

- Why BN could solve the problem?
  - BN is equivalent to another mapping process/layer, this process will learn to transform the input data into the the most suitable scale.
    - I.e.$x_{output} = \gamma x_{input} + \beta $, the $(\gamma, \beta)$ is learned during training.
      - Support Ref: [MLMastery](https://machinelearningmastery.com/batch-normalization-for-training-of-deep-neural-networks): "These parameters are learned by..."
  - <div  align="center"><img src=./techniques_asset/batch_normalization_fig_1.png style = "zoom:30%"></div> 
- Effect of BN:
  - Accelerate the training. (Of course, it is BN's target)
  - Regularization: 
    - When the feature is insensitive to the feature, the BN will transform the feature to a very large scale, then the output will be insensitive to the feature, like ignore the feature, a soft version of dropout/feature elimination.
  
  
  <!-- - Outliers: when the value is larger than the most suitable scale learned by BN, it will be suppressed by non-linearity of NN. Make the network robust. -->


### FAQ

- Relation between mini-batch size and batch normalization.
  - The batch normalization should have better result, since the mini-batch is more closer to the population.
  - Ref: [StackExchange](https://datascience.stackexchange.com/questions/41873/batch-normalization-vs-batch-size)

## Appendix

### Apd 1. Batch-size related concept

Full batch learning: for each step, use the full dataset to update the parameter.
Mini-batch learning: for each step, use subset of dataset to update parameter.
Online learning:  for each step, use only 1 sample to update the parameter.

Ref: [kaggle-notebook](https://www.kaggle.com/residentmario/full-batch-mini-batch-and-online-learning), [StackExchange](https://stats.stackexchange.com/questions/110078/full-batch-vs-online-learning-vs-mini-batch)


Larger batch size, training time longer, generalize better (each step are smoother.)
Smaller batch size, converge (training time) faster, but the direction of step may be not stable.