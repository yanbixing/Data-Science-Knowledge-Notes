# LSTM

## 1. Motivation

Ref: [Wiki-LSTM](https://en.wikipedia.org/wiki/Long_short-term_memory)

For "vanilla/classic/simple/naive" RNN, when training RNN with back-propagation, will vanish or explode.

### 1.1. Gradient vanishing in RNN


<div  align="center"><img src=https://sds-platform-private.s3-us-east-2.amazonaws.com/uploads/31_blog_image_2.png style = "zoom:40%"></div> 

Generally, the gradient of particular param $\theta$ is a sum of loss at different position. I.e. 

$$\begin{aligned}
\boldsymbol{gard}(\theta) := \partial_\theta R_{tot} := \sum^T_{\tau = 1} \partial_\theta  R_\tau     
\end{aligned}$$
where: 
 $$\begin{aligned}
\partial_\theta  R_\tau = \sum^\tau_{i=1} \frac{\partial R_\tau}{\partial\boldsymbol{h_\tau}}\frac{\partial h_\tau}{\partial h_i}\frac{\partial h_i}{\partial \theta}
\end{aligned}$$
where:
 $$\begin{aligned}
\frac{\partial h_\tau}{\partial h_i} & =  \frac{\partial h_\tau}{\partial h_{\tau-1} } \frac{\partial h_{\tau-1}}{\partial h_{\tau-2}} \dots \frac{\partial h_{i+1}}{\partial h_i}\\
& = \prod^{\tau-1}_{j = i}  \frac{\partial h_{j+1}}{\partial h_j} = \prod^{\tau-1}_{j = i}  \frac{\partial g(z_{j+1})}{\partial h_j}
\\
& = \prod^{\tau-1}_{j = i}  \frac{\partial g(W^{(rec)}h_j+ f(\boldsymbol{x}_j))}{\partial h_j} = \prod^{\tau-1}_{j = i}  \frac{\partial g( W^{(rec)}h_j+ f(\boldsymbol{x}_j) )}{\partial W^{(rec)}h_j+ f(\boldsymbol{x}_j)}\frac{\partial W^{(rec)}h_j+ f(\boldsymbol{x}_j)}{\partial h_j}\\
& = \prod^{\tau-1}_{j = i} W^{(rec)} \partial_{z_{j+1}}g(z_{j+1})\\
& = [W^{(rec)}]^{\tau - i} \prod^{\tau-1}_{j = i} \partial_z g(z)|_{z=z_j}
\end{aligned}$$

To more obviously reveal the gradient vanishing, let's we only have 1 output at $T$.

 $$\begin{aligned}
\partial_\theta  R_T & = \sum^T_{i=1} \frac{\partial R_T}{\partial\boldsymbol{\boldsymbol{h}_T}}\frac{\partial \boldsymbol{h}_T}{\partial \boldsymbol{h}_i}\frac{\partial \boldsymbol{h}_i}{\partial \theta} \\
&=  \sum^T_{i=1} \frac{\partial R_T}{\partial\boldsymbol{\boldsymbol{h}_T}}\frac{\partial \boldsymbol{h}_i}{\partial \theta} \left[  [W^{(rec)}]^{T - i} \prod^{T-1}_{j = i} \partial_z g(z)|_{z=z_j} \right]
\end{aligned}$$

This means, for RNN, the grad of loss on particular parameter $\theta$ is a sum of grad of loss to parameter at different recurrent step/position/unit $i$. For step/position/unit i far away from the final output, (assume $[W^{(rec)}]^{T - i} < 1$) the gradient contribution is $\rightarrow 0$, i.e. naive RNN don't have long term dependency.

Note: same mechanism but different behavior. 

For DNN, gradient pass from output back layer by layer, "gradient vanishing" means the **gradient** at very deep layers (layer far from the output) is 0 (vanish). Making the learning-process/parameter-update on the **deep layers extremely slow.**

For RNN, since the weight will be used multiple times, the gradient is a **sum** of gradient on different step, gradient vanishing means the **gradient contribution/components** from the layer far from output is 0. Making the RNN **have no long-term dependency** (insensitive to long-term input).


### 1.2. Why LSTM can restore long-term dependency?

Like skip-layer, LSTM could directly send the contribution of deep/long-term input to the output, so the gradient of output will also directly backpropagate to that layer (no prod of weights, the gard contribution of those deep/long-term input not vanish), so LSTM can have long term dependency.

Note: for very long dependency, LSTM will also have gradient vanishing, because cell state still changes during forward propagation. Ref: [MLMastery-blog](https://machinelearningmastery.com/exploding-gradients-in-neural-networks/)



## 2. Structure

- RNN structure
  - The hidden state will be simply determined by hidden state and new input.
    <div  align="center"><img src=https://colah.github.io/posts/2015-08-Understanding-LSTMs/img/LSTM3-SimpleRNN.png style = "zoom:40%"></div> 

- LSTM structure:  

  - Besides the hidden state, the is a memory cell that preserve/keep the info from previous states. The cell may k
    <div  align="center"><img src=https://colah.github.io/posts/2015-08-Understanding-LSTMs/img/LSTM3-chain.png style = "zoom:40%"></div> 

## 2.1. LSTM structure

In sum:
- **Forget gate:** generate forget vector $f_t = F(h_{t-1},x_t)$
  - So that we can attenuate/forget previous cell $C_{t,tmp} = f_t * C_{t-1} $
- **Input gate:** generate input vector $i_t = I(h_{t-1},x_t)$
  - So that we can revise previous cell $C_{t} = f_t * C_{t-1} + i_t * \tilde{c}_{t}$
  - $\tilde{c}_t = \tilde{c}_{gen}(h_{t-1},x_t) $ 
- **Output gate:** generate output vector $o_t = O(h_{t-1},x_t)$
  - $h_t = \tanh(C_{t})$

Ref: 

- [Github-blog](http://colah.github.io/posts/2015-08-Understanding-LSTMs/): LSTM structure
- [StackExchange](https://stats.stackexchange.com/questions/512542/what-is-the-encoder-in-a-seq2seq-rnn-doing): source of fig.
- [TowardsDataScience](https://towardsdatascience.com/illustrated-guide-to-lstms-and-gru-s-a-step-by-step-explanation-44e9eb85bf21): visualization of 

<div  align="center"><img src=https://i.stack.imgur.com/3Rn8m.png style = "zoom:40%"></div> 

- Cell state: cell state is a weighted sum of previous cell sate and the new input cell component.
  - Weight vector is output from forget gate and input gate respectively.
    - Notation:
      - $C_{t-1}$: input cell state
      - $C_{t}$: output cell state
      - $\tilde{C}_{t}$: cell state component calculated from hidden state $h_{t-1}$ and new input $x_t$
        <div  align="center"><img src=http://colah.github.io/posts/2015-08-Understanding-LSTMs/img/LSTM3-focus-C.png  style = "zoom:40%"></div> 

  - **Forget gate**: a sigmoid function with hidden state $h_t$ and input variable $x_t$ as input, give the "forget vector" to cell state.
    - Forget vector is a vector having same dimension with the cell state, each position have a numerical value [0,1] denoting how much value the cell state should keep at that position.
        <div  align="center"><img src=http://colah.github.io/posts/2015-08-Understanding-LSTMs/img/LSTM3-focus-f.png style = "zoom:40%"></div> 
    
    - Note:
      - $\sigma$: sigmoid layer, here, the input and output vector having the same dimension with cell state, the value at each position isbetween 0 and 1.
      - $*$ means pointwise/elementwise multiplication.
      - $+$ here means pointwise addition.
        - Ref: [TowardsDataScience](https://towardsdatascience.com/illustrated-guide-to-lstms-and-gru-s-a-step-by-step-explanation-44e9eb85bf21)
  - new input cell state: a vector have same dimension with cell state, calculated from hidden state $h_t$ and input variable $x_t$.
  - **Input gate**:  sigmoid function with hidden state $h_t$ and input variable $x_t$ as input, give an "input vector" denoting, on each position, how much the new input cell state should be-superposed-to/added prev cell state.
        <div  align="center"><img src=http://colah.github.io/posts/2015-08-Understanding-LSTMs/img/LSTM3-focus-i.png style = "zoom:40%"></div> 


- Hidden state: output hidden state $h_t$ is created based on the updated cell state $C_t$ and "output vector" $o_t$
  - **Output gate**: generate an output vector $o_t$ denoting, on each position of the cell state, how much we want to use as the output hidden states. 
  - The weight is 
<div  align="center"><img src=http://colah.github.io/posts/2015-08-Understanding-LSTMs/img/LSTM3-focus-o.png style = "zoom:40%"></div> 

### 2.3. LSTM vs GRU

GRU combines the forget and input gates into a single “update gate.”


<div  align="center"><img src=http://colah.github.io/posts/2015-08-Understanding-LSTMs/img/LSTM3-var-GRU.png style = "zoom:40%"></div> 