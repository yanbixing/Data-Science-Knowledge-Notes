# Word2Vec

## 1. Basics

### 1.1. what is embedding?

A process to convert word to vector

Typically, map from one word one dimension (token, one-hot encoding) to less dimension, most common tech is word2vec.

Ref: [Blog](https://machinelearningmastery.com/what-are-word-embeddings/):?(TBD)

### 1.2. How word2vec embedding comes?

For example, in your word2vec model:

- your vocabulary size (input layer size, receiving the one-hot encoding) $V=$10k 
- your hidden layer size $n$ is 300, 

Then you will have a weight matrix $\boldsymbol{W}_{V\times n}$ ($\boldsymbol{h}_{1\times n}:=\boldsymbol{x}_{1\times V} \boldsymbol{W}_{V\times n}$, i.e. ,the ref use row vector) 

<div  align="center">
  <img src= http://mccormickml.com/assets/word2vec/word2vec_weight_matrix_lookup_table.png style = "zoom:40%">
</div>

Why? Because for each one-hot encoding of j-th term $\tau_j$ , there is only one "1" in the vector, i.e. $\boldsymbol{x}_{\tau_j} = [0,\dots,\underset{\text{j-th}}{1},0,\dots]$. So $[\boldsymbol{x}_{\tau_j}]_{1\times V} \boldsymbol{W}_{V\times n}$ will output the j-th row in $\boldsymbol{W}_{V\times n}$, a vector with dimension $n$.


Ref: [Blog](http://mccormickml.com/2016/04/19/word2vec-tutorial-the-skip-gram-model/), [Medium](https://becominghuman.ai/how-does-word2vecs-skip-gram-work-f92e0525def4), [Zhihu](https://zhuanlan.zhihu.com/p/27234078)

- Which matrix to use: there are two weight matrix, 
  - e.g. 
    - input: 10k dim
    - Weight: $W_{1, (300\times10k)}$, map 10k-dim input to 300-dim hidden
    - Weight: $W_{2, (10k\times300)}$, map 300-dim hidden back to 10k-dim output
    - output: 10k dim
  - ans: first one. 
  - why? Because it map 10k one-hot input to 300-dim hidden, to 300 row/col corresponds to each of 10 tokens. The hidden states is the embedding, so use the first matrix $W_{1, (300\times10k)}$,.


### 1.3. How does the word2vec training work? (Intuitive)

Skip-gram: given a word, predict its context ((maximize the probability of its context words).
CBOW: given a word's context, predict the word (maximize the probability of the word).

Ref:[StackOverflow](https://stackoverflow.com/questions/52866407/sequence-models-word2vec)

## 2. Model


Word2Vec has two architectures/ways-to-implement, continuous skip-gram (SG) and bag-of-word model (CBOW).


<div  align="center">
  <img src= http://idli.group/assets/2017-01-22-Natural%20Language%20Processing%20using%20Word2Vec/word2vec.png style = "zoom:60%">
</div>

Ref: 

- [Medium](https://medium.com/analytics-vidhya/word2vec-cbow-skip-gram-algorithmic-optimizations-921d6f62d739), [Wiki-Word2Vec](https://en.wikipedia.org/wiki/Word2vec#CBOW_and_skip_grams), [StackOverflow](https://stackoverflow.com/questions/38287772/cbow-v-s-skip-gram-why-invert-context-and-target-words),[TowardsDataScience](https://towardsdatascience.com/an-implementation-guide-to-word2vec-using-numpy-and-google-sheets-13445eebd281), [Blog](http://idli.group/Natural-Language-Processing-using-Vectoriziation.html): two ways to implement, and the difference.
- [TowardsDataScience](https://towardsdatascience.com/an-implementation-guide-to-word2vec-using-numpy-and-google-sheets-13445eebd281): an implementation guide of skip n-gram
- [GoogleSlides](https://docs.google.com/presentation/d/1yQWN1CDWLzxGeIAvnGgDsIJr5xmy4dB0VmHFKkLiibo/edit?usp=sharing): a summary slide


### 2.1. Skip-gram (SG)


Skip-gram means using a word to predict the background words.

In real application, there will be a window to define "background words". 

#### 2.1.1. Training sample preparation

E.g. In the following example, the yellow one is the input word and the green words will be the target we would like to predict.

<div  align="center">
  <img src=https://miro.medium.com/max/1400/1*jkxbwD55_8M3XBRb1bGm7A.png style = "zoom:60%">
</div>

In vector representation, each term will be represented with a one-hot encoding vector, with the size of the vocabulary size $V$, like the j-th word in i-th doc will be represented as: 

- $t_{i,j} = [0,\dots,0, p_{idx(t_{i,j})} = 1,0,\dots,0]^T_{(V \times 1)}$
  - "T" means this is a column vector
- Then the training samples will be like $(t_{i,j}, t_{i,j-2})$, $(t_{i,j}, t_{i,j-1})$, $(t_{i,j}, t_{i,j+1})$,$(t_{i,j}, t_{i,j+1})$, etc

#### 2.1.2. Model details:

For simplicity, 

- we denote the input with letter $x$, $x_{i,j} := t_{i,j} $; 
- on output side, we still use letter $t$ as the ground true label. 
- The model output probability distribution of different words is denoted with letter $\hat{y}$, $\hat{y}(x_{ij}) = [p_1,\dots,p_V]^T_{(V \times 1)}$

In word2vec model, the implemented is like this:

<div  align="center">
  <img src=http://mccormickml.com/assets/word2vec/skip_gram_net_arch.png style = "zoom:60%">
</div>

The input will be $x_{i,j}$, the output will be $\hat{y}(x_{ij})$, if the training sample is $(x_{i,j}, t_{i,j+1})$, the loss contribution of the sample is like (for simplicity, here use RMSE as example, in real application, usually use log probability see Sec 2.1.4. ): 
$$E = [t_{i,j+1}-\hat{y}(x_{i,j})]^2 = [t_{i,j+1}-\hat{y}(x_{i,j})]^T[t_{i,j+1}-\hat{y}(x_{i,j})]$$

Ref: [TowardsDataScience](https://towardsdatascience.com/an-implementation-guide-to-word2vec-using-numpy-and-google-sheets-13445eebd281),[Blog](http://mccormickml.com/2016/04/19/word2vec-tutorial-the-skip-gram-model/), [TowardsDataScience](https://towardsdatascience.com/nlp-101-word2vec-skip-gram-and-cbow-93512ee24314), [GoogleSlides](https://docs.google.com/presentation/d/1yQWN1CDWLzxGeIAvnGgDsIJr5xmy4dB0VmHFKkLiibo/edit?usp=sharing)


#### 2.1.3. Main parameters 

- window_size: the width of the window
- n: dimension of the embedding

Ref: [TowardsDataScience](https://towardsdatascience.com/an-implementation-guide-to-word2vec-using-numpy-and-google-sheets-13445eebd281)

#### 2.1.4. Loss

The strategy for word2vec is MLE. The loss is usually **negative log probability** (cross entropy).

<div  align="center">
  <img src=https://miro.medium.com/max/994/1*XPhzBnf1xEb0u67qazx9nA.png style = "zoom:70%">
</div>

Ref: 
- [TowardsDataScience](https://towardsdatascience.com/an-implementation-guide-to-word2vec-using-numpy-and-google-sheets-13445eebd281): NNL
- [HeidelbergU-Slide](https://www.cl.uni-heidelberg.de/courses/ss19/emb/material/word2vec2.pdf): Loss is NNL, i.e. CE.





### 2.2. Continuous Bag-of-Word (CBOW) (TBD)

CBOW means use the background words to predict the target word.

Ref: [GoogleSlides](https://docs.google.com/presentation/d/1yQWN1CDWLzxGeIAvnGgDsIJr5xmy4dB0VmHFKkLiibo/edit?usp=sharing),[TowardsDataScience](https://towardsdatascience.com/nlp-101-word2vec-skip-gram-and-cbow-93512ee24314)

### 2.3. Comparision between two archetecture

Ref: [StackOverflow](https://stackoverflow.com/questions/38287772/cbow-v-s-skip-gram-why-invert-context-and-target-words), [TowardsDataScience](https://towardsdatascience.com/nlp-101-word2vec-skip-gram-and-cbow-93512ee24314)
- Skip-gram: works well with small amount of the training data, represents well even rare words or phrases.

- CBOW: several times faster to train than the skip-gram, slightly better accuracy for the frequent words
- Why? (TBD)
  - From the prediction-input view
    - CBOW input context, predict the masked/missing word
      - For same context: *"yesterday is a _ day"*
      - CBOW prefer to predict high frequent "happy" rather than rare "delightful"
      - So CBOW will give better embedding for frequent words
    - For skip-gram
      - Since we use word to predict output
      - Both happy and delight would guess same context
      - So, skip-gram typically have better quality in rare words
  - From the algorithm view:
    - CBOW input are averaged during the process, so embedding tends to be better on statistical level, so favor frequent term.
    - skip-gram not have averaging process, so the embedding should be able to better reflect background, thus better embedding on word level.
  - Ref: [StackExchange](https://stats.stackexchange.com/questions/180548/why-is-skip-gram-better-for-infrequent-words-than-cbow)


Ref: [Medium](https://medium.com/analytics-vidhya/word2vec-cbow-skip-gram-algorithmic-optimizations-921d6f62d739)

1. CBOW is comparatively faster to train than skip-gram (as CBOW has to train only one softmax).
2. CBOW is better for frequently occurring words (because if a word occurs more often it will have more training words to train).
3. Skip-gram is slower but works well for the smaller amount of data then CBOW.
4. Skip-gram works well for less frequently occurring words than CBOW.
5. CBOW is a simpler problem than the Skip-gram (because in CBOW we just need to predict the one focus word given many context words).


## Deep Dive: Static vs Contextualized embeddings (TBD)

### static vs contextualized:
- “static”: word’s embedding not change with context;
- “contextualized”: word’s embedding changes with context. e.g.:
 - Sentence1: I eat an apple.
 - Sentence2: Apple iphone.
 - Static: vectors of “apple” is same in sentence 1 and 2.
 - Contextualized: vectors of “apple” are different in sentence 1 and 2.

Ref:
- [Quora](https://www.quora.com/What-are-the-main-differences-between-the-word-embeddings-of-ELMo-BERT-Word2vec-and-GloVe): “static” word embedding (like word2vec, glove, fasttext) vs “contextualized” word embedding (elmo, bert). 

### Differentiate different static embeddings

- Word2vec is the most basic one.
- Glove considers **concurrence**, so having **better linearity**.
- Fasttext considers **character n-gram**, having better ability to handle **out-of-vocabulary words**.

Ref:
- [Quora](https://www.quora.com/How-is-GloVe-different-from-word2vec): Word2vec vs Glove.
- [Medium](https://medium.com/swlh/a-quick-overview-of-the-main-difference-between-word2vec-and-fasttext-b9d3f6e274e9): Word2vec vs Fasttext

### Differentiate different contextualized embeddings:

- BERT uses Transformer,
- ELMO uses LSTM. 
  - (In english, BERT is a more deep/“advanced” model than ELMO.)

Ref:
- [Quora](https://www.quora.com/What-are-the-main-differences-between-the-word-embeddings-of-ELMo-BERT-Word2vec-and-GloVe): “static” word embedding (like word2vec, glove, fasttext) vs “contextualized” word embedding (elmo, bert). 



## Possible interview questions:

How to do word2vec? From [1p3c](https://www.1point3acres.com/bbs/thread-504099-1-1.html).


## DD2. Negative sampling

Instead of maximize the prob of all words, the negative sampling use select a handle of words to maximize their probability, which could reduce the complexity.

Ref: 
- [PersonalBlog](https://www.baeldung.com/cs/nlps-word2vec-negative-sampling)

Q: maximize similarity? Relation between it and entropy?
- Ref: [Stackoverflow](https://stackoverflow.com/questions/27860652/word2vec-negative-sampling-in-layman-term)