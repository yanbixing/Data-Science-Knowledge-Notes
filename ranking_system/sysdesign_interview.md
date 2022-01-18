# SysDesign Interview Framework

## RecSys SysDesign

### Clarifying Questions

- Ask what is the high-level / long-term goal? 
  - Used to define long-term/online metric.
  - Ads:
    - Just action rate
    - Or the value score, should be decomposed to (bid, action rate, quality)
- Ask what is the specific prediction target of our model?
  - Use to define model-selection/offline metric.
  - Ads:
    - Action rate: click? conversion?
      - build a model to predict the user's probability of click (action)
      - framework is more like ranking system.
    - Quality score: 
      - build a model to generate predefined quality score based on other user
        - framework is more like collaborative filtering.
      - quality score can be used as a feature for action rate prediction.
        - Or can be used as an individual factor to consider, avoid user-aversion/providing-unfavored-info to annoy user.
  - News feed/Posts:
    - Rate of likes/

<!-- - Ask multi-stage or just one? 
  - Multi-stage need to consider different data size/precision requirement on different
  - Multi-stage need ot consider different input feature and output label. -->


### Model Selection

- Ask what is the output label?
- Feature exploration, input of the model
  - The above features for feature-based models.
  - The user-item matrix (past history) for collaborative filtering
- Models selection:
  - Collaborative filtering - use when only have user-item rating matrix:
    - Ref: [CF.md](./recommender_system/collaborative_filtering.md)
    - Similarity based:
      - user-based: user-similarity weighted * rating scores of the same item from different users.
        - Use when
          - user often change, item not: Then, no need to recalculate user-user similarity, save computational resources.
          - more item than user: the user-user similarity much less item-item similarity, save space.
      - item-based: item-similarity weighted * rating on different items from the same user.
        - Use when
          - item often change, user not: Then, no need to recalculated item-item similarity, save computation resources.
          - more user than item: the item-item similarity much less user-user similarity, save space.
    - Model based CF: 
      - neural CF: rather than calculate item/user similarity directly from the columns/rows as embedding, the input is the user and item's one hot encoding, and output is the rating score
        - Besides rating score, we can get a embedding of user and item.
  - Content-based model:
    - Similarity based:
    - Model based:
    <!-- - pointer wise, predict the click prob.
      - Classical or NN, binary classification
    - pairwise (ranking) model, predict the ranking score or just the order.
      - Classical or NN, binary classification
      - Typically NN (RankNet, LambdaNet) -->
  
### Evaluation metrics

Evaluation:
  - One position: precision, recall
  - Multiple positions: ranking metrics precision@K, MRR, etc.
  - Rating score: MSE (mean square error)


### Deployment

### Other aspects to consider

long term short term
online, offline

cold start




## SEO SysDesin

- Ask what is the high level goal?
  - Just action rate
  - Or the value score, should be decomposed to (bid, action rate, quality)
- Ask what is the specific prediction target of our model
  - Action rate: click? conversion?
    - build a model to predict the user's probability of click (action)
    - framework is more like ranking system.
  - Quality score: 
    - build a model to generate predefined quality score based on other user
      - framework is more like collaborative filtering.
    - quality score can be used as a feature for action rate prediction.
      - Or can be used as an individual factor to consider, avoid user-aversion/providing-unfavored-info to annoy user.
- Ask multi-stage or just one? 
  - Multi-stage need to consider different data size/precision requirement on different
  - Multi-stage need ot consider different input feature and output label.
- Ask what is the output label.
- Feature exploration
  - The above features for feature-based models.
  - The user-item matrix (past history) for collaborative filtering
- Models selection:
  - Feature-based model:
    - pointer wise, predict the click prob.
      - Classical or NN, binary classification
    - pairwise (ranking) model, predict the ranking score or just the order.
      - Classical or NN, binary classification
      - Typically NN (RankNet, LambdaNet)
  - Collaborative filtering:
    - user-based: use-similarity weighted * rating scores of the same item from different users.
    - item-based: item-similarity weighted * rating on different items from the same user.
- Evaluation:
  - One position: precision, recall
  - Multiple positions: ranking metrics precision@K, MRR, etc.
  - Rating score: MSE (mean square error)