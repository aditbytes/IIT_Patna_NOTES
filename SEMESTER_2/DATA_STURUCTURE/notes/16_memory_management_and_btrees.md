# Topic 16: Memory Management and B-Trees

## 1. Overview

This topic covers how Python manages memory (allocation, garbage collection, references) and introduces **B-Trees** — balanced search trees designed for disk-based storage and databases.

---

## 2. Memory Management in Python

### 2.1 How Python Manages Memory

- Python uses a **private heap** to store all objects and data structures.
- The **memory manager** handles allocation and deallocation internally.
- Programmers don't have direct access to the heap.

### 2.2 Reference Counting

Python tracks how many references point to each object. When the count drops to **0**, the memory is freed.

```python
import sys

a = [1, 2, 3]
print(sys.getrefcount(a))  # 2 (one for 'a', one for the argument to getrefcount)

b = a               # Reference count increases
print(sys.getrefcount(a))  # 3

del b               # Reference count decreases
print(sys.getrefcount(a))  # 2
```

### 2.3 Garbage Collection

Python also has a **garbage collector** (in the `gc` module) to handle **circular references** that reference counting alone can't clean up.

```python
import gc

# Circular reference
class Node:
    def __init__(self):
        self.ref = None

a = Node()
b = Node()
a.ref = b
b.ref = a  # Circular!

del a
del b
# Reference counts don't reach 0 — garbage collector handles this.

gc.collect()  # Force garbage collection
print(gc.get_count())  # (num_gen0, num_gen1, num_gen2)
```

### 2.4 Generational Garbage Collection

Python uses **3 generations** (0, 1, 2):
- **Generation 0:** Newly created objects — collected most frequently
- **Generation 1:** Survived one collection
- **Generation 2:** Long-lived objects — collected least frequently

### 2.5 Memory Optimization Tips

```python
# Use __slots__ to reduce memory per object
class PointRegular:
    def __init__(self, x, y):
        self.x = x
        self.y = y

class PointSlots:
    __slots__ = ['x', 'y']
    def __init__(self, x, y):
        self.x = x
        self.y = y

import sys
p1 = PointRegular(1, 2)
p2 = PointSlots(1, 2)
print(sys.getsizeof(p1.__dict__))  # ~104 bytes (dict overhead)
# p2 has no __dict__ → less memory

# Use generators instead of lists for large sequences
def gen_squares(n):
    for i in range(n):
        yield i ** 2

# Only one value in memory at a time
for sq in gen_squares(1000000):
    pass  # Processes one at a time
```

### 2.6 `id()` and `is` vs `==`

```python
a = [1, 2, 3]
b = a           # b points to same object
c = [1, 2, 3]  # c is a different object with same value

print(a == b)   # True (same value)
print(a is b)   # True (same object — same id)
print(a == c)   # True (same value)
print(a is c)   # False (different objects)
print(id(a), id(b), id(c))

# Integer caching: Python caches integers -5 to 256
x = 100
y = 100
print(x is y)   # True (cached)
z = 1000
w = 1000
print(z is w)   # May be False (not cached)
```

---

## 3. B-Trees

### 3.1 What is a B-Tree?

A **B-Tree** of order $m$ (or minimum degree $t$) is a self-balancing search tree where:
- Each node can have at most $m$ children (and $m-1$ keys)
- Each node (except root) has at least $\lceil m/2 \rceil$ children
- All **leaves** are at the **same level**
- Keys within a node are sorted

**B-Trees are designed for systems that read/write large blocks of data** (databases, file systems) to minimize disk I/O.

### 3.2 B-Tree Properties (order $m$)

| Property | Value |
|----------|-------|
| Max children per node | $m$ |
| Max keys per node | $m - 1$ |
| Min children (non-root internal) | $\lceil m/2 \rceil$ |
| Min keys (non-root) | $\lceil m/2 \rceil - 1$ |
| All leaves at | Same depth |

### 3.3 B-Tree vs BST vs AVL

| Feature | BST | AVL | B-Tree |
|---------|-----|-----|--------|
| Branching | Binary | Binary | Multi-way |
| Balance | No guarantee | Strict (height ±1) | All leaves same level |
| Height | $O(n)$ worst | $O(\log n)$ | $O(\log_m n)$ |
| Disk I/O | High | High | **Low** (designed for disk) |
| Use case | In-memory | In-memory | Databases, file systems |

### 3.4 B-Tree Operations Complexity

| Operation | Time Complexity |
|-----------|----------------|
| Search | $O(\log n)$ |
| Insert | $O(\log n)$ |
| Delete | $O(\log n)$ |

All operations involve $O(\log_m n)$ **disk accesses**, which is the key optimization.

---

## 4. Code Examples

### 4.1 B-Tree Node and Search

```python
class BTreeNode:
    def __init__(self, leaf=True):
        self.leaf = leaf
        self.keys = []
        self.children = []

class BTree:
    def __init__(self, t):
        """t = minimum degree (each node has at least t-1 keys, at most 2t-1 keys)."""
        self.root = BTreeNode()
        self.t = t

    def search(self, key, node=None):
        if node is None:
            node = self.root

        i = 0
        while i < len(node.keys) and key > node.keys[i]:
            i += 1

        if i < len(node.keys) and key == node.keys[i]:
            return (node, i)

        if node.leaf:
            return None

        return self.search(key, node.children[i])

    def insert(self, key):
        root = self.root
        if len(root.keys) == 2 * self.t - 1:
            # Root is full — split
            new_root = BTreeNode(leaf=False)
            new_root.children.append(self.root)
            self._split_child(new_root, 0)
            self.root = new_root
        self._insert_non_full(self.root, key)

    def _insert_non_full(self, node, key):
        i = len(node.keys) - 1
        if node.leaf:
            node.keys.append(None)
            while i >= 0 and key < node.keys[i]:
                node.keys[i + 1] = node.keys[i]
                i -= 1
            node.keys[i + 1] = key
        else:
            while i >= 0 and key < node.keys[i]:
                i -= 1
            i += 1
            if len(node.children[i].keys) == 2 * self.t - 1:
                self._split_child(node, i)
                if key > node.keys[i]:
                    i += 1
            self._insert_non_full(node.children[i], key)

    def _split_child(self, parent, i):
        t = self.t
        child = parent.children[i]
        new_node = BTreeNode(leaf=child.leaf)

        parent.keys.insert(i, child.keys[t - 1])
        parent.children.insert(i + 1, new_node)

        new_node.keys = child.keys[t:]
        child.keys = child.keys[:t - 1]

        if not child.leaf:
            new_node.children = child.children[t:]
            child.children = child.children[:t]

    def print_tree(self, node=None, level=0):
        if node is None:
            node = self.root
        print(f"Level {level}: {node.keys}")
        if not node.leaf:
            for child in node.children:
                self.print_tree(child, level + 1)


# Usage
btree = BTree(t=2)  # 2-3-4 tree (min degree 2)
for key in [10, 20, 5, 6, 12, 30, 7, 17]:
    btree.insert(key)

btree.print_tree()
# Level 0: [10]
# Level 1: [5, 6, 7]
# Level 1: [12, 17, 20, 30]

result = btree.search(12)
print(f"Found: {result is not None}")  # Found: True
```

### 4.2 Memory Profiling

```python
import sys

# Size of common objects
print(sys.getsizeof(0))          # 28 bytes
print(sys.getsizeof(1))          # 28 bytes
print(sys.getsizeof(""))         # 49 bytes
print(sys.getsizeof("hello"))    # 54 bytes
print(sys.getsizeof([]))         # 56 bytes
print(sys.getsizeof([1,2,3]))    # 80 bytes
print(sys.getsizeof({}))         # 64 bytes
print(sys.getsizeof(set()))      # 216 bytes
```

---

## 5. MCQs (12 Questions)

**Q1.** Python's primary memory management technique is:
- A) Manual allocation
- B) Reference counting + garbage collection
- C) Stack-based allocation only
- D) Compile-time memory allocation

**Answer:** B) Reference counting + garbage collection

---

**Q2.** What does `sys.getrefcount(x)` return?
- A) Memory address of x
- B) Size of x in bytes
- C) Number of references pointing to x
- D) The type of x

**Answer:** C) Number of references pointing to x (note: the function argument itself adds 1)

---

**Q3.** Python's garbage collector handles:
- A) All memory deallocation
- B) Circular references that reference counting can't clean
- C) Only string objects
- D) Disk storage

**Answer:** B) Circular references

---

**Q4.** `__slots__` in a Python class:
- A) Adds more methods
- B) Restricts attributes and reduces memory by eliminating `__dict__`
- C) Makes the class immutable
- D) Enables garbage collection

**Answer:** B) Restricts attributes and reduces memory by eliminating `__dict__`

---

**Q5.** `a is b` in Python checks:
- A) If values are equal
- B) If they refer to the same object in memory (same `id`)
- C) If types are the same
- D) If both are None

**Answer:** B) If they refer to the same object in memory

---

**Q6.** A B-Tree of order $m$ has at most how many keys per node?
- A) $m$
- B) $m - 1$
- C) $m + 1$
- D) $2m$

**Answer:** B) $m - 1$

---

**Q7.** All leaves in a B-Tree are at:
- A) Different levels
- B) The same level
- C) Level 0
- D) The root level

**Answer:** B) The same level

---

**Q8.** B-Trees are primarily designed for:
- A) In-memory data structures
- B) Minimizing disk I/O in databases and file systems
- C) Graph algorithms
- D) String processing

**Answer:** B) Minimizing disk I/O in databases and file systems

---

**Q9.** Search time complexity in a B-Tree with $n$ keys:
- A) $O(n)$
- B) $O(\log n)$
- C) $O(n \log n)$
- D) $O(1)$

**Answer:** B) $O(\log n)$ — specifically $O(\log_m n)$ disk accesses.

---

**Q10.** Python caches integers in which range?
- A) 0 to 100
- B) -5 to 256
- C) -128 to 127
- D) All integers

**Answer:** B) -5 to 256

---

**Q11.** When a B-Tree node is full and a new key must be inserted:
- A) The key is discarded
- B) The node is **split** into two nodes
- C) The tree is rebuilt
- D) An error occurs

**Answer:** B) The node is split into two nodes, with the median key promoted to the parent.

---

**Q12.** Python's generational garbage collection has how many generations?
- A) 1
- B) 2
- C) 3
- D) 4

**Answer:** C) 3 — Generation 0 (youngest), 1, and 2 (oldest).
