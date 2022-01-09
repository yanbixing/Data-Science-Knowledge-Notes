# Coin Flips

## 1. Number of trials to get two consecutive Heads

### 1.0. Basic version

- Ref: 
  - [1p3a]( https://www.1point3acres.com/bbs/thread-808363-1-1.html): Interview Q of L
  - [ZhiHu-blog](https://www.zhihu.com/question/396385155): ref for solution (expectation of three heads).
  - [CSDN](https://blog.csdn.net/weixin_41888257/article/details/107595186): source for markov chain figure.

Assume, after $n$ trials, we can get two consecutive heads, what is the expectation of $n$?


Answer: 6

#### 1.0.1. Solution 1: Logical deduction + definition of expectation

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

#### 1.0.2. Markov chain + definition of expectation

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



## 2. Two person toss coin alternatively

### 2.0 Basics version: win as head appears

- Ref:
  - [ZhiHu](https://www.zhihu.com/question/290055193): Q and Solution
  - [Zigya](https://www.zigya.com/question/VFVGRlRqRXlNRFkxTmprNA==): Solution - Exhaustive
A and B toss a coin alternatively, A starts first, i.e. A, B, A, B, A ....
The one who get head first wins, what is the probability A wins?

Ans: 2/3

#### 2.0.1. Logical deduction + definition of expectation

Assume the answer is $x$, there are two cases depending on the outcome of first trial:

- If **first trial** gives **head**
  - A wins. 
  - I.e. the probability of A win is 1.
- If **first trial** gives **tail**:
  - Then, in the following trials, it is like the sequence is start with B.
  - So the win rate of B is $x$
  - Then the win rate of A is $1-x$

From the above two cases, we can have such equation:

$$\begin{aligned}
  \Pr(\text{A wins}) &= \Pr(\text{A wins}|H)P(H) + \Pr(\text{A wins}|T)P(T)\\
  \Pr(\text{A wins}) &= \Pr(\text{A wins}|H)P(H) + [1-\Pr(\text{B wins}|T)]P(T)\\
  x &=1 \cdot 0.5 + (1-x)\cdot 0.5\\
  1.5x &=1\\
  x &= \frac{2}{3}
\end{aligned}$$


#### 2.0.2. Exhaustive

All the cases A can win are shown as:

$\begin{aligned}
  &H\\
  &TTH\\
  &TTTTH\\
  &TT...TH \leftarrow (n-1) Tails
\end{aligned}$


So 

$\begin{aligned}
  \Pr(\text{A wins}) &= \frac{1}{2} + \left(\frac{1}{2}\right)^3 + \left(\frac{1}{2}\right)^5 + \dots\\
  &= \frac{1}{2} (1 + \left(\frac{1}{4}\right)^1 + \left(\frac{1}{4}\right)^2+\dots)\\
  &= \frac{1}{2} \cdot \frac{1}{1-\frac{1}{4}}= \frac{1}{2} \cdot \frac{4}{3} \\
  &= \frac{2}{3}
\end{aligned}$

Note: $1+...q^n = \frac{1-q^{n+1}}{1-q}$


### 2.1 Advanced version: win as specific sequence appears

Ref:[1p3a](https://www.1point3acres.com/bbs/thread-830062-1-1.html ): Interview Q, M 

Still A, B toss a coil alternatively
Win condition: head appears first, then follow with a tail, i.e. "HT" appears.
Ask for the win rate of A.

#### 2.1.1. Logical deduction + Exhaustive

- If **first trial** gives **head**
  - If there is a head, no difference
  - Once there is a tail, the one toss tail wins
  - So, if A wins, the sequence should be like
  - $HHT, HHHHT,\dots,(H)^{2n}T$ 
    - since the first 1 is confirmed, after the first toss give $H$, the following is: 
    - $HT, HHHT,\dots,(H)^{2n-1}T$ 
- If **first trial** gives **tail**:
  - Tail no help to construct "HT" at all.
  - It is like we start with "B toss the first coin"
  - The win rate of B is x.
  - The win rate of A is (1-x).

So, the equation is like:

$$\begin{aligned}
  \Pr(\text{A wins}) & = \Pr(\text{A wins}|H)P(H) + \Pr(\text{A wins}|H)P(T)\\
  \Pr(\text{A wins}) & = \Pr(\text{A wins}|H)P(H) + [ 1-\Pr(\text{B wins}|H) ]P(T)\\
  x &= \frac{1}{2}( \left(\frac{1}{2}\right)^2 +  \left(\frac{1}{2}\right)^4 + \dots  \left(\frac{1}{2}\right)^{2n}   ) +  \frac{1}{2}(1-x)\\
  x &= \frac{1}{2} \left \{ \left[ \frac{1}{4}  (1+\left(\frac{1}{4}\right)^1 + \dots  \left(\frac{1}{4}\right)^{n-1}   \right] \right\}   + \frac{1}{2}(1-x)\\
  x &= \frac{1}{8} \frac{1}{1-\frac{1}{4}}  + \frac{1}{2}(1-x)\\
  x &= \frac{1}{6}   + \frac{1}{2}-\frac{1}{2}x\\
  \frac{3}{2}x &= \frac{2}{3}\\
  x&= \frac{4}{9}
\end{aligned}$$


### 2.2. Looks similar but different version: different win conditions

Ref: [NowCoder](https://www.nowcoder.com/questionTerminal/e2951684f39c4db69113b881c72e7e05?from=14pdf): Q and solution.

Still A, B toss a coil alternatively

Win condition: 

- when "HT" appears, A wins
- when "TT" appears, B wins

Ask for the win rate of A.

Ans: 3/4

#### 2.2.1. Logical deduction:

Here, one of the trick is that, once H appears, definitely A will wins, we can use "proof by contradiction" to solve this problem.

**Proof by contradiction:**

- H appears:
  - If the following is H: go back to original "H appears state"
  - If the following is T: achieve "HT", A wins
- So, it is impossible to get "TT" once "H" appears.
  - I.e. Once H appears, A definitely wins.

  
So, consider the first two tosses:

- If the first is H: definitely A wins
- If the first is T:
  - If the second is T: B wins
  - If the second is H: definitely A wins

Obviously
- $\Pr(\text{B wins}) = \frac{1}{2} \cdot \frac{1}{2} = \frac{1}{4}$
- $\Pr(\text{A wins}) = \frac{1}{2} + \frac{1}{4} = \frac{3}{4} = 1- \Pr(\text{B wins})$