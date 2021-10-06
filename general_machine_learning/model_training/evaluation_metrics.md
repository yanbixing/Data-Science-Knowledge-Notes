# Evaluation metrics

## 1. Regression:

- Mean square error (MSE)
- Mean absolute error (MAE)

## 2. Classification:

### 2.1. Entropy-based

- Information gain
- Mutual info

### 2.2. TP,FP,TN,FN-based

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

#### 2.2.2 AUC

Area Under (ROC) Curve (ROC Curve: receiver operating characteristic )

- ROC Curve
  <div  align="center"><img src=https://upload.wikimedia.org/wikipedia/commons/6/6b/Roccurves.png style = "zoom:40%"></div>

  $FPR = \frac{FP}{FP+TN} = \frac{FP}{N} = \text{significant level}= \alpha =  \text{type I error} $
  - Ref: [Wiki-FPR](https://en.wikipedia.org/wiki/False_positive_rate#Comparison_with_other_error_rates):"false positive rate is mathematically equal to the type I error rate"

  $TPR = \frac{TP}{TP+FN} = \frac{TP}{P} = \text{recall} = \text{sensitivity}= 1-\beta$
  - $\beta:$ type II error rate
  - Ref: [Wiki-Sensitivity](https://en.wikipedia.org/wiki/Sensitivity_and_specificity): Sensitivity (True Positive Rate)

  <br>

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

- AUC vs. Precision-Recall:
  - AUC:
    - Pro: 
      - **Not affected by class imbalance**.
      Explain: The AUC of random guesser is always 0.5, regardless of balance or not.
      - More comprehensive
      Explain: Have all $TP, FP, TN, FN$
  - Precision-Recall
    - Con:
      - Sensitive to class imbalance.
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


### Deep Dive "Base rate invariant" metrics

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


