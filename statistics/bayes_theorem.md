# Bayes' theorem

## 1. Basics:

$$P(A|B) = \frac{P(A,B)}{P(B)}$$

## 2. Probability density functions (pdf)

### 2.1 Joint pdf

- Form: 
$$f(x,y)$$
- Properties:
  - $f(x,y) \geq 0 \forall (x,y)$
  - $\int\int f(x,y) dx dy =1$


[Ref: bayesball, Sec 6.4](https://bayesball.github.io/BOOK/joint-probability-distributions.html)

### 2.2 Marginal pdf

- Form:
$$f_{X}(x) = \int f(x,y) dy$$

"Marginal" is just the "prior". Ref: [Utah.edu](https://my.eng.utah.edu/~cs5961/Resources/bayes.pdf)

### 2.3 Conditional pdf

- Form:
$$f_{Y|X}(y|X=x) = \frac{f(x,y)}{f_X(x)}$$

Ref [Wiki: discrete v.s. continuos distribution](https://en.wikipedia.org/wiki/Conditional_probability_distribution)

Posterior is a kind of conditional probability

Ref: [StakeExchange](https://math.stackexchange.com/questions/1823669/what-is-the-difference-between-conditional-and-posterior-probability#:~:text=Posterior%20probability%20is%20a%20conditional,the%20parameter%20given%20observed%20X.)


## 3. Application



### 3.1. Seattle's Rain

You have three friends A,B,C each of them have probability $l_{a}$, $l_{b}$, $l_{c}$ to lie to you. If all of them tell you Seattle is raining, what is the probability that Seattle is raining?

**Solution**

Notation: E.g. denote "it is raining when all A,B,C claim it is raining" statement as $\Pr(r|A:r, B:r, C:r)$, denote not raining as "$n$"

Then, 

$$\begin{aligned}
  \Pr(r|A:r, B:r, C:r) =& \frac{\Pr(r, A:r, B:r, C:r) }{\Pr(A:r, B:r, C:r)}\\
   =& \frac{\Pr(r, A:r, B:r, C:r)}{\Pr(r, A:r, B:r, C:r) + \Pr(n, A:r, B:r, C:r)}\\
   = &\frac{\Pr(A:r, B:r, C:r | r)\Pr(r) }{\Pr(A:r, B:r, C:r | r)\Pr(r) + \Pr(A:r, B:r, C:r | n)\Pr(n) }\\
   =& \frac{\Pr(r)(1-l_a)(1-l_b)(1-l_c)  }{\Pr(r)(1-l_a)(1-l_b)(1-l_c)+\Pr(n)l_al_bl_c}
\end{aligned}$$

- if $\Pr(r) = 0.5, l_a=l_b=l_c=1/3$
  - ans = $\frac{ \frac{1}{2}\cdot \frac{8}{27} }{\frac{1}{2}\cdot \frac{8}{27} + \frac{1}{2}\cdot \frac{1}{27}} = \frac{8}{9}$

Ref: [Zhihu]()

**Detailed explanation:**

Interview Q - L, Ref: [1p3a](https://www.1point3acres.com/bbs/thread-711352-1-1.html)

- Use two friends as illustration, there are a lot of different situations:
  - $\Pr(A:r,B:r|r) = (1-l_a)(1-l_b)$
  - $\Pr(A:n,B:r|r) = l_a(1-l_b)$
  - $\Pr(A:r,B:n|r) = (1-l_a)l_b$
  - $\Pr(A:n,B:n|r) = l_al_b$
  - $\Pr(A:r,B:r|n) = l_al_b$
  - $\Pr(A:n,B:r|n) = (1-l_a)l_b$
  - $\Pr(A:r,B:n|n) = l_a(1-l_b)$
  - $\Pr(A:n,B:n|n) = (1-l_a)(1-l_b)$
- We use two of them 
  $$\begin{aligned}
  \Pr(r|A:r, B:r) =& \frac{\Pr(r, A:r, B:r)}{\Pr(A:r, B:r)}\\
   =& \frac{\Pr(r, A:r, B:r)}{\Pr(r, A:r, B:r) + \Pr(n, A:r, B:r)}\\
   = &\frac{\Pr(A:r, B:r | r)\Pr(r) }{\Pr(A:r, B:r| r)\Pr(r) + \Pr(A:r, B:r | n)\Pr(n) }\\
   =& \frac{\Pr(r)(1-l_a)(1-l_b)  }{\Pr(r)(1-l_a)(1-l_b)+\Pr(n)l_al_b}
\end{aligned}$$
