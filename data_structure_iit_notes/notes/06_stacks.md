# Topic 6: Stacks

## 1. Overview

A **Stack** is a linear data structure that follows the **LIFO (Last-In, First-Out)** principle. The last element added is the first one removed — like a stack of plates.

---

## 2. Key Concepts

### 2.1 Stack Operations

| Operation | Description | Time Complexity |
|-----------|-------------|----------------|
| `push(e)` | Add element `e` to the top | $O(1)$ |
| `pop()` | Remove and return the top element | $O(1)$ |
| `top()` / `peek()` | Return the top element without removing | $O(1)$ |
| `is_empty()` | Check if stack is empty | $O(1)$ |
| `len()` / `size()` | Return number of elements | $O(1)$ |

### 2.2 Stack ADT

```
push(5) → [5]
push(3) → [5, 3]
push(7) → [5, 3, 7]
pop()   → returns 7, stack = [5, 3]
peek()  → returns 3
pop()   → returns 3, stack = [5]
```

### 2.3 Applications of Stacks

- **Undo/Redo** operations in text editors
- **Browser back button** (history)
- **Function call management** (call stack)
- **Expression evaluation** (postfix, prefix)
- **Balanced parenthesis** checking
- **String reversal**
- **Depth-First Search (DFS)** in graphs

---

## 3. Code Examples

### 3.1 Stack Using Python List

```python
class Stack:
    def __init__(self):
        self._data = []

    def push(self, e):
        self._data.append(e)

    def pop(self):
        if self.is_empty():
            raise IndexError("Stack is empty")
        return self._data.pop()

    def peek(self):
        if self.is_empty():
            raise IndexError("Stack is empty")
        return self._data[-1]

    def is_empty(self):
        return len(self._data) == 0

    def __len__(self):
        return len(self._data)

    def __str__(self):
        return str(self._data)

# Usage
s = Stack()
s.push(10)
s.push(20)
s.push(30)
print(s)          # [10, 20, 30]
print(s.pop())    # 30
print(s.peek())   # 20
print(len(s))     # 2
```

### 3.2 Reverse a String Using Stack

*(From IIT Patna Lecture)*

```python
def reverse_string(string):
    stack = []

    for char in string:
        stack.append(char)

    reversed_string = ''
    while stack:
        reversed_string += stack.pop()

    return reversed_string

result = reverse_string("Hello")
print("Reversed string:", result)  # olleH
```

### 3.3 Balanced Parenthesis Checker

*(From IIT Patna Lecture)*

**Algorithm:**
1. Create an empty stack
2. Traverse each character:
   - If opening bracket `(`, `{`, `[` → push onto stack
   - If closing bracket `)`, `}`, `]`:
     - If stack is empty → Not Balanced
     - Pop top element → check if it matches
3. After traversal: if stack is empty → Balanced; else → Not Balanced

```python
def is_balanced(expr):
    stack = []
    bracket_map = {')': '(', ']': '[', '}': '{'}

    for char in expr:
        if char in "({[":
            stack.append(char)
        elif char in ")}]":
            if not stack:
                return "Not Balanced"
            top = stack.pop()
            if bracket_map[char] != top:
                return "Not Balanced"

    if not stack:
        return "Balanced"
    else:
        return "Not Balanced"

print(is_balanced("({[]})"))   # Balanced
print(is_balanced("()[]}"))    # Not Balanced
print(is_balanced("((()))"))   # Balanced
print(is_balanced("(()"))      # Not Balanced
```

### 3.4 Postfix Expression Evaluation

```python
def evaluate_postfix(expression):
    stack = []
    for token in expression.split():
        if token.isdigit():
            stack.append(int(token))
        else:
            b = stack.pop()
            a = stack.pop()
            if token == '+': stack.append(a + b)
            elif token == '-': stack.append(a - b)
            elif token == '*': stack.append(a * b)
            elif token == '/': stack.append(a // b)
    return stack.pop()

# Expression: (3 + 4) * 5 = 35
# Postfix: 3 4 + 5 *
print(evaluate_postfix("3 4 + 5 *"))  # 35

# Expression: 2 + 3 * 4 = 14
# Postfix: 2 3 4 * +
print(evaluate_postfix("2 3 4 * +"))  # 14
```

### 3.5 Infix to Postfix Conversion

```python
def infix_to_postfix(expression):
    precedence = {'+': 1, '-': 1, '*': 2, '/': 2, '^': 3}
    stack = []
    output = []

    for token in expression.split():
        if token.isalnum():
            output.append(token)
        elif token == '(':
            stack.append(token)
        elif token == ')':
            while stack and stack[-1] != '(':
                output.append(stack.pop())
            stack.pop()  # remove '('
        else:  # operator
            while (stack and stack[-1] != '(' and
                   stack[-1] in precedence and
                   precedence[stack[-1]] >= precedence[token]):
                output.append(stack.pop())
            stack.append(token)

    while stack:
        output.append(stack.pop())

    return ' '.join(output)

print(infix_to_postfix("3 + 4 * 5"))        # 3 4 5 * +
print(infix_to_postfix("( 3 + 4 ) * 5"))    # 3 4 + 5 *
```

### 3.6 Min Stack

```python
class MinStack:
    def __init__(self):
        self.stack = []
        self.min_stack = []

    def push(self, val):
        self.stack.append(val)
        if not self.min_stack or val <= self.min_stack[-1]:
            self.min_stack.append(val)

    def pop(self):
        val = self.stack.pop()
        if val == self.min_stack[-1]:
            self.min_stack.pop()
        return val

    def get_min(self):
        return self.min_stack[-1]

ms = MinStack()
ms.push(5)
ms.push(2)
ms.push(7)
ms.push(1)
print(ms.get_min())  # 1
ms.pop()
print(ms.get_min())  # 2
```

---

## 4. MCQs (15 Questions)

**Q1.** A stack follows which principle?
- A) FIFO
- B) LIFO
- C) Random access
- D) Priority-based

**Answer:** B) LIFO (Last-In, First-Out)

---

**Q2.** What is the time complexity of `push()` on a stack (array-based)?
- A) $O(n)$
- B) $O(\log n)$
- C) $O(1)$ amortized
- D) $O(n^2)$

**Answer:** C) $O(1)$ amortized

---

**Q3.** What data structure is used by function calls in a program?
- A) Queue
- B) Array
- C) Stack (call stack)
- D) Linked list

**Answer:** C) Stack (call stack)

---

**Q4.** What is the result of these operations: push(1), push(2), pop(), push(3), pop(), pop()?
- A) 1, 2, 3
- B) 2, 3, 1
- C) 3, 2, 1
- D) 2, 1, 3

**Answer:** B) 2, 3, 1

---

**Q5.** Which application uses a stack?
- A) Print job scheduling
- B) Balanced parenthesis checking
- C) Breadth-first search
- D) Round-robin scheduling

**Answer:** B) Balanced parenthesis checking

---

**Q6.** The postfix expression `2 3 + 4 *` evaluates to:
- A) 14
- B) 20
- C) 24
- D) 10

**Answer:** B) 20 — $(2 + 3) \times 4 = 20$

---

**Q7.** What happens when you `pop()` from an empty stack?
- A) Returns None
- B) Returns 0
- C) Raises an error (underflow)
- D) Returns -1

**Answer:** C) Raises an error (underflow)

---

**Q8.** Which expression notation does NOT need parentheses?
- A) Infix
- B) Prefix
- C) Postfix
- D) Both B and C

**Answer:** D) Both B and C — Prefix and postfix notations are unambiguous without parentheses.

---

**Q9.** In the balanced parenthesis problem, the string `({[}])` is:
- A) Balanced
- B) Not Balanced

**Answer:** B) Not Balanced — `[` is closed by `}`, not `]`.

---

**Q10.** What is `peek()` / `top()` operation?
- A) Remove and return top element
- B) Return top element without removing
- C) Check if stack is empty
- D) Return the bottom element

**Answer:** B) Return top element without removing

---

**Q11.** How many stacks are needed to implement a queue?
- A) 1
- B) 2
- C) 3
- D) 0

**Answer:** B) 2

---

**Q12.** The infix expression `A + B * C` in postfix is:
- A) `A B + C *`
- B) `A B C * +`
- C) `+ A * B C`
- D) `A B C + *`

**Answer:** B) `A B C * +` — Multiplication has higher precedence.

---

**Q13.** Which Python list method acts like `push()` for a stack?
- A) `insert(0, x)`
- B) `append(x)`
- C) `extend(x)`
- D) `add(x)`

**Answer:** B) `append(x)`

---

**Q14.** A stack can be implemented using:
- A) Array only
- B) Linked list only
- C) Both array and linked list
- D) Neither

**Answer:** C) Both array and linked list

---

**Q15.** If a stack contains (bottom to top): [A, B, C, D], what is returned by `pop()` followed by `peek()`?
- A) D then D
- B) D then C
- C) A then B
- D) C then B

**Answer:** B) D then C — `pop()` returns D (top), then `peek()` returns C (new top).
