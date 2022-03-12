# Multi-variate Time Series (TBD)

Some concepts may be used for multi-variate time series:

- Cross-correlation: the "convolution" we used in CNN. 
  - An operation can be used to study the similarity between two time series. 
    - Ref: [ResearchGate](https://www.researchgate.net/post/How-can-I-find-the-cross-correlation-between-two-time-series-atmospheric-data)
  - The formal convolution concept need to do a mirror reflection on one of the factor function. While the cross-correlation keep the direction of the two functions unchanged.
  - In CNN, the operation we use is actually cross-correlation, not the formal convolution.
  - Definition: $(f\star g)(\tau )\ \triangleq \int _{-\infty }^{\infty }{\overline {f(t)}}g(t+\tau )\,dt$
    - when $\tau=0$, cross-correlation is just the correlation, i.e., $\text{cross-corr(x,y)(0) = corr(x,y)}$
  - Ref: [Wiki-Cross_correlation](https://en.wikipedia.org/wiki/Cross-correlation)

<div align="center"><img src=https://upload.wikimedia.org/wikipedia/commons/thumb/2/21/Comparison_convolution_correlation.svg/800px-Comparison_convolution_correlation.svg.png style = "zoom:60%"></div>




- Vector AutoRegressive: 
  - similar to AR, but for each step $t$, the scaler $X_t$ in AR will be replaced with $n$-dim vector $\boldsymbol{X}_t$, and the scaler $\phi_t$ will be replaced with an $n\times n$ matrix $\boldsymbol{\phi}_t$
    - I.e. $\boldsymbol{X}_t =  \sum^P_{p=1}\boldsymbol{\phi}_p \boldsymbol{X}_{t-p} + \boldsymbol{Z}_t$
  - Ref: [Wiki-Vector_AR](https://en.wikipedia.org/wiki/Vector_autoregression), [AnalyticsVidhya-Blog](https://www.analyticsvidhya.com/blog/2018/09/multivariate-time-series-guide-forecasting-modeling-python-codes/)



- Dynamic Time Warping (DTW)
  - Ref: [Wiki-DTW](https://en.wikipedia.org/wiki/Dynamic_time_warping)
  - This algorithm can give a distance (DTW distance), which you can use it to do classification (KNN) or clustering (DBSCAN). Ref: [StackExchange](https://stats.stackexchange.com/questions/131281/dynamic-time-warping-clustering)
    - Note: 
      - Classical K-means algorithm cannot do clustering (centroid ← cluster center)
      - But a modified K-means may be used to do clustering for time series data.
      - Ref: [Personal-Blog](http://alexminnaar.com/2014/04/16/Time-Series-Classification-and-Clustering-with-Python.html)