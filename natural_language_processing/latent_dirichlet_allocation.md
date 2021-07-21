# Latent Dirichlet Allocation (LDA)

## 0. Notation

Quantities

- $\mathcal{S}$: the corpus.
  - $m$: total number of documents in the corpus.
  - $N_{\mathcal{S}-uni}$: total number of unique words in the corpus.
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

LDA is a generative model. It assume the word and topic follows a [multinomial distribution](https://en.wikipedia.org/wiki/Multinomial_distribution) distribution depending on some hidden states (parameter $\boldsymbol{\varphi}, \boldsymbol{\theta}$). And the unobserved/hidden states (parameter) of the multinomial distribution follows a dirichlet distribution.

- Note: 
  - [Dirichlet distribution](https://en.wikipedia.org/wiki/Dirichlet_distribution) is a multivariate version [beta distribution](https://en.m.wikipedia.org/wiki/Beta_distribution). 
  - Beta distribution is probabilistic version of binominal distribution.
  - Binominal distribution is the distribution of results when we throw coins, like (3T5H).
  - Ref: [TowardsDataScience-blog-dirichlet_distribution](https://towardsdatascience.com/dirichlet-distribution-a82ab942a879), [Wiki-Beta_distribution](https://en.m.wikipedia.org/wiki/Beta_distribution), [TowardsDataScience-blog-beta_distribution](https://towardsdatascience.com/beta-distribution-intuition-examples-and-derivation-cf00f4db57af#:~:text=The%20difference%20between%20the%20binomial,probability%20is%20a%20random%20variable.)

### 1.1. Idea of the model

Ref: [Wiki-LDA](https://en.wikipedia.org/wiki/Latent_Dirichlet_allocation)

LDA model assumes:

- On word level, each **document** $d_i$ would have its a probability distribution on **topic**: $F(\boldsymbol{\theta}_i)$ for $d_i$ .
  - e.g.: Document $i$ have 10 words in total. 3 words have topic $t_1$, 6 words have topic $t_2$
    - The topic distribution $F(\boldsymbol{\theta}_i) = \{t_1: 0.3, t_2: 0.7\}$
- The parameters $\boldsymbol{\theta}$ of all documents (over the corpus) i.e. $\{\boldsymbol{\theta}_1, \boldsymbol{\theta}_2, ...\boldsymbol{\theta}_m\}$, follows a [Dirichlet distribution](https://en.wikipedia.org/wiki/Dirichlet_distribution) $\operatorname {Dir} ({\boldsymbol {\alpha }})$. 
  
- Each **topic** $k$ have a probability distribution on **word** $G(\boldsymbol{\varphi}_k)$
- The parameters $\boldsymbol{\varphi}$ of all topics i.e. $\{\boldsymbol{\varphi}_1, \boldsymbol{\varphi}_2, ...\boldsymbol{\varphi}_K\}$, follows a Dirichlet distribution $\operatorname {Dir} ({\boldsymbol {\beta }})$. 

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

<div  align="center"><img src=https://upload.wikimedia.org/wikipedia/commons/4/4d/Smoothed_LDA.png style = "zoom:60%"></div>


### 1.3. Training of the model: 

#### 1.3.0. Notation:
- Assume there are $K$ topics across all the corpus ($M$ documents).
- $F_{d_i}$: the topic distribution in i-th doc.
  - e.g. $F_{d_i} = \{\tau_1: 0.3, \tau_2:0.7 \}$ means: in $d_i$, 30\% words are labeled with $\tau_1$, 70\% words are labeled with $\tau_2$
- $T_{w_{i,j}}$: the topic distribution for word $w_{i,j}$ In the corpus $\mathcal{S}$. 
  - e.g. $T_{w_{i,j}} = \{\tau_1: 0.1, \tau_2:0.9 \}$ means: In the corpus $\mathcal{S}$, 10\% word $w_{i,j}$ are labeled with $\tau_1$, 90\% word $w_{i,j}$ are labeled with $\tau_2$
- $n_{(\tau_k,d_i)}$: number of topic $\tau_k$ in $d_i$ 
- $n_{(\tau_k,\mathcal{S})}$:  number of topic $\tau_k$ in the corpus $\mathcal{S}$ 
- $n_{(w_{i,j},\tau_k)}$: number of word $w_{i,j}$ have the topic $\tau_k$ in the corpus $\mathcal{S}$

#### 1.3.1 Params and Hyperparams:

- $\varphi, \theta, Z$ are the parameters/coefficients to be fit in the training process. (like the weight coefficients in the LR.)
- $\alpha, \beta(\eta), K$ are the hyperparameter need to be set manually. (Like the regularization strength in the LR.)
  - Other hyperparameter includes: decay

#### 1.3.2. Training Algorithm: (Collapsed) Gibbs sampling

- Randomly distribute the K topics to all words in all documents. Then we can have two quantities: 
  - $[F_{d}]_{1\times k}$: the topic distribution in document $d$
    - $\because m$ documents in the corpus, the dimension of all info will be $m\times k$
  - $[T_{w}]_{1\times k}$: the topic distribution for a word $w$ in the corpus
    - $\because N_{\mathcal{S}-uni}$ unique words in the corpus, the dimension of all info will be $N_{\mathcal{S}-uni}\times k$
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
- Iterate the step over, the topic distribution will converge finally converge.
  - Why converge? Ref: [TowardsDataScience](https://towardsdatascience.com/light-on-math-machine-learning-intuitive-guide-to-latent-dirichlet-allocation-437c81220158)

#### 1.3.3. FAQs: 

- How to assign topic according to $F_{d_i}$ and $T_{w_{i,j}}$?
    - e.g.
      - $F_{d_i} = \{\tau_1: 0.3, \tau_2:0.7 \}$: the probability of a topic in the **document** $d_i$.
      - $T_{w_{i,j}} = \{\tau_1: 0.1, \tau_2:0.9 \}$: the probability of a topic for the **word** $w_{ij}$ in the **corpus**.
      - We use their product as the probability of a topic for the **word** $w_{ij}$ in **document** $d_i$, i.e.: 
      - $T_{(w_{i,j},d_i)} =  \{\tau_1: 0.01, \tau_2:0.63 \} \underset{ \text{normalize} }{\rightarrow} \{\tau_1: \frac{0.01}{0.64}, \tau_2:\frac{0.63}{0.64} \}$ 
      - The topic should be drawn from this distribution.

#### Referece: 

- Main refs:
  - [NEU lecture](https://www.ccs.neu.edu/home/vip/teach/DMcourse/5_topicmodel_summ/notes_slides/sampling/darling-lda.pdf): good algorithm procedure representation.
  - [Mediem - blog](https://medium.com/analytics-vidhya/topic-modeling-using-lda-and-gibbs-sampling-explained-49d49b3d1045) & [Youtube Video](https://www.youtube.com/watch?v=Cpt97BpI-t4): good algorithm visual representation.
    - Blue horizontal bar: probability distribution of topic in the document $i$ (exclude the contribution of the existing topic of the current word)
    - Origin vertical bar: probability distribution of topic for the word over documents. (exclude the contribution of the current word $w_{i,j}$)
    - Grey area: the new probability distribution (not normalized) of topic of current word $w_{i,j}$ in doc $i$.
  - [TowardsDataScience-blog](https://towardsdatascience.com/lda-topic-modeling-an-explanation-e184c90aadcd): description of the training algorithm.

- Other refs:
  - [Github-blog](https://wiseodd.github.io/techblog/2017/09/07/lda-gibbs/): code for algorithm.
  - [Zhihu-blog](https://zhuanlan.zhihu.com/p/31470216): detailed explanation/summary, from background math to training.
  - [Zhihu-blog](https://zhuanlan.zhihu.com/p/32871471): chinese version of algorithm.
  
  
### 1.4. Hyperparameter tuning.

#### 1.4.0. Understanding hyperparameters? (TBD)

- $\alpha$ if the topics between documents are distinct (?)
- $\beta (\eta)$ how much words belong to similar topic, better to use the default value (?)
- Num_topic and decay is usually determined by perplexity (?)
- Decay, learning decay, percent weight kept in each iteration, used to avoid local minimum.
  
Refs:
- [Baidu-Wenku](https://wenku.baidu.com/view/4eeb6d26102de2bd970588ad.html?re=view): hyperparameter tuning.
- [Weibo](https://www.weibo.com/ttarticle/p/show?id=2309403938800962247233): ineffective link?

#### 1.4.1. Strategy? (TBD)

- Loss function: Likelihood.
    <div  align="center"><img src=https://pic1.zhimg.com/80/v2-926f3b8f62090834a2c1eef5a744ded0_1440w.jpeg style = "zoom:80%"></div>
- Strategy: MLE / MAP ?
- Unsolved Q, what is the relation between MLE and Gibbs sampling and variational inference?

Ref: 
- [Zhihu-blog](https://zhuanlan.zhihu.com/p/86838212): loss function things?
- [Toutiao-blog](https://toutiao.io/posts/tov27b/preview): chinese version summary, EM algorithm to train the model?
- [TowardsDataScience-blog](https://towardsdatascience.com/light-on-math-machine-learning-intuitive-guide-to-latent-dirichlet-allocation-437c81220158): variational inference? What is it?


#### 1.4.2. Evaluation metrics.

- Perplexity:
  - Most classical metric.
  - We want to **minimize perplexity**.
  - **Definition**: $ppl := 2^{H(P)}$, where $H(P):=-\sum_xP(x)\log P(x)$ is the entropy.
    - $ppl \downarrow H \downarrow P \uparrow$: the lower perplexity, the lower entropy, the higher likelihood, the better the model.
    - Ref: [Medium-blog](https://medium.com/analytics-vidhya/no-need-to-be-perplexed-by-perplexity-cd4cb71ac97b)
  - **Typical pattern**: $k \uparrow ppl \downarrow$
    - The more k, the complexer the model, the easier to overfit.
    - We want fewer topics (smaller k) and smaller perplexity (smaller ppl), that is how we select hyperparameters.
    - English explanation: perplexity ~ confusion/disorder/chaos. The less confused/disorder the result is, the better the model is.
- (Topic) coherence: 
  - Recent metric.
    - Due to that ppl is usually counter to human intuition. And usually $k \uparrow ppl \uparrow$ so that we cannot determine $k$ with ppl.
  - We want to **maximize coherence**.
  - Positively correlated with the distance between topics.
    - The topic is represented with words. The distance is calculated based on high weight/frequent words in the topic.
    - The larger distance among topic, the less similar, the better the topic is.
  - English explanation: coherence ~ reasonable/consistency/logical/make-sense. The more reasonable/logical the result is, the better the model is.
- Ref: 
  - [Zhihu-blog](https://www.zhihu.com/question/32286630): how to determine number of topics $k$.
  - [TowardsDataScience](https://towardsdatascience.com/evaluate-topic-model-in-python-latent-dirichlet-allocation-lda-7d57484bb5d0): Why coherence and example code.



## Model comparison

Ref: [Medium - blog](https://medium.com/nanonets/topic-modeling-with-lsa-psla-lda-and-lda2vec-555ff65b0b05)

### LDS vs LSI (TBD, 

[LSI (LSA).md](./latent_semantic_analysis.md)

- LSI (LSA) is SVD. Can be used for dimension reduction. Similarity is kept.
  - Simpler and faster than LDA but lower accuracy.
  - The matrix is value term frequency.

- PLSA is a probabilistic version of LSA. It can also be viewed as generative model. It has simpler structure than LDA. 
  - The matrix value is doc-term probability.
  - It assume each document have a certain definite topic distribution, and each topic have a definite word distribution.
    <div  align="center"><img src=https://upload.wikimedia.org/wikipedia/commons/thumb/e/ec/Plsi_1.svg/640px-Plsi_1.svg.png style = "zoom:40%"></div>
  - Looks like a transient/bridging algorithm between LSA and LDA. (personal understanding)

- LDA is a probabilistic model. It go a step further than PLSA:
  - It also assume each document have a certain definite topic distribution, and each topic have a definite word distribution.
  - But the doc's topic distribution and topic's word distribution generated from some hidden states $\boldsymbol{\theta}, \boldsymbol{\varphi}$, the hidden states is further generated from a **Dirichlet distribution** rely on given hyperparameter $\boldsymbol{\alpha}, \boldsymbol{\beta}$.
    - This design/measure can help LDA prevent overfitting and give better result compared to PLSA. Ref: [Reddit](https://www.reddit.com/r/MachineLearning/comments/10mdtf/lsa_vs_plsa_vs_lda/).
    <div  align="center"><img src=https://upload.wikimedia.org/wikipedia/commons/4/4d/Smoothed_LDA.png style = "zoom:60%"></div>

Other refs: [Quora](https://www.quora.com/Whats-the-difference-between-Latent-Semantic-Indexing-LSI-and-Latent-Dirichlet-Allocation-LDA)

## Notes:

- [Dirichlet distribution](https://en.wikipedia.org/wiki/Dirichlet_distribution): a kind of multivariate probability distribution.
