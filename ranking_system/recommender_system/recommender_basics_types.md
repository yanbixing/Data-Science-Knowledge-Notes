
# Different types of RecSys models

## 0. What is recommendation task

In recommendation task, we usually need to predict a user's preference on an item. 
Ref: [Textbook: Recommender Systems: An Introduction](https://www.google.com/books/edition/Recommender_Systems/eygTJBd_U2cC?hl=en&gbpv=0)


## 1. Collaborative filtering:

Predict missing rating score purely based on user-item rating matrix. Or, in other words, only based on $(U,I,R)$ triplet. ($U$: user, $I$: item, $R$: rating)


- Assumption: 
  - people who agreed in the past will agree in the future
  - they will like similar kinds of items as they liked in the past
  - Ref: [Wiki-RecSys-CF](https://en.wikipedia.org/wiki/Recommender_system#Collaborative_filtering)

Ref:
- [Wiki-RecSys-CF](https://en.wikipedia.org/wiki/Recommender_system#Collaborative_filtering)
- [TowardsDataScience](https://towardsdatascience.com/recommendation-systems-a-review-d4592b6caf4b)
- [JMLR-Paper-P627](https://www.jmlr.org/papers/volume10/takacs09a/takacs09a.pdf): "$(U,I,R)$ triplet" representation of matrix info, better representation when the matrix have missing value.

### 1.1. Memory-based CF

Directly rely calculated the unknown rating as a similarity weighted known rating score. E.g. user-based memory-based CF:

$$r_{mn} = \frac{\sum^M_{i=1} \mathrm{sim}(\boldsymbol{u}_m,\boldsymbol{u}_i)r_{in}}{\sum^M_{i=1} \mathrm{sim}(\boldsymbol{u}_m,\boldsymbol{u}_i)}$$

The similarity is the cosine similarity between different rows (user-based) or columns (item-based)

Other memory based models

- KNN: Find the k-nearest neighbour to do weighted average.

Ref:
- [Wiki-RecSys-CF](https://en.wikipedia.org/wiki/Recommender_system#Collaborative_filtering): KNN is memory based model
- [memory_based_cf.md](./memory_based_cf.md): basics of memory based cf RecSys

### 1.2. Model-based CF

Still only rely on user-item rating matrix, but may use some complex model to predict the rating, like:

- Matrix Factorization: 
  - **Idea:** find the best decomposition (least reconstruction error) of matrix, and then we can get the rating by reconstruct the matrix.
  - Usually NMF, 
    - Aim to decompose $\boldsymbol{X}_{m\times n} \sim \boldsymbol{W}_{m\times \tau}\boldsymbol{H}_{\tau \times n}$
    - Method: treat all elements  in $\boldsymbol{W}_{m\times \tau}\boldsymbol{H}_{\tau \times n}$ as parameter, minimize reconstruction error. I.e.:
      -  $\boldsymbol{W}_{m\times \tau}\boldsymbol{H}_{\tau \times n} = \underset{\boldsymbol{W}_{m\times \tau}\boldsymbol{H}_{\tau \times n} }{\argmin} \mathrm{SSE}$
      - $\mathrm{SSE} = \sum_{(i,j)} ( r_{ij} - \sum_k w_{ik}h_{kj} )$
    - Prediction: $\hat{\boldsymbol{X}}_{m\times n} =\boldsymbol{W}_{m\times \tau}\boldsymbol{H}_{\tau \times n}$
    - Ref: [matrix_factorization.md](../../general_machine_learning/math_topics/matrix_factorization.md)
  - Similar methods includes: SVD, LDA
- Clustering models
  - **Idea:** Find the cluster of the target user, recommend him the item favored by the users in the cluster.
    - Ref: [ReviewPaper-P4](https://arxiv.org/pdf/2109.12839.pdf)
  - Pro (TBD): "Clustering models is immune to sparsity" 
    - The paper claims sparsity make similarity based model hard to define similar users. I.e. "inadequate for defining an effective list of similar users." But still hard to understand why clustering can handle sparsity.
    - Ref: [ReviewPaper-P4](https://arxiv.org/pdf/2109.12839.pdf)
    - Maybe because clustering is friendly to dimension reduction? Don't need to calculate similarity?
      - Ref: [ResearchGate](https://www.researchgate.net/post/Is_there_any_clustering_algorithm_preferable_if_one_has_to_deal_with_very_sparse_data)
- Neural CF
  - **Idea:** train a network based on existing $(U,I,R)$ info, predict unknown ratings.
    - Raw input is the one-hot encoding of user $U$ and item $I$
    - Output is the rating score
    - Besides rating, usually also aim to get embedding of different users and items.



## 2. Content-based



Rather than relying on user-item matrix. Content based model rely on
- **features** of **items**.
- target user's preference info:
  - usually historical rating data.
  - can also be **features** of **users**, like user profile

Ref:
- [Textbook: Recommender Systems: An Introduction](https://www.google.com/books/edition/Recommender_Systems/eygTJBd_U2cC?hl=en&gbpv=0): P51, P52 (Chp 3.1.)
- [TowardsDataScience](https://towardsdatascience.com/recommendation-systems-a-review-d4592b6caf4b)


### 2.1. Similarity-based:

The process is similar to item-based similarity-based CF:

I.e. Rating score is still item-item similarity weighted rating score of items in user history.



$$r_{mn} = \frac{\sum^N_{j=1} \mathrm{sim}(\boldsymbol{v}_n,\boldsymbol{v}_j)r_{mj}}{\sum^N_{j=1} \mathrm{sim}(\boldsymbol{v}_n,\boldsymbol{v}_j)}$$


The difference is that:

- Similarity-based content-based model calculated similarity based on item features
- Item-based similarity-based CF calculated similarity based on columns in the user-item matrix, only behavioral data, not content-related data.

  
### 2.2. Model-based:

Model-based content-based RecSys can be viewed as a typical supervised classification/regression problem. (Depending on whether it is binary decision, like purchase-or-not, or continuos value, like movie rating.)

Besides item features, we also require info on user's preference features, like:

- user's rating history
- user's profile info

So, usually, the model is like: 

- multi-class output
  - Input: user features
  - Output: rating of different items
  - I.e. $\boldsymbol{r} = f(\boldsymbol{u})$

- single-class output
  - Input: user features and item features
  - Output: rating of the item
  - I.e. $r_{uv} = f(\boldsymbol{u},\boldsymbol{v})$
  - Seems could be connected with ranking models? point-wise/pair-wise models.

Ref: [TowardsDataScience](https://towardsdatascience.com/recommendation-systems-a-review-d4592b6caf4b)

## 3. Hybrid

Include info from both user-item rating score matrix and features. Approaches:

- Implement content-based and CF-based model separately, combine their prediction.
- Incorporate content-based info to CF-based model
  - E.g. Add feature-based user-user similarity or item-item similarity to the CF model
- Use CF-based method in content-based model
  - Methods/Features depending on the whole corpus would be a general collaborative method E.g. 
    - Use MF to reduce user's rating history as 
    - Reduce dimension of other input features
    - Tfidf score vector of purchase history

Ref: [TowardsDataScience](https://towardsdatascience.com/recommendation-systems-a-review-d4592b6caf4b)



## 3. Evaluation metrics

- Offline metrics:
  - Ranking metrics:
    - Precision/Recall/F1@K: the precision/recall/F1 if we only consider the top K items.
    - Other common ranking metrics: 
      - MAP(mean average precision): order-sensitive version of precision@k
      - MRR (mean reciprocal ranking): mean of inverse of the rank of first correct item, the higher ranking of first correct item, the higher score
      - DCG@K (discounted cumulative gain): the higher relevance score of top-k items, the higher score.
    - Ref: [GithubBlog-LTR](https://everdark.github.io/k9/notebooks/ml/learning_to_rank/learning_to_rank.html)
  - MSE/RMSE: used when our focus on score, rather than the correctness of the item ranking. I.e. we want to get more accurate rating score, then we should minimize the (root-) mean-squared-error.
- Online metrics:
  - User engagement
  - Video Watched
  - Session watch time
    - Bounce rate?
  - Typically combined with A/B testing, to determine whether to launch the new model or not.


## Frequent Questions

- How to fill missing value?
  - Answer: 0 
  - Reason: no contribution to similarity.


## Other refs:

- [TowardsDataScience](https://towardsdatascience.com/how-did-we-build-book-recommender-systems-in-an-hour-part-2-k-nearest-neighbors-and-matrix-c04b3c2ef55c#:~:text=Collaborative%20Filtering%20Using%20k%2DNearest,of%20top%2Dk%20nearest%20neighbors.): KNN vs Matrix Factorization.