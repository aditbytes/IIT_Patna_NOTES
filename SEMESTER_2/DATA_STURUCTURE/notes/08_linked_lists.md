# Topic 8: Linked Lists

## 1. Overview

A **Linked List** is a linear data structure where elements (nodes) are stored in separate memory locations and connected via **pointers**. Unlike arrays, linked lists do not use contiguous memory.

```
[Data|Next] → [Data|Next] → [Data|Next] → None
   10            20              30
```

---

## 2. Key Concepts

### 2.1 Node Structure

Each **node** contains:
1. **Data** — the value stored
2. **Next** — a reference (pointer) to the next node

### 2.2 Types of Linked Lists

| Type | Description |
|------|-------------|
| **Singly Linked** | Each node points to the next node |
| **Doubly Linked** | Each node points to both next and previous |
| **Circular Singly** | Last node points back to the head |
| **Circular Doubly** | Doubly linked + last connects to head |

### 2.3 Linked List vs Array

| Feature | Array | Linked List |
|---------|-------|-------------|
| Memory | Contiguous | Non-contiguous |
| Access by index | $O(1)$ | $O(n)$ |
| Insert at beginning | $O(n)$ | $O(1)$ |
| Insert at end | $O(1)$ amortized | $O(n)$ or $O(1)$ with tail |
| Insert at position | $O(n)$ | $O(n)$ |
| Delete from beginning | $O(n)$ | $O(1)$ |
| Memory overhead | Low | Extra pointer per node |
| Cache performance | Good (contiguous) | Poor (scattered) |

---

## 3. Code Examples

### 3.1 Singly Linked List

*(From IIT Patna Lecture)*

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None


class LinkedList:
    def __init__(self):
        self.head = None

    def insert_at_beginning(self, data):
        new_node = Node(data)
        new_node.next = self.head
        self.head = new_node

    def insert_at_end(self, data):
        new_node = Node(data)
        if self.head is None:
            self.head = new_node
            return
        temp = self.head
        while temp.next:
            temp = temp.next
        temp.next = new_node

    def insert_at_position(self, data, position):
        if position == 0:
            self.insert_at_beginning(data)
            return
        new_node = Node(data)
        temp = self.head
        for _ in range(position - 1):
            if temp is None:
                raise IndexError("Position out of range")
            temp = temp.next
        new_node.next = temp.next
        temp.next = new_node

    def delete_node(self, key):
        temp = self.head
        # If head node itself holds the key
        if temp and temp.data == key:
            self.head = temp.next
            return
        # Search for the key
        prev = None
        while temp and temp.data != key:
            prev = temp
            temp = temp.next
        if temp is None:
            print(f"{key} not found")
            return
        prev.next = temp.next

    def search(self, key):
        temp = self.head
        pos = 0
        while temp:
            if temp.data == key:
                return pos
            temp = temp.next
            pos += 1
        return -1

    def length(self):
        count = 0
        temp = self.head
        while temp:
            count += 1
            temp = temp.next
        return count

    def display(self):
        temp = self.head
        while temp:
            print(temp.data, end=" -> ")
            temp = temp.next
        print("None")


# Usage
ll = LinkedList()
ll.insert_at_beginning(10)
ll.insert_at_beginning(5)
ll.insert_at_end(20)
ll.insert_at_end(30)
ll.display()  # 5 -> 10 -> 20 -> 30 -> None

ll.insert_at_position(15, 2)
ll.display()  # 5 -> 10 -> 15 -> 20 -> 30 -> None

ll.delete_node(15)
ll.display()  # 5 -> 10 -> 20 -> 30 -> None

print(ll.search(20))  # 2
print(ll.length())    # 4
```

### 3.2 Doubly Linked List

```python
class DNode:
    def __init__(self, data):
        self.data = data
        self.prev = None
        self.next = None


class DoublyLinkedList:
    def __init__(self):
        self.head = None

    def insert_at_beginning(self, data):
        new_node = DNode(data)
        new_node.next = self.head
        if self.head:
            self.head.prev = new_node
        self.head = new_node

    def insert_at_end(self, data):
        new_node = DNode(data)
        if not self.head:
            self.head = new_node
            return
        temp = self.head
        while temp.next:
            temp = temp.next
        temp.next = new_node
        new_node.prev = temp

    def delete_node(self, key):
        temp = self.head
        while temp:
            if temp.data == key:
                if temp.prev:
                    temp.prev.next = temp.next
                else:
                    self.head = temp.next
                if temp.next:
                    temp.next.prev = temp.prev
                return
            temp = temp.next

    def display_forward(self):
        temp = self.head
        while temp:
            print(temp.data, end=" <-> ")
            temp = temp.next
        print("None")

    def display_backward(self):
        temp = self.head
        if not temp:
            print("None")
            return
        while temp.next:
            temp = temp.next
        while temp:
            print(temp.data, end=" <-> ")
            temp = temp.prev
        print("None")


# Usage
dll = DoublyLinkedList()
dll.insert_at_end(10)
dll.insert_at_end(20)
dll.insert_at_end(30)
dll.insert_at_beginning(5)
dll.display_forward()   # 5 <-> 10 <-> 20 <-> 30 <-> None
dll.display_backward()  # 30 <-> 20 <-> 10 <-> 5 <-> None
dll.delete_node(20)
dll.display_forward()   # 5 <-> 10 <-> 30 <-> None
```

### 3.3 Circular Linked List

```python
class CircularLinkedList:
    def __init__(self):
        self.head = None

    def insert(self, data):
        new_node = Node(data)
        if not self.head:
            self.head = new_node
            new_node.next = self.head
            return
        temp = self.head
        while temp.next != self.head:
            temp = temp.next
        temp.next = new_node
        new_node.next = self.head

    def display(self):
        if not self.head:
            print("Empty")
            return
        temp = self.head
        while True:
            print(temp.data, end=" -> ")
            temp = temp.next
            if temp == self.head:
                break
        print(f"(back to {self.head.data})")

cll = CircularLinkedList()
cll.insert(1)
cll.insert(2)
cll.insert(3)
cll.display()  # 1 -> 2 -> 3 -> (back to 1)
```

### 3.4 Reverse a Linked List

```python
def reverse_list(head):
    prev = None
    current = head
    while current:
        next_node = current.next
        current.next = prev
        prev = current
        current = next_node
    return prev

# Using the LinkedList class
ll = LinkedList()
for x in [1, 2, 3, 4, 5]:
    ll.insert_at_end(x)
ll.display()              # 1 -> 2 -> 3 -> 4 -> 5 -> None
ll.head = reverse_list(ll.head)
ll.display()              # 5 -> 4 -> 3 -> 2 -> 1 -> None
```

### 3.5 Detect Cycle (Floyd's Algorithm)

```python
def has_cycle(head):
    slow = head
    fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False
```

### 3.6 Find Middle Node

```python
def find_middle(head):
    slow = head
    fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow.data

ll = LinkedList()
for x in [1, 2, 3, 4, 5]:
    ll.insert_at_end(x)
print(find_middle(ll.head))  # 3
```

---

## 4. MCQs (15 Questions)

**Q1.** A node in a singly linked list contains:
- A) Only data
- B) Data and a pointer to the next node
- C) Data and pointers to next and previous nodes
- D) Only a pointer

**Answer:** B) Data and a pointer to the next node

---

**Q2.** Time complexity of inserting at the beginning of a singly linked list:
- A) $O(n)$
- B) $O(\log n)$
- C) $O(1)$
- D) $O(n^2)$

**Answer:** C) $O(1)$

---

**Q3.** Time complexity of accessing the $k$-th element in a linked list:
- A) $O(1)$
- B) $O(k)$
- C) $O(n)$
- D) $O(\log n)$

**Answer:** B) $O(k)$ — Must traverse from head. Worst case is $O(n)$.

---

**Q4.** In a doubly linked list, each node has:
- A) One pointer
- B) Two pointers (next and previous)
- C) Three pointers
- D) No pointers

**Answer:** B) Two pointers (next and previous)

---

**Q5.** The last node of a singly linked list has its `next` pointer set to:
- A) The head node
- B) 0
- C) `None` (null)
- D) Itself

**Answer:** C) `None` (null)

---

**Q6.** What is the advantage of a linked list over an array?
- A) Faster access by index
- B) Better cache performance
- C) Dynamic size / efficient insertion at beginning
- D) Less memory usage

**Answer:** C) Dynamic size / efficient insertion at beginning

---

**Q7.** Floyd's cycle detection uses:
- A) One pointer
- B) Two pointers (slow and fast)
- C) A stack
- D) A hash map

**Answer:** B) Two pointers (slow and fast) — Slow moves 1 step, fast moves 2 steps.

---

**Q8.** In a circular linked list, the last node points to:
- A) None
- B) The head node
- C) The second node
- D) Itself

**Answer:** B) The head node

---

**Q9.** Time complexity to delete a node at a given position in a singly linked list:
- A) $O(1)$
- B) $O(\log n)$
- C) $O(n)$
- D) $O(n^2)$

**Answer:** C) $O(n)$ — Need to traverse to find the node before the target.

---

**Q10.** Reversing a singly linked list takes:
- A) $O(1)$ time, $O(n)$ space
- B) $O(n)$ time, $O(1)$ space
- C) $O(n)$ time, $O(n)$ space
- D) $O(n^2)$ time, $O(1)$ space

**Answer:** B) $O(n)$ time, $O(1)$ space — In-place reversal using three pointers.

---

**Q11.** How do you find the middle element of a linked list in one pass?
- A) Count nodes first, then traverse to middle
- B) Use slow and fast pointers
- C) Use a stack
- D) It's not possible in one pass

**Answer:** B) Use slow and fast pointers

---

**Q12.** Which linked list allows traversal in both directions?
- A) Singly linked list
- B) Doubly linked list
- C) Circular singly linked list
- D) None of the above

**Answer:** B) Doubly linked list

---

**Q13.** Space overhead per node in a doubly linked list vs singly linked list:
- A) Same
- B) One extra pointer
- C) Two extra pointers
- D) No overhead

**Answer:** B) One extra pointer (the `prev` pointer)

---

**Q14.** What is the output? (Using the singly linked list code above)
```python
ll = LinkedList()
ll.insert_at_beginning(3)
ll.insert_at_beginning(2)
ll.insert_at_beginning(1)
ll.insert_at_end(4)
ll.display()
```
- A) `1 -> 2 -> 3 -> 4 -> None`
- B) `3 -> 2 -> 1 -> 4 -> None`
- C) `4 -> 3 -> 2 -> 1 -> None`
- D) `1 -> 2 -> 4 -> 3 -> None`

**Answer:** A) `1 -> 2 -> 3 -> 4 -> None`

---

**Q15.** To implement a stack using a linked list, push and pop should operate on:
- A) The tail
- B) The head
- C) The middle
- D) Both head and tail

**Answer:** B) The head — Insert/delete at head is $O(1)$.
