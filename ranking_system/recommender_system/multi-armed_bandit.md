# Multi-armed_bandit

**Personal understanding:** 

- There is a system can give us feedback (reward) according to our action, there is some rules between our action and feed back, but these rules are unknown to us. 
- If we want to know the rules, we need to do experiment, i.e. taking different actions and observe the results, so that we can infer the rule based on the observations. This process is called explore.
- We can take actions randomly, but, obviously, we cannot achieve maximum reward in this way. Take the action give us maximum reward according to our experience should be a smart choice. This process is call exploit.
- The more randomly we take action, the more confident we can confirm whether each action is good or not. However, the more tests on not-best-actions, the more loss we may have. So, this is trade-off between "explore" vs "exploit".

## Basics

Ref: [Wiki-MAB](https://en.wikipedia.org/wiki/Multi-armed_bandit)

- Definition:
  - K levers(摇杆, ~game machine)
    - The reward distribution of each lever is:
      - $B={R_1, R_2,…, R_K }$
    - With mean value of the distribution as:
      - $\mu_1, \mu_2, …, \mu_K$
    - $T$ rounds
      - Each round the gambler play one machine, get a reward r_t
  - Then, the *"regret"* $\rho$ is defined as the difference between our strategy with the ground truth best strategy:
    - $\rho =T\mu ^{*}-\sum _{t=1}^{T}{\widehat {r}}_{t}$
      - Where:
        - $\mu ^{*}=\max _{k}\{\mu _{k}\}$
  - I.e. target is to minimize the regret
  - Zero-regret strategy: the strategy that make $\underset{T \to \infty}{\lim}P( \frac{\rho}{T} \to 0)\to 1$
i.e. the strategy can




## Others Refs:

[TowardsDataScience](https://towardsdatascience.com/multi-armed-bandits-and-reinforcement-learning-dc9001dcb8da): Multi-arm bandit is a kind of reinforcement learning
[WashingtonU-PPT](http://grail.cs.washington.edu/projects/olteaching/LCOnlineLearning.pptx): Multi-arm bandit can belong to both online learning and reinforcement learning area.


## Solutions

[PersonalBlog](https://kojinoshiba.com/recsys-cold-start/): MAB for cold start problems

### $\epsilon$ greedy

### Upper Confidence Bound (UCB)

### Thompson sampling (TS) (TBD)

Ref: [Wiki-TS](https://en.wikipedia.org/wiki/Thompson_sampling)

**Personal understanding:**

- Our observation is the dataset $\mathcal{D}$, 
  - Each sample looks like $d= (\text{context} x, \text{actions } a, \text{reward } r)$
- We can define a likelihood function $\Pr(r|a,x,\theta)$ to describe relation between action and reward. (The parameter is $\theta$, and use $x$ to include context info.)
  - The reward is not certain value but has a probability distribution
  - For RecSys, the scheme is like
    - $\Pr(r|a,x,\theta) \sim \text{click-prob}=f(\text{item}, \text{user};\theta)$ 
- From:
  - the user-defined likelihood function $\Pr(r|a,x,\theta)$, 
  - Dataset $\mathcal{D}$, 
  - And a pre-defined d prior distribution $\Pr(\theta)$)
- We can get a distribution for parameter $\Pr(\theta|\mathcal{D})$
  - $\Pr(d|\theta) = \Pr(r,a,x,|\theta) = \Pr(r|a,x,\theta)\Pr(a,x|\theta)$
    - Note: $(a,x)\perp \theta$, so we should have $\Pr(a,x|\theta) = \Pr(a,x)$, i.e. the $\Pr(a',x') = \frac{M_{a',x'}}{M}$, i.e. the number of samples with such value in the dataset.
  - Then, we can get $\Pr(D|\theta) = \prod_i (d_i|\theta)$
  - Then, with bayesian theorem: $\Pr(\theta|\mathcal{D}) \propto \Pr(D|\theta) \Pr(\theta)$,
- When need to decide next action $a_{t+1}$, we can use the following way to select an action:
  - We know: given a certain $\theta'$, we can find the action that can maximize the expectation of r. 
    - $a_{maxE(r)}' = \underset{a}{\argmax} \mathbb{E}(r|\theta',a,x)$
  - So, given the distribution of $\theta$, i.e., $\Pr(\theta|\mathcal{D})$, we can have a distribution $\Pr(a_{maxE(r)}'|\mathcal{D})$
  - We can sample an action $a_{t+1}$ from the $\Pr(a_{maxE(r)}'|\mathcal{D})$ (That is why we call the method "sampling")
- After we take $a_{t+1}$, we will have a new sample add to $\mathcal{D}_{t+1} = \mathcal{D} + (x,a_{t+1},r_{t+1})$, and with $\mathcal{D}_{t+1}$ we can sample $a_{t+2}$...
  - Then the system will be optimized gradually
  - Note: in each step, the the best action is a prob distribution, according to prob distribution of parameter, so, there is a chance for all available actions, just the probability is different,
    - "explore" = we sampled an actions that have less chance to be the best action choice, i.e. small $\Pr(a_{maxE(r)}'|\mathcal{D})$
    - "exploit" = we sampled an actions that have large chance to be the best action choice, i.e. large $\Pr(a_{maxE(r)}'|\mathcal{D})$

**In sum:**


- Defined a likelihood function $\Pr(r|a,x,\theta)$ to describe the relation between action and expectation reward
- Given dataset $\mathcal{D}$, we can get the prob distribution of parameter $\theta$, i.e $\Pr(\theta|\mathcal{D})$
- Given a specific $\theta'$, we can find a best action $a_{maxE(r)}$, so if $\theta$ follow a prob distribution $\Pr(\theta|\mathcal{D})$, best action $a_{maxE(r)}$ should also follow a prob distribution $\Pr(a_{maxE(r)}|\mathcal{D})$
- We can sample action $a_{t+1}$ from $\Pr(a_{maxE(r)}|\mathcal{D})$ and use it as the action of next step. 
  - Note: Add feedback to $\mathcal{D}$ $\mathcal{D}_{t+1} : =(x,a_{t+1},r_{t+1}) + D $, with $\mathcal{D}_{t+1}$ we can get $a_{t+2}$...

### Application of MAB

**Personal feeling:** 

- Since TS (MAB) can sample unfavorable actions in small chance to "explore", so, may be used as a solution for position bias, 
- i.e. the low rank samples can be viewed as a kind of "unfavorable" action, we have some probability to expose them with TS.
- Ref: [NIPS-Paper](https://proceedings.neurips.cc/paper/2017/hash/c57168a952f5d46724cf35dfc3d48a7f-Abstract.html): this paper use MAB to solve position bias.
