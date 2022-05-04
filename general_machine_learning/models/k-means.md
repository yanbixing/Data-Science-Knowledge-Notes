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
  - The algorithm has converged when the assignments no longer change. Then we can stop the iteration.

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
  - Minimize AIC/BIC
  - $AIC/BIC := D^*(K) + \Re(K)$
    - AIC punishment on K is not related to sample size; AIC punishment on K is not related to sample size, the more samples, the larger weight.
    - For very large sample set, AIC is usually better, especially , since BIC will have very high punishment on number of parameter, select very bad (underfitting) model.
    - Personal understanding: For K-means, BIC may be better, since the more sample the larger $D^*$, BIC will scale the punishment with the scale of $D^*$.
    - Ref:[Wiki-AIC_vs_BIC](https://en.wikipedia.org/wiki/Akaike_information_criterion#Comparison_with_BIC)
  - Details:
    - $\mathrm {AIC} := -2\ln({\hat {L}}) + 2n_{param} \underset{K-means}{ = } D^*(K) + 2Kn_{\dim}$ 
    - $\mathrm {BIC} :=-2\ln({\hat {L}}) + \ln(m)n_{param}\underset{K-means}{ = } D^*(K) + \ln(m)Kn_{\dim}$
      - $n_{param}:=$the number of estimated parameters
        - $n_{\dim}:=$ dimension of the clustering space
        - For k-means, $n_{param} = Kn_{\dim}$
      - $\hat {L}:=$ maximum value of the likelihood function for the model
        - For k-means maximize likelihood = minimize $D^*(K)$
      - $m:=$ number of samples
      
      - Ref: [Wiki - AIC](https://en.wikipedia.org/wiki/Akaike_information_criterion), [Wiki - BIC](https://en.wikipedia.org/wiki/Bayesian_information_criterion), [StackOverflow-AIC_to_KmeansAIC](https://stats.stackexchange.com/questions/271516/akaike-information-criterion-for-k-means), [StackOverflow-KmeansAIC_in_R](https://stackoverflow.com/questions/15839774/how-to-calculate-bic-for-k-means-clustering-in-r)
  
- Silhouette score: [Ref: Wiki-Silhouette](https://en.wikipedia.org/wiki/Silhouette_(clustering)), [Howto: Dzone](https://dzone.com/articles/kmeans-silhouette-score-explained-with-python-exam)
  - ${\text{silhouette score }} := \frac{\textbf{inter } \text{distance} - \textbf{intra } \text{distance}}{\max(\textbf{inter } \text{distance}, \textbf{intra } \text{distance})} \underset{\approx }{\propto} \frac{ { \textbf{inter } \text{distance}} }{ { \textbf{intra } \text{distance}} }$
    - "inter-distance": the average distance between points in **different** centroids
    - "intra-distance": the average distance between points within a **same** centroids
  - Maximize silhouette score, i.e. the larger silhouette, the better clustering result (the more similar for points in a same cluster.)



## 3. Other clustering methods

### 3.1. Gaussian Mixture Model (GMM)

#### 3.1.1. What is GMM?

A Gaussian mixture model is a probabilistic model that assumes **all the data points are generated from a mixture of a finite number of Gaussian distributions with unknown parameters.** 

One can think of mixture models as generalizing k-means clustering to incorporate information about the covariance structure of the data as well as the centers of the latent Gaussians. (???TBD)

Ref: [Sklearn-GMM](https://scikit-learn.org/stable/modules/mixture.html)

#### 3.1.2. Algorithm: GMM vs K-means (TBD)


 Kmeans can be viewed as a special case of GMM by :
- "Hard assign" the point to the closest point, rather than consider probablity
  - Ref: [Zhihu](https://zhuanlan.zhihu.com/p/71574416) <协方差矩阵(covariance matrix)趋于0? >, [CSDN](https://blog.csdn.net/qq_39638957/article/details/88744877): 
- Hard set the centroid have single variance rather than covariance matrix, i.e. set the shape to be spherical.
  - Ref: [Sklearn-GMM](https://scikit-learn.org/stable/modules/mixture.html#mixture)

Or GMM can be viewed as kind of more advanced K-means, by:

- Assign each centroid with a gaussian distribution $\boldsymbol{\mu}_k  \rightarrow \boldsymbol{\theta}_k = (\boldsymbol{\mu}_k, \boldsymbol{\Sigma}_k)$ (then the shape can be ellipse, not-spherical)
- The label of a data point is not a single label anymore. Instead, it becomes a $K$-dim vector denotes the probability the point belongs to $k$-th centroids.
- The objective change from "minimize sum distance" $\rightarrow$ "maximize likelihood"
  - Solve with EM algorithm
    - $\boldsymbol{Z}$ refers to the label distribution $\{\boldsymbol{s}_k\}$
    - $\boldsymbol{X}$ refers to the dataset $\mathcal{S}$.
- Usually, GMM use K-means to find starting points.
  - [Wiki-Kmeans](https://en.wikipedia.org/wiki/K-means_clustering#Gaussian_mixture_model), [TowardsDataScience](https://towardsdatascience.com/gaussian-mixture-models-vs-k-means-which-one-to-choose-62f2736025f0))


#### 3.1.3. Tunning (TBD)

- n_components: number of centroids.
- covariance_type: a param to control the shape of GMM, through covariance matrix.
  - 'spherical': each component/centroid have its own single **variance**.
    - I.e. the shape of cluster is spherical.
    - Diff vs k-mean: points label is determined by probability, which is related to the parameter. Not necessarily the closest centroid.
  - 'diag': each component/centroid have its own **diagonal** covariance matrix
    - I.e. the axis of ellipse are toward to coordinate, cannot be tilt.
  - 'tied': all components **share the same** covariance matrix
    - I.e. all the eclipses have a same shape/axis_tilt_direction.
  - 'full': each component/centroid have **its own** covariance matrix.
    - I.e. the shape/axis_tilt_direction/param of the eclipses are full determined by data.
  
  - <div  align="center"><img src=https://scikit-learn.org/stable/_images/sphx_glr_plot_gmm_covariances_001.png style = "zoom:60%"></div>



#### 3.1.4 Pro and Cons:


- Pros:
  - More flexible than K-mean: Can be eclipse shape, not necessarily spherical.
    - Ref: [Wiki-Kmeans](https://en.wikipedia.org/wiki/K-means_clustering)
  - Can give probability interpretation. Not necessarily belong to a single cluster.
- Cons:
  - More complex model, harder to tune, harder to select.
    - K-means are already determined by initialization and number of Ks. The flexibility in shape will give more uncertainty on final result.
- Also, see: [sklearn-mixture_models](https://scikit-learn.org/stable/modules/mixture.html#mixture)


### 4.2 DBSCAN:

#### 4.2.1. Algorithm:

Ref: [sklearn-Clustering-DBSCAN](https://scikit-learn.org/stable/modules/clustering.html#dbscan), [sklearn-DBSCAN](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.DBSCAN.html)

Idea: 

- Points that within certain distance (eps) will be considered as in a same cluster (label). (The two points within eps are considered as neighbours.)
  - For a point, if its number_of_neighbours > min_samples, it will be considered as a "core point"
  - For a point that have neighbour, but number_of_neighbours < min_samples, it will be considered as a non-core point, but still belong to the cluster.
- If a cluster don't have "core point", then the cluster will be considered as noise.
  - Formally: "Any sample that is not a core sample, and is at least eps in distance from any core sample, is considered an outlier by the algorithm."
  - Roughly (intuitive but NOT correct): If the number of samples in a cluster is not enough (below min_samples), then these points will be considered as noise.

#### 4.2.2 Tunning:

Param:

  - eps: the minimum distance between two samples to be considered belong to a same cluster.
    - eps $\downarrow$, (result) complexity $\uparrow$, model variance $\uparrow$
  - min_samples: the minimum num of samples a cluster must have, below which, will be considered as noise.
    - min_samples $\downarrow$, (result) complexity $\uparrow$, model variance $\uparrow$


#### Pro and Cons:

- Pros:
  - Arbitrary shape, not necessarily spherical like k-means or eclipse like GMM
  - No need to predetermine number of cluster i.e. K. 
    - In practice, determine distance threshold are more easy/intuitive than determine number of clusters.
  - Not affected by initialization. The "solution" is fixed/definite/unique given fixed param and data.
    - Because DBSCAN don't have "centroid", it works based on distance between points, which is always fixed.
    - Ref: [Dummies](https://www.dummies.com/programming/big-data/data-science/how-to-create-an-unsupervised-learning-model-with-dbscan/)
  - In practice, usually faster, because no need to iterate.
    - Worst time complexity: O(n²). [Wiki-DBSCAN](https://en.wikipedia.org/wiki/DBSCAN#Complexity)
- Cons:
  - No "centroid" concept, usually a lot of "core points".
  - Sensitive to scale, the eps should varies when you normalize the scale of date.
  - Take more storage space than KNN.
    - KNN model-storage complexity: O(K)
    - DBSCAN model-storage complexity: O(n)(need to remember all points).[Wiki-DBSCAN](https://en.wikipedia.org/wiki/DBSCAN#Complexity)

#### Misc

- non-parametric. Ref: [Wiki-DBSCAN](https://en.wikipedia.org/wiki/DBSCAN)

  

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
  - E-step: Calculate $P(Z_t|X,\theta_t)$ and then  $L({\boldsymbol {\theta }};\mathbf {X} )_{|\boldsymbol{Z} = \boldsymbol{Z}_t}$, i.e.:
    - "E" for "Expectation of Z" : update distribution of label $\boldsymbol{Z}_t$ of each hidden state based based on current parameter $\boldsymbol{\theta}_t$ and observable $\mathbf{X}$, so that we can get the expected log-likelihood function $${\displaystyle L({\boldsymbol {\theta }};\mathbf {X} )=p(\mathbf {X} \mid {\boldsymbol {\theta }})=\int p(\mathbf {X} ,\mathbf {Z} \mid {\boldsymbol {\theta }})\,d\mathbf {Z} =\int p(\mathbf {X} \mid \mathbf {Z} ,{\boldsymbol {\theta }})p(\mathbf {Z} \mid {\boldsymbol {\theta }})\,d\mathbf {Z} }$$.
  - M-step: Calculate $\boldsymbol{\theta}_{t+1} = \underset{\boldsymbol{\theta}}{\argmin} L({\boldsymbol {\theta }};\mathbf {X} )_{|\boldsymbol{Z} = \boldsymbol{Z}_t}$
    - "M" for "Maximize likelihood": update the parameter $\boldsymbol{\theta}$ by maximize the likelihood of current label distribution.
- Further understanding:
  - EM algorithm can be understand as a generalized coordinate descent:
    - $\boldsymbol{\theta}$ is an axis and $\boldsymbol{Z}$ is another axis
    - $\boldsymbol{\theta}$ and $\boldsymbol{Z}$ is optimized/updated iteratively
      - In formal coordinate descent, axis $x^{(1)}...x^{(N)}$ is optimized iteratively.
    - Ref: [RochesterU](https://www.cs.rochester.edu/u/jliu/CSC-576/class-note-8.pdf)


