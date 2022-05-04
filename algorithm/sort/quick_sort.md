# Quick Sort

## 1. Quick Sort

## 1.1. Idea

## 1.2. Implementation

### 1.3. complexity

- TC:
  - Worst: O(N^2)
  - Best: O(NlogN)
- SC:
  - Extra O(1), because its in-place


- Ref: 
  - [GeeksForGeeks](https://www.geeksforgeeks.org/quick-sort/): implementation, complexity deduction, etc.

## Quick selection

- Ref:
  - [GeeksForGeeks](https://www.geeksforgeeks.org/quickselect-algorithm/): implementation, complexity, etc.
  - [Wiki-Quick_select](https://en.wikipedia.org/wiki/Quickselect)
  - [GeeksForGeeks](https://www.geeksforgeeks.org/kth-smallestlargest-element-unsorted-array-set-2-expected-linear-time/?ref=lbp): quick selection, expected linear time
  - [GeeksForGeeks](https://www.geeksforgeeks.org/kth-smallestlargest-element-unsorted-array-set-3-worst-case-linear-time/):median of median, worst linear time.
  - [Wiki-MedianOfMedian](https://en.wikipedia.org/wiki/Median_of_medians)


### Complexity

Consider of permutation cases.

- Average: O(N)
  - Proof: [Blog](https://rcoh.me/posts/linear-time-median-finding/)
  - Idea: on average, the left size to check = n/2
  - TC = N + N/2 + N/4 +... = N (1+ 0.5 + 0.5^2 +...) = N 1/(1-0.5) ～ 2N = O(N) 
- Worst: O(N^2)
  - Each time just eliminate one element
  - TC = N + (N-1) + (N-2)... = N(N-1)/2 = O(N^2


### Median of Median (TBD)

Use the median of medians a 








