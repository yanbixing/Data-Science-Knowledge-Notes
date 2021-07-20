# Latent Dirichlet Allocation (LDA)

## 0. Notation

Quantities

- $\mathcal{S}$: the corpus.
- $d_i$: the $i$-th doc in the corpus.
  - $m$: total number of documents in the corpus
- $w_{ij}$: the $j$-th word in the $d_i$.
  - $N_i$: total number of words in $d_i$
- $k$: the index of $k$-th topic
  - $K$: total number of topic over the corpus.


Expressions:

- Distribution $F(\boldsymbol{\theta}_i)$ means the distribution is determined by parameter $\theta$.
- $\theta \sim \operatorname {Dir} (\alpha )$ means randomly draw a sample as $\theta$ from the Dirichlet distribution
- $\operatorname {Dir} ({\boldsymbol {\alpha }})$ is a [Dirichlet distribution](https://en.wikipedia.org/wiki/Dirichlet_distribution) with parameter $\alpha$. 
  - Just like Gaussian distribution have parameter $\mu, \sigma$, [Dirichlet distribution](https://en.wikipedia.org/wiki/Dirichlet_distribution) also have parameter to tune the shape of the distribution.

## 1. LDA model

LDA is a generative model

### 1.1. Idea of the model

Ref: [Wiki-LDA](https://en.wikipedia.org/wiki/Latent_Dirichlet_allocation)

LDA model assumes:

- On word level, each **document** $d_i$ would have its a probability distribution on **topic**: $F(\boldsymbol{\theta}_i)$ for $d_i$ .
  - e.g.: Document $i$ have 10 words in total. 3 words have topic $t_1$, 6 words have topic $t_2$
    - The topic distribution $F(\boldsymbol{\theta}_i) = \{t_1: 0.3, t_2: 0.7\}$
- The parameters $\boldsymbol{\theta}$ of all documents (over the corpus) i.e. $\{\boldsymbol{\theta}_1, \boldsymbol{\theta}_2, ...\boldsymbol{\theta}_m\}$, follows a [Dirichlet distribution](https://en.wikipedia.org/wiki/Dirichlet_distribution) $\operatorname {Dir} ({\boldsymbol {\alpha }})$. 
  
- Each **topic** $k$ have a probability distribution on **word** $G(\boldsymbol{\varphi}_k)$
- The parameters $\boldsymbol{\varphi}$ of all topics i.e. $\{\boldsymbol{\varphi}_1, \boldsymbol{\varphi}_2, ...\boldsymbol{\varphi}_K\}$, follows a [Dirichlet distribution](https://en.wikipedia.org/wiki/Dirichlet_distribution) $\operatorname {Dir} ({\boldsymbol {\beta }})$. 

**Note:**

- $\boldsymbol{\theta}$ and $F(\boldsymbol{\theta})$ are document level quantity. 
  - I.e. different document $i$ have different $\boldsymbol{\theta}_i$, $F(\boldsymbol{\theta}_i)$
- $\boldsymbol{\varphi}$ and $G(\boldsymbol{\varphi})$ are corpus level quantity. 
  - I.e. different document $i$ share a same $\boldsymbol{\varphi}_k$, $G(\boldsymbol{\varphi}_k)$, $k$ denotes the $k$-th topic
  - I.e. the word distribution $G(\boldsymbol{\varphi}_k)$ is different for different topic. But for different documents, it is same for the same topic.

### 1.2. Generative process

Ref: [Wiki-LDA](https://en.wikipedia.org/wiki/Latent_Dirichlet_allocation)

Task: Given $\boldsymbol{\alpha}, \boldsymbol{\beta}$, we aim to generate m documents.

<!-- <div  align="center">
  <img src=https://upload.wikimedia.org/wikipedia/commons/4/4d/Smoothed_LDA.png style = "zoom:60%">
</div> -->

- For the whole corpus, choose a $\boldsymbol{\varphi}_{k}\sim \operatorname {Dir} (\boldsymbol{\beta} )$ for each topic $k$
   - I.e. Then we a word distribution parameter for every topic: $\{\boldsymbol{\varphi}_1, \boldsymbol{\varphi}_2, ...\boldsymbol{\varphi}_K\}$
   - I.e. Then we a certain word distribution for every topic: $\{G(\boldsymbol{\varphi}_1), G(\boldsymbol{\varphi}_2), ...G(\boldsymbol{\varphi}_K)\}$

- When generating the i-th documents (the length of document is set as $N_i$):
  - Generate a $\boldsymbol{\theta}_{i}\sim \operatorname {Dir} (\boldsymbol{\alpha} )$
  - For each word position $(i,j)$ ($j\in\{1,2,...,N_i\}$):
    - Choose a topic $\tau_{i,j} \sim F(\boldsymbol{\theta}_i)$ 
    - Choose a word $w_{i,j} \sim G(\boldsymbol{\varphi}_{\tau_{i,j}})$ 

An illustration on how the word $w_{i,j}$ is generated:

<div  align="center">
  <img src=https://upload.wikimedia.org/wikipedia/commons/4/4d/Smoothed_LDA.png style = "zoom:60%">
</div>


### 1.3. Training: 



#### Param and Hyperparam:

- $\varphi, \theta, Z$ are the parameter to be fit in the training process
- $\alpha, \beta(\eta), K$ are the hyperparameter need to be set manually.
  - Other hyperparameter includes: decay

#### Notation:
- Assume there are $K$ topics across all the corpus ($M$ documents).
- $F_{d_i}$: the topic distribution in i-th doc.
  - e.g. $F_{d_i} = \{\tau_1: 0.3, \tau_2:0.7 \}$ means: in $d_i$, 30\% words are labeled with $\tau_1$, 70\% words are labeled with $\tau_2$
- $T_{w_{i,j}}$: the topic distribution for word $w_{i,j}$ In the corpus $\mathcal{S}$. 
  - e.g. $T_{w_{i,j}} = \{\tau_1: 0.1, \tau_2:0.9 \}$ means: In the corpus $\mathcal{S}$, 10\% word $w_{i,j}$ are labeled with $\tau_1$, 90\% word $w_{i,j}$ are labeled with $\tau_2$
- $n_{(\tau_k,d_i)}$: number of topic $\tau_k$ in $d_i$ 
- $n_{(\tau_k,\mathcal{S})}$:  number of topic $\tau_k$ in the corpus $\mathcal{S}$ 
- $n_{(w_{i,j},\tau_k)}$: number of word $w_{i,j}$ have the topic $\tau_k$ in the corpus $\mathcal{S}$

#### Algorithm: (Collapsed) Gibbs sampling

- Randomly distribute the K topics to all words in all documents. Then we can have two quantities:
 


- For $i=1 \rightarrow m$ (for each document in the corpus)
  - For $j=1 \rightarrow N_i$ (for each word in the $i$-th document)
    - Exclude the word $w_{ij}$ from the doc (and corpus) (also its corresponding topic $\tau_k$ is removed.). The related quantity is recalculated as:
      - $n'_{(\tau_k,d_i)} = n_{(\tau_k,d_i)}-1$ ($\because$ remove one $\tau$)
      - $n'_{(\tau_k,\mathcal{S})} = n_{(\tau_k,\mathcal{S})} -1$, ($\because$ remove one $\tau$)
      - $n'_{(w_{i,j},\tau_k)} = n_{(w_{i,j},\tau_k)}-1$ ($\because$ remove 1 word with $\tau$)
      - $N'_{d_i} = N_{d_i}-1$ ($\because$ remove 1 word from $d_i$)
    - Recalculate (the quantities above will be used): 
      - the topic distribution in the $i$-th document $F'_{d_i}$
      - the topic distribution for word $w_{i,j}$ in the corpus $T'_{w_{i,j}}$.
    - Add the word back, and assign a new topic to the word according to the new $F'_{d_i}$ and $T'_{w_{i,j}}$

Note: How to assign topic according to $F_{d_i}$ and $T_{w_{i,j}}$?
e.g.
- $F_{d_i} = \{\tau_1: 0.3, \tau_2:0.7 \}$: the probability of a topic in the **document** $d_i$.
- $T_{w_{i,j}} = \{\tau_1: 0.1, \tau_2:0.9 \}$: the probability of a topic for the **word** $w_{ij}$ in the **corpus**.
- We use their product as the probability of a topic for the **word** $w_{ij}$ in **document** $d_i$, i.e.: 
  - $T_{(w_{i,j},d_i)} =  \{\tau_1: 0.01, \tau_2:0.63 \} \underset{ \text{normalize} }{\rightarrow} \{\tau_1: \frac{0.01}{0.64}, \tau_2:\frac{0.63}{0.64} \}$ 
  - The topic should be drawn from this distribution.



#### Referece: 
  - [NEU lecture](https://www.ccs.neu.edu/home/vip/teach/DMcourse/5_topicmodel_summ/notes_slides/sampling/darling-lda.pdf): good algorithm procedure representation.
  - [Mediem - blog](https://medium.com/analytics-vidhya/topic-modeling-using-lda-and-gibbs-sampling-explained-49d49b3d1045): good algorithm visual representation.
    - Blue horizontal bar: probability distribution of topic in the document $i$ (exclude the contribution of the existing topic of the current word)
    - Origin vertical bar: probability distribution of topic for the word over documents. (exclude the contribution of the current word $w_{i,j}$)
    - Grey area: the new probability distribution (not normalized) of topic of current word $w_{i,j}$ in doc $i$.
  - [](), 
  - [Github-blog](https://wiseodd.github.io/techblog/2017/09/07/lda-gibbs/): code for algorithm

## Model comparison

Ref: [Medium - blog](https://medium.com/nanonets/topic-modeling-with-lsa-psla-lda-and-lda2vec-555ff65b0b05)

### LDS vs LSI

[LSI (LSA).md](./latent_semantic_analysis.md)

LSI is SVD. Can be used for dimension reduction. Similarity is kept.

LDA is a probability model.



## Notes:

- [Dirichlet distribution](https://en.wikipedia.org/wiki/Dirichlet_distribution): a kind of multivariate probability distribution.
