# Text vectorization

## Vectorizer

### HashVectorizer

####  Algo: 

- breaking the string to words (bag-of-words representation, a multi-column/dict object)
-  use feature_hasher on the bag-of-words representation.
-  Ref. [feature_engineering.md-Sec1.2.3.5](../general_machine_learning/data_engineering/feature_engineering.md)

#### Pro and Con

- Pros:
  - smaller: can be lower dimension compared to counter vectorizer.
  - faster: no need to memorize vocabulary dictionary
  - ? it can be used in a streaming (partial fit) or parallel pipeline as there is no state computed during fit.
- Cons:
  - cannot inverse transform (from vector to bag-of-words representation), due to
    - Hash function cannot be inverse
    - The vocabulary dictionary is not memorized.
  - collisions: distinct tokens can be mapped to the same feature index.
  - cannot apply IDF weighting, as each column is not a word unit feature.
- Ref: [HashingVectorizer](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.HashingVectorizer.html)
