# Normal distribution

## 1. Basics

The pdf of normal distribution is

$$f(x)=\frac {1}{\sigma {\sqrt {2\pi }}}e^{-{\frac {1}{2}}\left({\frac {x-\mu }{\sigma }}\right)^{2}}$$

## 2. Key statistics

$X\sim \mathcal{N}(\mu,\sigma)$: random variable $X$ follow normal distribution.

- mean: $\mathbb{E}(X) = \mu$
- variance: $\mathrm{Var}(X) = \sigma^2$
  - SD: $\sigma(X) = \sigma$
- median: $\mathrm{mid}(X) = \mu$
- mode: $\mathrm{mod}(X) = \mu$


## Deep Dive 1. Sum of two independent normal distribution

Ref: [CSDN](https://blog.csdn.net/chaosir1991/article/details/106960408)

Let $X\sim \mathcal{N}(\mu_1,\sigma_1)$, $Y\sim \mathcal{N}(\mu_2,\sigma_2)$, $Z=X+Y$, then what is the distribution of Z.

Note, the probability of X=x and Y=y, i.e. $P(X=x, Y=y, Z=x+y)$ is NOT summation ~~(P(X=x)+P(Y=y))~~ but product:

$$P(X=x, Y=y, Z=x+y) = P(X=x)P(Y=y)$$

So, the distribution of variable $Z$'s value $z$ should be:

$$\begin{aligned}
    P(Z=z) & = \int^{\infty}_{x\rightarrow-\infty}f(X=x)f(Y=z-x)dx\\
    & = \int^{\infty}_{x\rightarrow-\infty}
    \frac {1}{\sigma_1 {\sqrt {2\pi }}}e^{-{\frac {1}{2}}\left({\frac {x-\mu_1 }{\sigma_1 }}\right)^{2}}
    \frac {1}{\sigma_2 {\sqrt {2\pi }}}e^{-{\frac {1}{2}}\left({\frac {z-x-\mu_2 }{\sigma_2 }}\right)^{2}}
    dx\\
    & = \int^{\infty}_{x\rightarrow-\infty}
    C_1e^{-{\frac {1}{2}}\left({\frac {x-\mu_1 }{\sigma_1 }}\right)^{2}-{\frac {1}{2}}\left({\frac {z-x-\mu_2 }{\sigma_2 }}\right)^{2}}
    dx\\
    & = \int^{\infty}_{x\rightarrow-\infty}
    C_1e^{-{\frac {1}{2}}\left({\frac {x-(C_3z+C_4) }{C_2 }}\right)^{2}-{\frac {1}{2}}\left({\frac {z-C_6 }{C_5 }}\right)^{2}}
    dx\\
    & = C_1e^{-{\frac {1}{2}}\left({\frac {z-C_6 }{C_5 }}\right)^{2}}\int^{\infty}_{x\rightarrow-\infty}
    e^{-{\frac {1}{2}}\left({\frac {x-(C_3z+C_4) }{C_2 }}\right)^{2}}
    dx\\
    &= C_1 e^{C_2z^2+C_3z+C_4} 
    \\& = C_1e^{-\frac{1}{2}(\frac{z-C_3}{C_2})^2}
    \\ \Rightarrow & Z \sim \mathcal{N}(\mu_z,\sigma_z)
\end{aligned}$$

Note: for simplicity, $C_i$ just denotes there is a constant in the equation, not refer to any fixed specific value. 

So, in short-answer/intuition, the sum of two or more independent normal distribution variable are still normal distribution.

From [CSDN](https://blog.csdn.net/chaosir1991/article/details/106960408), we can know, here $Z \sim \mathcal{N}(\mu_z,\sigma_z)$:

$$\begin{cases}
    \mu_z = \mu_x+\mu_y\\
    \sigma^2_z = \sigma^2_x+\sigma^2_y
\end{cases}$$

Similarly, if $Z:=X-Y$, still $Z \sim \mathcal{N}(\mu_z,\sigma_z)$ and:

$$\begin{cases}
    \mu_z = \mu_x-\mu_y\\
    \sigma^2_z = \sigma^2_x+\sigma^2_y
\end{cases}$$


### DD-1. Extra: How about product or division of two independent normal distribution?

Ref: [Wiki-Ratio_Distribution](https://en.wikipedia.org/wiki/Ratio_distribution)
If $Z:=X/Y$, then the distribution of $Z$ is not normal anymore, as:

$$\begin{aligned}
    P(Z=z) & = \int^{\infty}_{x\rightarrow-\infty}f(X=x)f(Y=x/z)dx\\
    & = \int^{\infty}_{x\rightarrow-\infty}
    \frac {1}{\sigma_1 {\sqrt {2\pi }}}e^{-{\frac {1}{2}}\left({\frac {x-\mu_1 }{\sigma_1 }}\right)^{2}}
    \frac {1}{\sigma_2 {\sqrt {2\pi }}}e^{-{\frac {1}{2}}\left({\frac {x/z-\mu_2 }{\sigma_2 }}\right)^{2}}
    dx\\
    & = \int^{\infty}_{x\rightarrow-\infty}
    \frac {1}{\sigma_1 {\sqrt {2\pi }}}e^{-{\frac {1}{2}}\left({\frac {x-\mu_1 }{\sigma_1 }}\right)^{2}}
    \frac {1}{\sigma_2 {\sqrt {2\pi }}}e^{-{\frac {1}{2}}\left({\frac {x-\mu_2 z }{\sigma_2 z }}\right)^{2}}
    dx\\
    & = \int^{\infty}_{x\rightarrow-\infty}
    C_1e^{-{\frac {1}{2}}\left({\frac {x-\mu_1 }{\sigma_1 }}\right)^{2}-{\frac {1}{2}}\left({\frac {x-\mu_2z }{\sigma_2z }}\right)^{2}}
    dx\\
    & = \int^{\infty}_{x\rightarrow-\infty}
    C_1e^{-{\frac {1}{2}}\left({\frac {x-(C_3z+C_4) }{C_2 z}}\right)^{2}-{\frac {1}{2}}\left({\frac {C_5z-C_6 }{C_7z-C_8 }}\right)^{2}}
    dx\\
    & = C_1e^{-{\frac {1}{2}}\left({\frac {C_5z-C_6 }{C_7z-C_8 }}\right)^{2}}\int^{\infty}_{x\rightarrow-\infty}
    e^{-{\frac {1}{2}}\left({\frac {x-(C_3z+C_4) }{C_2z }}\right)^{2}}
    dx\\
    &= C_1 e^{\frac{C_2z^2+C_3z+C_4}{C_5z^2+C_6z+C_7}} 
\end{aligned}$$

If $Z:=X\times Y$, Ref: [Wiki-Product_distribution](https://en.wikipedia.org/wiki/Distribution_of_the_product_of_two_random_variables#Independent_central-normal_distributions), [pdf](http://www1.up.poznan.pl/cb48/prezentacje/Oliveira.pdf)

$$\begin{aligned}
    P(Z=z) & = \int^{\infty}_{x\rightarrow-\infty}f(X=x)f(Y=x/z)dx\\
    & = \int^{\infty}_{x\rightarrow-\infty}
    \frac {1}{\sigma_1 {\sqrt {2\pi }}}e^{-{\frac {1}{2}}\left({\frac {x-\mu_1 }{\sigma_1 }}\right)^{2}}
    \frac {1}{\sigma_2 {\sqrt {2\pi }}}e^{-{\frac {1}{2}}\left({\frac {z/x-\mu_2 }{\sigma_2 }}\right)^{2}}
    dx\\
    & = \int^{\infty}_{x\rightarrow-\infty}
    \frac {1}{\sigma_1 {\sqrt {2\pi }}}e^{-{\frac {1}{2}}\left({\frac {x-\mu_1 }{\sigma_1 }}\right)^{2}}
    \frac {1}{\sigma_2 {\sqrt {2\pi }}}e^{-{\frac {1}{2}}\left({\frac {z-\mu_2 x }{\sigma_2 x }}\right)^{2}}
    dx\\
    & = \int^{\infty}_{x\rightarrow-\infty}
    C_1e^{-{\frac {1}{2}}\left({\frac {x-\mu_1 }{\sigma_1 }}\right)^{2}-{\frac {1}{2}}\left({\frac {z-\mu_2x }{\sigma_2x }}\right)^{2}}
    dx\\
    & \approx \int^{\infty}_{x\rightarrow-\infty}
    C_1e^{-{\frac {1}{2}}\left({\frac {[x+(C_3z+C_4)]^2 }{C_2 x}}\right)^{2}-{\frac {1}{2}}\left({\frac {z-C_6 }{C_5 }}\right)^{4}}
    dx\\
    & = C_1e^{-{\frac {1}{2}}\left({\frac {z-C_6 }{C_5 }}\right)^{4}}\int^{\infty}_{x\rightarrow-\infty}
    e^{-{\frac {1}{2}}\left({\frac { [x-(C_3z+C_4)]^2 }{C_2x }}\right)^{2}}
    dx\\
    &\approx C_1e^{-{\frac {1}{2}}\left({\frac {z-C_6 }{C_5 }}\right)^{4}}
\end{aligned}$$

From the above two qualitative deduction, we can find that, for division or product, the distribution is not normal.