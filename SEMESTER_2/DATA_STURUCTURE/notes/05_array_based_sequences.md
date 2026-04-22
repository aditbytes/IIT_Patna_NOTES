# Topic 5: Array-Based Sequences

## 1. Overview

An **array** is a contiguous block of memory storing elements of the same type. Python provides several array-based sequences: **list**, **tuple**, and **str**. These support indexing, slicing, and iteration.

---

## 2. Key Concepts

### 2.1 Low-Level Arrays

- A block of **contiguous memory cells**, each the same size.
- Each cell is accessed by its **index** in $O(1)$ time.
- Memory address of element `i` = `start + i × element_size`

### 2.2 Referential Arrays (Python Lists)

Python lists store **references (pointers)** to objects, not the objects themselves.

```python
# All elements are references
arr = [1, "hello", 3.14, [1,2]]
# Each slot in the internal array holds a pointer to the actual object
```

### 2.3 Dynamic Arrays

Python lists are **dynamic arrays** — they automatically resize when capacity is exceeded.

**How resizing works:**
1. Allocate a new, larger array (typically 2× the current size).
2. Copy all elements from the old array.
3. Redirect the reference to the new array.

```python
import sys

data = []
for i in range(30):
    a = len(data)
    b = sys.getsizeof(data)
    print(f"Length: {a:3d}  Size in bytes: {b:4d}")
    data.append(None)
```

**Amortized cost of append:** $O(1)$

### 2.4 Compact Arrays (array module)

For numeric data, Python's `array` module stores actual values (not references):

```python
import array

# 'i' = signed int, 'f' = float, 'd' = double
arr = array.array('i', [1, 2, 3, 4, 5])
print(arr[2])    # 3
arr.append(6)
print(arr)       # array('i', [1, 2, 3, 4, 5, 6])
```

---

## 3. Operations and Complexities

### 3.1 List Operations

| Operation | Syntax | Time Complexity |
|-----------|--------|----------------|
| Access by index | `arr[i]` | $O(1)$ |
| Append | `arr.append(x)` | $O(1)$ amortized |
| Insert at index | `arr.insert(i, x)` | $O(n)$ |
| Remove by value | `arr.remove(x)` | $O(n)$ |
| Pop last | `arr.pop()` | $O(1)$ |
| Pop at index | `arr.pop(i)` | $O(n)$ |
| Search (in) | `x in arr` | $O(n)$ |
| Length | `len(arr)` | $O(1)$ |
| Slice | `arr[a:b]` | $O(b - a)$ |
| Sort | `arr.sort()` | $O(n \log n)$ |
| Reverse | `arr.reverse()` | $O(n)$ |
| Concatenation | `arr1 + arr2` | $O(n + m)$ |
| Extend | `arr.extend(other)` | $O(k)$ where k = len(other) |

### 3.2 Insertion and Deletion

```python
# Insert at beginning — O(n), shifts all elements
arr = [1, 2, 3, 4]
arr.insert(0, 0)   # [0, 1, 2, 3, 4]

# Insert at end — O(1) amortized
arr.append(5)       # [0, 1, 2, 3, 4, 5]

# Delete from beginning — O(n)
arr.pop(0)          # [1, 2, 3, 4, 5]

# Delete from end — O(1)
arr.pop()           # [1, 2, 3, 4]
```

---

## 4. Code Examples

### 4.1 Implementing a Dynamic Array

```python
import ctypes

class DynamicArray:
    def __init__(self):
        self._n = 0          # actual number of elements
        self._capacity = 1   # current capacity
        self._A = self._make_array(self._capacity)

    def __len__(self):
        return self._n

    def __getitem__(self, k):
        if not 0 <= k < self._n:
            raise IndexError("Index out of range")
        return self._A[k]

    def append(self, obj):
        if self._n == self._capacity:
            self._resize(2 * self._capacity)
        self._A[self._n] = obj
        self._n += 1

    def _resize(self, c):
        B = self._make_array(c)
        for k in range(self._n):
            B[k] = self._A[k]
        self._A = B
        self._capacity = c

    def _make_array(self, c):
        return (c * ctypes.py_object)()

# Usage
da = DynamicArray()
for i in range(10):
    da.append(i)
print(len(da))   # 10
print(da[5])     # 5
```

### 4.2 Rotating an Array

```python
def rotate_left(arr, k):
    """Rotate array left by k positions."""
    n = len(arr)
    k = k % n
    return arr[k:] + arr[:k]

def rotate_right(arr, k):
    """Rotate array right by k positions."""
    n = len(arr)
    k = k % n
    return arr[n-k:] + arr[:n-k]

arr = [1, 2, 3, 4, 5]
print(rotate_left(arr, 2))   # [3, 4, 5, 1, 2]
print(rotate_right(arr, 2))  # [4, 5, 1, 2, 3]
```

### 4.3 Caesar Cipher

```python
def caesar_encrypt(text, shift):
    result = []
    for char in text:
        if char.isalpha():
            base = ord('A') if char.isupper() else ord('a')
            result.append(chr((ord(char) - base + shift) % 26 + base))
        else:
            result.append(char)
    return ''.join(result)

def caesar_decrypt(text, shift):
    return caesar_encrypt(text, -shift)

encrypted = caesar_encrypt("Hello World", 3)
print(encrypted)                          # Khoor Zruog
print(caesar_decrypt(encrypted, 3))       # Hello World
```

### 4.4 Two-Dimensional Arrays (Matrices)

```python
# Create a 3x3 matrix
matrix = [[0] * 3 for _ in range(3)]

# Fill with values
for i in range(3):
    for j in range(3):
        matrix[i][j] = i * 3 + j + 1

# Print matrix
for row in matrix:
    print(row)
# [1, 2, 3]
# [4, 5, 6]
# [7, 8, 9]

# Transpose
def transpose(matrix):
    n = len(matrix)
    m = len(matrix[0])
    return [[matrix[j][i] for j in range(n)] for i in range(m)]

print(transpose(matrix))
# [[1, 4, 7], [2, 5, 8], [3, 6, 9]]
```

### 4.5 Tuple and String (Immutable Sequences)

```python
# Tuples — immutable
t = (1, 2, 3)
# t[0] = 10  # TypeError!
print(t[0])    # 1
print(t + (4, 5))  # (1, 2, 3, 4, 5)

# Strings — immutable sequence of characters
s = "hello"
# s[0] = 'H'  # TypeError!
print(s.upper())     # "HELLO"
print(s[1:4])        # "ell"
```

---

## 5. MCQs (12 Questions)

**Q1.** What is the time complexity of accessing element at index `i` in a Python list?
- A) $O(n)$
- B) $O(\log n)$
- C) $O(1)$
- D) $O(n^2)$

**Answer:** C) $O(1)$ — Direct access via memory address calculation.

---

**Q2.** What is the amortized time complexity of `list.append()` in Python?
- A) $O(n)$
- B) $O(1)$
- C) $O(\log n)$
- D) $O(n^2)$

**Answer:** B) $O(1)$

---

**Q3.** What happens internally when a Python list runs out of capacity?
- A) An error is raised
- B) A new larger array is allocated and elements are copied
- C) Elements are compressed
- D) A linked list is used instead

**Answer:** B) A new larger array is allocated and elements are copied

---

**Q4.** Time complexity of `arr.insert(0, x)` for a list of size n:
- A) $O(1)$
- B) $O(\log n)$
- C) $O(n)$
- D) $O(n^2)$

**Answer:** C) $O(n)$ — All elements must be shifted right.

---

**Q5.** Which Python sequence type is immutable?
- A) list
- B) tuple
- C) dict
- D) set

**Answer:** B) tuple

---

**Q6.** What does Python's `sys.getsizeof([])` measure?
- A) Number of elements
- B) Memory used by the list object in bytes
- C) Capacity of the list
- D) Total memory of all elements

**Answer:** B) Memory used by the list object in bytes

---

**Q7.** Python lists are:
- A) Arrays of actual values stored contiguously
- B) Linked lists
- C) Arrays of references (pointers) to objects
- D) Hash tables

**Answer:** C) Arrays of references (pointers) to objects

---

**Q8.** Time complexity of `x in arr` for an unsorted list:
- A) $O(1)$
- B) $O(\log n)$
- C) $O(n)$
- D) $O(n^2)$

**Answer:** C) $O(n)$

---

**Q9.** What is the output of `[1,2,3][-1]`?
- A) Error
- B) 1
- C) 3
- D) -1

**Answer:** C) 3 — Negative indexing accesses from the end.

---

**Q10.** The `array.array('i', [1,2,3])` stores:
- A) References to integer objects
- B) Actual integer values contiguously
- C) Strings
- D) Pointers to arrays

**Answer:** B) Actual integer values contiguously — This is a compact array.

---

**Q11.** Slicing `arr[2:5]` creates:
- A) A view of the original list
- B) A new list with elements at indices 2, 3, 4
- C) A new list with elements at indices 2, 3, 4, 5
- D) A reference to the same list

**Answer:** B) A new list with elements at indices 2, 3, 4

---

**Q12.** Why is `[[0]*3]*3` problematic for 2D arrays?
- A) It creates a syntax error
- B) All three inner lists reference the same list object
- C) It creates too much memory
- D) It doesn't create a 2D array

**Answer:** B) All three inner lists reference the same list object — Modifying one row affects all rows.
