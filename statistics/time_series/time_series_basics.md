# Time Series

## 1. Basics


### 1.1. Stochastic process

**Stochastic process** is a sequence of events in which the outcome *at any stage* depends on some probability. Ref: [OSU-Math](https://people.math.osu.edu/husen.1/teaching/571/markov_1.pdf), [hidden_markov_model.md](../../natural_language_processing/hidden_markov_model.md)

E.g. If you generate a stochastic process $\{X_t\}$
- First, you need to know the distribution function $P_i(x)$ for $i$-th position of the stochastic process.
- Then, random sample a value from the distribution, i.e. $x_i \sim P_i(x)$ assign it to the $i$-th position in $\{X_t\}$, i.e. let $X_i = x_i$



<!-- - Before final observation, each position in the series is random value, not definite, though follow some certain distribution.
- I.e. This means if you generated the data at different time, the series you get is different. -->
Since the value of each position has a distribution, like in normal statistics, it is reasonable to define typical statistical metrics for each position, like:

- $\mu_{t_i} := E(X_{t_i})$
- $\sigma_{t_i}:=SD(X_{t_i}) = \sqrt{E[(X_{t_i}-\mu_{t_i})^2]}$


**Note:** stochastic=random, random doesn't means no pattern, it just means the result is not definite, there may be a distribution. The "no-pattern random" is called white-noise.

### 1.2. Stationary process

#### 1.2.1. Definition

A time series $\{X_t\}$ is **weak stationary process** if:

- $E(X^2_t)<\infty$
- For all $t$, $E(X_t)$ are a same constant $C$
-  $K_{XX}(t_i, t_j) = K_{XX}(t_i+T, t_j+T)$ stands for any $T$.
   -  Note: here, the value of autocovariance $K_{XX}(t_i, t_j)$ is only determined by difference between $t_i,t_j$, not related to its absolute value.

#### 1.2.2. Properties of stationary process

How to prove $\mathrm{Var}(X_t)$ is also a same constant for all t, under weak stationary conditions? (this is true, see [Wiki-Autocorrelation](https://en.wikipedia.org/wiki/Autocorrelation) (TBD)



### 1.3. Autocorrelation

#### 1.3.1. Definition

AutoCovariance: 

$$K_{XX}(t_i, t_j):= \mathrm{Cov}(X_{t_i},X_{t_j}) = E[(X_{t_i} - \mu_{t_i})(X_{t_j} - \mu_{t_j})]$$

AutoCorrelation: 
    
$$\rho_{XX}(t_i, t_j):= \frac{K_{XX}(t_i, t_j)}{\sigma_{t_i}\sigma_{t_j}}$$

#### 1.3.2. Autocorrelation for weak stationary process

Since the value of autocovariance $K_{XX}(t_i, t_j)$ is only determined by difference between $t_i,t_j$, not related to its absolute value.

AutoCovariance value can be write as a function of time interval: 
      
$$K_{XX}(\tau):=K_{XX}(t,t+\tau)$$

also:

$$\rho_{XX}(\tau):= \frac{K_{XX}(\tau)}{\mathrm{Var}(X_t)}$$

### 1.4. Decomposition of time series data

A non-stationary time series data $Y_t$ typically can be decomposed into three components: Trend $F_t$, Seasonal component $S_t$ and stationary component $X_t$

$$Y_t =F_t+ S_t+ X_t$$


