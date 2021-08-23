# Time Series Basics


## 0. Stochastic process

**Stochastic process** is a sequence of events in which the outcome *at any stage* depends on some probability. Ref: [OSU-Math](https://people.math.osu.edu/husen.1/teaching/571/markov_1.pdf), [hidden_markov_model.md](../../natural_language_processing/hidden_markov_model.md)

E.g. If you generate a stochastic process $\{X_t\}$
- First, you need to know the distribution function $P_i(x)$ for $i$-th position of the stochastic process.
- Then, random sample a value from the distribution, i.e. $x_i \sim P_i(x)$ assign it to the $i$-th position in $\{X_t\}$, i.e. let $X_i = x_i$



<!-- - Before final observation, each position in the series is random value, not definite, though follow some certain distribution.
- I.e. This means if you generated the data at different time, the series you get is different. -->
- 
Since the value of each position has a distribution, like in normal statistics, it is reasonable to define typical statistical metrics for each position, like:

- $\mu_{t_i} := E(X_{t_i})$
- $\sigma_{t_i}:=SD(X_{t_i}) = \sqrt{E[(X_{t_i}-\mu_{t_i})^2]}$


**Note:** stochastic=random, random doesn't means no pattern, it just means the result is not definite, there may be a distribution. The "no-pattern random" is called white-noise.


## 1. Stationary process

Typically, "Time series modelling" is a topic mainly focusing on fitting the stationary signal.

### 1.1. Definition

A time series $\{X_t\}$ is **weak stationary process** if:

- $E(X^2_t)<\infty$
- For all $t$, $E(X_t)$ are a same constant $C$
-  $K_{XX}(t_i, t_j) = K_{XX}(t_i+T, t_j+T)$ stands for any $T$.
   -  Note: here, the value of autocovariance $K_{XX}(t_i, t_j)$ is only determined by difference between $t_i,t_j$, not related to its absolute value.

### 1.2. Properties of stationary process

How to prove $\mathrm{Var}(X_t)$ is also a same constant for all t, under weak stationary conditions? (this is true, see [Wiki-Autocorrelation](https://en.wikipedia.org/wiki/Autocorrelation) (TBD)



### 1.3. Decomposition of time series data

A non-stationary time series data $Y_t$ typically can be decomposed into three components: 
- Trend $F_t$, 
- Seasonal component $S_t$
- stationary component $X_t$

$$Y_t =F_t+ S_t+ X_t$$

So, to get stationary component, we first need to **de-trend** and **de-seasonality**.


#### 1.3.1 Concept differentiation: seasonal vs cyclic

- Seasonality: the value repetition has fixed period, like a day, a week, a year, etc.
- Cyclic: has no fixed period.
  - The value will fluctuate (up and down), but there is no fixed period.
  - E.g. Economic condition, development of a industry.
- Ref: [Blog](https://robjhyndman.com/hyndsight/cyclicts/), [StackExchange](https://stats.stackexchange.com/questions/234492/what-is-the-difference-between-period-cycle-and-seasonality)



## 3. Autocorrelation

### 3.1. Definition

AutoCovariance: 

$$K_{XX}(t_i, t_j):= \mathrm{Cov}(X_{t_i},X_{t_j}) = E[(X_{t_i} - \mu_{t_i})(X_{t_j} - \mu_{t_j})]$$

AutoCorrelation: 
    
$$\rho_{XX}(t_i, t_j):= \frac{K_{XX}(t_i, t_j)}{\sigma_{t_i}\sigma_{t_j}} = \frac{\mathrm{Cov}(X_{t_i},X_{t_j})}{\sigma_{t_i}\sigma_{t_j}} = r(t_i, t_j)$$

Note: when $t_i = t_j$, $K_{XX}(t_i, t_i)=\mathrm{Cov}(X_{t_i},X_{t_i})= E[(X_{t_i} - \mu_{t_i})(X_{t_i} - \mu_{t_i})] = \mathrm{Var}(X_{t_i})$

### 3.2. Autocorrelation for weak stationary process

Since the value of autocovariance $K_{XX}(t_i, t_j)$ is only determined by difference between $t_i,t_j$, not related to its absolute value.

AutoCovariance value can be write as a function of time interval: 
      
$$K_{XX}(\tau):=K_{XX}(t,t+\tau)$$

also **autocorrelation function (ACF)** is defined as:

$$\rho_{XX}(\tau):= \frac{K_{XX}(\tau)}{\mathrm{Var}(X_t)} = \frac{K_{XX}(\tau)}{K_{XX}(0)}$$

An ACF plot is usually used to show the ACF value at different lag $\tau$, as:


<div align="center"><img src=./time_series_asset/acf_plot.png style = "zoom:30%"></div>



## 4. Partial autocorrelation

Ref: [PSU-Stat510-Lesson2](https://online.stat.psu.edu/stat510/lesson/2/2.2)

$$\pi_{XX}(t_i, t_j): = \frac{\mathrm{Cov}(X_{t_i},X_{t_j}| X_{t_i+1}\dots X_{t_{j-1}} )}{ \sqrt{\mathrm{Var}(X_{t_i}| X_{t_i+1}\dots X_{t_{j-1}} )}  \sqrt{\mathrm{Var}(X_{t_j}| X_{t_i+1}\dots X_{t_{j-1}} )}}$$

Understanding: the covariance between $X_{t_i}$, $X_{t_j}$ after eliminating the effect of other steps.
Ref:[StackExchange](https://stats.stackexchange.com/questions/483383/difference-between-autocorrelation-and-partial-autocorrelation)


