# Entropy related statistics

## 0. Notations

- $\boldsymbol{x}$: an $N$-dim vector. Can be understood as a certain item/event.
  - $\mathcal{X}$: A set of all possible $\boldsymbol{x}$ values, i.e. $\boldsymbol{x} \in \mathcal{X}$. Can be understood as the set of all available items/events.
  - $X$: the "axis"/"variable name" of $\boldsymbol{x}$. E.g. "$X=\boldsymbol{x}$" denotes the $X$-axis take value of $\boldsymbol{x}$
  - $\boldsymbol{y}$ denotes another specific item/event of $Y$ variable and $\boldsymbol{y} \in \mathcal{Y}$.
- $P(\boldsymbol{x})$, $Q(\boldsymbol{x})$: different probability distributions with $\boldsymbol{x}$ with a the same independent variable $\boldsymbol{x}$.
- $H$: entropy.
- $P$ vs $p$:
  - Capital $P$ denotes a probability distribution function (pdf), i.e., can be viewed as a "function name". E.g. $P_{(Y,X)}$ means the joint probability of variable $X$ and $Y$, $P_{(Y|\boldsymbol{x})}(\boldsymbol{y})$ denotes a function with variable $Y$, whose value is conditional probability of $\boldsymbol{y}$ given a fixed $\boldsymbol{x}$, i.e. $p(\boldsymbol{y}|\boldsymbol{x})$
  - lower $p$ is the "mark" of probability. E.g. $p(\boldsymbol{x},\boldsymbol{y})$ is the joint probability, $p(\boldsymbol{y}|\boldsymbol{x})$ is the conditional probability.

## 1. Entropy concepts

<!-- ### 1.1. Entropy, Cross entropy and Kullback–Leibler (KL) divergence -->

### 1.1. Entropy

$$H(P) := - \sum_{x\in\mathcal{X}} P(x)\log P(x)$$

- Entropy measures: the disorder, randomness, or uncertainty of a distribution. [[Wiki-Entropy](https://en.wikipedia.org/wiki/Entropy)]
  - i.e.  Expected number of bits (binary questions) needed to identify a certain item $\boldsymbol{x}$ from the set $\mathcal{X}$. [[Entropy_Guide-Chapter3](http://charlottewerndl.net/Entropy_Guide.pdf)]
- Ref: [../models/tree_models.md](../models/tree_models.md), [Wiki-Entropy](https://en.wikipedia.org/wiki/Entropy), [Entropy_Guide-Chapter3](http://charlottewerndl.net/Entropy_Guide.pdf)

### 1.2. Cross Entropy

evaluate two distributions on a same dataset.
$$H(P,Q):=-\sum _{x\in {\mathcal {X}}}P(x)\,\log Q(x)$$ 

- Cross Entropy measures: evaluate the uncertainty of using an estimated distribution $Q$ to identify events with true distribution $P$.
  - i.e. Expected number of bits needed to identify a $\boldsymbol{x}$ from $\mathcal{X}$, with the estimated distribution $Q$ rather than ground true distribution $P$.
- $P$ and $Q$ defined on the same probability space $\mathcal{X}$
- Ref: [Wiki-Cross_Entropy](https://en.wikipedia.org/wiki/Cross_entropy)

### 1.3. KL Divergence

$$\begin{aligned}
  D_{\mathrm{KL} }(P\parallel Q) &:=\sum _{x\in {\mathcal {X}}}P(x)\log \left({\frac {P(x)}{Q(x)}}\right) \\
  &= H(P,Q) - H(P)
\end{aligned}$$

- KL Divergence measures: how one probability distribution is different from a second, reference probability distribution.
  - i.e. Expected number of bits difference between using $Q$ and using $P$ to identify a event $\boldsymbol{x}$ from $\mathcal{X}$ where the ground true distribution is $P$.
- $P$ and $Q$ defined on the same probability space $\mathcal{X}$
- KL Divergence equals value "cross entropy - entropy", so it is also called **"relative entropy"** or **"information gain"**:
  - *"relative entropy of $P$ with respect to Q"*
  - *"information gain achieved if $P$ would be used instead of $Q$ which is currently used."*
- Ref: [Wiki-KL_divergence](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence), [Wiki-Cross_entropy](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence#Cross_entropy)

### 1.4. Conditional Entropy

$$H(Y|X) = -\sum_{\boldsymbol{x}\in \mathcal{\boldsymbol{x}} }\sum_{\boldsymbol{y}\in \mathcal{Y} } p(\boldsymbol{x}, \boldsymbol{y}) \log_2 p(\boldsymbol{y}|\boldsymbol{x}) $$
where: 
$$p(\boldsymbol{y}|\boldsymbol{x}) = \frac{p(\boldsymbol{x}, \boldsymbol{y})}{p(\boldsymbol{x})}$$

- Conditional Entropy describe:
  - i.e. Expected number of bits required to identify an event $(\boldsymbol{x},\boldsymbol{y})$ from event set $(\mathcal{X},\mathcal{Y})$ when $\boldsymbol{x}$ is known

### 2. Entropy based concepts

#### 2.1. Mutual information (MI)

$$\operatorname{MI}(Y;X)=D_{\mathrm {KL} }(P_{(Y,X)}\|P_{Y}\otimes P_{X})$$

Discrete representation:

$$MI (Y;X) = \sum_{x\in {\mathcal {X}}} \sum _{y\in {\mathcal {Y}}}{P_{(Y,X)}(y,x) \log {\left({\frac {P_{(Y,X)}(y,x)}{P_{Y}(y)P_{X}(x)}}\right)}}$$

Note:

- Both $P_{(Y,X)}$ and $P_{Y}\otimes P_{X}$ denotes certain probability distribution over space $\mathcal{X} \times \mathcal{Y}$, the variable is denoted with $(X,Y)$ and a specific point is denoted with $(\boldsymbol{x},\boldsymbol{y})$
  - Mapping to definition of $D_\mathrm{KL}$: $P \rightarrow P_{(Y,X)}$ and $Q \rightarrow P_{Y}\otimes P_{X}$, $\boldsymbol{x} \rightarrow (\boldsymbol{x},\boldsymbol{y})$ in 
    - $P_{(Y,X)}(\boldsymbol{y},\boldsymbol{x})$: the joint distribution of variable pair $(X,Y)$.
    - $P_{Y}\otimes P_{X}(\boldsymbol{y},\boldsymbol{x})$ the product of marginal distributions of variable $X$ and variable $Y$ ("multiplied marginal probability(?)" [[Ref:StakeExchange](https://stats.stackexchange.com/questions/495172/difference-between-multiplied-marginal-probabilities-and-joint-probability)]), i.e. 
      $$P_{Y}\otimes P_{X}(\boldsymbol{y},\boldsymbol{x}) = p_{Y}(y)p_{X}(x)$$
  - $P_{Y}:=P_{Y}(\boldsymbol{y}):=\sum_{\boldsymbol{x}\in\mathcal{X}}P_{(Y,X)}(\boldsymbol{y},\boldsymbol{x})$ is the marginal distribution of $Y$ variable.

<!-- $$MI(Y;X_i) = \sum_{x \in \mathcal{X_i}} \sum _{y\in \mathcal{Y}} p_{(Y,X_i)}(y,x) \log \left( \frac{p_{(Y,X_i)}(y,x)}{p_{Y}(y)p_{X_i}(x)} \right)$$ -->

Ref: [Wiki-Multual_information](https://en.wikipedia.org/wiki/Mutual_information) [Wiki-Marginal_distribution](https://en.wikipedia.org/wiki/Marginal_distribution)

<!-- ## Deep: KL Divergence, IG and MI -->

<!-- ### Kullback–Leibler (KL) divergence

$$D_{KL}(P\parallel Q) =\sum _{x\in {\mathcal {X}}}P(x)\log \left({\frac {P(x)}{Q(x)}}\right) $$
where:

- $P$ and $Q$ defined on the same probability space $\mathcal{X}$
- The DL divergence is also call "**relative entropy**"
- Ref: [Wiki](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence) -->

#### 2.1.1. MI and Entropy

  $$\begin{aligned}
    \operatorname{MI} (Y;X) = & \sum _{\boldsymbol{x}\in {\mathcal {X}}} \sum^{}_{\boldsymbol{y}\in\mathcal{Y} }{p(\boldsymbol{y},\boldsymbol{x})\log {\left(
      {\frac {p(\boldsymbol{y},\boldsymbol{x})}{p(\boldsymbol{y})\,p(\boldsymbol{x})}}
      \right)}}
    \\
    = & \sum_{\boldsymbol{x}\in {\mathcal {X}}}\sum_{ \boldsymbol{y}\in\mathcal{Y}}{p(\boldsymbol{y},\boldsymbol{x})\log {\left({\frac {p(\boldsymbol{y}|\boldsymbol{x})}{p(\boldsymbol{y})}}\right)}}
    \\
    = & \sum_{\boldsymbol{x}\in {\mathcal {X}}}\sum_{\boldsymbol{y}\in\mathcal{Y} } \left[
      p(\boldsymbol{y},\boldsymbol{x}) \log p(\boldsymbol{y}|\boldsymbol{x}) - p(\boldsymbol{y},\boldsymbol{x}) \log p(\boldsymbol{y})
      \right]
      \\
    = & - 
      [ -\sum_{\boldsymbol{y}\in\mathcal{Y} }\sum_{\boldsymbol{x}\in {\mathcal {X}}}p(\boldsymbol{y},\boldsymbol{x}) \log p(\boldsymbol{y}|\boldsymbol{x}) ] +
      \sum_{\boldsymbol{y}\in\mathcal{Y} } - \log p(\boldsymbol{y})
      \sum_{\boldsymbol{x}\in {\mathcal {X}}} p(\boldsymbol{y},\boldsymbol{x}) 
      \\
    = & -H(Y|X) + 
      \sum_{\boldsymbol{y}\in\mathcal{Y} } - p(\boldsymbol{y})\log p(\boldsymbol{y})
      \\
    = & H(Y) - H(Y|X_i)   
  \end{aligned}$$
  - Ref: [StakeExchange](https://stats.stackexchange.com/questions/103175/information-gain-is-kl-divergence)


#### 2.1.2. MI in application

##### 2.1.2.1. Concept differentiation: "Information Gain" in decision tree

In the decision tree, the "information gain" metric used to select "feature to split" is actually the mutual information.

However, in some books/papaers/websites, IG is defined as: 
<!-- $$IG_{Y,X}{(Y,X_i)}=D_{\text{KL}}{\left(p{(Y|X_i)}\|p{(Y)}\right)}$$ -->
  $$\begin{aligned}
    \operatorname{IG}_{Y;X}{(\boldsymbol{x})}&=D_{\text{KL}}{\left(P_{(Y|\boldsymbol{x})}{(\boldsymbol{y})}\|P_Y{(\boldsymbol{y})}\right)}
    \\
    &=\sum^{}_{\boldsymbol{y}\in\mathcal{Y}} p(\boldsymbol{y}|\boldsymbol{x})\log \left({\frac { p(\boldsymbol{y}|\boldsymbol{x}) }{ p(\boldsymbol{y}) }}\right)
  \end{aligned}$$

Note: $P_{(Y|\boldsymbol{x})}{(\boldsymbol{y})}:=p(\boldsymbol{y}|\boldsymbol{x})$ i.e. the value of the function exactly equals to conditional probability $p(\boldsymbol{y}|\boldsymbol{x})$. The point of using $P_{(Y|\boldsymbol{x})}{(\boldsymbol{y})}$ is we should view it as a function of variable $Y$, dependent on the parameter $\boldsymbol{x}$.

Actually, such IG can be called "Information gain (IG) for specific feature value $X=\boldsymbol{x}$". And when selecting feature, we should take the expectation of such IG over all $\boldsymbol{x}\in\mathcal{X}$, and "the expectation of such IG" is exactly the MI: 

  $$\begin{aligned}
    \operatorname{MI} (Y;X) = & \sum _{\boldsymbol{x}\in {\mathcal {X}}} \sum^{}_{\boldsymbol{y}\in\mathcal{Y} }{p(\boldsymbol{y},\boldsymbol{x})\log {\left(
      {\frac {p(\boldsymbol{y},\boldsymbol{x})}{p(\boldsymbol{y})\,p(\boldsymbol{x})}}
      \right)}}
    \\
    = & \sum _{\boldsymbol{x}\in {\mathcal {X}}} \left( 
      \sum^{}_{\boldsymbol{y}\in\mathcal{Y} }{p(\boldsymbol{y}|\boldsymbol{x})p(\boldsymbol{x})\log {\left({\frac {p(\boldsymbol{y}|\boldsymbol{x})}{p(\boldsymbol{y})}}\right)}}
      \right)
    \\
    = & \sum _{\boldsymbol{x}\in {\mathcal {X}}} p(\boldsymbol{x}) \left( 
      \sum^{}_{\boldsymbol{y}\in\mathcal{Y} }{p(\boldsymbol{y}|\boldsymbol{x})\log {\left({\frac {p(\boldsymbol{y}|\boldsymbol{x})}{p(\boldsymbol{y})}}\right)}}
      \right)
    \\
    = & \sum _{\boldsymbol{x}\in {\mathcal {X}}} p(\boldsymbol{x}) 
      D_{KL}(p(\boldsymbol{y}|\boldsymbol{x}) \| p(\boldsymbol{y}))
    \\
    = & \sum _{\boldsymbol{x}\in {\mathcal {X}}} p(\boldsymbol{x}) IG_{Y;X}(\boldsymbol{x})
    \\
    = &  \underset{\boldsymbol{x} \in \mathcal{X}}{\mathbb{E}}[ IG_{Y;X}(\boldsymbol{x}) ]
  \end{aligned}$$
  - Ref: [StakeExchange](https://stats.stackexchange.com/questions/103175/information-gain-is-kl-divergence)


I.e. The "information gain" concept in the tree model can be understood as:

-  We can have "IG" in two levels:
   -  IG of label variable $Y$ w.r.t. specific feature value $\boldsymbol{x}$
   -  IG of label variable $Y$ w.r.t. feature value $X$, i.e. the MI
-  When we want to identify the feature which we want to split, we use the IG of label variable $Y$ w.r.t. feature value $X$ (i.e MI)
-  To get the MI:
   -  First, calculate IG for w.r.t each specific feature $\boldsymbol{x}$
   -  Then, take the MI as the weighted average of such IG.



##### 2.1.2.1. Application Example: MI Calculation process for decision tree

Usually, the MI is calculated from Entropy and Conditional Entropy.

Assume We have two labels (0,1), and 1 input feature $x\in [-1, 1]$:

- Before the treestump split, the entropy is calculated by
  $$\begin{aligned}
    H(Y) = - [& p(y=0)\log p(y=0) \\
    &+ p(y=1)\log p(y=1) ]
  \end{aligned}$$

- After treestump, let's say the threshold for $x$ is 0, the conditional entropy is calculated by:
  $$ \begin{aligned}
    H(Y|X) = - [& p(x<0,y=0)\log p(y=0|x<0) \\
    &+ p(x>0,y=0)\log p(y=0|x>0) \\
    &+ p(x<0,y=1)\log p(y=1|x<0) \\
    &+ p(x>0,y=1)\log p(y=1|x>0) ] 
  \end{aligned}$$
- Then, the MI of label $Y$ w.r.t. feature $X$
  $$MI(Y;X) = H(Y)-H(Y|X)$$

## 3. Entropy related concepts

### 3.1. Negative Log-Likelihood (NLL)

- Negative log likelihood:
  $$NLL(p(\mathcal{S})) = -\log(p(\mathcal{S})) $$
  - Likelihood is discussed in the perspective of likelihood of (appearance of) dataset $\mathcal{S}$
  - Ref: [Wiki-MLE](https://en.wikipedia.org/wiki/Maximum_likelihood_estimation)

#### 3.1.1 Cross Entropy and NNL in logistic regression.

Typically, cross entropy and NNL are not comparable as they are conceptually different. (CE consider two distribution; NLL consider the probability of a dataset.)

But <mark style="background-color:yellow;">in the case of Logistic regression</mark>, the <mark style="background-color:yellow;">"NNL loss"</mark> is <mark style="background-color:yellow;">equivalent</mark> to the <mark style="background-color:yellow;">"cross entropy loss"</mark>.

- Definition:
  - "NNL loss": NNL of the whole dataset
  - "cross entropy loss": average of sample's cross entropy between true label distribution and predicted label distribution

**Proof:**
- Notation:
  - $\mathcal{S}$ denotes the training set with $M$ samples $\{...s_i...\}$
    - $s_i=(\boldsymbol{x}_i,y_i)$ is the (feature,label) of the i-th sample
    - the label is binary, i.e., $y\in \{0,1\}$
  - $\theta$ is an $(N+1)$-dim vector contains the "weights" and the "intercept" of the features.
  - $v$: an independent binary variable: $v \in \{0,1\}$ 
- Likelihood of the dataset given parameter $\theta$ is:
  $$p(\mathcal{S}|\theta) = \prod^M_{i=1} p(\hat{y} = y_i|\boldsymbol{x}_i)$$
- Then:
  $$\begin{aligned}
      NLL(\mathcal{S}|\theta) & = - \sum^M_{i=1} \log[ p(\hat{y} = y_i|\boldsymbol{x}_i) ]\\
      & = - \sum^M_{i=1} \log[ p(\hat{y} = y_i|s_i) ] \\
      & \underset{LR}{=} - \sum^M_{i=1} H_i(P_i^*(v), \hat{P}_i(v))\\
      & = M\cdot J(\theta)
  \end{aligned}$$
  - Note: the core fact here is, 
    - for individual sample, the NLL equals to the cross entropy
    - this fact holds when the label is binary $\{0,1\}$, not generally holds.
  - Minimize NLL = MLE
  - Thus, also, **in LR**, the training strategy "minimize the average cross-entropy loss" can also be viewed as MLE

Backup refs: [StackExchange](https://stats.stackexchange.com/questions/198038/cross-entropy-or-log-likelihood-in-output-layer), [Quora](https://www.quora.com/What-are-the-differences-between-maximum-likelihood-and-cross-entropy-as-a-loss-function)