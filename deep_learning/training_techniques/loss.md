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



### Deep Dive: Ranking loss vs CE? (TBD)

Also see: [sysdesign_interview.md](../../ranking_system/sysdesign_interview.md)


Seems both CE and ranking loss (pairwise-loss or triplet loss) can also be used in ranking models (Learn To Rank, LTR task): 

Ref: 

- [Medium](https://hemanthkumargadi.medium.com/introduction-to-pairwise-loss-function-a47f2282f16): RankSVM, RankBoost, RankNet user pairwise loss
    <div  align="center"><img src=https://miro.medium.com/max/916/1*DORaAoXpUSlIiMU1vA2Sxw.jpeg style = "zoom:60%"> </div>

  - function $\phi$ can be hinge, exponential or logistic.
- [GithubBlog](https://gombru.github.io/2019/04/03/ranking_loss/): pairwise loss and triplet loss for image embedding.
  - Pairwise loss: $L(r_0,r_1,y) = y\left \| r_0 - r_1 \right \| + (1-y) max(0,m - \left \| r_0 - r_1 \right \|)$
  - Triplet loss: $L(r_a,r_p,r_n) = max(0,m + d(r_a,r_p) - d(r_a,r_n))$
- Pairwise loss takes $(r_0,r_1,y)$, from the format of data,pairwise ranking should use pair wise loss.

Other refs:

- [Github-blog-Intro_to_LTR](https://everdark.github.io/k9/notebooks/ml/learning_to_rank/learning_to_rank.html): RankNet use cross entropy.
- [Medium](https://medium.com/analytics-vidhya/triplet-loss-b9da35be21b8): Ranking loss is often used to train network that create embedding, output is the similarity score.
<!-- - [DL-loss.md](../deep_learning/training_techniques/loss.md) -->