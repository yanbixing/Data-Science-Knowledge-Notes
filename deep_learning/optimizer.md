# Optimizer

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

apply "momentum" to a variable refers that, the variable's value at time $t$ will **inherit** part of value from previous time(s). The general format is like:
    $$v_t =  \beta_{mom} \cdot v_{t-1} +  (1-\beta_{mom}) v^{\text{original}}_t$$

Advantage: avoid sudden change of the variable. I.e. the variable will change smoothly/gradually.

#### 1.2.3 GD with momentum

SGD with momentum means we add momentum to gradient, define it as **"velocity"**



$$\begin{aligned}
    \text{step size} &:= \text{constant lr} \cdot \text{velocity}\\
    & := \text{constant lr} \cdot \text{``momentumized grad"}
\end{aligned}$$


Advantage vs SGD: can overcome local minimums.

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