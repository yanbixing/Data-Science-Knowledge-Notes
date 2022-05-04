# Overfitting and Underfitting

## 1. Overfitting 

### 1.1. Definition and detection

Definition: model describes random error/noise instead of underlying relationship. When a model is excessively complex.

Behavior: 

- training error is low 
- test val error is high



### 1.2. Solutions

- Reduce model complexity
- More data / Data augmentation
- Regularization
- Early stopping
- DNN
  - Dropout: prevent you from too much focus on particular feature
    
  - Batch normalization
- Feature elimination (delete noise features)
  - Similar to reduce model complexity.
- Model Ensemble (Ensemble learning)
    - Ensemble different models to reduce variance, like bagging trees.
- Cross validation: mainly help you to **detect** overfitting and **tune hyperparameter** (model complexity.)
  - Ref: [StakeExchange](https://stats.stackexchange.com/questions/9053/how-does-cross-validation-overcome-the-overfitting-problem), [CMU-slide](https://clm.utexas.edu/fietelab/QuantNeuro/readings/crossvalidation_slides_Moore_CMU.pdf), [EliteDataScience](https://elitedatascience.com/overfitting-in-machine-learning)
  
- Ref: [KDNuggets](https://www.kdnuggets.com/2019/12/5-techniques-prevent-overfitting-neural-networks.html),[EliteDataScience](https://elitedatascience.com/overfitting-in-machine-learning),[TowardsDataScience](https://towardsdatascience.com/overfitting-vs-underfitting-a-complete-example-d05dd7e19765), [AnalyticsIndiaMagazine](https://analyticsindiamag.com/tackling-underfitting-and-overfitting-problems-in-data-science/)


### 1.2.1. Dropout

- Why dropout works?
  - Overfitting happens when the result is highly dependent on particular neurons (features).
  - Dropout can can <mark style="background-color:yellow;"><font color="#0000dd">**prevent the network from relying/focusing too much on particular neurons**</font></mark>
  - The <mark style="background-color:yellow;"><font color="#0000dd">larger dropout ratio</font></mark>, the less importance/influence of an individual neural, the less variance, the <mark style="background-color:yellow;"><font color="#0000dd">less model complexity</font></mark>.
- How to dropout?
  - In each batch/step, randomly set some features (neuron, $x$) to zero
    - So this feature has no contribution to next layer at all.
    - Also no grad from this feature. I.e. When doing backpropagation, grad contributed from this feature to prev layer is 0.
    - Note:
      - Different batch have different neurons to dropout; if we forever drop one node, it is equivalent to remove it, it is useless.
    - Ref: [Pytorch-SourceCode-Dropout](https://pytorch.org/docs/stable/generated/torch.nn.Dropout.html), [PersonalBlog](https://agustinus.kristia.de/techblog/2016/06/25/dropout/), [StackOverflow](https://stackoverflow.com/questions/68631000/implementing-dropout-with-pytorch)
  - Following-up: why set feature $x$ rather than weight $w$ to zero?
    - Set feature $x_i \to 0$, $\Rightarrow$ the contribution from $x_i$ to all $y_j$ in next layer is 0.
    - Set weight $w_{ij} \to 0$, $\Rightarrow$ the contribution from $x_i$ to other $y_k$ is not 0.
    - Here, our purpose is to avoid reliance on particular **feature**, so we need to set feature to 0.
- Different between training and test for "dropout"?
  - Train: randomly select a ratio of neuron (feature, not weight) to zero.
  - Test: no dropout, use all feature
  - Ref: ? (TBD)




## 2. Underfitting:

Def: model cannot adequately capture the underlying structure of the data.

[Wiki-Overfitting](https://en.wikipedia.org/wiki/Overfitting)

### 2.1. Definition and detection

Behavior: 

- training error is high 
- test val error is also high


### 2.2. Solutions

- More complex model
- Increase training time

Ref: [EliteDataScience](https://elitedatascience.com/overfitting-in-machine-learning),[TowardsDataScience](https://towardsdatascience.com/overfitting-vs-underfitting-a-complete-example-d05dd7e19765), [AnalyticsIndiaMagazine](https://analyticsindiamag.com/tackling-underfitting-and-overfitting-problems-in-data-science/)






### Other Qs:


#### Drop out

