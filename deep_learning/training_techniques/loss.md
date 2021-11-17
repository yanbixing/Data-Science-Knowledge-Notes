# Loss

## 1. Cross Entropy Loss

### 1.1 Cross Entropy Loss (CE)

Most common multi-classification loss/risk.

$$l:= R: = H := \sum_k P^*_k \log \hat{P}_k$$
i.e.

The individual sample contribution is:

$$l(x_i,y_i) = \log \hat{P}_{y=y_i}(x_i)$$

### 1.2 Focal loss

Ref: [PaperWithCode](https://paperswithcode.com/method/focal-loss)

Loss based on CE, Add a factor before logP, 

$$l(x_i,y_i) = \log \hat{P}_{y=y_i}(x_i)$$

so we can have more punishment on "hard" misclassified samples, less punishment when the correct class have high prob. i.e.

<div  align="center"><img src=https://production-media.paperswithcode.com/methods/Screen_Shot_2020-06-07_at_4.45.06_PM_leJm2yh.png style = "zoom:30%"></div> 


## 2. Other DL loss

### 2.1. Triplet Loss

Ref: [Medium](https://medium.com/analytics-vidhya/triplet-loss-b9da35be21b8)

The loss function have three input, prediction $A$, positive (a correct) "label" $P$, and negative (an incorrect) "label" $N$.

$${\displaystyle {\mathcal {L}}\left(A,P,N\right)=\operatorname {max} \left({\|\operatorname {f} \left(A\right)-\operatorname {f} \left(P\right)\|}^{2}-{\|\operatorname {f} \left(A\right)-\operatorname {f} \left(N\right)\|}^{2}+\alpha ,0\right)}$$


The loss value punish the distance between prediction and incorrect "label" and reward correct "label".

Typically, the prediction and label here is in a vector form, and the loss is small when prediction is similar to correct info and large when loss is similar to incorrect info. E.g.

<div  align="center"><img src=https://miro.medium.com/max/1302/1*SKWGC3ehCbGCsbJVge6kmg.png style = "zoom:60%"></div> 



