# K-means

K-means is an unsupervised clustering method.

## 0. Notations:

- $K$: number of centroids. 
  - $\boldsymbol{c}_k$: the position of $k$-th centroid.
- $\mathcal{S}$: the sample sets, with $M$ number of samples $\{\boldsymbol{x}_1,...,\boldsymbol{x}_M\}$, 
- $\mathcal{s}(\boldsymbol{x}_i)$: the label of $i$-th sample.
  - $\mathcal{s}_k$: the group of samples belongs to $k$-th centroid.
  - $|\mathcal{s}_k|$: number of samples in $\mathcal{s}_k$.
- $d(\boldsymbol{x}_i,\boldsymbol{x}_j)$: the distance between $\boldsymbol{x}_i, \boldsymbol{x}_j$


## 1. Training

Ref: [wiki-K_means](https://en.wikipedia.org/wiki/K-means_clustering#Standard_algorithm_(naive_k-means))

### 1.1. Algorithm

- Objective: Given certain $K$, minimize sum of distance from centroids to corresponding data points. i.e.:
  $$\text{minimize } \sum^K_{k=1}\sum_{x_i \in \mathcal{s}_k}d(\boldsymbol{x}_i, \boldsymbol{c}_k)$$
- Algorithm: EM algorithm
  - E-step: $\forall i$ calculate $s(\boldsymbol{x}_i) = \underset{k}{\argmin} d(\boldsymbol{x}_i, \boldsymbol{c}_k)$
    - i.e.: Assign sample $\boldsymbol{x}_i$ to its closest centroid $k$ for all samples ($\{\mathcal{s}_k\}$ is updated)
  - M-step: $\boldsymbol{c}_k =  \frac{1}{|\mathcal{s}_k|}\sum_{\boldsymbol{x}_i \in \mathcal{s}_k} \boldsymbol{x_i} $
    - i.e.: Move the centroid to the center of corresponding group ($\{\boldsymbol{c}_k\}$ is updated.)

### 1.2. Centroids Initialization

- Forgy: randomly choose $K$ points from the dataset and use them as the initial centroid position
  - Idea: the method = Initialize EM-algorithm with a randomized M-step state, then, start with E-step.
  - Compare to Random Partition, Forgy method tends to **spread out** the centroids.
- Random partition: randomly assign
  - Idea: the method = Initialize EM-algorithm with a randomized E-step state, then, start with M-step.
  - Compare to Forgy,the Random Partition tends to make centroids **closer to the center of the data set**.
- K-means++: a more recent initialization method, better spread out the centroids. [Ref: Wiki-K_means++](https://en.wikipedia.org/wiki/K-means%2B%2B)
  - Target: separate the centroids as farther as possible.
    - Idea: the more a point far away from the centroids, the better to choose it as the initial position for a centroids
  - Algorithm:
    - Randomly choose a data points as the centroid.
    - For a remaining points, calculate their distance to its nearest centroid.
    - The probability of this point chosen as next centroid is proportional to the square of the distance
  

## 2. Parameter tunning

<!-- In the above part, we can find the optimized position of centroids given the number of centroids. Then, how to know how many centroids should we take? -->
Different $K$ corresponds to different optimized centroids distribution, then, which $K$ value is the best?

Define $D^*(K) := \min[ \sum^K_{k=1}\sum_{x_i \in \mathcal{s}_k}d(\boldsymbol{x}_i, \boldsymbol{c}_k) ]$

Typically, $K\uparrow \Rightarrow D^*(K) \downarrow$ 

The metrics used to select K is as following.

- $D^*(K)$:
  - Elbow method, i.e., manually identify the elbow point on $(K,D^*(K))$ plot. [Ref: Wiki-Elbow_method](https://en.wikipedia.org/wiki/Elbow_method_(clustering))
- AIC/BIC: '$K$-regularized' $D^*$ score (i.e. add a punishment on $K$)
  - $AIC/BIC := D^*(K) + \Re(K)$
  - Minimize AIC/BIC
- Silhouette score: [Ref: Wiki-Silhouette](https://en.wikipedia.org/wiki/Silhouette_(clustering))
  - ${\text{silhouette score }} \approx \frac{ { \textbf{inter } \text{distance}} }{ { \textbf{intra } \text{distance}} }$
    - "inter-distance": the average distance between points in **different** centroids
    - "intra-distance": the average distance between points within a **same** centroids
  - Maximize silhouette score, i.e. the larger silhouette, the better clustering result (the more similar for points in a same cluster.)


## 3. Gaussian Mixture Model (GMM)

In short, a more advance K-means:

- Assign each centroid with a gaussian distribution $\boldsymbol{\mu}_k \rightarrow \boldsymbol{\theta}_k = (\boldsymbol{\mu}_k, \boldsymbol{\Sigma}_k)$
- The label of a data point is not a single label anymore. Instead, it becomes a $K$-dim vector denotes the probability the point belongs to $k$-th centroids.
- The objective change from "minimize sum distance" $\rightarrow$ "maximize likelihood"
  - Solve with EM algorithm
    - $\boldsymbol{Z}$ refers to the label distribution $\{\boldsymbol{s}_k\}$
    - $\boldsymbol{X}$ refers to the dataset $\mathcal{S}$.

## 4. Other clustering methods



## Deep Dive: Expectation–maximization (EM) algorithm 

Also see: [NLP-HMM](../../natural_language_processing/hidden_markov_model.md)

Ref: [Wiki-EM_algorithm](https://en.wikipedia.org/wiki/Expectation%E2%80%93maximization_algorithm)

- When using EM on a function, the function usually have three components:
  - $\boldsymbol{X}$: the observed data. E.g.:
    - GMM: Data points
    - HMM: Observation
  - $\boldsymbol{Z}$: latent/unknown intermediate data.
    - GMM: Centroids' label.
    - HMM: Hidden states' label.
  - $\boldsymbol{\theta}$: the parameters.
    - GMM: parameters for centroids, i.e., $\boldsymbol{\theta} = (\boldsymbol{\mu}, \boldsymbol{\Sigma})$
    - HMM: parameters for hidden states, i.e., $\boldsymbol{\theta} = (\boldsymbol{A}, \boldsymbol{B}, \boldsymbol{\pi})$
- Algorithm: 
  - E-step: Calculate $P(Z|X,\theta)$, i.e.:
    - "E" for "Expectation of Z" : update distribution of label $\boldsymbol{Z}$ assigned to each data point in $\boldsymbol{X}$ based on current parameter $\boldsymbol{\theta}$
  - M-step: Calculate $\boldsymbol{\theta} = \underset{\boldsymbol{\theta}}{\argmin}P(\boldsymbol{X},\boldsymbol{Z}|\boldsymbol{\theta})$
    - "M" for "Maximize likelihood": update the parameter $\boldsymbol{\theta}$ by maximize the likelihood of current label distribution.
- Further understanding:
  - EM algorithm can be understand as a generalized coordinate descent:
    - $\boldsymbol{\theta}$ is an axis and $\boldsymbol{Z}$ is another axis
    - $\boldsymbol{\theta}$ and $\boldsymbol{Z}$ is optimized/updated iteratively
      - In formal coordinate descent, axis $x^{(1)}...x^{(N)}$ is optimized iteratively.
    - Ref: [RochesterU](https://www.cs.rochester.edu/u/jliu/CSC-576/class-note-8.pdf)


