# NLP General

## NLP Process

### Tokenization (Tokenizer):

- A process split string to words.
  - Splitter: whitespace, comma, enter, etc. 
- Related techs:
  - Stemming: return the root of the word
  - Lemmatization: return the base form of the word.
  - Stop words: words to be filtered out, usually the most common "no-meaning" words (e.g. the, to, of, for, etc.)

### Vectorization (Vectorizer):

A process converting string to vector.

#### Related techs:

- [Bag of words (BOW)](https://en.wikipedia.org/wiki/Bag-of-words_model): break the sentence to words, disregarding grammar or order, only keep word frequency.
  - e.g. string object -> dict object {words:frequency}
  - Cons: 
    - No order
      - Can be solved with:
        - n-gram tech (bag of n-gram)
        - sequential embedding techs like RNN.
    - Not normalized
      - Can be solved with vectorization techs considering document frequency like tfidf.
    - Too sparse (1 word 1 dimension)

- [n-gram](https://en.wikipedia.org/wiki/Bag-of-words_model#n-gram_model): combine n-words together as the basic unit.
  - "bag of n-gram": some degree considering the order.
  - Cons: 
    - Dimension is too high: n-gram's dimension is n-th power of the dimension of uni-gram.
      - Can be solved by threshold cutting: remove word beyond thresholds like max_freq, min_freq
    - Too sparse, too many 0s
      - Can be solved by
        - Dimension reduction techs 
          - Ref: [feature_engineering-Sec 2.2](../general_machine_learning/data_engineering/feature_engineering.md)
        - 0 filling
          - Missing value techs, ref [feature_engineering-Sec 2.1](../general_machine_learning/data_engineering/feature_engineering.md)
          - Smoothing techs, like Laplacian smoothing: For all features, add 1 count to the bag. (To guarantee all features have at least 1 count; then, the total counts increase with the number of features. ) Ref: [Lazyprogrammer blog](https://lazyprogrammer.me/probability-smoothing-for-natural-language-processing/)

## NLP Tasks

### Part of speech (PoS) Tagging

Find the grammatical tag (noun, adjective, verb, etc.) of the word.

A classification task. The output is 

#### Typical models

- HMM (TBD)
  - Training method: MLE (Maximum likelihood estimation)
    - Expression of the likelihood? $L(\mathcal{S}) = \prod^m_i p( {\text{tag}}_i, {\text{word}}_i)$?
  - The transition matrix?
  - P as a function of transition matrix?