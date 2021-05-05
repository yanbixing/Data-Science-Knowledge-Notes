# Support Vector Machine

## 0. Basics

#### 0.0 Notations

- $(\mathbf{x},y)$ the input of a sample
  - $\mathbf{x}$ ($\vec{x}$) is an n-dim vector with n features $\mathbf{x}^{(i)} :=X^{(i)}$ 
- $\mathcal{S}$ denotes the training set with m samples $\{...(\mathbf{x}_i,y_i)...\}$
- $\mathbf{w}$ ($\vec{w}$) is the weight vector aheard in the prediction fucntion
- $\lambda$ the coefficient on the weight regularization, $c:=\frac{1}{2\lambda}$
#### 0.1 Equations

- Prediction (score) function: $f(\mathbf{x}) = \mathbf{w}^T\mathbf{x} - b$
  - Use $-b$ for convenience.
  - Distance between the origin and the hyperplane is $\tfrac {b}{\|\mathbf {w} \|}$. Distance between the two margin hyperplane is $\tfrac {2}{\|\mathbf {w} \|}$ . Ref: [StackExchange](https://math.stackexchange.com/questions/1305925/why-is-the-svm-margin-equal-to-frac2-mathbfw), [Wiki - SVM](https://en.wikipedia.org/wiki/Support-vector_machine#SVM_and_the_hinge_loss) 
  <div  align="center"><img src=https://upload.wikimedia.org/wikipedia/commons/thumb/7/72/SVM_margin.png/617px-SVM_margin.png style = "zoom:40%"></div>
- Margin: $y\cdot f(x)$
- Hinge loss: $l(x,y) = \max(0,1-y f(x))$
- Loss function (let $c:=\frac{1}{2\lambda}$): 
$$\begin{aligned}
    L(\mathcal{S}) &= \left[{\frac {1}{m}}\sum _{i=1}^{m}\max \left(0,1-y_{i}(\mathbf {w} ^{T}\mathbf {x} -b)\right)\right]+\lambda \|\mathbf {w} \|^{2}\\
    &= \left[{\frac {c}{m}}\sum _{i=1}^{m}\max \left(0,1-y_{i}(\mathbf {w} ^{T}\mathbf {x} -b)\right)\right]+\frac{1}{2} \|\mathbf {w} \|^{2}
\end{aligned}$$

#### Training

- Minimize Hinge loss over training sets: $w = \underset{w}{\argmin}[L(\mathcal{S})]$
- Solution: $\vec{w} = \sum^m_{i=1}\alpha_i y_i \vec{x}_i$
  - For points "outside/beyond" margin ($y_if(x_i)>1$), $\alpha_i=0$, make no contribute to model.
  - For points <font color="#0000dd">**on the margin**</font> ($y_if(x_i)=1$), $\alpha_i \in [0,\frac{c}{m}]$ and points <font color="#0000dd">**inside/below the margin**</font> ($y_if(x_i)<1$), $\alpha_i=\frac{c}{m}$, these points will contribute to the model, thus are called <font color="#0000dd">**support vectors**</font>
  - I.e. $w$ vector is a <font color="#0000dd">**linear combination** of </font> the <font color="#0000dd">**support vectors**</font> $\{x_i\}$
  - Ref: [NYU-ML-2019](https://davidrosenberg.github.io/ml2019/#home), [NYU-ML-2019:SVM](https://davidrosenberg.github.io/mlcourse/Archive/2019/Lectures/04b.SVM.pdf)

## Deep Dive: why margin is $\tfrac {2}{\|\mathbf {w} \|}$?
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


## Deep Dive: SVM solution

Ref: [Zhihu](https://zhuanlan.zhihu.com/p/24638007)

### Basics

#### constraints

For $x \in \mathbb{R}^d$, The equality constraints like f(x)=0, the solution is a $n-1$ dim hyperplane.

The inequality constraints like f(x)<0, the solution is a subspace in the $\mathbb{R}^d$ space.

Minimize $L(\mathcal{S})$


#### equality constrained optimization solution

An equality constrained problem can be expressed like:
$${\text{minimize }} f(x)$$
$${\text{subject to }} g(x) = 0$$

<div  align="center">
  <img src=./support_vector_machine_asset/equality_constrained_optimization_figure.jpeg style = "zoom:30%">
</div>

Denote the optimized solution as $x^*$ and corresponding $f(x^*):=f^*$, we have:

- No.1: The gradient at optimized point $\nabla f(x^*)$ must be orthogonal to its contour $f(x) = f^*$.
  - This can be inferred from the definition, the gradient is always normal to contour, or the contour will not have constant value.
- No.2: The gradient of $g(x)$ at the optimized point, i.e. $\nabla g(x^*)$ must be orthogonal to hypersurface $g(x) = 0$
  - Similar to the above reason in No.1, $g(x) = 0$ is also a contour for function $g(x)$, gradient must be normal to contour.

- No.3: The gradient at optimized point (solution $x^*$), i.e. $\nabla f(x^*)$, must be orthogonal (normal/perpendicular) to the hypersurface $g(x)=0$.
  - The solution must lies on the hypersurface $g(x)=0$.
  - If the gradient's tangent projection $\nabla f(x)$ is not zero, then, we can still move on the surface to reduce the value of f(x), the point is not extreme value on the hypersurface.

- The hypersurface $f(x) = f^*$ must be tangent to $g(x) = 0$ (locally, only have 1 intersection point, but at other area, may also have other intersection.)
  - From the above No.2 and No.3 we can know **$\nabla f(x^*)$ and $\nabla g(x^*)$ is in a same line** (i.e. same/opposite direction), then the tangent is in a same line, then the two hypersurface $g(x)=0$ and $f(x)=f^*$ are tangent to each other.

#### Lagrangian Multiplier Method

The target of lagrangian multiplier method is optimization problem that with constraints. 

The purpose of lagrangian multiplier method is to "merge" constraints to objective function to form a new function (Lagrangian). So that we can convert constrained optimization problem to un-constrained problem.

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


