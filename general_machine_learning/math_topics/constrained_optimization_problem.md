# Constrained optimization problem

Ref: [Zhihu](https://zhuanlan.zhihu.com/p/24638007)

## 0. Summary: Mind map from optimization to KKT

In this part, we summarize the logical relations between constrained optimization, Lagrange function, Dual problem and KKT.

- Target: solve constrained optimization problem (primal): $x_P^* = \underset{x\in\Phi}{\argmin}f(x)$
  - Difficulty: solving function with constraints is hard
  - Idea: remove the constraints
  - Result: convert the problem to $x_P^* = \underset{x\in \mathbb{R}^N}{\argmin}\theta_P(x)$, where $\theta_P(\boldsymbol{x}):=\underset{{\boldsymbol{\alpha},~\boldsymbol{\beta};~\beta_j\geq0}}{\max}L(\boldsymbol{x},\boldsymbol{\alpha},\boldsymbol{\beta})$.
    - Denote the solution as $(x^*_P, \alpha^*_P, \beta^*_P)$
    - Denote the optimal value as $p^*=\underset{x\in \mathbb{R}^N}{\min} \theta_P(x) =\underset{x\in \mathbb{R}^N}{\min}[ \underset{{\boldsymbol{\alpha},~\boldsymbol{\beta};~\beta_j\geq0}}{\max}L(\boldsymbol{x},\boldsymbol{\alpha},\boldsymbol{\beta}) ]$
    - Note: $\Phi$ is the is the **domain** of the primal problem, $\Phi \subseteq \mathbb{R}^N$ 
- Target: solve $x^* = \underset{x\in \mathbb{R}^N}{\argmin}\theta_P(x)$
  - Difficulty: $\theta_P(x)$ may be not convex, hard to solve. 
  - Idea: convert the problem to a convex problem
  - Tentative solution: 
    - Introduce dual problem: $\underset{\boldsymbol{\alpha},\boldsymbol{\beta};\beta_j\geq0}{ {\text{maximaze }} } \theta_D(\alpha,\beta)$ where $\theta_D(\boldsymbol{\alpha},\boldsymbol{\beta}) = \underset{x\in\Phi}{\min} L(\boldsymbol{x},\boldsymbol{\alpha},\boldsymbol{\beta})$
      - Denote the solution as $(x^*_D, \alpha^*_D, \beta^*_D)$. 
      - Denote the optimal value as $d^* = \underset{\boldsymbol{\alpha},\boldsymbol{\beta};\beta_j\geq0}{ {\max} } \theta_D(\alpha,\beta) = \underset{\boldsymbol{\alpha},\boldsymbol{\beta};\beta_j\geq0}{ {\max} } [ \underset{x\in\Phi}{\min} L(\boldsymbol{x},\boldsymbol{\alpha},\boldsymbol{\beta}) ]$
    - Dual problem is always concave even the primal is not convex, easier to solve. 
    - However, the problem is the solution $(x^*_D, \alpha^*_D, \beta^*_D)$ is usually not same with $(x^*_P, \alpha^*_P, \beta^*_P)$ i.e. the solution to dual is usually not the solution to primal.
- Question: when the solution to dual is also the solution to primal, i.e. $(x^*_D, \alpha^*_D, \beta^*_D)$ = $(x^*_P, \alpha^*_P, \beta^*_P)$?
  - Answer: Strong duality. i.e. $p^*=d^*$
  - Quick prove: $L(\boldsymbol{x_P^*},\boldsymbol{\alpha_P^*},\boldsymbol{\beta_P^*}) = p^* =d^* = L(\boldsymbol{x_D^*},\boldsymbol{\alpha_D^*},\boldsymbol{\beta_D^*}) \Rightarrow (\boldsymbol{x_P^*},\boldsymbol{\alpha_P^*},\boldsymbol{\beta_P^*}) = (\boldsymbol{x_D^*},\boldsymbol{\alpha_D^*},\boldsymbol{\beta_D^*})$
- Question: When strong duality holds?
  - Answer: In a lot of case strong duality holds. One of them (the most common used) is: (1) The primal problem is convex + (2) The Slater's condition is satisfied.
- Question: How solve the problem when the above two condition holds? [i.e.(1) The primal problem is convex + (2) The Slater's condition is satisfied]
  - Answer: Solve equations in KKT conditions.
  - Quick prove: When (1) The primal problem is convex + (2) The Slater's condition is satisfied, we have:
    - $x^*$ is the solution for primal problem. 
    - $\Leftrightarrow$ $\forall j,\exist \beta_j\geq0$ that $(x^*, \alpha^*,\beta^*)$ is the saddle point of the $L(x,\alpha,\beta)$
    - $\Leftrightarrow$ $(x^*,\alpha^*,\beta^*)$  satisfy KKT conditions
- Note: there many ways to solve the primal, the above map is the most widely applied one.

## 1. Constrained optimization problem

Generally, a constrained optimization problem can be expressed as:
$$\begin{aligned}
{\text{minimize }} & f(x) \\
{\text{subject to }} & g_i(x) \leq 0 {\text{ for }} i=1,...,I \\
& h_j(x) = 0 {\text{ for }} j=1,...,J
\end{aligned}$$

### 1.1. Geometric Understanding on constraints

For $x \in \mathbb{R}^N$ 
- The equality constraints like f(x)=0, the solution is a $n-1$ dim hyperplane.
- The inequality constraints like $f(x)<0$, the solution is a subspace in the $\mathbb{R}^d$ space.

### 1.2. Another view of the constrained optimization problem

From the above discussion, we can see the effect of constraints is to limit the solution into a certain subspace. 

Thus, we can use the mathematical concept "**domain**" to express the problem in simpler format. **Domain** is the "feasible solution area" of a problem, i.e. the subspace where $x$ can satisfy all the constraints Ref: [Wiki](https://en.wikipedia.org/wiki/Domain_of_a_function).

Denote *the **domain** of the problem* as $\Phi$, i.e.: 

$$x\in\Phi \Leftrightarrow g_i(x) \leq 0 {\text{ and }} h_j(x) = 0 {\text{ }} \forall i,j$$

Then, the problem can be expressed as:

$${\text{minimize }} f(x) {\text{ for }} x\in\Phi$$

## 2. Convert the constrained optimization to unconstrained optimization

The difficulty in solving constrained optimization problem is the constraints. If we can convert it to an unconstrained problem, it is easier to solve.

### 2.1. Design principle for the unconstrained problem.

From the above expression, i.e. 
$${\text{minimize }} f(x) {\text{ for }} x\in\Phi$$

The problem can be understood as "minimize the function $f(x)$ in the feasible subspace $\Phi$, $\Phi\subseteq \mathbb{R}^N$". Then, if we can construct a function $\theta_P(x)$ ($_P$ for "Primal") equal to $f(x)$ in the "feasible area" and have positive infinity value in the "infeasible area", i.e.:

$$\theta_P(x)=\left\{ \begin{array}{ll} f(x) & x\in\Phi\\ +\infty & \textrm{otherwise} \end{array} \right.$$
Then, *"minimize $f(x)$ in the feasible subspace $\Phi$"* is equivalent to *"minimize the $\theta_P(x)$ over the whole space $\mathbb{R}^N$"*, i.e.:

$${\text{minimize }} f(x) {\text{ for }} x\in\Phi \Leftrightarrow {\text{minimize }} \theta_P(\boldsymbol{x})$$

I.e. The constrained optimization problem is converted into an unconstrained optimization problem.

### 2.2. Validate a $\theta_P(x)$ constructed with Lagrangian function

There may be infinite ways to construction $\theta_P(x)$, here we validate one of the $\theta_P(x)$ expression constructed with Lagrangian function, as following:

$$\theta_P(\boldsymbol{x}):=\max_{\boldsymbol{\alpha},~\boldsymbol{\beta};~\beta_j\geq0}L(\boldsymbol{x},\boldsymbol{\alpha},\boldsymbol{\beta}) \tag{Eq. 2.2-1}$$

$$L(\boldsymbol{x},\boldsymbol{\alpha},\boldsymbol{\beta})=f(\boldsymbol{x})+\sum_{i=1}^m\alpha_i h_i(\boldsymbol{x})+\sum_{j=1}^n\beta_j g_j(\boldsymbol{x}) \tag{Eq. 2.2-2}$$

where:
- $\boldsymbol{\alpha}:=[\alpha_1,\alpha_2,\ldots, \alpha_m]^T,~\boldsymbol{\beta}:=[\beta_1,\beta_2,\ldots, \beta_n]^T$

Now, we validate our $\theta_P(x)$:
- Note the max is for $\boldsymbol{\alpha}, \boldsymbol{\beta}$, not relavent to $\boldsymbol{x}$, then:
$$\theta_P(\boldsymbol{x}) = f(x)+ \max_{\boldsymbol{\alpha},~\boldsymbol{\beta};~\beta_j\geq0} \left[ \sum_{i=1}^m\alpha_i h_i(\boldsymbol{x})+\sum_{j=1}^n\beta_j g_j(\boldsymbol{x}) \right]$$
- In the feasible area, i.e. $x\in\Phi$, we have $h_i(x) = 0$ and $g_j(x) \leq 0$. And, note, under max sign, we define $\beta_j \geq 0$. So we always have
$$\begin{aligned}
  & \sum_{i=1}^m\alpha_i h_i(\boldsymbol{x})+\sum_{j=1}^n\beta_j g_j(\boldsymbol{x}) \leq 0 \\
  \Rightarrow & \max_{\boldsymbol{\alpha},~\boldsymbol{\beta};~\beta_j\geq0} \left[ \sum_{i=1}^m\alpha_i h_i(\boldsymbol{x})+\sum_{j=1}^n\beta_j g_j(\boldsymbol{x}) \right] = 0 \\
  \Rightarrow & \theta_P(\boldsymbol{x}) = f(x)
\end{aligned}$$
- In the infeasible area, i.e. $x\notin\Phi$, we either have $h_i(x) \neq 0$ or $g_j(x) > 0$. And, note, under max sign, we define $\beta_j \geq 0$. By assigning corresponding $\alpha_i \rightarrow \pm \infty$ or $\beta_i \rightarrow + \infty$, we can have:
$$
\begin{aligned}
  & \max_{\boldsymbol{\alpha},~\boldsymbol{\beta};~\beta_j\geq0} \left[ \sum_{i=1}^m\alpha_i h_i(\boldsymbol{x})+\sum_{j=1}^n\beta_j g_j(\boldsymbol{x}) \right] \rightarrow +\infty \\
  \Rightarrow & \theta_P(\boldsymbol{x}) \rightarrow +\infty
\end{aligned} 
$$
- In sum, for $\theta_P(x)$
$$\theta_P(x)=\left\{ \begin{array}{ll} f(x) & x\in\Phi\\ +\infty & x\notin\Phi \end{array} \right. \tag{Eq. 2.2-3}$$
- Then:
$${\text{minimize }} f(x) {\text{ for }} x\in\Phi \Leftrightarrow {\text{minimize }} \theta_P(\boldsymbol{x}) \tag{Eq. 2.2-4}$$

## 3. Dual Problem

**Duality**/**duality principle** is a concept in optimization theory.

**Duality** means optimization problems may be viewed from either of two perspectives. The primal and the dual. Typically, dual problem provides a lower bound to the solution of the primal (minimization) problem.

Their difference is called the duality gap. Typically, for convex optimization problems, the duality gap could be zero under specific constraint qualification conditions.

### 3.0. Summary: Why to introduce dual problem (To be Refined)

In short, primal problem may be not convex, hard to solve; but dual problem is concave no matter whether the primal is convex or not, easier to solve. Specifically:

- Denote:
  - $(x_{p^*},\alpha_{p^*},\beta_{p^*})$ as the solution of primal problem, i.e.: $L(x_{p^*},\alpha_{p^*},\beta_{p^*}) = p^*$
  - $(x_{d^*},\alpha_{d^*},\beta_{d^*})$ as the solution of dual problem, i.e.: $L(x_{d^*},\alpha_{d^*},\beta_{d^*}) = d^*$
- We want to solve the primal problem, get $(x_{p^*},\alpha_{p^*},\beta_{p^*})$
- But the primal problem may be not convex, hard to solve.
- Given that:
  - The dual problem is always concave (convex), no matter whether the primal is convex or not.
  - In strong duality, the solution of dual problem is also the solution of the primal problem:
  $$\begin{aligned}
    & d^*=p^* \\
  \Rightarrow & L(x_{d^*},\alpha_{d^*},\beta_{d^*}) = d^* = p^*\\
  \Rightarrow & (x_{p^*},\alpha_{p^*},\beta_{p^*}) = (x_{d^*},\alpha_{d^*},\beta_{d^*}) 
  \end{aligned}$$
- Thus, instead solving primal problem, it is equal to solve dual problem if the strong duality holds.
  - Note: If strong duality doesn't hold, only weak duality holds, the solution to dual problem is not the solution of primal, we only use $d^*$ as a lower boundary of $p^*$ 
- Ref:
  - [StakeExchange: Why is the lagrange dual function concave?](https://math.stackexchange.com/questions/1374399/why-is-the-lagrange-dual-function-concave)
  - [Zhihu: 学习笔记 - 优化问题的对偶性](https://zhuanlan.zhihu.com/p/26890194)
  - [CSDN Blog: 优化问题中的对偶性理论](https://blog.csdn.net/ecnu18918079120/article/details/72971034)
  - [Zhihu: 为什么SVM要用拉格朗日对偶算法来解问题](https://www.zhihu.com/question/300015357/answer/519104070)

### 3.1. Lagrange dual function and Lagrange dual problem

Lagrange dual function:

$$\theta_D(\boldsymbol{\alpha},\boldsymbol{\beta}) = \underset{x\in\Phi}{\min} L(\boldsymbol{x},\boldsymbol{\alpha},\boldsymbol{\beta}) \tag{Eq. 3.1-1}$$

<!-- $$\theta_D(\boldsymbol{\alpha},\boldsymbol{\beta}) = \underset{x\in R^N}{\min} L(\boldsymbol{x},\boldsymbol{\alpha},\boldsymbol{\beta})$$ -->

- $\theta_D$ is called **Lagrange dual function**(*Convex Optimization, Sec 5.1.2, p216*)
- The dual function is always convex even the primal is not. (*Convex Optimization, Sec 5.1.2, p216*) 
  - Why? Ref: [StakeExchange](https://math.stackexchange.com/questions/1374399/why-is-the-lagrange-dual-function-concave)

Lagrange dual problem:

$$\begin{aligned}
{\text{maximize }} & \theta_D(\boldsymbol{\alpha},\boldsymbol{\beta}) \\
{\text{subject to }} & \beta_j \geq 0 {\text{ for }} j=1,...,J 
\end{aligned}\tag{Eq. 3.1-2}$$
<!-- \\ & h_j(x) = 0 {\text{ for }} j=1,...,J -->

- Solving such $\theta_D$ is called **Lagrange dual problem** (*Convex Optimization, Sec 5.2, p223*) 
- the original problem i.e. "minimize $f(x)$ under constraints." is call **primal problem**) (*Convex Optimization, Sec 5.2, p223*)

### 3.2. Optimal values of primal and dual problem

<!-- **Primal problem**: $p^* = \underset{x\in\Phi}{\min} f(x)$ or $p^* = \underset{x\in \mathbb{R}^N}{\min} \theta_P(x;\alpha,\beta)|_{\beta_j\geq0}$ 
**Dual problem**: $d^* = \underset{\beta_j\geq0}{\max} \theta_D(\alpha,\beta)$ -->

Define $p^*$ as the optimal value of the primal problem i.e.:
$$p^* = \underset{x\in\Phi}{\min} f(x) = \min(\theta_P(x)) \tag{Eq. 3.2-1}$$

Define $d^*$ as the optimal value of the dual problem, i.e.:
$$d^* = \underset{\beta_j\geq0}{\max} \theta_D(\alpha,\beta) \tag{Eq. 3.2-2}$$

$p^*$ can also be understood as the highest lower bound of constrained $f(x)$ ($x$ is constrained in the feasible area $\Phi$).

$d^*$ can also be understood as the lowest upper bound of constrained $\theta_D$ ($\beta$ is constrained with $\beta_i>0$)


**Note:**

- The domain (feasible area) for different problem are different
  - For $f(x)$ representation, the domain is $x\in\Phi$ ($\alpha, \beta$ are not introduced.)
  - For $\theta_P$ representation, the domain is free in x i.e. $x\in \mathbb{R}^N$ but constrained on $\beta$ i.e. $\forall j$, $\beta_j\geq0$ ($\beta$ is introduced at the definition of $\theta_P$)
  - For $\theta_D$ representation, the domain requires both $x\in \Phi$ and $\beta_j\geq0$

### 3.3. Relation between $d^*$ and $p^*$

Remember:

$$\begin{aligned}
d^* &= \underset{\beta_j\geq0}{\max} \theta_D(\alpha,\beta)\\
\theta_D(\boldsymbol{\alpha},\boldsymbol{\beta}) &= \underset{x\in\Phi}{\min} L(\boldsymbol{x},\boldsymbol{\alpha},\boldsymbol{\beta})\\
L(\boldsymbol{x},\boldsymbol{\alpha},\boldsymbol{\beta}) & = f(\boldsymbol{x})+\sum_{i=1}^m\alpha_i h_i(\boldsymbol{x})+\sum_{j=1}^n\beta_j g_j(\boldsymbol{x})
\end{aligned}$$

We can deduct:

$$d^*\leq p^*$$

The **deduction** process is shown below:

**Note:** In the **deduction**, we also show that:
- For **dual problem**, without any constraints: $x\in\Phi$ or $\beta_j \geq0$, the above inequality relation does **NOT** holds anymore, also shown in the deduction part.

**Deduction:**

- $x\in\Phi$: $\forall i,j$, $h_i(x)=0$ and $g_j(x)\leq0$
  - $\beta\geq0$: 
    <!-- - $\theta_D(\alpha,\beta) = \underset{x\in\Phi}{\min}[f(x)+\sum_j\beta_jg_j(x)] \leq \underset{x\in\Phi}{\min} f(x)=p^*$
    - $\therefore d^* = \underset{\beta \geq 0}{\max}\theta_D(\alpha,\beta)\leq p^*$ -->
    - $\alpha_ih_i(x) = 0$ and $\beta_jg_j(x) \leq 0$
    - $\Rightarrow f(x)+\sum_i\alpha_ih_i(x)+\sum_j\beta_jg_j(x) = f(x)+\sum_j\beta_jg_j(x)\leq f(x)$
    - $\Rightarrow \theta_D(\alpha,\beta) = \underset{x}{\min}[f(x)+\sum_j\beta_jg_j(x)] \leq \underset{x}{\min} f(x)=p^*$
    - $\Rightarrow d^* = \underset{\alpha,\beta}{\max}\theta_D(\alpha,\beta)\leq p^*$
  - $\beta<0$:
    - $\alpha_ih_i(x) = 0$ and $\beta_jg_j(x) \geq 0$
    - $\Rightarrow f(x)+\sum_i\alpha_ih_i(x)+\sum_j\beta_jg_j(x) = f(x)+\sum_j\beta_jg_j(x)\geq f(x)$
    - $\Rightarrow \theta_D(\alpha,\beta) = \underset{x}{\min}[f(x)+\sum_j\beta_jg_j(x)] \geq \underset{x}{\min} f(x) = p^*$
    - $\Rightarrow d^* = \underset{\alpha,\beta}{\max}\theta_D(\alpha,\beta)\geq p^*$
  - $\Rightarrow$ if no constraints on $\beta$, $d^*$ and $p^*$'s equality relation is not certain.
- $x\not\in\Phi$: $h_i(x)\not = 0$ or $g_j(x)>0$ for at least 1 $i$ or $j$.
  - $\beta\geq0$: 
    <!-- - $\alpha_ih_i(x) \not = 0$ or $\beta_jg_j(x) > 0$ for at least 1 $i$ or $j$ -->
    - Define $x_{\theta_D}(\alpha,\beta):=\underset{x}{\argmin}[f(x)+\sum_i\alpha_ih_i(x)+\sum_j\beta_jg_j(x)]$
    - $d^* = \underset{\alpha,\beta}{\max}[ f(x_{\theta_D})+\sum_i\alpha_ih_i(x_{\theta_D})+\sum_j\beta_jg_j(x_{\theta_D})  ]$
    - Since $x_{\theta_D}\not\in\Phi$, at least 1 of the following must happen:
      - if any $h_i(x_{\theta_D})\not = 0$, by assigning $\alpha_i \rightarrow \pm \infty$, $d^*\rightarrow +\infty$
      - if any $g_j(x_{\theta_D}) > 0$, by assigning $\beta_j \rightarrow + \infty$, $d^*\rightarrow +\infty$
    - $\therefore d^* \rightarrow + \infty$
  - $\beta<0$: 
    - Similarly, $d^* = \underset{\alpha,\beta}{\max}[ f(x_{\theta_D})+\sum_i\alpha_ih_i(x_{\theta_D})+\sum_j\beta_jg_j(x_{\theta_D})  ]$
    - if any $h_i(x_{\theta_D})\not = 0$, by assigning $\alpha_i \rightarrow \pm \infty$, $d^*\rightarrow +\infty$
    - if any $g_j(x_{\theta_D}) < 0$, by assigning $\beta_j \rightarrow - \infty$, $d^*\rightarrow +\infty$
    - if all $g_j(x_{\theta_D}) \geq 0$ and $h_i(x_{\theta_D})=0$:
      - $d^*= \underset{\alpha,\beta}{\max} [ f(x_{\theta_D})+\sum_j\beta_jg_j(x_{\theta_D}) ]$
      - $d^* \rightarrow f(x_{\theta_D})$ at $\beta\rightarrow 0^-$ 

#### 3.3. Appendix. Zhihu's deduction process on $d^* \leq p^*$ is NOT correct.

[Zhihu](https://zhuanlan.zhihu.com/p/24638007) is not correct, for an arbitrary function, We have: $\underset{y}{\min} f(x,y) \leq f(x,y)$, and $\underset{y}{\max} f(x,y) \geq f(x,y)$, this is because:

- $\underset{y}{\min} f(x,y)$ can be view as a function as x, i.e. $g(x)$
- $g(x) \leq f(x,y)$ means: $\forall x$, $g(x)\leq f(x,y) \forall y$ 

But we CAN**NOT** infer that $\underset{x}{\max}[\underset{y}{\min} f(x,y)] \leq f(x,y)$, $\underset{x}{\min}[\underset{y}{\max} f(x,y)] \geq f(x,y)$

- Since, e.g. for arbitrary function $a,b$: $\forall x, a(x) \leq b(x) \not \Rightarrow \underset{x}{\max}[a(x)] \leq b(x)$
- Thus, $\underset{y}{\min} f(x,y) \leq f(x,y) \not \Rightarrow \underset{x}{\max}[\underset{y}{\min} f(x,y)] \leq f(x,y)$

<div  align="center">
  <img src=./constrained_optimization_problem_asset/zhihu_wrong_mimmax_deduction.jpeg style = "zoom:20%">
</div>

<!-- $$\theta_D(\boldsymbol{\alpha},\boldsymbol{\beta}) = \underset{x\in\Phi}{\min} L(\boldsymbol{x},\boldsymbol{\alpha},\boldsymbol{\beta})$$

$$L(\boldsymbol{x},\boldsymbol{\alpha},\boldsymbol{\beta})=f(\boldsymbol{x})+\sum_{i=1}^m\alpha_i h_i(\boldsymbol{x})+\sum_{j=1}^n\beta_j g_j(\boldsymbol{x})$$ -->




## 4. Lagrangian multiplier and KKT condition basics

We have make the problem easier to solve by introducing dual problem, then how to solve? The concept "Lagrangian multiplier" and "KKT condition" is related to solving the constrained optimization problem. 

**Note:** From the below KKT v.s. primal-dual relation deduction, it seems dual problem is not mentioned/used superficially. But, pls note that the slater's theorem is related to duality gap. **Guess:** If we deep dive into the topic, dual problem should be employed there to bridge between "saddle point" and "solution of primal".

<!-- ### 6.1 Constraints
For $x \in \mathbb{R}^N$, The equality constraints like f(x)=0, the solution is a $n-1$ dim hyperplane.

The inequality constraints like $f(x)<0$, the solution is a subspace in the $\mathbb{R}^d$ space. -->

<!-- Minimize $L(\mathcal{S})$ -->

### 4.0. Relation between KKT and primal-dual problem (To Be Refined)

- When Slater's condition holds + primal probem is convex:
  - $x^*$ is the solution for primal problem. 
  - $\Leftrightarrow$ $\forall j,\exist \beta_j\geq0$ that $(x^*, \alpha^*,\beta^*)$ is the saddle point of the $L(x,\alpha,\beta)$
  - $\Leftrightarrow$ $(x^*,\alpha^*,\beta^*)$  satisfy KKT conditions

**Ref:**

- Ref: [CSDN Blog: 优化问题中的对偶性理论](https://blog.csdn.net/ecnu18918079120/article/details/72971034)
  - Strong duality holds $\Leftarrow$ Slater's condition holds + primal problem is convex (**Slater's Theorem**) 
- Ref: [LECTURES ON MODERN CONVEX OPTIMIZATION, D.2.3.1, p626](https://www2.isye.gatech.edu/~nemirovs/LMCO_LN.pdf)
  - p627 (i) $x^*$ is the solution for primal problem $\Leftarrow$ $\forall j,\exist \beta_j\geq0$ that $(x^*, \alpha^*,\beta^*)$ is the saddle point of the $L(x,\alpha,\beta)$
  - p627 (ii) When Slater's condition holds + primal problem is convex:
    - $x^*$ is the solution for primal problem $\Rightarrow$ $\forall j,\exist \beta_j\geq0$ that $(x^*, \alpha^*,\beta^*)$ is the saddle point of the $L(x,\alpha,\beta)$
- Ref: [LECTURES ON MODERN CONVEX OPTIMIZATION, D.2.3.2, p629](https://www2.isye.gatech.edu/~nemirovs/LMCO_LN.pdf), [Wiki - KKT](https://en.wikipedia.org/wiki/Karush%E2%80%93Kuhn%E2%80%93Tucker_conditions)
  - When primal probem is convex:
    - $(x^*,\alpha^*,\beta^*)$ is the saddle point of $L(x,\alpha,\beta)$ $\Leftarrow$ $(x^*,\alpha^*,\beta^*)$  satisfy KKT conditions
  - When Slater's condition holds primal probem is convex:
    - $(x^*,\alpha^*,\beta^*)$ is the saddle point of $L(x,\alpha,\beta)$ $\Rightarrow$ $(x^*,\alpha^*,\beta^*)$  satisfy KKT conditions
<!-- - When problem is convex:
  - $L(X^*)=p^*=d^*$ $\Leftarrow$ $X^*$ satisfy KKT condition + problem is convex
- When Slater's condition holds + primal problem is convex:
  - Strong duality holds $\Leftrightarrow$ KKT
  - **Note:** Acutally, KKT is defined related to Saddle point,  -->
- Other Ref:
  - [StkkeExchange: KKT and Slater's condition](https://math.stackexchange.com/questions/379543/kkt-and-slaters-condition#:~:text=%22If%20a%20convex%20optimization%20problem,so%20x%20is%20optimal%20if)
  - [StkkeExchange: :Is KKT conditions necessary and sufficient for any convex problems?](https://math.stackexchange.com/questions/2513300/is-kkt-conditions-necessary-and-sufficient-for-any-convex-problems)
  - [LECTURES ON MODERN CONVEX OPTIMIZATION, p626](https://www2.isye.gatech.edu/~nemirovs/LMCO_LN.pdf)
    - Note: IC: primal problem, IC* dual problem

### 1.2 Lagrange multiplier method

The target of lagrangian multiplier method is optimization problem that with constraints. 

The purpose of lagrangian multiplier method is to "merge" constraints to objective function to form a new function (Lagrangian). So that we can convert constrained optimization problem to un-constrained problem.

An equality constrained optimization problem can be expressed like:
$$\begin{aligned}
  &{\text{minimize }} f(x)\\
  &{\text{subject to }} g(x) = 0
\end{aligned}\tag{Eq. 1.2-1}
$$

Lagrange multiplier method convert the above equality constrained optimization problem to an unconstrained problem.

"The solution corresponding to the original constrained optimization is always a saddle point of the Lagrangian function" Ref: [Wiki-Lagrange multiplier](https://en.wikipedia.org/wiki/Lagrange_multiplier)

#### 1.2.1 Geometric understanding on equality constrained optimization 

<!-- An equality constrained optimization problem can be expressed like:
$${\text{minimize }} f(x)$$
$${\text{subject to }} g(x) = 0$$ -->
Denote the optimized solution as $x^*$ and corresponding $f(x^*):=f^*$, the problem can be illustrated as:

<div  align="center">
  <img src=./constrained_optimization_problem_asset/equality_constrained_optimization_figure.jpeg style = "zoom:30%">
</div>

In geometric understanding, we have:

- No.1: The gradient at optimized point $\nabla f(x^*)$ must be orthogonal to its contour $f(x) = f^*$.
  - This can be inferred from the definition, the gradient is always normal to contour, or the contour will not have constant value.
- No.2: The gradient of $g(x)$ at the optimized point, i.e. $\nabla g(x^*)$ must be orthogonal to hypersurface $g(x) = 0$
  - Similar to the above reason in No.1, $g(x) = 0$ is also a contour for function $g(x)$, gradient must be normal to contour.

- No.3: The gradient at optimized point (solution $x^*$), i.e. $\nabla f(x^*)$, must be orthogonal (normal/perpendicular) to the hypersurface $g(x)=0$.
  - The solution must lies on the hypersurface $g(x)=0$.
  - If the gradient's tangent projection $\nabla f(x)$ is not zero, then, we can still move on the surface to reduce the value of f(x), the point is not extreme value on the hypersurface.

- The hypersurface $f(x) = f^*$ must be tangent to $g(x) = 0$ (locally, only have 1 intersection point, but at other area, may also have other intersection.)
  - From the above No.2 and No.3 we can know **$\nabla f(x^*)$ and $\nabla g(x^*)$ is in a same line** (i.e. same/opposite direction), then the tangent is in a same line, then the two hypersurface $g(x)=0$ and $f(x)=f^*$ are tangent to each other.

#### 1.2.2 Lagrangian Function for equality constrained optimization

- From "**$\nabla f(x^*)$ and $\nabla g(x^*)$ is in a same line**", we can know, for the solution $x^*$, we have: $\nabla f(x^*)+\lambda \nabla g(x^*) = \vec{0}$
  - Number of variable: n+1. I.e. $\{x^{*(1)},...x^{*(n)}\}$ and $\lambda$ 
  - Number of function: n. I.e. $\partial_{x^{(i)}}f(x^*) + \lambda \partial_{x^{(i)}}g(x^*) = 0$
  - To solve the $x^*$ we need 1 more function.
- From the above No.2, we can know $g(x^*) =0$, thus, from these function, we should be able to solve $x^*$.

From the above two points, we can know:

- Define a new function: $L(x,\lambda) = f(x) + \lambda g(x)$, it's equivalent to solve $\nabla L(x,\lambda) = 0$ i.e.
  - for $i = 1...n$, $\partial_{x^{(i)}}L(x,\lambda) = \partial_{x^{(i)}}f(x) + \lambda \partial_{x^{(i)}}g(x) = 0$
  - $\partial_{\lambda}L(x,\lambda) = g(x) = 0$

Thus, we can explain why Lagrangian objective $L(x,\lambda)$ can convert a constrained optimization problem to an equivalent un-constrained optimization problem.

### 1.3 KKT conditions

KKT conditions extends (generalize) the Lagrange multipliers method on inequality constraints.

"If ${\displaystyle (\mathbf {x} ^{\ast },\mathbf {\mu } ^{\ast })}$ is a saddle point of ${\displaystyle L(\mathbf {x} ,\mathbf {\mu } )}$ in $\mathbf{x} \in \mathbf{X}$, ${\displaystyle \mathbf {\mu } \geq \mathbf {0} }$, then ${\displaystyle \mathbf {x} ^{\ast }}$ is an optimal vector for the above optimization problem." [Wiki-KKT conditions](https://en.wikipedia.org/wiki/Karush%E2%80%93Kuhn%E2%80%93Tucker_conditions)

#### 1.3.1 Geometric understanding and Lagrangian function for inequality constrained optimization
<!-- #### Geometric understanding on inequality constrained optimization  -->
<!-- #### Inequality constraints and KKT conditions -->

An inequality constrained optimization problem can be expressed as 
$$\begin{aligned}
  &{\text{minimize }} f(x)\\
  &{\text{subject to }} g(x) \leq 0
\end{aligned}\tag{Eq. 1.3.1-1}
$$
There two cases for the position of optimal solution $x^*$, as:

<div  align="center">
  <img src=./constrained_optimization_problem_asset/inequality_constrained_optimization_figure.jpeg style = "zoom:30%">
</div>

- (a) extreme f(x) are beyond the constraint area, i.e., the constrained optimal solution $x^*$ appear on the constraint boundary. 
  - In this case, it is equal to replace $g(x) \leq 0$ with $g(x) = 0$
  - Then, same to the equality case: with $g(x) = 0$, we should have: $\nabla f(x^*)+\lambda \nabla g(x^*) = \vec{0}$
  - From the fact that the direction of gradient is towards the larger value. Thus, we can know, the $\nabla f(x^*)$ and $\nabla g(x^*)$ should have opposite direction. Then, in the above equation, we should have $\lambda>0$.
- (b) extreme f(x) appear in the constraint area, i.e., the constrained optimal solution same with the unconstrained optimal solution. 
  - Then, $\nabla f(x^*) = 0$, equivalent to set $\lambda = 0$ for $\nabla f(x^*)+\lambda \nabla g(x^*) = \vec{0}$
- Summarizing the above two cases, for the optimal solution $x^*$, we have:
  - $\nabla f(x^*)+\lambda \nabla g(x^*) = \vec{0}$
  - $\lambda \geq 0$
  - $\lambda g(x) = 0$

## Deep Dive: Saddle point

What is a saddle point.