# Time series models

Ref: [CourseHero-Slides: NYU](https://www.coursehero.com/sitemap/schools/1602-New-York-University/courses/1788428-COURANTG632707/)

## 1. White noise (WN)

A $\mathrm{WN}(0,\sigma^2)$ time series sequence $\{Z_t\}$ is a "totally" random, no-pattern, process in our intuition. Mathematically:

- $\mathbb{E}(Z_t)=0$
- $\mathbb{E}(Z^2_t)=\sigma^2$
- $K_{XX}(t_i,t_j) = \begin{cases} \sigma^2 & t_i=t_j \\0 & t_i \neq t_j \end{cases}$
  - I.e. for $t_i\neq t_j$, $Z_{t_i}$ and $Z_{t_j}$ are uncorrelated.
  - I.e. $\{Z_{t}\}$ is an $\mathrm{IID}(0,\sigma^2)$ process.


WN process is (weakly) stationary.

<div align="center"><img src=./time_series_asset/wn_plot_and_acf.png style = "zoom:50%"></div>

Note: the blue line are the 95% confidence interval for the ACF value is 0. Ref: [StackExchange](https://stats.stackexchange.com/questions/139796/autocorrelation-and-partial-correlation-plots-in-arma-models)

## 2. Random walk (RW)

### 2.1. Definition

An $\text{RW}$ time series sequence $\{X_t\}$ is the time $t$'s coordinates of a particle that is doing random walk. The displacement vector at each unit time interval is an iid, denoted as $V_t\sim IID(0,\sigma_v^2)$.

$$\begin{aligned}
    X_t &= \sum^t_{q=1} V_q \\
    & = X_{t-1} + V_t \\
    & = X_{t-\tau} + \sum^t_{q=t-\tau+1}V_q
\end{aligned}$$

### 2.2. Mean and variance:

- Mean: $\mathbb{E}(X_t) = \sum_q \mathbb{V_q} = 0$
- Variance: $\text{Var}(X_t) = t\sigma_v^2$

### 2.3. Autocorrelation

Ref: [PSU-Sta4853-P29](http://www.personal.psu.edu/asb17/old/sta4853/files/sta4853-2.pdf), [Uc3m-Slide-P18](http://www.est.uc3m.es/amalonso/esp/TSAtema5petten.pdf)

- AutoCovariance: 
    $$K_{XX}(t,s)\text{Cov}(X_t, X_s) = \text{Cov}(\sum^t_{i=1} V_i ,\sum^s_{j=1} V_j ) = \min(t,s)\sigma_v^2$$
- Correlation (assume $T>>\tau$):
    $$\begin{aligned}
        \rho_{XX}(\tau):&=K_{XX}(T,T+\tau) = \frac{t\sigma_v^2}{\sqrt{T \sigma_v^2}\sqrt{(T+\tau) \sigma_v^2}}\\
        & = \sqrt{1-\frac{\tau}{T+\tau}}\\
        & \sim 1-\frac{\tau}{2T}
    \end{aligned}
    $$
    - Note: define $f(x) = \sqrt{x}$
    $$\lim_{\Delta x \rightarrow 0} \sqrt{1+\Delta x} = f(1)+\partial_x f(x)|_{x=1} \Delta x = 1+\frac{1}{2}\Delta x$$

So:  

- In ideal case ($T\rightarrow\infty$):  $\rho_{XX}(\tau) = 1$
  - I.e. ACF is a constant: 1.
- For real life data: $\rho_{XX}(\tau) = 1-\tau\frac{1}{2T} = 1-\tau \cdot \text{const}$
  - I.e. ACF decreases **linearly**.

Ideal case:

<div align="center"><img src=./time_series_asset/rn_plot_and_acf.png style = "zoom:50%"></div>

For real data:

<div align="center"><img src=https://cdn.analyticsvidhya.com/wp-content/uploads/2017/04/10062952/TS8.png style = "zoom:80%"></div>

Fig.Ref: [analyticsvidhya-blog-Q36](https://www.analyticsvidhya.com/blog/2017/04/40-questions-on-time-series-solution-skillpower-time-series-datafest-2017/)

## 3. Moving average (MA) process

### 3.1. Definition

An $\text{MA}(Q)$ time series sequence $\{X_t\}$ is a weighted average of most recent $Q+1$ white noise values (current value + previous $Q$ values). The white noise values at each time $t$ is denoted as $Z_t \sim WN(0,\sigma_z^2)$

$$\begin{aligned}
X_t &= Z_t + \sum^Q_{q=1}\theta_qZ_{t-q}\\
&=(1+\sum^Q_{q=1}\theta_qB^q)Z_t\\
&=(\sum^Q_{q=0}\theta_qB^q)Z_t
\end{aligned}$$

where $B$ is is the backshift operator as $BZ_t = Z_{t-1}$ and $\theta_0:=1$

### 3.2. Mean and Variance


$\mathbb{E}(X_t) = 0$
$\mathrm{Var}(X_t) = \sigma_z^2 \sum^Q_{q=0}\theta_q^2 $

Ref: [Bookdown](https://bookdown.org/gary_a_napier/time_series_lecture_notes/ChapterFour.html)

### 3.3. Autocorrelation

For $\text{MA}(Q)$ process, the $Q$ can be inferred from the ACF: When lag $\tau>Q$, the ACF $\rho_{XX}(\tau)$ will be 0:

$$\rho_{XX}(\tau) = \begin{cases} 1 & \tau =0\\
   \frac{\sum^{Q-\tau}_{q=0} \theta_q\theta_{q+\tau}}{\sum^Q_{q=0} \theta^2_q } \in [0,1] & 1 \leq \tau \leq Q\\
    0 & \tau>Q
\end{cases}$$


Ref: [Bookdown](https://bookdown.org/gary_a_napier/time_series_lecture_notes/ChapterFour.html)

<div align="center"><img src=./time_series_asset/ma1_plot_and_acf_positive.png style = "zoom:50%"></div>

<div align="center"><img src=./time_series_asset/ma1_plot_and_acf_negative.png style = "zoom:50%"></div>

## 4. AutoRegressive (AR) process

### 4.1. Definition

An $\text{AR}(P)$ time series sequence $\{X_t\}$ is a weighted average of most recent $p$ time series values $\{X_{t-p}\}$ plus current white noise $Z_t \sim WN(0,\sigma_z^2)$.

$$\begin{aligned}
X_t &=  \sum^P_{p=1}\phi_pX_{t-p} + Z_t\\
(1-\sum^P_{p=1}\phi_pB^p)X_t&=Z_t
\end{aligned}$$


### 4.2. Mean and Variance


$\mathbb{E}(X_t) = 0$
$\mathrm{Var}(X_t) = \sigma_z^2 + \sum^P_{p=1}\theta_p K_{XX}(p) $

Ref: [Bookdown](https://bookdown.org/gary_a_napier/time_series_lecture_notes/ChapterThree.html)

#### 2.4.3. Autocorrelation

Unlike MA process, the $P$ hyperparameter of an $\text{AR}(P)$ process CANNOT be inferred with ACF.

$ K_{XX}(\tau) = \sum^P_{p=1}\phi_p K_{XX}(\tau-p)$

$\rho_{XX}(\tau) = \sum^P_{p=1}\phi_p\rho_{XX}(\tau-p)$


Ref: [Bookdown](https://bookdown.org/gary_a_napier/time_series_lecture_notes/ChapterThree.html)

E.g. For $\text{AR}(1)$: $\rho_{XX}(0)=1$, $\rho_{XX}(1)=\phi_1$, $\rho_{XX}(2)=\phi^2_1$, ...,  $\rho_{XX}(h)=\phi^h_1$

<div align="center"><img src=./time_series_asset/ar1_plot_and_acf_positive.png style = "zoom:50%"></div>

<div align="center"><img src=./time_series_asset/ar1_plot_and_acf_negative.png style = "zoom:50%"></div>

### 4.3. Partial autocorrelation

For $\text{AR}(P)$ process, the $P$ is usually inferred from the partial autocorrelation (PACF) $\pi_{XX}$, as:

$$\pi_{XX}(\tau) = \begin{cases} positive & \tau \leq P\\
    0 & \tau>P
\end{cases}$$

Ref: [Bookdown](https://bookdown.org/gary_a_napier/time_series_lecture_notes/ChapterThree.html)


### 4.4. AR/MA duality/invertibility

AR and MA process can be converted to each other. Usually, a finite $\text{AR}(P)$ process can be converted to a infinite $\text{MA}(\infty)$ process. A finite $\text{MA}(Q)$ process can be converted to infinite $\text{MA}(\infty)$ process. Also, $\text{ARIMA}{P,Q}$ can be converted to  infinite $\text{MA}(\infty)$ or $\text{MA}(\infty)$ process. This is called "AR/MA duality" or invertibility Ref: [UToronto-Sta457](https://mcs.utm.utoronto.ca/~nosedal/sta457/duality-of-ma-and-ar.pdf), [StackExchange](https://stats.stackexchange.com/questions/387715/converting-ma1-to-arp), [BerkeleySlide](https://www.stat.berkeley.edu/~bartlett/courses/153-fall2010/lectures/5.pdf)

- E.g. $\text{MA}(1) \rightarrow \text{AR}(\infty)$ 
  - Analytic method: [PSU-Stat510-Lesson2](https://online.stat.psu.edu/stat510/lesson/2/2.1)
  - Backshift operator method: [BerkeleySlide: P16](https://www.stat.berkeley.edu/~bartlett/courses/153-fall2010/lectures/5.pdf) or [CourseHero-Slides: NYU-Lecture3](https://www.coursehero.com/sitemap/schools/1602-New-York-University/courses/1788428-COURANTG632707/)
- E.g. $\text{AR}(2) \rightarrow \text{MA}(\infty)$ [StackExchange](https://stats.stackexchange.com/questions/117519/how-to-write-an-ar2-stationary-process-in-the-wold-representation)
  - The MA representation of a time series seems also called [wold representation](https://en.wikipedia.org/wiki/Wold%27s_theorem). (TBD)
- E.g. $\text{ARIMA}(1,1) \rightarrow MA(\infty)$: [StackExchange](https://stats.stackexchange.com/questions/117619/wold-representation-for-an-arma-1-1)


**Note:** The invertibility has requirement on the parameter value. I.e. in some cases, we cannot convert between MA and AR process. E.g.ref: [RealStatisticsBlog](https://www.real-statistics.com/time-series-analysis/moving-average-processes/invertibility-ma-processes/)

## 5 ARIMA


### 5.1. ARIMA(P,Q): stationary process

An $\text{ARIMA}(P,Q)$ process is like a combination of $\text{AR}(P)$ and $\text{MA}(Q)$ process, as:


$$\begin{aligned}
X_t &=  \sum^P_{p=1}\phi_pX_{t-p} + Z_t + \sum^Q_{q=1}\theta_qZ_{t-q}\\
(1-\sum^P_{p=1}\phi_pB^p)X_t&=(\sum^Q_{q=0}\theta_qB^q)Z_t\\
\Phi(B)X_t &= \Theta(B)Z_t
\end{aligned}$$

Note: $Z_t \sim WN(0,\sigma_z^2)$.





### 5.2. ARIMA(P,D,Q): integrated process

The “I” in ARIMA stands for integrated process. If $D$ is not zero, it means the process is an integrated process.

#### 5.2.1. Integrated process: origin of D


**Operational interpretation:** If after $D$ times of differentiation, the process become stationary, we say the process is **integrated of order $D$**. The process is denoted as $I(D)$. 


**Functional interpretation:**: For ARIMA, if $D$ is not zero, the MA part can be decomposed with a factor $(1-B)^D$, as:

$$\begin{aligned}
    &\Phi(B)X_t = \Theta(B)Z_t\\
    =&(1-\sum^P_{p=1}\phi_pB^p)X_t\\
    \underset{D\neq 0}{=}&(1-\sum^{P-d}_{p=1}\lambda_pB^p)(1-B)^2X_t\\
    =&\Lambda(B)(1-B)^D X_t\\
\end{aligned}$$

Note:
  - $Z_t \sim \text{WN}(0,\sigma_z^2)$
  - An $\text{ARIMA}(P,D,Q)$ process is an $I(D)$ process。

**Stationarity:** Integrated processes are **non**-stationary. A stationary process always have $D=0$. In other words, stationary process is always $I(0)$, i.e. $\text{ARIMA}(P,0,Q)$. Ref: [Uc3m-Slide, P3, P10](http://www.est.uc3m.es/amalonso/esp/TSAtema5petten.pdf)



#### 5.2.2 ARIMA(0,1,0) and RW

$\text{ARIMA}(0,1,0)$ is a special case of $\text{Random walk (RW)}$ process where each step of walk $V_t \sim \text{WN}(0,\sigma^2_z)$:
- $\text{ARIMA}(0,1,0): (1-B)X_t=Z_t \Rightarrow X_t = X_{t-1}+Z_t$, $Z_t \sim \text{WN}(0,\sigma_z^2)$
- $\text{RW}:  X_t = \sum_q V_q = X_{t-1}+V_t$, $V_q \sim \text{IID}(0,\sigma^2_v)$

Note: $I(1)$ vs $\text{RW}$
  - $\text{RW} = \text{ARIMA}(0,1,0)$
  - $I(1) =  \text{ARIMA}(P,1,Q)$
  - $\text{RW}$ is a special case of $I(1)$
  - Ref: [StackExchange](https://stats.stackexchange.com/questions/141812/difference-between-random-walk-and-process-integrated-of-order-one), [Bookdown-Chp5](https://bookdown.org/gary_a_napier/time_series_lecture_notes/ChapterFive.html)

#### 5.2.3 ARIMA(0,1,0) vs AR(1)

Ref: [Uc3m-Slide-P4](http://www.est.uc3m.es/amalonso/esp/TSAtema5petten.pdf), Sec 2.3. above, [StackExchange](https://stats.stackexchange.com/questions/310353/how-to-interpret-arima0-1-0)

**Interpretation:** ARIMA(0,1,0) is a **"non-stationary version"** of AR(1) i.e. ARIMA(1,0,0) process, as:

- $\text{ARIMA}(0,1,0): X_t = X_{t-1}+Z_t$
  - is a $\text{RW}$ process
- $\text{AR}(1): X_t = \phi_1X_{t-1}+Z_t$
- For both: $Z_t \sim \text{WN}(0,\sigma_z^2)$

**ACF plot:** For real data, ACF of **integrated process** $I(D)$ decrease **linearly**,  ACF of $\text{AR}(Q)$ decrease geometrically, e.g.:

- $\text{ARIMA}(0,1,0) \text{ is RW}$:  
  - Ideally: $\rho_{XX}(\tau) = 1$
  - For real data: $\rho_{XX}(\tau) = 1-\tau\frac{1}{2T} = 1-\tau \cdot \text{const}$
- For $\text{AR}(1)$: $\rho_{XX}(\tau)=\phi^\tau$

#### 5.2.4 ARIMA(0,1,0) and ARIMA(0,0,0)

Ref: [StackExchange, See question.](https://stats.stackexchange.com/questions/310353/how-to-interpret-arima0-1-0  )

$\text{ARIMA}(0,0,0)$ is white noise: 
$$X_t=Z_t \sim \text{WN}(0,\sigma_z^2)$$

$\text{ARIMA}(0,1,0): X_t = X_{t-1}+Z_t = \sum^t_{q=1}Z_q$

Thus, $\text{ARIMA}(0,1,0)$ is a can be understand as a sum (integration) of the $\text{ARIMA}(0,0,0)$ process. This is consistent with that differentiation will make integrated process stationary.


### 5.3 ARIMA modeling

#### 5.3.1. Typical process

1. Try to make signal stationary by eliminating trend and seasonal signal.
   - Detrend
   - Deseasonalize
2. Check if the data is stationary, if not, differentiate the data $D$ times to make it stationary, determine the rough range of $D$.
3. Use ACF and PACF plot to determine the rough range of $P$ and $Q$.
4. Calculate the likelihood of the data for different (p,q) or (p,d,q) pairs.
   - (p,q) for differentiated data or $(p,d,q)$ for un-differentiated data.
5. Check whether the residue is white noise.


**Note:**

- We use MLE two times:
   - For each $(p,q)$ pair, we use MLE to solve coefficients $\{\phi_p,\theta_q\}$
   - When determine the best $(P,Q)$ value, we will see their coefficient-optimized likelihood, and take the largest one. 
     - To prevent overfitting, we may also use complexity-penalized likelihood, like AIC or BIC.

- Q: How to determine if your time series data is stationary or not?
  - Use hypothesis testing to examine the mean and autocovariance value of the data. E.g. Dickey-Fuller test) Ref: [UIC-Chp4](https://ademos.people.uic.edu/Chapter23.html#41_determining_stationarity_with_an_augmented_dickey-fuller_test)

#### 5.3.2. Likelihood （TBD）

Typically, we will assume $\{X_t\}$ is a stationary Gaussian process (i.e. error/residue is normally distributed), then, we can represent likelihood of the observation $\{X_t\}$ under ARIMA model with the Multivariate Normal Distribution function, as:

$$f(X) = (\frac{1}{\sqrt{2\pi} })^{n} \cdot \sqrt{\det\Sigma}  \cdot \exp \left[ -\frac{1}{2} (X-\mu)^T \Sigma^{-1} (X-\mu) \right] $$

Ref: [CourseHero-Slides: NYU-Lec4](https://www.coursehero.com/sitemap/schools/1602-New-York-University/courses/1788428-COURANTG632707/)


#### 5.3.3 How to prevent overfitting?

Penalize the model complexity with metrics like AIC, BIC

- $\text{AIC} = -2\cdot \ln (p(\mathcal{S})) + 2K$
- $\text{AIC} = -2\cdot \ln (p(\mathcal{S})) + \ln(m)K$
- Notation:
  - $p(\mathcal{S})$: likelihood of the time series data
  - $K$: degree of freedom of model
  - $m$: length of the time series data (number of samples)

#### 5.3.4. Cross validation for time series

Forward Chaining Cross Validation:

- fold 1 : training [1], test [2]
- fold 2 : training [1 2], test [3]
- fold 3 : training [1 2 3], test [4]
- fold 4 : training [1 2 3 4], test [5]
- fold 5 : training [1 2 3 4 5], test [6]

#### 5.3.5. Stationarity of ARIMA solution. (TBD)

**Conclusion:** The solution ($\{\phi_p,\theta_q\}$) of arima model is stationary if **NO** roots $x$ of the equation $\Phi(x)=0$ polynomial lies **on** the boundary of the unit circle.

- The equation is only about the AP part ($\phi_p$ coefficients).
- Inside and outside solutions are fine. 

Deduction Ref: [medium-blog](https://medium.com/analytics-vidhya/a-complete-introduction-to-time-series-analysis-with-r-arma-processes-part-ii-85a6bb5becae), []

E.g. In the following figure, if B point appear in the roots $\Phi(x)=0$, the solution ($\{\phi_p,\theta_q\}$) of arima model is not stationary. While, A and C points are "safe", i.e., no indication of non-stationary.

<div align="center"><img src=https://miro.medium.com/max/1060/1*e4uvI0fXppALdL-BEl96rg.png style = "zoom:60%"></div>



- Q: why some slides says "outside circle", is that related to inverse root? Like:[mcmaster_U-Slide](https://socialsciences.mcmaster.ca/magee/761_762/other%20material/unit%20and%20char%20roots.pdf), [Github-pdf](http://matthieustigler.github.io/Lectures/Lect2ARMA.pdf)
  - Solution is **stationary** if all roots are **not on** the circle
  - Solution is **casual** if all roots are **outside** the circle
  - Ref: [BerkeleySlides-C153-Lec6-P6](https://www.stat.berkeley.edu/~bartlett/courses/153-fall2010/lectures/6.pdf), [BerkeleySlides-C153-Lec5-P25,P27,](https://www.stat.berkeley.edu/~bartlett/courses/153-fall2010/lectures/5.pdf) "Introduction to Time Series and Forecasting" by Brockwell and Davis recommended from [StackExchange](https://stats.stackexchange.com/questions/348047/relation-between-arp-stationarity-and-causality)
  - Additional note:
    - Solutions is invertible if all roots of $\Theta(x) = 0$ are outside of the unit circle
      - Ref: [Youtube-math_et_al](https://www.youtube.com/watch?v=uciHswYSA3k)
- Q: what is causality? (TBD)
  - Ref: [BerkeleySlides-C153-Lec5-P11](https://www.stat.berkeley.edu/~bartlett/courses/153-fall2010/lectures/5.pdf)

- Q: what is invertibility? (TBD)
  - Ref: [Youtube-math_et_al](https://www.youtube.com/watch?v=uciHswYSA3k)

Other Refs:
- [Bauer-Slides](https://www.bauer.uh.edu/rsusmel/phd/ec2-3.pdf): Stationarity, causality, invertibility for arima.


#### 5.3.6. Sum of two independent arima process:

$\{X_t\}$ and $\{Y_t\}$ are two independent ARIMA process:

$$\begin{aligned}
  X_t = \text{ARIMA}(p_1,q_1)\\
  Y_t = \text{ARIMA}(p_2,q_2)
\end{aligned}$$

The process $\{Z_t\}$ have $Z_t = X_t+Y_t$, then $\{Z_t\}$ is also a ARIMA process, as 

$$Z_t = \text{ARIMA}(P,Q)$$

where:

$$\begin{cases}
  P\leq p_1 +p_2\\
  Q\leq \max(p_1+q_2,q_1+p_2)
\end{cases}$$

Ref: [Origin papar: GrangerMorris1976](http://rainbow.ldeo.columbia.edu/~alexeyk/Papers/GrangerMorris1976.pdf), [SUNY_SB-Slides](http://www.ams.sunysb.edu/~zhu/ams586/SUM.pdf)

**Note:** in real application, we useually take "equal", i.e.:
$$\begin{cases}
  P = p_1 +p_2\\
  Q = \max(p_1+q_2,q_1+p_2)
\end{cases}$$
This is because the ARIMA with high order (value) of P,Q incorporate ARIMA with lower P,Q. This is obvious: if you don't need the higher-order term, you can set those $\phi_p, \theta_q$ coefficients to 0, then it is equal to ARIMA model with lower P,Q.

Ref:[NYU-Stern-Slides](http://people.stern.nyu.edu/churvich/Forecasting/Handouts/Chapt3.3.pdf)