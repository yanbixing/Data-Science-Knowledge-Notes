# Max pooling


Ref: [StackExchange](https://stats.stackexchange.com/questions/288261/why-is-max-pooling-necessary-in-convolutional-neural-networks)

Average pooling help to smoothing the features.
Max pooling help to extract the "sharpest" features, feature with most significant value.


Ref: [StackExchange](https://stats.stackexchange.com/questions/486376/why-do-we-even-need-max-pooling-layers)
Pooling layers create a lower resolution version of the input that still contains the large or important structural elements.



### Gradient descent on pooling layer.

#### Max-pooling

**Understanding:** Max-pooling will only pass the gradient to position has the max value, since only that position is passed to next layer. 

- The gradient for other position is 0, since the final result have no relation with them.

Ref: [DSStackExchange](https://datascience.stackexchange.com/questions/11699/backprop-through-max-pooling-layers) [StackOverflow](https://stackoverflow.com/questions/40712031/backpropagation-in-pooling-layer-subsamplig-layer-in-cnn)

Note(?TBD): In implementation, can use $argmax$ function to find the position.

#### Avg-pooling

**Understanding:** Each position contribute its $\frac{1}{N}$ to $\frac{1}{N}$ of next layer. So each position receive $\frac{1}{N}$ grad from next layer.


**Quick proof:**

$\boldsymbol{X}^{(1)}$ is N-dim vector
$X^{(2)} =AVG(\boldsymbol{X}^{(1)})= \frac{1}{N}\sum^N_{i=1} X^{(1),i}$
$R = R(X^{(2)})$

then:

$\frac{\partial R}{\partial X^{(1),i}} = \frac{d R}{d X^{(2)}} \frac{\partial X^{(2)}}{ \partial X^{(1),i}} = \frac{1}{N} \frac{\partial R}{\partial X^{(2)}}$


Ref: [Stackoverflow](https://stackoverflow.com/questions/36628181/backpropagation-in-convolutional-neural-networks)