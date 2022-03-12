# Evaluation metrics

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
