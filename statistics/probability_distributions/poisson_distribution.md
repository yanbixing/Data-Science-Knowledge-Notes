# Poisson distribution

## 1. Basics

Ref: [Wiki](https://en.wikipedia.org/wiki/Poisson_distribution)



$$P(X=k)=\frac{e^{-\lambda}\lambda^k}{k!}$$

- $P(X=k)$: In unit time-interval/area/batch, the probability that incidence happens exactly $k$ times.
- $\lambda$: In unit time-interval/area/batch, on average, the incidence happens $\lambda$ times.

**Tips for Qs**: 
- you can choose whatever amount time/area/units as your "unit" time-interval/area/batch for convenience.
  - E.g. you can choose $6min, 8h, 233products, 14m^2$ as unit amount, not necessarily $1min, 1h, 100 product,1m^2$.
  - Then, you just need to scale your $\lambda$ to your unit amount. Following this way, it is more convenient/straightforward to use poisson prob function. 
- factorial of zero, i.e., $0! = 1$
- $k$ is integer $>0$, so $P(X \geq k) = 1-\sum^{k-1}_{i=0}P(X=i)$

## 2. TBD

## 3. Applications:

Describe the number of phone calls.

Ref: [StackExchange](https://math.stackexchange.com/questions/1914125/finding-a-distribution-for-phone-calls)

### Q1. Car traffic

Ref: [Zhihu-post](https://zhuanlan.zhihu.com/p/35698275)

On a road, there are 9 cars pass by per hour, calculate the probability that there are exactly 2 cars pass by in one hour.

Ans: 0.5%

Solution:

- For poisson distribution, $k = 2$ and $\lambda = 9$
- $P(X=2) = \frac{e^{-9}9^2}{2!} = 0.005 = 0.5\%$



### Q2. Defective product

Ref: [CSDN-blog](https://blog.csdn.net/xian__xian/article/details/112261123)

In a company the defective rate of product is 0.1%, calculate the probability that there are at least 2 defective products in 1000 products.

Ans: 26.4%

Solution:

- Here, the batch is 1000 product
- The average number of defective product for 1 batch is $\lambda = 1000*0.1\% = 1$
- $P(X>=2) = 1-P(X=0) - P(X=1) = 0.264 = 26.4\%$
  - $P(X=0) = \frac{e^{-1}1^0}{0!} = e^{-1} = 0.367$
  - $P(X=1) = \frac{e^{-1}1^1}{1!} = e^{-1} = 0.367$
  - Note: factorial of zero i.e. $0! = 1$


### Q3. Phone call


Ref: [CSDN-blog](https://blog.csdn.net/xian__xian/article/details/112261123)

On average, in 1 hour, you will receive 2 phone call. Calculate:

- (1) For 10 min, the probability you can receive exactly 1 phone call.
  - ans: 23.9%
- (2) If you need to leave, what is the longest time you can leave when the probability of no phone call over 0.5.
  - ans: 0.346h

Solution:

- (1)
  - Choose the unit time as 10 min, then $\lambda = 2/6 = 1/3$
  - $P(X=1) = \frac{e^{-\frac{1}{3}}(\frac{1}{3})^1}{1!} = e^{-\frac{1}{3}}/3 = 0.239=23.9\%$
- (2)
  - Choose the unit time as t (hour), then $\lambda = 2t$
  - $P(X=0) = \frac{e^{-2t}(2t)^0}{0!} = e^{-2t} >= 0.5 $
    - $\Rightarrow t<=-\frac{\ln 0.5}{2} = 0.346$

### Q4. Num song you like (Real interview Q)

Ref: [1p3a](https://www.1point3acres.com/bbs/thread-792313-1-1.html)

In the prev 8 mins you heard about 2 songs in your favorite, calculate the prob you heard at least one song in your favorite in next 5 mins.

MySolution:
- Use 5 min as the unit time, $\lambda = \frac{2}{8}*5 = 1.25$
- $P(X\geq1) = 1-P(X=0) = 1 - \frac{ e^{-1.25} 1.25^0 }{0!} = 0.713 = 71.3\%$
  - $P(X=K) = \frac{e^{-\lambda} \lambda^k }{k!}$


## Deep Dive 1: Poisson distribution and binomial distribution

Conclusion: A Poisson case can be viewed as a kind of binomial cases, where, during the continuos time interval, we do $\infty$ trials, and obtain $\lambda$ successes.


### DD1.1. Concept explanation

Poisson: the prob of incidence (success) in continuous (unit) time.
$$P(X=K) = \frac{e^{-\lambda} \lambda^k }{k!} = \frac{e^{-\lambda}  }{k!}\lambda^k$$

Binomial: the prob of success (incidence) in $n$ trials.
$$\begin{aligned}
    \Pr(X=k;n,p)=&{\binom {n}{k}}p^{k}(1-p)^{n-k} \\ 
    =& \frac{n!}{k!(n-k)!}p^{k}(1-p)^{n-k}\\
    =& \frac{\frac{n!}{(n-k)!}(1-p)^{n-k}}{k!} p^k
\end{aligned}$$

Poisson describe observation in continuous time, while binomial describe the observation in discrete trial. To link them together, i.e., to bring discrete to continuos, we need to take limit that we keep doing $\infty$ trial during the continuous time.

So, <mark style="background-color:yellow;"><font color="#0000dd">a Poisson case can be viewed as a kind of binomial cases, where, during the continuos time interval, we do $\infty$ trials, and obtain $\lambda$ successes.</font></mark>


Ref: [EmoryU](http://mathcenter.oxford.emory.edu/site/math117/connectingPoissonAndBinomial/)

### DD1.2. Proof: Binomial to Poisson

Ref: [EmoryU](http://mathcenter.oxford.emory.edu/site/math117/connectingPoissonAndBinomial/)

Mathematical expression of above statement:

- Poisson distribution is a limiting case of a Binomial distribution when:
  - The number of trials is large:  $n\rightarrow \infty$
  - The probability of success in each trial is: $p=\frac{\lambda}{n}$


Binomial: 
$$\begin{aligned}
    \Pr(X=k;n,p)=&{\binom {n}{k}}p^{k}(1-p)^{n-k} \\ 
    =& \frac{n!}{k!(n-k)!}p^{k}(1-p)^{n-k}\\
    =& \frac{\frac{n!}{(n-k)!}(1-p)^{n-k}}{k!} p^k\\
    =& \frac{\frac{n!}{(n-k)!}(1-\frac{\lambda}{n})^{n-k}}{k!} (\frac{\lambda}{n})^k\\
    =& \frac{\frac{n!}{(n-k)! n^k} (1+\frac{-\lambda}{n})^{-k}  \cdot  (1+\frac{-\lambda}{n})^{n}   }{k!} \lambda^k\\
    =& \frac{n!}{(n-k)! n^k} \cdot (1+\frac{-\lambda}{n})^{-k} \cdot \frac{   (1+\frac{-\lambda}{n})^{n}   }{k!} \lambda^k
\end{aligned}$$

Here, when $n\rightarrow \infty$ and $k$ is a constant:
- $\because \frac{n!}{(n-k)!} = \prod^{k-1}_i (n-k) \underset{n\to\infty}{\to} n^k$
  - $\therefore \underset{n\to \infty}{\lim}\frac{n!}{(n-k)! n^k} \rightarrow 1$
- $\because k=const$ and $\lambda=const \Rightarrow \underset{n\to \infty}{\lim}\frac{-\lambda}{n}\to 0$ 
  - $\therefore \underset{n\to \infty}{\lim} (1+\frac{-\lambda}{n})^{-k} \to 1^{-k} =1$
- $\because \exp x=\lim _{n\to \infty }\left(1+{\frac {x}{n}}\right)^{n}$
  - $\therefore (1+\frac{-\lambda}{n})^{n} = e^{-\lambda}$
  - Note:
    - $e=\lim _{n\to \infty }\left(1+{\frac {1}{n}}\right)^{n}$
    - ${\displaystyle \exp x=\lim _{n\to \infty }\left(1+{\frac {x}{n}}\right)^{n}.}$
    - Ref: [Wiki-exp](https://en.wikipedia.org/wiki/Exponential_function)

Thus:

$$\begin{aligned}
    \lim _{n\to \infty } \Pr_{binomial}(X=k;n,p)=&\frac{n!}{(n-k)! n^k} \cdot (1+\frac{-\lambda}{n})^{-k} \cdot \frac{   (1+\frac{-\lambda}{n})^{n}   }{k!} \lambda^k\\
    =& 1\cdot 1\cdot \frac{e^{-\lambda}  }{k!}\lambda^k\\
    =&\frac{e^{-\lambda}  }{k!}\lambda^k\\
    =& \Pr_{poisson}(X=k;\lambda)
\end{aligned}
$$



