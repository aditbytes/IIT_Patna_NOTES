# Topic 14: Text Processing

## 1. Overview

Text processing involves algorithms for **pattern matching**, **string manipulation**, and **text analysis**. Key topics include brute-force matching, the KMP algorithm, Boyer-Moore, tries, and text compression.

---

## 2. Key Concepts

### 2.1 Pattern Matching Problem

Given a text $T$ of length $n$ and a pattern $P$ of length $m$, find all occurrences of $P$ in $T$.

### 2.2 Pattern Matching Algorithms

| Algorithm | Preprocessing | Matching | Worst Case |
|-----------|--------------|----------|------------|
| Brute Force | None | $O(nm)$ | $O(nm)$ |
| KMP | $O(m)$ | $O(n)$ | $O(n + m)$ |
| Boyer-Moore | $O(m + |\Sigma|)$ | $O(n/m)$ avg | $O(nm)$ |
| Rabin-Karp | $O(m)$ | $O(n)$ avg | $O(nm)$ |

### 2.3 Tries

A **Trie** (prefix tree) is a tree structure for storing strings where each edge represents a character. Used for autocomplete, spell-checking, and IP routing.

---

## 3. Code Examples

### 3.1 Brute Force Pattern Matching

```python
def brute_force_search(text, pattern):
    n = len(text)
    m = len(pattern)
    positions = []
    for i in range(n - m + 1):
        match = True
        for j in range(m):
            if text[i + j] != pattern[j]:
                match = False
                break
        if match:
            positions.append(i)
    return positions

text = "AABAACAADAABAABA"
pattern = "AABA"
print(brute_force_search(text, pattern))  # [0, 9, 12]
```

### 3.2 KMP (Knuth-Morris-Pratt) Algorithm

**Key Idea:** Precompute a **failure function** (prefix table) to avoid re-examining characters.

```python
def compute_lps(pattern):
    """Compute Longest Proper Prefix which is also Suffix."""
    m = len(pattern)
    lps = [0] * m
    length = 0
    i = 1
    while i < m:
        if pattern[i] == pattern[length]:
            length += 1
            lps[i] = length
            i += 1
        else:
            if length != 0:
                length = lps[length - 1]
            else:
                lps[i] = 0
                i += 1
    return lps

def kmp_search(text, pattern):
    n = len(text)
    m = len(pattern)
    lps = compute_lps(pattern)
    positions = []

    i = 0  # index in text
    j = 0  # index in pattern
    while i < n:
        if text[i] == pattern[j]:
            i += 1
            j += 1
        if j == m:
            positions.append(i - j)
            j = lps[j - 1]
        elif i < n and text[i] != pattern[j]:
            if j != 0:
                j = lps[j - 1]
            else:
                i += 1
    return positions

text = "AABAACAADAABAABA"
pattern = "AABA"
print(kmp_search(text, pattern))  # [0, 9, 12]
```

### 3.3 Rabin-Karp Algorithm

Uses **rolling hash** to compare pattern hash with text window hash.

```python
def rabin_karp(text, pattern, d=256, q=101):
    n = len(text)
    m = len(pattern)
    h = pow(d, m - 1) % q  # d^(m-1) mod q
    p_hash = 0  # hash of pattern
    t_hash = 0  # hash of text window
    positions = []

    # Compute initial hashes
    for i in range(m):
        p_hash = (d * p_hash + ord(pattern[i])) % q
        t_hash = (d * t_hash + ord(text[i])) % q

    for i in range(n - m + 1):
        if p_hash == t_hash:
            # Verify character by character
            if text[i:i + m] == pattern:
                positions.append(i)
        if i < n - m:
            t_hash = (d * (t_hash - ord(text[i]) * h) + ord(text[i + m])) % q
            if t_hash < 0:
                t_hash += q
    return positions

print(rabin_karp("AABAACAADAABAABA", "AABA"))  # [0, 9, 12]
```

### 3.4 Trie Implementation

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end = True

    def search(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end

    def starts_with(self, prefix):
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True

    def get_words_with_prefix(self, prefix):
        node = self.root
        for char in prefix:
            if char not in node.children:
                return []
            node = node.children[char]
        words = []
        self._dfs(node, prefix, words)
        return words

    def _dfs(self, node, prefix, words):
        if node.is_end:
            words.append(prefix)
        for char, child in node.children.items():
            self._dfs(child, prefix + char, words)


# Usage
trie = Trie()
for word in ["apple", "app", "application", "bat", "ball"]:
    trie.insert(word)

print(trie.search("app"))        # True
print(trie.search("ap"))         # False
print(trie.starts_with("app"))   # True
print(trie.get_words_with_prefix("app"))  # ['apple', 'app', 'application']
```

### 3.5 Python String Methods for Text Processing

```python
text = "Hello World, Hello Python"

# Find
print(text.find("Hello"))     # 0
print(text.find("Hello", 1))  # 13 (start from index 1)
print(text.count("Hello"))    # 2

# Replace
print(text.replace("Hello", "Hi"))  # "Hi World, Hi Python"

# Split & Join
words = text.split()
print(words)                     # ['Hello', 'World,', 'Hello', 'Python']
print("-".join(words))           # "Hello-World,-Hello-Python"

# Strip
s = "  hello  "
print(s.strip())   # "hello"
print(s.lstrip())  # "hello  "
print(s.rstrip())  # "  hello"

# Check
print("hello".isalpha())    # True
print("123".isdigit())      # True
print("hello123".isalnum()) # True
```

---

## 4. MCQs (12 Questions)

**Q1.** Brute force pattern matching has worst-case complexity:
- A) $O(n)$
- B) $O(m)$
- C) $O(nm)$
- D) $O(n + m)$

**Answer:** C) $O(nm)$

---

**Q2.** KMP algorithm's total time complexity is:
- A) $O(n^2)$
- B) $O(nm)$
- C) $O(n + m)$
- D) $O(n \log m)$

**Answer:** C) $O(n + m)$ — $O(m)$ for preprocessing + $O(n)$ for matching.

---

**Q3.** The failure function (LPS array) in KMP is computed for:
- A) The text
- B) The pattern
- C) Both text and pattern
- D) Neither

**Answer:** B) The pattern

---

**Q4.** Rabin-Karp uses which technique to speed up comparison?
- A) Binary search
- B) Rolling hash
- C) Divide and conquer
- D) Dynamic programming

**Answer:** B) Rolling hash

---

**Q5.** A Trie's search time for a word of length $L$ is:
- A) $O(1)$
- B) $O(L)$
- C) $O(n)$
- D) $O(n \log n)$

**Answer:** B) $O(L)$ — Proportional to the word length.

---

**Q6.** The LPS array for pattern "AABAACAABAA" at the last position is:
- A) 0
- B) 1
- C) 3
- D) 5

**Answer:** D) 5 — "AABAA" is a proper prefix that is also a suffix.

---

**Q7.** Which data structure is best for implementing autocomplete?
- A) Hash table
- B) Binary search tree
- C) Trie
- D) Stack

**Answer:** C) Trie

---

**Q8.** `"hello".find("xyz")` returns:
- A) `None`
- B) `False`
- C) `-1`
- D) `0`

**Answer:** C) `-1` — `find()` returns -1 when substring is not found.

---

**Q9.** Boyer-Moore's best-case pattern matching is:
- A) $O(n)$
- B) $O(n/m)$
- C) $O(nm)$
- D) $O(m)$

**Answer:** B) $O(n/m)$ — Can skip large portions of text.

---

**Q10.** In a trie with $n$ words of average length $L$, the space complexity is:
- A) $O(n)$
- B) $O(nL)$ in the worst case
- C) $O(L)$
- D) $O(n^2)$

**Answer:** B) $O(nL)$ in the worst case

---

**Q11.** `"abcabc".count("abc")` returns:
- A) 1
- B) 2
- C) 3
- D) 0

**Answer:** B) 2

---

**Q12.** The Rabin-Karp algorithm can have worst case $O(nm)$ because:
- A) Hash computation is slow
- B) Many hash collisions (spurious hits) may require character-by-character verification
- C) The text is too long
- D) It doesn't use a hash function

**Answer:** B) Many hash collisions may require character-by-character verification
