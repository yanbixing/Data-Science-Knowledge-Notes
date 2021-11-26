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


### 2.1. Pointwise approach

Idea: take one candidate each time, compute score between candidate and query

#### 2.1.1 Model structure

- input: (query, item)
- output: click prob
  - label: clicked or not
- loss: cross entropy

#### 2.1.2 Typical models

Any classification model.

#### 2.1.3 How to rank

Given the query, predict the click prob of each doc. 
Rank docs according to the predicted click prob.



### 2.2 Pairwise approach (TBD)

Take two candidate each time, compute score between the two candidates

- Ref:
  - [Github-blog](https://gombru.github.io/2019/04/03/ranking_loss/): good ranking loss explanation, also check OneNote.

#### 2.1.1 Model structure (TBD)

- input: (doc_i, doc_j)
- output: distance between (doc_i and doc_j)
- loss: pairwise loss


#### 2.2.2 Typical models

RankNet, LambdaRank, etc.


#### 2.2.3 how to rank docs

- Give a query,

  - Ref: [Blog-pairwise_ranking](https://www.questionpro.com/tour/pairwise-ranking-and-comparison.html)
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



### 2.3 Listwise approach (TBD)

Take the entire list, optimize its order.