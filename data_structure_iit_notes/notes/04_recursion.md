# Topic 4: Recursion

## 1. Overview

**Recursion** is a technique where a function calls itself to solve smaller instances of the same problem. Every recursive function must have:
1. **Base case** — the simplest case that stops recursion
2. **Recursive case** — the function calls itself with a smaller/simpler input

---

## 2. Key Concepts

### 2.1 How Recursion Works

Each recursive call creates a new **stack frame** in memory. When the base case is reached, the stack "unwinds" and returns values back up.

```
factorial(4)
  → 4 * factorial(3)
       → 3 * factorial(2)
            → 2 * factorial(1)
                 → 1  (base case)
            ← 2 * 1 = 2
       ← 3 * 2 = 6
  ← 4 * 6 = 24
```

### 2.2 Recursion vs Iteration

| Feature | Recursion | Iteration |
|---------|-----------|-----------|
| Approach | Function calls itself | Uses loops |
| Memory | Uses call stack ($O(n)$ space) | Usually $O(1)$ space |
| Readability | Often more elegant | Can be more straightforward |
| Performance | Overhead of function calls | Generally faster |
| Risk | Stack overflow for deep recursion | No stack overflow |

---

## 3. Code Examples

### 3.1 Factorial

```python
def factorial(n):
    if n == 0 or n == 1:   # base case
        return 1
    return n * factorial(n - 1)  # recursive case

print(factorial(5))  # 120
```

### 3.2 Sum of First N Natural Numbers

*(From IIT Patna Lecture)*

```python
def sum_of_n(n):
    if n == 1:
        return 1
    return n + sum_of_n(n - 1)

print(sum_of_n(10))  # 55
```

**Trace:**
```
sum_of_n(4) = 4 + sum_of_n(3)
            = 4 + 3 + sum_of_n(2)
            = 4 + 3 + 2 + sum_of_n(1)
            = 4 + 3 + 2 + 1
            = 10
```

### 3.3 Fibonacci Sequence

*(From IIT Patna Lecture)*

```python
def fibonacci(n):
    if n == 0:
        return 0
    if n == 1:
        return 1
    return fibonacci(n - 1) + fibonacci(n - 2)

for i in range(10):
    print(fibonacci(i), end=" ")
# 0 1 1 2 3 5 8 13 21 34
```

**Time Complexity:** $O(2^n)$ — exponential (many repeated subproblems)
**Space Complexity:** $O(n)$ — max recursion depth

### 3.4 Fibonacci with Memoization

```python
def fibonacci_memo(n, memo={}):
    if n in memo:
        return memo[n]
    if n <= 1:
        return n
    memo[n] = fibonacci_memo(n - 1, memo) + fibonacci_memo(n - 2, memo)
    return memo[n]

print(fibonacci_memo(50))  # 12586269025 — fast!
# Time: O(n), Space: O(n)
```

### 3.5 Power Function

```python
# O(n) — linear recursion
def power(base, exp):
    if exp == 0:
        return 1
    return base * power(base, exp - 1)

# O(log n) — fast exponentiation
def fast_power(base, exp):
    if exp == 0:
        return 1
    if exp % 2 == 0:
        half = fast_power(base, exp // 2)
        return half * half
    else:
        return base * fast_power(base, exp - 1)

print(power(2, 10))       # 1024
print(fast_power(2, 10))  # 1024
```

### 3.6 Binary Search (Recursive)

```python
def binary_search(arr, target, low, high):
    if low > high:
        return -1
    mid = (low + high) // 2
    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        return binary_search(arr, target, mid + 1, high)
    else:
        return binary_search(arr, target, low, mid - 1)

arr = [2, 5, 8, 12, 16, 23, 38, 56, 72, 91]
print(binary_search(arr, 23, 0, len(arr) - 1))  # 5
```

### 3.7 Tower of Hanoi

```python
def tower_of_hanoi(n, source, auxiliary, destination):
    if n == 1:
        print(f"Move disk 1 from {source} to {destination}")
        return
    tower_of_hanoi(n - 1, source, destination, auxiliary)
    print(f"Move disk {n} from {source} to {destination}")
    tower_of_hanoi(n - 1, auxiliary, source, destination)

tower_of_hanoi(3, 'A', 'B', 'C')
# Move disk 1 from A to C
# Move disk 2 from A to B
# Move disk 1 from C to B
# Move disk 3 from A to C
# Move disk 1 from B to A
# Move disk 2 from B to C
# Move disk 1 from A to C
```

**Number of moves:** $2^n - 1$

### 3.8 Sum of Digits

```python
def sum_of_digits(n):
    if n < 10:
        return n
    return n % 10 + sum_of_digits(n // 10)

print(sum_of_digits(1234))  # 10 (1+2+3+4)
```

### 3.9 Palindrome Check

```python
def is_palindrome(s):
    if len(s) <= 1:
        return True
    if s[0] != s[-1]:
        return False
    return is_palindrome(s[1:-1])

print(is_palindrome("racecar"))  # True
print(is_palindrome("hello"))    # False
```

---

## 4. Types of Recursion

| Type | Description | Example |
|------|-------------|---------|
| **Linear** | One recursive call per invocation | Factorial, sum |
| **Binary** | Two recursive calls | Fibonacci, merge sort |
| **Tail** | Recursive call is the last operation | Can be optimized by compiler |
| **Mutual** | Function A calls B, B calls A | Even/odd check |

**Tail Recursion Example:**
```python
def factorial_tail(n, acc=1):
    if n <= 1:
        return acc
    return factorial_tail(n - 1, n * acc)

print(factorial_tail(5))  # 120
# Note: Python does NOT optimize tail recursion, but the concept is important.
```

---

## 5. Python Recursion Limit

```python
import sys
print(sys.getrecursionlimit())  # Default: 1000
sys.setrecursionlimit(10000)    # Increase if needed (use cautiously!)
```

---

## 6. MCQs (15 Questions)

**Q1.** Every recursive function must have:
- A) A loop
- B) A base case
- C) A global variable
- D) Multiple parameters

**Answer:** B) A base case

---

**Q2.** What happens if a recursive function has no base case?
- A) It returns 0
- B) It causes infinite recursion (stack overflow)
- C) It returns None
- D) It optimizes automatically

**Answer:** B) It causes infinite recursion (stack overflow)

---

**Q3.** What is the output of `factorial(0)`?
```python
def factorial(n):
    if n == 0: return 1
    return n * factorial(n-1)
```
- A) 0
- B) 1
- C) Error
- D) None

**Answer:** B) 1

---

**Q4.** The time complexity of naive recursive Fibonacci is:
- A) $O(n)$
- B) $O(n^2)$
- C) $O(2^n)$
- D) $O(\log n)$

**Answer:** C) $O(2^n)$

---

**Q5.** How many moves are needed for Tower of Hanoi with 4 disks?
- A) 8
- B) 15
- C) 16
- D) 31

**Answer:** B) 15 — $2^4 - 1 = 15$

---

**Q6.** What is **tail recursion**?
- A) Recursion that starts from the end
- B) A recursive call that is the last operation in the function
- C) Recursion that uses a tail pointer
- D) Recursion with two base cases

**Answer:** B) A recursive call that is the last operation in the function

---

**Q7.** What is the space complexity of a recursive function with $n$ recursive calls (no tail optimization)?
- A) $O(1)$
- B) $O(\log n)$
- C) $O(n)$
- D) $O(n^2)$

**Answer:** C) $O(n)$ — Each call adds a frame to the call stack.

---

**Q8.** What does memoization do?
- A) Removes the base case
- B) Stores results of subproblems to avoid recomputation
- C) Converts recursion to iteration
- D) Reduces space complexity

**Answer:** B) Stores results of subproblems to avoid recomputation

---

**Q9.** What is the output?
```python
def f(n):
    if n <= 0: return
    print(n, end=" ")
    f(n - 2)
f(5)
```
- A) `5 4 3 2 1`
- B) `5 3 1`
- C) `5 3 1 -1`
- D) `1 3 5`

**Answer:** B) `5 3 1`

---

**Q10.** Python's default recursion limit is approximately:
- A) 100
- B) 500
- C) 1000
- D) 10000

**Answer:** C) 1000

---

**Q11.** Which problem is NOT naturally suited for recursion?
- A) Tree traversal
- B) Finding maximum in an array
- C) Tower of Hanoi
- D) Fibonacci

**Answer:** B) Finding maximum in an array — Better solved iteratively.

---

**Q12.** The recursive call `binary_search(arr, target, mid+1, high)` reduces the problem by:
- A) One element
- B) Half the array
- C) Two elements
- D) Depends on the array

**Answer:** B) Half the array

---

**Q13.** What is the output?
```python
def f(n):
    if n <= 0: return 0
    return n + f(n - 1)
print(f(3))
```
- A) 3
- B) 6
- C) 9
- D) 0

**Answer:** B) 6 — $3 + 2 + 1 + 0 = 6$

---

**Q14.** Memoized Fibonacci has time complexity:
- A) $O(2^n)$
- B) $O(n^2)$
- C) $O(n)$
- D) $O(\log n)$

**Answer:** C) $O(n)$

---

**Q15.** Which type of recursion involves two functions calling each other?
- A) Linear recursion
- B) Binary recursion
- C) Mutual recursion
- D) Tail recursion

**Answer:** C) Mutual recursion
