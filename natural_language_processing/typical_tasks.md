# Typical Tasks

## 1. Text classification

### 1.1. Sentiment analysis

- Given doc, predict the label of the whole doc.
  - A multi-classification task for whole text.

- Data: (text, label)
- Model: RNN, LSTM, BERT, etc.
  - Inpu: text (embedding), 
  - Output: Prob for each unique label
  - Loss cross entropy, 
- Evaluation metric: prec/recall, etc


## 2. Classification at each position
### 2.1 Part-of-Speech (POS) Tagging

- Given word sequence, predict the label on each word position.
  - A multi-classification task for each position.

- DL Model: RNN, LSTM, BERT, etc
  - Input: word, pos_tag
  - Output: prob for each unique entity label
  - Loss: cross_entropy
- Classical Model: HMM
  - Param: transition matrix and outcome(emission) matrix, denote the probability between hidden-hidden and hidden-observable state.
  - Training: BW algorithm (a kind of EM algorithm)
    - E: update distribution of states according to param (get form of expectation of likelihood over different possible states, w.r.t to parameters)
    - M: update param by maximize the (expectation of) likelihood
  - Loss: likelihood (MLE)
- Eval: accuracy, prec, recall

### 2.2. Named Entity Recognition (NER)

- Given word sequence, predict the entity label on each word (each RNN position).
  - A multi-classification task for each position.


- Data: [(word, pos_tag, entity_label)...]sequence
- Model: RNN, LSTM, BERT, etc
  - Input: word, pos_tag
  - Output: prob for each unique entity label
  - Loss: cross_entropy
- Eval: accuracy, prec, recall


- Ref: 
  - [TowardsDataScience](https://towardsdatascience.com/named-entity-recognition-ner-using-keras-bidirectional-lstm-28cd3f301f54): DL on NER
  - [Github](https://github.com/soutsios/pos-tagger-bert): BERT on NER


## 3. Language Modeling (LM)

- Given prev words, prediction next words
- Model: RNN, LSTM, BERT, etc
  - Input: $[w_1, w_2, \dots w_{t-1}]$
  - Output: $w_t$ at each position
    - How to proceed? input onehot prob or select word with max prob? Should be use ground true. (TBD)
  - Loss: cross entropy
- Eval: perplexity, cross entropy and bits-per-character
- Ref:
  - [Blog](https://thegradient.pub/understanding-evaluation-metrics-for-language-models/): eval metrics

### 4.1. Natural language generation (TBD)

An application of LM.

Can generate next word with prev word, iteratively.

### 4.2. Machine translation (TBD)

- input all text, generate new sequence according to hidden state


## 4. Topic Modeling

- Given text, find potential topics
- LSI: i.e. SVD
  - doc-term matrix decompose to 
    - $U$: doc-topic
    - $V$: topic-term matrix
      - We can use term to represent topic
    - $\Sigma$ is the weight/importance of topic in the corpus (TBD)


## Short/Long Text Similarity