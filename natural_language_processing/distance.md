# Metric

## Euclidean Distance vs Cosine Distance

### Basics

- Euclidean Distance:
  $$D_{euc}(\vec{\mu}, \vec{\nu}) = \sqrt{\sum_i(\mu_i - \nu_i)^2}$$
- Cosine Distance
  $$D_{cos}(\vec{u}, \vec{v}) = 1-\frac{\sum_i \mu_i \nu_i}{\sqrt{\sum_i \mu_i^2}\sqrt{\sum_i \nu_i^2}} $$

### Relation
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

## Deep Dive: why NLP prefer $D_{cos}(\vec{u}, \vec{v})$

- NLP cares more about the ratio between different components (features)
  - i.e. we have a 2-dim space, $x$ represent 'CS', $y$ represent 'Biz'
    - $\vec{\mu} = CS + Biz$
    - $\vec{\nu} = 2\cdot CS + 2\cdot Biz$
    - In NLP, we prefer two think $\vec{\mu}$, $\vec{\nu}$ are very similar. I.e. we care about ratio between different components rather than their length.
- In NLP, the embeddings are usually not normalized, although squared Euclidean distance after normalization is same with cosine distance, it is more straightforward to cosine distance.

## Deep Dive: negative cosine similarity

- Q: What does negative cosine similarity means?
  - A: It means opposite property. E.g. Two user with -1 cosine similarity, it means they have opposite taste/preference.
  - Suggestion: Two avoid this confusion, it is better to use $\text{cosine\_distance} = 1 - \text{cosine\_similarity}$, which ranges from [0,2], the higher the more different.