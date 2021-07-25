# Standard Deviation

## SD vs SE: different kinds of "standard deviation"
- SD: standard deviation of the **population**
  - We have a sample set with N samples: $\mathcal{S} = \{x_1,\dots, x_N\}$
    - $SD:=$ the standard deviation of theses samples. 
  - $SD:=\sigma(x) = \sqrt{\frac{\sum^N_{i-1} (x_i - \mu)^2 }{N}} = \sqrt{Var(x)}$
    - $Var(x) := E[ (x-\bar{x})^2] = E(x^2) - [E(x)]^2$
    - $\mu(x)$ means the standard deviation of the x variable.
    - $E(x)$ means the expectation of the x variable.'
  - From the definition, we can see **SD is almost independent of sample size**. I.e. $E(SD) \perp N$ (TBD: any proof/ref?)
- SE: standard 'error' of the **mean value**. Also written as "SEM"
  - We have enormous sample sets (each has $N$ samples)  $\{\mathcal{S}_1,\mathcal{S}_2,\dots\}$ drawn from a same distribution.
    - For each sample set, we have a mean value: $\mu_1, \mu_2, \dots$
  - $SE:=$ the standard deviation of the these mean values
    - $SE:=\sigma(\mu)$
    - $\sigma(\mu)$ means the standard deviation of the $\mu$ variable.
    - $SE=\frac{SD}{\sqrt{N}}\propto\frac{1}{\sqrt{N}}$
- Ref: 
  - [Openanesthesia](https://www.openanesthesia.org/se_vs-_sd_calculation/): 'SD' vs. 'SE'
  - [Wiki-Standard_deviation](https://en.wikipedia.org/wiki/Standard_deviation#:~:text=The%20mean's%20standard%20error%20turns,root%20of%20the%20sample%20size.): $SE=\frac{SD}{\sqrt{N}}$