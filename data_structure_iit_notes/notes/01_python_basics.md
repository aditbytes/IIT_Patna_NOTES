# Topic 1: Python Basics & Programming Introduction

## 1. Overview

Python is a high-level, interpreted, general-purpose programming language known for its readability and simplicity. It supports multiple paradigms: procedural, object-oriented, and functional programming.

---

## 2. Key Concepts

### 2.1 Variables and Data Types

Python is **dynamically typed** — you don't declare variable types explicitly.

```python
# Basic data types
x = 10          # int
y = 3.14        # float
name = "Alice"  # str
flag = True     # bool
```

**Common built-in types:**
| Type | Example | Description |
|------|---------|-------------|
| `int` | `42` | Integer numbers |
| `float` | `3.14` | Floating-point numbers |
| `str` | `"hello"` | String (text) |
| `bool` | `True` / `False` | Boolean values |
| `list` | `[1, 2, 3]` | Ordered, mutable sequence |
| `tuple` | `(1, 2, 3)` | Ordered, immutable sequence |
| `dict` | `{"a": 1}` | Key-value mapping |
| `set` | `{1, 2, 3}` | Unordered, unique elements |

### 2.2 Operators

```python
# Arithmetic
a = 10 + 3   # 13
b = 10 - 3   # 7
c = 10 * 3   # 30
d = 10 / 3   # 3.333...
e = 10 // 3  # 3 (floor division)
f = 10 % 3   # 1 (modulus)
g = 2 ** 3   # 8 (exponentiation)

# Comparison
10 == 10   # True
10 != 5    # True
10 > 5     # True
10 <= 10   # True

# Logical
True and False   # False
True or False    # True
not True         # False
```

### 2.3 Control Flow

```python
# if-elif-else
x = 15
if x > 20:
    print("Large")
elif x > 10:
    print("Medium")
else:
    print("Small")
# Output: Medium

# for loop
for i in range(5):
    print(i, end=" ")
# Output: 0 1 2 3 4

# while loop
count = 0
while count < 3:
    print(count)
    count += 1
```

### 2.4 Functions

Functions are defined using the `def` keyword.

```python
def greet(name):
    """Return a greeting message."""
    return f"Hello, {name}!"

print(greet("Alice"))  # Hello, Alice!
```

**Default and keyword arguments:**
```python
def power(base, exp=2):
    return base ** exp

print(power(3))       # 9
print(power(3, 3))    # 27
print(power(exp=4, base=2))  # 16
```

**Variable-length arguments:**
```python
def add_all(*args):
    return sum(args)

print(add_all(1, 2, 3, 4))  # 10

def show_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

show_info(name="Alice", age=20)
```

### 2.5 Lists and List Comprehensions

```python
# List operations
nums = [3, 1, 4, 1, 5, 9]
nums.append(2)       # [3, 1, 4, 1, 5, 9, 2]
nums.sort()          # [1, 1, 2, 3, 4, 5, 9]
nums.reverse()       # [9, 5, 4, 3, 2, 1, 1]
print(len(nums))     # 7
print(nums[0])       # 9
print(nums[-1])      # 1

# List comprehension
squares = [x**2 for x in range(1, 6)]
# [1, 4, 9, 16, 25]

evens = [x for x in range(20) if x % 2 == 0]
# [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
```

### 2.6 Strings

```python
s = "Hello, World!"
print(s.upper())       # "HELLO, WORLD!"
print(s.lower())       # "hello, world!"
print(s.split(", "))   # ["Hello", "World!"]
print(s.replace("World", "Python"))  # "Hello, Python!"
print(len(s))          # 13
print(s[0:5])          # "Hello"
print(s[::-1])         # "!dlroW ,olleH" (reverse)
```

### 2.7 Dictionaries

```python
student = {"name": "Alice", "roll": 101, "marks": 85}

# Access
print(student["name"])          # Alice
print(student.get("marks", 0))  # 85

# Iterate
for key, value in student.items():
    print(f"{key}: {value}")

# Add / Update
student["grade"] = "A"
student["marks"] = 90
```

### 2.8 Input / Output

```python
name = input("Enter your name: ")
age = int(input("Enter your age: "))
print(f"Name: {name}, Age: {age}")
```

### 2.9 Exception Handling

```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero!")
finally:
    print("This always executes.")
```

---

## 3. Code Examples from IIT Patna Lectures

### Sum of First N Natural Numbers (Iterative)

```python
def sum_n(n):
    total = 0
    for i in range(1, n + 1):
        total += i
    return total

print(sum_n(10))  # 55
```

### Sum of First N Natural Numbers (Formula)

```python
def sum_n_formula(n):
    return n * (n + 1) // 2

print(sum_n_formula(10))  # 55
```

---

## 4. MCQs (10 Questions)

**Q1.** What is the output of `type(3.0)` in Python?
- A) `<class 'int'>`
- B) `<class 'float'>`
- C) `<class 'double'>`
- D) `<class 'number'>`

**Answer:** B) `<class 'float'>`

---

**Q2.** What does `10 // 3` evaluate to?
- A) 3.33
- B) 3.0
- C) 3
- D) 4

**Answer:** C) 3

---

**Q3.** Which of the following is **immutable** in Python?
- A) list
- B) dict
- C) set
- D) tuple

**Answer:** D) tuple

---

**Q4.** What is the output of `[1, 2, 3] + [4, 5]`?
- A) `[5, 7]`
- B) `[1, 2, 3, 4, 5]`
- C) Error
- D) `[[1,2,3],[4,5]]`

**Answer:** B) `[1, 2, 3, 4, 5]`

---

**Q5.** What keyword is used to define a function in Python?
- A) `func`
- B) `function`
- C) `def`
- D) `define`

**Answer:** C) `def`

---

**Q6.** What is the output of `len("Python")`?
- A) 5
- B) 6
- C) 7
- D) Error

**Answer:** B) 6

---

**Q7.** Which operator is used for exponentiation in Python?
- A) `^`
- B) `**`
- C) `//`
- D) `%%`

**Answer:** B) `**`

---

**Q8.** What is the output of `bool("")`?
- A) `True`
- B) `False`
- C) `None`
- D) Error

**Answer:** B) `False` — Empty string is falsy in Python.

---

**Q9.** What does `range(5)` generate?
- A) `[1, 2, 3, 4, 5]`
- B) `[0, 1, 2, 3, 4]`
- C) `(0, 1, 2, 3, 4)`
- D) `{0, 1, 2, 3, 4}`

**Answer:** B) `[0, 1, 2, 3, 4]` (conceptually; `range` returns a range object)

---

**Q10.** What is the result of `"abc" * 3`?
- A) `"abc3"`
- B) `"abcabcabc"`
- C) Error
- D) `"aaa bbb ccc"`

**Answer:** B) `"abcabcabc"`

---

**Q11.** Which method adds an element to the end of a list?
- A) `insert()`
- B) `add()`
- C) `append()`
- D) `push()`

**Answer:** C) `append()`

---

**Q12.** What is the output of `{"a": 1, "b": 2}.get("c", 0)`?
- A) `None`
- B) `0`
- C) Error
- D) `"c"`

**Answer:** B) `0`
