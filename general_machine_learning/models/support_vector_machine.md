# Support Vector Machine

## 0. Basics

### 0.0 Notations

- $(\mathbf{x},y)$ the input of a sample
  - $\mathbf{x}$ ($\vec{x}$) is an n-dim vector with $N$ features $\mathbf{x}^{(i)} :=X^{(i)}$ 
  - $y$ label of the sample, binary variable: $y \in \{-1,+1\}$
- $\mathcal{S}$ denotes the training set with $M$ samples $\{...(\mathbf{x}_i,y_i)...\}$
- $[\mathbf{w} (\text{or }\vec{w}),b]$ is the (weight vector, intercept) in the prediction function
- $\lambda$ the coefficient on the weight regularization, $c:=\frac{1}{2\lambda}$

### 0.1 Equations

- Prediction (score) function: $f(\mathbf{x}) = \mathbf{w}^T\mathbf{x} - b$
  - Use $-b$ for convenience.
  - Distance between the origin and the hyperplane is $\tfrac {b}{\|\mathbf {w} \|}$. Distance between the two margin hyperplane is $\tfrac {2}{\|\mathbf {w} \|}$ . Ref: [StackExchange](https://math.stackexchange.com/questions/1305925/why-is-the-svm-margin-equal-to-frac2-mathbfw), [Wiki - SVM](https://en.wikipedia.org/wiki/Support-vector_machine#SVM_and_the_hinge_loss) 
  <div  align="center"><img src=https://upload.wikimedia.org/wikipedia/commons/thumb/7/72/SVM_margin.png/617px-SVM_margin.png style = "zoom:40%"></div>
- Margin
  - Functional margin: $y \cdot f(x)$
    - $y \in \{-1,+1\}$, $\hat{y} \in R$
    - We want to maximize margin
      - the higher correct score of the data points, the better classifier
  - Geometric margin: $d_m = \frac{2}{\| \mathbf{w} \|}$
    - This is the distance between the f(x) = +1 and the f(x) = -1 hyperplane, no matter the margin is hard or soft.



## 1. Hard-margin vs Soft-Margin

### 1.1. Hard-margin

All the sample must be predict correctly, no data-points are allow within margin, that is called hard margin. ($\Rightarrow y_{i}(\mathbf {w} ^{T}\mathbf {x}_i -b) \geq 1$)

Since we tend to maximize the geometric margin $d_m=\frac{2}{\|\mathbf{w}\|}$. ($\Rightarrow {\text{minimize }} \|\mathbf{w}\|$)

Thus the training algorithm is:
$$\begin{aligned}
  {\text{minimize }} & \|\mathbf{w}\| \\
  {\text{subject to }} & y_{i}(\mathbf {w} ^{T}\mathbf {x}_i -b) \geq 1
\end{aligned}$$

The problem is a typical [constrained optimization problem](../math_topics/constrained_optimization_problem.md).

### 1.2. Soft-margin

#### 1.2.1 Basic

Data points are allowed to be with-in margin or misclassified. Penalty on with-in margin points and misclassification is realized by Hinge loss. 

- Hinge loss: $l_H(x,y) = \max(0,1-y f(x))$

Under certain degree of penalty on misclassification (parameter $c$), we still aims to maximize margin $d_m$, i.e. minimize $\|\mathbf{w}\|$, thus we add l2-regularization on $\|\mathbf{w}\|$ to the loss functions

- Loss function (let $c:=\frac{1}{2\lambda}$): <br> $\begin{aligned}
    l(\mathcal{S},\mathbf{w},b) &= \lambda \|\mathbf {w} \|^{2} + \left[{\frac {1}{M}}\sum _{i=1}^{M}\max \left(0,1-y_{i}(\mathbf {w} ^{T}\mathbf {x}_i -b)\right)\right]\\
    &= \frac{1}{2} \|\mathbf {w} \|^{2} + \left[{\frac {c}{M}}\sum _{i=1}^{M}\max \left(0,1-y_{i}(\mathbf {w} ^{T}\mathbf {x}_i -b)\right)\right]
\end{aligned}$


#### 1.2.2 Training

- Minimize Hinge loss over training sets: $\mathbf{w} = \underset{\mathbf{w}}{\argmin}[l(\mathbf{w},b|\mathcal{S})]$

- Solution: 
  - $\vec{w} = \sum^M_{i=1}\alpha_i y_i \vec{x}_i$
    - For points "outside/beyond" margin ($y_if(x_i)>1$), $\alpha_i=0$, make no contribute to model.
    - For points <mark style="background-color:yellow;"><font color="#0000dd">**on the margin**</font></mark> ($y_if(x_i)=1$), $\alpha_i \in [0,\frac{c}{m}]$ and points <mark style="background-color:yellow;"><font color="#0000dd">**inside/below the margin**</font></mark> ($y_if(x_i)<1$), $\alpha_i=\frac{c}{m}$, these points will contribute to the model, thus are called <mark style="background-color:yellow;"><font color="#0000dd">**support vectors**</font></mark>
    - I.e. <mark style="background-color:yellow;">$\mathbf{w}$ vector is a <mark style="background-color:yellow;"><font color="#0000dd">**linear combination**</font></mark> of the <font color="#0000dd">**support vectors**</font> $\{\mathbf{x_i}\}$</mark>
  - $b=\mathbf {w} ^{T}\mathbf {x}_i - y_i$ for any point $(\mathbf{x}_i, y_i)$ on the margin 
    - $\Leftarrow$ On the margin: $\mathbf {w} ^{T}\mathbf {x}_i -b = y_i = \pm1$
  - Ref: [NYU-ML](https://davidrosenberg.github.io/ml2019/#home), [NYU-ML-04.b.SVM](https://davidrosenberg.github.io/mlcourse/Archive/2019/Lectures/04b.SVM.pdf), [NYU-ML-04.c.SVM](https://davidrosenberg.github.io/mlcourse/Archive/2019/Lectures/04c.SVM-ComplementarySlackness.pdf)


**Note:** For hard margin SVM, there are no points inside the margin, only points on the margin, so the support vectors are just points on the margin. For both soft and hard margin SVM, the support vectors $(\mathbf {x_{(sv)}}_i,y_{(sv)i})$ can be expressed as:
- $y_{(sv)i}(\mathbf {w} ^{T}\mathbf {x_{(sv)}}_i -b)\leq 1$
  - ${\text{ i.e. : }}  -1  s\leq f(\mathbf {x_{(sv)}}_i) \leq 1$
- Ref: [StackOverflow](https://stackoverflow.com/questions/50141815/are-the-points-inside-the-margin-of-a-svm-classifier-also-support-vectors)

#### 1.2.3 Variance-bias tradeoff

- c: regularization parameter (inverse to weight regularization)
  - $c \uparrow$
  - $\Rightarrow$
    - smaller penalty on $\|\mathbf{w}\|$ $\Rightarrow$ larger $\|\mathbf{w}\|$
    - larger penalty on with-in margin points $\Rightarrow$ smaller geometric margin width $d_m = \frac{2}{\|\mathbf{w}\|}$, consistent with "larger $\|\mathbf{w}\|$"
  - $\Rightarrow$ Less tolerance on misclassification
    - = complexer boundary 
    - = **increase** model complexity/**variance**

#### 1.2.4 Soft Margin to Hard Margin

**Summary**: The **regularization parameter** $c$ can be viewed as the <font color="#0000dd">*"hardness of the margin"*</font>, when $c \rightarrow \infty$, the soft margin SVM become hard margin SVM. [Ref: Github Blog](https://jakevdp.github.io/PythonDataScienceHandbook/05.07-support-vector-machines.html)

**Proof**:

- For hard margin SVM, the problem is: <p>$$\begin{aligned}
  {\text{minimize }} & \|\mathbf{w}\| \\
  {\text{subject to }} & y_{i}(\mathbf {w} ^{T}\mathbf {x}_i -b) \geq 1
\end{aligned}$$
  - Define the domain of $(\mathbf{w},b)$ as $\Phi_{(\mathbf{w},b)}$, i.e.:<p>
  $$(\mathbf{w},b)\in\Phi_{(\mathbf{w},b)} \Leftrightarrow y_{i}(\mathbf {w} ^{T}\mathbf {x}_i -b) \geq 1$$
  - Since the following is equivalent:<p>
  $$\underset{}{\text{minimize }} \|\mathbf {w} \|\Leftrightarrow \underset{}{\text{minimize }} \frac{1}{2} \|\mathbf {w} \|^{2}$$
  - The problem can be written as:<p>
  $$\begin{aligned}
    \underset{(\mathbf{w},b)\in\Phi_{(\mathbf{w},b)}}{\text{minimize }} \frac{1}{2} \|\mathbf {w} \|^{2}
  \end{aligned}$$

- For soft margin SVM, the problem is $\underset{\mathbf{w}}{\text{minimize }}l(\mathbf{w},b|\mathcal{S})$, i.e.:<p>
$$\begin{aligned}
  \underset{(\mathbf{w},b)}{\text{minimize }} \frac{1}{2} \|\mathbf {w} \|^{2} + \left[{\frac {c}{M}}\sum _{i=1}^{M}\max \left(0,1-y_{i}(\mathbf {w} ^{T}\mathbf {x}_i -b)\right)\right]
\end{aligned}$$
  - When $c \rightarrow \infty$:<p>
  $$\left[{\frac {c}{M}}\sum _{i=1}^{M}\max \left(0,1-y_{i}(\mathbf {w} ^{T}\mathbf {x}_i -b)\right)\right]=\left\{ \begin{array}{ll} 0 & (\mathbf{w},b)\in\Phi_{(\mathbf{w},b)}\\ +\infty & \textrm{otherwise} \end{array} \right.$$
  - I.e.:<p>$$l(\mathbf{w},b|\mathcal{S})=\left\{ \begin{array}{ll} \frac{1}{2} \|\mathbf {w} \|^{2} & (\mathbf{w},b)\in\Phi_{(\mathbf{w},b)}\\ +\infty & \textrm{otherwise} \end{array} \right.$$
  - Then, the problem $\underset{\mathbf{w}}{\text{minimize }}l(\mathbf{w},b|\mathcal{S})$ has a same solution with the following equation, which is exactly the hard margin SVM problem: <p>
  $$\begin{aligned}
    \underset{(\mathbf{w},b)\in\Phi_{(\mathbf{w},b)}}{\text{minimize }} \frac{1}{2} \|\mathbf {w} \|^{2}
  \end{aligned}$$

### 1.3. Inner product representation

#### 1.3.1. Prediction function

$$\begin{aligned}
f(\mathbf{x}) &= \mathbf{w}^T\mathbf{x} +b \\
&= \sum_{i}\alpha_{i} y_{i}\mathbf{x}_i^T\mathbf{x}  +b\\
& := \sum_{i}\alpha_{i} y_{i} \langle\mathbf{x}_{i}, \mathbf{x} \rangle +b
\end{aligned}$$
- Denote $\langle \mathbf{x}_i, \mathbf{x}_j\rangle$ as the **inner product** of $\mathbf{x}_i$ and $\mathbf{x}_j$
  - $\langle \mathbf{x}_i, \mathbf{x}_j\rangle = \mathbf{x}_i^T\mathbf{x}_j = \vec{x}_i\cdot\vec{x}_j$
- If define the index of support vector as set $\mathcal{M}_{sv} \subseteq \{1,2,...,M\}$, i.e., the weight coefficient $\alpha_{i_{sv}} \not = 0 \forall i_{sv}  \in \mathcal{M}_{sv} $
  - $f(\mathbf{x}) = \sum_{i_{sv}\in\mathcal{M_{sv}}}\alpha_{i_{sv}} y_{i_{sv}} \mathbf{x}_{i_{sv}}^T\mathbf{x} +b := \sum_{i_{sv}\in\mathcal{M_{sv}}}\alpha_{i_{sv}} y_{i_{sv}} \langle\mathbf{x}_{i_{sv}}, \mathbf{x} \rangle +b$

#### 1.3.2. Optimization function

The optimization problem (dual problem) can also be written with inner product. (TBD). Ref: [NYU-ML: 04b.SVM](https://davidrosenberg.github.io/mlcourse/Archive/2019/Lectures/04b.SVM.pdf)

$$\begin{aligned}
\underset{\mathbf{\alpha},\mathbf{\beta}}{ {\text{maximize }} } & 
\sum_{i=1}^M \alpha_i - \frac{1}{2}\sum_{i,j=1}^M\alpha_i\alpha_jy_iy_j\langle \mathbf{x}_i, \mathbf{x_j}\rangle
\\
{\text{subject to }} & \sum_{i=1}^M \alpha_iy_i=0 \\
& \alpha_i+ \beta_i = \frac{c}{M}
\\
& \alpha_i \geq 0
\\
& \beta_i \geq 0
\end{aligned}$$ 
where:
- $\alpha_i$ is the Lagrange Multiplier for constraints $1-y_{i}(\mathbf {w} ^{T}\mathbf{x}_i -b) - \xi_i\leq 0$
- $\beta_i$ is the Lagrange Multiplier for constraints $-\xi_i \leq 0$
- $\alpha_i$ is the $\alpha_i$ in $\vec{w} = \sum^M_{i=1}\alpha_i y_i \vec{x}_i$, $c$ is the regulation parameter $c$ in $l(\mathcal{S},\mathbf{w},b)$

  

## 3.Kernel Method Basic Understanding

Ref: [Blog](https://blog.pluskid.org/?p=685&utm_source=ZHShareTargetIDMore&utm_medium=social&utm_oi=914313775224623104), [TowardsDataScience](https://towardsdatascience.com/the-kernel-trick-c98cdbcaeb3f)

### 3.1 Why to introduce kernel method?

- The most "naive" SVM **only** deal with <mark style="background-color:yellow;"><font color="#0000dd">**linear separable problem**. </font></mark>
  - I.e. the boundary of a "naive" SVM is always linear.
  - I.e. the space "separator" is always a hyperplane, not a hypersurface. 
  - Q: Why the boundary must be linear?
    - Because prediction function $f(\mathbf{x}) = \mathbf{w}^T\mathbf{x} - b$ is linear to $\mathbf{x}$ and $f(x) = 0$ is a hyperplane.)
- To make SVM work with <font color="#0000dd">**not-linear-separable data**</font>. We introduce kernel method.

### 3.2. How kernel method works?



#### 3.2.1 Basic idea: project data to higher dimension

The <font color="#0000dd">not-linear-separable</font> data may be <mark style="background-color:yellow;"><font color="#0000dd">linear separable in higher dimension</font></mark>.

<div  align="center"><img src= ./support_vector_machine_asset/project_data_to_higher_dimension.jpeg style = "zoom:40%"></div>

For example, in the above figure (a), we have the data:

- $\mathbf{x} = (x^{(1)},x^{(2)})$, $y\in\{-1,1\}$, 
- The underlying generation function is $y=\sqrt{( x^{(1)} )^2+(x^{(2)})^2}-1$.
- The data $(\mathbf{x},y)$ is not linear-separable

However, we can do a projection, like shown in figure (b):

- project function $\mathbf{\phi}: \mathbf{x} \rightarrow \mathbf{z}$
  - $\mathbf{z} = (z^{(1)},z^{(2)},z^{(3)} )$ where $z^{(1)}=x^{(1)}$, $z^{(2)}=x^{(2)}, z^{(3)}= ( x^{(1)} )^2+(x^{(2)})^2 $
- The data $(\mathbf{z},y)$ i.e. $(\mathbf{\phi}(\mathbf{x}),y)$, as shown in figure (b) is linear separable with hyperplane $F(\mathbf{z})=0$
- Note: 
  - After project to higher dimension, the 2D space is a hyper-surface in 3D, it will not saturate all the 3D space.
  - In 3D, the separator is a hyperplane, i.e. the score function $F(\mathbf{z}) = c_{1}z^{(3)} + c_{0}$ is linear to $\mathbf{z}$; But project back to 2D, the score function $f(\mathbf{x}) = c_{1}[ ( x^{(1)} )^2+(x^{(2)})^2 ] + c_{0}$ is non-linear to $\mathbf{x}$
  - $F(\mathbf{z})$ and $f(\mathbf{x})$ is actually a "same" function i.e. (for $\mathbf{x'} \in \mathbb{R}^2$ and $\mathbf{z'}=\mathbf{\phi}(\mathbf{x'})$, $ F(\mathbf{z'})=f(\mathcal{x'}) $), the difference is: $F(\mathbf{z})$ has boarder domain ($\mathbb{R}^3$, i.e., the whole space in 3D); but domain of $f(\mathbf{x})$ is $\mathbb{R}^2$, or, a 2D hypersurface in 3D.


<!-- - <div  align="center"><img src=https://blog.pluskid.org/wp-content/uploads/2010/09/two_circles.png style = "zoom:60%"><img src=https://blog.pluskid.org/wp-content/uploads/2010/09/rotate.gif style = "zoom:40%"></div> -->

#### 3.2.2 Kernel trick

Projecting data to higher dimension possibly make data linear separable. But higher dimension vector is inconvenient to represent and taking more memory. Moreover, in math, we can project data to infinity dimension, which is impossible to represent it explicitly.

The kernel trick is a technique, with which we equivalently project the data to higher dimension, but no need to explicitly represent the projected vector.

Here we use a quadratic kernel $\kappa(\mathbf{x}_i,\mathbf{x}_j) = (\langle \mathbf{x}_i,\mathbf{x}_j \rangle + 1)^2$ as example.

Let's say we have the data:

- $\mathbf{x} = (x^{(1)},x^{(2)})$, $y\in\{-1,1\}$

We want to (explicitly) project $\mathbf{x}$ to a $\mathbb{R}^6$ space (denote with $\mathbf{z}$), as:

- Define: <p> $\begin{aligned}
    \mathbf{z} & := \mathbf{\phi}(\mathbf{x}) \\
    & := (z^{(1)},z^{(2)},z^{(3)},z^{(4)},z^{(5)},z^{(6)})\\
    & := (\sqrt{2}x^{(1)}, (x^{(1)})^2,\sqrt{2}x^{(2)}, (x^{(2)})^2,\sqrt{2}x^{(1)}x^{(2)},1)
  \end{aligned}$
- Then, our problem becomes:
  - the data points: $(\mathbf{\phi}(\mathbf{x}),y)$
  - the optimization problem: 
  $$\begin{aligned}
  \underset{\mathbf{\alpha_{\mathbf{\phi}}},\mathbf{\beta_{\mathbf{\phi}}}}{ {\text{maximize }} } & 
  \sum_{i=1}^M \alpha_{\mathbf{\phi}i} - \frac{1}{2}\sum_{i,j=1}^M\alpha_{\mathbf{\phi}i}\alpha_{\mathbf{\phi}j}y_iy_j\langle \mathbf{\phi}(\mathbf{x}_i), \mathbf{\phi}(\mathbf{x_j}) \rangle
  \\
  {\text{subject to }}& ...( \alpha_{\mathbf{\phi}i}, \beta_{\mathbf{\phi}i} {\text{ related conditions}} )
  \end{aligned}$$
  - the prediction functions becomes:
  $$f(\mathbf{x}) = \sum_{i}\alpha_{\phi i} y_{i} \langle \mathbf{\phi}(\mathbf{x}_{i}), \mathbf{\phi}(\mathbf{x}) \rangle +b$$

Here, we can define a kernel function $\mathbf{\kappa}(\mathbf{x}_i,\mathbf{x}_j) = (\langle \mathbf{x}_i,\mathbf{x}_j \rangle + 1)^2$ and prove:

$$\begin{aligned}
  \mathbf{\kappa}(\mathbf{x}_i,\mathbf{x}_j) =& (\langle \mathbf{x}_i,\mathbf{x}_j \rangle + 1)^2\\
  =&(x_i^{(1)}x_j^{(1)} + x_i^{(2)}x_j^{(2)} + 1)^2\\
  =&(x_i^{(1)}x_j^{(1)} + x_i^{(2)}x_j^{(2)} + 1)(x_i^{(1)}x_j^{(1)} + x_i^{(2)}x_j^{(2)} + 1)\\
  =& ( x_i^{(1)}x_j^{(1)} )^2 +x_i^{(1)}x_j^{(1)}x_i^{(2)}x_j^{(2)}+x_i^{(1)}x_j^{(1)}\\
  &+x_i^{(1)}x_j^{(1)}x_i^{(2)}x_j^{(2)}+( x_i^{(2)}x_j^{(2)} )^2 +x_i^{(2)}x_j^{(2)} \\
  &+x_i^{(1)}x_j^{(1)} + x_i^{(2)}x_j^{(2)} + 1 \\
  =& 2x_i^{(1)}x_j^{(1)} + ( x_i^{(1)}x_j^{(1)} )^2 + 2x_i^{(2)}x_j^{(2)} + ( x_i^{(2)}x_j^{(2)} )^2 + 2 x_i^{(1)}x_j^{(1)}x_i^{(2)}x_j^{(2)} + 1\\
  =&\langle \mathbf{\phi}(\mathbf{x}_{i}), \mathbf{\phi}(\mathbf{x}_j) \rangle
\end{aligned} $$

Then the problem (after projected to high dimension) can be representative as:

- the data points: $(\mathbf{x},y)$
- the optimization problem:
 $$\begin{aligned}
\underset{\mathbf{\alpha_{\mathbf{\kappa}}},\mathbf{\beta_{\mathbf{\kappa}}}}{ {\text{maximize }} } & 
\sum_{i=1}^M \alpha_{\mathbf{\kappa}i} - \frac{1}{2}\sum_{i,j=1}^M\alpha_{\mathbf{\kappa}i}\alpha_{\mathbf{\kappa}j} y_i y_j \mathbf{\kappa}(\mathbf{x}_i,\mathbf{x}_j)
\\
{\text{subject to }}& ...( \alpha_{\mathbf{\kappa}i}, \beta_{\mathbf{\kappa}i} {\text{ related conditions}} )
\end{aligned}$$
- the prediction functions:
$$f(\mathbf{x}) = \sum_{i}\alpha_{\kappa i} y_{i} \kappa(\mathbf{x}_i,\mathbf{x}_j) +b$$

It looks like: 

- Rather than explicitly projecting data points to higher dimension, we just use the kernel function to replace the inner product $\langle \mathbf{x}_i,\mathbf{x}_j \rangle$ appears in the problem. And the effect (training/prediction process/result) is exactly same with the operation explicitly projecting data points to higher dimension. 

Such method is called <mark style="background-color:yellow;">**kernel trick**</mark>, i.e.:

- <mark style="background-color:yellow;">Replace inner product with kernel function</mark>
  - <mark style="background-color:yellow;">without explicitly representing data in higher dimension</mark>.
- <mark style="background-color:yellow;">the effect (training and prediction process) exactly same with projecting data to higher dimension</mark>


**Note:** <mark style="background-color:yellow;">**kernel trick** relies on the <font color="#0000dd">appearance of **inner product**</font>, that is why among common ML algorithms, kernel trick can be **only used in SVM** but **CANNOT be used in other algorithms**</mark>. 

### 3.3. Typical kernel functions

#### 3.3.1 Polynomial kernel

Ref [Wiki: Polynomial kernel](https://en.wikipedia.org/wiki/Polynomial_kernel)

$$\kappa(\mathbf{x}_i,\mathbf{x}_j) = (\langle \mathbf{x}_i,\mathbf{x}_j \rangle + c)^d$$

Project data to finite higher dimension.

#### 3.3.2 Gaussian (RBF) kernel

Ref: [Wiki: Radial basis function kernel](https://en.wikipedia.org/wiki/Radial_basis_function_kernel)

$$\kappa(\mathbf{x}_i,\mathbf{x}_j)=\exp \left(-{\frac {\|\mathbf {x}_i -\mathbf {x}_j \|^{2}}{2\sigma ^{2}}}\right)$$

Project data to infinite higher dimension.


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

## D-2. Deep Dive: How to solve soft-margin SVM?

The SVM problem can be expressed as: Minimize Hinge loss over training sets: $(\mathbf{w},b) = \underset{\mathbf{w},b}{\argmin}[l(\mathcal{\mathbf{w},b|S})]$ where:
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

## D-3. Modern methods to solve SVM (TBD)

Ref:[Wiki-SVM](https://en.wikipedia.org/wiki/Support-vector_machine)

### Sub-gradient descent (TBD)

### Coordinate descent (TBD)

## FAQ:

### Quick QA

#### Basic

- Advantage of SVM for linear separable data. (Why maximize margin in SVM))
  - Without maximizing margin, there are infinite solutions (hyperplanes) for linear separable data.
  - With maximize margin, SVM has single/unique solution.


- Hard margin vs Soft margin
  - C control the hardness of the margin, ${\text{Soft margin}}\overset{c\rightarrow\infty}{\rightarrow}{\text{Hard margin}}$
  - Understand:
    - Soft Margin: Larger C, less tolerance on "misclassified/not-confident"($|f(x)|<1$) samples
    - Hard Margin: No tolerance on "misclassified/not-confident"($|f(x)|<1$) samples
  - Ref: [Zhihu](https://www.zhihu.com/question/40217487)

#### Training

- How to train?
  - Maximize the geometric margin.
- Why to introduce dual Problem
  - Easy to solve primal problem: dual problem is always convex regardless the convexity of primal problem
- How to solve SVM?
  - Convert to form of constrained optimization problem.
    - Langrage Equation/Multiplier: Convert constrained optimization to unconstrained problem
    - Dual problem: The dual problem is always convex (easier to solve) regardless whether the convexity of primal
    - Strong Duality: under strong duality, the primal and dual problem has the same solution:
    - KKT conditions: under certain condition (primal is convex + slater conditions), the strong duality hold and the solution for primal (dual) is got from solving equations in KKT conditions.
  - Modern ways: sub-gradient descendant, coordinate descendant.

#### Kernel
Ref: [Blog](http://blog.pluskid.org/?p=685&utm_source=ZHShareTargetIDMore&utm_medium=social&utm_oi=914313775224623104)
- Why introduce kernel?
  - The data is not linear separable.
- Effect of kernel in the original data space.
  - Non-linear boundary in the original space. (still linear in the higher dimension.)
- Why SVM can use kernel?
  - Because SVM naturally include inner product.
- What is "kernel trick"?
  - By just replacing the inner product with the kernel function, we can realize the effect of transform the data to higher dimension without explicitly introduce higher dimension features (i.e. represent the sample in higher dimension.)

### Un-summarized question

- Will feature scaling affect model?
  - Yes, for most applied svm models.
    - E.g.: Like for Gaussian Kernel, the result is dependent on the euclean distance among points, which will be heavily affected by scaling.[Ref:?]()
    - Ref: [TowardsDataScience](https://www.google.com/search?q=feature+scaling+and+svm&oq=feature+scaling+and+svm&aqs=chrome..69i57j0i22i30j0i390j69i64l3.5348j1j7&sourceid=chrome&ie=UTF-8), [StackExchange](https://stats.stackexchange.com/questions/65094/why-scaling-is-important-for-the-linear-svm-classification)
  - No, for linear, hard-margin svm.
    - Ref: [StackExchange](https://stats.stackexchange.com/a/154751)
    - For soft margin, scaling Ref[StackExchange](https://stats.stackexchange.com/questions/77876/why-would-scaling-features-decrease-svm-performance) [StackExchange](https://stats.stackexchange.com/questions/118092/why-does-the-scaling-of-feature-vectors-improve-performance-of-svm-classifier)
- SVM is a convex problem.
  - Ref: [StakeExchange](https://math.stackexchange.com/questions/1127464/how-to-show-that-svm-is-convex-problem),
  - Logistic regression is convex.
  - K-means could be random
  - Tree could be random (if same info gain or if missing data)
- Why use hinge loss?
  - More accurate decision boundary:
    - The loss focus on the near-boundary samples (support vectors), ignore the "very confident" (far-away-from-boundary ) samples.
    - Thus, the shape of boundary is determined by the near-boundary samples not affected by "very confident" points.
    - Thus, the model should have a higher accuracy on near-boundary sample (without affecting far-away-from-boundary points.)
    - Thus, the model should have a higher overall accuracy (more accurate decision boundary)
  - Other loss like log-loss would more or less affected by "very confident" (far-away-from-boundary ) samples.
- SVM is sensitive to missing values
  - SVM has no strategy to automatically deal with missing values.
  - Naive SVM require linear separable, so the distribution of data is important
  - Some other algorithm like XGBoost have strategy to deal with missing value
  - Ref: [CSDN Blog](https://blog.csdn.net/szlcw1/article/details/52259668)
- SVM is sensitive to imbalanced data. 
  - The svm more favor to predict the majority.
  - E.g. Assume $m(y=+1) >> m(y=-1)$, comparing to balanced data case, the imbalanced data has:
    - More (y=+1, majority) points fall into the inside-margin area
    - Those points will push margin towards  (y=-1, minority) area
    - The prediction score will be higher than ground truth, i.e. the model prefer to predict/give (y=+1, majority) result.
    - Solution: class-weighted SVM, i.e. assign more weight on loss of minority. (This is actually duplicate data, equivalent to add more data for minority.)
- Understanding on "Weight Regularization" "$\|\mathbf{w}\|$" or "$\|\mathbf{w}\|^2$" 
  - In SVM, the "$\|\mathbf{w}\|$" or "$\|\mathbf{w}\|^2$" must be contained, cannot be removed. Actually
    - it is NOT "weight regularization"
    - it originates from the core idea "maximize (geometric) margin" $d=\frac{2}{\|\mathbf{w}\|}$

- Different types of kernel:
  - Linear kernel: produce linear boundary, the most simple one is just the inner product. [ref:?]()
  - Polynomial kernel: could produce non-linear boundary, simulate finite higher dimension. [ref:?]()
  - RBF (Gaussian) kernel: could produce non-linear boundary, simulate infinite higher dimension.[ref:?]()
  - Ref: [sklearn-SVM](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html) [Quora](https://www.quora.com/What-differentiates-a-radial-basis-function-from-a-gaussian-kernel-while-using-SVM)
- Gaussian Kernel (? Not strict, TBD)
  - $\kappa(\mathbf{x}_i, \mathbf{x}_j) = \exp(-\frac{\|\mathbf{x}_i - \mathbf{x}_j\|^2}{2\sigma^2})$
    - Infinity dimensions
      - There are infinite terms/orders (Taylor expansion), can be written as inner produce of infinity dimensions: <p>
      $$\begin{aligned}
        \kappa(\mathbf{x}_i, \mathbf{x}_j) &= \exp(-\frac{\|\mathbf{x}_i - \mathbf{x}_j\|^2}{2\sigma^2})\\
        &=\frac{c_1}{\sigma^2}\|(\mathbf{x}_i - \mathbf{x}_j\|)^2 + ... + \frac{c_n}{\sigma^{2\infty} }(\|\mathbf{x}_i - \mathbf{x}_j\|)^{2\infty}\\
        &=\sum^\infty_{r=2} \sum^r_{t=0} \sum_{d_i,d_j}\frac{c^{(2)}_{t,r,d_i,d_j}}{\sigma^{2r}} [\mathbf{x}^{(d_i)}_i]^{2t} [\mathbf{x}^{(d_j)}_j]^{2(r-t)}\\
        &= \phi(\mathbf{x}_i) \cdot \phi(\mathbf{x}_j)
      \end{aligned}$$
      - where $\phi(\mathbf{x}) = ( ..., \frac{c^{(3)}_{t,r,i}}{\sigma^r}\prod_i [\mathbf{x}^{(d_i)}]^{t_i} ,... )$
  - By tuning $\sigma$, the "effective dimension" could change.
    - $\sigma \uparrow$, effective dimension $\downarrow$, model complexity $\downarrow$, model variance $\downarrow$, overfitting $\downarrow$
      - The larger $\sigma$:
        - The higher shrinkage on high dimensions (due to $\frac{1}{\sigma^r}$)
        - The lower effective dimension
      - The lower model complexity, i.e. lower model variance
        - Reduce overfitting

## Backup refs:

References: [CSDN-Blog:SVM](https://blog.csdn.net/cppjava_/article/details/68060439), [CSDN-Blog:SVM-FAQ](https://blog.csdn.net/szlcw1/article/details/52259668), [Zhihu](https://zhuanlan.zhihu.com/p/81890745)