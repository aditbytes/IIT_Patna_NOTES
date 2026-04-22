# 💻 MATLAB Code To-Do List — CDA 106
## Numerical Methods for Data Science | Topic-Wise

> Track your MATLAB coding practice for the end semester exam.  
> Mark `[x]` when completed.

---

## Module 1: Numerical Integration (Lectures 1–6)

### 1.1 Newton-Cotes Formulas

- [ ] **Trapezoidal Rule (Single interval)**
  - Input: function `f(x)`, limits `a`, `b`
  - Formula: `I = (h/2) * (f(a) + f(b))`
  - Test with: `∫₀¹ x² dx`

- [ ] **Composite Trapezoidal Rule**
  - Input: function `f(x)`, limits `a`, `b`, number of subintervals `n`
  - Formula: `I = (h/2) * [f(a) + 2*Σf(xᵢ) + f(b)]`
  - Test with: `∫₀π sin(x) dx` with n = 10

- [ ] **Simpson's 1/3 Rule (Single interval)**
  - Input: function `f(x)`, limits `a`, `b`
  - Formula: `I = (h/3) * [f(a) + 4*f(a+h) + f(b)]`
  - Test with: `∫₀¹ eˣ dx`

- [ ] **Composite Simpson's 1/3 Rule**
  - Input: function, limits, `n` (must be even)
  - Formula: `I = (h/3) * [f₀ + 4*(f₁+f₃+...) + 2*(f₂+f₄+...) + fₙ]`
  - Test with: `∫₁² (1/x) dx` with n = 10

- [ ] **Simpson's 3/8 Rule**
  - Input: function, limits `a`, `b`
  - Formula: `I = (3h/8) * [f₀ + 3f₁ + 3f₂ + f₃]`
  - Test with: `∫₀¹ x³ dx`

- [ ] **Error Comparison Script**
  - Compare Trapezoidal vs Simpson's 1/3 vs Simpson's 3/8 for same integral
  - Plot error vs `n` for each method

### 1.2 Gauss Quadrature

- [ ] **Gauss-Legendre 2-Point Quadrature**
  - Nodes: `x = ±1/√3`, Weights: `w = 1`
  - Transform `[a,b]` to `[-1,1]` using substitution
  - Test with: `∫₀¹ (x² + 1) dx`

- [ ] **Gauss-Legendre 3-Point Quadrature**
  - Nodes: `x = 0, ±√(3/5)`, Weights: `w = 8/9, 5/9, 5/9`
  - Test with: `∫₋₁¹ x⁴ dx`

- [ ] **Gauss-Legendre n-Point (General)**
  - Use MATLAB's `lgwt()` or manually compute nodes/weights
  - Compare accuracy for n = 2, 3, 4, 5

- [ ] **Gauss-Hermite 2-Point Quadrature**
  - Evaluate `∫₋∞^∞ f(x) e^(-x²) dx`
  - Nodes: `x = ±1`, Weights: `w = √π/2`
  - Test with: `f(x) = x²`

- [ ] **Gauss-Hermite 3-Point Quadrature**
  - Nodes: `x = 0, ±√(3/2)`, Weights: `w₀ = 2√π/3, w₁ = √π/6`
  - Test with: `f(x) = x⁴`

- [ ] **Interval Transformation Utility**
  - Write a function to map `[a,b] → [-1,1]` for Gauss-Legendre
  - `x = ((b-a)*t + (b+a)) / 2`

---

## Module 2: Numerical Differentiation & Interpolation (Lectures 7–8)

### 2.1 Finite Differences

- [ ] **Forward Difference Table Generator**
  - Input: `x` values, `f(x)` values
  - Output: complete forward difference table (Δf, Δ²f, ...)

- [ ] **Backward Difference Table Generator**
  - Input: `x` values, `f(x)` values
  - Output: complete backward difference table (∇f, ∇²f, ...)

- [ ] **First Derivative using Forward Difference**
  - Formula: `f'(x) ≈ (f(x+h) - f(x)) / h`
  - Also: higher-order formulas using Δ²f, Δ³f

- [ ] **First Derivative using Backward Difference**
  - Formula: `f'(x) ≈ (f(x) - f(x-h)) / h`

- [ ] **First Derivative using Central Difference**
  - Formula: `f'(x) ≈ (f(x+h) - f(x-h)) / (2h)`

- [ ] **Second Derivative using Central Difference**
  - Formula: `f''(x) ≈ (f(x+h) - 2f(x) + f(x-h)) / h²`

### 2.2 Interpolation

- [ ] **Newton-Gregory Forward Interpolation**
  - Build forward difference table
  - Compute `P(x)` using `s = (x - x₀)/h`
  - Test: Interpolate at a midpoint between given data

- [ ] **Newton-Gregory Backward Interpolation**
  - Build backward difference table
  - Compute `P(x)` using `s = (x - xₙ)/h`
  - Test: Interpolate near the end of data

- [ ] **Newton's Divided Difference Interpolation**
  - Build divided difference table
  - Works for unequally spaced data
  - Test with non-uniform spacing

- [ ] **Lagrange Interpolation**
  - Input: data points `(xᵢ, yᵢ)`, evaluation point `x`
  - Formula: `P(x) = Σ yᵢ * Lᵢ(x)`
  - Test with 4 data points

---

## Module 3: Ordinary Differential Equations (Lecture 9)

### 3.1 Initial Value Problems (IVP)

- [ ] **Euler's Method**
  - Input: `f(x,y)`, `x₀`, `y₀`, `h`, `xₙ`
  - Formula: `y_{n+1} = yₙ + h * f(xₙ, yₙ)`
  - Test: `dy/dx = x + y`, `y(0) = 1`, solve to `x = 1`

- [ ] **Modified Euler's Method (Heun's Method)**
  - Predictor: `ỹ = yₙ + h * f(xₙ, yₙ)`
  - Corrector: `y_{n+1} = yₙ + (h/2) * [f(xₙ,yₙ) + f(x_{n+1}, ỹ)]`
  - Test with same problem as Euler

- [ ] **Runge-Kutta 2nd Order (RK2)**
  - `k₁ = h * f(xₙ, yₙ)`
  - `k₂ = h * f(xₙ + h, yₙ + k₁)`
  - `y_{n+1} = yₙ + (k₁ + k₂)/2`

- [ ] **Runge-Kutta 4th Order (RK4)**
  - `k₁ = h * f(xₙ, yₙ)`
  - `k₂ = h * f(xₙ + h/2, yₙ + k₁/2)`
  - `k₃ = h * f(xₙ + h/2, yₙ + k₂/2)`
  - `k₄ = h * f(xₙ + h, yₙ + k₃)`
  - `y_{n+1} = yₙ + (k₁ + 2k₂ + 2k₃ + k₄)/6`
  - Test & compare error with Euler's method

- [ ] **Convergence Comparison Plot**
  - Solve same ODE using Euler, Modified Euler, RK2, RK4
  - Plot all solutions vs exact solution
  - Compare error for different step sizes `h`

---

## Module 4: Linear Algebra — Direct Methods (Lectures 10–14)

### 4.1 Gauss Elimination

- [ ] **Naive Gauss Elimination**
  - Input: augmented matrix `[A|b]`
  - Forward elimination → back substitution
  - Test with 3×3 system

- [ ] **Gauss Elimination with Partial Pivoting**
  - Add row swapping for largest pivot element
  - Test with a system that fails without pivoting

- [ ] **Gauss Elimination with Scaled Partial Pivoting**
  - Scale-based pivot selection
  - Test with ill-conditioned system

### 4.2 LU Decomposition

- [ ] **LU Decomposition (Doolittle's Method)**
  - Input: matrix `A`
  - Output: `L` (lower triangular, 1s on diagonal), `U` (upper triangular)
  - Verify: `L * U == A`

- [ ] **Solve Ax = b using LU Decomposition**
  - Step 1: Decompose `A = LU`
  - Step 2: Solve `Ly = b` (forward substitution)
  - Step 3: Solve `Ux = y` (backward substitution)
  - Test with 3×3 and 4×4 systems

- [ ] **Forward Substitution (Standalone)**
  - Input: Lower triangular matrix `L`, vector `b`
  - Output: solution `y`

- [ ] **Backward Substitution (Standalone)**
  - Input: Upper triangular matrix `U`, vector `y`
  - Output: solution `x`

### 4.3 PA = LU Decomposition

- [ ] **PA = LU with Permutation Matrix**
  - Input: matrix `A`
  - Output: permutation matrix `P`, `L`, `U`
  - Include row exchanges tracking
  - Verify: `P * A == L * U`

### 4.4 Cholesky Decomposition

- [ ] **Cholesky Decomposition (A = LL^T)**
  - Input: symmetric positive definite matrix `A`
  - Output: lower triangular `L`
  - Check positive definiteness first
  - Verify: `L * L' == A`

- [ ] **Solve Ax = b using Cholesky**
  - Decompose, then solve `Ly = b` and `L'x = y`
  - Test with SPD matrix

---

## Module 5: Eigenvalue Problems (Lectures 15–16)

### 5.1 Power Method

- [ ] **Power Method (Dominant Eigenvalue)**
  - Input: matrix `A`, initial guess `x₀`, tolerance, max iterations
  - Iterate: `x_{k+1} = Ax_k / ||Ax_k||`
  - Output: largest eigenvalue `λ₁` and eigenvector
  - Test with 3×3 matrix, verify with `eig(A)`

- [ ] **Inverse Power Method (Smallest Eigenvalue)**
  - Use `A⁻¹` (or solve `Ax = y` at each step)
  - Converges to smallest eigenvalue
  - Test and compare with `eig(A)`

- [ ] **Shifted Power Method**
  - For eigenvalue closest to a given shift `σ`
  - Apply inverse power method on `(A - σI)`
  - Test: find eigenvalue closest to a specific value

- [ ] **Convergence Analysis Script**
  - Plot eigenvalue estimate vs iteration number
  - Show convergence rate depends on `|λ₂/λ₁|`

---

## Module 6: QR Decomposition & SVD (Lectures 17–20)

### 6.1 Gram-Schmidt Process

- [ ] **Classical Gram-Schmidt Orthogonalization**
  - Input: set of vectors (columns of A)
  - Output: orthonormal set `{q₁, q₂, ..., qₙ}`
  - Algorithm:
    ```
    v₁ = a₁, q₁ = v₁/||v₁||
    vₖ = aₖ - Σ <aₖ, qⱼ> qⱼ, qₖ = vₖ/||vₖ||
    ```

- [ ] **Modified Gram-Schmidt (Numerically Stable)**
  - More stable version for ill-conditioned matrices
  - Compare orthogonality of Q with classical vs modified

### 6.2 QR Decomposition

- [ ] **QR Decomposition via Gram-Schmidt**
  - Input: matrix `A`
  - Output: orthogonal `Q`, upper triangular `R`
  - `R(i,j) = <aⱼ, qᵢ>`
  - Verify: `Q * R == A`, `Q' * Q == I`

- [ ] **Solve Ax = b using QR**
  - `QRx = b` → `Rx = Q'b` → back substitution
  - Compare with LU method

### 6.3 QR Algorithm for Eigenvalues

- [ ] **QR Algorithm (Basic)**
  - Iterate: decompose `Aₖ = QₖRₖ`, then `A_{k+1} = RₖQₖ`
  - Converges to upper triangular with eigenvalues on diagonal
  - Test with symmetric matrix

- [ ] **QR Algorithm with Shifts**
  - Wilkinson shift for faster convergence
  - Test convergence speed vs basic QR

### 6.4 Singular Value Decomposition (SVD)

- [ ] **SVD Computation (A = UΣV^T)**
  - Step 1: Compute `A^T A` and find its eigenvalues → singular values `σᵢ`
  - Step 2: Find `V` (eigenvectors of `A^T A`)
  - Step 3: Find `U` from `u_i = Av_i / σ_i`
  - Verify: `U * S * V' == A`

- [ ] **Compute SVD for Rectangular Matrix**
  - Handle `m × n` matrix where `m ≠ n`
  - Test with a 3×2 matrix

- [ ] **Low-Rank Approximation using SVD**
  - Keep only top `k` singular values
  - `Aₖ = UₖΣₖVₖ^T`
  - Compute approximation error `||A - Aₖ||`

- [ ] **SVD Applications Script**
  - Image compression using SVD (bonus)
  - Pseudoinverse computation: `A⁺ = VΣ⁻¹U^T`

---

## 🔧 Utility Functions To-Do

- [ ] **Matrix display function** — pretty print matrices with formatting
- [ ] **Norm computation** — L1, L2, L∞, Frobenius norms
- [ ] **Check positive definiteness** — verify all eigenvalues > 0
- [ ] **Condition number calculator** — `cond(A) = ||A|| * ||A⁻¹||`
- [ ] **Error calculator** — absolute error, relative error, % error

---

## 📋 Summary Checklist

| Module | Total Codes | Completed |
|--------|------------|-----------|
| Numerical Integration | 12 | _ / 12 |
| Numerical Differentiation & Interpolation | 10 | _ / 10 |
| ODE Methods | 5 | _ / 5 |
| Linear Algebra — Direct Methods | 10 | _ / 10 |
| Eigenvalue Problems | 4 | _ / 4 |
| QR & SVD | 9 | _ / 9 |
| Utilities | 5 | _ / 5 |
| **TOTAL** | **55** | **_ / 55** |

---

> **Tip:** For each code, first write pseudocode on paper, then implement in MATLAB. Test with small examples where you know the exact answer.
