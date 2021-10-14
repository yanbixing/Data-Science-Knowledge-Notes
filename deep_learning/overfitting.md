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
    - The larger dropout ratio, the less importance/influence of an individual neural, the less variance, the less model complexity.
  - Batch normalization
- Feature elimination (delete noise features)
  - Similar to reduce model complexity.
- Model Ensemble (Ensemble learning)
    - Ensemble different models to reduce variance, like bagging trees.
- Cross validation: mainly help you to **detect** overfitting and **tune hyperparameter** (model complexity.)
  - Ref: [StakeExchange](https://stats.stackexchange.com/questions/9053/how-does-cross-validation-overcome-the-overfitting-problem), [CMU-slide](https://clm.utexas.edu/fietelab/QuantNeuro/readings/crossvalidation_slides_Moore_CMU.pdf), [EliteDataScience](https://elitedatascience.com/overfitting-in-machine-learning)
  
- Ref: [KDNuggets](https://www.kdnuggets.com/2019/12/5-techniques-prevent-overfitting-neural-networks.html),[EliteDataScience](https://elitedatascience.com/overfitting-in-machine-learning),[TowardsDataScience](https://towardsdatascience.com/overfitting-vs-underfitting-a-complete-example-d05dd7e19765), [AnalyticsIndiaMagazine](https://analyticsindiamag.com/tackling-underfitting-and-overfitting-problems-in-data-science/)






## 2. Underfitting:

### 2.1. Definition and detection

Behavior: 

- training error is high 
- test val error is also high


### 2.2. Solutions

- More complex model
- Increase training time

Ref: [EliteDataScience](https://elitedatascience.com/overfitting-in-machine-learning),[TowardsDataScience](https://towardsdatascience.com/overfitting-vs-underfitting-a-complete-example-d05dd7e19765), [AnalyticsIndiaMagazine](https://analyticsindiamag.com/tackling-underfitting-and-overfitting-problems-in-data-science/)