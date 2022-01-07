# k-nearest neighbors

## Notation

- $K$: number of nearest neighbors to take
- $N$: number of samples in the ("training") dataset

## Algorithm

k-nearest neighbors (KNN) algorithm: given a data point $\boldsymbol{x}$, the prediction $f(x)$ is computed from $K$ "nearest" points in certain distance metrics.

- The distance metric can be chosen for different purpose, typical options include Euclidean distance and cosine distance.
- KNN can be used as both regressor (taking the average) or classifier (taking the majority).

## Properties

- KNN is a non-parametric model (the number of parameter varies.)
- KNN doesn't have a "training" process, computation is made during the inference process.
  - Computationally and memory expensive if the dataset is large. [Time complexity: $O(KN)$, space complexity: $O(N)$]
    - Calculate distance from sample to every point in training set: $O(N)$
    - Select K points with smallest distance: $O(KN)$ 
      - Select 1 points with smallest distance: $O(N)$ [Ref: StackOverflow](https://stackoverflow.com/questions/53122831/complexity-of-the-following-algorithm-to-find-minimum-value-in-an-unsorted-array) 
      - Repeat $K$ times.
  - There are algorithms/implementation to accelerate the process by partition the space to regions ( e.g. [Approximate Nearest Neighbors: "ANNOY" by spotify](https://github.com/spotify/annoy) )


## Parameter Tunning (Bias-Variance-Tradeoff)

- $K \uparrow \Rightarrow$ model variance $\downarrow$, i.e.:
  - Large $K$: high bias low variance, prone to underfitting
  - Small $K$: high variance low bias, prone to overfitting 
  <div  align="center"><img src=https://github.com/ShuaiW/data-science-question-answer/blob/master/assets/knn.png?raw=true style = "zoom:90%"></div>