# ML model concepts (specific)

### K-means vs KNN

- [KNN](../models/k-nearest_neighbors.md): K-nearest neighbor 
  - Label is predicted based on label of k nearest neighbors. 
  - Supervised learning.
- [K-means](../models/k-means.md): K centroids (locating at the center/**mean** of its cluster)
  - Unsupervised learning.
- Source: Ref: [Springboard-blog]( https://www.springboard.com/blog/machine-learning-interview-questions/)


### Ranking score and probability score.



- The score from most model is comparative "within-model", i.e. able to be used as ranking score.
  - Logistic Regression, SVM, Tree models
- The score from a only a few of models can be interpreted as probability:
  - Logistic Regression

- SVM: score is not probability.
- Tree models:
  - Decision Tree: a single decision tree cannot give prob, only give prediction.
  - Bagging Tree: can, but the probability is better interpreted as an ensemble of votes from different individual decision tree.




