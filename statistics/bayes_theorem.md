# Bayes' theorem

## 1. Basics:

$$P(A|B) = \frac{P(A,B)}{P(B)}$$

## 2. Probability density functions (pdf)

### 2.1 Joint pdf

- Form: 
$$f(x,y)$$
- Properties:
  - $f(x,y) \geq 0 \forall (x,y)$
  - $\int\int f(x,y) dx dy =1$


[Ref: bayesball, Sec 6.4](https://bayesball.github.io/BOOK/joint-probability-distributions.html)

### 2.2 Marginal pdf

- Form:
$$f_{X}(x) = \int f(x,y) dy$$

"Marginal" is just the "prior". Ref: [Utah.edu](https://my.eng.utah.edu/~cs5961/Resources/bayes.pdf)

### 2.3 Conditional pdf

- Form:
$$f_{Y|X}(y|X=x) = \frac{f(x,y)}{f_X(x)}$$

Ref [Wiki: discrete v.s. continuos distribution](https://en.wikipedia.org/wiki/Conditional_probability_distribution)

Posterior is a kind of conditional probability

Ref: [StakeExchange](https://math.stackexchange.com/questions/1823669/what-is-the-difference-between-conditional-and-posterior-probability#:~:text=Posterior%20probability%20is%20a%20conditional,the%20parameter%20given%20observed%20X.)
