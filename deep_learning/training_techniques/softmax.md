# Activation functions

Ref: [TowardsDataScience](https://towardsdatascience.com/comparison-of-activation-functions-for-deep-neural-networks-706ac4284c8a)

## 0. Why use activation function in NN?

- introduce non-linearity
  - Or the NN will be linear, effect of multilayers of weights $\boldsymbol{W_i}$ equal to a single layer as: $W=\prod W_i$
- Some times normalize the output (limit the scale/scope of the output.)

## 1. Sigmoid

Used for a single-neural output value. Transform $[-\infty,\infty]$ to [-1,1]


Sigmoid:

$${\displaystyle S(x)={\frac {1}{1+e^{-x}}}={\frac {e^{x}}{e^{x}+1}}=1-S(-x).}$$

<div  align="center"><img src=https://upload.wikimedia.org/wikipedia/commons/8/88/Logistic-curve.svg style = "zoom:50%"></div>


## 2. Softmax

Usually used in multi-class classification tasks, as a individual layer. I.e. used to normalize multi-dimension output.

$${\displaystyle \sigma (\mathbf {z} )_{i}={\frac {e^{z_{i}}}{\sum _{j=1}^{K}e^{z_{j}}}}\ \ \ \ {\text{ for }}i=1,\dotsc ,K{\text{ and }}\mathbf {z} =(z_{1},\dotsc ,z_{K})\in \mathbb {R} ^{K}.}$$

### Differentiation: softmax vs sigmoid

In terms of the value, for a individual/single output-position/neuron:

- The softmax output value is dependent on other values, i.e. the sum of all output neural will be normalized to 1.
- The sigmoid output value is only dependent on itself, not related to other positions.

In terms of the taks:

- Softmax is for multi-class classification.
- Sigmoid is for binary classification.
  - Ref: [StackExchange](https://stats.stackexchange.com/questions/233658/softmax-vs-sigmoid-function-in-logistic-classifier)


##  3. ReLu

Advantage: introduce non linear



