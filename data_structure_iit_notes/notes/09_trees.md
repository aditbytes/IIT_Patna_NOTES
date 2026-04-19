# Topic 9: Trees

## 1. Overview

A **Tree** is a hierarchical, non-linear data structure consisting of nodes connected by edges. A tree has a **root** node, and every node has zero or more **child** nodes.

---

## 2. Key Concepts

### 2.1 Terminology

| Term | Definition |
|------|------------|
| **Root** | The topmost node (no parent) |
| **Parent** | Node directly above another node |
| **Child** | Node directly below another node |
| **Leaf** | Node with no children |
| **Internal node** | Node with at least one child |
| **Sibling** | Nodes sharing the same parent |
| **Depth** | Number of edges from root to the node |
| **Height** | Number of edges from the node to the deepest leaf |
| **Level** | Depth + 1 (root is level 1) |
| **Subtree** | A node and all its descendants |
| **Degree** | Number of children of a node |

### 2.2 Types of Trees

| Type | Description |
|------|-------------|
| **General Tree** | Each node can have any number of children |
| **Binary Tree** | Each node has at most 2 children (left, right) |
| **Binary Search Tree** | Binary tree with ordering: left < parent < right |
| **Complete Binary Tree** | All levels fully filled except possibly the last (filled left to right) |
| **Full Binary Tree** | Every node has 0 or 2 children |
| **Perfect Binary Tree** | All internal nodes have 2 children, all leaves at same level |
| **Balanced Binary Tree** | Height difference of left and right subtrees ≤ 1 |

### 2.3 Binary Tree Properties

- Maximum nodes at level $l$: $2^{l-1}$ (root at level 1)
- Maximum nodes in a tree of height $h$: $2^{h+1} - 1$
- Minimum height of tree with $n$ nodes: $\lfloor \log_2 n \rfloor$
- In a binary tree: number of leaf nodes = number of nodes with 2 children + 1

---

## 3. Code Examples

### 3.1 Binary Tree Node

*(From IIT Patna Lecture)*

```python
class Node:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None
```

### 3.2 Tree Traversals

```
        1
       / \
      2   3
     / \   \
    4   5   6
```

#### Inorder (Left → Root → Right)

```python
def inorder(root):
    if root:
        inorder(root.left)
        print(root.value, end=" ")
        inorder(root.right)

# Output: 4 2 5 1 3 6
```

#### Preorder (Root → Left → Right)

```python
def preorder(root):
    if root:
        print(root.value, end=" ")
        preorder(root.left)
        preorder(root.right)

# Output: 1 2 4 5 3 6
```

#### Postorder (Left → Right → Root)

```python
def postorder(root):
    if root:
        postorder(root.left)
        postorder(root.right)
        print(root.value, end=" ")

# Output: 4 5 2 6 3 1
```

#### Level-Order (BFS)

*(From IIT Patna Lecture — uses deque)*

```python
from collections import deque

def level_order(root):
    if not root:
        return
    queue = deque([root])
    while queue:
        node = queue.popleft()
        print(node.value, end=" ")
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)

# Output: 1 2 3 4 5 6
```

### 3.3 Build Tree and Traverse

```python
# Build the tree
root = Node(1)
root.left = Node(2)
root.right = Node(3)
root.left.left = Node(4)
root.left.right = Node(5)
root.right.right = Node(6)

print("Inorder:    ", end=""); inorder(root); print()
print("Preorder:   ", end=""); preorder(root); print()
print("Postorder:  ", end=""); postorder(root); print()
print("Level-order:", end=" "); level_order(root); print()
```

### 3.4 Search in Binary Tree

*(From IIT Patna Lecture)* — Time: $O(n)$

```python
def search(root, key):
    if root is None:
        return False
    if root.value == key:
        return True
    return search(root.left, key) or search(root.right, key)

print(search(root, 5))   # True
print(search(root, 10))  # False
```

### 3.5 Insert in Binary Tree (Level-Order)

*(From IIT Patna Lecture)*

```python
from collections import deque

def insert(root, value):
    new_node = Node(value)
    if root is None:
        return new_node

    queue = deque([root])
    while queue:
        temp = queue.popleft()
        if temp.left is None:
            temp.left = new_node
            return root
        else:
            queue.append(temp.left)
        if temp.right is None:
            temp.right = new_node
            return root
        else:
            queue.append(temp.right)
    return root
```

### 3.6 Delete a Node in Binary Tree

*(From IIT Patna Lecture)*

Strategy: Replace the node to delete with the **deepest rightmost** node, then delete the deepest node.

```python
from collections import deque

def delete_deepest(root, d_node):
    queue = deque([root])
    while queue:
        temp = queue.popleft()
        if temp is d_node:
            temp = None
            return
        if temp.right:
            if temp.right is d_node:
                temp.right = None
                return
            queue.append(temp.right)
        if temp.left:
            if temp.left is d_node:
                temp.left = None
                return
            queue.append(temp.left)

def delete(root, key):
    if root is None:
        return None
    if root.left is None and root.right is None:
        if root.value == key:
            return None
        return root

    queue = deque([root])
    key_node = None
    last_node = None
    while queue:
        last_node = queue.popleft()
        if last_node.value == key:
            key_node = last_node
        if last_node.left:
            queue.append(last_node.left)
        if last_node.right:
            queue.append(last_node.right)

    if key_node:
        key_node.value = last_node.value
        delete_deepest(root, last_node)
    return root
```

### 3.7 Height of a Tree

```python
def height(root):
    if root is None:
        return -1  # -1 for edge count, 0 for node count
    return 1 + max(height(root.left), height(root.right))

print(height(root))  # 2 (root → left → leaf)
```

### 3.8 Count Nodes

```python
def count_nodes(root):
    if root is None:
        return 0
    return 1 + count_nodes(root.left) + count_nodes(root.right)

print(count_nodes(root))  # 6
```

### 3.9 Iterative Traversals Using Stack

```python
def inorder_iterative(root):
    result = []
    stack = []
    current = root
    while current or stack:
        while current:
            stack.append(current)
            current = current.left
        current = stack.pop()
        result.append(current.value)
        current = current.right
    return result

def preorder_iterative(root):
    if not root:
        return []
    result = []
    stack = [root]
    while stack:
        node = stack.pop()
        result.append(node.value)
        if node.right:
            stack.append(node.right)
        if node.left:
            stack.append(node.left)
    return result
```

---

## 4. MCQs (15 Questions)

**Q1.** The maximum number of nodes at level $l$ of a binary tree is:
- A) $2l$
- B) $l^2$
- C) $2^{l-1}$ (root at level 1)
- D) $2^l + 1$

**Answer:** C) $2^{l-1}$

---

**Q2.** Which traversal visits: Left → Root → Right?
- A) Preorder
- B) Inorder
- C) Postorder
- D) Level-order

**Answer:** B) Inorder

---

**Q3.** The height of a tree with a single node is:
- A) -1
- B) 0
- C) 1
- D) Undefined

**Answer:** B) 0 (when measuring by edges)

---

**Q4.** In a binary tree with $n$ nodes, the minimum possible height is:
- A) $n - 1$
- B) $\log_2 n$
- C) $\lfloor \log_2 n \rfloor$
- D) $n / 2$

**Answer:** C) $\lfloor \log_2 n \rfloor$

---

**Q5.** Which traversal uses a queue?
- A) Inorder
- B) Preorder
- C) Postorder
- D) Level-order

**Answer:** D) Level-order (BFS)

---

**Q6.** Preorder traversal of the tree `[1, 2, 3, 4, 5]` (complete binary tree):
```
      1
     / \
    2   3
   / \
  4   5
```
- A) `4 2 5 1 3`
- B) `1 2 4 5 3`
- C) `4 5 2 3 1`
- D) `1 2 3 4 5`

**Answer:** B) `1 2 4 5 3`

---

**Q7.** Postorder traversal of the same tree:
- A) `4 5 2 3 1`
- B) `1 2 4 5 3`
- C) `4 2 5 1 3`
- D) `1 2 3 4 5`

**Answer:** A) `4 5 2 3 1`

---

**Q8.** Time complexity of searching for a value in a general binary tree:
- A) $O(1)$
- B) $O(\log n)$
- C) $O(n)$
- D) $O(n^2)$

**Answer:** C) $O(n)$ — Must potentially visit all nodes.

---

**Q9.** A **full binary tree** is one where:
- A) All levels are completely filled
- B) Every node has 0 or 2 children
- C) All leaves are at the same level
- D) It has the maximum number of nodes

**Answer:** B) Every node has 0 or 2 children

---

**Q10.** A **complete binary tree** is one where:
- A) Every node has 2 children
- B) All levels are full except possibly the last, which fills left to right
- C) All leaves are at the same depth
- D) Height equals number of nodes

**Answer:** B) All levels are full except possibly the last, which fills left to right

---

**Q11.** In a binary tree with 10 leaf nodes, how many internal nodes with 2 children exist?
- A) 9
- B) 10
- C) 11
- D) Cannot determine

**Answer:** A) 9 — For a binary tree: leaves = nodes_with_2_children + 1.

---

**Q12.** Which data structure is used for iterative inorder traversal?
- A) Queue
- B) Stack
- C) Array
- D) Hash table

**Answer:** B) Stack

---

**Q13.** Time complexity of all four traversals (inorder, preorder, postorder, level-order):
- A) $O(\log n)$
- B) $O(n)$
- C) $O(n \log n)$
- D) $O(n^2)$

**Answer:** B) $O(n)$ — Each node is visited exactly once.

---

**Q14.** A tree with $n$ nodes has how many edges?
- A) $n$
- B) $n - 1$
- C) $n + 1$
- D) $2n$

**Answer:** B) $n - 1$

---

**Q15.** The **depth** of the root node is:
- A) -1
- B) 0
- C) 1
- D) Depends on the tree

**Answer:** B) 0
