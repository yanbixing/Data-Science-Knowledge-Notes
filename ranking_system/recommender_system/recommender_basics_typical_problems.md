# RecSys Typical problems (TBD)

[Wiki-RecSys-CF](https://en.wikipedia.org/wiki/Recommender_system#Collaborative_filtering)

## 1. Model training/prediction related problems

### 1.1. Cold start

For new user and new items, there is few data to rely on to make recommendation.

Solution: 

- Representative based (?TBD)
  - Idea: Try to find the group the user belong to (with the limited info), and use the group average to infer the user's preference.
    - Sounds to me like K-means
  - Pro: consistent with CF idea.
  - Con: cannot work with absolutely zero info, still inaccurate while few interactions.
- Content based:
  - Idea: rather than user behavior, explore other content features like text, social networks to get the similarities.
  - Pro: better the only user behavior info
  - Con: feature exploration is time-spending and tricky.
- Multi-arm bandit methods:
  - Idea: you need do more tests to understand new user better; random ranking/sampling is comprehensive but will harm CTR (loss), use MAB method can help you to reduce loss while collecting data.
  - Treat the problem as a MAB problem: i.e. when the system is unknown to us, 
    - on one hand, we need test different actions to "explore" the rule/feature/parameter in the system; 
    - on the other hand, we will "exploit" the past observation to find the best actions to maximize the reward.
  - I.e. Optimize the system gradually: 
    - like online learning: we will update system while collecting user behavior
    - like randomization: the low-rank samples also have a chance to be put on top to gather more comprehensive data.
  - Pro: Reduce loss while exploration
  - Con: Need more design and engineering work to implement such process to the system.
- Deep learning:
  - There are some DL methods can help to reduce cold-start (sparse feature) problem, like dropout, can reduce our reliance on particular feature.


<!-- - Representative based:
  - Find subset of items and users that similar to the new item. -->

Ref:

- [Wiki-RecSys-CF](https://en.wikipedia.org/wiki/Recommender_system#Collaborative_filtering)
- [PersonalBlog](https://kojinoshiba.com/recsys-cold-start/): different strategy/methods to solve cold start

Other ref: [Wiki-ColdStart](https://en.wikipedia.org/wiki/Cold_start_(recommender_systems))



### 1.2. Scalability (TBD)

Amount of user/item is huge, it is computationally expensive to add new user/item when there are more and more users.

- Solution:
  - Online learning: update parameter for each new item/user, sequentially.
  - Distributed/parallel computation.
  - Personal understanding: should works for model-based RecSys, i.e, a model to predict the rating score based on user feature and item feature.
  - CF-based:
    - Prediction for new user/item should not be problem, just model update may be problem?
    - Clustering: rather than user-item matrix, perform recommendation on UserCluster-ItemCluster matrix. Thus, the size of matrix will be much smaller and no need to update so frequently.

- Ref:
  - [Wiki-RecSys-CF](https://en.wikipedia.org/wiki/Recommender_system#Collaborative_filtering): issue.
  - [MIT-PhD-Thesis](https://dspace.mit.edu/bitstream/handle/1721.1/99785/927438195-MIT.pdf): Different solutions for scalability issue. Online leanring and distributed computing
  - [Paper](https://www.researchgate.net/publication/338096172_Scalable_Recommendations_Using_Clustering_Based_Collaborative_Filtering): Clustering to solve scalability issues.
    - "The most related model to this paper is the clustering-based CF model. The objective of any clustering algorithms to form groups (clusters) of similar users or similar items.Recommendation task is then performed using the resultant clusters. Predictions for a target user in a particular cluster are generally computed on the basis of the average ratings of the users in that cluster."

### 1.3. Sparsity (TBD)

The amount of user and items are huge, the actions are limited, so the matrix is sparse.

- Solution:
  - Data imputation: fill missing value with substitute values.
    - Ref: [Wiki-DataImputation](https://en.wikipedia.org/wiki/Imputation_(statistics))
  - Dimension reduction: PCA or SVD (matrix factorization?).
    - PCA keep the highest variance direction
    - NMF (Non-negative Matrix Factorization)
      - $\boldsymbol{X}_{mn} = \boldsymbol{W}_{m\tau}\boldsymbol{H}_{\tau n}$
        - $\boldsymbol{W}_{m \tau}$: the user-topic matrix, will keep the user similarity
          - (TBD) Should be able to be used to convert new user-item vector to topic-item vector, as long as the user dimension not change.
        - $\boldsymbol{H}_{\tau n}$: the topic-item matrix, will keep the item similarity.
          - (TBD) Should be able to be used to convert new user-item vector to user-topic vector, as long as the item dimension not change.
      - NMF vs SVD:
        - NMF have no negative values, more suitable for RecSys
        - SVD have negative values, less suitable for RecSys
      - Ref: [matrix_factorization.md](../../general_machine_learning/math_topics/matrix_factorization.md)
  - <mark style="background-color:yellow;">**!!!Caution!!!:** Don't fill missing values when doing MF!</font></mark>
    - MF seek to reduce the reconstruction error.
      - Missing value will be skipped during the process.
      - 0 will be thought as lowest rating score, not skipped.
    - PCA seek to find the direction with max variance
      - Filling value can influence the mean as well as variance
      - <mark style="background-color:yellow;">There are specialized algorithms for PCA/SVD algos to deal with missing value</font></mark>, like 
        - SVD impute: fill missing value with linear combination of most significant variable.
        - Probabilistic PCA: missing value is treated as parameter to be optimized during EM process.
        - Ref: 
          - [Medium-blog](https://medium.com/@seb231/principal-component-analysis-with-missing-data-9e28f440ce93): different missing value PCA/SVD algos. 
          - [TensorFlowDoc](https://www.tensorflow.org/probability/examples/Probabilistic_PCA): Implementation of probabilistic PCA.
          - [IIT-ProjReport](https://adityakumarakash.github.io/rnd/RndReport.pdf): missing value will be treated as parameter to be learned during the EM process.
          - [StackExchange](https://stats.stackexchange.com/questions/283515/principal-component-analysis-with-missing-data): Typically cannot implement SVD/PCA on matrix with missing value.

- Ref:
  - [Wiki-RecSys-CF](https://en.wikipedia.org/wiki/Recommender_system#Collaborative_filtering)


## 2. Data collection related problems

### 2.1. Position bias

- **Definition:** User tends to interact with items with higher rank, regardless the true relevance of the item. 
  - Also called "under-explored problem"

- **Problem:** The true relevant items in low rank may be missed. 

- **Solution:**
  - Introduce **Randomization** to ranking service. 
    - E.g., 2% of requests will get random candidates, and 98% will get sorted candidates from Ad Ranking Service.
    - Cons: Hurt user experience


- Ref:
  - [Educative-MLSysDesign](https://www.educative.io/courses/machine-learning-system-design/xlrLVmXRDnq): popular interview question. "Under-explored"
  - [USTC-Slide](http://staff.ustc.edu.cn/~hexn/slides/sigir21-tutorial-bias-slides.pdf):Defnition and solution.
  - [ArxivPaper](https://arxiv.org/pdf/2001.11358.pdf): Definition. "Position bias makes higher ranked results (e.g., top search engine results) more likely to be clicked even if they are not the most relevant."
  - [ArxivPaper](https://arxiv.org/pdf/2010.03240.pdf): Definition and solution. "Position bias happens as users tend to interact with items in higher position of the recommendation list regardless of the items’ actual relevance so that the interacted items might not be highly relevant."


### 2.2. Selection bias (ToBeFurtherExplored)

- Ref:
  - [USTC-Slide](http://staff.ustc.edu.cn/~hexn/slides/sigir21-tutorial-bias-slides.pdf):Defnition and solution.
  - [ArxivPaper](https://arxiv.org/pdf/2010.03240.pdf): the paper corresponding to the above slides.

- Cause: comes from user behavior, i.e., user may choose the item they like to rate, the rated items may be not representative
- Problem:  the not selected items is a kind of **missing value**, we can use missing value filling methods to solve this problem.
- Solution: Fill missing value.
  - E.g. Average filling, train a model to predict, etc.


### 2.3. Conformity bias (ToBeFurtherExplored)

- Ref:
  - [USTC-Slide](http://staff.ustc.edu.cn/~hexn/slides/sigir21-tutorial-bias-slides.pdf):Defnition and solution.
  - [ArxivPaper](https://arxiv.org/pdf/2010.03240.pdf): the paper corresponding to the above slides.

- Cause: the user's opinion/rating may be affected by the population or his local environment.
- Problem: the observed rating not reflect the user's true rating, not helpful to predict user's true action.
- Solution (TBD): 
  - disentangling: train a model to learn user's true ratings based on observed rating and the local environment ratings. E.g.
    - Input:
      - local environment ratings, like the average rating of people connected with the user
      - user features, like the user's background info or user's preference
    - Output: observed user rating
    - Target: from the user feature, we can add an embedding layer "user interest embedding" on its top, and the embedding can be the "true user rating", because it is generated from user feature, free from environment preference.
    - Implementation: a binary classification NN
      - Feature 1: click rate of the population
      - Other features: user features
        - There is a 1 neuron embedding on top of user features, represent the true click prob of the user, if not affected by population
      - Output: the observed click-or-not behavior of user
        - It is a weighted sum of public click-prob and user's true-click prob.
      - Target: we want to get the mapping relation from feature to the true click prob.
    - Ref: 
      - [YouTube-THU-Presentation](https://www.youtube.com/watch?v=BaWePjtxbOs): binary classification structure understanding.
        <div  align="center"><img src=./recommender_basics_asset/disentangling_as_embedding_model.png style = "zoom:100%"> </div>
      - [USTC-Slide](http://staff.ustc.edu.cn/~hexn/slides/sigir21-tutorial-bias-slides.pdf): embedding understanding.
        <div  align="center"><img src=./recommender_basics_asset/disentangling_as_binary_classification_model.png style = "zoom:100%"> </div>
    
### 2.4. Exposure bias (ToBeFurtherExplored)

- Ref:
  - [USTC-Slide](http://staff.ustc.edu.cn/~hexn/slides/sigir21-tutorial-bias-slides.pdf):Defnition and solution.
  - [ArxivPaper](https://arxiv.org/pdf/2010.03240.pdf): the paper corresponding to the above slides.

- Cause: because of policy/webpage-design/algorithm-design, only part of data is exposed to user
  - **Personal understanding**: position bias may be also a kind of exposure bias due to algorithm.
- Problem: the data collection is not comprehensive, i.e. not a fair reflection/representation of user's true preference.
- Solution: 
  - Sampling (TBD):
    - May be find evaluator to do manual labelling for missing values?
    - Or randomization to reduce the risk of non-exposure for lower ranked samples?


## 3. Real application problems

### 3.1. Diversity

Ref: 
- [Educative-Grokking_ML_interview-Feed_based_system](https://www.educative.io/courses/grokking-the-machine-learning-interview/B1X08zyx05N)


**Problem**:

- Items with very similar property will have similar value scores 
- Show similar item consecutively both no help and may affect the user experience

**Solution**:

- Give "repetition penalty" if similar items ranked consecutively.
  - e.g. Final score = raw score - 0.1
    - "0.1" here is the repetition panelty
- Shift down the rank if similar items ranked consecutively.
  - e.g. Final rank = raw rank + 5

Note (personal understanding): seems there is no way to improve the diversity from model training stage, since, our model generate score rather than directly the rank. Similar items definitely give similar score. ( If not, i.e. if similar item give different score, it means the model have high variance error, i.e., model is overfitting.....)