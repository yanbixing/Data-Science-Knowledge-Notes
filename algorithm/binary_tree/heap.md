# Heap

## 1. Basics

### 1.1. Max heap vs Min heap

- Prerequisite: heap should be a "complete binary tree"
- Max heap: parent.val is larger than children.val 
  - So, the largest value are at root.
- Min heap:parent.val is smaller than children.val 
  - So, the smallest value are at root.

<div  align="center"><img src=https://leetcode.com/explore/learn/card/heap/643/heap/4018/../../../../Figures/heap_explore/1_1_min_max_heap_diagram_new.png style = "zoom:80%"></div>

### 1.2 Operations

#### 1.2.1 Heap insert

Use Min heap as example (python heapq default is min-heap)

- Find the position to insert a new node
  - How?(TBD) Because the heap tree must be complete, so we can use BFS, search nodes (parent) layer by layer, and examine the first node.left/right that is None, and that is the position we add node
- if the new node's val is smaller than its parent, swap its value with its parent.
- O complexity:
  - TC: O(H) = O(logN)
  - SC: O(1)
- Ref: 
  - [LC.Learn.Heap](https://leetcode.com/explore/learn/card/heap/643/heap/4019/): knowledge
  - [LC.Learn.Heap](https://leetcode.com/explore/learn/card/heap/644/common-applications-of-heap/4023/): implementation

#### 1.2.2 Heap Deletion

Use Min-heap as example (python heapq default is min-heap)

- Find the last node, swap its value with the root node, and delete it 
  - So the tree is still complete
  - How?
- If the root node's value is larger than its children, swap its value with the smallest children, until node.val is smaller than both children.
- Ref: [LC.learn.heap](https://leetcode.com/explore/learn/card/heap/643/heap/4020/)
- O complexity:
  - TC: O(H) = O(logN)
  - SC: O(1)
- Ref: 
  - [LC.Learn.Heap](https://leetcode.com/explore/learn/card/heap/643/heap/4020/): knowledge
  - [LC.Learn.Heap](https://leetcode.com/explore/learn/card/heap/644/common-applications-of-heap/4025/): implementation

## 2. Implementation of heap

### 2.1. Array representation

<div  align="center"><img src=https://web.cs.wpi.edu/~mebalazs/cs507/slides05/slides-1.gif style = "zoom:80%"></div>

- The array representation of heap is in **level-order**:
  - e.g. array = [level_1_nodes] + [level_2_nodes] +... [level_n_nodes]
  - The nodes in a level is ordered from left to right.
- Ref: [GeeksForGeeks](https://www.geeksforgeeks.org/array-representation-of-binary-heap/)

- The array <-> heap binary tree have:
  - when start index is 1
    - [index of node.parent] = [index of node] // 2
    - [index of node.left] = [index of node] * 2
    - [index of node.right] = [index of node] * 2 + 1
    - Ref: [LC.learn.heap.implementation](https://leetcode.com/explore/learn/card/heap/643/heap/4017/)
  - when start index is 0 (python):
    - [index of node.parent] = ([index of node]-1) // 2
    - [index of node.left] = [index of node] * 2 + 1
    - [index of node.right] = [index of node] * 2 + 2
- Proof (idx start from 0):
  - the j-th element in i-th level should have the index:
    - i = 1: $\text{arr\_idx}(i,j) = j-1$
    - i>1:
      $\begin{aligned}
        \text{arr\_idx}(i,j) &= 2^0+2^1+\dots+2^{i-2} + j - 1\\
        &= \frac{1-2^{i-1} }{1-2} + j - 1\\
        &= (2^{i-1}-1) + j-1
      \end{aligned}$ 
    - e.g.:
      $\begin{aligned}
        &\text{arr\_idx}(1,1) = 0,\\
        &\text{arr\_idx}(2,1) = 1, \text{arr\_idx}(2,2) = 2\\
        &\text{arr\_idx}(3,1)=3,\text{arr\_idx}(3,2)=4,\dots
      \end{aligned}$
  - the children of $\text{arr\_idx}(i,j)$ should be 
    - left:
      $\begin{aligned}
        &\text{arr\_idx}(i+1,2j-1)\\
        =& (2^{i}-1) + (2j-1) - 1\\
        =& 2 (2^{i-1}-1) + 1 +   (2j-1) - 1\\
        =& 2 (2^{i-1}-1) + 2j-1 \\
        =& 2 [(2^{i-1}-1) + j-1]+1 \\
        =& 2\text{arr\_idx}(i,j) + 1
      \end{aligned}$ 
    - right: 
      $\begin{aligned}
        &\text{arr\_idx}(i+1,2j)\\
        =& 2\text{arr\_idx}(i,j) + 2
      \end{aligned}$ 
    - validate: 
      - arr[0]'s children is arr[1], arr[2]
      - arr[1]'s children is arr[3], arr[4]
      - arr[2]'s children is arr[5], arr[6]
  - reversely:
    - the parent of arr[i] is arr[ (i-1)//2 ] 


## 3. Application of heap

### 3.1. Basics operations

[python - heapq package](https://docs.python.org/3/library/heapq.html): how to use heapq package.
[My_algo_notebook](https://colab.research.google.com/drive/1f1fXyEhhpHOp_XCX7qOMrtR-kxXvtl0F?usp=sharing): the notebook we test.

#### 3.1.1 Construct heap

```python
# heapify
import heapq
min_heap = [5,4,3,2,1] #raw_list
heapq.heapify(min_heap) # heapq.heapify is in-place operation, TC: O(N), SC: O(N)(taken by list, extra O(1))
min_heap, type(min_heap) # print: [1, 2, 3, 5, 4], list
```

- Q: why heapify is O(N)?
  -  proof: 
     -  [StackOverflow](https://stackoverflow.com/questions/51735692/python-heapify-time-complexity): math derivation
     -  [StackOverflow](https://stackoverflow.com/questions/9755721/how-can-building-a-heap-be-on-time-complexity): heapify is NOT building heap by insert
        -  If build by insert one by one (Williams’ method), then it is O(NlogN) [Wiki-heap](https://en.wikipedia.org/wiki/Binary_heap)
        -  For each node from the end (the parent of last nodes), execute a "Min-Heapify" function
           -  "Min-Heapify" function: send the root to the leave node until it is smaller than both leaves, on one node, the complexity is O(h), (h=0,1,2,3....)
           -  Num nodes on h-th layer $n(h) \leq \frac{N}{2^h}$
           -  $\sum^{\log N}_{h=0} \frac{N}{2^h} O(h) = O(N\sum^{\log N}_{h=0} \frac{h}{2^h}) = O(N)$ 
           -  Ref: [Wiki-heap-build_a_heap](https://en.wikipedia.org/wiki/Binary_heap#Building_a_heap)
     -  [StackOverflow](https://stackoverflow.com/questions/27888321/build-heap-time-complexity-worst-case-vs-upper-bound-tight-upper-bound): always O(N)
  -  Personal understand: heapify is not sorted, so the complexity requirement is smaller than sort.

#### 3.1.4. Get top element & get length

```python
# after heapify, the object is still list, so list operations is ok.
# get top (smallest) element, get_length
min_heap[0], len(min_heap) # print: 1, 5  TC: O(1),  SC: O(1)
```

#### 3.1.2. Insert an element

```python
# insert
heapq.heappush(min_heap, -1) # inplace operation, TC: O(H=logN),  SC: O(1)
heapq.heappush(min_heap, 2.5)
heapq.heappush(min_heap, 7)
min_heap # print: [-1, 2, 1, 5, 4, 3, 2.5, 7]
```

#### 3.1.3. Delete top element

```python
# pop top (smallest) element
heapq.heappop(min_heap) # inplace operation: TC: O(H=logN),  SC: O(1)
# Note: cannot use min_heap.pop(), 
#       it will pop out the last element 7, 
#       because type(min_heap) is still List object
min_heap
```





### 3.2 Typical tasks

#### 3.2.1 Heap Sort

- Algo: keep popping the smallest element in the heap. 
  - TC: O(NlogN)
    - For each pop operation, the complexity is O(logN), so the total complexity is O(NlogN).
  - SC：O(N), O(1) if heapify in-place

```python
# heap_sort TC:O(NlogN) SC: O(N), O(1) if in-place
import heapq

def heapsort(arr):
  min_heap = arr[:] # copy arr, avoid revising arr object
  heapq.heapify(min_heap) # TC: O(N), SC: O(N)
  ans = []
  for i in range(len(min_heap)): # TC:O(NlogN)
    ans.append( heapq.heappop(min_heap) ) # TC:O(logN)
  return ans

arr = [5,1,3,2,4]
heapsort(arr) # print([1, 2, 3, 4, 5])
```

#### 3.2.2 Top K item

- Algo: keep popping the smallest element in the heap until k.
  - TC: O(KlogN + N)
    - For each pop operation, the complexity is O(logN), so the total complexity is O(NlogN).
    - Heapify: O(N)
  - SC：O(N), O(1) if heapify in-place

```python
# Top K smallest items
import heapq

# self-defined function or directly call heapq.nlargest(k,arr), heapq.nsmallest(k,arr)
def heap_k_smallest(arr,k):
  min_heap = arr[:] 
  heapq.heapify(min_heap) # TC: O(N), SC: O(N), O(1) if in-place
  ans = []
  for i in range(k): # TC:O(NlogN)
    ans.append( heapq.heappop(min_heap) ) # TC:O(logN)
  return ans

arr = [5,1,3,2,4]
k = 3
heap_k_smallest(arr,k), heapq.nsmallest(k,arr) # print: [1, 2, 3], [1, 2, 3] in ascending order
```

#### 3.2.3 K-th element

- Algo: keep popping the smallest element in the heap until k.
  - TC: O(KlogN + N)
    - For each pop operation, the complexity is O(logN), so the total complexity is O(NlogN).
    - Heapify: O(N)
  - SC：O(N), O(1) if heapify in-place

```python
# K-th smallest item
import heapq

# self-defined function or directly call heapq.nlargest(k,arr), heapq.nsmallest(k,arr)
def heap_kth_smallest_val_idx(arr,k):
  min_heap = [(val,idx) for idx,val in enumerate(arr)] # we can also get idx
  heapq.heapify(min_heap) # TC: O(N), SC: O(N), O(1) if in-place
  ans = []
  for i in range(k): # TC:O(KlogN)
    ans = heapq.heappop(min_heap) # TC:O(logN)
  return ans

arr = [5,1,3,2,4]
k = 2

kth_smallest_val,idx = heap_kth_smallest(arr,k)
kth_smallest_val, (idx, arr[idx]), heapq.nsmallest(k,arr)[-1] # print: 2, (idx = 3, arr[idx] = 2), 2
```

### 3.3. Practice questions

[LC.Learn.Heap](https://leetcode.com/explore/learn/card/heap/646/practices/4014/)