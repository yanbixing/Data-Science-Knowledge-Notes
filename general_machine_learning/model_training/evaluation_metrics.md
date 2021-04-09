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

- $\text{precision} = \frac{TP}{TP+FP}$
English: Among the predicted positive, how many are correct (condition positive $P$ ).
- $\text{recall} = \frac{TP}{TP+FN} = TPR = \text{sensitivity}$
English: Among the condition positive ($P$), how may are correct (predicted positive).
- $\text{f1} = \frac{1}{\frac{ \frac{1}{\text{precision}} + \frac{1}{Recall} }{2}} = \frac{2\cdot \text{precision} \cdot \text{recall}}{\text{precision} + \text{recall}}$
English: The harmonic mean of precision and recall
- $\text{accuracy} = \frac{TP+TN}{P+N} $ 
  - For balanced dataset, $\text{accuracy}\overset{P=N}{\rightarrow} \frac{1}{2} (\frac{TP}{P} + \frac{TN}{N}) = \frac{1}{2}(TPR+TNR)$
  - $TNR = \frac{TN}{N}$
- Ref: [Blog](https://www.springboard.com/blog/machine-learning-interview-questions/)

#### 2.2.2 AUC

Area Under (ROC) Curve (ROC Curve: receiver operating characteristic )

- ROC Curve
  <div  align="left"><img src=https://upload.wikimedia.org/wikipedia/commons/6/6b/Roccurves.png style = "zoom:40%"></div>

  $FPR = \frac{FP}{FP+TN} = \frac{FP}{N}$

  $TPR = \frac{TP}{TP+FN} = \frac{TP}{P} = \text{recall} = \text{sensitivity}$
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
      - Not affected by class imbalance.
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
          - $TP = P $, $FN = 0 $, $FP = N$, $TN = 0$
          - $\text{precision} = \frac{TP}{TP+FP} = \frac{P}{P+N}$
          $\text{recall} = \frac{TP}{TP+FN} = \frac{P}{P} = 1$
      - Less comprehensive
      Explain: No info of $TN$

## 3. Ranking

- Precision@k (recall@k, accuracy@k)

## 4. Clustering

- Silhouette score