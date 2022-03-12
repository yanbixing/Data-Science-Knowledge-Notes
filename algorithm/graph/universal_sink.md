# Universal sink

**Definition:** A universal sink is a vertex which has no edge emanating from it, and all other vertices have an edge towards the sink. Ref: [GeeksForGeeks](https://www.geeksforgeeks.org/determine-whether-universal-sink-exists-directed-graph/)

The task is usually to find the universal sink.

## 1. Matrix representation

Graph can be represented with matrix. We can design algorithm to find universal sink based on its unique geometric patten on the matrix.

### 1.1. Matrix representation of universal sink.

Ref: [StackOverflow](https://stackoverflow.com/questions/29259365/how-to-find-the-universal-sink-of-a-directed-graph-with-an-adjacency-matrix-repr)

Use a $N \times N$ matrix $G$ to denote the directed edge between $N$ different vertices. 

- $G[i][j]$ represent whether there is an edge from $i\to j$

Then, for a graph that have a universal sink, denoted as $K$. The matrix must satisfy:

- All element in the k-th row is 0. 
  - I.e. $G[K][i] = 0 $, $ \forall i\in[0,N-1]$
- All element except $G[K][K]$ in the col is 1.
  - I.e. $G[i][K] = 1$, $\forall i\in[0,N-1]$ except $i = K$

The matrix looks like: 
```
            k
  +---+---+---+---+---+
  |   |   | 1 |   |   |
  +---+---+---+---+---+
  |   |   | 1 |   |   |
  +---+---+---+---+---+
k | 0 | 0 | 0 | 0 | 0 |
  +---+---+---+---+---+
  |   |   | 1 |   |   |
  +---+---+---+---+---+
  |   |   | 1 |   |   |
  +---+---+---+---+---+
```

### 1.2. Algorithm based on geometric pattern

#### 1.2.1. Main algorithm:

When there is a universal sink, the matrix will look like above, then, we can use the following algorithm to find the $k$ value.
```
Start from i=0,j=0
while j<N:
    if G[i][j] is 0: 
        move rightward (j++)
    if cur G[i][j] is 1:
        move downward (i++) 
return i
```

#### 1.2.2. Why the algo works:

**Programmatically:** 

- No matter how i,j moves, i,j will always move downward $\downarrow$ or rightward $\rightarrow$ for 1 step, until i = N or j = N
  - so one of i,j must reach k first, this is inevitable.
- Then, there are two cases (exhaustive)
  - if i reach k first before j reach k,
    - since the whole row is 0, it will always move rightward $\rightarrow$ in the following steps, 
    - so when the pointer (i,j) stop, it will stop at (k,N)
    - i will be the answer k.
  - if j reach k first before i reach k, 
    - since the whole column is 1, it will always move downward $\downarrow$ (i++) until i=k
    - after i=k, (i,j) will only move rightward and stop at (k,N)
    - i will be the answer k

**Intuitively:** 

- the '1' in k-th col should behave as a single-channel filter
- the pointer (i,j) alway want to move rightward if possible, 
  - if not, it will move downward
- so the only way tha the pointer can penetrate the '1' wall at k-th column is the k-th row
  - As long as pointer (i,j) reach k-th row, it will slip straight to the right most and the program stops
- so, following the algo, we should give i=k

#### 1.2.3. What if no universal sink?

<mark style="background-color:yellow;"><font color="#0000dd">The above algo is **correct only when there is a universal sink**. What if there is no universal sink?</font></mark>

Since the case "There is a universal sink in G" and case "There is no universal sink in G" are "collectively exhaustive" ([Wiki-Collectively_exhaustive](https://en.wikipedia.org/wiki/Collectively_exhaustive_events))

<mark style="background-color:yellow;"><font color="#0000dd">We just need to verify whether our result k is a true universal sink or not.</font></mark>

- <mark style="background-color:yellow;"><font color="#0000dd">If $k$ is a true universal sink</font></mark>, it is the ans (the only universal node) and it is safe to <mark style="background-color:yellow;"><font color="#0000dd">return $k$</font></mark>.
  - Note: there should be at most 1 universal sink, cannot be more, easy to prove: 
    - universal sink require all other nodes points to it but it points no nodes.
    - if there are two nodes k1, k2 point to no nodes, then, k1 receive no edge from k2 and k2 receive no edge from k1
    - because both of them cannot satisfy "universal sink require all other nodes points to it"
    - both of them couldn't be universal edge
- <mark style="background-color:yellow;"><font color="#0000dd">If $k$ is not a true universal sink</font></mark>, there must be NO such pattern (k-th row all 0, k-th coll all 1 except k-th row) in the matrix, then, <mark style="background-color:yellow;"><font color="#0000dd">there is no universal sink at all</font></mark>.
  - Proof by contradiction: 
    - if there is "such pattern" (k-th row all 0, k-th coll all 1 except k-th row), then the k given by algo must be the true universal sink. 
    - So, k given by the algo is not true universal sink, there must be no "such pattern" (i.e. there is no universal sink.)

### 1.3. Algorithm implementation

```python
def find_universal_sink(G):
    """Algo to find universal sink."""
    # Main part, i=k if there is a universal sink k in G.
    N = len(G)
    i, j = 0,0
    while j < N:
        if G[i][j] == 0:
            j+=1
        else:
            i+=1
            if i == N: return -1 
    
    # Check whether k is really a universal sink or not.
    if is_universal_sink(G,i):
        return i
    else:
        return -1

def is_universal_sink(G,k):
    """Check whether the k-th node is really universal sink or not."""
    N = len(G)

    for j in range(N):
        if G[i][k] != 0:
            return False

    for i in range(N):
        if i==k:
            continue
        else:
            if G[i][k] != 1:
                return False
                
    return True
```

## 2. Functional representation

The info of edge can be also provide in a functional way, where we don't have access to the whole matrix at once but could query the edge info by providing node info. More like we are using a sparse matrix.

### 2.1. Functional represetation of the graph

Ref: [LC.277](https://leetcode.com/problems/find-the-celebrity/): find-the-celebrity
- Solution ref: Official solution 2

Define a function to provide the edge info: 
```python
def a_to_b(a,b) -> bool:
    """Return True if there is an edge a->b"""
    return G[a][b] # actually, what a_to_b return is just G[a][b]
```

### 2.2. Algorithm based on the property of universal sink

#### 2.2.1 Algorithm implementation

Ref: [LC.277](https://leetcode.com/problems/find-the-celebrity/): find-the-celebrity
- Solution ref: Official solution 2

```python
def find_universal_sink(N) -> int:
    p=0
    for i in range(1,N):
        if a_to_b(p, i): 
            p = i
    
    if is_universal_sink(p):
        return p
    else:
        return -1
        

def is_universal_sink(k):
    for i in range(N):
        if i == k: continue
        else:
            if a_to_b(k, i):
                return False
            if not a_to_b(i, k):
                return False
    return True
```

#### 2.2.2. Why the algorithm works?

- Following the above algo, during the iteration:
  - when $i<k$，
    - assume $p = x$ before execute ```if a_to_b(p, i): p=i```, 
    - then after execute ```if a_to_b(p, i): p=i```
      - 1, stop at $x$, don't move
      - 2, point to $i$, i.e $p=i$
    - However, whatever action it take, since i not iterated to k, x must $<k$, and $i<k$
      - so p must $<k$
  - Then, when i reach k, 
    - Since all vertices must point to k
    - p must stop at k
  - When i >k:
    - Because, previously p fall into k, and there is should no outgoing edge from k
    - p must stop at k
- So, as a conclusion，if there is a universal sink k，p must stop at k.
  - Until here, the complexity is O(N)
- However, similar to the above section, there may be no universal sink, the result may be false, so we need to verify it is a true universal sink:
  - universal sink should points to no vertex: O(N)
  - all other vertices should points to universal sink O(N)
- The TC is O(3N).


### 2.3 Matrix understanding on the algorithm

#### 2.3.1 why the algorithm works

```
            k
  +---+---+---+---+---+
  |   |   | 1 |   |   |
  +---+---+---+---+---+
  |   |   | 1 |   |   |
  +---+---+---+---+---+
k | 0 | 0 | 0 | 0 | 0 |
  +---+---+---+---+---+
  |   |   | 1 |   |   |
  +---+---+---+---+---+
  |   |   | 1 |   |   |
  +---+---+---+---+---+
```

**Why the algo works:** From row position $p \to 0$, scan over the row from $j =0 \to N-1$, if there is any $G[p][j]=1$, we will relocate $p \to j$.

- When $j<k$, no matter whether $G[p][j]=$ 1 or 0, the pointer $(p,j)$ will stay in the square ($row<k, col<k$)   
  - if $G[p][j]=1$, pointer will move to $[p][p]$; $\because p<k$, so $[p][p]$ is within square ($row<k, col<k$) 
  - if $G[p][j]=1$, pointer remain $[p][j]$; $\because p,j<k$, so $[p][j]$ is within square ($row<k, col<k$) 
- When $j=k$, since all $G[p][k]==1 \forall p<k$, so definitely, ```p=j``` will be executed and pointer will move to $[k][k]$
- After pointer fall into row k, all $G[k][j]=0$, so the $p$ will not be assigned with other values anymore, then k will be the final answer.

#### 2.3.2. Algorithm implementation

```python
def find_universal_sink(G) -> int:
    N = len(G)
    p=0
    for j in range(1,N):
        if G[p][j]: 
            p = j
    
    if is_universal_sink(p):
        return p
    else:
        return -1
        

def is_universal_sink(G,k):
    N=len(G)
    for i in range(N):
        if i == k: continue
        else:
            if G[k][i]:
                return False
            if not G[i][k]:
                return False
    return True
```