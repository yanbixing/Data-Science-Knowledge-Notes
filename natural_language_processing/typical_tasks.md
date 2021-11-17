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


## 4. Topic Modeling (Unsupervised)

- Given text, find potential topics
- LSI: i.e. SVD
  - doc-term matrix decompose to 
    - $U$: doc-topic matrix
      - $U_{iz} = p(d_i|\tau_z)$: the probability of document $i$ given topic $\tau_z$
    - $V$: topic-term matrix
      - $V_{jz} = p(w_j|\tau_z)$: the probability of term $j$ given topic $\tau_z$
      - We can use term to represent topic
    - $\Sigma$ is the prior-prob of topic in the corpus
      - $\sigma_z = p(\tau_z)$: the prior probability of the k topics in the corpus.
    - Ref: [LSA.md](./latent_dirichlet_allocation.md)
- LDA: (generative model)
  - Hyper param: $\alpha$, $\beta$, $K$
  - Param to be learned: $\theta$,$\phi$
  - Mechanism:
    - Given $\alpha$, $\beta$, we can generate 
      - topic distribution param for each doc: $\boldsymbol{\theta}_{i}\sim \operatorname {Dir} (\boldsymbol{\alpha} )$
        - Then, we can generate topic from topic distribution for doc: $\tau_{i,j} \sim F(\boldsymbol{\theta}_i)$ 

      - word distribution param for each topic: $\boldsymbol{\varphi}_{k}\sim \operatorname {Dir} (\boldsymbol{\beta} )$ for each topic $k$
        - Then, we can generate word from word distribution for topic: $w_{i,j} \sim G(\boldsymbol{\varphi}_{\tau_{i,j}})$ 
  - Training: Gibbs sampling
    - Initialize: Randomly assign topic to all words, get initial topic distribution for doc $F(\boldsymbol{\theta}_i)$ and word distribution for topic $G(\boldsymbol{\varphi}_{\tau_{i,j}})$ 
    - Iteration over all doc i on all position j:
      - Remove the word $w_{ij}$ from the doc (and corpus) (also its corresponding topic $\tau_k$ is removed.).
      - Recalculate the topic distribution for doc as $F'(\boldsymbol{\theta}_i)$ and word distribution for topic as $G'(\boldsymbol{\varphi}_{\tau_{i,j}})$ 
      - Add the word $w_{ij}$ back and label it with new topic $\tau'_k$ according to $F'(\boldsymbol{\theta}_i)$, $G'(\boldsymbol{\varphi}_{\tau_{i,j}})$
- Evaluation metric: 
  - Classical: perplexity
    - Measure the uncertainty of prediction on each position
    - "Likelihood" is the likelihood of each doc
      - For LSA, use its probability interpretation
  - New: coherence
    - Measure the dis-similarity (distance) between different topics, the larger the better
    - Use the word representation for each topic.
- Note: Supervised topic modeling is like a multi-classification task, like multi-label version sentiment analysis.
      

## Short/Long Text Similarity