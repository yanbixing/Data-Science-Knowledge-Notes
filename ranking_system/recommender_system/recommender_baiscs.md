
# Recommender System

## 1. What is recommendation task

In recommendation task, we usually need to predict a user's preference on an item.

Ref: [Textbook: Recommender Systems: An Introduction](https://www.google.com/books/edition/Recommender_Systems/eygTJBd_U2cC?hl=en&gbpv=0)


## 2. Different types of RecSys

### 2.1. Collaborative filtering:

Predict missing rating score purely based on user-item rating matrix. 

Ref:
- [Wiki-RecSys-CF](https://en.wikipedia.org/wiki/Recommender_system#Collaborative_filtering)
- [TowardsDataScience](https://towardsdatascience.com/recommendation-systems-a-review-d4592b6caf4b)

#### 2.2.1. Memory-based

Calculated user-user or item-item similarity directly use the rows or columns as embedding.

#### 2.2.2. Model based

Still only rely on user-item rating matrix, but may use some complex model to predict the rating, like neural CF.


### 2.2. Content-based



Rather than relying on user-item matrix. Content based model rely on **features** of **items**.

Ref:
- [Textbook: Recommender Systems: An Introduction](https://www.google.com/books/edition/Recommender_Systems/eygTJBd_U2cC?hl=en&gbpv=0): P51, P52 (Chp 3.1.)
- [TowardsDataScience](https://towardsdatascience.com/recommendation-systems-a-review-d4592b6caf4b)


#### 2.2.1. Similarity-based (Memory-based):


- Calculated item-item similarity based on the item features
- estimate the user's rating on current item based on user's historical rating on similar items, 
- usually use item-item similarity as weight and take the weighted average of user's historical ratings.


##### Comparison vs Item-based CF:

Similarity-based content-based ReySys is similar to item-based CF: 

- Both algo give the user's rating on new item as a weighted averaging rating of user's previous item.

The difference is:

- content-based similarity based algo: use item's features to calculate item-item similarity
- item-based CF: use column (item's user embedding) to calcuate item-item similarity

  
#### 2.2.2. Model-based:

Model-based content-based RecSys can be viewed as a classification problem. Since besides item similarity, we also require info on user's preferance, so, usually, we also require user features to build a model.

E.g. Directly fit model with item and user features as input and rating score as output.


### 2.3. Hybrid

Include info from both user-item rating score matrix and features.

[TowardsDataScience](https://towardsdatascience.com/recommendation-systems-a-review-d4592b6caf4b)



## 3. Evaluation metrics

- Ranking metrics:
  - Precision/Recall/F1@K: the precision/recall/F1 if we only consider the top K items.
  - Other common ranking metrics: 
    - MAP(mean average precision): order-sensitive version of precision@k
    - MRR (mean reciprocal ranking): mean of inverse of the rank of first correct item, the higher ranking of first correct item, the higher score
    - DCG@K (discounted cumulative gain): the higher relevance score of top-k items, the higher score.
  - Ref: [GithubBlog-LTR](https://everdark.github.io/k9/notebooks/ml/learning_to_rank/learning_to_rank.html)
- MSE/RMSE: used when our focus on score, rather than the correctness of the item ranking. I.e. we want to get more accurate rating score, then we should minimize the (root-) mean-squared-error.


## 4. Typical problems

[Wiki-RecSys-CF](https://en.wikipedia.org/wiki/Recommender_system#Collaborative_filtering)

### 4.1. Cold start

[PersonalBlog](https://kojinoshiba.com/recsys-cold-start/)

Two aspects:

- new user
- new item

Solution: 

- Multi-arm bandit:
  - Consider exploration vs exploitations tradeoff in new items.
  - Personal understanding: Optimize the system gradually at the same time while collect the user behavior data.
- Content/feature based:
  - Explore further info like text, social networks to get similarities.

<!-- - Representative based:
  - Find subset of items and users that similar to the new item. -->



Other ref: [Wiki-ColdStart](https://en.wikipedia.org/wiki/Cold_start_(recommender_systems))

### 4.2. Scalability (TBD)

### 4.2. Sparsity (TBD)