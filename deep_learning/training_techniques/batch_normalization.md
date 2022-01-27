# Batch Normalization

## 1.What is batch norm?

Batch normalization (also known as batch norm) is a method used to make artificial neural networks faster and more stable through normalization of the layers' inputs by **re-centering and re-scaling**.

Ref: [Wiki-BN](https://en.wikipedia.org/wiki/Batch_normalization)

## 2. Why Batch Norm?

Ref: [Youtube](https://www.youtube.com/watch?v=-5hESl-Lj-4), [CSDN](https://blog.csdn.net/u010698086/article/details/78045532), [Zhihu](https://zhuanlan.zhihu.com/p/24810318),[Zhihu](https://www.zhihu.com/question/38102762)


- What problem the BN targets? (TBD)
  - When activation function saturates, the training is slow, and "gradient vanishing happens" if a lot neurons saturates.
    - Activation saturates = derivative $g'(z)\to 0$
    - $\partial_{w^{(i)}}L \propto \underset{\forall j>i}{\prod} W^{(j)} \underset{\forall j>i}{\prod} g'(z^{(j)})$
    - ref: [gradient_vanishing.md](./gradient_vanishing.md)
  - "Saturates": When the network is non-linear, <mark style="background-color:yellow;"><font color="#0000dd">**large value will be suppressed.**</font></mark>
    - Before the network is well trained, the weight may be very large, then the network will be insensitive to large input. (saturate) 
    - However, in the ground truth, the output may be sensitive to the large input. In this case, change in weight*feature cannot efficiently passed to the output, <mark style="background-color:yellow;"><font color="#0000dd">**make the training slow**</font></mark>.
      - I.e. large change in weight*feature result in small change in output. The apparent conclusion then is **output is not sensitive to the feature**. But this conclusion is actually false/incorrect. (Why?)
    - <div  align="center"><img src=./techniques_asset/batch_normalization_fig_1.png style = "zoom:30%"></div> 

- Why BN could solve the problem? 
  - BN is equivalent to another mapping process/layer, this process will learn to <mark style="background-color:yellow;"><font color="#0000dd">transform the input data into the the **most suitable scale,** avoid the suppression/insensitivity on large value.</font></mark>
    - (TBD?Not sure) I.e.$x_{output} = \gamma x_{input} + \beta $, the $(\gamma, \beta)$ is learned during training.
    - (TBD?Not sure) How to train? (TBD) 这个似乎是错的，或者advanced一点 **Personal understanding**: BN layer is similar to a diagonalized weight layer, the i-th diagonal element is $\gamma_i$. Since there is only diagonal element, so, there is a one-to-one scaling mapping for each neural.
      - Support Ref: [MLMastery](https://machinelearningmastery.com/batch-normalization-for-training-of-deep-neural-networks): "These parameters are learned by..."
      - 正确的: 
        - Ref: [Quora](https://www.quora.com/How-does-batch-normalization-behave-differently-at-training-time-and-test-time)
  - <div  align="center"><img src=./techniques_asset/batch_normalization_fig_2.png style = "zoom:30%"></div> 
- Effect of BN:
  - Accelerate the training. (Of course, it is BN's target)
  - Regularization: 
    - When the feature is insensitive to the feature, the BN will transform the feature to a very large scale, then the output will be insensitive to the feature, like ignore the feature, a soft version of dropout/feature elimination.
  
  
  <!-- - Outliers: when the value is larger than the most suitable scale learned by BN, it will be suppressed by non-linearity of NN. Make the network robust. -->

## 3. Difference on train vs test stage

- Train: 
  - Theoretically, the samples just <mark style="background-color:yellow;"><font color="#0000dd">**normalized with batch statistics**</font></mark>: $\mu_{\text{batch}}$ & $\mathrm{Var}_{\text{batch}}$, the mean and variance of **this current batch**, <mark style="background-color:yellow;"><font color="#0000dd">keep changing during the training process</font></mark>, as the algorithm shown above.
    - I.e. when we normalize a batch, we just normalize the samples w.r.t. this batch, we don't care about the value in other batches.
    - I.e. <mark style="background-color:yellow;"><font color="#0000dd">For **different batch**, the mean and variance is **different**.</font></mark>
  - Practically, we will care about other batches, we will update "the mean and variance used to do normalization" ($\mu_{\text{global},t}$&$\mathrm{Var}_{\text{global}, t}$) as an weighted average of mean and variance in current batch ($\mu_{\text{batch},t}$&$\mathrm{Var}_{\text{batch}, t}$) and mean and variance in previous steps ($\mu_{\text{global},t-1}$&$\mathrm{Var}_{\text{global}, t}-1$), as:
    - $\mu_{\text{global},t} = \mathrm{decay}\cdot\mu_{\text{global},t-1} + (1-\mathrm{decay})\cdot\mu_{\text{batch},t}$
    - $\mathrm{Var}_{\text{global},t} = \mathrm{decay}\cdot\mathrm{Var}_{\text{global},t-1} + (1-\mathrm{decay})\cdot\mathrm{Var}_{\text{batch},t}$
- Test: 
  - the samples are <mark style="background-color:yellow;"><font color="#0000dd">**normalized with population statistics**</font></mark>: $\mu_{\text{whole}}$ & $\mathrm{Var}_{\text{whole}}$, the mean and variance of this **the whole dataset**. <mark style="background-color:yellow;"><font color="#0000dd">No change during the test process</font></mark>.
    - I.e. <mark style="background-color:yellow;"><font color="#0000dd">For **different samples**, the mean and variance is **the same**.</font></mark>
    - One way to implement this is:
      - During training, memorize all $\mu_{\text{batch},t}$&$\mathrm{Var}_{\text{batch},t}$ values
      - When test: $\mu_{\text{whole}} = \text{avg}(\{ \mu_{\text{batch}} \})$, $\mathrm{Var}_{\text{whole}} = \text{avg}(\{ \mathrm{Var}_{\text{batch}} \})$
- Ref: [Quora](https://www.quora.com/How-does-batch-normalization-behave-differently-at-training-time-and-test-time): the weighted ($\mu_{\text{global},t}$&$\mathrm{Var}_{\text{global}, t}$) is implement in TensorFlow? (TBD)
- **Personal thinking**: maybe we can update the $\mu_{\text{global},t}$&$\mathrm{Var}_{\text{global}, t}$ in a stream way?
  - Like the algo calculate $\mu(\text{stream}[:i])$&$\mathrm{Var(\text{stream}[:i])}$ for a stream?
  - See: [algo-stream.md](../../algorithm/stream.md)

## 4. Effect of batch size on batch norm

Ref: [StackExchange](https://datascience.stackexchange.com/questions/41873/batch-normalization-vs-batch-size)

Since:

- During training, the sample are normalized with batch $\mu_{\text{batch}}$ & $\mathrm{Var}_{\text{batch}}$
  - Each individual batch corresponds each individual step
- But when test, the samples are normalized with $\mu_{\text{whole}}$ & $\mathrm{Var}_{\text{whole}}$
  
Thus, obviously

- The  <mark style="background-color:yellow;"><font color="#0000dd">**larger batch size**</font></mark>
- The more close of $\mu_{\text{batch}}$ & $\mathrm{Var}_{\text{batch}}$  $\to$ $\mu_{\text{whole}}$ & $\mathrm{Var}_{\text{whole}}$
- The  <mark style="background-color:yellow;"><font color="#0000dd">**more accurate the step is**</font></mark>.
  - I.e. the  <mark style="background-color:yellow;"><font color="#0000dd">**less randomness in each step**</font></mark>.

Pro vs Cons

Similar to the effect when we increase batch in GD.

- Pros: 
  - Accurate step may have help
    - The convergence process be <mark style="background-color:yellow;"><font color="#0000dd">**more smooth**</font></mark>.
    - The optimization solution be <mark style="background-color:yellow;"><font color="#0000dd">**more stable**</font></mark>.
- Cons: 
  - Less randomness $\Rightarrow$ <mark style="background-color:yellow;"><font color="#0000dd">**harder to escape from shallow local minima**</font></mark>.
  - Larger batch $\Rightarrow$ training process <mark style="background-color:yellow;"><font color="#0000dd">**slower**</font></mark> 
    - I.e. Need more samples to take one step, thus need more iterations to reach convergence point.


### FAQ

- Relation between mini-batch size and batch normalization.
  - The batch normalization should have better result, since the mini-batch is more closer to the population.
  - Ref: [StackExchange](https://datascience.stackexchange.com/questions/41873/batch-normalization-vs-batch-size)