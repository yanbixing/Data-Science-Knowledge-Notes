# Optimization

## 0. Basics

### 0.1. TBD

### 0.2. Interview Qs:

- Why GD works?
  - MyAns: $\mathbf{grad}$ is the direction that loss increase; GD go to the reverse direction, loss will decrease
  - Ref: W-MyOwnExp, L-[1p3a](https://www.1point3acres.com/bbs/thread-704600-1-1.html)

## 1. Different optimizers (TBD)

TBD: seems not too strict, need further docs.

Ref: [TowardsDataScience](https://towardsdatascience.com/deep-learning-optimizers-436171c9e23f), [Medium](https://medium.com/@Biboswan98/optim-adam-vs-optim-sgd-lets-dive-in-8dbf1890fbdc), [Wiki-Adam](https://en.wikipedia.org/wiki/Stochastic_gradient_descent#Adam)


The difference between different optimizer is actually about how we define the size of the step in the gradient descent process.

### 1.0 Notation

- $\mathbf{grad}^{(i)}_t :=  \left.  \frac{\partial L}{\partial w^{(i)}}\right|_{\boldsymbol{w} = \boldsymbol{w}_t}$
  - I.e. the gradient on $w^{(i)}$ at time $t$.
- $\text{lr}$: learning rate.



### 1.1. GD

For most basic GD, 

- the learning rate $lr$ is constant. 
- The step size is proportional to the gradient.

$$\text{step size} = \text{constant lr} \cdot \text{grad}$$
i.e.
$$\begin{aligned}
    \Delta w^{(i)}_t & =  -\mathrm{lr} \cdot  \mathbf{grad}^{(i)}_t 
\end{aligned}$$

- Batch GD: GD with batch sample.
- SGD: stochastic GD, extreme case of batch GD, GD with only one sample.

### 1.2. GD with momentum


#### 1.2.0. Momentum

Apply "momentum" to a variable refers that, the variable's value at time $t$ will **inherit** part of value from previous time(s). The general format is like:
    $$v_t =  \beta_{mom} \cdot v_{t-1} +  (1-\beta_{mom}) v^{\text{original}}_t$$

**Advantage**: Avoid sudden change of the variable. I.e. the variable will change smoothly/gradually. 
<mark style="background-color:yellow;"><font color="#0000dd"> For GD</font></mark>, avoid sudden change of step size can:

- <mark style="background-color:yellow;"><font color="#0000dd">Reduce</font></mark> occasional <mark style="background-color:yellow;"><font color="#0000dd">extreme large</font></mark> step size: make the <mark style="background-color:yellow;"><font color="#0000dd">training/convergence process more smooth</font></mark>. 
- <mark style="background-color:yellow;"><font color="#0000dd">Enlarge</font></mark> occasional <mark style="background-color:yellow;"><font color="#0000dd">extreme small</font></mark> step size: help solution <mark style="background-color:yellow;"><font color="#0000dd">overcome</font></mark> occasional <mark style="background-color:yellow;"><font color="#0000dd">local (shallow) minima</font></mark>.



#### 1.2.3 GD with momentum

SGD with momentum means we add momentum to gradient, define it as **"velocity"**



$$\begin{aligned}
    \text{step size} &:= \text{constant lr} \cdot \text{velocity}\\
    & := \text{constant lr} \cdot \text{``momentumized grad"}
\end{aligned}$$




I.e.

$$\begin{aligned}
    \Delta w^{(i)}_t & =  \mathrm{lr} \cdot v^{i}_t 
\end{aligned}$$

where: 
$$
v^{(i)}_t:=
\begin{cases}-\mathbf{grad}^{(i)}_t & t=1,\\
 \beta_{mom} \cdot v^{(i)}_{t-1} + (1-\beta_{mom})(-\mathbf{grad}^{(i)}_t)  & t>1 \\
\end{cases}
$$


### 1.3. Ada-grad

#### 1.3.0. Ada-

"Ada" stands for adaptive, means that the learning rate $lr$ is not constant, i.e. adaptively change with the value of gradients.

Usually, for learning rate, the adaptive form can be expressed like:
    $$\begin{aligned}
        \text{lr}_t & := \mathrm{F_{lr}}( \text{lr}_0, \mathbf{grad}_t, \mathbf{grad}_{t-1}, \dots )\\
        &:= \frac{\text{lr}_0}{\sqrt{\alpha_t}+\varepsilon}
    \end{aligned}$$

where:

- $\text{lr}_0$ indicate the the input learning rate param, a constant.
- $\varepsilon$ is a small constant that guarantee the denominator is not 0.
- $\alpha_t := \sum^{t}_{j=1} \| \mathbf{grad}_j \|^2$: sum squared gradient. It is the "adaptive coefficient" (NOT official name).


#### 1.3.1. Ada-grad

Ada-grad apply "adaptive" technique to learning rate. I.e. learning rate change with the (scale) of gradient. 

$$\text{step size} = \text{variable lr} \cdot \text{grad}$$

Advantage vs SGD: avoid too large or too small step size. 

I.e.

$$ \Delta w^{(i)}_t  =  -\mathrm{lr}_t \cdot  \mathbf{grad}^{(i)}_t $$

#### 1.3.1. Effect of Ada-grad on learning rate

- The learning rate will be smaller and smaller over time.
  - Proof: 
    - $\because \alpha_t := \sum^{t}_{j=1} \| \mathbf{grad}_j \|^2$
    - $\therefore t \uparrow \Rightarrow \alpha_t \uparrow \Rightarrow \mathrm{lr}_t \downarrow$
  - Benefit:
    - Better convergence quality
- Stabilize/smooth the step size during training, avoid occasional extreme large step size during occasional extreme large $\mathbf{grad}$ value.
  - Proof:
    - $\because \alpha_t := \sum^{t}_{j=1} \| \mathbf{grad}_j \|^2$
    - $\therefore \mathbf{grad_j} \uparrow \Rightarrow\alpha_j \uparrow \Rightarrow \mathrm{lr} \downarrow \Rightarrow \mathrm{step} = \mathrm{lr} (\downarrow)\cdot \mathbf{grad_j}(\uparrow)  \text{more stable}$
  - Application:
    - Counter the harm of occasional "too much"/extreme randomness during by mini-/stochastic GD.


### 1.4.Ada-Delta

Ada-delta go a step further than ada-grad, i.e. it applies "momentum" technique on learning rate.

$$\text{step size} = \text{momentumized variable lr} \cdot \text{grad}$$

- Advantage vs Ada-grad: more smooth, avoid occasional extreme large/small learning rate due to occasional extreme small/large $\alpha$ ("adaptive coefficient") values.

I.e.

 $$\Delta w^{(i)}_t =  -\text{m\_lr}_t \cdot  \mathbf{grad}^{(i)}_t$$

where: 

$$
\begin{cases} \text{m\_lr}_t := \frac{\text{lr}_0}{\sqrt{\text{m\_}\alpha_t}+\varepsilon} & ,\\
 {\text{m\_}\alpha_t} := \beta_{\text{m\_}\alpha}\cdot {\text{m\_}\alpha_{t-1}} + (1-\beta_{\text{m\_}\alpha})\alpha_t  \\
 \alpha_t := \sum^{t}_{j=1} \| \mathbf{grad}_j \|^2
\end{cases}
$$

### 1.5. Adam

#### 1.5.1. Adam

A combination on "Ada-delta" and "SGD with momentum". I.e.:

- Apply momentum technique on grad.
- Apply adaptive + momentum technique on learning rate.

$$\begin{aligned}
    \text{step size} &= \text{momentumized variable lr} \cdot \text{velocity}\\
    &=\text{momentumized variable lr} \cdot \text{momentumized grad}
\end{aligned}$$

I.e.

$$ \Delta w^{(i)}_t  =  -\text{m\_lr}_t \cdot  v^{i}_t $$

where: 

$$
v^{(i)}_t:=
\begin{cases}-\mathbf{grad}^{(i)}_t & t=1,\\
 \beta_{mom} \cdot v^{(i)}_{t-1} + (1-\beta_{mom})(-\mathbf{grad}^{(i)}_t)  & t>1 \\
\end{cases}
$$

and

$$
\begin{cases} \text{m\_lr}_t := \frac{\text{lr}_0}{\sqrt{\text{m\_}\alpha_t}+\varepsilon} & ,\\
 {\text{m\_}\alpha_t} := \beta_{\text{m\_}\alpha}\cdot {\text{m\_}\alpha_{t-1}} + (1-\beta_{\text{m\_}\alpha})\alpha_t  \\
 \alpha_t := \sum^{t}_{j=1} \| \mathbf{grad}_j \|^2
\end{cases}
$$

#### 1.5.2. Adam vs SGD

- Apply adaptive technique on learning rate: **learning rate scales** according to scale of gradient, avoid too large or too small step size. 
- Apply momentum on learning rate and gradient: **avoid sudden change (vanishing)** of the gradient and learning rate. Can effective **across local minimums**.
- Ref: [Medium](https://medium.com/@Biboswan98/optim-adam-vs-optim-sgd-lets-dive-in-8dbf1890fbdc)


## 2. Mini-batch (Stochastic) learning

### 2.1. Learning with different batch size

  - Full batch learning: for each step, use the full dataset to update the parameter.
  - Mini-batch learning: for each step, use subset of dataset to update parameter.
  - Online learning:  for each step, use only 1 sample to update the parameter.

Ref: [kaggle-notebook](https://www.kaggle.com/residentmario/full-batch-mini-batch-and-online-learning), [StackExchange](https://stats.stackexchange.com/questions/110078/full-batch-vs-online-learning-vs-mini-batch)

### 2.2. Effect of batch size

- Larger batch size:
  - convergence slower (training time longer) 
    - None: here, the "convergence slower" means to achieve same convergence progress, we need to iterate on more individual samples.
  - Each step are smoother: generalize better generalization/convergence quality.
- Smaller batch size:
  - convergence faster (training time shorter)
  - Introduce more randomness in each step
    - Cons: 
      - Make the optimization point not stable
      - May jump out of the global minimum
    - Pros: 
      - Also may help the solution “jump” out of local minima and find a global minimum. Ref: [Quora](https://www.quora.com/How-does-the-batch-size-of-a-neural-network-affect-accuracy)

## 3. Comparison: Momentum vs mini-batch vs adaptive

- Momentum:
  - No significance improvement on convergence (training) speed.
    - Note: convergence speed is discussed w.r.t. iteration of samples
    - <font color="#aa0000">(Con)</font> May harm/slower training speed, since the step size change slowly.
  - Step size more stable. 
    - <font color="#00aaaa">(Pro)</font>  Overcome local minima by inertia ([ɪˈnɜːrʃə]) / consistency brought by momentum. 
    - No significance improvement on convergence quality/stability.
- mini-batch/stochastic:
  - <font color="#00aaaa">(Pro)</font> Faster convergence speed 
    - i.e. required less iteration of samples to reach same quality)
  - More randomness by step
    - <font color="#00aaaa">(Pro)</font>  Overcome local minima by randomness brought by individual sample.
    - <font color="#aa0000">(Con)</font> Make convergence point (solution) less stable, lower convergence quality.
- Adaptive learning:
  - Learning rate (so as step size) gradually decrease
    - <font color="#00aaaa">(Pro)</font> Make the convergence point (solution) more stable, higer convergence quality.
    - Can start with large learning rate, not sacrifice too much on convergence (training) speed.
  - Step size is more stable but also randomized
    - <font color="#00aaaa">(Pro)</font> Avoid influence of occasional extreme $\mathbf{grad}$ value (counter by changing $\mathrm{lr}$ according to $\mathbf{grad}$)
    - <font color="#00aaaa">(Pro)</font> Help to overcome shallow local minima (by introducing randomness to $\mathrm{lr}$)