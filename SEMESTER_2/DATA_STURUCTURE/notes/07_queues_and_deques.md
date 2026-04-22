# Topic 7: Queues and Deques

## 1. Overview

A **Queue** is a linear data structure that follows the **FIFO (First-In, First-Out)** principle — the first element added is the first one removed, like a line at a ticket counter.

A **Deque (Double-Ended Queue)** allows insertion and deletion at **both** ends.

---

## 2. Key Concepts

### 2.1 Queue Operations

| Operation | Description | Time Complexity |
|-----------|-------------|----------------|
| `enqueue(e)` | Add element to the rear | $O(1)$ |
| `dequeue()` | Remove and return front element | $O(1)$* |
| `front()` | Return front element without removing | $O(1)$ |
| `is_empty()` | Check if queue is empty | $O(1)$ |
| `len()` | Return number of elements | $O(1)$ |

*$O(1)$ with circular array or linked list; $O(n)$ with `list.pop(0)`

### 2.2 Deque Operations

| Operation | Description |
|-----------|-------------|
| `add_first(e)` | Add to front |
| `add_last(e)` | Add to rear |
| `delete_first()` | Remove from front |
| `delete_last()` | Remove from rear |
| `first()` | Peek at front |
| `last()` | Peek at rear |

### 2.3 Applications of Queues

- **Task scheduling** (CPU, print jobs)
- **Breadth-First Search (BFS)** in graphs/trees
- **Ticket counter / Customer service** simulation
- **Buffer** for data streams
- **Level-order tree traversal**

---

## 3. Code Examples

### 3.1 Queue Using Python List

*(From IIT Patna Lecture — Ticket Counter)*

```python
class Queue:
    def __init__(self):
        self._data = []

    def enqueue(self, e):
        self._data.append(e)

    def dequeue(self):
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self._data.pop(0)  # O(n) — not ideal

    def front(self):
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self._data[0]

    def is_empty(self):
        return len(self._data) == 0

    def __len__(self):
        return len(self._data)

    def __str__(self):
        return str(self._data)
```

### 3.2 Ticket Counter Simulation

*(From IIT Patna Lecture)*

```python
queue = []

def enqueue(name):
    queue.append(name)
    print(f"{name} has joined the ticket line.")

def dequeue():
    if not queue:
        print("No customers in line.")
    else:
        served = queue.pop(0)
        print(f"{served} got the ticket and left the line.")

def display():
    if not queue:
        print("The queue is empty.")
    else:
        print("Customers in line:", queue)

# Simulation
enqueue("Alice")     # Alice has joined the ticket line.
enqueue("Bob")       # Bob has joined the ticket line.
enqueue("Charlie")   # Charlie has joined the ticket line.
display()            # Customers in line: ['Alice', 'Bob', 'Charlie']
dequeue()            # Alice got the ticket and left the line.
display()            # Customers in line: ['Bob', 'Charlie']
```

### 3.3 Circular Queue (Efficient Implementation)

```python
class CircularQueue:
    DEFAULT_CAPACITY = 10

    def __init__(self):
        self._data = [None] * CircularQueue.DEFAULT_CAPACITY
        self._size = 0
        self._front = 0

    def __len__(self):
        return self._size

    def is_empty(self):
        return self._size == 0

    def front(self):
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self._data[self._front]

    def enqueue(self, e):
        if self._size == len(self._data):
            self._resize(2 * len(self._data))
        avail = (self._front + self._size) % len(self._data)
        self._data[avail] = e
        self._size += 1

    def dequeue(self):
        if self.is_empty():
            raise IndexError("Queue is empty")
        answer = self._data[self._front]
        self._data[self._front] = None
        self._front = (self._front + 1) % len(self._data)
        self._size -= 1
        return answer

    def _resize(self, cap):
        old = self._data
        self._data = [None] * cap
        walk = self._front
        for k in range(self._size):
            self._data[k] = old[walk]
            walk = (walk + 1) % len(old)
        self._front = 0

# Usage
cq = CircularQueue()
cq.enqueue(10)
cq.enqueue(20)
cq.enqueue(30)
print(cq.dequeue())  # 10
print(cq.front())    # 20
```

### 3.4 Queue Using collections.deque

```python
from collections import deque

q = deque()
q.append(10)      # enqueue
q.append(20)
q.append(30)
print(q.popleft()) # dequeue → 10 (O(1))
print(q)           # deque([20, 30])
```

### 3.5 Deque Implementation

```python
from collections import deque

d = deque()

# Add to both ends
d.append(10)       # add to rear:  [10]
d.append(20)       # add to rear:  [10, 20]
d.appendleft(5)    # add to front: [5, 10, 20]

# Remove from both ends
d.pop()            # remove from rear:  20 → [5, 10]
d.popleft()        # remove from front: 5  → [10]

# Peek
print(d[0])        # front: 10
print(d[-1])       # rear: 10

# Rotate
d = deque([1, 2, 3, 4, 5])
d.rotate(2)        # right rotate by 2: [4, 5, 1, 2, 3]
d.rotate(-2)       # left rotate by 2:  [1, 2, 3, 4, 5]
```

### 3.6 BFS Using Queue (Level-Order Tree Traversal)

```python
from collections import deque

class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None

def level_order(root):
    if not root:
        return []
    result = []
    queue = deque([root])
    while queue:
        node = queue.popleft()
        result.append(node.val)
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
    return result

# Build tree:    1
#              /   \
#             2     3
#            / \
#           4   5
root = TreeNode(1)
root.left = TreeNode(2)
root.right = TreeNode(3)
root.left.left = TreeNode(4)
root.left.right = TreeNode(5)

print(level_order(root))  # [1, 2, 3, 4, 5]
```

---

## 4. Queue vs Deque vs Stack

| Feature | Stack | Queue | Deque |
|---------|-------|-------|-------|
| Principle | LIFO | FIFO | Both ends |
| Insert | Top only | Rear only | Front or Rear |
| Remove | Top only | Front only | Front or Rear |
| Use case | Undo, DFS | BFS, scheduling | Sliding window, palindrome |

---

## 5. MCQs (15 Questions)

**Q1.** A queue follows which principle?
- A) LIFO
- B) FIFO
- C) Random access
- D) Priority-based

**Answer:** B) FIFO (First-In, First-Out)

---

**Q2.** Time complexity of `dequeue()` using `list.pop(0)` in Python?
- A) $O(1)$
- B) $O(\log n)$
- C) $O(n)$
- D) $O(n^2)$

**Answer:** C) $O(n)$ — All remaining elements are shifted.

---

**Q3.** Which Python module provides an efficient deque?
- A) `queue`
- B) `collections`
- C) `array`
- D) `itertools`

**Answer:** B) `collections` — `from collections import deque`

---

**Q4.** In a circular queue with capacity 5, if front=3 and size=4, where is the rear?
- A) Index 6
- B) Index 1
- C) Index 2
- D) Index 7

**Answer:** C) Index 2 — rear = $(3 + 4 - 1) \% 5 = 6 \% 5 = 1$. Wait, the next available slot is $(3 + 4) \% 5 = 2$. Last element is at index $(3+3)\%5 = 1$.

---

**Q5.** What is a deque?
- A) A queue that can only grow
- B) A double-ended queue (insert/remove at both ends)
- C) A queue with priority
- D) A stack with two tops

**Answer:** B) A double-ended queue (insert/remove at both ends)

---

**Q6.** Which traversal of a tree uses a queue?
- A) Inorder
- B) Preorder
- C) Postorder
- D) Level-order (BFS)

**Answer:** D) Level-order (BFS)

---

**Q7.** Operations: enqueue(1), enqueue(2), dequeue(), enqueue(3), dequeue(). What is returned?
- A) 1, 2
- B) 2, 3
- C) 1, 3
- D) 3, 1

**Answer:** A) 1, 2 — FIFO: first dequeue returns 1, second returns 2.

---

**Q8.** `deque.appendleft(x)` time complexity is:
- A) $O(n)$
- B) $O(\log n)$
- C) $O(1)$
- D) $O(n^2)$

**Answer:** C) $O(1)$

---

**Q9.** Which is NOT an application of queues?
- A) BFS
- B) Print job scheduling
- C) Undo operation
- D) CPU task scheduling

**Answer:** C) Undo operation — Uses a stack, not a queue.

---

**Q10.** A circular queue avoids which problem of linear queue?
- A) Overflow
- B) Wasted space at the front
- C) Underflow
- D) Slow access

**Answer:** B) Wasted space at the front — Circular queue reuses freed front positions.

---

**Q11.** In a deque, which operation is NOT possible?
- A) Insert at front
- B) Insert at rear
- C) Delete from middle
- D) Delete from rear

**Answer:** C) Delete from middle — Deque only supports operations at the two ends.

---

**Q12.** `deque.rotate(2)` on `[1, 2, 3, 4, 5]` gives:
- A) `[3, 4, 5, 1, 2]`
- B) `[4, 5, 1, 2, 3]`
- C) `[2, 3, 4, 5, 1]`
- D) `[5, 1, 2, 3, 4]`

**Answer:** B) `[4, 5, 1, 2, 3]` — Right rotation by 2.

---

**Q13.** How can you implement a stack using queues?
- A) Using 1 queue
- B) Using 2 queues
- C) It's not possible
- D) Both A and B

**Answer:** D) Both A and B — Possible with 1 queue (costly push) or 2 queues.

---

**Q14.** What is the worst-case space complexity of a queue with $n$ elements?
- A) $O(1)$
- B) $O(\log n)$
- C) $O(n)$
- D) $O(n^2)$

**Answer:** C) $O(n)$

---

**Q15.** Python's `collections.deque` is implemented as:
- A) Dynamic array
- B) Singly linked list
- C) Doubly linked list
- D) Circular array of fixed-size blocks

**Answer:** D) Circular array of fixed-size blocks (doubly-linked list of blocks)
