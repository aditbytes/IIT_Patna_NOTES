# 📚 DSA Practice Questions — IIT Patna Semester 2
### 100+ Questions | Based on Assignment & Quiz PDF

> **Source**: DSA Assignment & Quiz Question Bank (Compiled by Scholars' Hub)
> **Topics**: Arrays, Linked Lists, Stacks, Queues, Trees, Graphs, Sorting, Hashing

---

## 📖 Table of Contents
1. [Arrays & Basic Data Structures](#1-arrays--basic-data-structures)
2. [Linked Lists](#2-linked-lists)
3. [Stacks](#3-stacks)
4. [Queues](#4-queues)
5. [Trees](#5-trees)
6. [Binary Search Trees (BST)](#6-binary-search-trees)
7. [Graphs](#7-graphs)
8. [Sorting Algorithms](#8-sorting-algorithms)
9. [Hashing](#9-hashing)
10. [Coding / Implementation Problems](#10-coding--implementation-problems)
11. [Answer Key](#11-answer-key)

---

## 1. Arrays & Basic Data Structures

**Q1.** What is the time complexity of accessing an element in an array by index?
- a) O(n)
- b) O(log n)
- c) O(1) ✅
- d) O(n²)

**Q2.** What will the following code print?
```python
a = (1, 2, 3)
a[0] = 99
print(a)
```
- a) None
- b) Error (tuple is immutable) ✅
- c) (99, 2, 3)
- d) (1, 2, 3)

**Q3.** Which of the following is a mutable data type in Python?
- a) Tuple
- b) String
- c) List ✅
- d) Integer

**Q4.** What is the worst-case time complexity of searching an element in an unsorted array?
- a) O(1)
- b) O(log n)
- c) O(n) ✅
- d) O(n log n)

**Q5.** Which data structure stores elements in contiguous memory locations?
- a) Linked List
- b) Array ✅
- c) Tree
- d) Graph

**Q6.** What is the time complexity of inserting an element at the beginning of an array of size n?
- a) O(1)
- b) O(n) ✅
- c) O(log n)
- d) O(n²)

**Q7.** Which of the following represents a recursive data structure?
- a) Linked list ✅
- b) Dictionary
- c) Tuple
- d) List

**Q8.** In Python, what is the output of `len([1, [2, 3], 4])`?
- a) 4
- b) 5
- c) 3 ✅
- d) Error

**Q9.** What is the space complexity of an array of n elements?
- a) O(1)
- b) O(n) ✅
- c) O(n²)
- d) O(log n)

**Q10.** Which operation is NOT efficient on arrays?
- a) Accessing by index
- b) Insertion at the middle ✅
- c) Traversal
- d) Binary search on sorted array

---

## 2. Linked Lists

**Q11.** If a linked list has 6 nodes, how many NULL pointers does it contain (for singly linked list)?
- a) 6
- b) 2
- c) 1 ✅
- d) 5

**Q12.** Which operation is faster in a linked list compared to arrays?
- a) Traversal
- b) Insertion/deletion at beginning ✅
- c) Random access
- d) Binary search

**Q13.** Deletion from the end of a singly linked list (head pointer only) requires:
- a) O(n) ✅
- b) O(1)
- c) O(n log n)
- d) O(n²)

**Q14.** In a doubly linked list, each node contains:
- a) One pointer
- b) Two pointers ✅
- c) Three pointers
- d) No pointers

**Q15.** What is the advantage of a doubly linked list over a singly linked list?
- a) Uses less memory
- b) Can be traversed in both directions ✅
- c) Faster insertion at head
- d) Simpler implementation

**Q16.** In a circular linked list, the last node points to:
- a) NULL
- b) The first node ✅
- c) The second node
- d) Itself

**Q17.** Time complexity of searching for an element in a singly linked list:
- a) O(1)
- b) O(log n)
- c) O(n) ✅
- d) O(n²)

**Q18.** Which of the following is true about a singly linked list?
- a) It can be traversed in both directions
- b) Each node has two data fields
- c) The last node points to NULL ✅
- d) Insertion at any position takes O(1)

**Q19.** What is the space overhead of a singly linked list compared to an array?
- a) Less space
- b) Same space
- c) More space due to pointers ✅
- d) Depends on data type

**Q20.** Insertion at the beginning of a singly linked list takes:
- a) O(n)
- b) O(1) ✅
- c) O(log n)
- d) O(n²)

**Q21.** To reverse a singly linked list, the minimum number of pointers needed is:
- a) 1
- b) 2
- c) 3 ✅
- d) 4

**Q22.** A linked list where the last node points to the head is called:
- a) Doubly linked list
- b) Circular linked list ✅
- c) Skip list
- d) Header linked list

---

## 3. Stacks

**Q23.** A stack follows which principle?
- a) FIFO
- b) LIFO ✅
- c) LILO
- d) Random access

**Q24.** Pushing onto a stack that is already full results in:
- a) Overflow ✅
- b) Crash
- c) User flow
- d) Underflow

**Q25.** After the operations:
```python
stack = []
stack.append(1)
stack.pop()
stack.pop()
```
What happens?
- a) Returns -1
- b) Raises IndexError ✅
- c) Prints None
- d) Stack is empty, no error

**Q26.** Which data structure is used for function call management?
- a) Queue
- b) Array
- c) Stack ✅
- d) Linked List

**Q27.** The postfix expression `2 3 + 4 *` evaluates to:
- a) 14
- b) 20 ✅
- c) 24
- d) 10

**Q28.** Which application does NOT use a stack?
- a) Undo operation in text editor
- b) Expression evaluation
- c) CPU task scheduling ✅
- d) Recursion

**Q29.** Popping from an empty stack results in:
- a) Overflow
- b) Underflow ✅
- c) Null
- d) Zero

**Q30.** What is the time complexity of push and pop operations on a stack?
- a) O(n)
- b) O(log n)
- c) O(1) ✅
- d) O(n²)

**Q31.** Infix expression `A + B * C` in postfix is:
- a) AB+C*
- b) ABC*+ ✅
- c) +A*BC
- d) A+BC*

**Q32.** Which data structure is used for converting infix to postfix?
- a) Queue
- b) Stack ✅
- c) Tree
- d) Graph

**Q33.** The prefix expression for `(A + B) * C` is:
- a) *+ABC ✅
- b) +A*BC
- c) ABC+*
- d) *A+BC

**Q34.** What is the minimum number of stacks needed to implement a queue?
- a) 1
- b) 2 ✅
- c) 3
- d) 4

---

## 4. Queues

**Q35.** A queue follows which principle?
- a) LIFO
- b) FIFO ✅
- c) LILO
- d) Random

**Q36.** Which application most commonly uses a queue?
- a) Infix to postfix conversion
- b) Function call recursion
- c) CPU task scheduling ✅
- d) Undo operation in text editor

**Q37.** In a circular queue of size N, the next position after index N-1 is:
- a) N
- b) N+1
- c) 0 ✅
- d) -1

**Q38.** Dequeue operation removes an element from:
- a) Rear
- b) Front ✅
- c) Middle
- d) Any position

**Q39.** A priority queue removes elements based on:
- a) Insertion order
- b) Priority value ✅
- c) Random selection
- d) Size

**Q40.** Time complexity of enqueue and dequeue in a queue (array implementation):
- a) O(n)
- b) O(1) ✅
- c) O(log n)
- d) O(n²)

**Q41.** In a circular queue, how do you check if the queue is full?
- a) front == rear
- b) (rear + 1) % size == front ✅
- c) rear == size
- d) front == 0

**Q42.** A double-ended queue (deque) allows:
- a) Insertion at front only
- b) Deletion at rear only
- c) Insertion and deletion at both ends ✅
- d) Random access

**Q43.** Which data structure is best for BFS traversal?
- a) Stack
- b) Queue ✅
- c) Array
- d) Heap

**Q44.** The circular queue solves which problem of linear queue?
- a) Overflow
- b) Underflow
- c) Memory wastage ✅
- d) Slow access

---

## 5. Trees

**Q45.** Given the root of a binary tree, its maximum depth is defined as:
- a) Number of edges from root to nearest leaf
- b) Number of nodes along the longest path from root to farthest leaf ✅
- c) Total number of nodes
- d) Number of leaf nodes

**Q46.** In a full binary tree, if the number of internal nodes is i, the number of leaf nodes is:
- a) i + 1 ✅
- b) i - 1
- c) 2i
- d) i

**Q47.** The maximum number of nodes at level L of a binary tree is:
- a) 2L
- b) 2^L ✅
- c) L²
- d) 2L + 1

**Q48.** Which traversal visits nodes in order: Left → Root → Right?
- a) Preorder
- b) Inorder ✅
- c) Postorder
- d) Level order

**Q49.** Which traversal visits nodes in order: Root → Left → Right?
- a) Preorder ✅
- b) Inorder
- c) Postorder
- d) Level order

**Q50.** The height of a complete binary tree with n nodes is:
- a) n
- b) n/2
- c) ⌊log₂n⌋ ✅
- d) 2n

**Q51.** A binary tree where every node has 0 or 2 children is called:
- a) Complete binary tree
- b) Full binary tree ✅
- c) Perfect binary tree
- d) Balanced binary tree

**Q52.** Level order traversal of a binary tree uses which data structure?
- a) Stack
- b) Queue ✅
- c) Array
- d) Linked List

**Q53.** Given: `root = [3,9,20,null,null,15,7]`, the maximum depth is:
- a) 2
- b) 3 ✅
- c) 4
- d) 5

**Q54.** Given: `root = [1,null,2]`, the maximum depth is:
- a) 1
- b) 2 ✅
- c) 3
- d) 0

**Q55.** The number of edges in a tree with n nodes is:
- a) n
- b) n + 1
- c) n - 1 ✅
- d) 2n

**Q56.** A binary tree with all levels completely filled is called:
- a) Full binary tree
- b) Complete binary tree
- c) Perfect binary tree ✅
- d) Skewed binary tree

---

## 6. Binary Search Trees

**Q57.** In a BST, the left subtree of a node contains:
- a) Keys greater than the node
- b) Keys less than the node ✅
- c) Keys equal to the node
- d) Random keys

**Q58.** The worst-case time complexity of search in a BST is:
- a) O(log n)
- b) O(n) ✅
- c) O(1)
- d) O(n²)

**Q59.** Inorder traversal of a BST gives:
- a) Random order
- b) Descending order
- c) Ascending (sorted) order ✅
- d) Level order

**Q60.** The best-case time complexity of search in a balanced BST is:
- a) O(n)
- b) O(log n) ✅
- c) O(1)
- d) O(n log n)

**Q61.** To delete a node with two children in a BST, we replace it with:
- a) Left child
- b) Right child
- c) Inorder successor or predecessor ✅
- d) Parent node

**Q62.** The minimum value in a BST is found at:
- a) Root
- b) Rightmost node
- c) Leftmost node ✅
- d) Any leaf

**Q63.** A BST degenerates into a linked list when elements are inserted in:
- a) Random order
- b) Sorted order ✅
- c) Reverse sorted order
- d) Both b and c ✅

**Q64.** Time complexity of insertion in a balanced BST:
- a) O(1)
- b) O(n)
- c) O(log n) ✅
- d) O(n²)

---

## 7. Graphs

**Q65.** In an undirected graph, the sum of degrees of all vertices is:
- a) E²
- b) 2E ✅
- c) E
- d) V

**Q66.** BFS (Breadth-First Search) uses which data structure?
- a) Stack
- b) Queue ✅
- c) Priority Queue
- d) Array

**Q67.** DFS (Depth-First Search) uses which data structure?
- a) Queue
- b) Stack ✅
- c) Priority Queue
- d) Heap

**Q68.** Time complexity of BFS with adjacency list representation:
- a) O(V)
- b) O(E)
- c) O(V + E) ✅
- d) O(V * E)

**Q69.** Time complexity of Kruskal's algorithm (with sorting):
- a) O(E log E) ✅
- b) O(V)
- c) O(E)
- d) O(V²)

**Q70.** A spanning tree of a graph with V vertices has exactly:
- a) V edges
- b) V - 1 edges ✅
- c) V + 1 edges
- d) 2V edges

**Q71.** Which algorithm is used to find the shortest path in an unweighted graph?
- a) Dijkstra's
- b) BFS ✅
- c) DFS
- d) Kruskal's

**Q72.** A graph with no cycles is called:
- a) Complete graph
- b) Acyclic graph ✅
- c) Bipartite graph
- d) Regular graph

**Q73.** The adjacency matrix of an undirected graph is:
- a) Upper triangular
- b) Lower triangular
- c) Symmetric ✅
- d) Diagonal

**Q74.** Space complexity of adjacency matrix for a graph with V vertices:
- a) O(V)
- b) O(V²) ✅
- c) O(E)
- d) O(V + E)

**Q75.** Space complexity of adjacency list for a graph:
- a) O(V²)
- b) O(V + E) ✅
- c) O(E²)
- d) O(V)

**Q76.** A connected graph with V vertices and exactly V-1 edges is a:
- a) Complete graph
- b) Tree ✅
- c) Cycle
- d) Bipartite graph

**Q77.** Kruskal's algorithm is used to find:
- a) Shortest path
- b) Minimum Spanning Tree ✅
- c) Maximum flow
- d) Topological sort

**Q78.** Prim's algorithm uses which data structure for efficiency?
- a) Stack
- b) Queue
- c) Min-heap / Priority Queue ✅
- d) Array

**Q79.** Which algorithm detects a cycle in an undirected graph?
- a) BFS
- b) DFS
- c) Union-Find ✅
- d) All of the above ✅

**Q80.** A directed graph with no cycles is called:
- a) DAG (Directed Acyclic Graph) ✅
- b) Tree
- c) Forest
- d) Complete graph

**Q81.** Topological sorting is possible only for:
- a) Undirected graphs
- b) DAGs ✅
- c) Complete graphs
- d) Cyclic graphs

---

## 8. Sorting Algorithms

**Q82.** When does the best case of Bubble Sort occur (sorting in ascending order)?
- a) No best case, always O(n²)
- b) Elements are not sorted
- c) Elements are sorted in descending order
- d) Elements are already sorted in ascending order ✅

**Q83.** Best-case time complexity of Bubble Sort (optimized):
- a) O(n²)
- b) O(n) ✅
- c) O(n log n)
- d) O(1)

**Q84.** Worst-case time complexity of Bubble Sort:
- a) O(n)
- b) O(n log n)
- c) O(n²) ✅
- d) O(log n)

**Q85.** Which sorting algorithm has worst-case O(n log n)?
- a) Bubble Sort
- b) Quick Sort
- c) Merge Sort ✅
- d) Insertion Sort

**Q86.** Quick Sort's average time complexity:
- a) O(n²)
- b) O(n)
- c) O(n log n) ✅
- d) O(log n)

**Q87.** Quick Sort's worst-case time complexity:
- a) O(n log n)
- b) O(n²) ✅
- c) O(n)
- d) O(log n)

**Q88.** Which sorting algorithm is NOT comparison-based?
- a) Merge Sort
- b) Quick Sort
- c) Counting Sort ✅
- d) Heap Sort

**Q89.** Insertion Sort is most efficient for:
- a) Large datasets
- b) Nearly sorted arrays ✅
- c) Reverse sorted arrays
- d) Random data

**Q90.** Which sorting algorithm is stable?
- a) Quick Sort
- b) Heap Sort
- c) Merge Sort ✅
- d) Selection Sort

**Q91.** Selection Sort's time complexity (all cases):
- a) O(n)
- b) O(n log n)
- c) O(n²) ✅
- d) O(log n)

**Q92.** The space complexity of Merge Sort:
- a) O(1)
- b) O(n) ✅
- c) O(n²)
- d) O(log n)

**Q93.** Which sort uses divide and conquer?
- a) Bubble Sort
- b) Insertion Sort
- c) Selection Sort
- d) Merge Sort ✅

**Q94.** Heap Sort's time complexity:
- a) O(n)
- b) O(n log n) ✅
- c) O(n²)
- d) O(log n)

---

## 9. Hashing

**Q95.** The primary purpose of a hash function is:
- a) Sorting data
- b) Mapping keys to indices ✅
- c) Encrypting data
- d) Compressing data

**Q96.** When two keys map to the same index, it is called:
- a) Overflow
- b) Collision ✅
- c) Hashing
- d) Rehashing

**Q97.** Which is a collision resolution technique?
- a) Chaining ✅
- b) Sorting
- c) Binary search
- d) Recursion

**Q98.** Average-case time complexity of search in a hash table:
- a) O(n)
- b) O(log n)
- c) O(1) ✅
- d) O(n²)

**Q99.** In open addressing, if a collision occurs:
- a) A new table is created
- b) The next available slot is probed ✅
- c) The element is discarded
- d) The table is resorted

**Q100.** Load factor of a hash table is defined as:
- a) Number of keys × table size
- b) Number of keys / table size ✅
- c) Table size / number of keys
- d) Table size - number of keys

---

## 10. Coding / Implementation Problems

**Q101.** Implement a circular queue of size N with operations: Enqueue, Dequeue, and Display. The queue should reuse empty spaces created after deletions.

> **Hint**: Use `front` and `rear` pointers with modulo arithmetic: `rear = (rear + 1) % N`

**Q102.** Given the root of a binary tree, return its maximum depth.
```
Example 1: root = [3,9,20,null,null,15,7] → Output: 3
Example 2: root = [1,null,2] → Output: 2
```

**Q103.** Given the root of a binary tree, return the right side view (nodes visible from the right).
```
Example 1: root = [1,2,3,null,5,null,4] → Output: [1,3,4]
Example 2: root = [1,null,3] → Output: [1,3]
Example 3: root = [] → Output: []
```

**Q104.** Given the root of a binary tree, the value of a target node, and an integer k, return all nodes at distance k from the target.
```
Example: root = [3,5,1,6,2,0,8,null,null,7,4], target = 5, k = 2
Output: [7,4,1]
```

**Q105.** While constructing an MST, detect if adding an edge creates a cycle using Union-Find.
```
Input: edges = [(0,1), (1,2), (2,0)]
Output: Cycle Detected
```

**Q106.** Write a function to reverse a singly linked list.
```python
# Input: 1 -> 2 -> 3 -> 4 -> NULL
# Output: 4 -> 3 -> 2 -> 1 -> NULL
```

**Q107.** Implement a stack using two queues.

**Q108.** Write a program to evaluate a postfix expression.
```
Input: "2 3 + 4 *"
Output: 20
```

**Q109.** Implement BFS and DFS on a graph represented as an adjacency list.

**Q110.** Write a function to detect a cycle in a linked list using Floyd's algorithm.

**Q111.** Implement Merge Sort and analyze its time complexity.

**Q112.** Write Kruskal's algorithm to find MST of a weighted graph.

**Q113.** Implement a BST with insert, search, and delete operations.

**Q114.** Convert an infix expression to postfix using a stack.
```
Input: "A + B * C"
Output: "ABC*+"
```

**Q115.** Implement level-order (BFS) traversal of a binary tree.

---

## 11. Answer Key

| Q# | Ans | Q# | Ans | Q# | Ans | Q# | Ans |
|----|-----|----|-----|----|-----|----|-----|
| 1  | c   | 26 | c   | 51 | b   | 76 | b   |
| 2  | b   | 27 | b   | 52 | b   | 77 | b   |
| 3  | c   | 28 | c   | 53 | b   | 78 | c   |
| 4  | c   | 29 | b   | 54 | b   | 79 | d   |
| 5  | b   | 30 | c   | 55 | c   | 80 | a   |
| 6  | b   | 31 | b   | 56 | c   | 81 | b   |
| 7  | a   | 32 | b   | 57 | b   | 82 | d   |
| 8  | c   | 33 | a   | 58 | b   | 83 | b   |
| 9  | b   | 34 | b   | 59 | c   | 84 | c   |
| 10 | b   | 35 | b   | 60 | b   | 85 | c   |
| 11 | c   | 36 | c   | 61 | c   | 86 | c   |
| 12 | b   | 37 | c   | 62 | c   | 87 | b   |
| 13 | a   | 38 | b   | 63 | d   | 88 | c   |
| 14 | b   | 39 | b   | 64 | c   | 89 | b   |
| 15 | b   | 40 | b   | 65 | b   | 90 | c   |
| 16 | b   | 41 | b   | 66 | b   | 91 | c   |
| 17 | c   | 42 | c   | 67 | b   | 92 | b   |
| 18 | c   | 43 | b   | 68 | c   | 93 | d   |
| 19 | c   | 44 | c   | 69 | a   | 94 | b   |
| 20 | b   | 45 | b   | 70 | b   | 95 | b   |
| 21 | c   | 46 | a   | 71 | b   | 96 | b   |
| 22 | b   | 47 | b   | 72 | b   | 97 | a   |
| 23 | b   | 48 | b   | 73 | c   | 98 | c   |
| 24 | a   | 49 | a   | 74 | b   | 99 | b   |
| 25 | b   | 50 | c   | 75 | b   | 100| b   |

---

> **💡 Pro Tip**: Practice the coding questions (Q101–Q115) in Python. Focus on understanding the *why* behind each answer, not just memorizing.

> **📌 Compiled for IIT Patna — Data Structures & Algorithms, Semester 2**
