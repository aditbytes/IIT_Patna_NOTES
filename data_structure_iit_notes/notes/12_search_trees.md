# Topic 12: Search Trees

## 1. Overview

A **Search Tree** is a tree data structure used for efficient searching, insertion, and deletion. The most fundamental type is the **Binary Search Tree (BST)**, and balanced variants like **AVL Trees** and **Red-Black Trees** guarantee $O(\log n)$ operations.

---

## 2. Key Concepts

### 2.1 Binary Search Tree (BST)

*(From IIT Patna Lecture)*

A BST is a binary tree where for every node:
- All values in the **left** subtree are **less than** the node's value
- All values in the **right** subtree are **greater than** the node's value

```
        8
       / \
      3   10
     / \    \
    1   6   14
       / \  /
      4  7 13
```

**Inorder traversal of a BST gives sorted order:** `1 3 4 6 7 8 10 13 14`

### 2.2 BST Operations Complexity

| Operation | Average | Worst (skewed) |
|-----------|---------|----------------|
| Search | $O(\log n)$ | $O(n)$ |
| Insert | $O(\log n)$ | $O(n)$ |
| Delete | $O(\log n)$ | $O(n)$ |
| Inorder | $O(n)$ | $O(n)$ |
| Min/Max | $O(\log n)$ | $O(n)$ |

### 2.3 AVL Tree

A **self-balancing BST** where the height difference (balance factor) between left and right subtrees of any node is at most 1.

**Balance Factor:** $BF(node) = height(left) - height(right)$

- $BF \in \{-1, 0, 1\}$ → Balanced
- $|BF| > 1$ → Needs rebalancing via **rotations**

### 2.4 AVL Rotations

| Case | Condition | Rotation |
|------|-----------|----------|
| Left-Left (LL) | Left child is left-heavy | Right rotation |
| Right-Right (RR) | Right child is right-heavy | Left rotation |
| Left-Right (LR) | Left child is right-heavy | Left rotation on left child, then right rotation |
| Right-Left (RL) | Right child is left-heavy | Right rotation on right child, then left rotation |

---

## 3. Code Examples

### 3.1 BST Node

```python
class BSTNode:
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None
```

### 3.2 BST Search

*(From IIT Patna Lecture)*

```python
def bst_search(root, key):
    if root is None:
        return False
    if key == root.key:
        return True
    elif key < root.key:
        return bst_search(root.left, key)
    else:
        return bst_search(root.right, key)
```

**Iterative version:**
```python
def bst_search_iter(root, key):
    while root:
        if key == root.key:
            return True
        elif key < root.key:
            root = root.left
        else:
            root = root.right
    return False
```

### 3.3 BST Insert

```python
def bst_insert(root, key):
    if root is None:
        return BSTNode(key)
    if key < root.key:
        root.left = bst_insert(root.left, key)
    elif key > root.key:
        root.right = bst_insert(root.right, key)
    # If key == root.key, duplicate — do nothing
    return root

# Build BST
root = None
for val in [8, 3, 10, 1, 6, 14, 4, 7, 13]:
    root = bst_insert(root, val)
```

### 3.4 BST Delete

Three cases:
1. **Leaf node** — simply remove
2. **One child** — replace node with its child
3. **Two children** — replace with **inorder successor** (smallest in right subtree) or **inorder predecessor** (largest in left subtree)

```python
def find_min(node):
    while node.left:
        node = node.left
    return node

def bst_delete(root, key):
    if root is None:
        return None

    if key < root.key:
        root.left = bst_delete(root.left, key)
    elif key > root.key:
        root.right = bst_delete(root.right, key)
    else:
        # Node found
        # Case 1: Leaf or Case 2: One child
        if root.left is None:
            return root.right
        elif root.right is None:
            return root.left

        # Case 3: Two children
        successor = find_min(root.right)
        root.key = successor.key
        root.right = bst_delete(root.right, successor.key)

    return root
```

### 3.5 BST: Find Min and Max

```python
def find_min(root):
    if root is None:
        return None
    while root.left:
        root = root.left
    return root.key

def find_max(root):
    if root is None:
        return None
    while root.right:
        root = root.right
    return root.key
```

### 3.6 BST: Inorder Traversal (Sorted)

```python
def inorder(root):
    if root:
        inorder(root.left)
        print(root.key, end=" ")
        inorder(root.right)

inorder(root)  # 1 3 4 6 7 8 10 13 14
```

### 3.7 AVL Tree Implementation

```python
class AVLNode:
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None
        self.height = 1

def get_height(node):
    return node.height if node else 0

def get_balance(node):
    return get_height(node.left) - get_height(node.right) if node else 0

def right_rotate(y):
    x = y.left
    T2 = x.right
    x.right = y
    y.left = T2
    y.height = 1 + max(get_height(y.left), get_height(y.right))
    x.height = 1 + max(get_height(x.left), get_height(x.right))
    return x

def left_rotate(x):
    y = x.right
    T2 = y.left
    y.left = x
    x.right = T2
    x.height = 1 + max(get_height(x.left), get_height(x.right))
    y.height = 1 + max(get_height(y.left), get_height(y.right))
    return y

def avl_insert(root, key):
    # Normal BST insert
    if not root:
        return AVLNode(key)
    if key < root.key:
        root.left = avl_insert(root.left, key)
    elif key > root.key:
        root.right = avl_insert(root.right, key)
    else:
        return root  # No duplicates

    # Update height
    root.height = 1 + max(get_height(root.left), get_height(root.right))

    # Check balance
    balance = get_balance(root)

    # Left-Left
    if balance > 1 and key < root.left.key:
        return right_rotate(root)

    # Right-Right
    if balance < -1 and key > root.right.key:
        return left_rotate(root)

    # Left-Right
    if balance > 1 and key > root.left.key:
        root.left = left_rotate(root.left)
        return right_rotate(root)

    # Right-Left
    if balance < -1 and key < root.right.key:
        root.right = right_rotate(root.right)
        return left_rotate(root)

    return root

# Build AVL tree
root = None
for val in [10, 20, 30, 40, 50, 25]:
    root = avl_insert(root, val)

def preorder(node):
    if node:
        print(node.key, end=" ")
        preorder(node.left)
        preorder(node.right)

preorder(root)  # Balanced tree
```

### 3.8 Validate BST

```python
def is_valid_bst(root, min_val=float('-inf'), max_val=float('inf')):
    if root is None:
        return True
    if root.key <= min_val or root.key >= max_val:
        return False
    return (is_valid_bst(root.left, min_val, root.key) and
            is_valid_bst(root.right, root.key, max_val))
```

---

## 4. MCQs (15 Questions)

**Q1.** In a BST, all values in the left subtree of a node are:
- A) Greater than the node
- B) Less than the node
- C) Equal to the node
- D) Random

**Answer:** B) Less than the node

---

**Q2.** Inorder traversal of a BST gives:
- A) Random order
- B) Sorted (ascending) order
- C) Reverse sorted order
- D) Level order

**Answer:** B) Sorted (ascending) order

---

**Q3.** Average-case search time in a balanced BST:
- A) $O(1)$
- B) $O(\log n)$
- C) $O(n)$
- D) $O(n \log n)$

**Answer:** B) $O(\log n)$

---

**Q4.** Worst-case search time in a skewed BST (all nodes on one side):
- A) $O(1)$
- B) $O(\log n)$
- C) $O(n)$
- D) $O(n^2)$

**Answer:** C) $O(n)$ — The tree degenerates to a linked list.

---

**Q5.** When deleting a BST node with two children, it is replaced by:
- A) Its parent
- B) Inorder successor or inorder predecessor
- C) The leftmost node
- D) The root

**Answer:** B) Inorder successor (smallest in right subtree) or inorder predecessor (largest in left subtree)

---

**Q6.** The inorder successor of a node is:
- A) The parent node
- B) The smallest node in its right subtree
- C) The largest node in its left subtree
- D) The next node in level-order

**Answer:** B) The smallest node in its right subtree

---

**Q7.** An AVL tree maintains a balance factor of:
- A) 0 for all nodes
- B) −1, 0, or 1 for all nodes
- C) Any value
- D) ≤ 2 for all nodes

**Answer:** B) −1, 0, or 1 for all nodes

---

**Q8.** After inserting into an AVL tree, if the balance factor becomes 2 and the left child's balance is 1, which rotation?
- A) Left rotation
- B) Right rotation (LL case)
- C) Left-Right rotation
- D) Right-Left rotation

**Answer:** B) Right rotation (LL case)

---

**Q9.** The height of an AVL tree with $n$ nodes is:
- A) $O(n)$
- B) $O(\log n)$
- C) $O(n \log n)$
- D) $O(\sqrt{n})$

**Answer:** B) $O(\log n)$ — AVL trees are balanced.

---

**Q10.** Inserting elements 1, 2, 3, 4, 5 (in order) into a BST creates:
- A) A balanced tree
- B) A right-skewed tree (like a linked list)
- C) A complete binary tree
- D) A left-skewed tree

**Answer:** B) A right-skewed tree

---

**Q11.** How many rotations may be needed for a single AVL insertion?
- A) 0
- B) At most 1 (single) or 2 (double rotation)
- C) $O(n)$
- D) $O(\log n)$

**Answer:** B) At most 1 single rotation or 1 double rotation (which is 2 single rotations).

---

**Q12.** The minimum number of nodes in an AVL tree of height $h$ is:
- A) $2^h - 1$
- B) $h + 1$
- C) Follows the Fibonacci-like recurrence: $N(h) = N(h-1) + N(h-2) + 1$
- D) $2^h$

**Answer:** C) Follows the recurrence $N(h) = N(h-1) + N(h-2) + 1$

---

**Q13.** To find the kth smallest element in a BST:
- A) Do level-order traversal
- B) Do inorder traversal and pick the kth element
- C) Do preorder traversal
- D) Search for k

**Answer:** B) Do inorder traversal and pick the kth element

---

**Q14.** Which of the following is NOT a self-balancing BST?
- A) AVL tree
- B) Red-Black tree
- C) Splay tree
- D) Binary tree (general)

**Answer:** D) Binary tree (general) — Not self-balancing.

---

**Q15.** In a BST, the minimum element is found at:
- A) The root
- B) The leftmost node
- C) The rightmost node
- D) Any leaf

**Answer:** B) The leftmost node
