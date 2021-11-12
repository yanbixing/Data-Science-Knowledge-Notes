# Activation functions

Ref: [TowardsDataScience](https://towardsdatascience.com/comparison-of-activation-functions-for-deep-neural-networks-706ac4284c8a)

## 0. Why use activation function in NN?

- introduce non-linearity
  - Or the NN will be linear, effect of multilayers of weights $\boldsymbol{W_i}$ equal to a single layer as: $W=\prod W_i$
- Some times normalize the output (limit the scale/scope of the output.)

## 1. Sigmoid

Used for a single-neural output value. Transform $[-\infty,\infty]$ to [-1,1]


Sigmoid:

$$ \begin{equation} {\displaystyle S(x)={\frac {1}{1+e^{-x}}}={\frac {e^{x}}{e^{x}+1}}=1-S(-x).} \end{equation} $$

<div  align="center"><img src=https://upload.wikimedia.org/wikipedia/commons/8/88/Logistic-curve.svg style = "zoom:50%"></div>


### 1.1. Sigmoid layer:

The input and output will have a same dimension, the output at $i$-th position only dependent on input at $i$-th position, independent-to/not affect by other positions.

Ref: [TowardsDataScience](https://towardsdatascience.com/illustrated-guide-to-lstms-and-gru-s-a-step-by-step-explanation-44e9eb85bf21)

### 1.2. tanh (hyperbolic tangent) vs sigmoid

$$\begin{equation} \mathrm{tanh} \left( x \right)  = \frac{e^x - e^{-x}}{e^x + e^{-x}} = 2 \cdot \sigma \left( 2 x \right) - 1 \end{equation}$$

From the definition, we can see the shape/tendency of tanh is identical to sigmoid.
- Difference:
  - tanh is [-1,1], sigmoid is [0,1]
  - In application, tanh have larger gradient around 0.
  - Ref: [StackExchange](https://stats.stackexchange.com/questions/101560/tanh-activation-function-vs-sigmoid-activation-function)

<div  align="center"><img src=https://miro.medium.com/max/1190/1*f9erByySVjTjohfFdNkJYQ.jpeg  style = "zoom:50%"></div>

Ref: [Wiki-tanh](), [StackExchange](https://stats.stackexchange.com/questions/101560/tanh-activation-function-vs-sigmoid-activation-function)

## 2. Softmax

Usually used in multi-class classification tasks, as a individual layer. I.e. used to normalize multi-dimension output.

$$ \begin{equation} {\displaystyle \sigma (\mathbf {z} )_{i}={\frac {e^{z_{i}}}{\sum _{j=1}^{K}e^{z_{j}}}}\ \ \ \ {\text{ for }}i=1,\dotsc ,K{\text{ and }}\mathbf {z} =(z_{1},\dotsc ,z_{K})\in \mathbb {R} ^{K}.} \end{equation}$$

### 2.1. softmax layer vs sigmoid layer

In terms of the value, for a individual/single output-position/neuron:

- The softmax output value is dependent on other values, i.e. the sum of all output neural will be normalized to 1.
- The sigmoid output value is only dependent on itself, not related to other positions.

In terms of the taks:

- Softmax is for multi-class classification.
- Sigmoid is for binary classification.
  - Ref: [StackExchange](https://stats.stackexchange.com/questions/233658/softmax-vs-sigmoid-function-in-logistic-classifier)


## 3. ReLu (Rectified Linear Unit)

$$\begin{equation} f(x) = \begin{cases} 0, & \text{if}\ x < 0 \\ x, & \text{otherwise} \\ \end{cases} \end{equation}$$

<div  align="center"><img src= https://miro.medium.com/max/357/1*oePAhrm74RNnNEolprmTaQ.png style = "zoom:100%"></div>

- Advantage: 
  - Less computational resource, calculated load is less than sigmoid or tanh.
  - Output is sparse, good in DL, because:
    - Larger sparsity mean fewer connection, mean the model is simple, less chance to be overfitting.
    - Larger sparsity mean few neuron is activated, means the features are more "distinct" /"linear separable" (different input activate less common neuron, share less same axis/info, which is more "independent"/"linear separable"/"easy to be separated".)
    - Computation faster, and less memory consumption, and model may be further compressed.
    - Ref: 
      - [Blog](https://www.machinecurve.com/index.php/2019/09/04/relu-sigmoid-and-tanh-todays-most-used-activation-functions/#in-short-the-relu-sigmoid-and-tanh-activation-functions): activation - sparsity is good
      - [Quora](https://www.quora.com/What-does-it-mean-that-activation-functions-like-ReLUs-in-NNs-induce-sparsity-in-the-hidden-units): why good.
      - [Quora](https://www.quora.com/Where-is-Sparsity-important-in-Deep-Learning): Why good.
  - No vanish grad? (TBD: why?)
    - **Personal understanding**: the gradient is product of later weights $W$ and derivative of the activation functions $\partial_zg(z)$, if the $\partial_zg(z)$ is small, then gradient will also vanish.
    - Ref:[StackExchange](https://stats.stackexchange.com/questions/126238/what-are-the-advantages-of-relu-over-sigmoid-function-in-deep-neural-networks), [Blog](https://www.machinecurve.com/index.php/2019/09/04/relu-sigmoid-and-tanh-todays-most-used-activation-functions/#in-short-the-relu-sigmoid-and-tanh-activation-functions)
- Disadvantage:
  - vs tanh/sigmoid: blow up the value (range of positive value is not limited.)
    - Ref:[StackExchange](https://stats.stackexchange.com/questions/126238/what-are-the-advantages-of-relu-over-sigmoid-function-in-deep-neural-networks)
  - Self-Dying ReLu problem: Grad when $x<0$ is 0, not learning.
    - Solution: other types of Relu like Leaky Relu:
      <div  align="center"><img src=https://www.researchgate.net/profile/Aditi-Shenoy-3/publication/334389306/figure/fig8/AS:779352161677313@1562823443351/Illustration-of-output-of-ELU-vs-ReLU-vs-Leaky-ReLU-function-with-varying-input-values.ppm style = "zoom:60%"></div>



## Temp Summary:
  
  
Generally, you in terms of the training speed (priority when NN is deep):
  Leaky Relu > Relu > tanh > sigmod
  - Leaky Relu > Relu: can update (have grad) when $x<0$ input is negative
  - Relu > tanh: less complex to compute
  - Tanh>sigmoid: sharper slope around 0.
  - Ref: [TowardsDataScience](https://towardsdatascience.com/comparison-of-activation-functions-for-deep-neural-networks-706ac4284c8a)








