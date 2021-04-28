# Tree model

## 0. Basic concepts


### Number of split points (Complexity of finding split point?): 

- Brute force, N data points then there is N-1 split points, check each of them one by one.
- This complexity is usually used for regression tree.

Multiclass-classification: each leaf nodes is a class. (majority voting)

### Loss function: 

- Classification: entropy, Gini
- Regression: square loss.

### Difference between "bagging", "random forest" and "boosting":

- Bagging: Use different **bootstrapped** training sets to train multiple different trees, and take the average / majority of their predictions.
  - Bootstrap sampling: random sample a subset of training samples with replacement.
    - A kind of "data augmentation" and can increase model **independence** thus **reduce model variance**
    - Ref:[Wiki](https://en.wikipedia.org/wiki/Bootstrapping_(statistics)), [Blog](https://nititek.wordpress.com/2013/12/10/bootstrapping/)
- Random forest: Use bootstrapped samples and different **subset of features** to train different trees.
  - i.e. Subset of samples (Bagged trees) + subset of features.
  - Further decorrelate different trees and reduce variance.
- Boosting: sequentially train multiple weak learners (tree-stump) based on previous residue, also take the weighted sum (?) / majority of all tree-stumps. 
- Stacking: use different types of model (rather than data) to get a weighted decision.
- Refs: [Cornell U](http://www.cs.cornell.edu/courses/cs578/2005fa/CS578.bagging.boosting.lecture.pdf), [Quora](https://www.quora.com/What-does-Bagging-reduces-the-variance-while-retaining-the-bias-mean), [Linkdin](https://www.linkedin.com/pulse/bias-variance-bagging-boosting-dummies-i-sray-agarwal/)

### Entropy:

#### Notation:

- $\mathcal{S}$ is a sampleset with $m$ sample $(x,y)$ = $(x,Y)$
  - $y$ denotes the label of the sample, there assume it is 1-dim label $Y$.
  - $p(C_k):=p(Y=C_k)$ denotes the probability of k-th class.
- $X_i$ denotes the $i$-th feature in the input feature vector $x$
  - $\mathcal{X_i}$ denotes all possible values for $X_i$
  - $p(x_i)$ is an abbreviation of $p(X=C_k)$

#### Entroy
$$H(\mathcal{S}):= H(Y) = \sum^K_{k=1} -p(C_k)\log_2 p(C_k)$$

**Note:** Entropy is maximized when each class have an equal probability (i.e. $\frac{1}{c}$) and minimized when all samples fall into a single category.

**Note:** There is a negative sign **"-"** in the equation.

#### Conditional Entropy:

$$H(Y|X_i) = -\sum_{X_i\in \mathcal{X_i} }\sum_{Y\in \mathcal{Y} } p(X_i, Y) \log_2 p(Y|X_i) $$
where:
$$p(Y|X_i) = \frac{p(X_i, Y)}{p(X_i)}$$


#### "Information gain" 

Actually, the typical term "information gain" used in decision tree cases is the expectation of information gain, i.e. mutual information:

$$\underset{X_i \in \mathcal{X_i}}{\mathbb{E}}[ IG(Y,X_i) ] = MI(Y;X_i) = E(Y) - E(Y|X_i)$$

## 1. Decision Tree:

### 1.1. How to grow (Training method):

Maximize the information gain.

- Greedy: Split samples on the features that can provide largest information gain.
  - For continuos feature: try at each points' interval. (N points have N-1 interval.)

### 1.2. Bias-variance tradeoff

Bias-variance tradeoff is dependent on depth of the tree.

- Fully grown tree: high variance, low bias, overfitting.
- Tree stump: high bias, low variance, underfitting.

### 1.3. Parameters to control bias-variance tradeoff

- Depth of tree: layers of splits 
  - The deeper, the higher model variance
- Min split size: below which, the node is not allowed to be split
  - The smaller, the more splits (the deeper), the higher model variance
- Min leaf size: the node's children below which, the node is not allowed to be split
  - The smaller, the more splits, the higher model variance
- Min information gain: below which, the node is not allowed to be split
  - The smaller, the more splits, the higher model variance

### 1.4. Pro and cons

- Pro:
  - Easy to interpret (verbalizable)
  - Easy to implement (can be realized with if-else)
  - Can handle both numerical and categorical data
  - Performs well on large dataset.
    - (?) Why?
  - Extremely fast
    - Complexity of prediction = depth of the tree
  - No need feature engineering
    - Can handle categorical/text features as is  (?)
    - Automatically detect non-linearities and interactions (?)
- Cons:
  - Prone to overfitting
    - Depth
  - Not global optimum
    - The algorithm is greedy, information gain is maximized locally not globally.
  - Unstable
    - Sensitive to where it/how the nodes are split. Small changes in variable and sample may result in different tree structure.
  - Not good at imbalanced data.
    - ?

## 2. Bagging and Random Forest

### 2.1. Bagging: bootstrap aggregating

#### 2.1.1. Training: 

Train multiple trees on different bootstrap sample set

- Thus, the process is parallelizable.
- Bootstrap sample: sample set drawn **with replacement** from training set.

#### 2.1.2. Prediction:

- Regression: average
- Classification: majority vote

#### 2.1.3. Bias variance trade-off:

- Compare to plain decision tree model:
  - Bagging can Reduce variance: starting from overfitting trees (high variance and low bias), reduce variance with model ensemble (average/majority from multiple models).
  - Bagging CANNOT reduce bias
    - Bias is minimized by increasing depth for each tree.


### 2.2 Random Forest

An upgraded version of bagging tree.

#### 2.1.1. Training: 

- Train multiple **fully-grown** trees 
- Tree are trained on different bootstrap sample set
- Tree are trained on different **subset of features**

#### 2.1.1. Prediction

Same with bagging.

#### 2.1.1. Bias-Variance tradeoff

- Compare to plain decision tree model:
  - Starting from low bias high variance individual tree, reduce the variance in the ensemble process.

- Compare to bagging trees model:
- For individual tree:
  - Subset of feature: result in high bias
  - Fully grown: reduce the bias, counter "using subset of features"
- In the ensemble process:
  - Bootstrap sample: de-correlate different tree, reduce variance
  - Subset of feature: de-correlate different tree, reduce variance

Compare to bagging tree:

- Further reduce variance.
- NOT reduce bias.

Compare to plain decision tree model:

- Reduce variance.
- NOT reduce bias.


#### 2.1.4. Bagging v.s. RF

- Similarity: both models trains multiple trees on bootstrap samples and reduce variance via model ensemble.
- Difference: 
  - RF randomly select a subset of the features for each tree, further de-correlate different trees, which could further decrease the variance.
  - RF typically use fully grown tree while

Ref: [StackExchange](https://stats.stackexchange.com/questions/264129/what-is-the-difference-between-bagging-and-random-forest-if-only-one-explanatory)


## 3. Boosting tree

### 3.0. General description

In each iteration, a new learner is added. (existing learners are kept unchanged.) 

All learners are weighted based on their performance (e.g., accuracy) After a weak learner is added: misclassified samples gain more weights, while examples that are correctly classified lose weights. Thus, future weak learners focus more on examples that previous weak learners misclassified.)
Ref: [Github](https://github.com/ShuaiW/data-science-question-answer#boosting-tree)

The k-th tree is build using information learned from all previous k−1k−1 trees. Ref [StackExchange]( https://datascience.stackexchange.com/questions/9735/what-is-the-difference-between-fully-developed-decision-trees-and-shallow-dec)

### 3.0 Boosting Basics

#### 3.0.1 What is "Boosting"

 Combines weak "learners" into a single strong learner in an **iterative** fashion. [Wiki - Gradient Boost](https://en.wikipedia.org/wiki/Gradient_boosting)

#### 3.0.2 What is a "Weak Learner"

The weak leaner used in boost is usually **tree stump** or shallow tree.

- Q: Why called it weak?
  - The accuracy (predicting power) is very low. Ref: [StakeExchange](https://stats.stackexchange.com/questions/82049/what-is-meant-by-weak-learner)
- Variance-bias tradeoff: high bias, low variance

#### 3.0.3 Classification Tree vs Regression Tree

- Regression tree: next stump fit the the residue (GB directly on residue, AdaBoost reweighing based on residue).
- Classification tree: misclassified samples are given more weight (Ada directly reweighing, GB focus on classification.)

#### 3.0.4  Bias-variance tradeoff

- Starting from weak classifier (under-fitting, high bias, low variance)
  - <font color="#0000dd"> Mainly reduce bias (by focusing on residue) </font>.
  - Also reduce variance since there is an ensemble process.

**Note:** Fitting reside = increase model complexity.

Ref: [Quora](https://www.quora.com/What-effect-does-boosting-have-on-bias-and-variance)

### 3.1. AdaBoost (Adaptive Boost)

Summary: The output of the other learning algorithms ('weak learners') is combined into a weighted sum that represents the final output of the boosted classifier.

Train by **reweighing** sample weights. (The most basic boost, the training process above.) 

#### 3.1.0. Prediction function

$$F_T(x) = \sum_{t=1}^T f_t(x)$$
where:

- $F_T(x)$ is the output of the model, there are $T$ weak learners in total. 
- $f_t = \alpha_t h_t(x)$ is the weighted t-th weak learner. $\alpha_t$ is the weight and $h_t(x)$ is the hypothesis.

#### 3.1.1.Training process

0. In the $t$-th step, (after $(t-1)$-th iterations)
   - the prediction function is written as $F_{t-1}(x)$, 
   - the error (risk/loss) for $i$-th sample is written as $R[y_i, F_{t-1}(x_i)]$
     - Note: For L1, the error is residue, for L2 the error is squared residue.
1. get new training set $\mathcal{S}_t$ by <font color="#0000dd">reweighing the samples</font>:
   - the weight of $i$-th sample at $t$-th step is set as $w_{i,t} = R[y_i,F_{t-1}(x_i))$
2. Train the $t$-th weak learner $h_t(x)$ with  $\mathcal{S}_t$
   - Normal training process, like, just train a model on a dataset
3. Calculate the learner weight $\alpha_t$ by minimizing the loss, i.e.:
    - $$\alpha_t = \underset{\alpha_t}{\argmin}R_t(\alpha_t) $$ 
    - $$R_t(\alpha_t) :=\sum_i R[y_i, F_{t}(x_i)] = \sum_i R\{y_i,[F_{t-1}(x_i)+\alpha_t h_t(x)]\}$$

Ref: [Wiki - AdaBoost](https://en.wikipedia.org/wiki/AdaBoost)

#### 3.1.2. Deep Dive: AdaBoost weights

- Differentiate two "weight" here.
  - Sample weight $w_{t,i}$: after each iteration, the sample will be re-weighed, the re-weighed sample is used to train t-th weak learner $h_t(x)$.
  - Learner weight $\alpha_t$: the coefficient for $t$-th learner, used to minimize loss when adding $h_t(x)$ after it is trained.
- What does "sample weight" means? How to implement it?
  - **Understanding:** You can view weight as occurrence of sample. e.g. 
    - $w=0 \Leftrightarrow$ remove the sample. $\underset{\text{linear loss}}{\Leftrightarrow}$ $0\times$ the loss contribution.
    - $w=3 \Leftrightarrow$ add two more same sample. $\underset{\text{linear loss}}{\Leftrightarrow}$  $3\times$ the loss contribution.
  - **Implementation:**
    - **Preferred: Sample the data from dataset with probability proportional to the weight.**($\Leftrightarrow$Directly add or remove samples.)
      - Pro: 
        - Extremely simple to implement.
        - No need to change code for non-linear loss or subsampling cases.
    - Modify the loss function.
      - Cons: 
        - For loss linear to number of sample, like L1/L2, just need to multiply its contribution by $w_{t,i}$
        - But for loss <font color="#dd0000">non-linear to number of sample, like entropy</font>, hard to change the code.
    - Ref: [TowardsDataScience](https://towardsdatascience.com/boosting-the-accuracy-of-your-machine-learning-models-f878d6a2d185)

<!-- The t-th iteration (to get the t-th weak learner). The error is expressed by:
$$E_t(x_i) = \sum_i E[F_{t-1}(x_i) + \alpha_t h_t(x_i)]$$ -->

### 3.2. Gradient Boost

Train on <font color="#0000dd">**pseudo residual**</font> (negative **gradient** of loss), to minimize loss. I.e. After each step, we generate a new pseudo residue, the new tree-stump will fit the new pseudo residue.

### 3.2.0. Pseudo residual

$$r_{t}(x) := -\frac{\partial R(y,F_{t-1}(x))}{\partial F_{t-1}(x)}$$

**Note**: The form of loss $R$ is NOT limited to MSE, though the name "Pseudo residual" come from MSE loss ($\times \frac{1}{2}$), the pseudo residual of MSE is the residual: 

$$r_{MSE}(x) = - \frac{1}{2} \frac{\partial (y-F_{t-1})^2}{\partial F_{t-1}}= (y-F_{t-1}):=residue$$

The idea is simple: if we can find a function to fit the residue. Adding it to our previous function will make our prediction more precise.

**Note**: limitation: the choice of loss must be differentiable.
Why <font color="#0000dd">**pseudo residue**</font> is the negative gradient of loss

### 3.2.1 Prediction Function

Same with AdaBoost:

$$F_{t}(x)=F_{{t-1}}(x)+\alpha_{t}h_{t}(x)$$

### 3.2.1 Training process:

Similar to AdaBoost:

- In the $t$-th step, (after $(t-1)$-th iterations)
   - the prediction function is written as $F_{t-1}(x)$, 
   - the loss for $i$-th sample is written as $R[y_i, F_{t-1}(x_i)]$

0. start with a constant value function: 
   - $F_0 = \underset{F_0}{\argmin}(\sum_i R(y_i,F_0))$
1. Get a new training set $\mathcal{S}_t = \{...(x_i,r_{t-1,i})...\}$ by <font color="#0000dd">calculating the pseudo reside</font>:
   - $r_{t-1,i}:=r_{t-1}(x_i)$ after $(t-1)$-th step and form a 
2. Fit the model $h_t(x)$ to $\mathcal{S_t}$
3. Calculate the learner weight $\alpha_t$ by minimizing the loss, i.e.:
    - $$\alpha_t = \underset{\alpha_t}{\argmin}R_t(\alpha_t) $$ 
    - $$R_t(\alpha_t) :=\sum_i R[y_i, F_{t}(x_i)] = \sum_i R\{y_i,[F_{t-1}(x_i)+\alpha_t h_t(x)]\}$$ 

Ref: [Wiki - Gradient Boost](https://en.wikipedia.org/wiki/Gradient_boosting)

Other Ref: [Auckland U - Slides](https://www.stat.auckland.ac.nz/~yee/784/files/ch10BoostingAdditiveTrees.pdf), [NEU - Slides](http://www.ccs.neu.edu/home/vip/teach/MLcourse/4_boosting/slides/gradient_boosting.pdf)

**AdaBoost vs Gradient Boost:** From the above process, we can see the only difference between AdaBoost and Gradient Boost is the way to generate training set $\mathcal{S}_t$ for $h_t(x)$, i.e.:

- AdaBoost reweigh sample weight based on loss
- Gradient Boost calculate pseudo gradient from loss

<font color="#dd0000"> AdaB = GB when square loss??? </font>

#### XGB
XGB use more regularized model formalizatioin to control overfitting, which gives better performance<font color="#dd0000"> (???) </font>


### 3.3 Model comparison

#### 3.3.1. Comparison AdaBoost vs Gradient Boost

AdaBoost: weak learner is trained on sample set reweighed, the loss minimization process is used to train the learner-weight.
Gradient Boost: weak learners directly fit to pseudo residue to minimize loss. (That is why gradient boost are usually more efficient.)

Note: AdaBoost has the concept of residue, but just not often mentioned.Ref: [sklearn](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.AdaBoostRegressor.html)

## Deep: KL Divergence, IG and MI

### Kullback–Leibler divergence

$$D_{KL}(P\parallel Q) =\sum _{x\in {\mathcal {X}}}P(x)\log \left({\frac {P(x)}{Q(x)}}\right) $$
where:

- $P$ and $Q$ defined on the same probability space $\mathcal{X}$
- The DL divergence is also call "**relative entropy**"
- Ref: [Wiki](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence)

### Information gain (IG)
<!-- $$IG_{Y,X}{(Y,X_i)}=D_{\text{KL}}{\left(p{(Y|X_i)}\|p{(Y)}\right)}$$ -->

$$\begin{aligned}
  IG_{Y,X}{(Y,X_i)}&=D_{\text{KL}}{\left(p{(Y|X_i)}\|p{(Y)}\right)}
  \\
  &=\sum^K_{k=1} p(C_k|X_i)\log \left({\frac { p(C_k|X_i) }{ p(C_k) }}\right)
\end{aligned}$$

### Mutual information (MI)

$$MI(Y;X_i)=D_{\mathrm {KL} }(P_{(Y,X_i)}\|P_{Y}\otimes P_{X_i})$$

Discrete representation:

$$\operatorname {MI} (Y;X_i) =  \sum _{x\in {\mathcal {X_i}}}\sum _{y\in {\mathcal {Y}}}{p_{(Y,X_i)}(y,x)\log {\left({\frac {p_{(Y,X_i)}(y,x)}{p_{Y}(y)\,p_{X_i}(x)}}\right)}}$$

Ref: [Wiki](https://en.wikipedia.org/wiki/Mutual_information)

#### MI is the expectation of IG 

$$\begin{aligned}
  \operatorname {MI} (Y;X_i) = & \sum _{X_i\in {\mathcal {X_i}}} \sum^K_{k=1 }{p(C_k,X_i)\log {\left(
    {\frac {p(C_k,X_i)}{p(C_k)\,p(X_i)}}
    \right)}}
   \\
   = & \sum _{X_i\in {\mathcal {X_i}}} \left( 
     \sum^K_{k=1 }{p(C_k|X_i)p(X_i)\log {\left({\frac {p(C_k|X_i)}{p(C_k)}}\right)}}
     \right)
  \\
   = & \sum _{X_i\in {\mathcal {X_i}}} p(X_i) \left( 
     \sum^K_{k=1 }{p(C_k|X_i)\log {\left({\frac {p(C_k|X_i)}{p(C_k)}}\right)}}
     \right)
  \\
   = & \sum _{X_i\in {\mathcal {X_i}}} p(X_i) 
     D_{KL}(p(Y|X_i) \| p(Y))
  \\
   = & \sum _{X_i\in {\mathcal {X_i}}} p(X_i) IG(Y,X_i)
   \\
   = &  \underset{X_i \in \mathcal{X_i}}{\mathbb{E}}[ IG(Y,X_i) ]
\end{aligned}$$

Ref: [StakeExchange](https://stats.stackexchange.com/questions/103175/information-gain-is-kl-divergence)

#### MI and Entropy

$$\begin{aligned}
  \operatorname {MI} (Y;X_i) = & \sum _{X_i\in {\mathcal {X_i}}} \sum^K_{k=1 }{p(C_k,X_i)\log {\left(
    {\frac {p(C_k,X_i)}{p(C_k)\,p(X_i)}}
    \right)}}
   \\
   = & \sum_{X_i\in {\mathcal {X_i}}}\sum^K_{k=1 }{p(C_k,X_i)\log {\left({\frac {p(C_k|X_i)}{p(C_k)}}\right)}}
   \\
   = & \sum_{X_i\in {\mathcal {X_i}}}\sum^K_{k=1 } \left[
     p(C_k,X_i) \log p(C_k|X_i) - p(C_k,X_i) \log p(C_k)
     \right]
    \\
   = & - 
     [ -\sum^K_{k=1 }\sum_{X_i\in {\mathcal {X_i}}}p(C_k,X_i) \log p(C_k|X_i) ] +
     \sum^K_{k=1 } - \log p(C_k)
     \sum_{X_i\in {\mathcal {X_i}}} p(C_k,X_i) 
     \\
   = & -H(Y|X_i) + 
     \sum^K_{k=1 } - p(C_k)\log p(C_k)
     \\
   = & H(Y) - H(Y|X_i)   
\end{aligned}$$

Ref: [StakeExchange](https://stats.stackexchange.com/questions/103175/information-gain-is-kl-divergence)

#### Example: MI Calculation process for decision tree

We have two labels (0,1), and 1 input feature $x\in [-1, 1]$:

Before the treestump split, the entropy is calculated by
$$\begin{aligned}
  H(Y) = - [& p(y=0)\log p(y=0) \\
  &+ p(y=1)\log p(y=1) ]
\end{aligned}$$

After treestump, let's say the threshold for $x$ is 0, the conditional entropy is calculated by:
$$ \begin{aligned}
  H(Y) = - [& p(x<0,y=0)\log p(y=0|x<0) \\
   &+ p(x>0,y=0)\log p(y=0|x>0) \\
   &+ p(x<0,y=1)\log p(y=1|x<0) \\
   &+ p(x>0,y=1)\log p(y=1|x>0) ] 
\end{aligned}$$

<!-- ## $D_{KL}$, $IG$ and $MI$

From KL Divergence to Information gain:
$$\begin{aligned}
  IG_{Y,X}{(Y,X_i)}&=D_{\text{KL}}{\left(p{(Y|X_i)}\|p{(Y)}\right)}
  \\
  &=\sum^K_{k=1} p(C_k|X_i)\log \left({\frac { p(C_k|X_i) }{ p(C_k) }}\right)
\end{aligned}$$ -->