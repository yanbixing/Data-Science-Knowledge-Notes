# Text vectorization

## HashVectorizer

### Pro and Con

Pros:
    it is very low memory scalable to large datasets as there is no need to store a vocabulary dictionary in memory
    it is fast to pickle and un-pickle as it holds no state besides the constructor parameters
    it can be used in a streaming (partial fit) or parallel pipeline as there is no state computed during fit.

Cons
    there is no way to compute the inverse transform (from feature indices to string feature names) which can be a problem when trying to introspect which features are most important to a model.
    there can be collisions: distinct tokens can be mapped to the same feature index. However in practice this is rarely an issue if n_features is large enough (e.g. 2 ** 18 for text classification problems).
no IDF weighting as this would render the transformer stateful.

Ref: [HashingVectorizer](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.HashingVectorizer.html)
