

## Multi-stage Ranking

Systems are often broken down in multiple stages for different objectives. eEach stage also have different performance and latency constraints.

Typically a search ranking system will have 3 stages:

- Retrieval (with a focus on recall)
- Relevance ranking (with a focus on optimizing for judged relevance) 
- Click and diversity ranking (with a focus on optimizing for direct user feedback). 

Ref: [Medium-blog](https://medium.com/better-ml/multi-stage-ranking-e0dacd81ac4)


<div  align="center"><img src= https://miro.medium.com/max/1320/1*UjeqczvHxNhxxxdD2jBi9g.png style = "zoom:60%"> </div>

## 1. Basic workflow

### 1.1. Retrieval model

- Task: Rank a list of candidates based on predicted **match score** $s_{1,i} = f_1(q, d_i)$ 
  - Purpose: Select the items **could possibility match** the query.
    - I.e. filter out the items irrelevant to the query.
- Candidate amount: over thousands
- Requirement: **Low serving latency**, **low memory consumption**.
- Typical model: Gradient boosted decision trees with:
  - **low number** of **high quality** features for **fast serving**. 
- Training data: usually human judged relevance data.
  - PersonalUnderstanding: the "relevance" data here is more like binary label, like "match-or-not".

### 1.2. Relevance ranking model

- Task: Rank a list of candidates based on predicted **relevance score** $s_{2,i} = f_2(q, d_i)$ 
  - Puprose: Select the items **highly relevant** to the query.
- Candidate amount: hundreds to thousands 
  - Candidates are from retrieve model
- Typical model: Usually an ensemble of different sub models focusing on different aspect.
  - Sub models:
    - Input: Different kinds of features
    - Output: Different scores focusing on different aspects. E.g.:
      - Doc quality, query quality, doc-query similarity,
    - Typical model: usually binary classifier (high-quality-or-not)
  - Final model:
    - Input: output score of sub models and any other features
      - "Features" e.g.: topic, subcategory, embedding, doesn't matter if it is used by sub models or not.
    - Output: relevance score
    - Typical model: usually shallow NN, just ensemble all info is fine.
- Training data: 
  - Sub models:usually human judged binary labels (e.g. high-quality-or-not)
  - Final models: usually human judged relevance  data.
    - PersonalUnderstanding: the "relevance" here is more like similarity score? Numerical data or in a comparison form?

<div  align="center"><img src= https://miro.medium.com/max/1400/1*WEeOQet8QnkcuQaAvLxoow.png style = "zoom:60%"> </div>

### 1.3. Click ranking model

- Task: Rank a list of candidates based on predicted **click probability** $s_{3,i} = f_3(q, d_i)$ 
  - Purpose: **precisely** prioritize the items with higher **click prob** **to user**.
- Candidates amount: tens to hundreds
  - Candidates are from the relevance models
- Typical model: complex NN model for precise ranking order prediction
- Training data: log of user click.


## 2. Pro and Cons

Pros:


- Decouple tasks with different data size and accuracy requirement.
  - E.g. 
    - stage1 document retrieval requires fast processing on large amount of samples, while number of features is small.
    - stage2 document relevance requires processing more features comprehensively
    - stage3 click ranking requires precise ranking of small number of candidates.
- Decouple tasks with different focus/fields.
  - E.g. in stage2, sub models are focusing on different aspects of the quality, like doc quality, query quality, query-doc relation


Cons:

- Hard to maintain, 
  - one part fail make whole system fail ("cascade" of failures)
  - one update need to update different models
- Hard to debug, 
  - when fail happens, need to check a few different models.


## Interview Qs:
  
- [1p3a](https://www.1point3acres.com/bbs/thread-792298-1-1.html): interview-F, multi stage ranking.