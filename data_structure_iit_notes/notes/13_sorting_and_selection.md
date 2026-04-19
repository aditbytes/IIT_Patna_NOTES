# Topic 13: Sorting and Selection

## 1. Overview

**Sorting** arranges elements in a specific order (ascending/descending). **Selection** finds the $k$-th smallest/largest element. Sorting is one of the most fundamental operations in computer science.

---

## 2. Key Concepts

### 2.1 Sorting Algorithm Comparison

| Algorithm | Best | Average | Worst | Space | Stable? |
|-----------|------|---------|-------|-------|---------|
| **Bubble Sort** | $O(n)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | Yes |
| **Selection Sort** | $O(n^2)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | No |
| **Insertion Sort** | $O(n)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | Yes |
| **Merge Sort** | $O(n \log n)$ | $O(n \log n)$ | $O(n \log n)$ | $O(n)$ | Yes |
| **Quick Sort** | $O(n \log n)$ | $O(n \log n)$ | $O(n^2)$ | $O(\log n)$ | No |
| **Heap Sort** | $O(n \log n)$ | $O(n \log n)$ | $O(n \log n)$ | $O(1)$ | No |
| **Counting Sort** | $O(n + k)$ | $O(n + k)$ | $O(n + k)$ | $O(k)$ | Yes |
| **Radix Sort** | $O(nk)$ | $O(nk)$ | $O(nk)$ | $O(n + k)$ | Yes |

- **Stable** = equal elements maintain their relative order
- $n$ = number of elements, $k$ = range of values

### 2.2 Lower Bound for Comparison-Based Sorting

Any comparison-based sorting algorithm requires at least $\Omega(n \log n)$ comparisons in the worst case.

---

## 3. Code Examples

### 3.1 Bubble Sort

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        swapped = False
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True
        if not swapped:
            break  # Already sorted
    return arr

print(bubble_sort([64, 34, 25, 12, 22, 11, 90]))
# [11, 12, 22, 25, 34, 64, 90]
```

### 3.2 Selection Sort

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

### 3.3 Insertion Sort

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key
    return arr

print(insertion_sort([12, 11, 13, 5, 6]))
# [5, 6, 11, 12, 13]
```

### 3.4 Merge Sort

*(From IIT Patna Lecture)*

**Divide and Conquer:**
1. **Divide** the array into two halves.
2. **Conquer** by recursively sorting each half.
3. **Merge** the two sorted halves.

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr

    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])

    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0

    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1

    result.extend(left[i:])
    result.extend(right[j:])
    return result

print(merge_sort([38, 27, 43, 3, 9, 82, 10]))
# [3, 9, 10, 27, 38, 43, 82]
```

**Time:** $O(n \log n)$ always | **Space:** $O(n)$

### 3.5 Quick Sort

```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr

    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]

    return quick_sort(left) + middle + quick_sort(right)

print(quick_sort([3, 6, 8, 10, 1, 2, 1]))
# [1, 1, 2, 3, 6, 8, 10]
```

**In-place Quick Sort:**
```python
def quick_sort_inplace(arr, low, high):
    if low < high:
        pi = partition(arr, low, high)
        quick_sort_inplace(arr, low, pi - 1)
        quick_sort_inplace(arr, pi + 1, high)

def partition(arr, low, high):
    pivot = arr[high]
    i = low - 1
    for j in range(low, high):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1

arr = [10, 7, 8, 9, 1, 5]
quick_sort_inplace(arr, 0, len(arr) - 1)
print(arr)  # [1, 5, 7, 8, 9, 10]
```

### 3.6 Counting Sort

```python
def counting_sort(arr):
    if not arr:
        return arr
    max_val = max(arr)
    count = [0] * (max_val + 1)

    for num in arr:
        count[num] += 1

    result = []
    for i, c in enumerate(count):
        result.extend([i] * c)
    return result

print(counting_sort([4, 2, 2, 8, 3, 3, 1]))
# [1, 2, 2, 3, 3, 4, 8]
```

### 3.7 Python Built-in Sort

```python
# Timsort — O(n log n), stable, hybrid of merge sort + insertion sort
arr = [5, 2, 8, 1, 9]
arr.sort()                    # In-place sort
print(arr)                     # [1, 2, 5, 8, 9]

sorted_arr = sorted([5, 2, 8, 1, 9])  # Returns new list
print(sorted_arr)              # [1, 2, 5, 8, 9]

# Custom key
students = [("Alice", 85), ("Bob", 92), ("Charlie", 78)]
students.sort(key=lambda x: x[1])  # Sort by marks
print(students)  # [('Charlie', 78), ('Alice', 85), ('Bob', 92)]
```

### 3.8 Selection: Kth Smallest (Quick Select)

```python
import random

def quick_select(arr, k):
    """Find kth smallest element (1-indexed)."""
    if len(arr) == 1:
        return arr[0]

    pivot = random.choice(arr)
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]

    if k <= len(left):
        return quick_select(left, k)
    elif k <= len(left) + len(middle):
        return pivot
    else:
        return quick_select(right, k - len(left) - len(middle))

arr = [3, 2, 1, 5, 6, 4]
print(quick_select(arr, 2))  # 2 (2nd smallest)
print(quick_select(arr, 5))  # 5 (5th smallest)
```

**Average:** $O(n)$ | **Worst:** $O(n^2)$

---

## 4. MCQs (15 Questions)

**Q1.** Which sorting algorithm has the best worst-case time complexity?
- A) Quick sort
- B) Bubble sort
- C) Merge sort
- D) Selection sort

**Answer:** C) Merge sort — $O(n \log n)$ worst case.

---

**Q2.** Merge sort uses which technique?
- A) Greedy
- B) Dynamic programming
- C) Divide and conquer
- D) Backtracking

**Answer:** C) Divide and conquer

---

**Q3.** Quick sort's worst case occurs when:
- A) Array is already sorted and pivot is always min/max
- B) Array has all equal elements
- C) Array is random
- D) Both A and B

**Answer:** D) Both A and B

---

**Q4.** Which sorting algorithm is **stable**?
- A) Quick sort
- B) Heap sort
- C) Selection sort
- D) Merge sort

**Answer:** D) Merge sort

---

**Q5.** Space complexity of merge sort:
- A) $O(1)$
- B) $O(\log n)$
- C) $O(n)$
- D) $O(n^2)$

**Answer:** C) $O(n)$ — Requires auxiliary arrays for merging.

---

**Q6.** Insertion sort is best suited for:
- A) Large random arrays
- B) Nearly sorted arrays
- C) Reverse sorted arrays
- D) Arrays with many duplicates

**Answer:** B) Nearly sorted arrays — Best case is $O(n)$.

---

**Q7.** The lower bound for comparison-based sorting is:
- A) $O(n)$
- B) $O(n \log n)$
- C) $O(n^2)$
- D) $O(\log n)$

**Answer:** B) $O(n \log n)$ — Proven by decision tree argument.

---

**Q8.** Counting sort's time complexity is:
- A) $O(n \log n)$
- B) $O(n + k)$ where $k$ is the range of values
- C) $O(n^2)$
- D) $O(n)$

**Answer:** B) $O(n + k)$

---

**Q9.** Which sort is used by Python's `sorted()` function?
- A) Quick sort
- B) Merge sort
- C) Timsort (hybrid of merge + insertion sort)
- D) Heap sort

**Answer:** C) Timsort

---

**Q10.** In merge sort, the merge step for two arrays of size $n/2$ takes:
- A) $O(1)$
- B) $O(\log n)$
- C) $O(n)$
- D) $O(n^2)$

**Answer:** C) $O(n)$ — Each element is compared and copied once.

---

**Q11.** Quick select finds the kth smallest element in:
- A) $O(n \log n)$ average
- B) $O(n)$ average
- C) $O(n^2)$ average
- D) $O(\log n)$ average

**Answer:** B) $O(n)$ average

---

**Q12.** Which sorting algorithm performs the minimum number of swaps?
- A) Bubble sort
- B) Selection sort
- C) Insertion sort
- D) Merge sort

**Answer:** B) Selection sort — At most $n - 1$ swaps.

---

**Q13.** Bubble sort's best case (already sorted array) is:
- A) $O(n^2)$
- B) $O(n \log n)$
- C) $O(n)$
- D) $O(1)$

**Answer:** C) $O(n)$ — With the `swapped` flag optimization.

---

**Q14.** Which is NOT an in-place sorting algorithm?
- A) Bubble sort
- B) Merge sort
- C) Quick sort
- D) Insertion sort

**Answer:** B) Merge sort — Requires $O(n)$ extra space.

---

**Q15.** To sort strings by length, then alphabetically:
```python
words = ["banana", "apple", "fig", "date"]
sorted(words, key=lambda x: (len(x), x))
```
Result:
- A) `['fig', 'date', 'apple', 'banana']`
- B) `['apple', 'banana', 'date', 'fig']`
- C) `['fig', 'date', 'apple', 'banana']`
- D) `['banana', 'apple', 'date', 'fig']`

**Answer:** A) `['fig', 'date', 'apple', 'banana']`
