# Coin Flips

## 1. Number of trials to get two consecutive Heads

Ref: [CSDN](https://blog.csdn.net/weixin_41888257/article/details/107595186)

Assume, after $n$ trials, we can get two consecutive heads, what is the expectation of $n$?


Answer: 6

**Solution 1:** Logical deduction + definition of expectation

Assume the answer is $x$, there are two cases depending on the outcome of first trial:

- If **first trial** gives **tail**:
  - The **probability** of such case is **0.5**
  - In this case, the expected total number of trials is $1+x$
    - Because the first tail no help, so, start from the second trail, the expected number of trials to get two consecutive heads is still $n$, so the **total expected number is $1+x$**
- If **first trial** gives **head**
  - The probability of such case is 0.5 
  - We have two cases for the **second trial**
    - If the second trial is **tail**, then, similar to "first trial give tail" casesthw first two trials no help and we need to restart
      - The **expected number of trial** after "HT" is **$2+x$**
      - The **probability** of "HT" is **0.5*0.5=0.25**.
    - If the second trial is **head**, then the experiment ends
      - The **expected total number** of trial is **$2$**
      - The **probability** of "HH" is **0.5*0.5=0.25**


Follow the definition of expectation, the expectation of variable N the sum of the product of different $N$ cases and corresponding probability, i.e.:

$$E(N) = \underset{\forall n}{\sum} p(n)\cdot n$$

We can get:

$$\begin{aligned}
    & x = 0.5(1+x) + 0.25(2+x) + 0.25\cdot2\\
    & \Rightarrow x = 0.75x+1.5 \Rightarrow x = 6
\end{aligned}$$

**Solution 2:** Markov chain + definition of expectation

The transition between states can be represent with markov chain:

<div  align="center"><img src=./coin_flips_asset/consecutive_heads_markov_chain_illustration.png style = "zoom:30%"></div> 

Denote the expected trials from $iH$ to $2H$ as $E_i$, from the markov chain, we can know

$$\left \{\begin{aligned} 
    E_0 =&  p(1+E_1) + (1-p)(1+E_0) \\
    =& 1+pE_1+(1-p)E_0\\
    E_1 =&  p(1+E_2) + (1-p)(1+E_0) \\
    =& 1+pE_2+(1-p)E_0\\
    E_2 =& 0
\end{aligned} \right .$$
$$\Rightarrow E_0 = 6$$

Ref: [ZhiHu-blog](https://www.zhihu.com/question/396385155): expectation of three heads.


### Related TBD:

- [Zhihu-Q](https://www.zhihu.com/question/443866807): the variance of $n$ to get two consecutive heads?
  - $\mathrm{Var}(N) = \underset{\forall n}{\sum} P(n)[n-E(n)]^2 = \underset{\forall n}{\sum} P(n)[n^2-E(n)^2]$
  - $E(n) = 6$
  - So, we need to know $E(n^2)$
  - How to know that?
  - Ref: [ZhiHu](https://www.zhihu.com/question/443866807/answer/1824441643) - Similar to the logical deduction idea:
    $$\begin{aligned}
        E(x^2) &= 0.5E[(x+1)^2] + 0.25* E[(x+2)^2] + 0.25*4  \\
        E(x^2) &= 0.5 (E(x^2) +2E(x)+1)+ 0.25*  [ E(x^2)+4E(x) + 4 ]+ 0.25*4 \\
        E(x^2) &= (0.5E(x^2) +E(x)+0.5)+ (0.25E(x^2)+E(x) + 1)+ 1 \\
        E(x^2) &= 0.75E(x^2) +2E(x)+2.5\\
        E(x^2) &= 0.75E(x^2) +12+2.5\\
        0.25E(x^2) &= 14.5\\
        E(x^2) &= 58\\
    \end{aligned}$$
  - So $Var(x)  = E(x^2) - [E(x)]^2 = 58 - 36 = 12$
  - Q remaining: how to use markov chain?


<!-- $$\left \{\begin{aligned} 
    V_0^2 =&  p(1+V_1)^2 + (1-p)(1+V_0)^2 \\
    V_1^2 =&  p(1+V_2)^2 + (1-p)(1+V_0)^2 \\
    V_2^2 =& 0
\end{aligned} \right .$$

$$\left \{\begin{aligned} 
    V_0^2 =&  p(1+V_1)^2 + (1-p)(1+V_0)^2 \\
    V_1^2 =&  p + (1-p)(1+V_0)^2 \\
\end{aligned} \right .$$

$$\left \{\begin{aligned} 
    V_0^2 =&  p(1+V_1)^2 + V_1^2 - p \\
    V_1^2 =&  p + (1-p)(1+V_0)^2 \\
\end{aligned} \right .$$

$$\left \{\begin{aligned} 
    V_0^2 =&  0.5(1+V_1)^2 + V_1^2 - 0.5  = \\
    V_1^2 =&  0.5 + 0.5(1+V_0)^2 = 37.61577\\
\end{aligned} \right .$$ -->

