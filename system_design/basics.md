# ML System Design

## 1. Framework

### 1.0. Exception case: problem diagnosis (case interview)

Breaking down + metrics

### 1.1. End to end ML workflow

Ref: [1p3a](https://www.1point3acres.com/bbs/forum.php?mod=viewthread&tid=674603&ctid=233463)

- Data extraction (EDA)
  - Imbalance:
    - Four aspects: data, metric, model, change strategy.
  - Duplication:
    - Need to consider whether to remove duplicate samples.
- Preprocessing:
  - Numerical feature - transformation:
    - non-linear transformation (power, prod, log, binning) 
    - Better make it normal distribution. 
      - Important for distanced-based models (K-means, SVM)and models have assumption on data distribution (Naive Bayes, GMM, LinearRegression). 
      - See: [feature_scaling.md](../general_machine_learning/data_engineering/feature_scaling.md)
  - Categorical feature - encoding/vectorization
    - with order: integer(label) with order
    - no order: one-hot encoding
    - require specific encoding dim (usually << one hot encoding): hash encoding
      - Pros: tunable/variable dimension of encoding
      - Cons: cannot convert encoding back to feature.
      - Ref. [feature_engineering.md-Sec1.2.3.5](../general_machine_learning/data_engineering/feature_engineering.md)
    - high cardinality: mean encoding
  - Text feature: embedding (BOW, word2vec, Bert)
  - Missing value: forward/backword, mean, model filling
  - Outliers: cluster and remove or keep it.
- Model selection:
  - Model pros cons. 
    - See [summary_model_comparison.md](../general_machine_learning/models/summary_model_comparision.md)
- Hyper param tuning:
  - Grid search:
  - Random search: provide the distribution of param rather than exact point.
  - Bayesian optimization:
    - Sequential model-based optimization
    - Next point is selected based on previous point.
  - Cross Validation: use n-1 folds as training and 1 folds are evaluation iteratively.
  - Ref: [Medium-blog](https://medium.com/analytics-vidhya/comparison-of-hyperparameter-tuning-algorithms-grid-search-random-search-bayesian-optimization-5326aaef1bd1): GridSearch, RandomSearch, BayesianOptimization.
- Model evaluation:
  - Metric (ML metric, goal metric)
  - Offline eval: AUC, prec, recall, ranking metrics
  - Online eval: A/B test, compare performance between old and new model
- Model maintenance:
  - Dataset ( (x,y) joint distribution) shift
    - Ref：[TowardsDataScience](https://towardsdatascience.com/understanding-dataset-shift-f2a5a262a766): Dataset shift


### Interview tips
  
- L - RecoSys
  - Know that we can predict user rating with user-item matrix (collaborative filtering)
  - Besides user/item matrix, could dig other feature helps to predict user's reaction
    - Use user's reaction to other product to infer whether user is actively searching jobs
      - User's post, user's view, user's click on other people post, search query from user, etc
    - User's background info
      - User's skill set, user's past job title,
    - User-item relation
      - Whether user's interests/skill/yoe match the job.
  - Know that we should avoid "impression discounting"
    - Impression discounting: not good to recommend the same result to user again and again
    - When design RecoSys, should include such effect
      - Include features like how many times user have seen it (impCount, impression count), the latest time user saw it (last seen)
      - Or directly calibrate the rating score, like $new_score = orig_score * (w1 * g(impCount) + w2 * g(lastSeen))$
    - Ref: [WordPress-blog](https://buildingrecommenders.wordpress.com/2016/04/06/impression-discounting/)
  - Understand some complex concept and know how to solve
    - Diversification
    - Serendipity
    - Explore
  - Know long term/short evaluation metrics for rec sys
    - Short term: CTR
    - Long term: user activity, conversion rate, user's time on feeds)
    - Short term approximate of long term matrix
      - Bounce rating? Us
  - Know how to reason and solution between mismatch between online and offline performance
    - User's behavior data may be affect by previous models, 
      - the data didn't reflect the true behavior pattern of user (the best item is missed by RecSys, not provided to user.)
      - or, can be viewed as a kind of sampling bias?
      - Solution: always add same predicted negative sample to users, so we can know avoid miss FN for future training.
    - Dataset shift (different user population, time period, etc.)