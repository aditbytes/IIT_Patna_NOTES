# Topic 3: Algorithm Analysis

## 1. Overview

Algorithm analysis is the study of the **efficiency** of algorithms — how their resource usage (time, space) grows with input size. This allows us to compare algorithms independently of hardware.

---

## 2. Key Concepts

### 2.1 Why Analyze Algorithms?

- Different algorithms solve the same problem with different efficiencies.
- We want to predict performance **before** running the code.
- Analysis helps choose the **best** algorithm for a given problem size.

### 2.2 Counting Primitive Operations

Each "step" is a **primitive operation**: assignment, comparison, arithmetic, array access, function call/return.

```python
def find_max(arr):
    max_val = arr[0]        # 1 op (assignment)
    for i in range(1, len(arr)):  # n-1 iterations
        if arr[i] > max_val:     # 1 comparison per iteration
            max_val = arr[i]      # 1 assignment (worst case)
    return max_val            # 1 op
# Total: O(n)
```

### 2.3 Asymptotic Notation

| Notation | Name | Meaning |
|----------|------|---------|
| $O(f(n))$ | Big-O | **Upper bound** — worst case grows at most as fast as $f(n)$ |
| $\Omega(f(n))$ | Big-Omega | **Lower bound** — best case grows at least as fast as $f(n)$ |
| $\Theta(f(n))$ | Big-Theta | **Tight bound** — grows exactly as fast as $f(n)$ |

**Formal Definitions:**

- $f(n) = O(g(n))$ if there exist constants $c > 0$ and $n_0$ such that $f(n) \leq c \cdot g(n)$ for all $n \geq n_0$
- $f(n) = \Omega(g(n))$ if there exist constants $c > 0$ and $n_0$ such that $f(n) \geq c \cdot g(n)$ for all $n \geq n_0$
- $f(n) = \Theta(g(n))$ if $f(n) = O(g(n))$ AND $f(n) = \Omega(g(n))$

### 2.4 Common Growth Rates

Listed from fastest to slowest:

| Complexity | Name | Example |
|-----------|------|---------|
| $O(1)$ | Constant | Array access by index |
| $O(\log n)$ | Logarithmic | Binary search |
| $O(n)$ | Linear | Linear search |
| $O(n \log n)$ | Linearithmic | Merge sort, heap sort |
| $O(n^2)$ | Quadratic | Bubble sort, selection sort |
| $O(n^3)$ | Cubic | Matrix multiplication (naive) |
| $O(2^n)$ | Exponential | All subsets, recursive Fibonacci |
| $O(n!)$ | Factorial | All permutations |

### 2.5 Rules for Big-O Analysis

1. **Drop constants:** $O(3n) = O(n)$
2. **Drop lower-order terms:** $O(n^2 + n) = O(n^2)$
3. **Consecutive statements:** Add complexities — $O(n) + O(n^2) = O(n^2)$
4. **Nested loops:** Multiply complexities — Two nested loops over $n$ → $O(n^2)$
5. **Conditional (if-else):** Take the worse branch

### 2.6 Analyzing Loops

```python
# O(n)
for i in range(n):
    print(i)

# O(n^2)
for i in range(n):
    for j in range(n):
        print(i, j)

# O(n * m)
for i in range(n):
    for j in range(m):
        print(i, j)

# O(log n)
i = n
while i > 1:
    i = i // 2

# O(n log n)
for i in range(n):       # O(n)
    j = n
    while j > 1:          # O(log n)
        j = j // 2
```

### 2.7 Best, Worst, and Average Case

For **Linear Search** in an array of $n$ elements:

| Case | When | Comparisons |
|------|------|------------|
| Best | Element is first | $O(1)$ |
| Worst | Element is last or absent | $O(n)$ |
| Average | Element is somewhere in middle | $O(n/2) = O(n)$ |

### 2.8 Space Complexity

Space complexity measures the **extra memory** used by an algorithm (beyond input).

```python
# O(1) space — iterative sum
def sum_iter(n):
    total = 0
    for i in range(1, n + 1):
        total += i
    return total

# O(n) space — recursive sum (call stack)
def sum_rec(n):
    if n == 0:
        return 0
    return n + sum_rec(n - 1)
```

### 2.9 Amortized Analysis

Some operations are occasionally expensive but cheap on average.

**Example:** Python `list.append()` — occasionally triggers resize (copy entire array), but amortized cost is $O(1)$ per append.

```python
# Dynamic array resizing
# append: O(1) amortized
# When capacity is full, Python doubles the array size
# Copy cost: O(n), but happens only every O(n) appends
# Amortized: O(n) / O(n) = O(1) per operation
```

---

## 3. Code Examples

### Timing Comparison

```python
import time

def linear_search(arr, target):
    for i in range(len(arr)):
        if arr[i] == target:
            return i
    return -1

def binary_search(arr, target):
    low, high = 0, len(arr) - 1
    while low <= high:
        mid = (low + high) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            low = mid + 1
        else:
            high = mid - 1
    return -1

n = 1_000_000
arr = list(range(n))
target = n - 1

start = time.time()
linear_search(arr, target)
print(f"Linear Search: {time.time() - start:.6f}s")

start = time.time()
binary_search(arr, target)
print(f"Binary Search: {time.time() - start:.6f}s")
```

### Prefix Averages — O(n²) vs O(n)

```python
# O(n^2) approach
def prefix_avg_quadratic(arr):
    n = len(arr)
    result = [0] * n
    for i in range(n):
        total = 0
        for j in range(i + 1):
            total += arr[j]
        result[i] = total / (i + 1)
    return result

# O(n) approach
def prefix_avg_linear(arr):
    n = len(arr)
    result = [0] * n
    total = 0
    for i in range(n):
        total += arr[i]
        result[i] = total / (i + 1)
    return result

arr = [1, 2, 3, 4, 5]
print(prefix_avg_linear(arr))  # [1.0, 1.5, 2.0, 2.5, 3.0]
```

---

## 4. MCQs (15 Questions)

**Q1.** What does Big-O notation represent?
- A) Best case
- B) Average case
- C) Upper bound (worst case growth)
- D) Exact running time

**Answer:** C) Upper bound (worst case growth)

---

**Q2.** What is the time complexity of accessing an element in an array by index?
- A) $O(n)$
- B) $O(\log n)$
- C) $O(1)$
- D) $O(n^2)$

**Answer:** C) $O(1)$

---

**Q3.** What is $O(3n^2 + 5n + 100)$ simplified?
- A) $O(3n^2)$
- B) $O(n^2)$
- C) $O(n)$
- D) $O(100)$

**Answer:** B) $O(n^2)$ — Drop constants and lower-order terms.

---

**Q4.** What is the time complexity of binary search?
- A) $O(n)$
- B) $O(n \log n)$
- C) $O(\log n)$
- D) $O(1)$

**Answer:** C) $O(\log n)$

---

**Q5.** Two nested loops each iterating $n$ times give:
- A) $O(n)$
- B) $O(2n)$
- C) $O(n^2)$
- D) $O(n \log n)$

**Answer:** C) $O(n^2)$

---

**Q6.** What is the space complexity of an algorithm that uses a fixed number of variables regardless of input?
- A) $O(n)$
- B) $O(\log n)$
- C) $O(1)$
- D) $O(n^2)$

**Answer:** C) $O(1)$

---

**Q7.** Which grows faster: $O(2^n)$ or $O(n^3)$?
- A) $O(n^3)$
- B) $O(2^n)$
- C) Same
- D) Depends on $n$

**Answer:** B) $O(2^n)$ — Exponential grows faster than polynomial for large $n$.

---

**Q8.** What is $\Theta$ notation?
- A) Upper bound only
- B) Lower bound only
- C) Tight bound (both upper and lower)
- D) Average case only

**Answer:** C) Tight bound (both upper and lower)

---

**Q9.** What is the time complexity of this code?
```python
i = 1
while i < n:
    i = i * 2
```
- A) $O(n)$
- B) $O(n^2)$
- C) $O(\log n)$
- D) $O(2^n)$

**Answer:** C) $O(\log n)$ — `i` doubles each iteration, so loop runs $\log_2 n$ times.

---

**Q10.** Amortized $O(1)$ for `list.append()` in Python means:
- A) Every append takes $O(1)$
- B) Average cost over many appends is $O(1)$
- C) Worst case is $O(1)$
- D) Best case is $O(1)$

**Answer:** B) Average cost over many appends is $O(1)$

---

**Q11.** Which sorting algorithm has worst-case $O(n \log n)$?
- A) Quicksort
- B) Bubble sort
- C) Merge sort
- D) Selection sort

**Answer:** C) Merge sort

---

**Q12.** If `f(n) = 5n + 3`, which is TRUE?
- A) $f(n) = O(n^2)$
- B) $f(n) = O(n)$
- C) $f(n) = \Theta(n)$
- D) All of the above

**Answer:** D) All of the above — $O(n^2)$ is a valid (loose) upper bound, $O(n)$ is tight upper bound, and $\Theta(n)$ is exact.

---

**Q13.** What is the worst-case complexity of linear search?
- A) $O(1)$
- B) $O(\log n)$
- C) $O(n)$
- D) $O(n^2)$

**Answer:** C) $O(n)$

---

**Q14.** Space complexity of recursive Fibonacci `fib(n)` (naive) is:
- A) $O(1)$
- B) $O(n)$
- C) $O(2^n)$
- D) $O(n^2)$

**Answer:** B) $O(n)$ — Maximum recursion depth is $n$.

---

**Q15.** Which of the following is NOT true about Big-O?
- A) $O(n) + O(n) = O(n)$
- B) $O(n) \times O(n) = O(n^2)$
- C) $O(n + m) = O(n)$ always
- D) $O(1) \subset O(n)$

**Answer:** C) $O(n + m) = O(n)$ always — This is only true if $m = O(n)$; in general, we cannot drop $m$.
