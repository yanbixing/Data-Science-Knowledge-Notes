# Linear Regression




## Deep Dive: Confidence interval vs prediction interval (TBD)

Ref: [YouTube-CI_vs_PI](https://youtu.be/o0UESA3UZss), [StakeExchange](https://stats.stackexchange.com/questions/16493/difference-between-confidence-intervals-and-prediction-intervals)

- Prediction interval (PI): **Given an estimator** function $f_{est}(x)$, the 90\% PI is the $2w_{PI}(x)$ width area centered with $f_{est}(x)$ (i.e. $[f_{est}(x)-w_{PI}(x), f_{est} +w_{PI}(x)]$) where **90% of data will fall into**.
  - $2w_{PI}(x)$ means the width of the PI area varies with x.
    <div  align="center"><img src=./linear_regression_asset/prediction_interval.png style = "zoom:20%"></div>


- Confidence interval (CI): **Given the data**, the 90\% PI is the $2w_{CI}(x)$ area centered with the observed best estimator (i.e. $[f^{*}_{obs}(x)-w_{CI}(x), f^*_{obs} +w_{CI}(x)]$) that where the **true best estimator** have 90% percent chance to fall into.
  - $2w_{CI}(x)$ means the width of the CI area varies with x.
  

    <div  align="center"><img src=./linear_regression_asset/confidence_interval.png style = "zoom:20%"></div>


- The exact equation of CI and PI is given by:
    <div  align="center"><img src=./linear_regression_asset/equation_for_ci_and_pi.png style = "zoom:20%"></div>

  
Note: CI for estimator seems different from CI for parameter. Eg. the 95% CI for slope $\beta_1$ is just $[\beta^{obs}_1\pm 1.96\cdot\mathrm{SE}_{\beta_1}]$ , 90% CI for intercept $\beta_0$ is just $[\beta^{obs}_0\pm 1.96\cdot\mathrm{SE}_{\beta_0}]$. Ref: [Econometrics](https://www.econometrics-with-r.org/5-2-cifrc.html), [Gatech_slides](https://www2.isye.gatech.edu/~yxie77/isye2028/lecture12.pdf), 


