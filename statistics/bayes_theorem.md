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

Ref: [Zhihu](https://www.zhihu.com/question/279696846)

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


### 3.2. Seattle's Rain (Advanced)

You have three friends A,B,C each of them have probability $l_{a}$, $l_{b}$, $l_{c}$ to lie to you. **They give you feedback anonymously, i.e. you know the number of rain-or-not answer but don't who give that answer.** If you know only 1 out of three people tell you Seattle is raining, what is the probability that Seattle is raining? 

**Solution:**

- Let define: $Y:=\text{rain-or-good}$, $H:=(\text{ans}_A, \text{ans}_B,\text{ans}_C)$, $X:=\text{ans\_stats} $

  $$\begin{aligned}
  & \Pr[r|X=(2g,1r)] \\ \\ 
  & (\because \Pr(Y|X) = \sum_H \Pr(Y,H|X) \therefore \Downarrow)\\ \\
  =& \sum_H \Pr[r,H|X=(2g,1r)]\\
  =& \Pr[r,H = (r,g,g)|X=(2g,1r)]\\
  &+\Pr[r,H = (g,r,g)|X=(2g,1r)]\\
  &+\Pr[r,H = (g,g,r)|X=(2g,1r)]\\ \\
  & (\because \Pr(Y,H|X) = \Pr(Y|H,X)\Pr(H|X) \therefore \Downarrow)\\ \\
  =& \Pr[r|H = (r,g,g),X=(2g,1r)]\cdot\Pr[H = (r,g,g)|X=(2g,1r)]\\
  &+\Pr[r|H = (g,r,g),X=(2g,1r)]\cdot\Pr[H = (g,r,g)|X=(2g,1r)]\\
  &+\Pr[r|H = (g,g,r),X=(2g,1r)]\cdot\Pr[H = (g,g,r)|X=(2g,1r)]\\
  \end{aligned}$$

- Here: $\Pr[H = (r,g,g)|X=(2g,1r)] = \frac{1}{3}$
  - So,  $\Pr[r|X=(2g,1r)]$ is a weighted sum of different cases
    $$\begin{aligned}
    & \Pr[r|X=(2g,1r)]\\
    =& \Pr\left[r|H = (r,g,g),X=(2g,1r)\right]\cdot\frac{1}{3}\\
    &+\Pr[r|H = (g,r,g),X=(2g,1r)]\cdot\frac{1}{3}\\
    &+\Pr[r|H = (g,g,r),X=(2g,1r)]\cdot\frac{1}{3}\\
    \end{aligned}$$
  - Intuitively, we know, here, $\Pr[r|H = (g,g,r),X=(2g,1r)] = \Pr[r|H = (g,g,r)]$, given $\Pr[X=(2g,1r)|H=(g,g,r)] =  1$
  - The hard point is how to prove it.

- Give $\Pr(Y,X|H) = \Pr(Y,|X,H) \Pr(X|H) = \Pr(X,|Y,H) \Pr(Y|H)$
  - $\Rightarrow \Pr(Y,|X,H) = \frac{ \Pr(X,|Y,H)\Pr(Y|H)}{\Pr(X|H)}$
  - Ref: [StackExchange](https://math.stackexchange.com/questions/1281454/bayes-rule-with-3-variables/1281558) 
  - We have: 
    $$\begin{aligned}
      &\Pr[r|H = (g,g,r),X=(2g,1r)] \\
      =&\Pr[r|X=(2g,1r), H = (g,g,r)] \\
      =&\frac{ \Pr[X|r,H = (g,g,r)] \Pr[r|H = (g,g,r)]  }{ \Pr[X=(2g,1r)|H = (g,g,r)] }\\
      =&\frac{ 1 \cdot \Pr[r|H = (g,g,r)]  }{ 1 }\\
      =& \Pr[r|H = (g,g,r)]
    \end{aligned}$$
- Therefore:
  $$\begin{aligned}
    & \Pr(r|X=(2g,1r))\\
    =& \Pr\left[r|H = (r,g,g)\right]\cdot\frac{1}{3}\\
    &+\Pr[r|H = (g,r,g)]\cdot\frac{1}{3}\\
    &+\Pr[r|H = (g,g,r)]\cdot\frac{1}{3}\\
    \end{aligned}$$

- Similar to above Q3.2, we can have prob for each case and we can have the overall prob, E.g.
  $$\begin{aligned}
    &\Pr[r|H = (g,g,r)]\\
    =&\frac{\Pr[r,H = (g,g,r)]}{\Pr[r,H = (g,g,r)]+\Pr[n,H = (g,g,r)]}\\
    =&\frac{\Pr[H = (g,g,r)|r]\cdot\Pr(r)}{\Pr[H = (g,g,r)|r]\cdot\Pr(r)+\Pr[H = (g,g,r)|g]\cdot\Pr(g)}  
    \\
    =&\frac{l_A l_B t_C\Pr(r)}{l_A l_B t_C\Pr(r)+t_A t_B l_C\Pr(g)} 
  \end{aligned}$$

- Let's say the truth prob is $t_A = t_B = t_C = 2/3$, then
  - $\Pr[r|H = (r,g,g)]=\Pr[r|H = (g,r,g)]=\Pr[r|H = (g,g,r)] = \frac{ \frac{1}{3} \frac{1}{3} \frac{2}{3} \Pr(r) }{\frac{1}{3} \frac{1}{3} \frac{2}{3} \Pr(r)  \frac{2}{3} \frac{2}{3} \frac{1}{3} \Pr(g) } = \frac{\Pr(r)}{\Pr(r) + 2 \Pr(g)}$
  - $\Pr(r|X=(2g,1r)) = \frac{1}{3} (3\cdot\frac{\Pr(r)}{\Pr(r) + 2 \Pr(g)}) = \frac{\Pr(r)}{\Pr(r) + 2 \Pr(g)}$


#### 3.2.Appendix: conditional prob for 3 variables


$$\begin{aligned}
  \Pr(Y|H,X) \Pr(H,X) &=  \Pr(X,Y,H) = \Pr(X|Y,H)\Pr(Y,H)\\
  \Pr(Y|H,X) \Pr(X|H)\Pr(H) & = \Pr(X|Y,H)\Pr(Y|H)P(H)\\
  \Pr(Y|H,X) \Pr(X|H) \cancel{\Pr(H)} & = \Pr(X|Y,H)\Pr(Y|H)\cancel{\Pr(H)}\\
  \Pr(Y|H,X)&=\frac{ \Pr(X|Y,H)\Pr(Y|H)}{\Pr(X|H)}
\end{aligned}$$

Ref: [StackExchange](https://math.stackexchange.com/questions/1281454/bayes-rule-with-3-variables/1281558) 



