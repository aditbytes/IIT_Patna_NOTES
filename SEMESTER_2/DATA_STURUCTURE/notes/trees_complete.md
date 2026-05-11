# Tree Data Structures — Basic to Advanced

---

## Table of Contents

1. [What is a Tree?](#1-what-is-a-tree)
2. [Tree Terminology](#2-tree-terminology)
3. [Types of Trees](#3-types-of-trees)
4. [Binary Tree — Implementation & Traversals](#4-binary-tree--implementation--traversals)
5. [Binary Tree — Common Operations](#5-binary-tree--common-operations)
6. [Binary Search Tree (BST)](#6-binary-search-tree-bst)
7. [Heap & Priority Queue](#7-heap--priority-queue)
8. [AVL Tree (Self-Balancing BST)](#8-avl-tree-self-balancing-bst)
9. [Segment Tree](#9-segment-tree)
10. [Trie (Prefix Tree)](#10-trie-prefix-tree)
11. [Lowest Common Ancestor (LCA)](#11-lowest-common-ancestor-lca)
12. [Important Tree Problems](#12-important-tree-problems)
13. [Complexity Summary](#13-complexity-summary)

---

## 1. What is a Tree?

A **Tree** is a hierarchical, non-linear data structure of **nodes** connected by **edges**. It has exactly one **root** node, and every non-root node has exactly one **parent**.

A tree with $n$ nodes has exactly $n - 1$ edges.

```
          1          ← root
         / \
        2   3        ← internal nodes
       / \   \
      4   5   6      ← leaves
```

---

## 2. Tree Terminology

| Term | Definition |
|------|------------|
| **Root** | The topmost node (no parent) |
| **Parent** | Node directly above another |
| **Child** | Node directly below another |
| **Leaf** | Node with no children |
| **Internal node** | Node with at least one child |
| **Sibling** | Nodes sharing the same parent |
| **Ancestor** | Any node on the path from root to a node |
| **Descendant** | Any node in the subtree of a node |
| **Depth of node** | Number of edges from root to that node |
| **Height of node** | Number of edges from that node to its deepest leaf |
| **Height of tree** | Height of the root |
| **Level** | Depth + 1 (root is level 1) |
| **Degree** | Number of children of a node |
| **Subtree** | A node and all of its descendants |

---

## 3. Types of Trees

| Type | Key Property |
|------|-------------|
| **General Tree** | Each node can have any number of children |
| **Binary Tree** | Each node has at most 2 children (left, right) |
| **Full Binary Tree** | Every node has 0 or 2 children |
| **Complete Binary Tree** | All levels filled except possibly the last (filled left to right) |
| **Perfect Binary Tree** | All internal nodes have 2 children; all leaves at same depth |
| **Balanced Binary Tree** | Height difference of left/right subtrees ≤ 1 for every node |
| **Binary Search Tree (BST)** | Left subtree < node < right subtree |
| **AVL Tree** | Self-balancing BST; balance factor ∈ {-1, 0, 1} |
| **Red-Black Tree** | Self-balancing BST with color properties |
| **Heap** | Complete binary tree satisfying heap-order property |
| **Trie** | Tree for storing strings; each path = a prefix |
| **Segment Tree** | Tree for range queries on arrays |
| **B-Tree** | Balanced tree optimised for disk access (databases) |

### Binary Tree Properties

$$\text{Max nodes at level } l = 2^{l-1}$$
$$\text{Max nodes in tree of height } h = 2^{h+1} - 1$$
$$\text{Min height for } n \text{ nodes} = \lfloor \log_2 n \rfloor$$
$$\text{Leaf nodes} = \text{nodes with 2 children} + 1$$

---

## 4. Binary Tree — Implementation & Traversals

### 4.1 Node and Tree Class

```python
class Node:
    def __init__(self, value):
        self.value = value
        self.left  = None
        self.right = None

# Build example tree
#         1
#        / \
#       2   3
#      / \   \
#     4   5   6

root = Node(1)
root.left        = Node(2)
root.right       = Node(3)
root.left.left   = Node(4)
root.left.right  = Node(5)
root.right.right = Node(6)
```

---

### 4.2 Recursive Traversals

**Inorder — Left → Root → Right** (gives sorted order for BST)

```python
def inorder(root):
    if root:
        inorder(root.left)
        print(root.value, end=' ')
        inorder(root.right)
# Output: 4 2 5 1 3 6
```

**Preorder — Root → Left → Right** (used to copy/serialize a tree)

```python
def preorder(root):
    if root:
        print(root.value, end=' ')
        preorder(root.left)
        preorder(root.right)
# Output: 1 2 4 5 3 6
```

**Postorder — Left → Right → Root** (used to delete a tree, evaluate expression trees)

```python
def postorder(root):
    if root:
        postorder(root.left)
        postorder(root.right)
        print(root.value, end=' ')
# Output: 4 5 2 6 3 1
```

---

### 4.3 Level-Order Traversal (BFS)

```python
from collections import deque

def level_order(root):
    if not root:
        return []
    result = []
    queue  = deque([root])
    while queue:
        node = queue.popleft()
        result.append(node.value)
        if node.left:  queue.append(node.left)
        if node.right: queue.append(node.right)
    return result

print(level_order(root))  # [1, 2, 3, 4, 5, 6]
```

**Level-by-level (each level as a separate list):**

```python
def level_order_levels(root):
    if not root:
        return []
    result = []
    queue  = deque([root])
    while queue:
        level_size = len(queue)
        level      = []
        for _ in range(level_size):
            node = queue.popleft()
            level.append(node.value)
            if node.left:  queue.append(node.left)
            if node.right: queue.append(node.right)
        result.append(level)
    return result

# [[1], [2, 3], [4, 5, 6]]
```

---

### 4.4 Iterative Traversals

**Iterative Inorder (using explicit stack):**

```python
def inorder_iterative(root):
    result, stack = [], []
    curr = root
    while curr or stack:
        while curr:
            stack.append(curr)
            curr = curr.left
        curr = stack.pop()
        result.append(curr.value)
        curr = curr.right
    return result
```

**Iterative Preorder:**

```python
def preorder_iterative(root):
    if not root: return []
    result, stack = [], [root]
    while stack:
        node = stack.pop()
        result.append(node.value)
        if node.right: stack.append(node.right)  # right first (processed later)
        if node.left:  stack.append(node.left)
    return result
```

---

### 4.5 Traversal Summary

| Traversal | Order | Key Use |
|-----------|-------|---------|
| Inorder | L → Root → R | Sorted output (BST) |
| Preorder | Root → L → R | Copy / serialise tree |
| Postorder | L → R → Root | Delete tree, expression eval |
| Level-order | Level by level | BFS, shortest path, serialise |

---

## 5. Binary Tree — Common Operations

### 5.1 Height of Tree

```python
def height(root):
    if root is None:
        return -1          # -1 for height; use 0 for depth count
    return 1 + max(height(root.left), height(root.right))

print(height(root))  # 2
```

### 5.2 Count Nodes

```python
def count_nodes(root):
    if root is None:
        return 0
    return 1 + count_nodes(root.left) + count_nodes(root.right)
```

### 5.3 Count Leaf Nodes

```python
def count_leaves(root):
    if root is None:
        return 0
    if root.left is None and root.right is None:
        return 1
    return count_leaves(root.left) + count_leaves(root.right)
```

### 5.4 Search in Binary Tree

```python
def search(root, key):
    if root is None:
        return False
    if root.value == key:
        return True
    return search(root.left, key) or search(root.right, key)
```

### 5.5 Insert (Level-Order — fills tree left to right)

```python
from collections import deque

def insert(root, value):
    new_node = Node(value)
    if root is None:
        return new_node
    queue = deque([root])
    while queue:
        node = queue.popleft()
        if node.left is None:
            node.left = new_node
            return root
        queue.append(node.left)
        if node.right is None:
            node.right = new_node
            return root
        queue.append(node.right)
    return root
```

### 5.6 Mirror / Invert a Binary Tree

```python
def invert(root):
    if root is None:
        return None
    root.left, root.right = invert(root.right), invert(root.left)
    return root
```

### 5.7 Check if Two Trees are Identical

```python
def is_identical(t1, t2):
    if t1 is None and t2 is None:
        return True
    if t1 is None or t2 is None:
        return False
    return (t1.value == t2.value and
            is_identical(t1.left, t2.left) and
            is_identical(t1.right, t2.right))
```

### 5.8 Diameter of a Binary Tree

Diameter = longest path between any two nodes (may not pass through root).

```python
def diameter(root):
    max_d = [0]

    def height(node):
        if node is None:
            return 0
        lh = height(node.left)
        rh = height(node.right)
        max_d[0] = max(max_d[0], lh + rh)
        return 1 + max(lh, rh)

    height(root)
    return max_d[0]
```

### 5.9 Check Balanced Binary Tree

```python
def is_balanced(root):
    def check(node):
        if node is None:
            return 0
        lh = check(node.left)
        if lh == -1: return -1
        rh = check(node.right)
        if rh == -1: return -1
        if abs(lh - rh) > 1:
            return -1
        return 1 + max(lh, rh)

    return check(root) != -1
```

---

## 6. Binary Search Tree (BST)

A BST maintains the **ordering property**: for every node,  
$$\text{all values in left subtree} < \text{node.key} < \text{all values in right subtree}$$

```
        8
       / \
      3   10
     / \    \
    1   6   14
       / \  /
      4  7 13
```

Inorder traversal always gives **sorted order**: `1 3 4 6 7 8 10 13 14`

---

### 6.1 BST Node & Operations Complexity

| Operation | Average | Worst (skewed) |
|-----------|---------|----------------|
| Search | $O(\log n)$ | $O(n)$ |
| Insert | $O(\log n)$ | $O(n)$ |
| Delete | $O(\log n)$ | $O(n)$ |
| Min / Max | $O(\log n)$ | $O(n)$ |
| Inorder (sorted) | $O(n)$ | $O(n)$ |

```python
class BSTNode:
    def __init__(self, key):
        self.key   = key
        self.left  = None
        self.right = None
```

### 6.2 Search

```python
def bst_search(root, key):
    if root is None or root.key == key:
        return root
    if key < root.key:
        return bst_search(root.left, key)
    return bst_search(root.right, key)

# Iterative (preferred — no recursion stack)
def bst_search_iter(root, key):
    while root:
        if key == root.key:   return root
        elif key < root.key:  root = root.left
        else:                 root = root.right
    return None
```

### 6.3 Insert

```python
def bst_insert(root, key):
    if root is None:
        return BSTNode(key)
    if key < root.key:
        root.left  = bst_insert(root.left, key)
    elif key > root.key:
        root.right = bst_insert(root.right, key)
    # key == root.key: duplicate, ignore
    return root

root = None
for val in [8, 3, 10, 1, 6, 14, 4, 7, 13]:
    root = bst_insert(root, val)
```

### 6.4 Delete

Three cases:
1. **Leaf** — remove directly
2. **One child** — replace node with its child
3. **Two children** — replace key with **inorder successor** (min of right subtree), then delete successor

```python
def find_min(node):
    while node.left:
        node = node.left
    return node

def bst_delete(root, key):
    if root is None:
        return None
    if key < root.key:
        root.left  = bst_delete(root.left, key)
    elif key > root.key:
        root.right = bst_delete(root.right, key)
    else:
        # Case 1 & 2
        if root.left is None:  return root.right
        if root.right is None: return root.left
        # Case 3
        successor  = find_min(root.right)
        root.key   = successor.key
        root.right = bst_delete(root.right, successor.key)
    return root
```

### 6.5 Min, Max & Inorder Successor

```python
def bst_min(root):
    while root.left: root = root.left
    return root.key

def bst_max(root):
    while root.right: root = root.right
    return root.key

def inorder_successor(root, key):
    successor = None
    while root:
        if key < root.key:
            successor = root
            root = root.left
        elif key > root.key:
            root = root.right
        else:
            if root.right:
                return find_min(root.right).key
            break
    return successor.key if successor else None
```

### 6.6 Validate a BST

```python
def is_valid_bst(root, lo=float('-inf'), hi=float('inf')):
    if root is None:
        return True
    if not (lo < root.key < hi):
        return False
    return (is_valid_bst(root.left,  lo,       root.key) and
            is_valid_bst(root.right, root.key,  hi))
```

### 6.7 Kth Smallest Element in BST

```python
def kth_smallest(root, k):
    stack = []
    count = 0
    curr  = root
    while curr or stack:
        while curr:
            stack.append(curr)
            curr = curr.left
        curr  = stack.pop()
        count += 1
        if count == k:
            return curr.key
        curr = curr.right
    return -1
```

---

## 7. Heap & Priority Queue

A **Heap** is a **complete binary tree** stored as an array that satisfies the **heap-order property**.

- **Min-Heap:** parent ≤ children (root = minimum)
- **Max-Heap:** parent ≥ children (root = maximum)

**Array index relationships** (0-indexed):

$$\text{left child of } i = 2i + 1 \qquad \text{right child of } i = 2i + 2 \qquad \text{parent of } i = \lfloor(i-1)/2\rfloor$$

```
Array:  [1, 3, 5, 7, 9, 8]
         0  1  2  3  4  5

Tree:       1
           / \
          3   5
         / \ /
        7  9 8
```

### 7.1 Min-Heap from Scratch

```python
class MinHeap:
    def __init__(self):
        self._data = []

    def __len__(self):     return len(self._data)
    def is_empty(self):    return len(self._data) == 0
    def peek(self):        return self._data[0] if self._data else None

    def _parent(self, i):  return (i - 1) // 2
    def _left(self, i):    return 2 * i + 1
    def _right(self, i):   return 2 * i + 2

    def _swap(self, i, j):
        self._data[i], self._data[j] = self._data[j], self._data[i]

    def _sift_up(self, i):
        p = self._parent(i)
        if i > 0 and self._data[i] < self._data[p]:
            self._swap(i, p)
            self._sift_up(p)

    def _sift_down(self, i):
        n = len(self._data)
        smallest = i
        l, r = self._left(i), self._right(i)
        if l < n and self._data[l] < self._data[smallest]: smallest = l
        if r < n and self._data[r] < self._data[smallest]: smallest = r
        if smallest != i:
            self._swap(i, smallest)
            self._sift_down(smallest)

    def insert(self, val):
        self._data.append(val)
        self._sift_up(len(self._data) - 1)

    def remove_min(self):
        if self.is_empty(): raise IndexError("Heap is empty")
        self._swap(0, len(self._data) - 1)
        val = self._data.pop()
        if not self.is_empty():
            self._sift_down(0)
        return val

heap = MinHeap()
for v in [5, 3, 8, 1, 9, 2]: heap.insert(v)
print(heap.remove_min())  # 1
print(heap.remove_min())  # 2
```

### 7.2 Python `heapq` Module

```python
import heapq

data = [5, 3, 8, 1, 9, 2]
heapq.heapify(data)               # in-place min-heap in O(n)
heapq.heappush(data, 0)           # insert  O(log n)
val = heapq.heappop(data)         # remove min  O(log n)

# Max-heap: negate values
max_heap = []
for v in [5, 3, 8, 1, 9]:
    heapq.heappush(max_heap, -v)
print(-heapq.heappop(max_heap))   # 9

# K smallest / K largest
nums = [5, 3, 8, 1, 9, 2, 7]
print(heapq.nsmallest(3, nums))   # [1, 2, 3]
print(heapq.nlargest(3, nums))    # [9, 8, 7]
```

### 7.3 Heap Sort

```python
def heap_sort(arr):
    heapq.heapify(arr)
    return [heapq.heappop(arr) for _ in range(len(arr))]

print(heap_sort([5, 3, 8, 1, 9, 2]))  # [1, 2, 3, 5, 8, 9]
```

**Time:** $O(n \log n)$ | **Space:** $O(1)$ in-place (using array)

### 7.4 Heap Operations Complexity

| Operation | Time |
|-----------|------|
| Build heap from array | $O(n)$ |
| Insert | $O(\log n)$ |
| Remove min/max | $O(\log n)$ |
| Peek min/max | $O(1)$ |
| Heap sort | $O(n \log n)$ |

---

## 8. AVL Tree (Self-Balancing BST)

An **AVL tree** is a BST where for every node:

$$\text{Balance Factor} = \text{height(left)} - \text{height(right)} \in \{-1,\ 0,\ 1\}$$

If any insertion/deletion violates this, the tree is rebalanced via **rotations**.

### 8.1 Rotation Cases

| Case | Condition | Fix |
|------|-----------|-----|
| Left-Left (LL) | Left child is left-heavy | Right rotation at node |
| Right-Right (RR) | Right child is right-heavy | Left rotation at node |
| Left-Right (LR) | Left child is right-heavy | Left rotation at child, then right rotation at node |
| Right-Left (RL) | Right child is left-heavy | Right rotation at child, then left rotation at node |

### 8.2 AVL Implementation

```python
class AVLNode:
    def __init__(self, key):
        self.key    = key
        self.left   = None
        self.right  = None
        self.height = 1

def get_height(node):
    return node.height if node else 0

def get_balance(node):
    return get_height(node.left) - get_height(node.right) if node else 0

def update_height(node):
    node.height = 1 + max(get_height(node.left), get_height(node.right))

# ── Rotations ──────────────────────────────────────────────────────
def rotate_right(y):
    x  = y.left
    T2 = x.right
    x.right = y
    y.left  = T2
    update_height(y)
    update_height(x)
    return x

def rotate_left(x):
    y  = x.right
    T2 = y.left
    y.left  = x
    x.right = T2
    update_height(x)
    update_height(y)
    return y

# ── Rebalance helper ────────────────────────────────────────────────
def rebalance(node):
    update_height(node)
    bal = get_balance(node)

    if bal > 1:                                       # Left-heavy
        if get_balance(node.left) < 0:                # LR case
            node.left = rotate_left(node.left)
        return rotate_right(node)                     # LL case

    if bal < -1:                                      # Right-heavy
        if get_balance(node.right) > 0:               # RL case
            node.right = rotate_right(node.right)
        return rotate_left(node)                      # RR case

    return node

# ── Insert ──────────────────────────────────────────────────────────
def avl_insert(root, key):
    if root is None:
        return AVLNode(key)
    if key < root.key:
        root.left  = avl_insert(root.left, key)
    elif key > root.key:
        root.right = avl_insert(root.right, key)
    else:
        return root   # duplicate
    return rebalance(root)

# ── Delete ──────────────────────────────────────────────────────────
def avl_delete(root, key):
    if root is None:
        return None
    if key < root.key:
        root.left  = avl_delete(root.left, key)
    elif key > root.key:
        root.right = avl_delete(root.right, key)
    else:
        if root.left is None:  return root.right
        if root.right is None: return root.left
        successor  = find_min(root.right)
        root.key   = successor.key
        root.right = avl_delete(root.right, successor.key)
    return rebalance(root)

# Test
root = None
for val in [10, 20, 30, 40, 50, 25]:
    root = avl_insert(root, val)
# Without AVL: degenerate chain. With AVL: balanced tree.
```

### 8.3 AVL vs BST vs Balanced

| | BST (avg) | BST (worst) | AVL |
|--|-----------|-------------|-----|
| Search | $O(\log n)$ | $O(n)$ | $O(\log n)$ |
| Insert | $O(\log n)$ | $O(n)$ | $O(\log n)$ |
| Delete | $O(\log n)$ | $O(n)$ | $O(\log n)$ |
| Space | $O(n)$ | $O(n)$ | $O(n)$ |
| Guarantee | No | No | Yes |

---

## 9. Segment Tree

A **Segment Tree** is a binary tree used to efficiently answer **range queries** (sum, min, max) and support **point updates** on an array.

- **Build:** $O(n)$
- **Query / Update:** $O(\log n)$
- **Space:** $O(4n)$

### 9.1 Range Sum Segment Tree

```python
class SegmentTree:
    def __init__(self, arr):
        self.n    = len(arr)
        self.tree = [0] * (4 * self.n)
        self._build(arr, 0, 0, self.n - 1)

    def _build(self, arr, node, start, end):
        if start == end:
            self.tree[node] = arr[start]
        else:
            mid = (start + end) // 2
            self._build(arr, 2*node+1, start,   mid)
            self._build(arr, 2*node+2, mid+1,   end)
            self.tree[node] = self.tree[2*node+1] + self.tree[2*node+2]

    def query(self, l, r, node=0, start=None, end=None):
        """Sum of arr[l..r]"""
        if start is None: start, end = 0, self.n - 1
        if r < start or end < l:          # no overlap
            return 0
        if l <= start and end <= r:       # total overlap
            return self.tree[node]
        mid = (start + end) // 2          # partial overlap
        left  = self.query(l, r, 2*node+1, start, mid)
        right = self.query(l, r, 2*node+2, mid+1, end)
        return left + right

    def update(self, idx, val, node=0, start=None, end=None):
        """Set arr[idx] = val"""
        if start is None: start, end = 0, self.n - 1
        if start == end:
            self.tree[node] = val
        else:
            mid = (start + end) // 2
            if idx <= mid:
                self.update(idx, val, 2*node+1, start, mid)
            else:
                self.update(idx, val, 2*node+2, mid+1, end)
            self.tree[node] = self.tree[2*node+1] + self.tree[2*node+2]

arr = [1, 3, 5, 7, 9, 11]
st  = SegmentTree(arr)
print(st.query(1, 4))     # 3+5+7+9 = 24
st.update(2, 10)           # arr[2] = 10
print(st.query(1, 4))     # 3+10+7+9 = 29
```

### 9.2 Range Min Segment Tree

```python
class MinSegTree:
    def __init__(self, arr):
        self.n    = len(arr)
        self.tree = [float('inf')] * (4 * self.n)
        self._build(arr, 0, 0, self.n - 1)

    def _build(self, arr, node, s, e):
        if s == e:
            self.tree[node] = arr[s]
        else:
            m = (s + e) // 2
            self._build(arr, 2*node+1, s,   m)
            self._build(arr, 2*node+2, m+1, e)
            self.tree[node] = min(self.tree[2*node+1], self.tree[2*node+2])

    def query(self, l, r, node=0, s=None, e=None):
        if s is None: s, e = 0, self.n - 1
        if r < s or e < l: return float('inf')
        if l <= s and e <= r: return self.tree[node]
        m = (s + e) // 2
        return min(self.query(l, r, 2*node+1, s, m),
                   self.query(l, r, 2*node+2, m+1, e))
```

---

## 10. Trie (Prefix Tree)

A **Trie** is a tree where each path from root to a node represents a **prefix** of some stored string. Ideal for autocomplete, spell checking, and prefix lookups.

- **Insert / Search:** $O(m)$ where $m$ = string length
- **Space:** $O(\text{total characters across all words})$

```python
class TrieNode:
    def __init__(self):
        self.children  = {}    # char → TrieNode
        self.is_end    = False  # marks end of a word


class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for ch in word:
            if ch not in node.children:
                node.children[ch] = TrieNode()
            node = node.children[ch]
        node.is_end = True

    def search(self, word):
        """Returns True if word is in the trie."""
        node = self.root
        for ch in word:
            if ch not in node.children:
                return False
            node = node.children[ch]
        return node.is_end

    def starts_with(self, prefix):
        """Returns True if any word starts with prefix."""
        node = self.root
        for ch in prefix:
            if ch not in node.children:
                return False
            node = node.children[ch]
        return True

    def delete(self, word):
        def _delete(node, word, depth):
            if depth == len(word):
                if not node.is_end:
                    return False
                node.is_end = False
                return len(node.children) == 0
            ch = word[depth]
            if ch not in node.children:
                return False
            should_delete = _delete(node.children[ch], word, depth + 1)
            if should_delete:
                del node.children[ch]
                return not node.is_end and len(node.children) == 0
            return False
        _delete(self.root, word, 0)

    def words_with_prefix(self, prefix):
        """Return all words that start with prefix."""
        node = self.root
        for ch in prefix:
            if ch not in node.children:
                return []
            node = node.children[ch]
        results = []
        self._dfs(node, list(prefix), results)
        return results

    def _dfs(self, node, path, results):
        if node.is_end:
            results.append(''.join(path))
        for ch, child in node.children.items():
            path.append(ch)
            self._dfs(child, path, results)
            path.pop()


# Usage
trie = Trie()
for word in ['apple', 'app', 'apt', 'bat', 'ball']:
    trie.insert(word)

print(trie.search('app'))          # True
print(trie.search('ap'))           # False
print(trie.starts_with('ap'))      # True
print(trie.words_with_prefix('ap'))# ['app', 'apple', 'apt']
trie.delete('app')
print(trie.search('app'))          # False
print(trie.search('apple'))        # True  (not deleted)
```

---

## 11. Lowest Common Ancestor (LCA)

The **LCA** of two nodes $u$ and $v$ in a tree is the deepest node that is an ancestor of both.

### 11.1 LCA in Binary Tree (no BST property)

```python
def lca(root, p, q):
    if root is None or root.value == p or root.value == q:
        return root
    left  = lca(root.left,  p, q)
    right = lca(root.right, p, q)
    if left and right:
        return root    # p and q are in different subtrees
    return left or right

# Example tree:  1 → (2 → (4,5)), (3 → (_,6))
print(lca(root, 4, 5).value)   # 2
print(lca(root, 4, 6).value)   # 1
```

### 11.2 LCA in BST (exploits ordering)

```python
def lca_bst(root, p, q):
    while root:
        if p < root.key and q < root.key:
            root = root.left
        elif p > root.key and q > root.key:
            root = root.right
        else:
            return root   # root.key is between p and q → LCA
    return None
```

**Time:** $O(h)$ — $O(\log n)$ balanced, $O(n)$ worst case

---

## 12. Important Tree Problems

### 12.1 Maximum Path Sum in Binary Tree

```python
def max_path_sum(root):
    max_sum = [float('-inf')]

    def dfs(node):
        if node is None: return 0
        left  = max(dfs(node.left),  0)   # ignore negative paths
        right = max(dfs(node.right), 0)
        max_sum[0] = max(max_sum[0], node.value + left + right)
        return node.value + max(left, right)

    dfs(root)
    return max_sum[0]
```

### 12.2 Serialize and Deserialize Binary Tree

```python
from collections import deque

def serialize(root):
    """Level-order serialisation."""
    if not root: return ''
    result, queue = [], deque([root])
    while queue:
        node = queue.popleft()
        if node:
            result.append(str(node.value))
            queue.append(node.left)
            queue.append(node.right)
        else:
            result.append('#')
    return ','.join(result)

def deserialize(data):
    if not data: return None
    vals  = deque(data.split(','))
    root  = Node(int(vals.popleft()))
    queue = deque([root])
    while queue:
        node = queue.popleft()
        left_val = vals.popleft()
        if left_val != '#':
            node.left = Node(int(left_val))
            queue.append(node.left)
        right_val = vals.popleft()
        if right_val != '#':
            node.right = Node(int(right_val))
            queue.append(node.right)
    return root
```

### 12.3 Right View of Binary Tree

```python
def right_view(root):
    if not root: return []
    result, queue = [], deque([root])
    while queue:
        for i in range(len(queue)):
            node = queue.popleft()
            if i == len(queue):    # last node at this level (before popleft reduces queue)
                result.append(node.value)
            if node.left:  queue.append(node.left)
            if node.right: queue.append(node.right)
    # Simpler approach using level size:
    return result

def right_view_v2(root):
    if not root: return []
    result, queue = [], deque([root])
    while queue:
        size = len(queue)
        for i in range(size):
            node = queue.popleft()
            if i == size - 1:
                result.append(node.value)
            if node.left:  queue.append(node.left)
            if node.right: queue.append(node.right)
    return result
```

### 12.4 Flatten Binary Tree to Linked List (Preorder)

```python
def flatten(root):
    """Flatten in-place to right-skewed linked list (preorder)."""
    if not root: return
    flatten(root.left)
    flatten(root.right)
    if root.left:
        tail = root.left
        while tail.right: tail = tail.right
        tail.right = root.right
        root.right = root.left
        root.left  = None
```

### 12.5 Construct Tree from Preorder + Inorder

```python
def build_tree(preorder, inorder):
    if not preorder or not inorder:
        return None
    root_val        = preorder[0]
    root            = Node(root_val)
    mid             = inorder.index(root_val)
    root.left  = build_tree(preorder[1:mid+1],  inorder[:mid])
    root.right = build_tree(preorder[mid+1:],   inorder[mid+1:])
    return root

# preorder=[1,2,4,5,3,6], inorder=[4,2,5,1,3,6]
# Reconstructs the example tree from section 4.1
```

### 12.6 Zigzag Level-Order Traversal

```python
from collections import deque

def zigzag(root):
    if not root: return []
    result, queue, left_to_right = [], deque([root]), True
    while queue:
        size  = len(queue)
        level = deque()
        for _ in range(size):
            node = queue.popleft()
            if left_to_right: level.append(node.value)
            else:             level.appendleft(node.value)
            if node.left:  queue.append(node.left)
            if node.right: queue.append(node.right)
        result.append(list(level))
        left_to_right = not left_to_right
    return result
```

---

## 13. Complexity Summary

| Operation / Algorithm | Time | Space | Notes |
|-----------------------|------|-------|-------|
| Binary tree traversal (any) | $O(n)$ | $O(h)$ | $h$ = height |
| Binary tree search | $O(n)$ | $O(h)$ | Brute force |
| BST search / insert / delete | $O(\log n)$ avg | $O(h)$ | $O(n)$ worst (skewed) |
| AVL search / insert / delete | $O(\log n)$ | $O(\log n)$ | Guaranteed |
| Heap insert | $O(\log n)$ | $O(1)$ | |
| Heap remove min/max | $O(\log n)$ | $O(1)$ | |
| Heap build from array | $O(n)$ | $O(1)$ | |
| Heap sort | $O(n \log n)$ | $O(1)$ | In-place |
| Segment tree build | $O(n)$ | $O(n)$ | |
| Segment tree query | $O(\log n)$ | $O(\log n)$ | |
| Segment tree update | $O(\log n)$ | $O(\log n)$ | |
| Trie insert / search | $O(m)$ | $O(m)$ | $m$ = string length |
| LCA (binary tree) | $O(n)$ | $O(h)$ | |
| LCA (BST) | $O(h)$ | $O(1)$ | |
| Diameter of tree | $O(n)$ | $O(h)$ | |
| Serialize / deserialize | $O(n)$ | $O(n)$ | |

---

*End of Tree Data Structures notes*
