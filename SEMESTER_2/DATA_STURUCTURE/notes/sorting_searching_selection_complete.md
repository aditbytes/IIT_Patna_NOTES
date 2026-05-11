# Sorting, Searching & Selection — Basic to Advanced

---

## Table of Contents

**SORTING**
1. [What is Sorting?](#1-what-is-sorting)
2. [Bubble Sort](#2-bubble-sort)
3. [Selection Sort](#3-selection-sort)
4. [Insertion Sort](#4-insertion-sort)
5. [Merge Sort](#5-merge-sort)
6. [Quick Sort](#6-quick-sort)
7. [Heap Sort](#7-heap-sort)
8. [Counting Sort](#8-counting-sort)
9. [Radix Sort](#9-radix-sort)
10. [Bucket Sort](#10-bucket-sort)
11. [Shell Sort](#11-shell-sort)
12. [Tim Sort (Python built-in)](#12-tim-sort-python-built-in)

**SEARCHING**

13. [Linear Search](#13-linear-search)
14. [Binary Search](#14-binary-search)
15. [Binary Search Variants](#15-binary-search-variants)
16. [Ternary Search](#16-ternary-search)
17. [Exponential Search](#17-exponential-search)
18. [Interpolation Search](#18-interpolation-search)

**SELECTION**

19. [Kth Smallest — Sorting](#19-kth-smallest--sorting)
20. [Kth Smallest — Min/Max Heap](#20-kth-smallest--minmax-heap)
21. [Quickselect Algorithm](#21-quickselect-algorithm)
22. [Median of Medians (BFPRT)](#22-median-of-medians-bfprt)

**REFERENCE**

23. [Full Complexity Summary](#23-full-complexity-summary)

---

# SORTING

---

## 1. What is Sorting?

**Sorting** arranges a sequence of elements in a defined order (usually ascending or descending).

### Key Properties

| Property | Meaning |
|----------|---------|
| **Stable** | Equal elements retain their original relative order |
| **In-place** | Uses $O(1)$ extra space (does not create a copy of the array) |
| **Comparison-based** | Elements are ordered using $<$, $>$, $=$ comparisons |
| **Non-comparison** | Exploits structure of the data (counting, radix, bucket) |

### Lower Bound

Any **comparison-based** sorting algorithm requires at least:

$$\Omega(n \log n) \text{ comparisons in the worst case}$$

This is proven via the decision-tree argument (the tree must have $n!$ leaves, so height $\geq \log_2(n!) = \Omega(n \log n)$).

**Non-comparison** sorts can beat this bound by exploiting the range or structure of data.

### Quick Reference Table

| Algorithm | Best | Average | Worst | Space | Stable | In-place |
|-----------|------|---------|-------|-------|--------|---------|
| Bubble Sort | $O(n)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | ✅ | ✅ |
| Selection Sort | $O(n^2)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | ❌ | ✅ |
| Insertion Sort | $O(n)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | ✅ | ✅ |
| Shell Sort | $O(n \log n)$ | $O(n^{1.3})$ | $O(n^2)$ | $O(1)$ | ❌ | ✅ |
| Merge Sort | $O(n \log n)$ | $O(n \log n)$ | $O(n \log n)$ | $O(n)$ | ✅ | ❌ |
| Quick Sort | $O(n \log n)$ | $O(n \log n)$ | $O(n^2)$ | $O(\log n)$ | ❌ | ✅ |
| Heap Sort | $O(n \log n)$ | $O(n \log n)$ | $O(n \log n)$ | $O(1)$ | ❌ | ✅ |
| Counting Sort | $O(n+k)$ | $O(n+k)$ | $O(n+k)$ | $O(k)$ | ✅ | ❌ |
| Radix Sort | $O(nk)$ | $O(nk)$ | $O(nk)$ | $O(n+k)$ | ✅ | ❌ |
| Bucket Sort | $O(n+k)$ | $O(n+k)$ | $O(n^2)$ | $O(n+k)$ | ✅ | ❌ |
| Tim Sort | $O(n)$ | $O(n \log n)$ | $O(n \log n)$ | $O(n)$ | ✅ | ❌ |

$n$ = input size, $k$ = range of values (Counting/Radix/Bucket)

---

## 2. Bubble Sort

**Idea:** Repeatedly compare adjacent elements and swap if out of order. After each pass, the largest unsorted element "bubbles" to its correct position.

```
Pass 1:  [64, 34, 25, 12, 22, 11, 90]
          64>34 → swap: [34, 64, 25, 12, 22, 11, 90]
          64>25 → swap: [34, 25, 64, 12, 22, 11, 90]
          ... → 90 reaches end
Pass 2:  64 reaches second-to-last position
...
```

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        swapped = False
        for j in range(0, n - i - 1):    # last i elements already sorted
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True
        if not swapped:
            break    # no swaps → already sorted → early exit O(n)
    return arr

print(bubble_sort([64, 34, 25, 12, 22, 11, 90]))
# [11, 12, 22, 25, 34, 64, 90]
```

| | Complexity |
|--|------------|
| Best (already sorted) | $O(n)$ |
| Average / Worst | $O(n^2)$ |
| Space | $O(1)$ |
| Stable | ✅ Yes |

**When to use:** Educational purposes; almost never in production.

---

## 3. Selection Sort

**Idea:** In each pass, find the minimum element in the unsorted portion and swap it to the front.

```
[64, 25, 12, 22, 11]
 min=11 → swap with 64 → [11, 25, 12, 22, 64]
 min=12 → swap with 25 → [11, 12, 25, 22, 64]
 min=22 → swap with 25 → [11, 12, 22, 25, 64]
 min=25 → already there → [11, 12, 22, 25, 64]
```

```python
def selection_sort(arr):
    n = len(arr)
    for i in range(n):
        min_idx = i
        for j in range(i + 1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
    return arr

print(selection_sort([64, 25, 12, 22, 11]))
# [11, 12, 22, 25, 64]
```

| | Complexity |
|--|------------|
| Best / Average / Worst | $O(n^2)$ |
| Space | $O(1)$ |
| Stable | ❌ No (swaps can disrupt order) |

**Advantage:** Makes at most $O(n)$ swaps — useful when writes are expensive.

---

## 4. Insertion Sort

**Idea:** Build the sorted array one element at a time by inserting each new element into its correct position among the already-sorted elements.

```
[12, 11, 13, 5, 6]
 i=1: key=11 → insert before 12  → [11, 12, 13, 5, 6]
 i=2: key=13 → stays             → [11, 12, 13, 5, 6]
 i=3: key=5  → shift 3 elements  → [5, 11, 12, 13, 6]
 i=4: key=6  → shift 3 elements  → [5, 6, 11, 12, 13]
```

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j   = i - 1
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key
    return arr

print(insertion_sort([12, 11, 13, 5, 6]))
# [5, 6, 11, 12, 13]
```

| | Complexity |
|--|------------|
| Best (nearly sorted) | $O(n)$ |
| Average / Worst | $O(n^2)$ |
| Space | $O(1)$ |
| Stable | ✅ Yes |

**When to use:** Small arrays ($n \leq 20$), nearly-sorted data, online sorting (elements arrive one by one). Used as the base case in Tim Sort and Intro Sort.

---

## 5. Merge Sort

**Idea (Divide & Conquer):**
1. **Divide** the array into two equal halves.
2. **Conquer** — recursively sort each half.
3. **Merge** the two sorted halves into one sorted array.

```
         [38, 27, 43, 3, 9, 82, 10]
              /                \
     [38, 27, 43, 3]      [9, 82, 10]
        /        \           /     \
   [38, 27]  [43, 3]    [9, 82]   [10]
    /    \    /    \    /    \
  [38] [27] [43] [3] [9] [82]
        ↑ merge ↑
   [27, 38]  [3, 43]  [9, 82] [10]
       ↑   merge  ↑      ↑ merge ↑
    [3, 27, 38, 43]    [9, 10, 82]
              ↑       merge      ↑
           [3, 9, 10, 27, 38, 43, 82]
```

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    mid   = len(arr) // 2
    left  = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return _merge(left, right)

def _merge(left, right):
    result = []
    i = j  = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:    # <= keeps it stable
            result.append(left[i]); i += 1
        else:
            result.append(right[j]); j += 1
    result.extend(left[i:])
    result.extend(right[j:])
    return result

print(merge_sort([38, 27, 43, 3, 9, 82, 10]))
# [3, 9, 10, 27, 38, 43, 82]
```

**In-place merge sort (bottom-up, iterative):**

```python
def merge_sort_iterative(arr):
    n    = len(arr)
    size = 1
    while size < n:
        for left in range(0, n, 2 * size):
            mid   = min(left + size,     n)
            right = min(left + 2 * size, n)
            arr[left:right] = _merge(arr[left:mid], arr[mid:right])
        size *= 2
    return arr
```

| | Complexity |
|--|------------|
| Best / Average / Worst | $O(n \log n)$ |
| Space | $O(n)$ |
| Stable | ✅ Yes |

**When to use:** When stability is required; when sorting linked lists (no random access needed); external sorting (sorting data larger than RAM).

---

## 6. Quick Sort

**Idea (Divide & Conquer):**
1. Pick a **pivot** element.
2. **Partition** — put elements smaller than pivot to the left, larger to the right.
3. **Recursively** sort the left and right partitions.

```
arr = [3, 6, 8, 10, 1, 2, 1],  pivot = 3
left  = [1, 2, 1]   (< 3)
middle= [3]          (== 3)
right = [6, 8, 10]  (> 3)
Result: sort(left) + middle + sort(right) = [1, 1, 2, 3, 6, 8, 10]
```

### 6.1 Simple (Functional)

```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    pivot  = arr[len(arr) // 2]
    left   = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right  = [x for x in arr if x > pivot]
    return quick_sort(left) + middle + quick_sort(right)

print(quick_sort([3, 6, 8, 10, 1, 2, 1]))
# [1, 1, 2, 3, 6, 8, 10]
```

### 6.2 In-place (Lomuto Partition)

```python
def quick_sort_inplace(arr, low=0, high=None):
    if high is None: high = len(arr) - 1
    if low < high:
        pi = _partition_lomuto(arr, low, high)
        quick_sort_inplace(arr, low,    pi - 1)
        quick_sort_inplace(arr, pi + 1, high)

def _partition_lomuto(arr, low, high):
    pivot = arr[high]    # last element as pivot
    i     = low - 1
    for j in range(low, high):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1

arr = [10, 7, 8, 9, 1, 5]
quick_sort_inplace(arr)
print(arr)  # [1, 5, 7, 8, 9, 10]
```

### 6.3 Hoare Partition (faster in practice)

```python
def _partition_hoare(arr, low, high):
    pivot = arr[(low + high) // 2]
    i     = low  - 1
    j     = high + 1
    while True:
        i += 1
        while arr[i] < pivot: i += 1
        j -= 1
        while arr[j] > pivot: j -= 1
        if i >= j: return j
        arr[i], arr[j] = arr[j], arr[i]
```

### 6.4 Randomised Quick Sort (avoids worst case)

```python
import random

def quick_sort_random(arr, low=0, high=None):
    if high is None: high = len(arr) - 1
    if low < high:
        # Randomise pivot
        rand_idx = random.randint(low, high)
        arr[rand_idx], arr[high] = arr[high], arr[rand_idx]
        pi = _partition_lomuto(arr, low, high)
        quick_sort_random(arr, low,    pi - 1)
        quick_sort_random(arr, pi + 1, high)
```

### 6.5 Three-way Quick Sort (handles duplicates efficiently)

```python
def quick_sort_3way(arr, low=0, high=None):
    if high is None: high = len(arr) - 1
    if low >= high: return
    pivot = arr[low]
    lt, gt, i = low, high, low
    while i <= gt:
        if arr[i] < pivot:
            arr[lt], arr[i] = arr[i], arr[lt]; lt += 1; i += 1
        elif arr[i] > pivot:
            arr[gt], arr[i] = arr[i], arr[gt]; gt -= 1
        else:
            i += 1
    quick_sort_3way(arr, low, lt - 1)
    quick_sort_3way(arr, gt + 1, high)
```

| | Complexity |
|--|------------|
| Best / Average | $O(n \log n)$ |
| Worst (sorted array + bad pivot) | $O(n^2)$ |
| Space (recursion stack) | $O(\log n)$ avg, $O(n)$ worst |
| Stable | ❌ No |

**When to use:** General-purpose sorting; fastest in practice for average cases. Use randomised or 3-way variant for robustness.

---

## 7. Heap Sort

**Idea:**
1. Build a **max-heap** from the array — $O(n)$.
2. Repeatedly extract the maximum (swap root with last), reduce heap size, and sift down — $O(n \log n)$.

```
arr = [4, 10, 3, 5, 1]
Build max-heap: [10, 5, 3, 4, 1]
Step 1: swap 10↔1 → [1, 5, 3, 4, | 10], heapify → [5, 4, 3, 1]
Step 2: swap 5↔1  → [1, 4, 3, | 5, 10], heapify → [4, 1, 3]
...
Result: [1, 3, 4, 5, 10]
```

```python
def heap_sort(arr):
    n = len(arr)

    def sift_down(arr, n, i):
        largest = i
        l, r    = 2*i + 1, 2*i + 2
        if l < n and arr[l] > arr[largest]: largest = l
        if r < n and arr[r] > arr[largest]: largest = r
        if largest != i:
            arr[i], arr[largest] = arr[largest], arr[i]
            sift_down(arr, n, largest)

    # Build max-heap (start from last non-leaf)
    for i in range(n // 2 - 1, -1, -1):
        sift_down(arr, n, i)

    # Extract elements one by one
    for i in range(n - 1, 0, -1):
        arr[0], arr[i] = arr[i], arr[0]    # move current max to end
        sift_down(arr, i, 0)

    return arr

print(heap_sort([4, 10, 3, 5, 1]))
# [1, 3, 4, 5, 10]
```

| | Complexity |
|--|------------|
| Best / Average / Worst | $O(n \log n)$ |
| Space | $O(1)$ |
| Stable | ❌ No |

**When to use:** When guaranteed $O(n \log n)$ and $O(1)$ space is required (e.g., embedded systems).

---

## 8. Counting Sort

**Idea:** Count the frequency of each value, then reconstruct the sorted array. Works only for **non-negative integers** within a known range $[0, k]$.

```
arr = [4, 2, 2, 8, 3, 3, 1]
count[1]=1, count[2]=2, count[3]=2, count[4]=1, count[8]=1
Output: [1, 2, 2, 3, 3, 4, 8]
```

```python
def counting_sort(arr):
    if not arr: return arr
    k      = max(arr)
    count  = [0] * (k + 1)
    for x in arr:
        count[x] += 1
    result = []
    for val, freq in enumerate(count):
        result.extend([val] * freq)
    return result

# Stable version (preserves original objects with same key)
def counting_sort_stable(arr, key=lambda x: x):
    if not arr: return arr
    k      = max(key(x) for x in arr)
    count  = [0] * (k + 1)
    for x in arr:
        count[key(x)] += 1
    # Prefix sum (cumulative count)
    for i in range(1, k + 1):
        count[i] += count[i - 1]
    output = [None] * len(arr)
    for x in reversed(arr):              # reversed → stable
        idx         = count[key(x)] - 1
        output[idx] = x
        count[key(x)] -= 1
    return output

print(counting_sort([4, 2, 2, 8, 3, 3, 1]))
# [1, 2, 2, 3, 3, 4, 8]
```

| | Complexity |
|--|------------|
| Best / Average / Worst | $O(n + k)$ |
| Space | $O(k)$ |
| Stable | ✅ Yes |

**When to use:** Integer data with small range $k \approx O(n)$.  
**Limitation:** Impractical when $k \gg n$ (e.g., sorting 10 values in range [0, 10^9]).

---

## 9. Radix Sort

**Idea:** Sort numbers digit by digit from **least significant** to **most significant** (LSD), using a stable sort (counting sort) at each digit.

```
arr = [170, 45, 75, 90, 802, 24, 2, 66]

Sort by units digit:  [170, 90, 802, 2, 24, 45, 75, 66]
Sort by tens digit:   [802, 2, 24, 45, 66, 170, 75, 90]
Sort by hundreds:     [2, 24, 45, 66, 75, 90, 170, 802]
```

```python
def radix_sort(arr):
    if not arr: return arr
    max_val = max(arr)
    exp     = 1
    while max_val // exp > 0:
        arr = _counting_sort_by_digit(arr, exp)
        exp *= 10
    return arr

def _counting_sort_by_digit(arr, exp):
    n      = len(arr)
    output = [0] * n
    count  = [0] * 10

    for x in arr:
        digit = (x // exp) % 10
        count[digit] += 1

    for i in range(1, 10):
        count[i] += count[i - 1]

    for x in reversed(arr):              # reversed → stable
        digit       = (x // exp) % 10
        idx         = count[digit] - 1
        output[idx] = x
        count[digit] -= 1

    return output

print(radix_sort([170, 45, 75, 90, 802, 24, 2, 66]))
# [2, 24, 45, 66, 75, 90, 170, 802]
```

**Radix sort for strings (lexicographic):**

```python
def radix_sort_strings(words):
    """Assumes all strings have the same length."""
    if not words: return words
    length = len(words[0])
    for pos in range(length - 1, -1, -1):   # rightmost char first
        words = sorted(words, key=lambda w: w[pos])   # stable sort
    return words
```

| | Complexity |
|--|------------|
| Best / Average / Worst | $O(nk)$ where $k$ = number of digits |
| Space | $O(n + b)$ ($b$ = base, usually 10) |
| Stable | ✅ Yes |

**When to use:** Sorting integers or fixed-length strings. Outperforms comparison sorts when $k = O(\log n)$.

---

## 10. Bucket Sort

**Idea:** Distribute elements into a number of **buckets**, sort each bucket (using insertion sort), then concatenate.  
Works best when elements are **uniformly distributed** in a known range.

```
arr = [0.78, 0.17, 0.39, 0.26, 0.72, 0.94, 0.21, 0.12]
n = 8 buckets, range [0, 1)

Bucket 0: [0.17, 0.12]
Bucket 2: [0.26, 0.21]
Bucket 3: [0.39]
Bucket 7: [0.78, 0.72]
Bucket 9: [0.94]
→ Sort each bucket → Concatenate
→ [0.12, 0.17, 0.21, 0.26, 0.39, 0.72, 0.78, 0.94]
```

```python
def bucket_sort(arr, num_buckets=None):
    if not arr: return arr
    n          = len(arr)
    min_val    = min(arr)
    max_val    = max(arr)
    if min_val == max_val:
        return arr
    if num_buckets is None:
        num_buckets = n

    bucket_range = (max_val - min_val) / num_buckets
    buckets      = [[] for _ in range(num_buckets)]

    for x in arr:
        idx = int((x - min_val) / bucket_range)
        if idx == num_buckets: idx -= 1   # handle max_val edge case
        buckets[idx].append(x)

    result = []
    for bucket in buckets:
        bucket.sort()           # insertion sort for small buckets
        result.extend(bucket)
    return result

print(bucket_sort([0.78, 0.17, 0.39, 0.26, 0.72, 0.94, 0.21, 0.12]))
# [0.12, 0.17, 0.21, 0.26, 0.39, 0.72, 0.78, 0.94]
```

| | Complexity |
|--|------------|
| Best / Average (uniform) | $O(n + k)$ |
| Worst (all in one bucket) | $O(n^2)$ |
| Space | $O(n + k)$ |
| Stable | ✅ (if per-bucket sort is stable) |

---

## 11. Shell Sort

**Idea:** Generalisation of insertion sort — sort elements at a **gap** $g$ apart; reduce gap until $g = 1$ (plain insertion sort). Far-apart elements are moved closer to their final positions early on.

```python
def shell_sort(arr):
    n   = len(arr)
    gap = n // 2

    while gap > 0:
        for i in range(gap, n):
            temp = arr[i]
            j    = i
            while j >= gap and arr[j - gap] > temp:
                arr[j] = arr[j - gap]
                j      -= gap
            arr[j] = temp
        gap //= 2
    return arr

print(shell_sort([12, 34, 54, 2, 3]))
# [2, 3, 12, 34, 54]
```

| | Complexity |
|--|------------|
| Best | $O(n \log n)$ |
| Average (Knuth gap) | $O(n^{1.5})$ |
| Worst | $O(n^2)$ |
| Space | $O(1)$ |
| Stable | ❌ No |

---

## 12. Tim Sort (Python built-in)

**Tim Sort** is a hybrid of **merge sort** and **insertion sort** — used by Python's `list.sort()` and `sorted()`.

- Splits the array into **runs** (naturally sorted subsequences, min length 32–64).
- Each run is sorted with **insertion sort**.
- Runs are merged using an optimised merge.

**Time:** $O(n \log n)$ worst, $O(n)$ best (already sorted)  
**Space:** $O(n)$ | **Stable:** ✅ Yes

```python
arr = [5, 2, 8, 1, 9, 3]

# In-place sort (modifies arr)
arr.sort()
print(arr)   # [1, 2, 3, 5, 8, 9]

# Returns new sorted list
result = sorted([5, 2, 8, 1, 9, 3])

# Custom key
students = [("Alice", 85), ("Bob", 92), ("Charlie", 78)]
students.sort(key=lambda s: s[1])               # sort by score
students.sort(key=lambda s: s[1], reverse=True) # descending

# Multi-key sort
data = [(2, 'b'), (1, 'a'), (2, 'a'), (1, 'b')]
data.sort(key=lambda x: (x[0], x[1]))
print(data)  # [(1, 'a'), (1, 'b'), (2, 'a'), (2, 'b')]

# Sort with cmp_to_key (custom comparator)
from functools import cmp_to_key
def compare(a, b):
    return a - b   # ascending
arr.sort(key=cmp_to_key(compare))
```

---

# SEARCHING

---

## 13. Linear Search

**Idea:** Examine every element one by one until the target is found.

```python
def linear_search(arr, target):
    for i, x in enumerate(arr):
        if x == target:
            return i
    return -1

print(linear_search([5, 3, 8, 1, 9], 8))  # 2
print(linear_search([5, 3, 8, 1, 9], 7))  # -1
```

| | Complexity |
|--|------------|
| Best (target at start) | $O(1)$ |
| Average / Worst | $O(n)$ |
| Space | $O(1)$ |
| Requires sorted? | ❌ No |

**When to use:** Unsorted data; small arrays; or when sorted order is not guaranteed.

---

## 14. Binary Search

**Idea:** On a **sorted** array, compare the target with the middle element:
- If equal → found.
- If target < mid → search the left half.
- If target > mid → search the right half.

Each step **halves** the search space → $O(\log n)$.

```
arr = [1, 3, 5, 7, 9, 11, 13, 15], target = 7

lo=0, hi=7, mid=3 → arr[3]=7 ✅ → found at index 3
```

### 14.1 Iterative (Preferred)

```python
def binary_search(arr, target):
    lo, hi = 0, len(arr) - 1
    while lo <= hi:
        mid = lo + (hi - lo) // 2    # avoids overflow
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return -1

arr = [1, 3, 5, 7, 9, 11, 13, 15]
print(binary_search(arr, 7))   # 3
print(binary_search(arr, 6))   # -1
```

### 14.2 Recursive

```python
def binary_search_rec(arr, target, lo=0, hi=None):
    if hi is None: hi = len(arr) - 1
    if lo > hi: return -1
    mid = lo + (hi - lo) // 2
    if arr[mid] == target:   return mid
    elif arr[mid] < target:  return binary_search_rec(arr, target, mid + 1, hi)
    else:                    return binary_search_rec(arr, target, lo, mid - 1)
```

### 14.3 Using Python `bisect`

```python
import bisect

arr = [1, 3, 5, 7, 9, 11]
bisect.insort(arr, 6)                   # insert 6 keeping arr sorted
print(arr)                               # [1, 3, 5, 6, 7, 9, 11]

idx = bisect.bisect_left(arr, 7)         # index of first element >= 7
print(idx)                               # 4

idx = bisect.bisect_right(arr, 7)        # index of first element > 7
print(idx)                               # 5
```

| | Complexity |
|--|------------|
| Best (target = mid) | $O(1)$ |
| Average / Worst | $O(\log n)$ |
| Space (iterative) | $O(1)$ |
| Requires sorted? | ✅ Yes |

---

## 15. Binary Search Variants

### 15.1 First Occurrence (Lower Bound)

```python
def first_occurrence(arr, target):
    lo, hi, result = 0, len(arr) - 1, -1
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if arr[mid] == target:
            result = mid
            hi     = mid - 1    # keep searching left
        elif arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return result

arr = [1, 2, 2, 2, 3, 4]
print(first_occurrence(arr, 2))  # 1
```

### 15.2 Last Occurrence (Upper Bound)

```python
def last_occurrence(arr, target):
    lo, hi, result = 0, len(arr) - 1, -1
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if arr[mid] == target:
            result = mid
            lo     = mid + 1    # keep searching right
        elif arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return result

print(last_occurrence(arr, 2))   # 3
```

### 15.3 Count Occurrences

```python
def count_occurrences(arr, target):
    first = first_occurrence(arr, target)
    if first == -1: return 0
    return last_occurrence(arr, target) - first + 1

print(count_occurrences(arr, 2))  # 3
```

### 15.4 Search in Rotated Sorted Array

```python
def search_rotated(arr, target):
    lo, hi = 0, len(arr) - 1
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if arr[mid] == target: return mid
        # Left half is sorted
        if arr[lo] <= arr[mid]:
            if arr[lo] <= target < arr[mid]:
                hi = mid - 1
            else:
                lo = mid + 1
        # Right half is sorted
        else:
            if arr[mid] < target <= arr[hi]:
                lo = mid + 1
            else:
                hi = mid - 1
    return -1

print(search_rotated([4, 5, 6, 7, 0, 1, 2], 0))  # 4
```

### 15.5 Find Peak Element

```python
def find_peak(arr):
    lo, hi = 0, len(arr) - 1
    while lo < hi:
        mid = lo + (hi - lo) // 2
        if arr[mid] > arr[mid + 1]:
            hi = mid       # peak is in left half (including mid)
        else:
            lo = mid + 1   # peak is in right half
    return lo

print(find_peak([1, 2, 3, 1]))  # 2 (index of peak element 3)
```

### 15.6 Search in 2D Sorted Matrix

```python
def search_matrix(matrix, target):
    """Matrix where each row and column is sorted."""
    if not matrix: return False
    rows, cols = len(matrix), len(matrix[0])
    r, c       = 0, cols - 1          # start at top-right
    while r < rows and c >= 0:
        if matrix[r][c] == target:     return True
        elif matrix[r][c] > target:    c -= 1
        else:                          r += 1
    return False

matrix = [[1,4,7],[2,5,8],[3,6,9]]
print(search_matrix(matrix, 5))   # True
print(search_matrix(matrix, 10))  # False
```

### 15.7 Binary Search on Answer (Parametric Search)

Used when you need to find the **minimum/maximum value** satisfying some condition, not a direct element.

**Example — Minimum capacity to ship packages in D days:**

```python
def ship_within_days(weights, days):
    def can_ship(capacity):
        ships, load = 1, 0
        for w in weights:
            if load + w > capacity:
                ships += 1; load = 0
            load += w
        return ships <= days

    lo = max(weights)        # minimum possible capacity
    hi = sum(weights)        # maximum possible capacity
    while lo < hi:
        mid = lo + (hi - lo) // 2
        if can_ship(mid):
            hi = mid         # try smaller
        else:
            lo = mid + 1     # need more capacity
    return lo

print(ship_within_days([1,2,3,4,5,6,7,8,9,10], 5))  # 15
```

---

## 16. Ternary Search

**Idea:** Divide the range into **three parts** to find the maximum/minimum of a **unimodal** function (one that increases then decreases or vice versa).

```python
def ternary_search(arr, lo, hi, target):
    """Binary search variant — works on sorted arr."""
    if lo > hi: return -1
    m1 = lo + (hi - lo) // 3
    m2 = hi - (hi - lo) // 3
    if arr[m1] == target: return m1
    if arr[m2] == target: return m2
    if target < arr[m1]:  return ternary_search(arr, lo,    m1 - 1, target)
    if target > arr[m2]:  return ternary_search(arr, m2 + 1, hi,    target)
    return ternary_search(arr, m1 + 1, m2 - 1, target)

# Find max of unimodal function (continuous)
def ternary_search_max(f, lo, hi, eps=1e-9):
    while hi - lo > eps:
        m1 = lo + (hi - lo) / 3
        m2 = hi - (hi - lo) / 3
        if f(m1) < f(m2): lo = m1
        else:             hi = m2
    return (lo + hi) / 2
```

**Time:** $O(\log_{3/2} n)$ ≈ $O(\log n)$ | Slower than binary search by a constant factor.

---

## 17. Exponential Search

**Idea:** For **unbounded** or very large arrays — double the search range until the element could be in the range, then binary search within it.

```python
def exponential_search(arr, target):
    if arr[0] == target: return 0
    n   = len(arr)
    i   = 1
    while i < n and arr[i] <= target:
        i *= 2
    # Binary search in arr[i//2 .. min(i, n-1)]
    return binary_search_range(arr, target, i // 2, min(i, n - 1))

def binary_search_range(arr, target, lo, hi):
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if arr[mid] == target:   return mid
        elif arr[mid] < target:  lo = mid + 1
        else:                    hi = mid - 1
    return -1

arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
print(exponential_search(arr, 7))  # 6
```

**Time:** $O(\log n)$ | **When to use:** Array size is unknown or infinite (streaming data).

---

## 18. Interpolation Search

**Idea:** Like binary search, but estimates the position using a **linear interpolation** formula — faster than binary search when elements are **uniformly distributed**.

$$\text{pos} = lo + \left\lfloor \frac{(target - arr[lo]) \times (hi - lo)}{arr[hi] - arr[lo]} \right\rfloor$$

```python
def interpolation_search(arr, target):
    lo, hi = 0, len(arr) - 1
    while lo <= hi and arr[lo] <= target <= arr[hi]:
        if arr[lo] == arr[hi]:
            return lo if arr[lo] == target else -1
        pos = lo + (target - arr[lo]) * (hi - lo) // (arr[hi] - arr[lo])
        if arr[pos] == target:   return pos
        elif arr[pos] < target:  lo = pos + 1
        else:                    hi = pos - 1
    return -1

arr = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
print(interpolation_search(arr, 70))  # 6
```

| | Complexity |
|--|------------|
| Average (uniform distribution) | $O(\log \log n)$ |
| Worst | $O(n)$ |
| Space | $O(1)$ |

---

# SELECTION

---

## 19. Kth Smallest — Sorting

The simplest approach: sort the array and return element at index $k - 1$.

```python
def kth_smallest_sort(arr, k):
    return sorted(arr)[k - 1]

print(kth_smallest_sort([3, 2, 1, 5, 6, 4], 2))  # 2
```

**Time:** $O(n \log n)$ | Fine when you need multiple queries.

---

## 20. Kth Smallest — Min/Max Heap

### Using Min-Heap (pop k times)

```python
import heapq

def kth_smallest_heap(arr, k):
    heap = arr[:]
    heapq.heapify(heap)           # O(n)
    for _ in range(k - 1):
        heapq.heappop(heap)       # O(log n) each
    return heapq.heappop(heap)

print(kth_smallest_heap([3, 2, 1, 5, 6, 4], 2))  # 2
```

**Time:** $O(n + k \log n)$ — good when $k \ll n$.

### Using Max-Heap of size k

```python
def kth_smallest_maxheap(arr, k):
    """Maintain a max-heap of size k. Root = kth smallest."""
    heap = []
    for x in arr:
        heapq.heappush(heap, -x)
        if len(heap) > k:
            heapq.heappop(heap)
    return -heap[0]

print(kth_smallest_maxheap([3, 2, 1, 5, 6, 4], 2))  # 2
```

**Time:** $O(n \log k)$ | **Space:** $O(k)$ — best when $k \ll n$ (e.g., streaming data).

### Kth Largest

```python
def kth_largest(arr, k):
    return kth_smallest_heap(arr, len(arr) - k + 1)
# Or using min-heap of size k:
def kth_largest_v2(arr, k):
    return heapq.nlargest(k, arr)[-1]
```

---

## 21. Quickselect Algorithm

**Idea:** Like Quick Sort, but only recurse into the partition that contains the $k$-th element.

Average-case $O(n)$ — much faster than sorting.

### 21.1 Simple (Functional)

```python
import random

def quickselect(arr, k):
    """Returns the kth smallest element (1-indexed)."""
    if len(arr) == 1:
        return arr[0]
    pivot  = random.choice(arr)
    left   = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right  = [x for x in arr if x > pivot]

    if k <= len(left):
        return quickselect(left, k)
    elif k <= len(left) + len(middle):
        return pivot
    else:
        return quickselect(right, k - len(left) - len(middle))

arr = [3, 2, 1, 5, 6, 4]
print(quickselect(arr, 2))  # 2  (2nd smallest)
print(quickselect(arr, 5))  # 5  (5th smallest)
```

### 21.2 In-place Quickselect

```python
def quickselect_inplace(arr, k, lo=0, hi=None):
    """Returns (k-1)th index element — 0-indexed k."""
    if hi is None: hi = len(arr) - 1
    if lo == hi: return arr[lo]

    pivot_idx = _partition_lomuto(arr, lo, hi)

    if pivot_idx == k:
        return arr[pivot_idx]
    elif k < pivot_idx:
        return quickselect_inplace(arr, k, lo, pivot_idx - 1)
    else:
        return quickselect_inplace(arr, k, pivot_idx + 1, hi)

def _partition_lomuto(arr, lo, hi):
    pivot = arr[hi]
    i     = lo - 1
    for j in range(lo, hi):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i+1], arr[hi] = arr[hi], arr[i+1]
    return i + 1
```

| | Complexity |
|--|------------|
| Average | $O(n)$ |
| Worst (bad pivot every time) | $O(n^2)$ |
| Space | $O(1)$ in-place |

---

## 22. Median of Medians (BFPRT)

Guarantees **worst-case $O(n)$** for selection by choosing a good pivot deterministically.

**Steps:**
1. Divide array into groups of 5.
2. Find the median of each group (by sorting each group of 5 — $O(1)$ each).
3. Recursively find the median of those medians → use as pivot.
4. Partition around pivot; recurse into the correct partition.

```python
def median_of_medians(arr, k):
    """Returns kth smallest element (0-indexed k), worst-case O(n)."""
    if len(arr) <= 5:
        return sorted(arr)[k]

    # Step 1 & 2: medians of groups of 5
    chunks  = [arr[i:i+5] for i in range(0, len(arr), 5)]
    medians = [sorted(c)[len(c) // 2] for c in chunks]

    # Step 3: median of medians
    pivot   = median_of_medians(medians, len(medians) // 2)

    # Step 4: partition
    low    = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    high   = [x for x in arr if x > pivot]

    if k < len(low):
        return median_of_medians(low, k)
    elif k < len(low) + len(middle):
        return pivot
    else:
        return median_of_medians(high, k - len(low) - len(middle))

arr = [3, 2, 1, 5, 6, 4, 8, 7, 9]
print(median_of_medians(arr, 4))  # 5 (5th smallest, 0-indexed: 4th)
```

| | Complexity |
|--|------------|
| Worst | $O(n)$ |
| Space | $O(n)$ |

**In practice:** Quickselect with random pivot is faster on average. Median of Medians is used when worst-case guarantee is required.

---

## 23. Full Complexity Summary

### Sorting

| Algorithm | Best | Average | Worst | Space | Stable | Notes |
|-----------|------|---------|-------|-------|--------|-------|
| Bubble Sort | $O(n)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | ✅ | Early exit optimisation |
| Selection Sort | $O(n^2)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | ❌ | Min swaps |
| Insertion Sort | $O(n)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | ✅ | Best for small/nearly-sorted |
| Shell Sort | $O(n \log n)$ | $O(n^{1.3})$ | $O(n^2)$ | $O(1)$ | ❌ | Gap-based insertion |
| Merge Sort | $O(n \log n)$ | $O(n \log n)$ | $O(n \log n)$ | $O(n)$ | ✅ | Best stable $O(n \log n)$ |
| Quick Sort | $O(n \log n)$ | $O(n \log n)$ | $O(n^2)$ | $O(\log n)$ | ❌ | Fastest in practice |
| Heap Sort | $O(n \log n)$ | $O(n \log n)$ | $O(n \log n)$ | $O(1)$ | ❌ | Guaranteed + in-place |
| Counting Sort | $O(n+k)$ | $O(n+k)$ | $O(n+k)$ | $O(k)$ | ✅ | Integer data, small $k$ |
| Radix Sort | $O(nk)$ | $O(nk)$ | $O(nk)$ | $O(n+b)$ | ✅ | Fixed-width integers/strings |
| Bucket Sort | $O(n+k)$ | $O(n+k)$ | $O(n^2)$ | $O(n+k)$ | ✅ | Uniform float data |
| Tim Sort | $O(n)$ | $O(n \log n)$ | $O(n \log n)$ | $O(n)$ | ✅ | Python default |

### Searching

| Algorithm | Best | Average | Worst | Space | Requires Sorted |
|-----------|------|---------|-------|-------|-----------------|
| Linear Search | $O(1)$ | $O(n)$ | $O(n)$ | $O(1)$ | ❌ |
| Binary Search | $O(1)$ | $O(\log n)$ | $O(\log n)$ | $O(1)$ | ✅ |
| Ternary Search | $O(1)$ | $O(\log n)$ | $O(\log n)$ | $O(1)$ | ✅ (unimodal) |
| Exponential Search | $O(1)$ | $O(\log n)$ | $O(\log n)$ | $O(1)$ | ✅ |
| Interpolation Search | $O(1)$ | $O(\log \log n)$ | $O(n)$ | $O(1)$ | ✅ (uniform) |

### Selection

| Algorithm | Average | Worst | Space | Notes |
|-----------|---------|-------|-------|-------|
| Sort + Index | $O(n \log n)$ | $O(n \log n)$ | $O(n)$ | Simplest |
| Min-Heap pop k | $O(n + k \log n)$ | $O(n + k \log n)$ | $O(n)$ | |
| Max-Heap size k | $O(n \log k)$ | $O(n \log k)$ | $O(k)$ | Streaming-friendly |
| Quickselect | $O(n)$ | $O(n^2)$ | $O(1)$ | Fastest avg |
| Median of Medians | $O(n)$ | $O(n)$ | $O(n)$ | Guaranteed $O(n)$ |

### When to Choose What

| Scenario | Recommended |
|----------|-------------|
| General-purpose, small $n$ | Insertion Sort |
| General-purpose, large $n$ | Quick Sort (randomised) |
| Need stable sort | Merge Sort / Tim Sort |
| Integer data, small range | Counting Sort |
| Fixed-width integers, large $n$ | Radix Sort |
| Float data, uniform distribution | Bucket Sort |
| Guaranteed $O(n \log n)$, $O(1)$ space | Heap Sort |
| Sorted array, find element | Binary Search |
| Find kth element, streaming | Max-Heap of size k |
| Find kth element, in-memory | Quickselect |
| Find kth, worst-case guarantee | Median of Medians |

---

*End of Sorting, Searching & Selection notes*
