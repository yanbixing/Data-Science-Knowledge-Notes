# SysDesign Interview Framework

## 0. Problem statement: what we want to predict?

- Search engine: given (query, item), prediction CTR
  - Aims to rank item according to their CTR. Return a list of items to user..
  - Both prediction/ranking score are CTR
- Recommendations: given (user, item), predict CTR(action rate)/value score
  - Aims to maximize the CTR/value of the recommended item. Usually return one or few items to user.
  - Post/News feed: 
    - Model target: action rate
      - Usually build different models for different actions
      - positive: like, share, comment, play (video), subscribe, time spend to view. Ref:Ref: [Educative-GMLI-Feed-Metrics](https://www.educative.io/courses/grokking-the-machine-learning-interview/B8WQBKzXkXo)
      - negative: hide, ignore, never-see, report, dislike/angry (?)
    - Ranking score: value score, a weighted action rate of different actions
      - Usually include both positive actions and negative actions.
      - Weight is determined by your business objective 
        - Manually
        - learned from model: (input-feature, output-goal_metric), then output metric value can be used as ranking score.
      - Ref: [Educative-GMLI-Feed-TweetSelection](https://www.educative.io/courses/grokking-the-machine-learning-interview/gxM4VzxZ1Aj)
  - Ads:
    - Model target: action(engagement) rate
      - The rate of desired action
        - positive: click, convert, add-to-cart, etc.
        - negative: ignore, hide, never-see, report
    - Ranking score: value score, usually containing:
      - action rate: usually CTR
      - bid: advertiser's price for one desired action
      - ads quality score: from other user's feedback
    - Ref: [Educative-GMLI-Ads-Metrics](https://www.educative.io/courses/grokking-the-machine-learning-interview/N0P4R1v8mXL)

## 1. Clarifying Questions

- Ask what is the business objective or long-term goal?
  - Used to define long-term/online metric.
  - E.g. User engagement? User population? Revenue? DAU/MAU?
- Ask what is the ideal model target? 
  - Used to define output of model
  - E.g. action rate? value score?
  - If action rate, what action? Click? conversion? add-to-cart?
- ~~Ask what is the form of interface to user / number of items shown to user.~~
  - ~~Used to determine offline metric~~
    - ~~If only one or few: classification metrics~~
    - ~~If a list of items: ranking metrics~~
  - Use both is safer. I.e. use classification metric to eval scoring model, use ranking metric to evaluate its ability to ranking.


Ref: 

- [Educative-GMLI-RecSys-Metrics](https://www.educative.io/courses/grokking-the-machine-learning-interview/q2NWLJNZ8Pk): Ranking metrics

## 2. System construction

### 2.0. Data generation:


#### 2.0.1. Form of data

Usually a triplet: (user/query, item, action-or-not), from the user behavior. 

Note: 

- If you have multiple actions and predict different actions respectively, you don't need to care about other action when prepare data for a specific action.
  - E.g. you are predicting "like", for a post have "share" but no "like", the label is still 0 for data prepared for "like" dataset. This is intuitive.

Ref: [Educative-GMLI-RecSys-Training_data_generation](https://www.educative.io/courses/grokking-the-machine-learning-interview/3Yw0XjjPXqx)


#### 2.0.2. Data preprocess

- Imbalance
- Missing value
- Duplicates
- Outliers
- Reweigh samples: you intentionally reweigh samples according to your requirement or business goal, e.g.
  - If want to focus on new items, can increase its weight (more new samples) in the dataset.
    - The info for new item is usually sparse. (I.e. cold start)
  - If you want to focus on/optimize nighttime/longer movie rec accuracy, you can add more samples at-nighttime/with-longer-duration.

Ref: 
- [Educative-GMLI-RecSys-Training_data_generation](https://www.educative.io/courses/grokking-the-machine-learning-interview/3Yw0XjjPXqx): time axis split, reweight samples.
- [Educative-GMLI-Feed-Training_data_generation](https://www.educative.io/courses/grokking-the-machine-learning-interview/39X7DmnxnEQ)

#### 2.0.3. Train test split

Cannot use random splitting, should split data on time axis, use the former part as train and succeeding part as val/test.

```
|----------------------||-------------|
        June-train         July-test
```

This is because, when in the application scenario, we are forecasting future behavior. And what we want to evaluate is the performance of model on future user behavior.

The performance on feature data should be lower than performance on current data. Because there should be effects like:

- Dataset drifting (user behavior changes with time)
  - So the performance in real application should be lower than randomly splitting, splitting on time axis should give fair estimation.
- Leakage (a user watched the 5th episodes usually watched the 2nd episodes)
  - So random splitting may falsely increase the accuracy of the model.


Ref: [Educative-GMLI-RecSys-Training_data_generation](https://www.educative.io/courses/grokking-the-machine-learning-interview/3Yw0XjjPXqx)


### 2.1. Model output

Most case it is action rate, can also be value score, see sec 0.

### 2.2. Features

Feature = "factors that may influence (help to predict) the action rate", usually have four aspects:

- User
  - Profile: age, gender, location, language
  - Activity: time spend on platform, active days per month
  - Preference: 
    - historical/recent actions (user-item matrix), 
    - stats from historical/recent actions (cumulative counts on different category, price, topic).
- Item
  - General:
    - Stats: population action rate (num actions), avg rating score
    - Release time: new-or-old, time since launch
    - Content-related: text, category/hashtag(topics), actors(movie), genre(movie), duration(movie)
    - Manufacture/Author(feed)/Advertiser(ads): 
      - Stats: popularity, avg_rating 
      - Content: brand name, industrial area/domain, topic
  - Topic specific
    - News/posts
      - Network features: actions of friends/following
    - Ads:
      - Advertiser features: see above
- User-item cross
  - Similarity: embedding similarities, jaccard similarities.
  - User-Advertiser(ads)/Author(feed) cross: 
    - Stats: num interactions before
    - SocialNetwork: num common friends, subscribed/following-or-not
    - Content: domain-match-or-not
- Context
  - (Current) Time: time of the day, day of the week, holiday, etc.
  - Current region
  - Device/platform: device type, screen size, network condition, etc.


Ref: 

- [Educative-GMLI-Ads-FeatureEngineering](https://www.educative.io/courses/grokking-the-machine-learning-interview/my8xD3DY703)
- [Educative-GMLI-Feed-FeatureEngineering](https://www.educative.io/courses/grokking-the-machine-learning-interview/mykLYEJ9vVG)
- [Educative-GMLI-RecSys-FeatureEngineering](https://www.educative.io/courses/grokking-the-machine-learning-interview/m7yvJPANx19)

### 2.3. Model selection

Usually it is model-based model, multi-stage

- CF-based: only rely on user-item matrix.
  - Similarity based
    - User based: user-similarity weighted, score of same item from different user.
    - Item based: item-similarity weighted, score of different items from same user.
  - Model based
    - Matrix factorization: usually NMF, because rating/prob is positive
    - KNN: average of most close user/items.
    - K-means: average of the cluster (group)
- Content-based: rely on features
  - Similarity based
    - Still rely on similarity, but similarity is calculated based on features rather than user-item matrix.
  - Model based
    - Classical linear classifiers
    - DNN
      - single task vs multi task
        - single-task: different model for different actions/features
          - Stacking-model: use the output of different sub-model as the input feature of a summarized action prediction model.
        - multi-task: like multi-class classification:
          - the network share same features but have multiple output channel:
            - $p_{a_1}, p_{a_2}, p_{a_3}, \dots$ 
          - The loss is the sum of CE of different output channel:
            - $L_{\text{total}} = \mathrm{CE}_{a_1}+\mathrm{CE}_{a_2} + \mathrm{CE}_{a_3}+\dots$ 
        - Ref: [Educative-GMLI-Feed-Ranking](https://www.educative.io/courses/grokking-the-machine-learning-interview/R1VOnXE904L)
- Hybrid:
  - Implement CF and Content-based respectively and combine their score
  - Incorporate Content-based info (like similarity) in to the CF-based model.
  - Introduce CF-based techniques to Content-based model. E.g.:
    - - Directly introduce the rating/prob from CF as a feature
    - Use CF-related tech, like MF, to reduce the dimension user's rating history and other sparse features.
    - Use CF-related concept to engineering the features, like use Tfidf score to highlight unique items.
- Ref: [recommender_basics_types.md](./recommender_system/recommender_basics_types.md)

CF vs Content based comparison:

- Collaborative filter: 
  - Pros: based on user behavior, require less domain knowledge (feature exploration/processing, simpler)
  - Cons: suffers from cold start problem, not info for new item/user
- Content-based: 
  - Pros: less suffers from cold start problem, as any way we can gather some info from user/item
  - Cons: may require complex feature exploration/engineering.


Ref: 
- [Educative-GMLI-RecSys-Candidnate_generation](https://www.educative.io/courses/grokking-the-machine-learning-interview/YQZR9pOMXJ9#Candidate-generation-techniques): different types, CF vs content-based

### 2.4. Training Loss

- Binary classification:
  - Cross Entropy
- Regression:
  - MSE


## 3. Evaluation metrics

Ref:

- [Educative-GMLI-Feed-Online_experiment](https://www.educative.io/courses/grokking-the-machine-learning-interview/7n71LgM1vYQ): AUC, online metrics
- [Educative-GMLI-RecSys-Metrics](https://www.educative.io/courses/grokking-the-machine-learning-interview/q2NWLJNZ8Pk): Ranking metric, online metric. (Movie RecSys)
- [Educative-GMLI-Ads-Metrics](https://www.educative.io/courses/grokking-the-machine-learning-interview/N0P4R1v8mXL): CE, CE vs AUC, online metrics:revenue, CTR/Conversion, negative feedback rate 


### 3.1. Offline

Note: you log history is usually a ranked list, like {user1@t1:[(item1,action1),(item2,action2),....],user1@t2..., user2@t1:....}

For offline evaluation, 

- Classification metric: 
  - Not often in report, may be used for debugging?
  - evaluate the accuracy of the score, 
  - evaluate the model in individual sample level, i.e. individual unit = (user,item,rating), with binary classification metrics.
- Ranking metric: 
  - Usually for reporting.
  - evaluate the performance of model on ranking task
  - evaluate your model in whole list level,i.e. individual unit = "user:[(item1,action1),(item2,action2),....]", with ranking metrics.



#### 3.1.1. Classification metrics: 

Used for

- ~~The scenario that **user can only see one or very few recommended item** ,so we just care about the click-or-not for the item(s), rather than the ranks of different items.~~
- Or you just want to **just** evaluate the accuracy of the **score** rather than the accuracy of whole ranking, i.e. break your ranking ranking list into samples, and evaluate the model in (user,item,rating) sample level.

Metircs:

- **Cross entropy: (best one)**
  - (Pro) threshold-invariant, not affected by choice of the threshold
  - (Pro) NOT scale-invariant, can help to **calibrate the score**.
  - (Con) Affected by data imbalance
- AUC: 
  - (Pro) threshold-invariant: not affected by the choice of the threshold.
  - **(Con)** scale-invariant: **just care about the relative ranking of positive vs. negative samples, don't care about the absolute value of the output probability, not good for model calibration.**
  - (Pro) Unaffected by data imbalance or not.
- Precision-Recall: 
  - (Pro) Intuitive
  - (Con) not threshold-invariant, affected by class imbalance.

#### 3.1.2. Ranking metrics: usually for search engine, i.e.

Used for 

- ~~The scenario that **user can see a list of (many) retrieved item**, ranking order of these items is important.~~
- I.e. you want to evaluate the ranking performance, i.e. the individual evaluation unit is a whole ranking list, like "user:[(item1,action1),(item2,action2),....]"

Metrics:

- Precision@k, recall@k
- MAP
- MRR

Ref: 

- [evaluation_metrics.md](../general_machine_learning/model_training/evaluation_metrics.md)

Note: ranking metric is typically offline metric, not online. Online metric is usually the metric in AB test. Ref: [Educative-GMLI-RecSys-Metrics](https://www.educative.io/courses/grokking-the-machine-learning-interview/q2NWLJNZ8Pk), [Wiki-IR_eval_metric](https://en.wikipedia.org/wiki/Evaluation_measures_(information_retrieval))


### 3.2. Online

Metrics determined by your goal. Usually used in **AB test**, determining whether to launch the system or not.

- In terms of business goal:
  - User engagement 
    - User level: num actions per user, 
    - Overall quantity: **CTR**, add-to-cart rate, conversion rate
    - Overall quality: session time, bounce rate, hide/never-see/report rate
  - Number of user (growth rate, churn rate, etc.)
  - User activity(DAU, MAU, active time per day, etc.)
  - Revenue (revenue per user, revenue grow rate, profit, etc.)

Also, don't forget to evaluate the **quality** of actions.

- Quantity vs Quality
  - Quantity: CTR
  - Quality: session time, bounce rate


### 4. Score post-modification

#### 4.1. (Probability) score calibration

Problem: Raise from the data preprocessing stage:

- When solve imbalance problem, upsampling/downsampling increase probability (density) of minority class in the feature space.
- Then the model will overestimate the probability score of the minority class. (The probability of minority should be smaller)
  - Note: the model is no problem, because when it sees the data, the probability of minority is already overestimated, the problem is on the data side.

Solution: Calibrate the probability score with the equation:
$$p^* =\frac{p_1}{k(1-p_1)+p_1} $$

Ref: [imbalanced_data.md](../general_machine_learning/data_engineering/imbalanced_data.md)


#### 4.2 Probability score to raking score

Application scenario: In real application，besides action rate, we usually need to consider other factors, E.g.

- Ads-auction: determine the final order of ads shown to user, based on:
  - Bid: the price advertiser pay for each desired action on ads
  - User engagement rate: the predicted user engagement rate (or 'engagement score' weighted action rate for different actions)
  - Ad quality score: the rating/action/feedback from people who seen the ads.
  - Budget: the user's budge for an ads
    - **Personal understanding:** budget is used for "pacing", if budget is too limited, we reduce the bid so that we can evenly spend budget over time.
  - Ref: [Educative-GMLI-Ads-Architecture](https://www.educative.io/courses/grokking-the-machine-learning-interview/7Dzp9RmVv91)



#### 4.3. Diversity: ranking score modification

Problem: Raise from the consideration on user experience:

- The model will give very similar items very similar score. 
  - Note: the model is no problem, because similar item should have similar score. Or the model is overfitting, i.e. fitting noise, having high variance error if give different score for similar items.
- However, from the application perspective, it is not good to show similar items consecutively/together, which may provide redundant info to user and thus affect user experience.

Solution: 

- penalize (reduce) on score if similar items have close rank. So similar items will have different rank. E.g. final_score = raw_score-0.1
- Or directly shift-down the rank of similar items. E.g. final_rank = raw_rank+5
- MyThought: maybe can add different penalization value according to similarity score.

Ref: 
- [recommender_basics_typical_problems.md](./recommender_system/recommender_basics_typical_problems.md)
- [Educative-GMLI-Feed-Diversity](https://www.educative.io/courses/grokking-the-machine-learning-interview/B1X08zyx05N)




## 5. Other interview topics

### 5.1. Multi-stage Rec/Ranking

- Condition/Rule/"Prior engagement score"-based filtering: filter candidates based on some condition, 
  - Note: Prior engagement score is the action rate/score from the population (people seen it, like ads quality score?)
    - I.e. filter out the items with bad quality directly.
  - E.g. like query or user-background have "data science", then filter out the item have no "data science"
  - Aim to select small amount of sample to large amount of samples
  - Ruble-based is very fast.
  - Actually is also a process focused on recall.

- Candidate generation: Focus on high recall, what to retrieve the relevant items and filter out irrelevant items.
  - Aim to select small amount of sample to large amount of samples
  - Usually use few, high correlation/prediction-power features, so that the score can be fast generated, though may be not so accurate.

- Ranker: Focus on high precision, intend to rank the relevant ones are precise as possible.
  - Aim to precisely rank a small number of candidates
  - Usually use a lot of features and complex models, so that we can make decision with info as comprehensive as possible.

Ref:

- [Educative-GMLI-RecSys-Architecture](https://www.educative.io/courses/grokking-the-machine-learning-interview/qZvA2xgAAp3): Candidate generation + Ranker
- [Educative-GMLI-RecSys-Ranker](https://www.educative.io/courses/grokking-the-machine-learning-interview/R8DzGkW9ZjR): Ranker focus on precision
- [Educative-GMLI-Ads-Architecture](https://www.educative.io/courses/grokking-the-machine-learning-interview/7Dzp9RmVv91): Ad selection + Ad prediction, similar to generation + ranker.
- [Educative-GMLI-Ads-Selection](https://www.educative.io/courses/grokking-the-machine-learning-interview/xo0ZRvABl3q): condition-based filtering, prior engagement score, etc.
- [multi-stage_ranking.md](./search_engine/multi-stage_ranking.md): retrieval model (recall) + relevance model (relevant) + click model (ranking precision).

Ref:[]

### 5.2. Typical problems

- Cold start: user-item are empty for new user/item
  - Solution:
    - Use content-based: content-based recsys is less rely on user action history.
    - Multi-arm bandit: optimize accuracy (CTR) while collecting the data, reduce the loss during the data collection process.
    - Representative-based: find the subpopulation of the user and use the subpopulation average.
    - Deep Learning techniques: e.g. use dropout to reduce the reliance on particular feature.
- Position bias: low-rank item have no chance to be exposed.
  - Solution: 
    - Randomization
    - Multi-arm bandit: MAB will expose low rank samples for "exploration" purpose.
- Scalability: require huge computation resource to train and predict.
  - Solution:
    - Online learning: decouple the complex feature generator part and simple final decision model part; only update final decision model. This can reduce the computation resources required.
    - Distributed/Parallel computing: accelerate compute
    - Clustering: use userCluster-itemCluster matrix rather than user-item matrix, can significantly reduce the dimension on user and item axis.
- Sparsity: user-item matrix are sparse
  - Data imputation, i.e. filling missing value
  - Dimension reduction: PCA, MF
    - missing value handling: 
      - MF: Don't fill missing value while using MF; 
      - SVD/PCA: there are specialized algos for SVD/PCA to process missing values.
- User fatigue: we don't want to show same negative feedback (or no-action) content again and again.
  - Introduce previous action/rating on this item.
- Other problem:
  - Selection bias: user only select the item they prefer to rating (TBD)
    - Solution: Fill missing value, 
    - Is similar to sparsity issue? 
  - Exposure bias: because of policy/mis-design, part of data are hide from user. (TBD)
    - Solution: manual labelling, randomization;
    - Seems position bias is a subset of exposure bias?
  - Conformity bias: the user's selection is affect by his local environment
    - Solution: disentangling, 
      - I.e. train a model to separate population embedding and his own embedding, so we can know his true preference, or predict better when he is in different environment. 
      - I.e. output is click-or-not, input is (user feature + local environment features)

Ref: 

[recommender_basics_typical_problems.md](./recommender_system/recommender_basics_typical_problems.md)

### 5.3. Online learning

Online learning means you update your model when launch it online, i.e. train the model in real time.

However, if the model is larger/complex, it is hard to train it in real time and consume a lot of computation resources; if we reduce model complexity, the model will not be accurate. To solve this dilemma: 

The Typical approach is to decouple your system into two parts:

- Feature generator
  - Only trained one times, NOT update during online training
    - Transfer learning is fine, i.e. during "one-time" training, still use the (user,item,action) data, no need to think of other complex tasks.
  - Usually very large/complexity, responsible for provide non-linear transformation for raw features
  - Usually use DNN or Tree model to provide feature embedding
- Final decision model
  - Trained/updated real-time during online learning
  - Usually very simple, like logistic model.
  - Input is the [raw features] + [generated non-linear features]
  - Output is the action rate.

Ref:

- [Educative-GMLI-Ads-Prediction](https://www.educative.io/courses/grokking-the-machine-learning-interview/R12R8pJ3B1q): typical sysdesign of online learning.

#### Appendix: embedding with tree models

Similar to embedding with DNN, embedding is weight but also the output of embedding layer if the input is in one-hot encoding format.

We can use the each different leaf nodes in each different trees as one individual dimension, 1 if its activated and 0 if not.

E.g. let's say have 2 tree and 8 leave nodes in total, then we will get a 7-dim embedding

<div  align="center"><img src= https://dl.acm.org/cms/attachment/e8c79bfb-3a9f-4b69-81a3-0a19ec3dd503/www2018-75-fig1.jpg style = "zoom:60%"> </div>

Since for each tree, only one leaf node will be activiated/fired for one sample, so the embedding will have and only have one "1" between 0-4th position, and 5-7-th position, look like

```
[0,1,0,0,0 | 0,1,0]
[1,0,0,0,0 | 0,0,1]
[0,0,0,0,1 | 1,0,0]
...
```
(You can think each leaf node is like a neuron in DNN. Then, it is easy to understand this approach.)

Ref:

- [Educative-GMLI-Ads-Prediction](https://www.educative.io/courses/grokking-the-machine-learning-interview/R12R8pJ3B1q)
- [ACM-paper](https://dl.acm.org/doi/fullHtml/10.1145/3178876.3186066):origin of image.

### 5.4 Scaling

- We can split the data into different partitions, and use distributed computing
  - E.g. select top 10 from 1000 example
    - Select top 10 from each 100-sample partitions, then have 100 filtered item
    - Select top 10 from 100 filtered items.
    - TC: 1000->200

Ref:

- [Educative-GMLI-Ads-Selection](https://www.educative.io/courses/grokking-the-machine-learning-interview/xo0ZRvABl3q): scaling with distributed computing.


### 5.6. pointwise vs pairwise ranking

- Pointer wise
  - Model output $\hat{s} := f(q,d)$
  - Training sample: $(q_1,d_1,a_1) \dots (q_i,d_i,a_i) \dots$
  - Loss: CE
- Pair wise
  - Model output $\Pr(\hat{s}_i>\hat{s}_j) := g(q,d_i,d_j) := g_f( f(q,d_i), f(q,d_j) )$
  - Training sample: $(q_1,d_1,d_2, l_{a_1>a_2}) \dots (q,d_i,d_j,l_{a_i>a_j}) \dots$
    - $l_{a^*_i > a^*_j} = l_{s^*_i > s^*_j} = l_{\frac{1}{r^*_i} > \frac{1}{r^*_j}}$: label whether item i should higher rank than item j.
    - Action (1,0) is just the true score. It can be determine the true rank, i.e. the clicked item should have higher rank, un-clicked item should have lower one.
  - Pro: more samples
  - Item ranking: 
    - generate a matrix $M[i][j] = \Pr(\hat{s}_i>\hat{s}_j)$ with model 
    - $s_{r,i} := \sum_j M[i][j]$ is the ranking score, it means how many items are ranking below $d_i$
  - Loss: also CE

Ref: [ranking_models.md](./search_engine/ranking_models.md)

### 5.6.1. Application: eval model with manual relevance score 

MyExp-Interview-W: how to use manual labelled relevance score (query,item, relevance) to evaluate a model trained with user click data (query,item,action)?

**My ans**: Use pair wise ranking. 

- For the pair wise ranking, we can training and evaluate model just with the relative order between two item.
- I.e. Instead of using click-or-not data ($a_i$)to generate samples, with label $l_{a^*_i > a^*_j}$, we can use relevance score ($s^*_i$) to generate samples, with label $l_{s^*_i > s^*_j}$. (Here, $s^*_i$ is understood as relevant score rather than click prob.）


### 5.6.2 Ranking loss vs CE? (TBD)


Seems both CE and ranking loss (pairwise-loss or triplet loss) can also be used in ranking models (Learn To Rank, LTR task): 

Ref: 

- [Medium](https://hemanthkumargadi.medium.com/introduction-to-pairwise-loss-function-a47f2282f16): RankSVM, RankBoost, RankNet user pairwise loss
    <div  align="center"><img src=https://miro.medium.com/max/916/1*DORaAoXpUSlIiMU1vA2Sxw.jpeg style = "zoom:60%"> </div>

  - function $\phi$ can be hinge, exponential or logistic.
- [GithubBlog](https://gombru.github.io/2019/04/03/ranking_loss/): pairwise loss and triplet loss for image embedding.
  - Pairwise loss: $L(r_0,r_1,y) = y\left \| r_0 - r_1 \right \| + (1-y) max(0,m - \left \| r_0 - r_1 \right \|)$
  - Triplet loss: $L(r_a,r_p,r_n) = max(0,m + d(r_a,r_p) - d(r_a,r_n))$
- Pairwise loss takes $(r_0,r_1,y)$, from the format of data,pairwise ranking should use pair wise loss.

Other refs on loss used by LTR:

- [Github-blog-Intro_to_LTR](https://everdark.github.io/k9/notebooks/ml/learning_to_rank/learning_to_rank.html): RankNet use cross entropy.
- [Medium](https://medium.com/analytics-vidhya/triplet-loss-b9da35be21b8): Ranking loss is often used to train network that create embedding, output is the similarity score.
- [DL-loss.md](../deep_learning/training_techniques/loss.md)

##### Ranking loss vs CE: (TBD)

Seems ranking loss may have better performance especially in complex task like image embedding.

**Personal feeling:** 

- CE just care about whether the (query,item) is correct or not. 
- Besides whether (query,item) is correct or not, contrastive loss also try to make embedding of similar (query,item) more similar, so that we can capture the similarity info in the embedding, should be more accurate?

Ref:

- [TowardsDataScience](https://towardsdatascience.com/contrastive-loss-for-supervised-classification-224ae35692e7): " model with supervised contrastive loss indeed outperforms the MLP baseline with cross entropy loss"
  - CE : cross entropy measure the differences between the ground truth class distribution and the distribution of the predicted classes 
    - (Actually DL-Divergence is better, CE is not 0 when distribution are same but KL-Divergence is 0)
  - Contrastive loss: Intuitively, this loss function encourages the neural network to learn a embedding to place samples with the same labels close to each other, while distancing the samples with different labels in the embedding space.
- [Medium](https://medium.com/@maksym.bekuzarov/losses-explained-contrastive-loss-f8f57fe32246#:~:text=The%20difference%20is%20that%20Cross,positions%20relative%20to%20each%20other.): The difference is that Cross-entropy loss is a classification loss which operates on class probabilities produced by the network independently for each sample, and Contrastive loss is a metric learning loss, which operates on the data points produced by network and their positions relative to each other.
- [Reddit](https://www.reddit.com/r/MachineLearning/comments/dtczgt/d_to_use_triplet_loss_or_not_when_classes_labels/): CE is usually more precise?



##### Contrastive loss vs triplet loss (TBD)

**personal feeling:**

triplet loss seems better, because besides the relative relation between two items, we also consider the their relation with anchor, more info is included, so the performance may be better (embedding are better, anchor is like a reference)?

- [Quora](https://www.quora.com/What-are-the-advantages-of-using-a-triplet-loss-function-over-a-contrastive-loss-How-would-you-decide-which-to-use): pairwise (contrastive) vs triplet loss
- [Github-blog](https://gombru.github.io/2019/04/03/ranking_loss/): seems contrastive loss is just pairwise loss




## 6. Task-specific topics

### 6.1. Ads - Auction: from action rate to value score

See sec 4.2. Use bid, engagement (action) rate, ads quality score, budget to build total value score.

Ref:

- [Educative-GMLI-Ads-Architecture](https://www.educative.io/courses/grokking-the-machine-learning-interview/7Dzp9RmVv91)

### 6.2. Ads - pacing

**Pacing**: we expect to spend budget evenly over time. Let's see we want to decrease the budget spend speed.

- The advertiser are charged one bid value per action. The total charge is proportional to the total number of actions: 
  - $\text{charge} = \text{bid} \cdot N_{\text{actions}} $
  - Reduce bid can reduce charge, but from the perspective of ranking/ML, we prefer to focus on reduce $N_{\text{actions}} \downarrow$
- Total number of actions is proportional to number of times we show ads to user:
  - $N_{\text{actions}} = N_{\text{show}}\cdot r_{\text{action}}$
  - Action rate is determined by model, objective value, not changed with our idea, so, we should reduce $N_{\text{show}} \downarrow$
- Chance user see the ads is proportion to the values score, the higher score, the higher rank the high chance the user see the ads.
  - $N_{\text{show}} \propto S_{\text{total\_value}}$
- Value score can be turned by turn down bid when budget is not enough, or directly linked value score with budget:
  - $S_{\text{total\_value}} = f(\text{bid},r_{\text{action}}, S_{\text{ads\_quality}})$, $\text{bid}=g(\text{budget})$, or
  - $S_{\text{total\_value}} = f(\text{bid},r_{\text{action}}, S_{\text{ads\_quality}},\text{budget})$


Ref:

- [Educative-GMLI-Ads-Architecture](https://www.educative.io/courses/grokking-the-machine-learning-interview/7Dzp9RmVv91)

## 7. Other basic topics

### 7.1. Explicit feedbacks vs implicit feedback

- Explicit feed back: A user provides an explicit assessment of a recommendation. 
  - E.g. rating of a movie
- Implicit feedback: user’s interaction with the recommended items.
  - E.g. watch-or-not for a movie.

Usually the recommendation system is based-on/predict the implicit feedback, because:

- Usually, the target action  we want to predict (business objective) is close to implicit feedback
  - E.g. we want to know whether a user will watch the recommended movie or not, rather than how much rating the user will give.
- Usually, the amount of implicit feedback data is much larger than explicit feedback.
  - E.g. User visit website > user who see the movie recommendation > user who click/watched the movie > user who provide rating score

Ref:

- [Educative-GMLI-RecSys-Problem_statement](https://www.educative.io/courses/grokking-the-machine-learning-interview/xlO33YAyVrz)


### 7.2. Sparse feature vs dense feature

- Dense feature: feature that is not sparse after converted to vector
- Sparse feature: features that is sparse after convert to vector。
  - E.g. text, high cardinality features like 
    - PCA
    - Embedding
    - Mean encoding
    - Hash encoder
    - Summarization: convert to dense or binary features, e.g.:
      - extract topic/category/keywords from text 
      - sentiment analysis on comments

Ref:

- [feature_engineering.md](../general_machine_learning/data_engineering/feature_engineering.md)
- [Educative-GMLI-RecSys-FeatureEngineering](https://www.educative.io/courses/grokking-the-machine-learning-interview/m7yvJPANx19)













  







<!-- ## SEO SysDesin

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
  - Rating score: MSE (mean square error) -->