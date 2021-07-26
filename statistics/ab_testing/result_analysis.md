# Result Analysis

## Evaluation metric analysis

### Examples

We use from cases from an Udacity course as the example. Link: [Udacity-Ab_test](https://classroom.udacity.com/courses/ud257/lessons/4018018619/concepts/40043987150923) or [Udacity-Ab_test(Youtube)](https://youtu.be/6pGDwrJHitw)

Q: the purple ranges (1) - (6) are different confident interval cases (for $\Delta \mu$), analyze the decision we should take.

<div  align="center"><img src=./result_analysis_asset/confidence_interval_cases.png style = "zoom:15%"></div>

**Explanation**:

1. $ \min(\Delta \mu) > d_{\min} \Rightarrow$ practically significant, 
   - I.e., $p_{EG}>p_{CG}+d_{min}$ must stand.
   - Therefore: the EG version are much better than CG version, should launch the new (EG) version.
2. $\Delta \mu$ may be zero, and $\max(\Delta \mu) < d_{\min}$ 
   - I.e. $p_{EG}=p_{CG}$ may stand, and $p_{EG}<p_{CG}+d_{min}$ must stands
   - Since: $p_{EG}<p_{CG}+d_{min}$ must stand.
   - Therefore: the improvement of EG version must be lower than our expectation (i.e. $d_{\min}$), thus, no need to launch it.
3. $\min(\Delta \mu) >0$, but $\max(\Delta \mu) < d_{\min}$ 
   - I.e. $p_{EG}>p_{CG}$ and $p_{EG}<p_{CG}+d_{min}$ must stand.
   - Since: $p_{EG}<p_{CG}+d_{min}$ must stands
   - Therefore: Although there must be an improvement of EG compared to CG, the improvement of must be lower than our expectation (i.e. $d_{\min}$). Thus, no need to launch it.
4. $\min(\Delta \mu) <-d_{\min}$, and $\max(\Delta \mu) > d_{\min}$ 
   - I.e. Both $p_{EG}<p_{CG} - d_{\min}$ or $p_{EG}>p_{CG}+d_{min}$ may stand.
   - Therefore: the EG may be much worse or may be better than our expectation, we are not sure, so need additional tests.
5. $\min(\Delta \mu) <0$, and $\max(\Delta \mu) > d_{\min}$ 
   - I.e. Both $p_{EG}<p_{CG}$ or $p_{EG}>p_{CG}+d_{min}$ may stand.
   - Therefore: the EG may be slightly worse or may be better than our expectation, we are not sure, so need additional tests.
6. $\min(\Delta \mu) >0$, and $\max(\Delta \mu) > d_{\min}$ 
   - I.e. Both $p_{EG}>p_{CG}$ must stand but $p_{EG}>p_{CG}+d_{min}$ may or may not stand.
   - Therefore: the EG must be better than CG, but we are not sure whether it is better than our expectation, so need additional tests.

## Sanity check

asdfasdfasf

## Deep dive: practically and statistically significant

### Meaning of "significant"

"Practically" or "statistically significant" is in terms of the statement "$p_{EG}>p_{CG}$"

"**Statistically significant**" means "$p_{EG}>p_{CG}$" is statistically/mathematically correct. 

However, if $p_{EG}$ just slightly larger than $p_{CG}$, then, even "$p_{EG}>p_{CG}$" is mathematically correct, there is not too much difference between the experimental version and control version. Then,the experimental version is not worthy to launch. I.e the conclusion "$p_{EG}>p_{CG}$" have no practical use.

Then, how to determine if the conclusion "$p_{EG}>p_{CG}$" have practical use?

Remember that, we set a $d_{\min}$ value. This implies we hope the $p_{EG} \geq p_{CG}+d_{\min}$, then, the experimental version is worthy to launch.

Thus, if we can confirm $p_{EG} \geq p_{CG}+d_{\min}$, then, we will say the conclusion "$p_{EG}>p_{CG}$" from the ab-testing is **practically significant**. I.e. the conclusion "$p_{EG}>p_{CG}$" from the ab-testing is of-practically-use/practically-useful.

### Experiment result and "significant"


In our experiment, $\Delta \mu$ is the difference between $p_{EG}$ and $p_{CG}$. I.e. $\Delta \mu = p_{EG} - p_{CG}$

From the above section, we can know:

- If $\Delta \mu>0$ stands, we can claim the result is statistically significant.
- IF $\Delta \mu>d_{min}$ stands, we can claim the result is practically significant.

However, with experiment, we can never get the ground true $\Delta \mu$, $\Delta \mu$ has a probability distribution, mathematically, $\Delta \mu$ could be all value. The only matter is that the probability (density) for different value is different.

Thus, we solve the problem in such way:

<div  align="center"><img src=./result_analysis_asset/statistically_and_practically_significant.jpeg style = "zoom:25%"></div>

 By setting the confidence level, we can determine limit the possible value of $\Delta \mu$ to a limit range, i.e. *confidence interval*. And if the minimum possible $\Delta \mu$ value, (i.e. $\Delta \mu_{\min}$), is larger than 0 (i.e. $\Delta \mu_{\min} > 0$), it is reasonable to claim $\Delta \mu$ must be larger than 0 ( i.e. $\Delta \mu >0$ stands). Then, we can claim the result is "statistically significant". I.e.:

$$\begin{aligned}
``\Delta\mu_{\min} >0" & = {\text{``the minimum possible } \Delta \mu \text{ value is larger than 0''}} 
\\ &= `` \Delta \mu \text{ must be larger than 0 "  } 
\\ &= `` p_{EG} \text{ must be larger than }p_{CG}" \rightarrow {``H_0 \text{ is incorrect"}}
\\ &\underset{ \text{i.e.} }{\rightarrow} \text{the result is statistically significant}
\end{aligned}$$

Similarly, when $\Delta \mu > d_{\min}$, we can claim $p_{EG} > p_{CG} + d_{\min}"$ must be correct, then we can claim the result is practically significant.

$$\begin{aligned}
``\Delta\mu_{\min} >d_{\min}" & = {\text{``the minimum possible } \Delta \mu \text{ value is larger than } d_{\min}"} 
\\ &= `` \Delta \mu \text{ must be larger than } d_{\min}"
\\ &= `` p_{EG} \text{ must be larger than }p_{CG} + d_{\min}" \rightarrow {``H_0 \text{ is incorrect"}}
\\&\underset{ \text{i.e.} }{\rightarrow} \text{the result is practically significant}
\end{aligned}$$

Note, the formula of $\Delta \mu_{\min}$:

$$\begin{aligned}
    \Delta \mu_{\min} &:= \min( \mathrm{CI}_{\Delta \mu} )\\ 
    &= \Delta\mu^{obs} - z\cdot \mathrm{SE} \\
    &= (p^{obs}_{EG} - p^{obs}_{CG}) - z\cdot \frac{s_{\mathrm{pooled}}}{\sqrt{N}} 
\end{aligned}$$
where z is determined by confidence level $\mathrm{C}$, i.e. $z = z(\mathrm{C})$. 

<!-- 
When $\Delta \mu > 0$, we can infer that  ${``H_0 (p_{EG} = p_{CG} )\text{ is incorrect"}} $ or ${`` p_{EG} \text{ must be larger than }p_{CG}" }$, then we will say the statement "$p_{EG}>p_{CG}$" is statistically significant.



We made decision by the question whether 

$\Delta \mu_{\min}$ is understand as the smallest possible value for $\Delta \mu$, given the experiment, at certain confidence $\mathrm{C}$.  [Note: $z=z(\mathrm{C})$]

$\Delta \mu = p_{EG}-p_{CG}$ -->






<!-- This is because: -->



<!-- 
In our experiment, 

When $\Delta \mu > d_{\min}$: -->


<!-- The judgement "H_0 is wrong" is statistical significant when  -->

<!-- <div  align="center"><img src=./result_analysis_asset/statistically_and_practically_significant.jpeg style = "zoom:25%"></div> -->

Ref: [Udacity-Ab_test](https://classroom.udacity.com/courses/ud257/lessons/4018018619/concepts/40043987150923): practical siginificant and statistical significant





## Deep Dive: Confidence Interval

Confidence interval( $\mathrm{CI}$ ): the range where the ground true value will fall in with the probability of confidence level.

Confidence level ( $\mathrm{C}$ ): the probability that the ground true value fall in confidence interval.

E.g. $\mathrm{CI}$ for mean value in ab testing (z-test.)

<div  align="center"><img src=https://qph.fs.quoracdn.net/main-qimg-87329ae87bcf6e926acfec80f426aa6b.webp style = "zoom:60%"></div>

$$\mathrm{CI := \mu \pm z\cdot \mathrm{SE}} = \mu \pm z \cdot\frac{SD}{\sqrt{N}} $$

where:

- $z$ score: the width of the $\mathrm{CI}$. 
  - A numerical value, the unit is the standard deviation of the distribution. 
  - Determined by confidence level $\mathrm{C}$. E.g. $z(\mathrm{C} = 95\% ) =1.96$
- Note: The distribution here is the distribution of $\Delta \mu$ i.e. $\sim$ population mean. Thus, the standard deviation of the distribution is SE.


Ref: [StatisticsHowTo](https://www.statisticshowto.com/probability-and-statistics/confidence-interval/), [Quora](https://www.quora.com/What-is-the-difference-between-confidence-interval-and-confidence-level), [Wiki-Confidence_interval](https://en.wikipedia.org/wiki/Confidence_interval)