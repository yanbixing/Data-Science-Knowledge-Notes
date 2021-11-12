# Gradient Vanishing

## 1. Origin: Backpropagation

$\boldsymbol{Z}^{(1)} = \boldsymbol{W}^{(1)} \boldsymbol{X}+\boldsymbol{b}^{(1)} $
$\boldsymbol{A}^{(1)}  = g(\boldsymbol{Z}^{(1)} )$
$\boldsymbol{Z}^{(2)}  = \boldsymbol{W}_2\boldsymbol{A}^{(1)} +\boldsymbol{b}^{(2)} $
$\boldsymbol{A}^{(2)}  = g(\boldsymbol{Z}^{(2)} )$
$\boldsymbol{Z}^{(3)}  = \boldsymbol{W}^{(3)}\boldsymbol{A}^{(2)} +\boldsymbol{b}^{(3)}$
$\boldsymbol{A}^{(3)}  = g(\boldsymbol{Z}^{(3)} )$
$R^{(3)} = L(\boldsymbol{A}^{(3)}, \boldsymbol{Y} ) = R(\boldsymbol{W}^{(i)}, \boldsymbol{b}^{(i)})$


Due to **chain rule:**

$\begin{aligned}
    \frac{\partial R^{(3)}}{\partial W^{(3)}} &= \frac{dR}{dA^{(3)}} \cdot \frac{dA^{(3)}}{dZ^{(3)}}\cdot \frac{\partial Z^{(3)}}{\partial W^{(3)}} \\ &= \frac{dR}{dA^{(3)}} \cdot g'( Z^{(3)}) \cdot \frac{\partial Z^{(3)}}{\partial W^{(3)}}
\end{aligned}$ 
$\begin{aligned}
    \frac{\partial R^{(3)}}{\partial W^{(2)}} &= \frac{dR}{dA^{(3)}} \frac{dA^{(3)}}{dZ^{(3)}} \frac{dZ^{(3)}}{dA^{(2)}}  \frac{dA^{(2)}}{dZ^{(2)}} \frac{\partial Z^{(2)}}{\partial W^{(2)}} \\&= \frac{dR}{dA^{(3)}}  g'(Z^{(3)}) W^{(3)} g'(Z^{(2)}) \frac{\partial Z^{(2)}}{\partial W^{(2)}}
\end{aligned}$
$\begin{aligned}
    \frac{\partial R^{(3)}}{\partial W^{(1)}} &= \frac{dR}{dA^{(3)}}  \frac{dA^{(3)}}{dZ^{(3)}}\frac{dZ^{(3)}}{dA^{(2)}}  \frac{dA^{(2)}}{dZ^{(2)}} \frac{dZ^{(2)}}{dA^{(1)}} \frac{dA^{(1)}}{dZ^{(1)}}\frac{\partial Z^{(1)}}{\partial W^{(1)}} 
    \\&= \frac{dR}{dA^{(3)}} \cdot g'(Z^{(3)})\cdot W^{(3)}  \cdot  g'(Z^{(2)})\cdot W^{(2)}  \cdot  g'(Z^{(1)})\cdot\frac{\partial Z^{(1)}}{\partial W^{(1)}}
\end{aligned}$

Ref: [Wiki-BP](https://en.wikipedia.org/wiki/Backpropagation), [Coursera](https://www.coursera.org/learn/neural-networks-deep-learning/lecture/Wh8NI/gradient-descent-for-neural-networks)

In sum:

$$ \frac{\partial R^{(n)}}{\partial W^{(n-k)}} =  \frac{dR^{n}}{dA^{(n)}} \prod^{n-k}_{i=n}g'(Z^{(i)})\prod^{n-k+1}_{j=n}W^{(j)}\frac{\partial Z^{(n-k)}}{\partial W^{(n-k)}}$$

Gradient vanishing: If $W$ is slightly smaller than 1, then $\prod^{n-k+1}_{j=n}W^{(j)} \rightarrow 0$
Gradient exploding: If $W$ is slightly larger than 1, then $\prod^{n-k+1}_{j=n}W^{(j)} \rightarrow \infty$

Gradient vanishing/exploding originates from **chain rule**: for very deep grad, there will be a product of weights from each layer.


## 2. Solution: 

### 2.1. skip connection

Without skip connection, when vanishing happens, i.e. $\prod^{n-k+1}_{j=n}W^{(j)} \rightarrow 0$

$$ \frac{\partial R^{(n)}}{\partial W^{(n-k)}} =  \frac{dR^{n}}{dA^{(n)}} \prod^{n-k}_{i=n}g'(Z^{(i)})\prod^{n-k+1}_{j=n}W^{(j)}\frac{\partial Z^{(n-k)}}{\partial W^{(n-k)}} \rightarrow 0$$


Let's say we make a skip connection from $Z^{(n-k)}$ to $Z^{(n)} $
$\boldsymbol{A}^{(n)}  = g(\boldsymbol{Z}^{(n)} + \boldsymbol{Z}^{(n-k)} )$
$R^{(n)} = L(\boldsymbol{A}^{(n)}, \boldsymbol{Y} ) = R(\boldsymbol{W}^{(i)}, \boldsymbol{b}^{(i)})$

$$ \begin{aligned}
    \frac{\partial R^{(n)}}{\partial W^{(n-k)}}  =&  \frac{dR^{(n)}}{dA^{(n)}} \frac{dA^{n}}{d (Z^{(n)}+Z^{(n-k)})}\frac{\partial (Z^{(n)}+Z^{(n-k)})}{\partial W^{(n-k)}}
    \\ = &\frac{dR^{(n)}}{dA^{(n)}} \frac{dA^{n}}{d (Z^{(n)}+Z^{(n-k)})}\frac{\partial Z^{(n)}}{\partial W^{(n-k)}} 
    \\& +\frac{dR^{(n)}}{dA^{(n)}} \frac{dA^{n}}{d (Z^{(n)}+Z^{(n-k)})}\frac{\partial Z^{(n-k)}}{\partial W^{(n-k)}}
    \\ = & \frac{dR^{n}}{dA^{(n)}} g'(Z^{(n)}+Z^{(n-k)}) \prod^{n-k}_{i=n-1}g'(Z^{(i)})\prod^{n-k+1}_{j=n}W^{(j)}\frac{\partial Z^{(n-k)}}{\partial W^{(n-k)}}
    \\ & +\frac{dR^{(n)}}{dA^{(n)}}  g'(Z^{(n)}+Z^{(n-k)})  \frac{\partial Z^{(n-k)}}{\partial W^{(n-k)}}
    \\ \rightarrow & \frac{dR^{(n)}}{dA^{(n)}}  g'(Z^{(n)}+Z^{(n-k)})  \frac{\partial Z^{(n-k)}}{\partial W^{(n-k)}}
\end{aligned} $$

**Understanding** A skip connection will eliminate the intermediate process from $(n-k)$-th layer to $n$-th layer, according to the chain rule, it will eliminate the matrix/derivative products from $(n-k)$-th to $n$-th layer. 

- I.e. skip connection directly pass the gradient from $n$-th to  $(n-k)$-th layer
- I.e. as long as the grad in $n$-th layer is non-zero, the grad in $(n-k)$-th layer will also be non-zero


### 2.2. Change activation function: 

Changing activation function could alleviate the gradient vanishing. Especially when you use sigmoid.

Use ReLu, tanh rather than sigmoid

**Personal understanding**: the gradient is product of later weight and derivative of the activation function, if the derivative of activation function is small, then gradient will also vanish.

LeakyRelu > ReLu: Relu (x<0) grad = 0
ReLu > tanh: Relu constant grad, grad=1 (x>0); tanh grad $\in [0,1]$ (see ref)
tanh > sigmoid: tanh have sharper shape.


- Ref: 
  - [TowardsDataScience](https://towardsdatascience.com/comparison-of-activation-functions-for-deep-neural-networks-706ac4284c8a): Derivative of different activation function. 
  - [TowardsDataScience](https://towardsdatascience.com/the-vanishing-gradient-problem-69bf08b15484): confirm changing activation can reduce grad vanishing 
  - [StackExchange](https://stats.stackexchange.com/questions/126238/what-are-the-advantages-of-relu-over-sigmoid-function-in-deep-neural-networks): confirm changing activation can reduce grad vanishing 
  - [Blog](https://www.machinecurve.com/index.php/2019/09/04/relu-sigmoid-and-tanh-todays-most-used-activation-functions/#in-short-the-relu-sigmoid-and-tanh-activation-functions): confirm changing activation can reduce grad vanishing 
  
