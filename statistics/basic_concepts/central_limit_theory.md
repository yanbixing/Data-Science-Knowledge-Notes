# Central limit theorem


## 1. CLT definition

**Plain English**: 
- No matter what specific distribution the $X$'s distribution has, its sample mean is always normal distribution.)


**Formal**:

- Denote:
  - $\{X_1,X_2,\dots,X_n\}$ are random samples from an i.i.d. (identical independent distribution.) Denote its variable name as X, i.e. $X\sim$ the i.i.d.
  - $\mu^{true}:=E(X)$, i.e. the true mean value of the distribution.
  - $\mathrm{SD(X)}^2:=Var(X)$, i.e. the true variance of the distribution.
  - $\mu^{obs}:= \frac{1}{N}\sum^N_{i=1} X_i$,i.e. the mean value calculated from the sample.
- **CLT**: 
  - when n is very large, 
  - the distribution of **their mean value** $\mu^{obs}$ is a normal distribution around $\mu^{true}$, 
  - with the standard error (of mean value) $\mathrm{SE}(\mu) = \frac{SD(X)}{\sqrt{n}}$. 

**More formally**: 
- the variable $z(\mu^{obs}):=\lim_{n\rightarrow\infty}\frac{\mu^{obs}-\mu^{true}}{\frac{SD}{\sqrt{n}}}$ follow the standard normal distribution (the normal distribution with mean=0 and standard deviation=1). Ref: [Wiki-Normal_distribution](https://en.wikipedia.org/wiki/Normal_distribution)

Ref: [Wiki-CLT](https://en.wikipedia.org/wiki/Central_limit_theorem)


### 1.1. Concept differentiation: "SE" vs "SD"

- SD, standard deviation of sample usually refers to the standard deviation of population, calculated from the variance of population data.
  - $SD  := SD(X)$
- SE, "standard error of sample **mean**" refers to the standard deviation of the **mean value** of a population
  - $SE := SE( E(x) ) := SD(E(x))$

**Plain English:** SE is the SD of mean value, "SD vs SE" is just a habit naming to differentiate/clarify whether it is the SD of an individual sample ( denoted as $SD(X)$ )or SD of the population mean (denoted as $SE(\mu)$)

## 2. Related concepts

### 2.1. Law of large numbers

**Plain English:** when doing a lot of trials, the observed mean value is the expectation.

**Wiki:** The average of the results obtained from a large number of trials should be close to the expected value and tends to become closer to the expected value as more trials are performed. Ref: [Wiki-LLN](https://en.wikipedia.org/wiki/Law_of_large_numbers)

$$ \lim _{n\to \infty }\sum _{i=1}^{n}{\frac {X_{i}}{n}}-{\overline {X}}=0$$

Other: The frequency of event A converge to its expectation when doing a large number of trials. Ref: [Zhihu](https://zhuanlan.zhihu.com/p/87299555)

### 2.2. Chebyshev's inequality

Let X (integrable) be a random variable with finite expected value μ and finite non-zero variance $\sigma^2$. Then for any real number k > 0,

$$\Pr(|X-\mu |\geq k\sigma )\leq {\frac {1}{k^{2}}}$$

Deduction：

$$\Pr(|X-\mu |\geq \varepsilon )\leq {\frac {\sigma^2}{\varepsilon^{2}}}$$

Note: $Var(X) := \sigma^2$, $SD(X):=\sigma$

### 2.3. Other refs (TBD)

[1p3a](https://instant.1point3acres.com/thread/835066): How to understand on large number theory. (How to remember.)

## 3. Application

### Q.3.1. Max num of loads that not overloads.

Ref: [People.com-Post](http://edu.people.com.cn/n/2014/0103/c1053-24014786.html): E.g.2

Each box has 50kg mean, with standard deviation of 5kg. Say the max load of a truck is 5 tons. What is the max number of boxes a truck can load, when the chance of not-overloading is over 0.977? (Given that $\Phi(2) = 0.977$, where $\Phi$ is the standard normal distribution.)

Solution:

- From the Q, we can know $\mu:=E(x)=50$, $SD(x)=5$
- Assume there are $N$ boxes on the truck, then
  - Then, the sample mean is $\mu = 5$, standard deviation of sample mean is $SE(\mu) = \frac{SD(x)}{\sqrt{N}}$
  - Thus, the population (sum) mean is $E(\sum_i x_i) = N\mu =50N$ and the population SE is $SE(\sum_i x_i) =  N\cdot SE(\mu) = \sqrt{N} SD(x) = 5\sqrt{N}$
- From $\Phi(2) = 0.977$, we can know the in $0.977$ chance, the population sum $\sum_i x_i < E(\sum_i x_i) + 2\cdot SE(\sum_i x_i)$
  - So the equation will be $50N + 10\sqrt{N} <= 5000 $
    - N = 100, $50N + 10\sqrt{N} = 5100$ ❌
    - N = 99, $50N + 10\sqrt{N} = 5049.5$ ❌
    - N = 98, $50N + 10\sqrt{N} = 4999.0$ ✅
- So, the final ans is 98.


### Q.3.2. Application of Chebyshev's inequality

Ref: [BaiduWenku](https://wenku.baidu.com/view/6ad25c652dc58bd63186bceb19e8b8f67d1cef0d.html?re=view)

Q1: $\Pr(|X-\mu |\geq \varepsilon )\leq ?$

Ans: $\frac{\sigma^2}{\varepsilon^2}$

Deduction:
- Chebyshev's inequality: $\Pr(|X-\mu |\geq k\sigma )\leq {\frac {1}{k^{2}}}$
- Let $\varepsilon := k\sigma$
- Then: $\Pr(|X-\mu |\geq \varepsilon )\leq \frac {1}{k^{2}} = \frac{\sigma^2}{k^2\sigma^2} = \frac{\sigma^2}{\varepsilon^2}$


Q2: Toss a coin $N$ times, the head appears $n_H$ times, assume the true probability of head is $p_H$, $\forall \varepsilon>0$, $\underset{N\to\infty}{\lim}\Pr(\left| \frac{n_H}{N} - p_H \right| < \varepsilon) =?$

Ans: 1

Deduction: binomial distribution perspective

$$\begin{aligned}
  &\Pr(\left| \frac{n_H}{N} - p_H \right| < \varepsilon)\\
  =&\Pr(\left| n_H - Np_H \right| < N\varepsilon)\\
  =&1-\Pr(\left| n_H - Np_H \right| \geq N\varepsilon)
\end{aligned} $$

- For binomial distribution 
  - $\mu = \mathbb{E}(X) = Np_H$, 
  - $\sigma = \sqrt{Np_Hq_H}$. ($\sigma^2 = Np_Hq_H$, $q_H := q-p_H$)
- If we define $X:=n_H$, $k\sigma := N\varepsilon$
  - $\Pr(\left| n_H - Np_H \right| \geq N\varepsilon) :=\Pr(|X-\mu |\geq k\sigma )\leq {\frac {1}{k^{2}}}$
- Similar to Q1,
  - $\Pr(|X-\mu |\geq N\varepsilon )\leq \frac {1}{k^{2}} = \frac{\sigma^2}{k^2\sigma^2} = \frac{\sigma^2}{N^2\varepsilon^2} = \frac{Np_Hq_H}{N^2\varepsilon} = \frac{pq}{N\varepsilon}$
  - So, $\underset{N\to\infty}{\lim}\Pr(|X-\mu |\geq N\varepsilon ) \leq0$
- So, $\underset{N\to\infty}{\lim}\Pr(\left| \frac{n_H}{N} - p_H \right| < \varepsilon) \geq 1-0=1$


Deduction2: bernoulli trial perspective

Similar to above process: 

$$\Pr(\left| \frac{n_H}{N} - p_H \right| < \varepsilon) = 1-\Pr(\left| \frac{n_H}{N} - p_H \right| \geq \varepsilon)$$

- For Bernoulli trial:

- If we define
  - $X: = \mu_{obs}=\frac{n_H}{N}$ is the observed sample mean, 
  - $\mu : = \mathbb{E}(\mu_{obs}) = \mathbb{E}(B)= p_H$ is the expectation of sample mean.
  - $\sigma := SE(\mu_{obs}) = \frac{SD(B)}{\sqrt{N}} = \sqrt{\frac{pq}{N}}$ is the standard deviation of sample mean.
    - Note: For bernoulli trial, $SD(B) = \sqrt{\mathrm{Var}(B)}=\sqrt{pq}$
- $\Pr(\left| \frac{n_H}{N} - p_H \right| \geq \varepsilon) = \Pr(|X-\mu |\geq \varepsilon) \leq \frac{\sigma^2}{\varepsilon^2}$
  - $\because \underset{N\to\infty}{\lim}\Pr(|\mu_{obs}- \mathbb{E}(\mu_{obs}) |\geq \varepsilon) \leq \frac{{SE(\mu_{obs})}^2}{\varepsilon^2} = \frac{pq}{N\varepsilon^2} \to 0$
- $\therefore \underset{N\to\infty}{\lim}\Pr(\left| \frac{n_H}{N} - p_H \right| < \varepsilon) \geq 1-0=1$

Note: $\Pr \in [0,1]$