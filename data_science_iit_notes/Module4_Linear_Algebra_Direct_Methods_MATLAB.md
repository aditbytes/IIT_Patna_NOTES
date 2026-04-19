# Module 4: Linear Algebra — Direct Methods — MATLAB Codes
## CDA 106 | Lectures 10–14

---

## 4.1 Naive Gauss Elimination

```matlab
function x = gauss_elimination_naive(A, b)
% GAUSS_ELIMINATION_NAIVE Solve Ax = b using naive Gauss Elimination
%   No pivoting — will fail if any pivot is zero

    n = length(b);
    Aug = [A, b(:)];   % Augmented matrix [A|b]

    fprintf('=== Naive Gauss Elimination ===\n');
    fprintf('Initial augmented matrix:\n');
    disp(Aug);

    % Forward Elimination
    for k = 1:n-1
        fprintf('--- Pivot on a(%d,%d) = %.6f ---\n', k, k, Aug(k,k));

        if Aug(k,k) == 0
            error('Zero pivot encountered at position (%d,%d)! Use pivoting.', k, k);
        end

        for i = k+1:n
            m = Aug(i,k) / Aug(k,k);    % multiplier
            Aug(i, k:n+1) = Aug(i, k:n+1) - m * Aug(k, k:n+1);
            fprintf('  R%d = R%d - (%.6f)*R%d\n', i, i, m, k);
        end
        fprintf('After elimination step %d:\n', k);
        disp(Aug);
    end

    % Back Substitution
    x = zeros(n, 1);
    x(n) = Aug(n, n+1) / Aug(n, n);
    for i = n-1:-1:1
        x(i) = (Aug(i, n+1) - Aug(i, i+1:n) * x(i+1:n)) / Aug(i, i);
    end

    fprintf('Solution:\n');
    for i = 1:n
        fprintf('  x(%d) = %.8f\n', i, x(i));
    end
end
```

**Test:**
```matlab
A = [2, 1, -1;
     -3, -1, 2;
     -2, 1, 2];
b = [8; -11; -3];

x = gauss_elimination_naive(A, b);
fprintf('\nVerification A*x:\n');
disp(A * x);
% Expected: x = [2; 3; -1]
```

---

## 4.2 Gauss Elimination with Partial Pivoting

```matlab
function [x, P_record] = gauss_elimination_partial_pivot(A, b)
% GAUSS_ELIMINATION_PARTIAL_PIVOT Solve Ax = b with partial pivoting
%   Swaps rows to use the largest element in the column as pivot

    n = length(b);
    Aug = [A, b(:)];
    P_record = 1:n;     % Track row permutations

    fprintf('=== Gauss Elimination with Partial Pivoting ===\n');

    % Forward Elimination with Partial Pivoting
    for k = 1:n-1
        % Find row with max element in column k (from row k downwards)
        [~, max_row] = max(abs(Aug(k:n, k)));
        max_row = max_row + k - 1;

        % Swap rows if needed
        if max_row ~= k
            Aug([k, max_row], :) = Aug([max_row, k], :);
            P_record([k, max_row]) = P_record([max_row, k]);
            fprintf('Swap R%d <-> R%d\n', k, max_row);
        end

        fprintf('Pivot: a(%d,%d) = %.6f\n', k, k, Aug(k,k));

        % Eliminate below pivot
        for i = k+1:n
            m = Aug(i,k) / Aug(k,k);
            Aug(i, k:n+1) = Aug(i, k:n+1) - m * Aug(k, k:n+1);
            fprintf('  R%d = R%d - (%.6f)*R%d\n', i, i, m, k);
        end

        fprintf('After step %d:\n', k);
        disp(Aug);
    end

    % Back Substitution
    x = zeros(n, 1);
    x(n) = Aug(n, n+1) / Aug(n, n);
    for i = n-1:-1:1
        x(i) = (Aug(i, n+1) - Aug(i, i+1:n) * x(i+1:n)) / Aug(i, i);
    end

    fprintf('Solution:\n');
    for i = 1:n
        fprintf('  x(%d) = %.8f\n', i, x(i));
    end
end
```

**Test:**
```matlab
% This system would fail without pivoting
A = [0, 2, 1;
     1, 1, 2;
     2, 1, 1];
b = [5; 6; 7];

x = gauss_elimination_partial_pivot(A, b);
fprintf('\nVerification A*x = b?\n');
disp(A * x);
```

---

## 4.3 Gauss Elimination with Scaled Partial Pivoting

```matlab
function x = gauss_elimination_scaled_pivot(A, b)
% GAUSS_ELIMINATION_SCALED_PIVOT Scaled partial pivoting
%   Scale factors: s(i) = max |a(i,j)| for each row
%   Pivot selection: max |a(i,k)| / s(i) for i = k,...,n

    n = length(b);
    Aug = [A, b(:)];

    % Scale factors
    s = max(abs(A), [], 2);
    fprintf('Scale factors: '); disp(s');

    idx = 1:n;  % row index tracker

    for k = 1:n-1
        % Scaled pivot selection
        ratios = abs(Aug(k:n, k)) ./ s(k:n);
        [~, max_pos] = max(ratios);
        max_row = max_pos + k - 1;

        if max_row ~= k
            Aug([k, max_row], :) = Aug([max_row, k], :);
            s([k, max_row]) = s([max_row, k]);
            idx([k, max_row]) = idx([max_row, k]);
            fprintf('Swap R%d <-> R%d (scaled ratio)\n', k, max_row);
        end

        for i = k+1:n
            m = Aug(i,k) / Aug(k,k);
            Aug(i, k:n+1) = Aug(i, k:n+1) - m * Aug(k, k:n+1);
        end
    end

    % Back Substitution
    x = zeros(n, 1);
    x(n) = Aug(n, n+1) / Aug(n, n);
    for i = n-1:-1:1
        x(i) = (Aug(i, n+1) - Aug(i, i+1:n) * x(i+1:n)) / Aug(i, i);
    end

    fprintf('Solution:\n');
    for i = 1:n
        fprintf('  x(%d) = %.8f\n', i, x(i));
    end
end
```

**Test:**
```matlab
A = [2, 100000, 100000;
     1, 1, 1;
     1, -1, 2];
b = [100002; 3; 4];

x = gauss_elimination_scaled_pivot(A, b);
fprintf('Verification: A*x = \n');
disp(A * x);
```

---

## 4.4 Forward Substitution (Standalone)

```matlab
function y = forward_substitution(L, b)
% FORWARD_SUBSTITUTION Solve Ly = b where L is lower triangular
%   L - lower triangular matrix
%   b - right-hand side vector

    n = length(b);
    y = zeros(n, 1);

    y(1) = b(1) / L(1, 1);
    for i = 2:n
        y(i) = (b(i) - L(i, 1:i-1) * y(1:i-1)) / L(i, i);
    end

    fprintf('Forward Substitution (Ly = b):\n');
    for i = 1:n
        fprintf('  y(%d) = %.8f\n', i, y(i));
    end
end
```

---

## 4.5 Backward Substitution (Standalone)

```matlab
function x = backward_substitution(U, y)
% BACKWARD_SUBSTITUTION Solve Ux = y where U is upper triangular
%   U - upper triangular matrix
%   y - right-hand side vector

    n = length(y);
    x = zeros(n, 1);

    x(n) = y(n) / U(n, n);
    for i = n-1:-1:1
        x(i) = (y(i) - U(i, i+1:n) * x(i+1:n)) / U(i, i);
    end

    fprintf('Backward Substitution (Ux = y):\n');
    for i = 1:n
        fprintf('  x(%d) = %.8f\n', i, x(i));
    end
end
```

**Test:**
```matlab
L = [1, 0, 0; 2, 1, 0; -1, 3, 1];
U = [4, 1, -1; 0, 2, 3; 0, 0, 5];
b = [4; 9; 12];

fprintf('--- Forward Substitution ---\n');
y = forward_substitution(L, b);

fprintf('\n--- Backward Substitution ---\n');
x = backward_substitution(U, y);

fprintf('\nVerification: L*U*x = \n');
disp(L * U * x);
```

---

## 4.6 LU Decomposition (Doolittle's Method)

```matlab
function [L, U] = lu_doolittle(A)
% LU_DOOLITTLE LU Decomposition using Doolittle's method
%   A = L * U
%   L - lower triangular with 1s on diagonal
%   U - upper triangular

    n = size(A, 1);
    L = eye(n);
    U = zeros(n);

    for k = 1:n
        % Compute U(k, k:n)
        for j = k:n
            U(k, j) = A(k, j) - L(k, 1:k-1) * U(1:k-1, j);
        end

        % Compute L(k+1:n, k)
        for i = k+1:n
            L(i, k) = (A(i, k) - L(i, 1:k-1) * U(1:k-1, k)) / U(k, k);
        end
    end

    fprintf('=== LU Decomposition (Doolittle) ===\n');
    fprintf('L = \n'); disp(L);
    fprintf('U = \n'); disp(U);
    fprintf('Verification: L*U = \n'); disp(L * U);
end
```

**Test:**
```matlab
A = [2, -1, 1;
     4, 1, -1;
     1, 1, 1];

[L, U] = lu_doolittle(A);

% Verify
fprintf('A = \n'); disp(A);
fprintf('L*U = \n'); disp(L*U);
fprintf('Max error: %.2e\n', max(max(abs(A - L*U))));
```

---

## 4.7 Solve Ax = b using LU Decomposition

```matlab
function x = solve_lu(A, b)
% SOLVE_LU Solve Ax = b using LU decomposition
%   Step 1: A = LU
%   Step 2: Ly = b (forward sub)
%   Step 3: Ux = y (backward sub)

    fprintf('=== Solving Ax = b via LU ===\n\n');

    % Step 1: LU decomposition
    [L, U] = lu_doolittle(A);

    % Step 2: Forward substitution for Ly = b
    n = length(b);
    y = zeros(n, 1);
    y(1) = b(1) / L(1,1);
    for i = 2:n
        y(i) = (b(i) - L(i, 1:i-1) * y(1:i-1)) / L(i, i);
    end
    fprintf('Ly = b => y = \n'); disp(y');

    % Step 3: Backward substitution for Ux = y
    x = zeros(n, 1);
    x(n) = y(n) / U(n, n);
    for i = n-1:-1:1
        x(i) = (y(i) - U(i, i+1:n) * x(i+1:n)) / U(i, i);
    end
    fprintf('Ux = y => x = \n'); disp(x');

    % Verify
    fprintf('Verification: A*x = \n'); disp((A*x)');
    fprintf('Original b   = \n'); disp(b');
end
```

**Test:**
```matlab
A = [1, 1, 0;
     2, 1, -1;
     3, -1, -1];
b = [3; 3; -1];

x = solve_lu(A, b);
```

---

## 4.8 LU Decomposition (Crout's Method)

```matlab
function [L, U] = lu_crout(A)
% LU_CROUT LU Decomposition using Crout's method
%   L - lower triangular (NON-unit diagonal)
%   U - upper triangular with 1s on diagonal

    n = size(A, 1);
    L = zeros(n);
    U = eye(n);

    for k = 1:n
        % Compute L(k:n, k)
        for i = k:n
            L(i, k) = A(i, k) - L(i, 1:k-1) * U(1:k-1, k);
        end

        % Compute U(k, k+1:n)
        for j = k+1:n
            U(k, j) = (A(k, j) - L(k, 1:k-1) * U(1:k-1, j)) / L(k, k);
        end
    end

    fprintf('=== LU Decomposition (Crout) ===\n');
    fprintf('L = \n'); disp(L);
    fprintf('U = \n'); disp(U);
    fprintf('Verification: L*U = \n'); disp(L * U);
end
```

**Test:**
```matlab
A = [2, -1, 3;
     4, 2, 1;
     -6, -1, 2];

[L, U] = lu_crout(A);
fprintf('Max error: %.2e\n', max(max(abs(A - L*U))));
```

---

## 4.9 PA = LU Decomposition with Permutation Matrix

```matlab
function [P, L, U] = pa_lu(A)
% PA_LU Compute PA = LU decomposition with partial pivoting
%   P - permutation matrix
%   L - lower triangular (unit diagonal)
%   U - upper triangular
%   P * A = L * U

    n = size(A, 1);
    L = eye(n);
    U = A;
    P = eye(n);

    fprintf('=== PA = LU Decomposition ===\n');
    fprintf('Original A:\n'); disp(A);

    for k = 1:n-1
        % Partial pivoting
        [~, max_row] = max(abs(U(k:n, k)));
        max_row = max_row + k - 1;

        if max_row ~= k
            % Swap rows in U
            U([k, max_row], :) = U([max_row, k], :);
            % Swap rows in P
            P([k, max_row], :) = P([max_row, k], :);
            % Swap rows in L (only the already-computed part)
            if k > 1
                L([k, max_row], 1:k-1) = L([max_row, k], 1:k-1);
            end
            fprintf('Swap rows %d and %d\n', k, max_row);
        end

        % Elimination
        for i = k+1:n
            L(i, k) = U(i, k) / U(k, k);
            U(i, k:n) = U(i, k:n) - L(i, k) * U(k, k:n);
        end
    end

    fprintf('\nP = \n'); disp(P);
    fprintf('L = \n'); disp(L);
    fprintf('U = \n'); disp(U);
    fprintf('Verification: P*A = \n'); disp(P * A);
    fprintf('L*U = \n'); disp(L * U);
    fprintf('Max error |PA - LU|: %.2e\n', max(max(abs(P*A - L*U))));
end
```

**Test:**
```matlab
A = [1, 2, 4;
     3, 8, 14;
     2, 6, 13];

[P, L, U] = pa_lu(A);

% Solve Ax = b using PA = LU
b = [3; 13; 4];
Pb = P * b;
y = forward_substitution(L, Pb);
x = backward_substitution(U, y);
fprintf('Solution x = \n'); disp(x');
fprintf('Check: A*x = \n'); disp((A*x)');
```

---

## 4.10 Cholesky Decomposition (A = LL^T)

```matlab
function L = cholesky_decomp(A)
% CHOLESKY_DECOMP Cholesky Decomposition A = L * L'
%   A must be symmetric positive definite
%   L - lower triangular matrix

    n = size(A, 1);

    % Check symmetry
    if max(max(abs(A - A'))) > 1e-10
        error('Matrix A is not symmetric!');
    end

    % Check positive definiteness
    eigenvalues = eig(A);
    if any(eigenvalues <= 0)
        error('Matrix A is not positive definite! Eigenvalues: %s', ...
            mat2str(eigenvalues', 4));
    end

    L = zeros(n);

    for j = 1:n
        % Diagonal element
        sum_sq = 0;
        for k = 1:j-1
            sum_sq = sum_sq + L(j, k)^2;
        end
        L(j, j) = sqrt(A(j, j) - sum_sq);

        % Below diagonal
        for i = j+1:n
            sum_prod = 0;
            for k = 1:j-1
                sum_prod = sum_prod + L(i, k) * L(j, k);
            end
            L(i, j) = (A(i, j) - sum_prod) / L(j, j);
        end
    end

    fprintf('=== Cholesky Decomposition ===\n');
    fprintf('A = \n'); disp(A);
    fprintf('L = \n'); disp(L);
    fprintf('L^T = \n'); disp(L');
    fprintf('Verification: L*L'' = \n'); disp(L * L');
    fprintf('Max error: %.2e\n', max(max(abs(A - L*L'))));
end
```

**Test:**
```matlab
% Symmetric positive definite matrix
A = [4, 2, -2;
     2, 10, 4;
     -2, 4, 5];

L = cholesky_decomp(A);
```

---

## 4.11 Solve Ax = b using Cholesky Decomposition

```matlab
function x = solve_cholesky(A, b)
% SOLVE_CHOLESKY Solve Ax = b using Cholesky decomposition
%   Step 1: A = L * L'
%   Step 2: Ly = b (forward sub)
%   Step 3: L'x = y (backward sub)

    fprintf('=== Solving Ax = b via Cholesky ===\n\n');

    % Step 1: Cholesky
    L = cholesky_decomp(A);

    n = length(b);

    % Step 2: Forward substitution Ly = b
    y = zeros(n, 1);
    for i = 1:n
        y(i) = (b(i) - L(i, 1:i-1) * y(1:i-1)) / L(i, i);
    end
    fprintf('Ly = b => y = \n'); disp(y');

    % Step 3: Backward substitution L'x = y
    Lt = L';
    x = zeros(n, 1);
    for i = n:-1:1
        x(i) = (y(i) - Lt(i, i+1:n) * x(i+1:n)) / Lt(i, i);
    end
    fprintf('L''x = y => x = \n'); disp(x');

    fprintf('Verification: A*x = \n'); disp((A*x)');
end
```

**Test:**
```matlab
A = [4, 2, -2;
     2, 10, 4;
     -2, 4, 5];
b = [6; 12; 9];

x = solve_cholesky(A, b);
```

---

## 4.12 Cholesky Step-by-Step for a 4×4 Matrix

```matlab
%% Detailed step-by-step Cholesky
clc;

A = [4, 12, -16, 4;
     12, 37, -43, 14;
     -16, -43, 98, -22;
     4, 14, -22, 17];

fprintf('Matrix A:\n'); disp(A);
fprintf('Eigenvalues of A: '); disp(eig(A)');

n = 4;
L = zeros(n);

% Column 1
L(1,1) = sqrt(A(1,1));
fprintf('L(1,1) = sqrt(A(1,1)) = sqrt(%.0f) = %.4f\n', A(1,1), L(1,1));
for i = 2:n
    L(i,1) = A(i,1) / L(1,1);
    fprintf('L(%d,1) = A(%d,1)/L(1,1) = %.0f/%.4f = %.4f\n', i, i, A(i,1), L(1,1), L(i,1));
end

% Column 2
L(2,2) = sqrt(A(2,2) - L(2,1)^2);
fprintf('\nL(2,2) = sqrt(A(2,2) - L(2,1)^2) = sqrt(%.0f - %.4f) = %.4f\n', ...
    A(2,2), L(2,1)^2, L(2,2));
for i = 3:n
    L(i,2) = (A(i,2) - L(i,1)*L(2,1)) / L(2,2);
    fprintf('L(%d,2) = (A(%d,2) - L(%d,1)*L(2,1))/L(2,2) = %.4f\n', i, i, i, L(i,2));
end

% Column 3
L(3,3) = sqrt(A(3,3) - L(3,1)^2 - L(3,2)^2);
fprintf('\nL(3,3) = sqrt(A(3,3) - L(3,1)^2 - L(3,2)^2) = %.4f\n', L(3,3));
for i = 4:n
    L(i,3) = (A(i,3) - L(i,1)*L(3,1) - L(i,2)*L(3,2)) / L(3,3);
    fprintf('L(%d,3) = %.4f\n', i, L(i,3));
end

% Column 4
L(4,4) = sqrt(A(4,4) - L(4,1)^2 - L(4,2)^2 - L(4,3)^2);
fprintf('\nL(4,4) = %.4f\n', L(4,4));

fprintf('\nFinal L:\n'); disp(L);
fprintf('Verify L*L'' = A:\n'); disp(L*L');
```

---

## 4.13 Matrix Inverse via LU

```matlab
function A_inv = inverse_lu(A)
% INVERSE_LU Compute A^{-1} by solving A * x_i = e_i for each column

    n = size(A, 1);
    [L, U] = lu_doolittle(A);
    A_inv = zeros(n);

    for j = 1:n
        e = zeros(n, 1);
        e(j) = 1;

        % Solve Ly = e
        y = zeros(n, 1);
        y(1) = e(1) / L(1,1);
        for i = 2:n
            y(i) = (e(i) - L(i, 1:i-1) * y(1:i-1)) / L(i, i);
        end

        % Solve Ux = y
        x = zeros(n, 1);
        x(n) = y(n) / U(n,n);
        for i = n-1:-1:1
            x(i) = (y(i) - U(i, i+1:n) * x(i+1:n)) / U(i, i);
        end

        A_inv(:, j) = x;
    end

    fprintf('A^{-1} = \n'); disp(A_inv);
    fprintf('Verify A * A^{-1} = I:\n'); disp(A * A_inv);
end
```

**Test:**
```matlab
A = [2, 1, 1;
     4, 3, 3;
     8, 7, 9];
A_inv = inverse_lu(A);
fprintf('Built-in inv(A):\n'); disp(inv(A));
```

---

## 4.14 Determinant via LU

```matlab
function d = det_lu(A)
% DET_LU Compute determinant using LU decomposition
%   det(A) = det(L) * det(U) = product of diagonal of U
%   (since L has 1s on diagonal in Doolittle)

    [L, U] = lu_doolittle(A);
    d = prod(diag(U));
    fprintf('det(A) = product of diag(U) = %.8f\n', d);
    fprintf('Built-in det(A) = %.8f\n', det(A));
end
```

**Test:**
```matlab
A = [1, 2, 3; 4, 5, 6; 7, 8, 10];
d = det_lu(A);
```

---

## 4.15 Condition Number

```matlab
function kappa = condition_number(A)
% CONDITION_NUMBER Compute condition number of matrix A
%   kappa = ||A|| * ||A^{-1}||

    % Using different norms
    k1 = norm(A, 1) * norm(inv(A), 1);
    k2 = norm(A, 2) * norm(inv(A), 2);
    kinf = norm(A, inf) * norm(inv(A), inf);

    fprintf('Condition Number of A:\n');
    fprintf('  kappa_1   (1-norm):   %.6f\n', k1);
    fprintf('  kappa_2   (2-norm):   %.6f\n', k2);
    fprintf('  kappa_inf (inf-norm): %.6f\n', kinf);
    fprintf('  Built-in cond(A):     %.6f\n', cond(A));

    kappa = k2;
end
```

**Test:**
```matlab
% Well-conditioned
A1 = [1, 0; 0, 1];
fprintf('Identity matrix:\n');
condition_number(A1);

% Ill-conditioned (Hilbert matrix)
A2 = hilb(5);
fprintf('\nHilbert 5x5:\n');
condition_number(A2);
```

---

## 4.16 Full Demo — Solving a 4×4 System All Methods

```matlab
%% Comprehensive demo: solve 4x4 system using all methods
clc; clear;

A = [2, 1, -1, 1;
     4, 5, -3, 5;
     -2, 5, -2, 6;
     4, 11, -4, 8];
b = [5; 9; 4; 2];

fprintf('============================================\n');
fprintf('Solving Ax = b (4x4 system)\n');
fprintf('============================================\n\n');

% Method 1: Naive Gauss
fprintf('--- Method 1: Naive Gauss Elimination ---\n');
x1 = gauss_elimination_naive(A, b);

% Method 2: Partial Pivoting
fprintf('\n--- Method 2: Gauss with Partial Pivoting ---\n');
x2 = gauss_elimination_partial_pivot(A, b);

% Method 3: LU Decomposition
fprintf('\n--- Method 3: LU Decomposition ---\n');
x3 = solve_lu(A, b);

% Method 4: PA = LU
fprintf('\n--- Method 4: PA = LU ---\n');
[P, L, U] = pa_lu(A);
y = forward_substitution(L, P*b);
x4 = backward_substitution(U, y);

% Compare
fprintf('\n============================================\n');
fprintf('Solution Comparison:\n');
fprintf('%-12s %-12s %-12s %-12s %-12s\n', 'Variable', 'Naive', 'Pivot', 'LU', 'PA=LU');
for i = 1:4
    fprintf('x(%d)         %-12.6f %-12.6f %-12.6f %-12.6f\n', i, x1(i), x2(i), x3(i), x4(i));
end

fprintf('\nBuilt-in MATLAB: x = \n');
disp((A\b)');
```

---
