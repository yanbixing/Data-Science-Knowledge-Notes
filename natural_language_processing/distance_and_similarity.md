# Distance and similarity

## 1. Euclidean Distance vs Cosine Distance

Note:
- **Vector-based**: Both euclidean distance and cosine distance are used on **vectors**

### 1.1 Basics

- Euclidean Distance:
  $$D_{euc}(\vec{\mu}, \vec{\nu}) = \sqrt{\sum_i(\mu_i - \nu_i)^2}$$
- Cosine Distance
  $$D_{cos}(\vec{u}, \vec{v}) = 1-\frac{\sum_i \mu_i \nu_i}{\sqrt{\sum_i \mu_i^2}\sqrt{\sum_i \nu_i^2}} $$

### 1.2 Relation between $D_{euc}$ and $D_{cos}$

**Conclusion:** 

- Diff
  - Cosine distance only consider the **angle** between two vector.
  - Euclidean distance consider both **angle** and **length (l2-norm)** of the two vectors.
- For **l2-normalized** vectors
  - **Squared** Euclidean distance is **equivalent** to the cosine distance.($D^2_{euc}(\vec{\mu}, \vec{\nu}) = 2 \times D_{cos}(\vec{u}, \vec{v})$)

Deduction:

- Cos Dist:
$$\begin{aligned}
    D_{cos}(\vec{u}, \vec{v}) &= 1- \frac{\mu^T\nu}{\sqrt{\mu^T\mu}\sqrt {\nu^T\nu}}\\
    & = 1 - \cos(\vec{\mu},\vec{\nu})
\end{aligned}$$
- Euc Dist:
$$\begin{aligned}
    D^2_{euc}(\vec{\mu}, \vec{\nu}) & = (\mu-\nu)^T(\mu-\nu)\\
    & = \mu^T\mu + \nu^T\nu - 2\mu^T\nu  \leftarrow \mu^T\nu = \nu^T\mu \\
    & = |\vec{\mu}|^2+|\vec{\nu}|^2 - 2 \cos(\vec{\mu},\vec{\nu})|\vec{\mu}||\vec{\nu}|\\
    & = \left[|\vec{\mu}|^2+|\vec{\nu}|^2 - 2|\vec{\mu}||\vec{\nu}|\right] + 2|\vec{\mu}||\vec{\nu}| (1-\cos(\vec{\mu}, \vec{\nu}))\\
    & = (|\mu| - |\nu|)^2 + 2|\vec{\mu}||\vec{\nu}| \cdot D_{cos}(\vec{u}, \vec{v})
\end{aligned}$$

- When vectors are normalized, i.e. $|\vec{\mu}| = |\vec{\nu}| = 1$
  - $D^2_{euc}(\vec{\mu}, \vec{\nu}) = 2(1-\cos(\vec{\mu},\vec{\nu})) = 2D_{cos}(\vec{u}, \vec{v})$

Ref: [StakeExchange](https://stats.stackexchange.com/questions/146221/is-cosine-similarity-identical-to-l2-normalized-euclidean-distance)

### 1.3 Deep Dive: why NLP prefer $D_{cos}(\vec{u}, \vec{v})$

- NLP cares more about the **ratio** between different components (features). Or in other words, NLP care about **composition** rather than scaling.
  - i.e. we have a 2-dim space, $x$ represent 'CS', $y$ represent 'Biz'
    - $\vec{\mu} = CS + Biz$
    - $\vec{\nu} = 2\cdot CS + 2\cdot Biz$
    - In NLP, we prefer two think $\vec{\mu}$, $\vec{\nu}$ are very similar. I.e. we care about ratio between different components rather than their length.
- For most NLP embedding techniques, the output vector are usually not normalized.

Thus, $D_{cos}$ is the most **straightforward** (only consider angle, don't consider scale) as well as **computationally cheap** choice, i.e.:

- If you use $D_{euc}$, you need two steps: normalization and calculation.
- If you use $D_{cos}$, you just need one step: just calculate, no need to normalize.

### 1.4 Deep Dive: understand negative cosine similarity

- Q: What does negative cosine similarity means?
  - A: It means opposite property. E.g. Two user with -1 cosine similarity, it means they have opposite taste/preference.
  - Suggestion: Two avoid this confusion, it is better to use ${\text{cosine\_distance}} = 1 - {\text{cosine\_similarity}}$, which ranges from [0,2], the higher the more different.
- Ref: [StakeExchange]( https://stats.stackexchange.com/questions/198810/interpreting-negative-cosine-similarity)


## 2. Jaccard similarity

Note:

- **Set-based**: Jaccard similarity is used on **sets**

### 2.1 Basics

$$J(A,B) = {{|A \cap B|}\over{|A \cup B|}} = {{|A \cap B|}\over{|A| + |B| - |A \cap B|}}$$

<div  align="center">
  <img src=https://upload.wikimedia.org/wikipedia/commons/thumb/1/1f/Intersection_of_sets_A_and_B.svg/371px-Intersection_of_sets_A_and_B.svg.png style = "zoom:40%">
  <img src=https://upload.wikimedia.org/wikipedia/commons/thumb/e/ee/Union_of_sets_A_and_B.svg/371px-Union_of_sets_A_and_B.svg.png style = "zoom:40%">
</div>

Ref: [Wiki](https://en.wikipedia.org/wiki/Jaccard_index)

### 2.2 Comparison

Compared to $D_{euc}$/$D_{cos}$, Jaccard similarity:

- Applied on set, thus don't require vector representation.
- Binary value, i.e. don't care about ratio between different composition.

## 3. Edit distance

Note:

- **String**: Edit distance is used on **strings**.

### 2.1. Basics

- Definition: Edit distance = The **number** of **operations** that can change **String A** to **String B**. 
- There are different kind of "edit distance" based on different sets of "**operation**" and their weight on "**number**". Ref: [Quora](https://www.quora.com/What-is-the-difference-between-Levenshtein-Distance-Algorithm-and-Minimum-Edit-Distance-Algorithm-in-string-processing)

### 2.2. Levenshtein distance

Most common form of edit distance:

- Operation includes:
  - remove
  - insert
  - substitute
- All operations have **equal weight**: 1

Ref: [Wiki - Levenshtein distance](https://en.wikipedia.org/wiki/Levenshtein_distance)


## 4. Doc similarity

- Embedding doc and calculate similarity
- Statistics (mean, mean(max())) on word-word similarity
- Ref?


## Deep Dive 1.: Other common distance

Asked in interview (L) Ref: []

### DD1.1. L1 distance

Ref:[TowardsDataScience](https://towardsdatascience.com/3-distances-that-every-data-scientist-should-know-59d864e5030a)

$$D_{L1}(\boldsymbol{u},\boldsymbol{v}) = \sum_i |\boldsymbol{u}^{(i)} = \boldsymbol{v}^{(i)}|  $$

Geometric understanding, L1 measure the shortest distance between two points if we are only able to move along axis direction.


<div  align="center"><img src=https://miro.medium.com/max/1400/1*88uZae0Utf7kavhQFvMqaw.png style = "zoom:40%"></div>


Note: L2 distance is just the Euclidean Distance $D_{L1}(\boldsymbol{u},\boldsymbol{v}):=D_{euc}(\vec{\mu}, \vec{\nu}) = \sqrt{\sum_i(\mu_i - \nu_i)^2}$, the size of a vector or the shorted distance between two points.


<div  align="center"><img src=https://miro.medium.com/max/1524/1*J2bK-UKhrW1Ill5EyAxXOQ.png style = "zoom:40%"></div>


### DD1.2. Standardized Euclidean distance (Mahalanobis distance)



Ref: [Wiki-Mahalanobis_distance](Wiki-https://en.wikipedia.org/wiki/Mahalanobis_distance)
Note: Mahalanobis is the inventor of the standardized Euclidean distance.

$$ D_{s-euc}({\vec {x}},{\vec {y}}):={\sqrt {({\vec {x}}-{\vec {y}})^{\mathsf {T}}\mathbf {S} ^{-1}({\vec {x}}-{\vec {y}})}}={\sqrt {\sum _{i=1}^{N}{\frac {(x_{i}-y_{i})^{2}}{s_{i}^{2}}}}}$$

where $s_i$ is the standard deviation of the dataset.

Standardized Euclidean distance is the Euclidean distance when the standard deviation on each direction is normalized to 1. 

**Application Example:** Note that, when define z score, we use 

$$Z = \frac{ | \mu^{obs} - \mu^{hypo} |}{SE(\mu)} = \frac{ | \bar{X} - \mu^{hypo} |}{ \frac{SD(X)}{\sqrt{N}} }$$

It mean how many standardized deviation is there between $\mu^{obs} \to \mu^{hypo}$

This is just the 1D Standardized Euclidean distance 

