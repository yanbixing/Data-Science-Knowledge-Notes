# ML ranking model (TBD)

## 0. Task description

Ranking model is usually used in search engine and recommender system. The model is like, given input (query in SE, user in RecSys), we can predict the similarity/ranking score for each item in our item pool. And then, rank the item according to the ranking score.

## 1. Data



- Training Sample form: (query:vector, item:vector, clicked_or_not:binary)
  - [TowardsDataScience](https://towardsdatascience.com/learning-to-rank-with-python-scikit-learn-327a5cfd81f)
- Model
  - Input: (query, item) pair
  - Output: click_probability, similarity (distance)


## 2. Approach and Model

- Ref:
  - [Blog](https://lucidworks.com/post/abcs-learning-to-rank/), 
  - [Github-blog](https://everdark.github.io/k9/notebooks/ml/learning_to_rank/learning_to_rank.html)
  - [Github-blog-Intro_to_LTR](https://everdark.github.io/k9/notebooks/ml/learning_to_rank/learning_to_rank.html): a detailed explanation on basics like pointwise vs pairwise vs listwise, Resnet vs LambdaNet, etc.






### 2.1. Pointwise approach

[Github-blog-Intro_to_LTR](https://everdark.github.io/k9/notebooks/ml/learning_to_rank/learning_to_rank.html): a detailed explanation on basics like pointwise vs pairwise vs listwise, Resnet vs LambdaNet, etc.

Idea: take one candidate each time, compute score between candidate and query

#### 2.1.1 Model structure

- Func to learn: score function $f(q,d)$
- training input: $(q_1,d_1) \dots (q_i,d_i) \dots$
- training output: click prob or similarity 
  - label: clicked-or-not (binary)
  - label: similarity (continuous)
- loss: cross entropy
  - If not DL, may not need "loss" concept
    - E.g. tree (minimize info gain)
    - Loss is only used to do GD?


loss to train: cross entropy

#### 2.1.2 Typical models

Any classification model.

#### 2.1.3 How to rank

Given the query, predict the click prob of each doc. 
Rank docs according to the predicted click prob.



### 2.2 Pairwise approach (TBD)

Take two candidate each time, compute score between the two candidates

- Ref:
  - [Github-blog-Intro_to_LTR](https://everdark.github.io/k9/notebooks/ml/learning_to_rank/learning_to_rank.html): a detailed explanation on basics like pointwise vs pairwise vs listwise, Resnet vs LambdaNet, etc.
  - [Github-blog](https://gombru.github.io/2019/04/03/ranking_loss/): good ranking loss explanation, also check OneNote.

<!-- #### 2.1.1 Model structure (TBD)

- input: (doc_i, doc_j)
- output: distance between (doc_i and doc_j)
- loss: pairwise loss


#### 2.2.2 Typical models

RankNet, LambdaRank, etc. -->

#### 2.2.1 Pairwise Ranking Framework (model structure)

Ref: [Github-blog-Intro_to_LTR](https://everdark.github.io/k9/notebooks/ml/learning_to_rank/learning_to_rank.html): a detailed explanation on basics like pointwise vs pairwise vs listwise, Resnet vs LambdaNet, etc.

- Func to learn: still scoring function $f(q,d)$
- training input feature:  $(q_1,d_{1,1},d_{1,2}) \dots (q_i,d_{i,1},d_{i,2}) \dots$
- training output target: (pairwise) binary label
  - 1 if d1 ranking should rank higher than d2 else 0. E.g.: 
    - $d_1$ gets clicked but while $d_2$ not
    - $d_1$ have higher relevance score than $d_2$ (may be from manual evaluation)
  - Note: The problem is still a binary classification problem.
- Model structure (personal feeling on RankNet): 
  - model $g(q,d_i,d_j)= \frac{1}{1 + exp^{-(f(q,d_i) - f(q,d_j))}}$
    - $s_i = f(q,d_i)$ is the score function
    - $Pr(i \succ j) = g(q,d_i,d_j)$ is the probability "$d_i$ have higher ranking than $d_j$" (i.e. the probability of "pairwise label = 1")
- Loss 
  - Cross entropy
    - $H(y^*,p) := - \frac{1}{M}\sum^M_{i=1} \{ y^*_{ij} \log p_{ij} + (1-y^*_{ij}) \log (1-p_{ij}) \}$
      - pairwise label y^*_{ij} is the "true distribution" 
        - also denoted as "$P^*$" (or "$y_{true}$"/"$y^*$" or "$P$"
      - $p_{ij} : = Pr(i \succ j) = g(q,d_i,d_j)$ is the "estimated distribution"
        - also denoted as "$\hat{P}$" or "$p$" or "$Q$"
    - CE is used by RankNet, LambdaNet
  - If not DL, may not need "loss" concept like tree (minimize info gain)

#### 2.2.2. Understanding on the model/framework

Ref: [Github-blog-Intro_to_LTR](https://everdark.github.io/k9/notebooks/ml/learning_to_rank/learning_to_rank.html): a detailed explanation on basics like pointwise vs pairwise vs listwise, Resnet vs LambdaNet, etc.

- We still rely on $s_i = f(q,d_i)$ as "basic unit":
    - I.e. the model to be trained is $f$, but
    - In pointwise ranking, we directly train the model click-or-not label,
      - And think the $s_i$ as click probability
    - In pairwise ranking, we use order between two pair to train
      - And think the $s_i$ as ranking score
  - When we are training, what we do is:
    - First, fetch two samples, input them to model and get the score from the model, i.e.$s_i = f(q,d_i)$, $s_j = f(q,d_j)$
    - Second, from the score, we can calculate the predicted probability that i have higher ranking than j: $Pr(i \succ j) \equiv \frac{1}{1 + exp^{-(s_i - s_j)}}$
  - We can regard the whole above process as one model:
  $$g(q,d_i,d_j)= \frac{1}{1 + exp^{-(f(q,d_i) - f(q,d_j))}}$$
  - Then the task/model can be viewed as a bianary classification task/model:
    - $Pr(i \succ j) = g(q,d_i,d_j)$ is just the predicted-score/output of the model $g(q,d_i,d_j)$
    - The parameter of $g$ is just the parameter of $f$, no difference.
  - Thus, given the ground true (pairwise) binary label, we can calculate the cross-entropy and train the model as a typical binary classification model (i.e. minimize CE with GD).


#### 2.2.3 how to rank docs (how to use the output pairwise label)

Ref: [Blog-pairwise_ranking](https://www.questionpro.com/tour/pairwise-ranking-and-comparison.html)

- Give a query $q$ and a list of docs $[\dots d_i \dots]$
  - Generate a 2D matrix M M[i,j]= the_higher_priority_doc(di,dj) = di or dj
  - calculate the frequency of different d_i in M: \{d1 = 1, d2=0, d3 =2 \}
  - we can rank those document by their frequency, here [d3, d1, d]
    - reasonable, since in M, for each unique doc pair, we return the doc with higher priority
    - so, the frequency of d_k actually means how many docs are behind the, like here, like d_3=2 means there are 2 docs behind the d_3, so it is reasonable to rank doc with their frequency/counts in M.
- English:
  - for all unit pair $(doc_i,doc_j)$, output the higher ranking doc
  - count the appearance of each $doc_i$, the count of appearance means how many docs are behind the $doc_i$
  - we can rank docs by its appearance count. 
    - E.g. $N$ doc in total, when comparing with all other docs, the first doc must be higher than all other $N-1$ docs, so its count of appearance must be $N-1$

#### 2.2.4. Typical models

All binary classification model works, both ML and DL

Most frequent models: RankNet, LambdaNet


### 2.3 Listwise approach (TBD)

Take the entire list, optimize its order.


## Deep Dive: Classical Model or Deep Models?

<!-- - Model for strategy?
  - Since pointwise ranking is simple binary classification, so we can use both classical ML and DL.
  - But pairwise ranking, we need to have a ?regression-like? model to calculate score, and use CE as the loss, and use BP to train 
    - => so we have to use DL, cannot use classical ML, or we cannot train?
  - Is this correct?
- Nope! -->

For both pointwise and pairwise ranking, we can use both classical ML or DL, since they are **just binary classification task**.
- Binary classification: split y=0 vs y=1, on $X$ feature space
  - Here, the input feature is just the composite/concatenated feature of two or three items, uncomfortable but no real difference.
  - i.e. We are still splitting y = 0/1 on feature space $X=[ [v_q]  [v_d] ]$ or $X=[ [v_q]  [v_{d_{i}}] [v_{d_{j}}]]$ 
- It seems a potential problem that some classical model cannot generate score function $s_i$
  - But **actually we don't need $s_i$ at all**:
    - For pointwise, we just a need final binary prediction: click-or-not
    - For pairwise, we just a need final binary prediction: i-rank-higher-than-j-or-not.


## Draft: github summary




### 0. MyQs TBD ??????









### Remaining Qs:

  - How to use pairwise loss? is that a "lambdaloss"? What is the relation between pairwise ranking and pairwise loss, seems pair wise ranking need triplet loss rather than pairwise loss?
    - (TBD)
    - Ref: [Github-blog](https://gombru.github.io/2019/04/03/ranking_loss/): good ranking loss explanation, also check OneNote.
- Method according to ran


