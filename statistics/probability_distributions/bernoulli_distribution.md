# Bernoulli Distribution

## 1. Bernoulli Trial

Bernoulli trail: an experiment

- the outcome can only give two possible values, i.e. 0 and 1.
- the possibility of 1 is same every time.

For example:

- toss a coin for 1 time is 1 time bernoulli trial. (H/T is the outcome.)
- show a button to a user for 1 time is 1 time bernoulli trial. (Whether the user click the button is the outcome)

Bernoulli trial is also called binominal trial. But Bernoulli distribution is different with binominal distribution.

Ref: [Wiki-Bernoulli_trial](https://en.wikipedia.org/wiki/Bernoulli_trial)

## 2. Bernoulli Distribution Basics

- A possibility distribution on the outcome i.e. 0,1 of the Bernoulli trial.

- $X\sim Bernoulli(p)$ means X is a variable follow bernoulli distribution. I.e. At each sampling, the possibility for $X=1$ is $p$ and the possibility for $X=0$ is $(1-p)$.

- The possibility mass function of Bernoulli distribution:
    <div  align="center"><img src=http://probabilitycourse.com/images/chapter3/bernoulli(p)%20color.png style = "zoom:30%"></div> 

  - The x axis is the outcome 0,1 of a bernoulli trial.
  - The y axis is the possibility of outcome 0, 1.
  - Ref: [Probabilitycourse](https://www.probabilitycourse.com/chapter3/3_1_5_special_discrete_distr.php)

- Note:

  - "[possibility mass function](https://en.wikipedia.org/wiki/Probability_mass_function)" describe the possibility distribution on **discrete** variable. 
    - On each x position, the y value is the **possibility** for that x value.
  - "Possibility density function (pdf)" describe for the possibility distribution on **continuous** variable.
    - On each x position, the y value is the **possibility density** for that x value.

## 3. Bernoulli Distribution Statistics

$X\sim Bernoulli(p)$

- Mean: $E(X) = p$
- Variance: $Var(X) = p(1-p)$
  - Population standard deviation $\sigma(x) := SD(X) = \sqrt{p(1-p)} $