# Latent Semantic Analysis (LSA)

Ref: [Analyticsvidhya](https://www.analyticsvidhya.com/blog/2018/10/stepwise-guide-topic-modeling-latent-semantic-analysis/)

## Summary



- LSA is an application of Truncated SVD.

- Why to use LSA (Purpose):
  - The document term matrix is too sparse, LSA is used for word/docment embedding.
  - Find the latent relation between words/doc in order to improve information understanding (topics).
- Topic modelling application:
  - We can truncate to keep top-k topics for the datasets.
  - Also, each topic can be represented with terms (often) or documents (rare) according to needs.
- Word/Doc embedding application:
  - The documents (often) and terms (rare) can be represented with the k topics respectively.
- LSA is an unsupervised model.


## LSA Interpretation

Note: all the vectors are column vectors by default.

### Data Matrix

$\boldsymbol{A}_{m \times n}$: **document-term matrix**, i.e., our dataset.  
- $m$ number of documents in the corpus
- $n$ numer of terms (different words) in the corpus
- The matrix value $A_{ij}$ in denotes the frequency of $j$-th term in $i$-th doc.
<div  align="center"><img src=https://cdn.analyticsvidhya.com/wp-content/uploads/2018/09/Screenshot_4.png style = "zoom:80%"></div>

### SVD Interpretation

$$\boldsymbol{A}_{m \times n} \approx \boldsymbol{U}_{m \times k} \boldsymbol{\Sigma}_{k \times k} (\boldsymbol{V}_{n \times k})^{T}$$
  <div  align="center"><img src=https://cdn.analyticsvidhya.com/wp-content/uploads/2018/09/Screenshot_7.png style = "zoom:80%"></div>

- $k$: the number of topics we want to keep.
- $\boldsymbol{U}_{m \times k}$: **document-topic** matrix. Can be interpreted in row and column respectively:
  - $\boldsymbol{U}_{m \times k} = \begin{bmatrix}\boldsymbol{d}^T_{(\tau) 1}\\ \vdots \\\boldsymbol{d}^T_{(\tau) m}\end{bmatrix}$:
    - the i-th row of $\boldsymbol{U}$ is the representation of i-th document with the $k$ topics.
    - often used to get similarity between documents.
  - $\boldsymbol{U}_{m \times k} = [\boldsymbol{\tau}_{(d)1}... \boldsymbol{\tau}_{(d)k}]$: : 
    - the j-th column of $\boldsymbol{U}$ is the representation of j-th topic with the m documents.
- $\boldsymbol{V}_{n \times k}$: **term-topic** matrix. Can be interpreted in row and column respectively:
  - $\boldsymbol{V}_{n \times k} = \begin{bmatrix}\boldsymbol{t}^T_{(\tau) 1}\\ \vdots \\\boldsymbol{t}^T_{(\tau) n}\end{bmatrix}$ i.e. $(\boldsymbol{V}^T)_{k \times n}  = [\boldsymbol{t}_{(\tau) 1} \dots \boldsymbol{t}_{(\tau) n}]$:
    - the i-th column of $\boldsymbol{V}^T$ (i-th row in $\boldsymbol{V}$) is the representation of i-th term (word) with the $k$ topics.
  - $\boldsymbol{V}_{n \times k} = [\boldsymbol{\tau}_{(t)1}... \boldsymbol{\tau}_{(t)k}]$ i.e. $(\boldsymbol{V}^T)_{k \times n} = \begin{bmatrix} \boldsymbol{\tau}^T_{(t)1}\\ \vdots \\ \boldsymbol{\tau}^T_{(t)k} \end{bmatrix}$:
    - the j-th row of $\boldsymbol{V}^T$ (j-th column of $\boldsymbol{V}$) is a representation of j-th topic with the n terms (words).
    - often used to get similarity between topics.

Note: larger value means larger weight, smaller value mean smaller weight.

## Pros and cons

Pros:
- LSA is fast and easy to implement.
- It gives decent results, much better than a plain vector space model.

Cons:
- Since it is a linear model, it might not do well on datasets with non-linear dependencies.
- LSA assumes a Gaussian distribution of the terms in the documents, which may not be true for all problems.
- LSA involves SVD, which is computationally intensive and hard to update as new data comes up.


## Deep Dive: pLSA - Probabilistic LSA

Ref: [wiki-pLSA](https://en.wikipedia.org/wiki/Probabilistic_latent_semantic_analysis)

In pLSA, the matrix elements is defined as:

$$A_{ij} := p(d_i, t_{j})$$

I.e. replace the term frequency in LSA matrix with the probability of **occurrence** of $d_i$ and $w_{ij}$ as : $$p(d_i, t_{j}) := \frac{ \text{num of words same to } w_{ij} \text{ in } d_i \times \text{num of docs same to } d_i \text{ in } \mathcal{S} }{\text{number of words in } \mathcal{S} }$$

Note: If no doc duplication, then $p(d_i, t_{j})$ is proportional to the "term frequency", i.e. $\text{num of words same to } w_{ij} \text{ in } d_i$.

### DD-1. A probability interpretation of LSA and SVD

Ref: [Medium-blog](https://medium.com/nanonets/topic-modeling-with-lsa-psla-lda-and-lda2vec-555ff65b0b05), [wiki-pLSA](https://en.wikipedia.org/wiki/Probabilistic_latent_semantic_analysis)


<!-- I.e. $\boldsymbol{A} = p(D, W)$ -->
With SVD (matrix interpretation), we can decompose the matrix $\boldsymbol{A}$ with
$\boldsymbol{A}_{m \times n} \approx \boldsymbol{U}_{m \times k} \boldsymbol{\Sigma}_{k \times k} (\boldsymbol{V}_{n \times k})^{T}$ as:
$$A_{ij} = \sum^k_{z=1} U_{iz}\sigma_{z}V_{jz}, {\text{ with }} \boldsymbol{\Sigma} =  \begin{bmatrix} \sigma_{1} & & \\ & \ddots & \\ & & \sigma_{k} \end{bmatrix}$$

On the other side, with the probability interpretation, we have: 
  $$p(d_i,t_{j}) = \sum^k_{z=1} p(\tau_z)p(d_i|\tau_z)p(t_{j}|\tau_z)$$
Thus, with:
  $$\sum^k_{z=1} U_{iz}\sigma_{z}V_{jz} = A_{ij} = p(d_i, t_{j}) = \sum^k_{z=1} p(\tau_z)p(d_i|\tau_z)p(t_{j}|\tau_z)$$
  we can see the following correspondence:
  <div  align="center"><img src=https://miro.medium.com/max/552/1*SWvSwy3jdIiATmMXBCqQVg.png style = "zoom:80%"></div>

I.e.:

- $\sigma_z = p(\tau_z)$: the prior probability of the k topics in the corpus.
- $U_{iz} = p(d_i|\tau_z)$: the probability of document $i$ given topic $\tau_z$
- $V_{jz} = p(w_j|\tau_z)$: the probability of term $j$ given topic $\tau_z$

Further deduction:

- The column vector of $\boldsymbol{V}$ is like $\boldsymbol{V}_{n \times k} = [\boldsymbol{\tau}_{(t)1}... \boldsymbol{\tau}_{(t)k}]$
  - Then, $\boldsymbol{\tau}_{(t)z} =  \begin{bmatrix} V_{1z}\\ \vdots \\ V_{nz} \end{bmatrix} = \begin{bmatrix}p(w_1|\tau_z)\\ \vdots \\ p(w_n|\tau_z) \end{bmatrix}$
  - That is why $\boldsymbol{\tau}_{(t)z}$ can be understood as the representation of topic $\tau_z$ with the $n$ terms.
- Also, $\boldsymbol{A_V}_{iz} := [\boldsymbol{A}\boldsymbol{V}]_{iz} = [\boldsymbol{U}\Sigma]_{iz} = \sum^k_{z=1} p(\tau_z)p(d_i|\tau_z) = p(d_i,\tau_{z})$
  - That is why $\boldsymbol{A_V}_{m \times k} = \boldsymbol{AV}$ can be used as an alternative representation of $\boldsymbol{A}_{m\times n}$
  - I.e. If we regard $\boldsymbol{A}_{m\times n} = [p(d_i, t_{j})]$ as a bag of word representation, then $\boldsymbol{A_V}_{m\times k} = \boldsymbol{AV} = [p(d_i, \tau_{z})]$ is a "bag of topics" representation.
  - Note - geometric interpretation: $\boldsymbol{A_V}$ is the coordinates of $\boldsymbol{A}$ in new coordinate system of $\boldsymbol{V}$), see "$\boldsymbol{X_V}$" in [matrix_factorization.md](../general_machine_learning/math_topics/matrix_factorization.md)

### DD-2. A generative topic model.

Ref: [wiki-pLSA](https://en.wikipedia.org/wiki/Probabilistic_latent_semantic_analysis)

The pLSA model can also be interpreted in this way:

$$P(w,d)=\sum _{c}P(c)P(d|c)P(w|c)=P(d)\sum _{c}P(c|d)P(w|c)$$

The generative process can be shown as:
  <div  align="center"><img src=https://upload.wikimedia.org/wikipedia/commons/thumb/e/ec/Plsi_1.svg/640px-Plsi_1.svg.png style = "zoom:40%"></div>

Compared with [LDA](./latent_dirichlet_allocation.md) is much simpler.

