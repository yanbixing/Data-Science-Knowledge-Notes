# Evaluation metrics

## 1. Regression:

- Mean square error (MSE)
- Mean absolute error (MAE)

## 2. Classification:

### 2.1. Entropy-based

- Information gain
- Mutual info

#### 2.1.1. Cross Entropy (TBD)


#### 2.1.1.2 CE vs AUC

**Conclusion:** For model output probability, CE is a better metric than AUC.

Similarity:

- Both CE and AUC are **threshold-invariant**.
  - I.e. the value of CE and AUC are independent from the choice of threshold.

Difference:

- CE is better than AUC if you need to calibrated your score, usually **when your score represent "probability"**.
  - AUC is **scale-invariant**.
    - I.e. when you change (stretch, compress, transform) the scale of the score, the metric value doesn't change 
    - I.e. **AUC** only cares about the ranking distribution of positive and relative samples, **NOT dependent (care about) on the absolute value of score**.
      - AUC is the prob that a random positive sample have higher ranking/score than a random negative sample.
    - This is not good when your score have specific meaning, like probability.
  - CE's value is dependant on the absolute value of the score. A
    - CE is positively correlated with to the difference between two distribution.
    - I.e. **CE is minimized when the estimated/predicted probability distribution is same with real probability distribution.**
    - Additional Notes:
      - $D_{KL} = P\log \frac{P}{Q} = CE + P\log P$ exactly measure the difference between two distribution. ($D_{KL} = 0 $ as $P=Q$, for two same distribution, KL-Divergence is 0.)
      - $M \cdot CE = NNL \Rightarrow$ minimize CE = Maximize likelihood (MLE)
      - Ref: [entropy_related_statistics.md](../math_topics/entropy_related_statistics.md)
- But CE is affected by class imbalance while AUC not.

Ref:
[Educative-GMLI-Ads-Metrics](https://www.educative.io/courses/grokking-the-machine-learning-interview/N0P4R1v8mXL): CE better than AUC when your output means "probability".

#### 2.1.1.3 CE and class imbalance (TBcleaned)

- E.g.You model always predict 0.7
- Case 1, 99 positive, 1 negative
  - CE
    - $100CE = NNL = -(99\cdot \log0.7 + 1\log0.3)$
  - AUC
    - TPR = TP/P = 99/99 = 100%
    - FPR = FP/N = 1/1 = 100%

- Case 2, 50 positive, 50 negative
  - CE
    - $100CE = NNL = -(50\cdot \log0.7 + 50\log0.3)$
  - AUC
    - TPR = TP/P = 50/50 = 100%
    - FPR = FP/N = 50/50 = 100%

### 2.2. Confusion matrix based

Confusion matrix: the matrix of (TP FP TN FN). Ref: [Wiki-Confusion_matrix](https://en.wikipedia.org/wiki/Confusion_matrix)

#### 2.2.1 Precision and Recall related

- $\text{precision} = \frac{TP}{TP+FP}$ <br> English: Among the predicted positive, how many are correct (condition positive $P$ ).
  - When to use: 
    - we don't want FP, i.e., .false positive is harmful to user.
    - E.g. Spam filter false positive will cause user miss important info.


- $\text{recall} = \frac{TP}{TP+FN} = TPR = \text{sensitivity}$
English: Among the condition positive ($P$), how may are correct (predicted positive).
  - When to use:
    - We don't want FN i.e. False negative is harmful to user.
    - E.g. Cancer identifier, false negative will cause the user miss the best treatment period.

- $\text{f1} = \frac{1}{\frac{ \frac{1}{\text{precision}} + \frac{1}{Recall} }{2}} = \frac{2\cdot \text{precision} \cdot \text{recall}}{\text{precision} + \text{recall}}$<br>English: The harmonic mean of precision and recall

- $\text{accuracy} = \frac{TP+TN}{P+N} $ 
  - For balanced dataset, $\text{accuracy}\overset{P=N}{\rightarrow} \frac{1}{2} (\frac{TP}{P} + \frac{TN}{N}) = \frac{1}{2}(TPR+TNR)$
  - $TNR = \frac{TN}{N}$
- Ref: [Blog](https://www.springboard.com/blog/machine-learning-interview-questions/)
- Note: precision-recall tradeoff: when you decrease the threshold, more sample will be classified as positive, 
  - $\text{recall} = \frac{TP \uparrow}{P \times} \uparrow$
  - $\text{precision} = \frac{TP}{TP+FP} = \frac{1}{1+\frac{FPR}{TPR}} = \frac{1}{1+\frac{1}{k_{roc}}}$
    - $k_{roc}:=\frac{TPR}{FPR}$, for **typical** ROC curve (model better than random guess) , when decrease threshold, $k_{roc}$ drop from $\infty \rightarrow 1$, i.e. $\text{precision} = \frac{1}{1+\frac{1}{k_{roc}\downarrow}\uparrow}\downarrow$

#### 2.2.2 AUC

Area Under (ROC) Curve (ROC Curve: receiver operating characteristic )

##### 2.2.2.1.  ROC Curve

<div  align="center"><img src=https://upload.wikimedia.org/wikipedia/commons/6/6b/Roccurves.png style = "zoom:40%"></div>

$FPR = \frac{FP}{FP+TN} = \frac{FP}{N} = \text{significant level}= \alpha =  \text{type I error} $
- Ref: [Wiki-FPR](https://en.wikipedia.org/wiki/False_positive_rate#Comparison_with_other_error_rates):"false positive rate is mathematically equal to the type I error rate"

$TPR = \frac{TP}{TP+FN} = \frac{TP}{P} = \text{recall} = \text{sensitivity}= 1-\beta$
- $\beta:$ type II error rate
- Ref: [Wiki-Sensitivity](https://en.wikipedia.org/wiki/Sensitivity_and_specificity): Sensitivity (True Positive Rate)
<br>

##### 2.2.2.2. Ranking/Probability interpretation:

- AUC provides an aggregate measure of performance across all possible classification thresholds
- AUC is as the probability that the model ranks a random positive example more highly than a random negative example.
- Ref: [Google_ML-AUC](https://developers.google.com/machine-learning/crash-course/classification/roc-and-auc), [CSDN-blog](https://blog.csdn.net/u013385925/article/details/80385873)
<br>

##### 2.2.2.3. Characteristics of AUC (TBD)

- AUC is **threshold-invariant**
  - AUC is not affect by the value of threshold.
  - (Pro) 
  - (Con) Cannot describe the metric TPR/FPR trade-off relation at different threshold
    - E.g. the following two models have exactly AUC, but different TPR/FPR behavior.
    <div  align="center"><img src=https://www.researchgate.net/publication/8636163/figure/fig3/AS:202684352208900@1425335123112/Two-ROC-curves-A-and-B-with-equal-area-under-the-ROC-curve-However-these-two-ROC_W640.jpg style = "zoom:60%"></div>
- AUC is **scale-invariant**
  - AUC only care about the relative ranking of different sample, don't care about absolution score value
  - (Con) Cannot select good model when we need to calibrate the score
    - "calibration" is needed usually when score is probability, we hope to prediction the correct probability, rather than relative ranking.
    - when calibration is needed, usually it is better to use **cross entropy** rather than AUC, cross entropy is **also threshold invariant but not scale-invariant**.
  
- AUC is invariant to class imbalance.


Ref:
- [GoogleML-AUC](https://developers.google.com/machine-learning/crash-course/classification/roc-and-auc): AUC understanding, pro and cons
- [Image](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQkNYtFoC0doRVIkeoZ8tld0tKFHdJ4pS0IIg&usqp=CAU): Same AUC, different TPR/FPR tradeoff
- [ResearchGage](https://www.researchgate.net/publication/8636163_Receiver_Operating_Characteristic_ROC_Curve_Practical_Review_for_Radiologists/figures?lo=1) and [Imaage](https://www.researchgate.net/publication/8636163/figure/fig3/AS:202684352208900@1425335123112/Two-ROC-curves-A-and-B-with-equal-area-under-the-ROC-curve-However-these-two-ROC_W640.jpg): Same AUC, different TPR/FPR tradeoff


##### 2.2.2.4. TBD

- **personal understanding on AUC ROC**:
With the sacrifice of incorrect positive prediction, how much correct positive prediction we can can get.
The high AUC, the higher "sacrifice" efficiency of incorrect prediction, the better model performance.
<br>

- ROC monotonously increase:
Explanation: the moving on ROC curve is actually the moving of threshold. 
  - As threshold decreases, the predicted positive will increase, 
  - $\rightarrow$ The samples previously predicted as false may become true (but prev true will not become false) 
  - $\rightarrow$ Both the TP and FP may increase i.e. TPR and FPR are always positively correlated
  -  $\rightarrow$ $\frac{d TPR}{dFPR}\geq0$ $\rightarrow$ ROC curve monotonously increase.
 <br>

 -  The brown dashed line (---) means the performance of random guess, with AUC = 0.5, regardless of number of pos/neg samples.
 Ref: [Blog](https://blog.csdn.net/elecjack/article/details/79972097)
<br>

##### Threshold determination:

TPR the higher the better (the more correct prediction), FPR the lower the better (the less mis-predictions).

- Maximize "G-mean": $\underset{c}{\text{maximaze}} \sqrt{TPR(c)*(1-FPR(c))}$
  - Ref: [TowardsDataScience](https://towardsdatascience.com/optimal-threshold-for-imbalanced-classification-5884e870c293)
- Self-defined metric like: $\underset{c}{\text{maximaze}} [ TPR(c)-FPR(c) ]$
  - Ref:[StackExchange](https://stats.stackexchange.com/questions/123124/how-to-determine-the-optimal-threshold-for-a-classifier-and-generate-roc-curve)


### 2.3. AUC vs. Precision-Recall:

  - AUC:
    - Pro: 
      - **Not affected by class imbalance**. Ref:[TowardsDataScience?(TBD)](https://towardsdatascience.com/metrics-for-imbalanced-classification-41c71549bbb5)
      Explain: The AUC of random guesser is always 0.5, regardless of balance or not.
      - More comprehensive
      Explain: Have all $TP, FP, TN, FN$
  - Precision-Recall
    - Con:
      - Sensitive to class imbalance. I.e. for same model, precision score changes when ratio (amount) of positive/negative changes.
        - For random guesser, 
          - $TP = \frac{1}{2}P$, $FN = \frac{1}{2}P$, $FP = \frac{1}{2}N$, $TN = \frac{1}{2}N$
          - $\text{precision} = \frac{TP}{TP+FP} = \frac{0.5P}{0.5P+0.5N} = \frac{P}{P+N}$
            $\text{recall} = \frac{TP}{TP+FN} = \frac{0.5P}{P} = 0.5$
        - For single prediction model,
          - $TP = P$, $FN = 0$, $FP = N$, $TN = 0$
          - $\text{precision} = \frac{TP}{TP+FP} = \frac{P}{P+N}$
          $\text{recall} = \frac{TP}{TP+FN} = \frac{P}{P} = 1$
      - Less comprehensive
      Explain: No info of $TN$

- "Separation of classes" [Ref: TowardsDataScience](https://towardsdatascience.com/understanding-auc-roc-curve-68b2303cc9c5)
  - AUC indicates the separability of the classes with our current model.
    
    <div align="center" ><img src=https://miro.medium.com/max/1014/1*yF8hvKR9eNfqqej2JnVKzg.png height="100"> <img src=https://miro.medium.com/max/730/1*-tPXUvvNIZDbqXP0qqYNuQ.png height="100" > </div>
    <div  align="center"  ><img src=https://miro.medium.com/max/1056/1*Uu-t4pOotRQFoyrfqEvIEg.png height="100"> <img src=https://miro.medium.com/max/860/1*iLW_BrJZRI0UZSflfMrmZQ.png height="100" ></div>

    - Note: threshold=0.5 is just a default example, the threshold can also be other values, 
      - E.g. The graph below, threshold is 0.8, AUC is 1. [Ref: StackExchange](https://stats.stackexchange.com/questions/200815/why-auc-1-even-classifier-has-misclassified-half-of-the-samples)
      <div align="center"><img src=https://i.stack.imgur.com/2MEb8.png height="200" > </div>
  - When AUC is smaller than 0.5?
    - AUC = 0.5 $\Rightarrow$ the model is random guesser.
    - AUC < 0.5 $\Rightarrow$ the model is worse than random guesser, i.e. the positive will be predicted as negative, the negative will be predicted as positive.
      - When the model is total reverse to the ground truth, AUC = 0.
    <div align="center"><img src=https://miro.medium.com/max/1112/1*aUZ7H-Lw74KSucoLlj1pgw.png height="100" > </div>


### 2.4. Multi-label classification

There are four different averaging strategy to create multi-label evaluation metric from single-label metrics: Macro-, micro-, weighted-, and sample- averaging.

Ref:[TowardsDataSicne](https://towardsdatascience.com/evaluating-multi-label-classifiers-a31be83da6ea)

#### 2.4.1. Macro averaging

Macro averaging is averaging over different classes.

e.g.: assume there are $k = K$ classes

$\mathrm{prec}(\text{macro}) = \sum^K_{k=1} \mathrm{prec}(C_k)$
where
- $\mathrm{prec}(C_k) = \frac{TP_k}{TP_k+FP_k}$
- $TP_k$ number of TP for k-th label.


#### 2.4.2. Micro averaging

Micro averaging is calculated from sample-level confusion matrix value (TP,FP, TN, FN), following the definition of metric. e.g.:

$\mathrm{prec}(\text{micro}) = \frac{ \sum^K_{k=1} TP_k}{\sum^K_{k=1}TP_k+\sum^K_{k=1}FP_k}$

#### 2.4.3. Weighted averaging

Weight averaging is a weighted version of "macro averaging", i.e. still calculated from class level data but consider its weight in dataset.

$\mathrm{prec}(\text{weighted}) = \sum^K_{k=1}\frac{n_{C_i}}{N} \mathrm{prec}(C_i)$


#### 2.4.4. Sample averaging

Calculate metric for each sample and do average. 

Note: for each individual sample the there only 1 sample, for each class k the confusion matrix value (TP,FP,TN,FN) will be either 0,1.

$\mathrm{prec}(\text{sample}) =\sum^N_{i=1} \mathrm{prec}(x_i) $

$\mathrm{prec}(x_i) = \frac{ \sum^K_{k=1} TP_{k,i}}{\sum^K_{k=1}TP_{k,i}+\sum^K_{k=1}FP_{k,i}}$

Note: Confusion matrix value on class level is the sum of confusion matrix value on sample level,i.e. $TP_k = \sum_i TP_{k,i}$

#### 2.4. Deep Dive: How to handle 0/0? (TBD)

Ref: [StackExchange](https://stats.stackexchange.com/questions/8025/what-are-correct-values-for-precision-and-recall-when-the-denominators-equal-0)

It is possible that we have 0 $TP$ and 0 $FP$ if our classifier predictor all negative. 

Then the precision will be $0/0 = nan$ (e.g. [FastAI-Forum](https://forums.fast.ai/t/nan-values-when-using-precision-in-multi-classification/59767))

How to handle it? Depends on your definition. E.g. when you have nan
- If you use ```np.mean```, it will give you nan
- If you use ```np.nanmean```, it will give you mean value ignore nan
- Ref: [StackOverflow](https://stackoverflow.com/questions/19852586/get-mean-value-avoiding-nan-using-numpy-in-python)

#### Deep Dive: "Base rate invariant" metrics

- [Base_rate](https://en.wikipedia.org/wiki/Base_rate): generally refers to the (base) class probabilities unconditioned on featural evidence, i.e. p(y)

- base-rate-invariant metric: the metric will not change with different p(y) distribution. I.e. the metric is not affected by data imbalance. [Ref: Imbalanced_data](../data_engineering/Imbalanced_data.md)
  - Mechanism:
    - Data imbalance: the ground true label is not equal.
    - For different models, 
      - The PredictedP (TP+FP) varies from model-to-model, 
      - GroundP (=TP+FN) is constant (it only relies on the data)
    - Thus, for genenral metrics 
      - like $Precision = \frac{TP}{TP+FP} = \frac{TP}{Predicted}$
      - the numerator varies from model to model
      - thus it is not ideal for performance comparison.
    - But, for base-rate invariant metrics:
      - like $Recall (TPR) = \frac{TP}{TP+FN} = \frac{TP}{GroundP}$, $FPR = \frac{FP}{GroundN}$, $AUC (ROC) = f(TPR, FPR)$
      - The numerator is invariant from model-to-model
      - thus it is a good metric for performance comparison.






## 3. Ranking

- Precision@k (recall@k, accuracy@k)

## 4. Clustering

- Silhouette score


## 5. Interview questions

### 5.1. Confusion matrix calculation

Q: Calculate precision from the given facts:
- Positive cases 1% over all cases; 
- True positive rate 99%, 
- True negative rate 99%

Ans: 50%

Solution:

- $prec = \frac{TP}{TP+FP}$, to get precision we need to get TP and FP
- Denote the sample size as $S$
  - The we have $P=0.01S, N=0.99S$
- Then:
  - $TPR = \frac{TP}{P} = 0.99$
    - $\Rightarrow TP = 0.99\cdot P = 0.0099S$
  - $TNR = \frac{TN}{N} = 0.99 $
    - $\Rightarrow FPR := \frac{FP}{N} = \frac{N-TN}{N} = 1-0.99=0.01$ 
    - $\Rightarrow FP = 0.01N = 0.0099S$
- So: $prec = \frac{TP}{TP+FP} = \frac{0.0099S}{0.0099S+0.0099S} = 0.5$

Ref:[1p3a](https://www.1point3acres.com/bbs/thread-805418-1-1.html):  Interview-L



