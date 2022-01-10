# Collaborative filtering (CF)

## 0. Task description

With CF RecSys, our task is usually to predict a user's preference based other people's preference.

Ref: [Textbook: Recommender Systems: An Introduction](https://www.google.com/books/edition/Recommender_Systems/eygTJBd_U2cC?hl=en&gbpv=0)


### 0.0. Notations

- $\boldsymbol{X}$, $\boldsymbol{X}_{M \times N}$: a rating matrix for $M$ users and $N$ items.
  - $r_{ij}:=X_{ij}$: the rating of i-th user on j-th item.
  - $\boldsymbol{u}_i$:  A column vector of the i-th user's ratings on all items. I.e. the (transpose of) i-th row in the matrix.
  - $\boldsymbol{v}_j$:  A column vector of the j-th item's ratings from all users. I.e. the j-th column in the matrix.

### 0.1. Mathematical abstraction

The Recommendation system problem can be abstracted as:

We have the data of different users' rating on different items, denoted as matrix $\boldsymbol{X}_{M \times N}$.

The rating of i-th user on j-th item, i.e. $r_{ij}:=X_{ij}$, is missing, how to predict it?


<div  align="center">
  <img src=./collaborative_filtering_asset/recsys_problem.png style = "zoom:25%">
</div>

## 1. Basics

The way of collaborative filtering is to estimate the missing rating purely based on other users' rating on other items, i.e. purely rely on user behavior, regardless of other features like user properties or item properties.

The central idea of collaborative filtering algorithm is the more similar between two users, the more similar rating they should have on same item (user-based perspective). 

(Or, in item-based perspective, the more similar between two items, the more possible they receive similar rating from same user.)

Mathematically, the missing value is estimated as a weighted average of value in same column (or row), the similarity between rows (or columns) is the weight.

### 1.1. User-based CF

User-based CF estimates the missing value from the perspective of user similarity, i.e. similar user will give similar rating. Thus, the missing rating is a user-similarity weighted average of other users' rating on the item. I.e.:

$$r_{mn} = \frac{\sum^M_{i=1} \mathrm{sim}(\boldsymbol{u}_m,\boldsymbol{u}_i)r_{in}}{\sum^M_{i=1} \mathrm{sim}(\boldsymbol{u}_m,\boldsymbol{u}_i)}$$

<div  align="center">
  <img src=./collaborative_filtering_asset/collaborative_filtering_user_based.png style = "zoom:25%">
</div>


### 1.2. Item-based CF

Item-based CF estimates the missing value from the perspective of item similarity, i.e. similar item will receive similar rating. Thus, the missing rating is a item-similarity weighted average of other items' rating from the user. I.e.:

$$r_{mn} = \frac{\sum^N_{j=1} \mathrm{sim}(\boldsymbol{v}_n,\boldsymbol{v}_j)r_{mj}}{\sum^N_{j=1} \mathrm{sim}(\boldsymbol{v}_n,\boldsymbol{v}_j)}$$

<div  align="center">
  <img src=./collaborative_filtering_asset/collaborative_filtering_item_based.png style = "zoom:25%">
</div>


### 1.3. Application scenario

User-based: 

- user is less than item
  - the user-user similarity are less than item-item similarity
  - less memory consumption
- user often change, item seldom change.
  - When adding new users, we just need to calculate similarity for the new users. The similarity between existing users not change.
  - When adding new item, all the user-user similarities need to be recalculated.
  - Less computational resources, more stable dataset.


Item based: 

- item is less than user.
  - the item-item similarity are less than user-user similarity
  - less memory consumption
- item often change, user seldom change.
  - When adding new users, all the item-item similarities need to be recalculated.
  - When adding new items, we just need to calculate similarity for the new items. The similarity between existing items not change.
  - Less computational resources, more stable dataset.

Ref: [Zhihu](https://zhuanlan.zhihu.com/p/54824332)

### 1.4. Drawback of CF

Computational & storage complexity.

We need to compute and remember $O(N^2)$ user-user or $O(M^2)$ item-item similarities.
Ref: [Zhihu](https://zhuanlan.zhihu.com/p/54824332)

## 2. Technical details

### 2.1. Missing values: 0-filling

Q: Usually, we will have missing values in the matrix, how to handle that?

A: **Ignore the missing value** when calculating the similarity. (I.e. skip the row/col position of the missing item; **I.e. mathematically, assign 0 to the missing values** while computing.)

For example, in the following table, in user based case, when considering similarity between "Alice" and "User3", we only consider the similarity on "Item1" to "Item4", ignore the missing column "Item5"


<div  align="center">
  <img src=./collaborative_filtering_asset/collaborative_filtering_user_based_ignore_missing.png style = "zoom:25%">
</div>

Or, in item based case, when considering similarity between "Item2" and "Item5", we only consider the similarity on "User1" to "User4", ignore the missing row "Alice".

<div  align="center">
  <img src=./collaborative_filtering_asset/collaborative_filtering_item_based_ignore_missing.png style = "zoom:25%">
</div>

**Note:** 0-filling are used only when directly calculate similarity and weighted average from matrix with missing values. The fundamental purpose is to ignore the contribution from missing values. 0-filling is mathematically equivalent to ignore those value, and more convenient.

However, if there is a matrix factorization before similarity calculation, **don't fill 0 before/when doing matrix factorization**. This is because when doing matrix factorization, the way the algo handle the missing value and 0 are different, missing values will be skipped/ignored while 0 value will be considered.

Ref: [TowardsDataScience](https://towardsdatascience.com/recommender-systems-in-practice-cef9033bb23a)


### 2.2. User baseline: mean subtraction

Different users usually have different "baseline", like, with 5 stars as max:
- my standard is looser: average I give 4, good I give 5, bad I give 3; 
- my friends' is stricter: average he give 2, good he give 3, bad he give 1.

Thus, subtract mean may be a good way to eliminate the baseline.

Then, the user-based CF becomes:

$$r'_{mn} = \frac{\sum^M_{i=1} \mathrm{sim}(\boldsymbol{u'}_m,\boldsymbol{u'}_i)r'_{in}}{\sum^M_{i=1} \mathrm{sim}(\boldsymbol{u'}_m,\boldsymbol{u'}_i)}$$

$$r_{mn} = r'_{mn} + \text{avg}(\boldsymbol{u}_m)$$

where:
- $r'_{ij} := r_{ij} - \text{avg}(\boldsymbol{u}_i)$: the modified rating, i.e. the original rating subtract the user's average rating.
  - I.e. we construct a new $\boldsymbol{X'}_{M \times N}$ with the row-mean-subtracted rating.
- $\boldsymbol{u'}_i$ the user vector with the modified rating.
  - I.e. $\boldsymbol{u'}_i = \boldsymbol{u}_i - \text{avg}(\boldsymbol{u}_i)$: the user's rating vector after subtracting his mean rating on all items.
  - $vector - scaler$ means every value in the vector will $- scaler$


Note:

- Don't forget the add back the user baseline to $r'_{mn}$. I.e. $r_{mn}$ is the rating we expect to observe, not $r'_{mn}$.
- Typically, we only apply mean subtraction to user-based CF. Because user are subjective, item usually don't have subjective baseline. This is a intuition/practical-habit. Not math things.
- We can also do more complex modifications accordingly, like normalization, i.e. scale it with standardized deviation, depending on your preference, mean subtraction is just an example.


### 2.3. Other typical formula modifications

- To avoid noise from non-similar samples, rather than taking the weighted average of all ratings, we can take the weighted average of **most similar samples**.
- To focus on most recent records, we can add **time decay** to similarity, i.e. the longer time interval between two items, the less weight/similarity we assign between them.
- To focus on nearest users, we can add **location-distance decay** to similarity, i.e. the farther distance between the users, the less weight/similarity we assign between them.
- To eliminate strong-background/noise from too general/common items, we can use **tfidf** to reduce the weight of those very common items.

Ref: [Zhihu](https://zhuanlan.zhihu.com/p/54824332), [Textbook: Recommender Systems: An Introduction](https://www.google.com/books/edition/Recommender_Systems/eygTJBd_U2cC?hl=en&gbpv=0)

### 2.4. Dimension reduction: matrix factorization

Ref: [matrix_factorization.md](../general_machine_learning/math_topics/matrix_factorization.md)

The core quantity here is the similarity.

If the dimension is too high, matrix factorization can be used to reduce the computation complexity to calculate similarity. 

Example:

NMF:

  $$\mathbf{X}_{m \times n} \approx \mathbf{W}_{m \times k} \mathbf{H}_{k \times n}$$

  - $\mathbf{W}_{m \times k} $ will keep the row similarity, can be used for user-based CF.
  - $\mathbf{H}_{m \times k} $ will keep the column similarity, can be used for item-based CF.

SVD:
  $$\mathbf{X}_{m \times n} \approx \mathbf{U}_{m \times k} \mathbf{\Sigma}_{k \times k} (\mathbf{V}_{n \times k})^{T}$$

  - $(\mathbf{X_{V}})_{m\times k} = \mathbf{X}\mathbf{V} =\mathbf{U} \mathbf{\Sigma} $ is the coordinate value of $\mathbf{X}$ in new coordinate system $\mathbf{V}$
    - It keeps user similarity, can be used for user based CF.


Note: the rating score part in the function, like $r_{in}$, is still fetched from the original matrix. The factorized matrix is just used to simplify the similarity part, like $\mathrm{sim}(\boldsymbol{u}_m,\boldsymbol{u}_i)$.




