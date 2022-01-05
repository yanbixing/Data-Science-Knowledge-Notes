# Tree algos

## 1. structure basics

### 1.1. Complete binary tree:

  - In each level except the last one, the level is completely filled
  - In the last level, the tree is filled from left to right, no None value
  - <div  align="center"><img src=https://web.cecs.pdx.edu/~sheard/course/Cs163/Graphics/CompleteBinary.jpg style = "zoom:80%"></div>
- How to verify: BFS 
  - e.g. [LC.958](https://leetcode.com/problems/check-completeness-of-a-binary-tree/), TC: O(N), SC: O(N)
- Ref: [PortlandStateU-CS163](https://web.cecs.pdx.edu/~sheard/course/Cs163/Doc/FullvsComplete.html)


### 1.2. Full binary tree

- Every nodes except the leaves have two children
- The leave nodes have no children.
- <div  align="center"><img src=https://web.cecs.pdx.edu/~sheard/course/Cs163/Graphics/FullBinary.jpg style = "zoom:80%"></div>



### 1.3. Binary search tree

- Every node satisfy (ignore if .val is None):
  - left_subtree_vals < node.val < right_subtree_vals
    - Deduction1: node.left.val < node.val < node.right.val
    - Deduction2: So in-order traversal will give ascending (non-descending) array.
  - <div  align="center"><img src=https://www.tutorialspoint.com/data_structures_algorithms/images/binary_search_tree.jpg style = "zoom:80%"></div>
- How to verify: bottom-up recursive:
  - [LC.98](https://leetcode.com/problems/validate-binary-search-tree/): recursively return the subtree_min/max of node.right/left, and compare it with node.val.
  - TC: O(N)
  - SC: O(H)
    - avg-O(logN): when tree is complete, H~logN
    - max-O(N): when tree is like a "line": H~N

- Ref: 
  - [tutorialspoint-BST](https://www.tutorialspoint.com/data_structures_algorithms/binary_search_tree.htm)

## 2. Traversal (TBD)

### 2.1. Pre-order

- Preorder traversal can be used to store/rebuild binary tree.
  - E.g. [LC.297](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/): Interview Q of L, ref: [1p3a](https://www.1point3acres.com/bbs/thread-711352-1-1.html)

### 2.2. In-order

#### 2.2.1. In-order and BST

In-order traversal on BST will give non-descending array.

- Application e.g.
  - [LC.501](https://leetcode.com/problems/find-mode-in-binary-search-tree/): get mode in BST

And if in-order traversal give non-descending array, the tree must be BST.

- i.e. Tree is BST $\Leftrightarrow$ in-order traversal give non-descending array
- Proof ref:
  - [StackOverflow](https://stackoverflow.com/questions/10170941/how-to-verify-if-a-given-tree-is-a-binary-search-tree-or-not): inorder traversal ascending is sufficient condition for BST
  - [Quora](https://www.quora.com/Can-a-binary-tree-have-a-sorted-inorder-traversal-sequence-without-being-a-BST): if inorder traversal ascending, must be BST, cannot be non-BST
  - [StackExchange](https://cs.stackexchange.com/questions/95329/if-inorder-traversal-of-a-tree-is-in-ascending-order-will-the-tree-definitely-be):intuitive proof why inorder traversal ascending => BST
- Application e.g.
  - [LC.98](https://leetcode.com/problems/validate-binary-search-tree/): identify/validate BST


### 2.3. Post-order
