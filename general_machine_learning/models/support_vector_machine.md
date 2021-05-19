# Support Vector Machine

## 0. Basics

#### 0.0 Notations

- $(\mathbf{x},y)$ the input of a sample
  - $\mathbf{x}$ ($\vec{x}$) is an n-dim vector with $N$ features $\mathbf{x}^{(i)} :=X^{(i)}$ 
- $\mathcal{S}$ denotes the training set with m samples $\{...(\mathbf{x}_i,y_i)...\}$
- $\mathbf{w}$ ($\vec{w}$) is the weight vector aheard in the prediction fucntion
- $\lambda$ the coefficient on the weight regularization, $c:=\frac{1}{2\lambda}$
#### 0.1 Equations

- Prediction (score) function: $f(\mathbf{x}) = \mathbf{w}^T\mathbf{x} - b$
  - Use $-b$ for convenience.
  - Distance between the origin and the hyperplane is $\tfrac {b}{\|\mathbf {w} \|}$. Distance between the two margin hyperplane is $\tfrac {2}{\|\mathbf {w} \|}$ . Ref: [StackExchange](https://math.stackexchange.com/questions/1305925/why-is-the-svm-margin-equal-to-frac2-mathbfw), [Wiki - SVM](https://en.wikipedia.org/wiki/Support-vector_machine#SVM_and_the_hinge_loss) 
  <div  align="center"><img src=https://upload.wikimedia.org/wikipedia/commons/thumb/7/72/SVM_margin.png/617px-SVM_margin.png style = "zoom:40%"></div>
- Margin: $y\cdot f(x)$
- Hinge loss: $l_H(x,y) = \max(0,1-y f(x))$
- Loss function (let $c:=\frac{1}{2\lambda}$): 
$$\begin{aligned}
    l(\mathcal{S},\mathbf{w},b) &= \left[{\frac {1}{m}}\sum _{i=1}^{m}\max \left(0,1-y_{i}(\mathbf {w} ^{T}\mathbf {x}_i -b)\right)\right]+\lambda \|\mathbf {w} \|^{2}\\
    &= \left[{\frac {c}{m}}\sum _{i=1}^{m}\max \left(0,1-y_{i}(\mathbf {w} ^{T}\mathbf {x}_i -b)\right)\right]+\frac{1}{2} \|\mathbf {w} \|^{2}
\end{aligned}$$

#### Training

- Minimize Hinge loss over training sets: $w = \underset{w}{\argmin}[l(\mathbf{w}|\mathcal{S})]$
- Solution: $\vec{w} = \sum^m_{i=1}\alpha_i y_i \vec{x}_i$
  - For points "outside/beyond" margin ($y_if(x_i)>1$), $\alpha_i=0$, make no contribute to model.
  - For points <font color="#0000dd">**on the margin**</font> ($y_if(x_i)=1$), $\alpha_i \in [0,\frac{c}{m}]$ and points <font color="#0000dd">**inside/below the margin**</font> ($y_if(x_i)<1$), $\alpha_i=\frac{c}{m}$, these points will contribute to the model, thus are called <font color="#0000dd">**support vectors**</font>
  - I.e. $w$ vector is a <font color="#0000dd">**linear combination** of </font> the <font color="#0000dd">**support vectors**</font> $\{x_i\}$
  - Ref: [NYU-ML-2019](https://davidrosenberg.github.io/ml2019/#home), [NYU-ML-2019:SVM](https://davidrosenberg.github.io/mlcourse/Archive/2019/Lectures/04b.SVM.pdf)

## D-1. Deep Dive: why margin is $\tfrac {2}{\|\mathbf {w} \|}$?
<div  align="center"><img src=https://upload.wikimedia.org/wikipedia/commons/thumb/7/72/SVM_margin.png/617px-SVM_margin.png style = "zoom:40%"></div>

- Fact: 
  - Gradient is alway normal (orthogonal, perpendicular) to the plane. Ref: [Larmar U](https://tutorial.math.lamar.edu/classes/calciii/gradientvectortangentplane.aspx#:~:text=This%20says%20that%20the%20gradient,the%20surface%20at%20a%20point.&text=This%20is%20a%20much%20more,derived%20in%20the%20previous%20section.)
- Deduction:
  - score function: $f(\vec{x}) = \vec{w}^T\vec{x} - b$
  - then, gradient for f is 
  $$(\partial_{x^{(1)}}f, ...,\partial_{x^{(i)}}f...,\partial_{x^{(n)}}f) = (w_1,...w_i,...w_n) = \vec{w}$$
  - normalize the vector $\vec{v}_p = \frac{\vec{w}}{\|\vec{w}\|}$
  - Origin point distance
    - To measure the distance $d$ from origin to 0-score plane, we can take a vector $\vec{v'}$ from origin point to 0-score plane, the vector's project on $\vec{v}_p$ (i.e. d = $\vec{v'}\cdot\vec{v}_p$) will be the distance from origin to the 0-score plane.
    - The starting point is origin point is $\vec{0}=(0,...,0,...0)$
    - The end will can be any random point on the 0-score plane $\vec{x}_{f=0}$, which satisfies $f(\vec{x}_{f=0}) =0 \Rightarrow \vec{w}\cdot\vec{x}_{f=0} - b = 0$
    - The vector will be $\vec{v'} = \vec{x}_{f=0}-\vec{0} = \vec{x}_{f=0}$, then we have:
    $$\vec{w}\cdot\vec{v'} - b = 0 \Rightarrow \|\vec{w}\|\vec{v}_p\cdot\vec{v'} =b \Rightarrow d=\frac{b}{\|\vec{w}\|}$$
  - Similarly, distance between margin plane:
    - Take any vector $\vec{v'}$ starting from a point on $f(x)=1$ hyperplane to $f(x)=-1$ hyperplane, the distance between two margin have $d=\vec{v'}\cdot\vec{v}_p$
    - Take two points $\vec{x}_{f=-1}$, $\vec{x}_{f=1}$ from the two hyperplane, they will statisfy $f(\vec{x}_{f=-1}) = -1$ and $f(\vec{x}_{f=1}) = 1$, i.e. $\vec{w}\cdot\vec{x}_{f=1} = 1+b$ and $\vec{w}\cdot\vec{x}_{f=-1} = -1+b$
    - $\vec{v'} = \vec{x}_{f=1} - \vec{x}_{f=-1}$ then we have:
    $$\vec{w}\cdot(\vec{x}_{f=1}-\vec{x}_{f=-1}) = 2 \Rightarrow \|\vec{w}\|\vec{v}_p\cdot\vec{v'}=2\Rightarrow d=\frac{2}{\|\vec{w}\|}$$

Ref: [StackExchange](https://math.stackexchange.com/questions/1305925/why-is-the-svm-margin-equal-to-frac2-mathbfw), [Wiki - SVM](https://en.wikipedia.org/wiki/Support-vector_machine#SVM_and_the_hinge_loss) 


Further dive concept: tangent plane function, normal vector.

## D-2. Deep Dive: How to solve SVM?

The SVM problem can be expressed as: Minimize Hinge loss over training sets: $(\mathbf{w},b) = \underset{\mathbf{w},b}{\argmin}[l(\mathcal{w|S})]$ where:
$$\begin{aligned}
    l(\mathcal{\mathbf{w},b|S}) = \left[{\frac {c}{m}}\sum _{i=1}^{m}\max \left(0,1-y_{i}(\mathbf {w} ^{T}\mathbf{x}_i -b)\right)\right]+\frac{1}{2} \|\mathbf {w} \|^{2}
\end{aligned} \tag{Eq. D-2.1}$$

The problem is $l(\mathbf{w},b|\mathcal{S})$ is not differentiable because of $\max$. Thus, if we can remove $\max$, then the problem is easier to solve.

First, we can introduce a slack variable $\mathbf{\xi}$, the problem $\underset{\mathbf{w},b}{ {\text{minimize }} } l(\mathbf{w},b|\mathcal{S})$ i.e.
$$\begin{aligned}
\underset{\mathbf{w},b}{ {\text{minimize }} } \frac{1}{2} \|\mathbf {w} \|^{2} + \left[{\frac {c}{m}}\sum _{i=1}^{m}\max \left(0,1-y_{i}(\mathbf {w} ^{T}\mathbf{x}_i -b)\right)\right]
\end{aligned}$$ 

can be converted (proof is shown in [D.2. Appendix] to: 

$$\begin{aligned}
\underset{\mathbf{w},\mathbf{\xi}}{ {\text{minimize }} } & \frac{1}{2} \|\mathbf {w} \|^{2}+ \left[{\frac {c}{m}}\sum _{i=1}^{m}  \xi_i  \right]\\
{\text{subject to }} & \xi_i\geq \max \left(0,1-y_{i}(\mathbf {w} ^{T}\mathbf{x}_i -b)\right)
\end{aligned}$$ 

further converted to:

$$\begin{aligned}
\underset{\mathbf{w},\mathbf{\xi}}{ {\text{minimize }} } & \frac{1}{2} \|\mathbf {w} \|^{2}+ \left[{\frac {c}{m}}\sum _{i=1}^{m}  \xi_i  \right]\\
{\text{subject to }} & \xi_i \geq 0\\
& \xi_i\geq 1-y_{i}(\mathbf {w} ^{T}\mathbf{x}_i -b)
\end{aligned}$$ 

reformatted as:

$$\begin{aligned}
\underset{\mathbf{w},\mathbf{\xi}}{ {\text{minimize }} } & \frac{1}{2} \|\mathbf {w} \|^{2}+ \left[{\frac {c}{m}}\sum _{i=1}^{m}  \xi_i  \right]\\
{\text{subject to }} & - \xi_i \leq 0\\
& 1-y_{i}(\mathbf {w} ^{T}\mathbf{x}_i -b) - \xi_i\leq 0
\end{aligned}$$ 

The above problem is a typical [constrained optimization problem](../math_topics/constrained_optimization_problem.md).

Ref: [NYU-ML](https://davidrosenberg.github.io/ml2019/#lectures), [NYU-ML,04.b. SVM](https://davidrosenberg.github.io/mlcourse/Archive/2019/Lectures/04b.SVM.pdf), [NYU-ML,04.c.SVM](https://davidrosenberg.github.io/mlcourse/Archive/2019/Lectures/04c.SVM-ComplementarySlackness.pdf), 

### D-2. Appendix: 
**problem formulation:**

Prove:
$$\underset{x}{ {\text{minimize }} } f(x) + g(x) \tag{Eq. D-2.A.1}$$
can be converted to: 
$$\begin{aligned}
\underset{x,\xi}{ {\text{minimize }} } & f(x) + \xi\\
{\text{subject to }} & \xi \geq g(x)
\end{aligned}\tag{Eq. D-2.A.2}$$ 

**Proof:**
<div  align="center"><img src=./support_vector_machine_asset/slack_variable_equivalent_optimization.jpeg style = "zoom:30%"></div>

- Geometric understanding:
  - Denote 
    - $F_1(x) = f(x)+g(x)$
    - $F_2(x,\xi) = f(x)+\xi$, subject to $\xi \geq g(x)$
  - Since 
    - for every point $x'$: $\xi\geq g(x')$, i.e. $\xi$ can take any value $\geq g(x')$
  - Thus, 
    - $F_2(x',\xi)$ can take any value $\geq F_1(x')$
    - I.e. $F_2(x',\xi)$ take the subspace $\geq F_1(x')$ as shown in the right figure.
  - Thus,
    - the optimal value $\min$ and solution of (Eq. D-2.A.1) and (Eq. D-2.A.2) should be same 

- Analytic proof:
  - The confusion in problem (Eq. D-2.A.2) come from the mixing of $x$ and $\xi$, thus, we can separately them by denoting:
    - $\gamma = f(x) + \xi$
    - $h(x) = f(x) + g(x)$
  - Then the problem (Eq. D-2.A.2) can be expressed as:
  $$\begin{aligned}
  \underset{\gamma}{ {\text{minimize }} } & \gamma\\
  {\text{subject to }} & \gamma \geq h(x)
  \end{aligned}$$ 
  - Obviously, $\underset{\gamma}{\min}\gamma = \underset{x}{\min}[h(x)] = \underset{x}{\min}[f(x)+g(x)]$
  - Thus, the problem (Eq. D-2.A.1) and (Eq. D-2.A.2) should be equivalent.


Ref: [NYU-ML,SVM-preparation](https://davidrosenberg.github.io/mlcourse/Archive/2019/Notes/svm-lecture-prep.pdf)

## FAQ:

- Advantage of SVM for linear separable data. (Why maximize margin in SVM))
  - Without maximizing margin, there are infinite solutions (hyperplanes) for linear separable data.
  - With maximize margin, SVM has single solution.

- Why introduce kernel?
  - Non-linear boundary
- Why SVM can use kernel?
  - Because SVM naturally include inner product.

- Will feature scaling affect model?
  - Yes. 
  - Ref: [TowardsDataScience](https://www.google.com/search?q=feature+scaling+and+svm&oq=feature+scaling+and+svm&aqs=chrome..69i57j0i22i30j0i390j69i64l3.5348j1j7&sourceid=chrome&ie=UTF-8), [StakeExchange](https://stats.stackexchange.com/questions/65094/why-scaling-is-important-for-the-linear-svm-classification)


