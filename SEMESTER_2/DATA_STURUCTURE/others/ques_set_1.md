# 📝 End Semester Examination (Guess Paper)

### Course: CDA 104 — Programming and Data Structures with Python
### Institution: Indian Institute of Technology Patna
### Session: January – April 2026
### Duration: 2 Hours | Maximum Marks: 100
### Total Questions: 50 (42 MCQs + 8 Coding Questions)

---

> **Instructions:**
> 1. **Section A (MCQs):** 42 questions × 1.5 marks = 63 marks. Choose the correct option.
> 2. **Section B (Coding):** 8 questions (marks indicated per question) = 37 marks. Write Python code.
> 3. All questions are compulsory.
> 4. No external libraries allowed unless specified.

---

---

# SECTION A — Multiple Choice Questions (42 × 1.5 = 63 Marks)

---

### Algorithm Analysis & Complexity

**Q1.** What is the time complexity of the following code?

```python
for i in range(n):
    j = 1
    while j < n:
        j = j * 2
```

- A) $O(n)$
- B) $O(n^2)$
- C) $O(n \log n)$
- D) $O(\log n)$

---

**Q2.** Which of the following represents the correct order of growth rates from slowest to fastest growing?

- A) $O(1) < O(\log n) < O(n) < O(n \log n) < O(n^2) < O(2^n)$
- B) $O(1) < O(n) < O(\log n) < O(n^2) < O(n \log n) < O(2^n)$
- C) $O(\log n) < O(1) < O(n) < O(n^2) < O(n \log n) < O(2^n)$
- D) $O(1) < O(\log n) < O(n) < O(n^2) < O(n \log n) < O(2^n)$

---

**Q3.** The amortized time complexity of `append()` on a Python list (dynamic array) is:

- A) $O(n)$
- B) $O(\log n)$
- C) $O(1)$
- D) $O(n^2)$

---

**Q4.** If $f(n) = 5n^3 + 2n^2 + 100n + 42$, then $f(n)$ is:

- A) $O(n^2)$
- B) $O(n^3)$
- C) $O(n^4)$
- D) $O(5n^3)$

---

### Recursion

**Q5.** What is the output of the following recursive function?

```python
def mystery(n):
    if n <= 0:
        return 0
    return n + mystery(n - 2)

print(mystery(7))
```

- A) 12
- B) 16
- C) 28
- D) 7

---

**Q6.** The time complexity of the naive recursive Fibonacci function `fib(n)` is:

- A) $O(n)$
- B) $O(n^2)$
- C) $O(2^n)$
- D) $O(n \log n)$

---

**Q7.** How many moves are required to solve the Tower of Hanoi problem for $n = 5$ disks?

- A) 25
- B) 32
- C) 31
- D) 63

---

**Q8.** What is the maximum depth of the call stack when executing `factorial(100)` (standard recursive implementation)?

- A) 1
- B) 50
- C) 100
- D) 101

---

### OOP Concepts

**Q9.** What is the output of the following code?

```python
class A:
    def show(self):
        print("A", end="")

class B(A):
    def show(self):
        print("B", end="")

class C(A):
    def show(self):
        print("C", end="")

class D(B, C):
    pass

obj = D()
obj.show()
```

- A) A
- B) B
- C) C
- D) Error

---

**Q10.** In Python, `self.__x` makes the variable:

- A) Public
- B) Protected
- C) Private (name mangled)
- D) Static

---

### Array-Based Sequences

**Q11.** What is the time complexity of inserting an element at index 0 in a Python list of size $n$?

- A) $O(1)$
- B) $O(\log n)$
- C) $O(n)$
- D) $O(n^2)$

---

**Q12.** What does the following code print?

```python
arr = [1, 2, 3, 4, 5]
print(arr[-2:] + arr[:2])
```

- A) `[4, 5, 1, 2]`
- B) `[5, 4, 1, 2]`
- C) `[3, 4, 1, 2]`
- D) `[4, 5, 2, 1]`

---

### Stacks

**Q13.** Consider the following sequence of stack operations on an initially empty stack:
`push(5), push(3), pop(), push(7), push(2), pop(), pop(), push(9), pop(), pop()`

The sequence of popped values is:

- A) 3, 2, 7, 9, 5
- B) 5, 3, 7, 2, 9
- C) 3, 7, 2, 9, 5
- D) 3, 2, 9, 7, 5

---

**Q14.** The postfix expression `6 3 2 * + 5 -` evaluates to:

- A) 7
- B) 13
- C) 1
- D) 5

---

**Q15.** Which data structure is used to convert an infix expression to postfix?

- A) Queue
- B) Stack
- C) Linked list
- D) Heap

---

**Q16.** What is the result of checking if the string `"({[}])"` has balanced brackets?

- A) Balanced
- B) Not Balanced
- C) Error
- D) Cannot determine

---

### Queues and Deques

**Q17.** In a circular queue of capacity 8 (0-indexed), if `front = 5` and `size = 4`, the index of the last (rear) element is:

- A) 0
- B) 1
- C) 8
- D) 9

---

**Q18.** Which data structure is most appropriate for implementing BFS (Breadth-First Search)?

- A) Stack
- B) Queue
- C) Priority Queue
- D) Deque

---

**Q19.** What is the time complexity of `dequeue()` in a circular array-based queue?

- A) $O(n)$
- B) $O(\log n)$
- C) $O(1)$
- D) $O(n^2)$

---

### Linked Lists

**Q20.** What is the time complexity of inserting a node at the beginning of a singly linked list?

- A) $O(n)$
- B) $O(\log n)$
- C) $O(1)$
- D) $O(n^2)$

---

**Q21.** In a doubly linked list, deleting a node (given a pointer to it) takes:

- A) $O(1)$
- B) $O(n)$
- C) $O(\log n)$
- D) $O(n^2)$

---

**Q22.** Floyd's cycle detection algorithm uses:

- A) One pointer
- B) Two pointers moving at different speeds
- C) A hash set
- D) Recursion only

---

**Q23.** After reversing the singly linked list `1 → 2 → 3 → 4 → None`, the resulting list is:

- A) `4 → 3 → 2 → 1 → None`
- B) `1 → 4 → 3 → 2 → None`
- C) `1 → 2 → 3 → 4 → None`
- D) `4 → 1 → 2 → 3 → None`

---

### Trees

**Q24.** For the following binary tree, what is the **inorder** traversal?

```
        10
       /  \
      5    15
     / \     \
    3   7    20
```

- A) `10, 5, 3, 7, 15, 20`
- B) `3, 5, 7, 10, 15, 20`
- C) `3, 7, 5, 20, 15, 10`
- D) `10, 5, 15, 3, 7, 20`

---

**Q25.** The **preorder** traversal of the same tree is:

- A) `10, 5, 3, 7, 15, 20`
- B) `3, 5, 7, 10, 15, 20`
- C) `3, 7, 5, 20, 15, 10`
- D) `10, 15, 20, 5, 3, 7`

---

**Q26.** In a binary tree with $n$ nodes, the maximum possible height is:

- A) $\log_2 n$
- B) $n - 1$
- C) $n$
- D) $n / 2$

---

**Q27.** A **complete binary tree** with 15 nodes has a height of:

- A) 3
- B) 4
- C) 5
- D) 15

---

**Q28.** In any binary tree, if the number of leaf nodes is $L$ and the number of internal nodes with degree 2 is $I$, then:

- A) $L = I$
- B) $L = I + 1$
- C) $L = I - 1$
- D) $L = 2I$

---

### Priority Queues and Heaps

**Q29.** In a min-heap stored as an array `[2, 5, 8, 10, 12, 9, 11]`, after inserting `1`, the new root will be:

- A) 2
- B) 1
- C) 5
- D) 8

---

**Q30.** The parent of a node at index $i$ (0-indexed) in a heap array is located at:

- A) $2i + 1$
- B) $2i + 2$
- C) $\lfloor (i - 1) / 2 \rfloor$
- D) $i / 2$

---

**Q31.** Building a heap from an unsorted array of $n$ elements (bottom-up) takes:

- A) $O(n \log n)$
- B) $O(n)$
- C) $O(n^2)$
- D) $O(\log n)$

---

### Hash Tables

**Q32.** Using the division hash function $h(k) = k \mod 7$, the keys `15, 22, 29` all hash to index:

- A) 0
- B) 1
- C) 7
- D) 3

---

**Q33.** In a hash table with **linear probing**, what is the main disadvantage?

- A) Uses too much memory
- B) Primary clustering
- C) Cannot handle deletions
- D) Requires sorted keys

---

**Q34.** The average-case time complexity of search in a hash table with a good hash function is:

- A) $O(n)$
- B) $O(\log n)$
- C) $O(1)$
- D) $O(n \log n)$

---

### Search Trees (BST & AVL)

**Q35.** Inserting the sequence `[50, 30, 70, 20, 40, 60, 80]` into an empty BST, the **inorder traversal** produces:

- A) `50, 30, 70, 20, 40, 60, 80`
- B) `20, 30, 40, 50, 60, 70, 80`
- C) `80, 70, 60, 50, 40, 30, 20`
- D) `50, 30, 20, 40, 70, 60, 80`

---

**Q36.** When deleting a node with **two children** in a BST, it is replaced by its:

- A) Left child
- B) Right child
- C) Inorder successor or inorder predecessor
- D) Parent

---

**Q37.** An AVL tree requires that the balance factor of every node is:

- A) 0
- B) $\{-1, 0, 1\}$
- C) $\{0, 1, 2\}$
- D) Any value

---

**Q38.** Inserting keys `[1, 2, 3]` into an empty AVL tree requires which rotation after inserting 3?

- A) Right rotation (LL case)
- B) Left rotation (RR case)
- C) Left-Right rotation (LR case)
- D) Right-Left rotation (RL case)

---

### Sorting

**Q39.** Which sorting algorithm has the best worst-case time complexity among the following?

- A) Quick Sort
- B) Bubble Sort
- C) Merge Sort
- D) Insertion Sort

---

**Q40.** Quick Sort's worst-case time complexity is:

- A) $O(n \log n)$
- B) $O(n)$
- C) $O(n^2)$
- D) $O(2^n)$

---

**Q41.** Which of the following sorting algorithms is **NOT stable**?

- A) Merge Sort
- B) Insertion Sort
- C) Bubble Sort
- D) Selection Sort

---

### Graph Algorithms

**Q42.** The time complexity of BFS/DFS on a graph with $V$ vertices and $E$ edges (adjacency list) is:

- A) $O(V^2)$
- B) $O(V + E)$
- C) $O(V \cdot E)$
- D) $O(E^2)$

---

---

# SECTION B — Coding Questions (37 Marks)

---

### Q43. [4 Marks] — Recursion

Write a **recursive** Python function `power(base, exp)` that computes $base^{exp}$ in $O(\log n)$ time using the fast exponentiation technique (also called exponentiation by squaring).

**Example:**
```
power(2, 10)  → 1024
power(3, 5)   → 243
```

---

### Q44. [4 Marks] — Stack Application

Write a Python function `evaluate_postfix(expression)` that takes a postfix expression as a string (tokens separated by spaces) and returns the result.

Supported operators: `+`, `-`, `*`, `/` (integer division).

**Example:**
```
evaluate_postfix("5 1 2 + 4 * + 3 -")  → 14
```

---

### Q45. [5 Marks] — Linked List

Write a Python function `detect_and_find_cycle_start(head)` that:
1. Detects if a cycle exists in a singly linked list using **Floyd's algorithm**.
2. If a cycle exists, returns the **node where the cycle begins**.
3. If no cycle exists, returns `None`.

Assume the `Node` class has `data` and `next` attributes.

---

### Q46. [5 Marks] — Binary Search Tree

Write Python functions for a BST:
1. `bst_insert(root, key)` — Insert a key into the BST.
2. `find_kth_smallest(root, k)` — Find the $k$-th smallest element in the BST using inorder traversal.

**Example:** For BST with keys `[8, 3, 10, 1, 6, 14, 4, 7]`, `find_kth_smallest(root, 3)` should return `4`.

---

### Q47. [5 Marks] — Sorting

Implement the **in-place Quick Sort** algorithm in Python with the Lomuto partition scheme. Your function should:
1. Partition the array around the **last element** as the pivot.
2. Recursively sort the subarrays.

```python
def quick_sort(arr, low, high):
    # Your code here

def partition(arr, low, high):
    # Your code here
```

**Example:**
```
arr = [10, 80, 30, 90, 40, 50, 70]
quick_sort(arr, 0, len(arr) - 1)
# arr → [10, 30, 40, 50, 70, 80, 90]
```

---

### Q48. [5 Marks] — Graph Algorithms

Write a Python function `dijkstra(graph, start)` that finds the shortest path from `start` to all other nodes in a weighted graph using Dijkstra's algorithm.

The graph is given as an adjacency list: `{node: [(neighbor, weight), ...]}`.

Return a dictionary of shortest distances from `start` to every other node.

**Example:**
```python
graph = {
    'A': [('B', 4), ('C', 1)],
    'B': [('A', 4), ('D', 1)],
    'C': [('A', 1), ('B', 2), ('D', 5)],
    'D': [('B', 1), ('C', 5)]
}
dijkstra(graph, 'A')  → {'A': 0, 'B': 3, 'C': 1, 'D': 4}
```

---

### Q49. [5 Marks] — Hash Table

Implement a **Hash Table with Separate Chaining** in Python. Your class should support:
1. `put(key, value)` — Insert or update a key-value pair.
2. `get(key)` — Retrieve the value for a key (raise `KeyError` if not found).
3. `delete(key)` — Remove a key-value pair.

Use the hash function: $h(key) = \text{sum of ASCII values of characters} \mod \text{table\_size}$

---

### Q50. [4 Marks] — Trees and Text Processing

Given a list of words, build a **Trie** (prefix tree) and implement the following methods:
1. `insert(word)` — Insert a word into the Trie.
2. `search(word)` — Return `True` if the word exists in the Trie.
3. `starts_with(prefix)` — Return `True` if any word in the Trie starts with the given prefix.

**Example:**
```
Insert: "apple", "app", "bat", "ball"
search("app")       → True
search("ap")        → False
starts_with("ba")   → True
starts_with("cat")  → False
```

---

---

# ANSWER KEY — SECTION A (MCQs)

---

| Q# | Answer | Explanation |
|----|--------|-------------|
| 1 | **C** | Outer loop $O(n)$, inner while-loop doubles $j$ so $O(\log n)$ → total $O(n \log n)$ |
| 2 | **A** | Correct ascending order of growth rates |
| 3 | **C** | Dynamic array resizing gives amortized $O(1)$ |
| 4 | **B** | Drop lower-order terms and constants: $O(n^3)$ |
| 5 | **B** | $7 + 5 + 3 + 1 = 16$ (only odd values since decreasing by 2) |
| 6 | **C** | Naive recursive Fibonacci has overlapping subproblems → $O(2^n)$ |
| 7 | **C** | Tower of Hanoi: $2^n - 1 = 2^5 - 1 = 31$ moves |
| 8 | **D** | `factorial(100)` calls `factorial(99)`...`factorial(0)` → 101 frames (including initial call) |
| 9 | **B** | MRO (Method Resolution Order) follows C3 linearization: D → B → C → A. `B.show()` is found first |
| 10 | **C** | Double underscore prefix triggers name mangling → private attribute |
| 11 | **C** | Inserting at index 0 shifts all $n$ elements → $O(n)$ |
| 12 | **A** | `arr[-2:]` = `[4,5]`, `arr[:2]` = `[1,2]` → `[4, 5, 1, 2]` |
| 13 | **A** | Trace: push 5,3 → pop 3; push 7,2 → pop 2, pop 7; push 9 → pop 9, pop 5. Popped: 3,2,7,9,5 |
| 14 | **A** | `6 3 2 * + 5 -` → `3*2=6`, `6+6=12`, `12-5=7` |
| 15 | **B** | Stack is used to hold operators during infix to postfix conversion |
| 16 | **B** | `{` is matched with `)` → mismatch → Not Balanced |
| 17 | **A** | Last element index = $(front + size - 1) \mod capacity = (5 + 4 - 1) \mod 8 = 0$ |
| 18 | **B** | BFS uses a Queue for level-by-level exploration |
| 19 | **C** | Circular queue dequeue is $O(1)$ — just update front pointer |
| 20 | **C** | Insert at beginning: create node, set next to head, update head → $O(1)$ |
| 21 | **A** | With a pointer to the node in a doubly linked list, update prev/next pointers → $O(1)$ |
| 22 | **B** | Floyd's algorithm uses slow (1 step) and fast (2 steps) pointers |
| 23 | **A** | Standard linked list reversal flips all pointers → `4 → 3 → 2 → 1 → None` |
| 24 | **B** | Inorder (left, root, right): `3, 5, 7, 10, 15, 20` (sorted order for BST) |
| 25 | **A** | Preorder (root, left, right): `10, 5, 3, 7, 15, 20` |
| 26 | **B** | Worst case: skewed tree (like a linked list) → height = $n - 1$ |
| 27 | **A** | $2^{h+1} - 1 = 15$ → $h = 3$ (height is number of edges from root to deepest leaf) |
| 28 | **B** | For any binary tree: $L = I + 1$ (leaf nodes = nodes with 2 children + 1) |
| 29 | **B** | Inserting 1 triggers bubble-up to root since 1 < 2 → new root is 1 |
| 30 | **C** | Parent index = $\lfloor (i - 1) / 2 \rfloor$ for 0-indexed array |
| 31 | **B** | Bottom-up heap construction (heapify) is $O(n)$ |
| 32 | **B** | $15 \mod 7 = 1$, $22 \mod 7 = 1$, $29 \mod 7 = 1$ → all map to index 1 |
| 33 | **B** | Linear probing causes primary clustering — long chains of occupied slots |
| 34 | **C** | Average case with good hash function and low load factor is $O(1)$ |
| 35 | **B** | Inorder traversal of any BST always produces sorted order: `20, 30, 40, 50, 60, 70, 80` |
| 36 | **C** | Replace with inorder successor (smallest in right subtree) or predecessor (largest in left) |
| 37 | **B** | AVL balance factor must be in $\{-1, 0, 1\}$ for every node |
| 38 | **B** | Inserting 1,2,3 creates right-skewed tree → RR case → Left rotation |
| 39 | **C** | Merge Sort: $O(n \log n)$ in all cases. Quick Sort worst case is $O(n^2)$ |
| 40 | **C** | Quick Sort worst case (already sorted + bad pivot) → $O(n^2)$ |
| 41 | **D** | Selection Sort is not stable — it can swap non-adjacent elements |
| 42 | **B** | BFS/DFS visits each vertex once and each edge once → $O(V + E)$ |

---

# ANSWER KEY — SECTION B (Coding)

---

### Q43 — Fast Exponentiation

```python
def power(base, exp):
    if exp == 0:
        return 1
    if exp % 2 == 0:
        half = power(base, exp // 2)
        return half * half
    else:
        return base * power(base, exp - 1)
```

---

### Q44 — Postfix Evaluation

```python
def evaluate_postfix(expression):
    stack = []
    for token in expression.split():
        if token.lstrip('-').isdigit():
            stack.append(int(token))
        else:
            b = stack.pop()
            a = stack.pop()
            if token == '+': stack.append(a + b)
            elif token == '-': stack.append(a - b)
            elif token == '*': stack.append(a * b)
            elif token == '/': stack.append(int(a / b))
    return stack.pop()
```

---

### Q45 — Floyd's Cycle Detection + Find Start

```python
def detect_and_find_cycle_start(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            # Cycle found — find start
            slow = head
            while slow != fast:
                slow = slow.next
                fast = fast.next
            return slow  # Start of cycle
    return None  # No cycle
```

---

### Q46 — BST Insert + Kth Smallest

```python
class BSTNode:
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None

def bst_insert(root, key):
    if root is None:
        return BSTNode(key)
    if key < root.key:
        root.left = bst_insert(root.left, key)
    elif key > root.key:
        root.right = bst_insert(root.right, key)
    return root

def find_kth_smallest(root, k):
    result = []
    def inorder(node):
        if node and len(result) < k:
            inorder(node.left)
            result.append(node.key)
            inorder(node.right)
    inorder(root)
    return result[k - 1] if k <= len(result) else None
```

---

### Q47 — In-Place Quick Sort (Lomuto)

```python
def quick_sort(arr, low, high):
    if low < high:
        pi = partition(arr, low, high)
        quick_sort(arr, low, pi - 1)
        quick_sort(arr, pi + 1, high)

def partition(arr, low, high):
    pivot = arr[high]
    i = low - 1
    for j in range(low, high):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1
```

---

### Q48 — Dijkstra's Algorithm

```python
import heapq

def dijkstra(graph, start):
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    pq = [(0, start)]

    while pq:
        current_dist, current = heapq.heappop(pq)
        if current_dist > distances[current]:
            continue
        for neighbor, weight in graph[current]:
            distance = current_dist + weight
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(pq, (distance, neighbor))

    return distances
```

---

### Q49 — Hash Table with Separate Chaining

```python
class HashTable:
    def __init__(self, size=10):
        self.size = size
        self.table = [[] for _ in range(size)]

    def _hash(self, key):
        return sum(ord(c) for c in str(key)) % self.size

    def put(self, key, value):
        idx = self._hash(key)
        for i, (k, v) in enumerate(self.table[idx]):
            if k == key:
                self.table[idx][i] = (key, value)
                return
        self.table[idx].append((key, value))

    def get(self, key):
        idx = self._hash(key)
        for k, v in self.table[idx]:
            if k == key:
                return v
        raise KeyError(key)

    def delete(self, key):
        idx = self._hash(key)
        for i, (k, v) in enumerate(self.table[idx]):
            if k == key:
                del self.table[idx][i]
                return
        raise KeyError(key)
```

---

### Q50 — Trie (Prefix Tree)

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end = True

    def search(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end

    def starts_with(self, prefix):
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True
```

---

> **💡 Preparation Tips:**
> - Focus on time/space complexities — at least 8-10 questions test this directly.
> - Practice tracing through traversals (inorder, preorder, postorder) by hand.
> - Know heap operations (bubble-up, bubble-down) and array representation.
> - Understand collision resolution in hash tables (chaining vs open addressing).
> - Be comfortable writing recursive solutions and iterative alternatives.
> - Graph questions: BFS/DFS traversal order and Dijkstra's are high-probability topics.
> - AVL rotations: Know which rotation applies for LL, RR, LR, RL cases.

---

*All the best!* 🎯
