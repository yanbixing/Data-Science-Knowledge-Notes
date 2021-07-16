# Latent Semantic Analysis (LSA)

Ref: [Analyticsvidhya](https://www.analyticsvidhya.com/blog/2018/10/stepwise-guide-topic-modeling-latent-semantic-analysis/)

## Summary

Note: all the vectors are column vectors by default.

- LSA is an application of Truncated SVD.
- LSA is a topic model.
  - We can truncate to keep top-k topics for the datasets.
  - The documents (often) and terms (rare) can be represented with the k topics respectively.
  - Also, each topic can be represented with terms (often) or documents (rare) according to needs.

## LSA Interpretation

### Data Matrix

$\mathbf{A}_{m \times n}$: **document-term matrix**, i.e., our dataset.  
- $m$ number of documents in the corpus
- $n$ numer of terms (different words) in the corpus
- The matrix value $A_{ij}$ in denotes the frequency of $j$-th term in $i$-th doc.
<div  align="center"><img src=https://cdn.analyticsvidhya.com/wp-content/uploads/2018/09/Screenshot_4.png style = "zoom:80%"></div>

### SVD Interpretation

$$\mathbf{A}_{m \times n} \approx \mathbf{U}_{m \times k} \mathbf{\Sigma}_{k \times k} (\mathbf{V}_{n \times k})^{T}$$
  <div  align="center"><img src=https://cdn.analyticsvidhya.com/wp-content/uploads/2018/09/Screenshot_7.png style = "zoom:80%"></div>

- $k$: the number of topics we want to keep.
- $\mathbf{U}_{m \times k}$: **document-topic** matrix. Can be interpreted in row and column respectively:
  - $\mathbf{U}_{m \times k} = \begin{bmatrix}\mathbf{d}^T_{(\tau) 1}\\ \vdots \\\mathbf{d}^T_{(\tau) m}\end{bmatrix}$:
    - the i-th row of $\mathbf{U}$ is the representation of i-th document with the $k$ topics.
    - often used to get similarity between documents.
  - $\mathbf{U}_{m \times k} = [\boldsymbol{\tau}_{(d)1}... \boldsymbol{\tau}_{(d)k}]$: : 
    - the j-th column of $\mathbf{U}$ is the representation of j-th topic with the m documents.
- $\mathbf{V}_{n \times k}$: **term-topic** matrix. Can be interpreted in row and column respectively:
  - $\mathbf{V}_{n \times k} = \begin{bmatrix}\mathbf{t}^T_{(\tau) 1}\\ \vdots \\\mathbf{t}^T_{(\tau) n}\end{bmatrix}$ i.e. $(\mathbf{V}^T)_{k \times n}  = [\mathbf{t}_{(\tau) 1} \dots \mathbf{t}_{(\tau) n}]$:
    - the i-th column of $\mathbf{V}^T$ (i-th row in $\mathbf{V}$) is the representation of i-th term (word) with the $k$ topics.
  - $\mathbf{V}_{n \times k} = [\boldsymbol{\tau}_{(t)1}... \boldsymbol{\tau}_{(t)k}]$ i.e. $(\mathbf{V}^T)_{k \times n} = \begin{bmatrix} \boldsymbol{\tau}^T_{(t)1}\\ \vdots \\ \boldsymbol{\tau}^T_{(t)k} \end{bmatrix}$:
    - the j-th row of $\mathbf{V}^T$ (j-th column of $\mathbf{V}$) is a representation of j-th topic with the n terms (words).
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