# Time Series

## 1. Basics


### 1.0. Stochastic process

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


### 1.1. Decomposition of time series data

A non-stationary time series data $Y_t$ typically can be decomposed into three components: 
- Trend $F_t$, 
- Seasonal component $S_t$
- stationary component $X_t$

$$Y_t =F_t+ S_t+ X_t$$

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
    
$$\rho_{XX}(t_i, t_j):= \frac{K_{XX}(t_i, t_j)}{\sigma_{t_i}\sigma_{t_j}} = \frac{\mathrm{Cov}(X_{t_i},X_{t_j})}{\sigma_{t_i}\sigma_{t_j}} = r(t_i, t_j)$$

Note: when $t_i = t_j$, $K_{XX}(t_i, t_i)=\mathrm{Cov}(X_{t_i},X_{t_i})= E[(X_{t_i} - \mu_{t_i})(X_{t_i} - \mu_{t_i})] = \mathrm{Var}(X_{t_i})$

#### 1.3.2. Autocorrelation for weak stationary process

Since the value of autocovariance $K_{XX}(t_i, t_j)$ is only determined by difference between $t_i,t_j$, not related to its absolute value.

AutoCovariance value can be write as a function of time interval: 
      
$$K_{XX}(\tau):=K_{XX}(t,t+\tau)$$

also **autocorrelation function (ACF)** is defined as:

$$\rho_{XX}(\tau):= \frac{K_{XX}(\tau)}{\mathrm{Var}(X_t)} = \frac{K_{XX}(\tau)}{K_{XX}(0)}$$

An ACF plot is usually used to show the ACF value at different lag $\tau$, as:


<div align="center"><img src=./time_series_asset/acf_plot.png style = "zoom:30%"></div>



### 1.4. Partial autocorrelation

Ref: [PSU-Stat510-Lesson2](https://online.stat.psu.edu/stat510/lesson/2/2.2)

$$\pi_{XX}(t_i, t_j): = \frac{\mathrm{Cov}(X_{t_i},X_{t_j}| X_{t_i+1}\dots X_{t_{j-1}} )}{ \sqrt{\mathrm{Var}(X_{t_i}| X_{t_i+1}\dots X_{t_{j-1}} )}  \sqrt{\mathrm{Var}(X_{t_j}| X_{t_i+1}\dots X_{t_{j-1}} )}}$$

Understanding: the covariance between $X_{t_i}$, $X_{t_j}$ after eliminating the effect of other steps.
Ref:[StackExchange](https://stats.stackexchange.com/questions/483383/difference-between-autocorrelation-and-partial-autocorrelation)


## 2. Different types of stochastic process

Ref: [CourseHero-Slides: NYU](https://www.coursehero.com/sitemap/schools/1602-New-York-University/courses/1788428-COURANTG632707/)

### 2.1. White noise (WN)

A $\mathrm{WN}(0,\sigma^2)$ time series sequence $\{Z_t\}$ is a "totally" random, no-pattern, process in our intuition. Mathematically:

- $\mathbb{E}(Z_t)=0$
- $\mathbb{E}(Z^2_t)=\sigma^2$
- $K_{XX}(t_i,t_j) = \begin{cases} \sigma^2 & t_i=t_j \\0 & t_i \neq t_j \end{cases}$
  - I.e. for $t_i\neq t_j$, $Z_{t_i}$ and $Z_{t_j}$ are uncorrelated.
  - I.e. $\{Z_{t}\}$ is an $\mathrm{IID}(0,\sigma^2)$ process.


WN process is (weakly) stationary.

<div align="center"><img src=./time_series_asset/wn_plot_and_acf.png style = "zoom:50%"></div>

Note: the blue line are the 95% confidence interval for the ACF value is 0. Ref: [StackExchange](https://stats.stackexchange.com/questions/139796/autocorrelation-and-partial-correlation-plots-in-arma-models)

### 2.2. Random Walk (RW)

An $\text{RW}$ time series sequence $\{X_t\}$ is the time $t$'s coordinates of a particle that is doing random walk. The displacement vector at each unit time interval is an iid, denoted as $V_t\sim IID(0,\sigma^2)$.

$$X_t = \sum_t V_t $$

<div align="center"><img src=./time_series_asset/rn_plot_and_acf.png style = "zoom:50%"></div>


### 2.3. Moving average

#### 2.3.1. Definition

An $\text{MA}(Q)$ time series sequence $\{X_t\}$ is a weighted average of most recent $Q+1$ white noise values (current value + previous $Q$ values). The white noise values at each time $t$ is denoted as $Z_t \sim WN(0,\sigma_z^2)$

$$\begin{aligned}
X_t &= Z_t + \sum^Q_{q=1}\theta_qZ_{t-q}\\
&=(1+\sum^Q_{q=1}\theta_qB^q)Z_t\\
&=(\sum^Q_{q=0}\theta_qB^q)Z_t
\end{aligned}$$

where $B$ is is the backshift operator as $BZ_t = Z_{t-1}$ and $\theta_0:=1$

#### 2.3.2. Mean and Variance


$\mathbb{E}(X_t) = 0$
$\mathrm{Var}(X_t) = \sigma_z^2 \sum^Q_{q=0}\theta_q^2 $

Ref: [Bookdown](https://bookdown.org/gary_a_napier/time_series_lecture_notes/ChapterFour.html)

#### 2.3.3. Autocorrelation

For $\text{MA}(Q)$ process, the $Q$ can be inferred from the ACF: When lag $\tau>Q$, the ACF $\rho_{XX}(\tau)$ will be 0:

$$\rho_{XX}(\tau) = \begin{cases} 1 & \tau =0\\
   \frac{\sum^{Q-\tau}_{q=0} \theta_q\theta_{q+\tau}}{\sum^Q_{q=0} \theta^2_q } \in [0,1] & 1 \leq \tau \leq Q\\
    0 & \tau>Q
\end{cases}$$


Ref: [Bookdown](https://bookdown.org/gary_a_napier/time_series_lecture_notes/ChapterFour.html)

<div align="center"><img src=./time_series_asset/ma1_plot_and_acf_positive.png style = "zoom:50%"></div>

<div align="center"><img src=./time_series_asset/ma1_plot_and_acf_negative.png style = "zoom:50%"></div>

### 2.4. AutoRegressive (AR)

#### 2.4.1. Definition

An $\text{AR}(P)$ time series sequence $\{X_t\}$ is a weighted average of most recent $p$ time series values $\{X_{t-p}\}$ plus current white noise $Z_t \sim WN(0,\sigma_z^2)$.

$$\begin{aligned}
X_t &=  \sum^P_{p=1}\phi_pX_{t-p} + Z_t\\
(1-\sum^P_{p=1}\phi_pB^p)X_t&=Z_t
\end{aligned}$$


#### 2.4.2. Mean and Variance


$\mathbb{E}(X_t) = 0$
$\mathrm{Var}(X_t) = \sigma_z^2 + \sum^P_{p=1}\theta_p K_{XX}(p) $

Ref: [Bookdown](https://bookdown.org/gary_a_napier/time_series_lecture_notes/ChapterThree.html)

#### 2.4.3. Autocorrelation

Unlike MA process, the $P$ hyperparameter of an $\text{AR}(P)$ process CANNOT be inferred with ACF.

$ K_{XX}(\tau) = \sum^P_{p=1}\phi_p K_{XX}(\tau-p)$

$\rho_{XX}(\tau) = \sum^P_{p=1}\phi_p\rho_{XX}(\tau-p)$


Ref: [Bookdown](https://bookdown.org/gary_a_napier/time_series_lecture_notes/ChapterThree.html)

E.g. For AR(1): $\rho_{XX}(0)=1$, $\rho_{XX}(1)=\phi_1$, $\rho_{XX}(2)=\phi^2_1$, ...,  $\rho_{XX}(h)=\phi^h_1$

<div align="center"><img src=./time_series_asset/ar1_plot_and_acf_positive.png style = "zoom:50%"></div>

<div align="center"><img src=./time_series_asset/ar1_plot_and_acf_negative.png style = "zoom:50%"></div>

#### 2.4.3. Partial autocorrelation

For $\text{AR}(P)$ process, the $P$ is usually inferred from the partial autocorrelation (PACF) $\pi_{XX}$, as:

$$\pi_{XX}(\tau) = \begin{cases} positive & \tau \leq P\\
    0 & \tau>P
\end{cases}$$

Ref: [Bookdown](https://bookdown.org/gary_a_napier/time_series_lecture_notes/ChapterThree.html)

### 2.5 ARIMA

An $\text{ARIMA}(P,Q)$ process is 