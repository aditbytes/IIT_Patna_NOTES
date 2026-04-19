# 📚 CDA 106 — Numerical Methods for Data Science
## End Semester Exam Study Plan (1 Week)
### Dr. Prashant Kumar Srivastava | IIT Patna

---

> **Exam window:** ~April 26–30, 2026  
> **Today:** April 19, 2026  
> **Available days:** 7 days  
> **Total Lectures:** 20 (Lecture 1–20)

---

## 🗂️ Syllabus Overview (Lecture-wise)

| Module | Lectures | Topics |
|--------|----------|--------|
| **Module 1: Numerical Integration** | L1–L6 | Newton-Cotes Formulas, Trapezoidal Rule, Simpson's 1/3 & 3/8 Rule, Composite Rules, Gauss Quadrature (Legendre, Hermite) |
| **Module 2: Numerical Differentiation & Interpolation** | L7–L8 | Forward Difference, Backward Difference, Newton-Gregory Forward/Backward Interpolation, Divided Differences |
| **Module 3: Ordinary Differential Equations (ODE)** | L9 | Euler's Method, Modified Euler, Runge-Kutta Methods, Initial Value Problems |
| **Module 4: Linear Algebra — Direct Methods** | L10–L14 | Gauss Elimination, Partial Pivoting, LU Decomposition, PA = LU, Cholesky Decomposition |
| **Module 5: Eigenvalue Problems** | L15–L16 | Power Method, Inverse Power Method, Shifted Power Method, Convergence |
| **Module 6: QR Decomposition & SVD** | L17–L20 | Gram-Schmidt Orthogonalization, QR Decomposition, QR Algorithm for Eigenvalues, Singular Value Decomposition (SVD) |

---

## 📅 Day-by-Day Study Plan

### Day 1 — Sunday, April 20
**Module 1 (Part 1): Numerical Integration Basics**

| Time | Task | Lectures |
|------|------|----------|
| 9:00 – 11:00 | Introduction, basic concepts of numerical integration, area under curve | L1 |
| 11:30 – 1:30 | Newton-Cotes formulas derivation: Trapezoidal Rule, Simpson's 1/3 Rule, Simpson's 3/8 Rule | L2, L3 |
| 3:00 – 5:00 | Composite Trapezoidal & Simpson's Rules, Error analysis | L4 |
| 5:30 – 7:00 | Practice problems on Trapezoidal & Simpson's rules | — |
| 8:00 – 9:00 | Write MATLAB codes for Trapezoidal & Simpson's rules | — |

**Target:** Understand derivation of all Newton-Cotes formulas & solve 5+ problems.

---

### Day 2 — Monday, April 21
**Module 1 (Part 2): Gauss Quadrature**

| Time | Task | Lectures |
|------|------|----------|
| 9:00 – 11:00 | Gauss-Legendre Quadrature (2-point, 3-point), weights & nodes derivation | L5 |
| 11:30 – 1:30 | Gauss-Hermite Quadrature (2-point, 3-point) | L6 |
| 3:00 – 5:00 | Comparison: Newton-Cotes vs Gauss Quadrature, accuracy analysis | L5, L6 |
| 5:30 – 7:00 | Practice problems on Gauss Quadrature | — |
| 8:00 – 9:00 | Write MATLAB codes for Gauss-Legendre & Gauss-Hermite | — |

**Target:** Master Gauss quadrature node/weight derivation & apply to examples.

---

### Day 3 — Tuesday, April 22
**Module 2: Numerical Differentiation & Interpolation**

| Time | Task | Lectures |
|------|------|----------|
| 9:00 – 11:00 | Forward Difference, Backward Difference, Central Difference formulas | L7 |
| 11:30 – 1:30 | Newton-Gregory Forward & Backward Interpolation formulas | L7, L8 |
| 3:00 – 5:00 | Newton's Divided Difference Interpolation, Lagrange Interpolation | L8 |
| 5:30 – 7:00 | Practice: Interpolation table construction, numerical differentiation problems | — |
| 8:00 – 9:00 | MATLAB codes for interpolation methods | — |

**Target:** Build difference tables, apply interpolation formulas, estimate derivatives.

---

### Day 4 — Wednesday, April 23
**Module 3 + Module 4 (Part 1): ODE + Gauss Elimination**

| Time | Task | Lectures |
|------|------|----------|
| 9:00 – 11:00 | Euler's Method, Modified Euler, step-size effect, error analysis | L9 |
| 11:30 – 1:30 | Runge-Kutta 2nd & 4th order methods | L9 |
| 3:00 – 5:00 | Systems of Linear Equations: Gauss Elimination with partial pivoting | L10 |
| 5:30 – 7:00 | Practice: Solve ODEs step-by-step & Gauss Elimination problems | — |
| 8:00 – 9:00 | MATLAB codes for Euler, RK4, Gauss Elimination | — |

**Target:** Solve IVPs using Euler/RK methods; perform Gauss elimination with pivoting.

---

### Day 5 — Thursday, April 24
**Module 4 (Part 2): LU, PA=LU, Cholesky Decomposition**

| Time | Task | Lectures |
|------|------|----------|
| 9:00 – 11:00 | LU Decomposition: concept, algorithm, forward/backward substitution | L11 |
| 11:30 – 1:30 | LU Decomposition: solving Ax = b using L and U | L12 |
| 3:00 – 5:00 | Cholesky Decomposition (A = LL^T) for positive definite matrices | L12, L13 |
| 5:30 – 7:00 | PA = LU Decomposition with permutation matrices | L14 |
| 8:00 – 9:00 | Practice problems: decompose matrices step-by-step | — |

**Target:** Perform LU, Cholesky, PA=LU by hand; understand when to use each.

---

### Day 6 — Friday, April 25
**Module 5 + Module 6 (Part 1): Eigenvalues + QR Decomposition**

| Time | Task | Lectures |
|------|------|----------|
| 9:00 – 11:00 | Power Method: finding dominant eigenvalue & eigenvector, convergence | L15, L16 |
| 11:30 – 1:30 | Inverse Power Method, Shifted Power Method | L16 |
| 3:00 – 5:00 | Gram-Schmidt Orthogonalization process | L18 |
| 5:30 – 7:00 | QR Decomposition: A = QR, properties of Q and R | L17, L19 |
| 8:00 – 9:00 | MATLAB codes for Power Method & QR Decomposition | — |

**Target:** Compute eigenvalues via Power Method; decompose matrices using Gram-Schmidt into QR.

---

### Day 7 — Saturday, April 26
**Module 6 (Part 2): SVD + Full Revision**

| Time | Task | Lectures |
|------|------|----------|
| 9:00 – 11:00 | QR Algorithm for computing all eigenvalues | L19 |
| 11:30 – 1:30 | Singular Value Decomposition (SVD): A = UΣV^T, computation steps | L20 |
| 3:00 – 4:00 | SVD applications, low-rank approximation | L20 |
| 4:30 – 6:00 | **FULL REVISION:** Formulas sheet — write down all key formulas | — |
| 6:30 – 8:00 | **Mock exam practice:** Solve mixed problems (1 from each module) | — |
| 8:30 – 9:30 | Review weak areas, re-read tricky derivations | — |

**Target:** Complete SVD, revise entire syllabus, solve a full practice set.

---

## 🔑 Key Formulas to Memorize

### Numerical Integration
- **Trapezoidal Rule:** $\int_a^b f(x)dx \approx \frac{h}{2}[f(a) + f(b)]$
- **Simpson's 1/3:** $\int_a^b f(x)dx \approx \frac{h}{3}[f(a) + 4f(a+h) + f(b)]$
- **Simpson's 3/8:** $\int_a^b f(x)dx \approx \frac{3h}{8}[f_0 + 3f_1 + 3f_2 + f_3]$
- **Composite Trapezoidal:** $\frac{h}{2}[f_0 + 2(f_1+f_2+\ldots+f_{n-1}) + f_n]$
- **Gauss-Legendre 2-pt:** $\int_{-1}^{1} f(x)dx \approx f(-\frac{1}{\sqrt{3}}) + f(\frac{1}{\sqrt{3}})$

### Numerical Differentiation
- **Forward:** $f'(x) \approx \frac{f(x+h) - f(x)}{h}$
- **Backward:** $f'(x) \approx \frac{f(x) - f(x-h)}{h}$
- **Central:** $f'(x) \approx \frac{f(x+h) - f(x-h)}{2h}$

### ODE Methods
- **Euler:** $y_{n+1} = y_n + h \cdot f(x_n, y_n)$
- **RK4:** $y_{n+1} = y_n + \frac{h}{6}(k_1 + 2k_2 + 2k_3 + k_4)$

### Linear Algebra
- **LU:** $A = LU$, solve $Ly = b$ then $Ux = y$
- **Cholesky:** $A = LL^T$ (A must be symmetric positive definite)
- **PA = LU:** With row permutation matrix P
- **Power Method:** $x^{(k+1)} = \frac{Ax^{(k)}}{||Ax^{(k)}||}$, eigenvalue $\lambda = \frac{x^T A x}{x^T x}$

### Matrix Decompositions
- **QR:** $A = QR$, $Q$ orthogonal, $R$ upper triangular
- **Gram-Schmidt:** $q_k = \frac{v_k}{||v_k||}$, $v_k = a_k - \sum_{j=1}^{k-1}\langle a_k, q_j \rangle q_j$
- **SVD:** $A = U\Sigma V^T$

---

## ⚡ Quick Tips
1. **Focus on algorithms:** Most exam questions test step-by-step computation
2. **Derivations matter:** Especially for Newton-Cotes & Gauss Quadrature
3. **Matrix operations:** Practice 3×3 and 4×4 examples by hand
4. **Error analysis:** Know the order of error for each method
5. **MATLAB codes:** Be ready to write pseudocode/MATLAB for every method
6. **Connections:** Understand when/why to choose one method over another

---

## 📊 Priority Weightage (Estimated)

| Topic | Estimated Weight | Priority |
|-------|-----------------|----------|
| Numerical Integration (Newton-Cotes + Gauss) | ~20-25% | ⭐⭐⭐ HIGH |
| LU / Cholesky / PA=LU Decomposition | ~20% | ⭐⭐⭐ HIGH |
| QR Decomposition & Gram-Schmidt | ~15% | ⭐⭐⭐ HIGH |
| Eigenvalue Methods (Power Method) | ~10-15% | ⭐⭐ MEDIUM |
| SVD | ~10% | ⭐⭐ MEDIUM |
| Numerical Differentiation & Interpolation | ~10% | ⭐⭐ MEDIUM |
| ODE (Euler, RK) | ~10% | ⭐⭐ MEDIUM |

---

*Good luck with the exam! Consistent daily effort > last-minute cramming.* 🎯
