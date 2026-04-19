# Topic 10: Priority Queues

## 1. Overview

A **Priority Queue** is an abstract data type where each element has a **priority**, and elements are served in order of priority (not insertion order). The most common implementation is the **Heap**.

---

## 2. Key Concepts

### 2.1 Priority Queue ADT

| Operation | Description | Heap Time |
|-----------|-------------|-----------|
| `insert(k, v)` | Add element with priority `k` | $O(\log n)$ |
| `min()` / `max()` | Return element with highest priority | $O(1)$ |
| `remove_min()` / `remove_max()` | Remove and return highest priority element | $O(\log n)$ |
| `is_empty()` | Check if empty | $O(1)$ |
| `len()` | Return number of elements | $O(1)$ |

### 2.2 Heap

A **Heap** is a complete binary tree that satisfies the **heap property**:
- **Min-Heap:** Parent ≤ Children (root is minimum)
- **Max-Heap:** Parent ≥ Children (root is maximum)

```
Min-Heap:          Max-Heap:
      1                  9
     / \                / \
    3   5              7   6
   / \                / \
  7   9              3   1
```

### 2.3 Heap as an Array

A complete binary tree can be stored in an array:
- **Root** at index 0
- **Left child** of node at index $i$: $2i + 1$
- **Right child** of node at index $i$: $2i + 2$
- **Parent** of node at index $i$: $\lfloor (i - 1) / 2 \rfloor$

```
Array: [1, 3, 5, 7, 9]
Tree:       1
           / \
          3   5
         / \
        7   9
```

### 2.4 Heap Operations

**Insert (Bubble Up / Sift Up):**
1. Add element at the end (next available position).
2. Compare with parent; if smaller (min-heap), swap.
3. Repeat until heap property is restored.

**Remove Min (Bubble Down / Sift Down):**
1. Replace root with the last element.
2. Remove the last element.
3. Compare root with children; swap with smaller child.
4. Repeat until heap property is restored.

---

## 3. Code Examples

### 3.1 Min-Heap Implementation

```python
class MinHeap:
    def __init__(self):
        self._data = []

    def __len__(self):
        return len(self._data)

    def is_empty(self):
        return len(self._data) == 0

    def _parent(self, i):
        return (i - 1) // 2

    def _left(self, i):
        return 2 * i + 1

    def _right(self, i):
        return 2 * i + 2

    def _swap(self, i, j):
        self._data[i], self._data[j] = self._data[j], self._data[i]

    def _bubble_up(self, i):
        parent = self._parent(i)
        if i > 0 and self._data[i] < self._data[parent]:
            self._swap(i, parent)
            self._bubble_up(parent)

    def _bubble_down(self, i):
        n = len(self._data)
        smallest = i
        left = self._left(i)
        right = self._right(i)

        if left < n and self._data[left] < self._data[smallest]:
            smallest = left
        if right < n and self._data[right] < self._data[smallest]:
            smallest = right

        if smallest != i:
            self._swap(i, smallest)
            self._bubble_down(smallest)

    def insert(self, val):
        self._data.append(val)
        self._bubble_up(len(self._data) - 1)

    def get_min(self):
        if self.is_empty():
            raise IndexError("Heap is empty")
        return self._data[0]

    def remove_min(self):
        if self.is_empty():
            raise IndexError("Heap is empty")
        self._swap(0, len(self._data) - 1)
        val = self._data.pop()
        if not self.is_empty():
            self._bubble_down(0)
        return val

    def __str__(self):
        return str(self._data)


# Usage
heap = MinHeap()
for val in [5, 3, 8, 1, 9, 2]:
    heap.insert(val)
print(heap)            # [1, 3, 2, 5, 9, 8]
print(heap.get_min())  # 1
print(heap.remove_min())  # 1
print(heap)            # [2, 3, 8, 5, 9]
```

### 3.2 Using Python's heapq Module

```python
import heapq

# Min-heap operations
data = [5, 3, 8, 1, 9, 2]
heapq.heapify(data)       # Convert list to heap: O(n)
print(data)                # [1, 3, 2, 5, 9, 8]

heapq.heappush(data, 0)   # Insert: O(log n)
print(data)                # [0, 3, 1, 5, 9, 8, 2]

val = heapq.heappop(data)  # Remove min: O(log n)
print(val)                  # 0

# k smallest / k largest
nums = [5, 3, 8, 1, 9, 2, 7]
print(heapq.nsmallest(3, nums))  # [1, 2, 3]
print(heapq.nlargest(3, nums))   # [9, 8, 7]
```

### 3.3 Max-Heap Using heapq (Negate Values)

```python
import heapq

# Python's heapq is min-heap. For max-heap, negate values.
max_heap = []
for val in [5, 3, 8, 1, 9]:
    heapq.heappush(max_heap, -val)

print(-heapq.heappop(max_heap))  # 9 (maximum)
print(-heapq.heappop(max_heap))  # 8
```

### 3.4 Heap Sort

```python
import heapq

def heap_sort(arr):
    heapq.heapify(arr)
    return [heapq.heappop(arr) for _ in range(len(arr))]

data = [5, 3, 8, 1, 9, 2]
print(heap_sort(data))  # [1, 2, 3, 5, 8, 9]
# Time: O(n log n), Space: O(n) or O(1) in-place variant
```

### 3.5 Priority Queue with (priority, value) Tuples

```python
import heapq

pq = []
heapq.heappush(pq, (3, "Low priority task"))
heapq.heappush(pq, (1, "High priority task"))
heapq.heappush(pq, (2, "Medium priority task"))

while pq:
    priority, task = heapq.heappop(pq)
    print(f"Priority {priority}: {task}")
# Priority 1: High priority task
# Priority 2: Medium priority task
# Priority 3: Low priority task
```

### 3.6 Kth Largest Element

```python
import heapq

def kth_largest(nums, k):
    # Use a min-heap of size k
    heap = nums[:k]
    heapq.heapify(heap)
    for num in nums[k:]:
        if num > heap[0]:
            heapq.heapreplace(heap, num)
    return heap[0]

print(kth_largest([3, 2, 1, 5, 6, 4], 2))  # 5
```

---

## 4. Complexity Summary

| Operation | Min-Heap | Sorted Array | Unsorted Array |
|-----------|----------|-------------|---------------|
| Insert | $O(\log n)$ | $O(n)$ | $O(1)$ |
| Find min | $O(1)$ | $O(1)$ | $O(n)$ |
| Remove min | $O(\log n)$ | $O(1)$* | $O(n)$ |
| Build from $n$ elements | $O(n)$ | $O(n \log n)$ | $O(n)$ |

*$O(1)$ if removing from end of sorted array

---

## 5. MCQs (12 Questions)

**Q1.** A min-heap guarantees:
- A) Left child < Right child
- B) Parent ≤ Children
- C) Tree is sorted left to right
- D) All leaves are smaller than internal nodes

**Answer:** B) Parent ≤ Children

---

**Q2.** Time complexity of inserting into a heap:
- A) $O(1)$
- B) $O(\log n)$
- C) $O(n)$
- D) $O(n \log n)$

**Answer:** B) $O(\log n)$

---

**Q3.** In an array representation of a heap, the left child of node at index $i$ is at:
- A) $i + 1$
- B) $2i$
- C) $2i + 1$
- D) $2i + 2$

**Answer:** C) $2i + 1$ (0-indexed array)

---

**Q4.** Building a heap from an unsorted array takes:
- A) $O(n \log n)$
- B) $O(n^2)$
- C) $O(n)$
- D) $O(\log n)$

**Answer:** C) $O(n)$ — Bottom-up heap construction.

---

**Q5.** Heap sort time complexity:
- A) $O(n)$
- B) $O(n \log n)$
- C) $O(n^2)$
- D) $O(\log n)$

**Answer:** B) $O(n \log n)$

---

**Q6.** Python's `heapq` module implements:
- A) Max-heap
- B) Min-heap
- C) Both
- D) Neither

**Answer:** B) Min-heap

---

**Q7.** A heap is always a:
- A) Binary Search Tree
- B) Complete binary tree
- C) Full binary tree
- D) Balanced BST

**Answer:** B) Complete binary tree

---

**Q8.** To implement a max-heap using Python's `heapq`:
- A) Use `heapq.maxheapify()`
- B) Negate all values before inserting
- C) Reverse the list after heapify
- D) It's not possible

**Answer:** B) Negate all values before inserting

---

**Q9.** The root of a min-heap always contains:
- A) The median element
- B) The maximum element
- C) The minimum element
- D) A random element

**Answer:** C) The minimum element

---

**Q10.** `heapq.heapreplace(heap, val)` does:
- A) Replace all values with val
- B) Pop min and push val (more efficient than separate pop + push)
- C) Replace max with val
- D) Sort the heap

**Answer:** B) Pop min and push val (more efficient than separate pop + push)

---

**Q11.** Which is NOT an application of priority queues?
- A) Dijkstra's shortest path
- B) Huffman coding
- C) Linear search
- D) Task scheduling

**Answer:** C) Linear search

---

**Q12.** In a max-heap with elements [9, 7, 6, 3, 1], removing the max gives:
- A) [7, 6, 3, 1] as a valid heap
- B) [7, 3, 6, 1] as a valid heap
- C) [6, 7, 3, 1]
- D) [1, 3, 6, 7]

**Answer:** B) [7, 3, 6, 1] — After removing 9, place last element (1) at root and bubble down.
