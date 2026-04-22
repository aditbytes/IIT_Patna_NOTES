# Prerequisites for Mathematics-II (CDA-102)

> **Purpose:** This file covers ALL the foundational concepts you MUST know before studying the lecture notes (1.1 → 3.3). If you're a complete beginner or feel lost in any lecture, come back here first.

---

# Table of Contents

1. [Number Systems](#1-number-systems)
2. [Basic Algebra You Can't Escape](#2-basic-algebra-you-cant-escape)
3. [Sets and Set Operations](#3-sets-and-set-operations)
4. [Functions and Mappings](#4-functions-and-mappings)
5. [Summation and Product Notation](#5-summation-and-product-notation)
6. [Complex Numbers](#6-complex-numbers)
7. [Coordinate Geometry Basics](#7-coordinate-geometry-basics)
8. [Determinants (from Class 12)](#8-determinants-from-class-12)
9. [Matrix Basics (from Class 12)](#9-matrix-basics-from-class-12)
10. [Systems of Equations (School Level)](#10-systems-of-equations-school-level)
11. [Inequalities and Absolute Values](#11-inequalities-and-absolute-values)
12. [Basic Proof Techniques](#12-basic-proof-techniques)
13. [Divisibility and GCD](#13-divisibility-and-gcd)
14. [Factorial, Permutation, Combination (Class 11)](#14-factorial-permutation-combination-class-11)
15. [Floor, Ceiling, and Modulus Functions](#15-floor-ceiling-and-modulus-functions)
16. [Greek Letters Used in This Course](#16-greek-letters-used-in-this-course)
17. [Prerequisite Map: What You Need for Each Topic](#17-prerequisite-map-what-you-need-for-each-topic)

---

# 1. Number Systems

Before anything else, you must know the different types of numbers.

## The Hierarchy

| Symbol | Name | What It Contains | Examples |
|--------|------|-----------------|----------|
| $\mathbb{N}$ | Natural numbers | Counting numbers | $1, 2, 3, 4, \ldots$ |
| $\mathbb{Z}$ | Integers | Naturals + zero + negatives | $\ldots, -2, -1, 0, 1, 2, \ldots$ |
| $\mathbb{Q}$ | Rationals | Fractions $p/q$ where $q \neq 0$ | $1/2, -3/4, 5, 0.333\ldots$ |
| $\mathbb{R}$ | Real numbers | Rationals + irrationals | $\pi, \sqrt{2}, e, -7, 0.5$ |
| $\mathbb{C}$ | Complex numbers | $a + bi$ where $i = \sqrt{-1}$ | $3 + 2i, -i, 5$ |

$$
\mathbb{N} \subset \mathbb{Z} \subset \mathbb{Q} \subset \mathbb{R} \subset \mathbb{C}
$$

### Why This Matters
- **Linear Algebra** works over $\mathbb{R}$ and $\mathbb{C}$ (real and complex matrices)
- **LPP** works over $\mathbb{R}$ (real numbers only)
- **Discrete Math** works mostly over $\mathbb{Z}$ and $\mathbb{N}$ (integers)

### Irrational Numbers
Numbers that CANNOT be written as $p/q$: $\sqrt{2}, \pi, e, \sqrt{3}$

They have non-terminating, non-repeating decimal expansions.

---

# 2. Basic Algebra You Can't Escape

These operations appear on literally every page of the notes.

## Exponent Rules

| Rule | Formula | Example |
|------|---------|---------|
| Product | $a^m \cdot a^n = a^{m+n}$ | $2^3 \cdot 2^4 = 2^7 = 128$ |
| Quotient | $a^m / a^n = a^{m-n}$ | $5^6 / 5^2 = 5^4$ |
| Power of power | $(a^m)^n = a^{mn}$ | $(3^2)^3 = 3^6 = 729$ |
| Zero exponent | $a^0 = 1$ (if $a \neq 0$) | $7^0 = 1$ |
| Negative exponent | $a^{-n} = 1/a^n$ | $2^{-3} = 1/8$ |

## Fraction Arithmetic

$$
\frac{a}{b} + \frac{c}{d} = \frac{ad + bc}{bd}
$$

$$
\frac{a}{b} \times \frac{c}{d} = \frac{ac}{bd}
$$

$$
\frac{a}{b} \div \frac{c}{d} = \frac{a}{b} \times \frac{d}{c} = \frac{ad}{bc}
$$

**You WILL do a LOT of fraction arithmetic** in Simplex Method tableaux (Topic 2.3). Practice until it's automatic.

## Solving Linear Equations

$$
ax + b = c \implies x = \frac{c - b}{a}
$$

Two equations, two unknowns (substitution or elimination):

$$
\begin{cases} 2x + 3y = 12 \\ x - y = 1 \end{cases}
$$

From equation 2: $x = y + 1$. Substitute into equation 1:

$2(y+1) + 3y = 12 \implies 5y = 10 \implies y = 2, \; x = 3$

## Quadratic Formula

$$
ax^2 + bx + c = 0 \implies x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}
$$

**Where you'll need this:** Finding eigenvalues (Topic 1.6). The characteristic equation is often quadratic or cubic.

### Discriminant

$\Delta = b^2 - 4ac$

| $\Delta$ | Roots |
|----------|-------|
| $> 0$ | Two distinct real roots |
| $= 0$ | One repeated real root |
| $< 0$ | Two complex conjugate roots |

## Polynomial Long Division / Factor Theorem

If $P(a) = 0$, then $(x - a)$ is a factor of $P(x)$.

**Example:** $P(x) = x^3 - 6x^2 + 11x - 6$

Try $x = 1$: $1 - 6 + 11 - 6 = 0$ ✓ → $(x-1)$ is a factor.

Divide: $x^3 - 6x^2 + 11x - 6 = (x-1)(x^2 - 5x + 6) = (x-1)(x-2)(x-3)$

**Where you'll need this:** Factoring characteristic polynomials to find eigenvalues (Topic 1.6).

---

# 3. Sets and Set Operations

Sets are the foundation of almost everything in this course.

## What is a Set?

A **set** is a well-defined collection of distinct objects. We write elements inside curly braces.

$$
A = \{1, 2, 3, 4\}, \quad B = \{x \in \mathbb{R} : x > 0\}
$$

## Membership

$3 \in A$ means "3 is in set A". $5 \notin A$ means "5 is not in A".

## Important Sets

| Notation | Meaning |
|----------|---------|
| $\emptyset$ or $\{\}$ | Empty set (no elements) |
| $\{1, 2, 3\}$ | Listing elements |
| $\{x : P(x)\}$ | Set-builder notation (all $x$ satisfying property $P$) |

## Set Operations

| Operation | Symbol | Meaning | Example |
|-----------|--------|---------|---------|
| **Union** | $A \cup B$ | Elements in A OR B (or both) | $\{1,2\} \cup \{2,3\} = \{1,2,3\}$ |
| **Intersection** | $A \cap B$ | Elements in BOTH A and B | $\{1,2\} \cap \{2,3\} = \{2\}$ |
| **Difference** | $A \setminus B$ or $A - B$ | Elements in A but NOT in B | $\{1,2,3\} - \{2\} = \{1,3\}$ |
| **Complement** | $A^c$ or $\bar{A}$ | Everything NOT in A | If $U = \{1,2,3,4\}$, $A=\{1,2\}$, then $A^c = \{3,4\}$ |
| **Subset** | $A \subseteq B$ | Every element of A is in B | $\{1,2\} \subseteq \{1,2,3\}$ |
| **Proper Subset** | $A \subset B$ | $A \subseteq B$ and $A \neq B$ | |
| **Cardinality** | $|A|$ | Number of elements in A | $|\{1,2,3\}| = 3$ |

## Cartesian Product

$$
A \times B = \{(a, b) : a \in A, b \in B\}
$$

**Example:** $\{1, 2\} \times \{a, b\} = \{(1,a), (1,b), (2,a), (2,b)\}$

**Where you'll need this:** Defining functions, relations, state transition functions in FSMs.

---

# 4. Functions and Mappings

## What is a Function?

A function $f: A \to B$ assigns to **each** element of $A$ **exactly one** element of $B$.

- $A$ = **domain** (inputs)
- $B$ = **codomain** (possible outputs)
- $f(A)$ = **range/image** (actual outputs)

## Types of Functions

| Type | Definition | Layman's Version |
|------|-----------|-----------------|
| **One-to-one (Injective)** | $f(a) = f(b) \implies a = b$ | Different inputs → different outputs |
| **Onto (Surjective)** | For every $b \in B$, there exists $a \in A$ with $f(a) = b$ | Every output is "hit" |
| **Bijective** | Both one-to-one and onto | Perfect pairing between A and B |

**Where you'll need this:** Linear transformations (Topic 1.5) ARE functions from one vector space to another. Understanding injective/surjective is crucial.

## Composition of Functions

$(g \circ f)(x) = g(f(x))$

Apply $f$ first, then $g$.

## Inverse Function

If $f$ is bijective, then $f^{-1}$ exists such that $f^{-1}(f(x)) = x$.

---

# 5. Summation and Product Notation

## Sigma Notation (Summation)

$$
\sum_{i=1}^{n} a_i = a_1 + a_2 + a_3 + \cdots + a_n
$$

**Examples:**

$$
\sum_{i=1}^{4} i = 1 + 2 + 3 + 4 = 10
$$

$$
\sum_{i=1}^{3} i^2 = 1 + 4 + 9 = 14
$$

## Useful Summation Formulas

| Sum | Formula |
|-----|---------|
| $\sum_{i=1}^{n} i$ | $\frac{n(n+1)}{2}$ |
| $\sum_{i=1}^{n} i^2$ | $\frac{n(n+1)(2n+1)}{6}$ |
| $\sum_{i=0}^{n} r^i$ (geometric) | $\frac{r^{n+1} - 1}{r - 1}$ for $r \neq 1$ |

## Pi Notation (Product)

$$
\prod_{i=1}^{n} a_i = a_1 \times a_2 \times \cdots \times a_n
$$

**Example:** $\prod_{i=1}^{4} i = 1 \times 2 \times 3 \times 4 = 24 = 4!$

## Double Summation

$$
\sum_{i=1}^{m} \sum_{j=1}^{n} a_{ij}
$$

This means: for each $i$, sum over all $j$. Think of summing all entries of an $m \times n$ table.

**Where you'll need this:** Matrix operations, trace, characteristic polynomials — they all use summation notation.

---

# 6. Complex Numbers

## Definition

$$
i = \sqrt{-1}, \quad i^2 = -1
$$

A complex number: $z = a + bi$ where $a, b \in \mathbb{R}$.

- $a$ = **real part** = $\text{Re}(z)$
- $b$ = **imaginary part** = $\text{Im}(z)$

## Arithmetic

| Operation | Formula |
|-----------|---------|
| Addition | $(a+bi) + (c+di) = (a+c) + (b+d)i$ |
| Subtraction | $(a+bi) - (c+di) = (a-c) + (b-d)i$ |
| Multiplication | $(a+bi)(c+di) = (ac-bd) + (ad+bc)i$ |
| Division | $\frac{a+bi}{c+di} = \frac{(a+bi)(c-di)}{c^2+d^2}$ |

## Conjugate

$$
\bar{z} = a - bi
$$

If $z = 3 + 2i$, then $\bar{z} = 3 - 2i$.

**Key property:** $z \cdot \bar{z} = a^2 + b^2 = |z|^2$

## Modulus (Magnitude)

$$
|z| = \sqrt{a^2 + b^2}
$$

## Powers of $i$

$$
i^1 = i, \quad i^2 = -1, \quad i^3 = -i, \quad i^4 = 1, \quad i^5 = i, \ldots
$$

Pattern repeats every 4.

**Where you'll need this:**
- **Topic 1.1:** Conjugate transpose of matrices ($A^* = \bar{A}^T$)
- **Topic 1.6:** Eigenvalues can be complex numbers
- **Topic 1.7:** Hermitian and Unitary matrices involve complex entries

---

# 7. Coordinate Geometry Basics

## The Cartesian Plane

Every point is represented as $(x, y)$.

- $x$-axis = horizontal, $y$-axis = vertical
- **Origin** = $(0, 0)$

## Equation of a Line

### Slope-Intercept Form
$$
y = mx + c
$$
$m$ = slope, $c$ = y-intercept

### General Form
$$
ax + by = c
$$

### Plotting a Line (2 points are enough)

To plot $2x + 3y = 12$:
- Set $x = 0$: $3y = 12 \implies y = 4$ → Point $(0, 4)$
- Set $y = 0$: $2x = 12 \implies x = 6$ → Point $(6, 0)$

Connect the two points.

## Finding Intersection of Two Lines

Solve the two equations simultaneously.

**Example:** $x + y = 5$ and $2x - y = 1$

Add: $3x = 6 \implies x = 2, y = 3$. Intersection: $(2, 3)$.

## Distance Between Two Points

$$
d = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}
$$

## Regions (Inequalities on the Plane)

$2x + 3y \leq 12$ represents a **half-plane** — all the area on one side of the line $2x + 3y = 12$.

To decide which side: test the origin $(0, 0)$. If $2(0) + 3(0) = 0 \leq 12$ ✓, then the origin side is the solution region.

**Where you'll need this:** Graphical method for LPP (Topic 2.2) is entirely about plotting lines, finding intersections, and identifying feasible regions.

---

# 8. Determinants (from Class 12)

## 2×2 Determinant

$$
\det \begin{pmatrix} a & b \\ c & d \end{pmatrix} = ad - bc
$$

**Example:**
$$
\det \begin{pmatrix} 3 & 1 \\ 2 & 4 \end{pmatrix} = 3(4) - 1(2) = 10
$$

## 3×3 Determinant (Cofactor Expansion along Row 1)

$$
\det \begin{pmatrix} a & b & c \\ d & e & f \\ g & h & i \end{pmatrix} = a(ei - fh) - b(di - fg) + c(dh - eg)
$$

**Example:**
$$
\det \begin{pmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \\ 7 & 8 & 9 \end{pmatrix} = 1(45 - 48) - 2(36 - 42) + 3(32 - 35)
$$
$$
= 1(-3) - 2(-6) + 3(-3) = -3 + 12 - 9 = 0
$$

## Properties of Determinants

| Property | Statement |
|----------|-----------|
| Transpose | $\det(A^T) = \det(A)$ |
| Row swap | Swapping two rows changes the sign |
| Scalar multiple | $\det(kA) = k^n \det(A)$ for $n \times n$ matrix |
| Product | $\det(AB) = \det(A) \cdot \det(B)$ |
| Zero row | If a row is all zeros, $\det = 0$ |
| Identical rows | If two rows are identical, $\det = 0$ |
| Triangular matrix | $\det =$ product of diagonal entries |
| Singular matrix | $\det(A) = 0 \iff A$ is not invertible |

**Where you'll need this:** Topics 1.1, 1.2, 1.3, and especially 1.6 (characteristic equation uses $\det(A - \lambda I) = 0$).

---

# 9. Matrix Basics (from Class 12)

## What is a Matrix?

A rectangular array of numbers arranged in rows and columns.

$$
A = \begin{pmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{pmatrix}
$$

This is a $2 \times 3$ matrix (2 rows, 3 columns).

The element in row $i$, column $j$ is denoted $a_{ij}$.

Here: $a_{12} = 2$ (row 1, column 2).

## Matrix Dimensions

An $m \times n$ matrix has $m$ rows and $n$ columns.

## Matrix Addition

Add corresponding entries. Both matrices MUST have the same dimensions.

$$
\begin{pmatrix} 1 & 2 \\ 3 & 4 \end{pmatrix} + \begin{pmatrix} 5 & 6 \\ 7 & 8 \end{pmatrix} = \begin{pmatrix} 6 & 8 \\ 10 & 12 \end{pmatrix}
$$

## Scalar Multiplication

Multiply every entry by the scalar.

$$
3 \begin{pmatrix} 1 & 2 \\ 3 & 4 \end{pmatrix} = \begin{pmatrix} 3 & 6 \\ 9 & 12 \end{pmatrix}
$$

## Matrix Multiplication

For $A$ ($m \times n$) and $B$ ($n \times p$), the product $AB$ is $m \times p$.

$$
(AB)_{ij} = \sum_{k=1}^{n} a_{ik} b_{kj}
$$

**In words:** Row $i$ of $A$ **dot** Column $j$ of $B$.

### Step-by-Step Example

$$
\begin{pmatrix} 1 & 2 \\ 3 & 4 \end{pmatrix} \begin{pmatrix} 5 & 6 \\ 7 & 8 \end{pmatrix}
$$

Position $(1,1)$: $1 \times 5 + 2 \times 7 = 5 + 14 = 19$

Position $(1,2)$: $1 \times 6 + 2 \times 8 = 6 + 16 = 22$

Position $(2,1)$: $3 \times 5 + 4 \times 7 = 15 + 28 = 43$

Position $(2,2)$: $3 \times 6 + 4 \times 8 = 18 + 32 = 50$

$$
AB = \begin{pmatrix} 19 & 22 \\ 43 & 50 \end{pmatrix}
$$

### ⚠️ Matrix Multiplication is NOT Commutative

$AB \neq BA$ in general! This is one of the most common mistakes.

## Transpose

Flip rows and columns: $(A^T)_{ij} = A_{ji}$

$$
A = \begin{pmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{pmatrix} \implies A^T = \begin{pmatrix} 1 & 4 \\ 2 & 5 \\ 3 & 6 \end{pmatrix}
$$

## Identity Matrix

$$
I_n = \begin{pmatrix} 1 & 0 & \cdots & 0 \\ 0 & 1 & \cdots & 0 \\ \vdots & & \ddots & \vdots \\ 0 & 0 & \cdots & 1 \end{pmatrix}
$$

$AI = IA = A$ for any compatible matrix $A$.

## Zero Matrix

All entries are 0. Denoted $O$ or $\mathbf{0}$.

**Where you'll need this:** ALL of Module 1 (Topics 1.1 through 1.7). You will be doing matrix arithmetic constantly.

---

# 10. Systems of Equations (School Level)

## What is a System?

Multiple equations that must ALL be satisfied simultaneously.

$$
\begin{cases}
2x + y = 5 \\
x - y = 1
\end{cases}
$$

## Methods of Solving

### Method 1: Substitution
Solve one equation for one variable, substitute into the other.

From equation 2: $x = y + 1$

Into equation 1: $2(y+1) + y = 5 \implies 3y = 3 \implies y = 1, x = 2$

### Method 2: Elimination
Add/subtract equations to eliminate a variable.

Add both equations: $3x = 6 \implies x = 2, y = 1$

### Method 3: Matrix Form

$$
\begin{pmatrix} 2 & 1 \\ 1 & -1 \end{pmatrix} \begin{pmatrix} x \\ y \end{pmatrix} = \begin{pmatrix} 5 \\ 1 \end{pmatrix}
$$

This is $AX = B$. This is exactly what Topic 1.3 is about!

## Three Possibilities

| Case | Geometric Meaning | Algebraic Condition |
|------|--------------------|---------------------|
| **Unique solution** | Lines intersect at one point | $\det(A) \neq 0$ |
| **No solution** | Lines are parallel | Inconsistent system |
| **Infinite solutions** | Lines overlap | Dependent system |

---

# 11. Inequalities and Absolute Values

## Inequality Rules

| Rule | Statement |
|------|-----------|
| Add/subtract | If $a > b$, then $a + c > b + c$ |
| Multiply by positive | If $a > b$ and $c > 0$, then $ac > bc$ |
| Multiply by negative | If $a > b$ and $c < 0$, then $ac < bc$ (**flip the sign!**) |

## Absolute Value

$$
|x| = \begin{cases} x & \text{if } x \geq 0 \\ -x & \text{if } x < 0 \end{cases}
$$

$|x|$ = distance from $x$ to 0 on the number line.

**Triangle inequality:** $|a + b| \leq |a| + |b|$

## Linear Inequalities in Two Variables

$2x + 3y \leq 12$ represents all points $(x, y)$ on or below the line $2x + 3y = 12$.

**Where you'll need this:** Every constraint in an LPP (Topics 2.1–2.4) is an inequality. The Graphical Method (Topic 2.2) is all about plotting and interpreting these.

---

# 12. Basic Proof Techniques

You'll encounter proofs throughout the course. Here are the methods used.

## Direct Proof

Assume the hypothesis, derive the conclusion step by step.

**Example:** Prove that the sum of two even numbers is even.

Let $a = 2m$, $b = 2n$. Then $a + b = 2(m+n)$, which is even. ✓

## Proof by Contradiction

Assume the opposite of what you want to prove, show it leads to a contradiction.

**Example:** Prove $\sqrt{2}$ is irrational.

Assume $\sqrt{2} = p/q$ (reduced form). Then $2q^2 = p^2$, so $p$ is even, say $p = 2k$. Then $2q^2 = 4k^2$, so $q^2 = 2k^2$, meaning $q$ is also even. But we said $p/q$ was reduced. Contradiction! ✓

## Proof by Induction

To prove a statement $P(n)$ for all $n \geq 1$:
1. **Base case:** Prove $P(1)$ is true.
2. **Inductive step:** Assume $P(k)$ is true (inductive hypothesis), prove $P(k+1)$.

**Example:** Prove $\sum_{i=1}^n i = \frac{n(n+1)}{2}$.

Base: $n=1$: $1 = 1(2)/2 = 1$ ✓

Inductive step: Assume true for $k$. Then:

$$
\sum_{i=1}^{k+1} i = \sum_{i=1}^k i + (k+1) = \frac{k(k+1)}{2} + (k+1) = \frac{(k+1)(k+2)}{2} \quad ✓
$$

## Counterexample

To disprove a statement, find ONE example where it fails.

**Example:** "All prime numbers are odd." Counterexample: $2$ is prime and even.

**Where you'll need this:** Vector space proofs (Topic 1.4), linear transformation properties (Topic 1.5), showing convexity (Topic 2.1), pigeonhole principle arguments (Topic 3.3).

---

# 13. Divisibility and GCD

## Divisibility

$a$ **divides** $b$ (written $a \mid b$) means $b = ka$ for some integer $k$.

**Example:** $3 \mid 12$ because $12 = 4 \times 3$.

**Example:** $3 \nmid 10$ because $10/3$ is not an integer.

## Greatest Common Divisor (GCD)

$\gcd(a, b)$ = largest positive integer that divides both $a$ and $b$.

**Example:** $\gcd(12, 18) = 6$

### Euclidean Algorithm (fast way to find GCD)

$$
\gcd(18, 12) = \gcd(12, 6) = \gcd(6, 0) = 6
$$

At each step: $\gcd(a, b) = \gcd(b, a \mod b)$

## Coprime / Relatively Prime

$a$ and $b$ are coprime if $\gcd(a, b) = 1$.

**Example:** $\gcd(8, 15) = 1$, so 8 and 15 are coprime.

## Division Algorithm

For any integers $a$ and $d > 0$:

$$
a = dq + r \quad \text{where } 0 \leq r < d
$$

$q$ = quotient, $r$ = remainder.

**Example:** $17 = 5 \times 3 + 2$, so $17 \div 5$ gives quotient 3, remainder 2.

**Where you'll need this:** Modular arithmetic (Topic 3.3), Euler's theorem, Fermat's little theorem.

---

# 14. Factorial, Permutation, Combination (Class 11)

## Factorial

$$
n! = n \times (n-1) \times (n-2) \times \cdots \times 2 \times 1
$$

| $n$ | $n!$ |
|-----|------|
| 0 | 1 |
| 1 | 1 |
| 2 | 2 |
| 3 | 6 |
| 4 | 24 |
| 5 | 120 |
| 6 | 720 |
| 7 | 5040 |
| 10 | 3628800 |

## Permutation (Order Matters)

$$
P(n, r) = \frac{n!}{(n-r)!}
$$

"Choose $r$ from $n$ and **arrange** them."

## Combination (Order Doesn't Matter)

$$
C(n, r) = \binom{n}{r} = \frac{n!}{r!(n-r)!}
$$

"Choose $r$ from $n$ (just select, no arrangement)."

### Quick Mental Math

$$
\binom{n}{0} = \binom{n}{n} = 1, \quad \binom{n}{1} = n, \quad \binom{n}{2} = \frac{n(n-1)}{2}
$$

**Where you'll need this:** Topic 3.3 (Counting), complete graphs $K_n$ have $\binom{n}{2}$ edges (Topic 3.1), binomial theorem.

---

# 15. Floor, Ceiling, and Modulus Functions

## Floor Function

$$
\lfloor x \rfloor = \text{greatest integer} \leq x
$$

| $x$ | $\lfloor x \rfloor$ |
|-----|---------------------|
| 3.7 | 3 |
| -1.2 | -2 |
| 5 | 5 |

## Ceiling Function

$$
\lceil x \rceil = \text{smallest integer} \geq x
$$

| $x$ | $\lceil x \rceil$ |
|-----|---------------------|
| 3.2 | 4 |
| -1.8 | -1 |
| 5 | 5 |

## Modulo Operation

$$
a \mod m = r \quad \text{where } a = mq + r, \; 0 \leq r < m
$$

| Expression | Result |
|-----------|--------|
| $17 \mod 5$ | 2 |
| $20 \mod 4$ | 0 |
| $7 \mod 3$ | 1 |

**Where you'll need this:** Generalized pigeonhole principle uses ceiling (Topic 3.3), modular arithmetic (Topic 3.3).

---

# 16. Greek Letters Used in This Course

You'll see these everywhere. Know them by sight.

| Letter | Name | Common Usage in This Course |
|--------|------|---------------------------|
| $\alpha$ | alpha | Scalar, angle |
| $\beta$ | beta | Scalar, angle |
| $\gamma$ | gamma | Scalar |
| $\delta$ | delta | Small change, transition function |
| $\varepsilon$ | epsilon | Very small positive number, neighbourhood |
| $\lambda$ | lambda | **Eigenvalue**, scalar in convex combination |
| $\mu$ | mu | Scalar |
| $\theta$ | theta | Angle |
| $\pi$ | pi | 3.14159... |
| $\sigma$ | sigma | Sum (upper case $\Sigma$) |
| $\phi$ | phi | Euler's totient function $\phi(n)$ |
| $\chi$ | chi | Chromatic number $\chi(G)$ |
| $\rho$ | rho | Rank |

---

# 17. Prerequisite Map: What You Need for Each Topic

This tells you exactly which prerequisites to review before studying each topic.

## Module 1: Linear Algebra

| Topic | Prerequisites You MUST Know |
|-------|---------------------------|
| **1.1 Matrices** | §9 Matrix basics, §2 Algebra, §6 Complex numbers |
| **1.2 Inverse & Adjoint** | §8 Determinants, §9 Matrices, §2 Fractions |
| **1.3 Systems of Equations** | §10 Systems of equations, §8 Determinants, §9 Matrices |
| **1.4 Vector Spaces** | §3 Sets, §4 Functions, §12 Proof techniques, §2 Algebra |
| **1.5 Linear Transformations** | §4 Functions (injective/surjective/bijective), §9 Matrices, Topic 1.4 |
| **1.6 Eigenvalues** | §8 Determinants, §2 Quadratic formula & factoring, §6 Complex numbers, Topic 1.1 |
| **1.7 Diagonalization** | §6 Complex numbers, Topics 1.6, 1.1 |

## Module 2: Linear Programming

| Topic | Prerequisites You MUST Know |
|-------|---------------------------|
| **2.1 LPP Formulation** | §11 Inequalities, §3 Sets, §7 Coordinate geometry basics |
| **2.2 Graphical Method** | §7 Coordinate geometry (plotting lines, intersections), §11 Inequalities |
| **2.3 Simplex Method** | §2 Fraction arithmetic (VERY important), §9 Matrix basics, §10 Systems of equations |
| **2.4 Artificial Variables** | §2 Fraction arithmetic, Topic 2.3 Simplex method |

## Module 3: Discrete Structures

| Topic | Prerequisites You MUST Know |
|-------|---------------------------|
| **3.1 Graph Theory** | §3 Sets, §5 Summation notation, §14 Combinations ($\binom{n}{2}$) |
| **3.2 State Machines** | §3 Sets, §4 Functions, §3 Cartesian product |
| **3.3 Counting** | §14 Factorials/Permutations/Combinations, §13 Divisibility & GCD, §15 Floor/Ceiling, §12 Proof by contradiction |

---

## Study Order Recommendation for Prerequisites

If you're starting from scratch, study these prerequisites in this order:

```
1. Number Systems (§1)           ← 15 minutes
2. Basic Algebra (§2)            ← 30 minutes (PRACTICE fractions!)
3. Sets (§3)                     ← 20 minutes
4. Functions (§4)                ← 20 minutes
5. Summation Notation (§5)       ← 15 minutes
6. Complex Numbers (§6)          ← 30 minutes
7. Coordinate Geometry (§7)      ← 20 minutes
8. Determinants (§8)             ← 45 minutes (PRACTICE 3×3!)
9. Matrix Basics (§9)            ← 45 minutes (PRACTICE multiplication!)
10. Systems of Equations (§10)    ← 20 minutes
11. Inequalities (§11)           ← 15 minutes
12. Proof Techniques (§12)       ← 30 minutes
13. Divisibility & GCD (§13)     ← 20 minutes
14. Factorial/nCr/nPr (§14)      ← 20 minutes
15. Floor/Ceiling (§15)          ← 10 minutes
```

**Total: ~5.5 hours** to cover ALL prerequisites from scratch.

**If you're NOT a complete beginner**, focus on:
- **§2 (fractions)** — you'll need this for Simplex Method
- **§8 (3×3 determinants)** — you'll need this for eigenvalues
- **§9 (matrix multiplication)** — you'll need this everywhere in Module 1
- **§6 (complex numbers)** — you'll need this for Hermitian/Unitary matrices

---

> **Remember:** If you're stuck on any lecture note, come back to this file and review the relevant prerequisite section. Don't try to understand eigenvalues if you can't compute a 3×3 determinant!
