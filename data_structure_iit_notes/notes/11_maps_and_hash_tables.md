# Topic 11: Maps and Hash Tables

## 1. Overview

A **Map** (or **Dictionary**) is an abstract data type that stores **key-value** pairs, supporting efficient lookup, insertion, and deletion by key. The most common implementation is the **Hash Table**, which uses a **hash function** to map keys to array indices.

---

## 2. Key Concepts

### 2.1 Map ADT Operations

| Operation | Description | Hash Table Avg | Hash Table Worst |
|-----------|-------------|---------------|-----------------|
| `get(k)` | Return value for key `k` | $O(1)$ | $O(n)$ |
| `put(k, v)` | Insert/update key-value pair | $O(1)$ | $O(n)$ |
| `delete(k)` | Remove key `k` | $O(1)$ | $O(n)$ |
| `contains(k)` | Check if key exists | $O(1)$ | $O(n)$ |
| `len()` | Number of entries | $O(1)$ | $O(1)$ |

### 2.2 Hash Function

A **hash function** maps a key to an integer (index in the hash table).

**Properties of a good hash function:**
1. **Deterministic** — same key always gives same hash
2. **Uniform distribution** — spreads keys evenly across the table
3. **Efficient** — fast to compute

### 2.3 Hash Function Methods

*(From IIT Patna Lecture)*

#### Division Method
$$h(k) = k \mod m$$
where $m$ is the table size (preferably a prime number).

```python
def hash_division(key, table_size):
    return key % table_size

print(hash_division(25, 10))  # 5
print(hash_division(37, 10))  # 7
```

#### Folding Method
Split the key into equal parts, sum them, then take mod.

```python
def hash_folding(key, table_size):
    key_str = str(key)
    parts = [key_str[i:i+2] for i in range(0, len(key_str), 2)]
    total = sum(int(part) for part in parts)
    return total % table_size

print(hash_folding(456789, 10))  # (45 + 67 + 89) % 10 = 201 % 10 = 1
```

#### Mid-Square Method
Square the key and extract the middle digits.

```python
def hash_mid_square(key, table_size):
    squared = str(key ** 2)
    mid = len(squared) // 2
    mid_digits = squared[mid - 1:mid + 1]
    return int(mid_digits) % table_size

print(hash_mid_square(25, 10))  # 25^2 = 625, mid = "62" → 62 % 10 = 2
```

#### String Hashing
Sum ASCII values and take mod.

*(From IIT Patna Lecture)*

```python
def hash_string(key, table_size):
    total = sum(ord(char) for char in key)
    return total % table_size

print(hash_string("hello", 10))  # (104+101+108+108+111) % 10 = 532 % 10 = 2
```

**Better string hash (polynomial rolling hash):**
```python
def hash_string_poly(key, table_size):
    h = 0
    for i, char in enumerate(key):
        h += ord(char) * (31 ** i)
    return h % table_size
```

### 2.4 Collisions

A **collision** occurs when two different keys hash to the same index.

### 2.5 Collision Resolution

*(From IIT Patna Lecture)*

#### Method 1: Separate Chaining (Open Hashing)

Each index stores a **linked list** (or list) of all entries that hash to that index.

```
Index 0: → (10, "A") → (20, "B") → None
Index 1: → (11, "C") → None
Index 2: → empty
...
```

#### Method 2: Open Addressing (Closed Hashing)

All entries are stored in the hash table itself. When a collision occurs, probe for the next available slot.

**Linear Probing:** $h(k, i) = (h(k) + i) \mod m$
- Try index $h(k)$, then $h(k)+1$, then $h(k)+2$, ...
- Problem: **Primary clustering** — long chains of occupied slots

**Quadratic Probing:** $h(k, i) = (h(k) + i^2) \mod m$
- Try $h(k)$, $h(k)+1$, $h(k)+4$, $h(k)+9$, ...
- Reduces primary clustering but can cause **secondary clustering**

**Double Hashing:** $h(k, i) = (h_1(k) + i \cdot h_2(k)) \mod m$
- Uses a second hash function for the step size
- Best distribution among open addressing methods

### 2.6 Load Factor

$$\alpha = \frac{n}{m}$$

where $n$ = number of entries, $m$ = table size.

- $\alpha < 0.75$ is generally recommended for good performance
- When $\alpha$ gets too high, **resize** the table (typically double)

---

## 3. Code Examples

### 3.1 Hash Table with Separate Chaining

```python
class HashTable:
    def __init__(self, size=10):
        self.size = size
        self.table = [[] for _ in range(size)]

    def _hash(self, key):
        if isinstance(key, str):
            return sum(ord(c) for c in key) % self.size
        return key % self.size

    def put(self, key, value):
        index = self._hash(key)
        # Update if key exists
        for i, (k, v) in enumerate(self.table[index]):
            if k == key:
                self.table[index][i] = (key, value)
                return
        # Otherwise append
        self.table[index].append((key, value))

    def get(self, key):
        index = self._hash(key)
        for k, v in self.table[index]:
            if k == key:
                return v
        raise KeyError(key)

    def delete(self, key):
        index = self._hash(key)
        for i, (k, v) in enumerate(self.table[index]):
            if k == key:
                del self.table[index][i]
                return
        raise KeyError(key)

    def __str__(self):
        result = []
        for i, bucket in enumerate(self.table):
            if bucket:
                result.append(f"  {i}: {bucket}")
        return "HashTable:\n" + "\n".join(result)


# Usage
ht = HashTable(10)
ht.put("name", "Alice")
ht.put("age", 20)
ht.put("roll", 101)
print(ht.get("name"))   # Alice
print(ht.get("age"))    # 20
ht.put("name", "Bob")   # Update
print(ht.get("name"))   # Bob
ht.delete("age")
print(ht)
```

### 3.2 Hash Table with Linear Probing

```python
class HashTableLP:
    def __init__(self, size=10):
        self.size = size
        self.keys = [None] * size
        self.values = [None] * size

    def _hash(self, key):
        return key % self.size

    def put(self, key, value):
        index = self._hash(key)
        while self.keys[index] is not None:
            if self.keys[index] == key:
                self.values[index] = value  # Update
                return
            index = (index + 1) % self.size  # Linear probe
        self.keys[index] = key
        self.values[index] = value

    def get(self, key):
        index = self._hash(key)
        start = index
        while self.keys[index] is not None:
            if self.keys[index] == key:
                return self.values[index]
            index = (index + 1) % self.size
            if index == start:
                break
        raise KeyError(key)

    def display(self):
        for i in range(self.size):
            print(f"  [{i}]: {self.keys[i]} → {self.values[i]}")


# Usage
ht = HashTableLP(7)
ht.put(10, "A")   # 10 % 7 = 3
ht.put(17, "B")   # 17 % 7 = 3 → collision → probe to 4
ht.put(24, "C")   # 24 % 7 = 3 → collision → probe to 5
ht.display()
print(ht.get(17))  # B
```

### 3.3 Python dict (Built-in Hash Map)

```python
# Python dict uses open addressing with a sophisticated probing strategy

d = {}
d["name"] = "Alice"
d["age"] = 20
d["marks"] = [85, 90, 78]

print(d["name"])      # Alice
print("age" in d)     # True

# Iterate
for key, value in d.items():
    print(f"{key}: {value}")

# Dictionary comprehension
squares = {x: x**2 for x in range(1, 6)}
print(squares)  # {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
```

---

## 4. MCQs (15 Questions)

**Q1.** A hash table provides average-case time complexity of:
- A) $O(n)$ for search
- B) $O(\log n)$ for search
- C) $O(1)$ for search, insert, delete
- D) $O(n \log n)$ for insert

**Answer:** C) $O(1)$ for search, insert, delete

---

**Q2.** A collision in a hash table occurs when:
- A) Two values are equal
- B) Two different keys produce the same hash index
- C) The table is full
- D) A key is None

**Answer:** B) Two different keys produce the same hash index

---

**Q3.** In the division method, $h(k) = k \mod m$, what should $m$ ideally be?
- A) A power of 2
- B) An even number
- C) A prime number
- D) Any number

**Answer:** C) A prime number — Reduces clustering and distributes keys more uniformly.

---

**Q4.** Which collision resolution stores multiple entries at the same index using a list?
- A) Linear probing
- B) Quadratic probing
- C) Separate chaining
- D) Double hashing

**Answer:** C) Separate chaining

---

**Q5.** Linear probing suffers from:
- A) Secondary clustering
- B) Primary clustering
- C) No clustering
- D) Hash overflow

**Answer:** B) Primary clustering — Consecutive occupied slots form long chains.

---

**Q6.** The load factor $\alpha$ of a hash table is:
- A) $m / n$
- B) $n / m$ (number of entries / table size)
- C) $n \times m$
- D) $\log(n/m)$

**Answer:** B) $n / m$

---

**Q7.** When the load factor gets too high, the hash table should:
- A) Delete old entries
- B) Resize (typically double) and rehash all entries
- C) Switch to linear search
- D) Compress the data

**Answer:** B) Resize and rehash all entries

---

**Q8.** The hash function `sum(ord(c) for c in key) % m` has what problem?
- A) It's too slow
- B) Anagrams produce the same hash value
- C) It only works for numbers
- D) It always produces 0

**Answer:** B) Anagrams produce the same hash value — e.g., "abc" and "cba" hash the same.

---

**Q9.** In double hashing, $h(k, i) = (h_1(k) + i \cdot h_2(k)) \mod m$. What must be true about $h_2(k)$?
- A) $h_2(k)$ must be 0
- B) $h_2(k)$ must never be 0
- C) $h_2(k)$ must equal $h_1(k)$
- D) $h_2(k)$ must be negative

**Answer:** B) $h_2(k)$ must never be 0 — Otherwise the probe doesn't advance.

---

**Q10.** Python's built-in `dict` uses:
- A) Separate chaining
- B) Open addressing
- C) Binary search tree
- D) Linked list

**Answer:** B) Open addressing (with a custom probing strategy)

---

**Q11.** Worst-case time complexity of hash table operations:
- A) $O(1)$
- B) $O(\log n)$
- C) $O(n)$
- D) $O(n^2)$

**Answer:** C) $O(n)$ — When all keys hash to the same index.

---

**Q12.** Which hash method squares the key and extracts middle digits?
- A) Division method
- B) Folding method
- C) Mid-square method
- D) Multiplication method

**Answer:** C) Mid-square method

---

**Q13.** The folding method for key 456789 with 2-digit parts gives:
- A) 45 + 67 + 89
- B) 4 + 5 + 6 + 7 + 8 + 9
- C) 456 + 789
- D) 45 × 67 × 89

**Answer:** A) 45 + 67 + 89 = 201

---

**Q14.** In quadratic probing, the probe sequence is:
- A) $h(k)$, $h(k)+1$, $h(k)+2$, ...
- B) $h(k)$, $h(k)+1$, $h(k)+4$, $h(k)+9$, ...
- C) $h(k)$, $h(k) \times 2$, $h(k) \times 3$, ...
- D) Random positions

**Answer:** B) $h(k)$, $h(k)+1$, $h(k)+4$, $h(k)+9$, ...

---

**Q15.** Which of the following is NOT a valid collision resolution technique?
- A) Separate chaining
- B) Linear probing
- C) Binary probing
- D) Double hashing

**Answer:** C) Binary probing — Not a standard collision resolution technique.
