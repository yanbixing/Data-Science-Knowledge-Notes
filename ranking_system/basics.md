# Basics

## Ranking (search engine) vs recommendation

- Ranking models (search engine): get ranking of different items given query.
  - Usually aim to get a function: $s = f(q,d)$
  - Usually, the q is explicit given.
  - Works based on item-query similarity
- Recommender: predict score of different items given user information.
  - Usually aim to get a rating function: $r = f(u,i)$
  - Usually, the feature of user is implicit, and we have a user-item rating matrix.
  - Seems we can also apply ranking models on recommendation, Ref: [LiveBook-RecSys](https://livebook.manning.com/book/practical-recommender-systems/chapter-13/11)
  - Usually Works based on item-item similarity, i.e. recommend items similar to previous high score items.


Ref:


[TowardsDataScience](https://towardsdatascience.com/recommendation-systems-a-review-d4592b6caf4b)
[Analytics](https://analyticsindiamag.com/why-a-product-search-engine-and-a-recommendation-engine-cant-replace-each-other/)
[BlogArticle](https://www.telusinternational.com/articles/what-are-search-recommendation-systems-in-machine-learning)


## 2.  Metrics

### 2.1. Ranking metrics:

Ref: [GithubBlog-LTR](https://everdark.github.io/k9/notebooks/ml/learning_to_rank/learning_to_rank.html)

- Precision/Recall/F-1@k
  - Mean average precision (MAP): a order-sensitive version of precision@k
- Reciprocal Ranking
  - Mean reciprocal ranking (MRR): 
    - Mean of inverse of rank of the first correct item in the predicted ranking order.
    - $MRR \equiv \frac{1}{Q} \sum_{i=1}^Q\frac{1}{r_i}$
  - Expected reciprocal ranking (ERR): (TBD)
- Discounted Cumulative Gain:
  - DCG@K：
    - $DCG@k := \sum_{i=1}^k\frac{2^{l_i} - 1}{log_2(i + 1)}$
    - Understanding: the metric rely on the relevance score of the top-k items, the higher relevance score, the higher metric value.



### 2.2. RecSys metrics

- Ranking metrics above.
- MSE/RMSE: used when our focus on score, rather than the correctness of the item ranking. I.e. we want to get more accurate rating score, then we should minimize the (root-) mean-squared-error. 