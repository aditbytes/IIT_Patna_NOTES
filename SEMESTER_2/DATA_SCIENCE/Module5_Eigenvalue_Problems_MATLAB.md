# Module 5: Eigenvalue Problems — MATLAB Codes
## CDA 106 | Lectures 15–16

---

## 5.1 Power Method (Dominant Eigenvalue)

```matlab
function [lambda, v, iter] = power_method(A, x0, tol, max_iter)
% POWER_METHOD Find the dominant (largest magnitude) eigenvalue and eigenvector
%   A        - square matrix
%   x0       - initial guess vector
%   tol      - convergence tolerance
%   max_iter - maximum iterations
%
%   Algorithm:
%     x_{k+1} = A * x_k
%     x_{k+1} = x_{k+1} / ||x_{k+1}||_inf
%     lambda   = x_{k+1}(p) / x_k(p)   where p is index of max element

    n = length(x0);
    x = x0(:);
    lambda = 0;

    fprintf('=== Power Method ===\n');
    fprintf('%-6s %-40s %-15s\n', 'Iter', 'x_k (normalized)', 'lambda');

    for iter = 1:max_iter
        % Multiply
        y = A * x;

        % Find the component with largest absolute value
        [~, p] = max(abs(y));
        lambda_new = y(p);

        % Normalize
        x_new = y / lambda_new;

        % Display
        fprintf('%-6d [', iter);
        fprintf('%.6f ', x_new);
        fprintf(']  lambda = %.8f\n', lambda_new);

        % Check convergence
        if abs(lambda_new - lambda) < tol
            fprintf('\nConverged after %d iterations!\n', iter);
            lambda = lambda_new;
            v = x_new;
            break;
        end

        lambda = lambda_new;
        x = x_new;
    end

    if iter == max_iter
        fprintf('\nWARNING: Did not converge in %d iterations.\n', max_iter);
        v = x_new;
    end

    fprintf('\nDominant eigenvalue: lambda = %.10f\n', lambda);
    fprintf('Eigenvector: [');
    fprintf('%.6f ', v);
    fprintf(']\n');

    % Verify with built-in
    eig_vals = sort(eig(A), 'descend', 'ComparisonMethod', 'abs');
    fprintf('Built-in eigenvalues: '); disp(eig_vals');
end
```

**Test:**
```matlab
A = [2, -1, 0;
     -1, 2, -1;
     0, -1, 2];

x0 = [1; 1; 1];
[lambda, v, iter] = power_method(A, x0, 1e-8, 100);
```

---

## 5.2 Power Method with Rayleigh Quotient

```matlab
function [lambda, v, iter] = power_method_rayleigh(A, x0, tol, max_iter)
% POWER_METHOD_RAYLEIGH Power Method with Rayleigh quotient for eigenvalue
%   lambda = (x' * A * x) / (x' * x)  — gives better convergence

    x = x0(:);
    x = x / norm(x);   % normalize with 2-norm

    fprintf('=== Power Method (Rayleigh Quotient) ===\n');
    fprintf('%-6s %-15s\n', 'Iter', 'lambda');

    lambda_old = 0;

    for iter = 1:max_iter
        y = A * x;
        x = y / norm(y);
        lambda = (x' * A * x) / (x' * x);

        fprintf('%-6d %.12f\n', iter, lambda);

        if abs(lambda - lambda_old) < tol
            fprintf('\nConverged after %d iterations!\n', iter);
            break;
        end
        lambda_old = lambda;
    end

    v = x;
    fprintf('Dominant eigenvalue: %.10f\n', lambda);
    fprintf('Eigenvector: ['); fprintf('%.6f ', v); fprintf(']\n');
end
```

**Test:**
```matlab
A = [6, 5, -5;
     2, 6, -2;
     2, 5, -1];
x0 = [1; 0; 0];
[lam, v, ~] = power_method_rayleigh(A, x0, 1e-10, 100);
fprintf('Exact eigenvalues: '); disp(sort(eig(A))');
```

---

## 5.3 Inverse Power Method (Smallest Eigenvalue)

```matlab
function [lambda, v, iter] = inverse_power_method(A, x0, tol, max_iter)
% INVERSE_POWER_METHOD Find the smallest (magnitude) eigenvalue
%   Applies power method to A^{-1} (without explicitly inverting)
%   At each step solves A * y = x_k

    n = length(x0);
    x = x0(:);
    x = x / norm(x, inf);
    lambda = 0;

    % LU decompose once for efficiency
    [L_mat, U_mat, P_mat] = lu(A);

    fprintf('=== Inverse Power Method ===\n');
    fprintf('%-6s %-15s\n', 'Iter', 'lambda (of A)');

    for iter = 1:max_iter
        % Solve A * y = x  (equivalent to y = A^{-1} * x)
        y = U_mat \ (L_mat \ (P_mat * x));

        % Normalize
        [~, p] = max(abs(y));
        mu = y(p);        % eigenvalue of A^{-1}
        x_new = y / mu;

        lambda_new = 1 / mu;   % eigenvalue of A

        fprintf('%-6d %.10f\n', iter, lambda_new);

        if abs(lambda_new - lambda) < tol
            fprintf('\nConverged after %d iterations!\n', iter);
            lambda = lambda_new;
            v = x_new;
            break;
        end

        lambda = lambda_new;
        x = x_new;
    end

    if iter == max_iter
        v = x_new;
    end

    fprintf('\nSmallest eigenvalue: %.10f\n', lambda);
    fprintf('Eigenvector: ['); fprintf('%.6f ', v); fprintf(']\n');

    eig_vals = sort(eig(A), 'ascend', 'ComparisonMethod', 'abs');
    fprintf('All eigenvalues (sorted by |.|): '); disp(eig_vals');
end
```

**Test:**
```matlab
A = [4, 1, 0;
     1, 3, 1;
     0, 1, 2];
x0 = [1; 1; 1];
[lam, v, ~] = inverse_power_method(A, x0, 1e-10, 100);
```

---

## 5.4 Shifted Inverse Power Method

```matlab
function [lambda, v, iter] = shifted_inverse_power(A, sigma, x0, tol, max_iter)
% SHIFTED_INVERSE_POWER Find eigenvalue closest to shift sigma
%   Apply inverse power method to (A - sigma*I)
%   Converges to eigenvalue nearest to sigma

    n = length(x0);
    B = A - sigma * eye(n);   % shifted matrix
    x = x0(:);
    x = x / norm(x, inf);
    lambda = 0;

    % LU decompose B
    [L_mat, U_mat, P_mat] = lu(B);

    fprintf('=== Shifted Inverse Power Method (sigma = %.4f) ===\n', sigma);
    fprintf('%-6s %-15s %-15s\n', 'Iter', 'mu (shift)', 'lambda (actual)');

    for iter = 1:max_iter
        % Solve (A - sigma*I) * y = x
        y = U_mat \ (L_mat \ (P_mat * x));

        [~, p] = max(abs(y));
        mu = y(p);
        x_new = y / mu;

        lambda_new = 1/mu + sigma;   % actual eigenvalue = 1/mu + sigma

        fprintf('%-6d %-15.10f %-15.10f\n', iter, 1/mu, lambda_new);

        if abs(lambda_new - lambda) < tol
            fprintf('\nConverged after %d iterations!\n', iter);
            lambda = lambda_new;
            v = x_new;
            break;
        end

        lambda = lambda_new;
        x = x_new;
    end

    if iter == max_iter
        v = x_new;
    end

    fprintf('\nEigenvalue closest to sigma=%.4f: lambda = %.10f\n', sigma, lambda);
    fprintf('Eigenvector: ['); fprintf('%.6f ', v); fprintf(']\n');

    eig_vals = eig(A);
    fprintf('All eigenvalues: '); disp(sort(eig_vals)');
end
```

**Test:**
```matlab
A = [6, 5, -5;
     2, 6, -2;
     2, 5, -1];
x0 = [1; 1; 1];

% Find eigenvalue closest to 2
[lam, v, ~] = shifted_inverse_power(A, 2, x0, 1e-10, 100);

% Find eigenvalue closest to 5
[lam2, v2, ~] = shifted_inverse_power(A, 5, x0, 1e-10, 100);
```

---

## 5.5 Convergence Analysis — Power Method

```matlab
%% Convergence rate study
clc; clear; close all;

A = [5, 4, 1, 1;
     4, 5, 1, 1;
     1, 1, 4, 2;
     1, 1, 2, 4];

eig_vals = sort(abs(eig(A)), 'descend');
ratio = eig_vals(2) / eig_vals(1);
fprintf('|lambda_2/lambda_1| = %.6f\n', ratio);
fprintf('Expected convergence rate: %.6f per iteration\n', ratio);

% Run power method and track convergence
x = [1; 0; 0; 0];
lambda_history = zeros(1, 30);
for k = 1:30
    y = A * x;
    [~, p] = max(abs(y));
    lambda_history(k) = y(p);
    x = y / y(p);
end

exact_lambda = max(abs(eig(A)));
errors = abs(lambda_history - exact_lambda);

figure;
semilogy(1:30, errors, 'b-o', 'LineWidth', 2);
xlabel('Iteration'); ylabel('|lambda_k - lambda_exact|');
title(sprintf('Power Method Convergence (|\\lambda_2/\\lambda_1| = %.4f)', ratio));
grid on;

% Add theoretical convergence line
hold on;
theoretical = errors(1) * ratio.^(0:29);
semilogy(1:30, theoretical, 'r--', 'LineWidth', 2);
legend('Actual error', 'Theoretical O(r^k)');
```

---

## 5.6 Gershgorin Circle Theorem — Eigenvalue Bounds

```matlab
function gershgorin_circles(A)
% GERSHGORIN_CIRCLES Plot Gershgorin circles and actual eigenvalues
%   Every eigenvalue lies in at least one Gershgorin disc
%   Center: a_{ii}, Radius: sum of |a_{ij}| for j ≠ i

    n = size(A, 1);
    eig_vals = eig(A);

    fprintf('=== Gershgorin Circles ===\n');
    figure; hold on;
    theta = linspace(0, 2*pi, 100);

    for i = 1:n
        center = A(i, i);
        radius = sum(abs(A(i, :))) - abs(A(i, i));
        fprintf('Disc %d: center = %.4f, radius = %.4f, range = [%.4f, %.4f]\n', ...
            i, center, radius, center-radius, center+radius);

        % Plot circle (in complex plane for general case)
        x_circle = real(center) + radius * cos(theta);
        y_circle = imag(center) + radius * sin(theta);
        fill(x_circle, y_circle, 'b', 'FaceAlpha', 0.1, 'EdgeColor', 'b', 'LineWidth', 2);
    end

    % Plot eigenvalues
    plot(real(eig_vals), imag(eig_vals), 'rx', 'MarkerSize', 15, 'LineWidth', 3);

    xlabel('Real'); ylabel('Imaginary');
    title('Gershgorin Circles');
    grid on; axis equal;
    legend('', '', '', '', 'Eigenvalues');
end
```

**Test:**
```matlab
A = [10, -1, 0;
     0.2, 8, -0.2;
     1, 1, 2];
gershgorin_circles(A);
```

---

## 5.7 Finding All Eigenvalues Using Deflation

```matlab
function eigenvalues = deflation_method(A, tol, max_iter)
% DEFLATION_METHOD Find all eigenvalues using Power Method + Deflation
%   Hotelling's deflation: A_new = A - lambda * v * v' / (v' * v)

    n = size(A, 1);
    eigenvalues = zeros(n, 1);
    A_curr = A;

    fprintf('=== Deflation Method ===\n');

    for k = 1:n
        if size(A_curr, 1) == 1
            eigenvalues(k) = A_curr(1, 1);
            fprintf('Eigenvalue %d: %.10f (1x1 matrix)\n', k, eigenvalues(k));
            break;
        end

        % Power method on current matrix
        m = size(A_curr, 1);
        x = ones(m, 1);
        lambda_old = 0;

        for iter = 1:max_iter
            y = A_curr * x;
            [~, p] = max(abs(y));
            lambda = y(p);
            x = y / lambda;

            if abs(lambda - lambda_old) < tol
                break;
            end
            lambda_old = lambda;
        end

        eigenvalues(k) = lambda;
        fprintf('Eigenvalue %d: %.10f (found in %d iterations)\n', k, lambda, iter);

        % Deflation: remove this eigenvalue
        v = x / norm(x);
        A_curr = A_curr - lambda * (v * v');
    end

    fprintf('\nAll eigenvalues found: '); disp(sort(eigenvalues)');
    fprintf('Built-in eig(A):       '); disp(sort(eig(A))');
end
```

**Test:**
```matlab
A = [4, 1, 0;
     1, 3, 1;
     0, 1, 2];
eigenvalues = deflation_method(A, 1e-10, 200);
```

---

## 5.8 Jacobi Eigenvalue Method (for Symmetric Matrices)

```matlab
function [eigenvalues, V] = jacobi_eigenvalue(A, tol, max_iter)
% JACOBI_EIGENVALUE Find ALL eigenvalues of symmetric matrix using Jacobi rotations
%   Iteratively applies plane rotations to zero out off-diagonal elements

    n = size(A, 1);
    V = eye(n);    % accumulate eigenvectors

    fprintf('=== Jacobi Eigenvalue Method ===\n');

    for iter = 1:max_iter
        % Find largest off-diagonal element
        max_val = 0;
        p = 1; q = 2;
        for i = 1:n
            for j = i+1:n
                if abs(A(i,j)) > max_val
                    max_val = abs(A(i,j));
                    p = i; q = j;
                end
            end
        end

        if max_val < tol
            fprintf('Converged after %d iterations (max off-diag = %.2e)\n', iter, max_val);
            break;
        end

        % Compute rotation angle
        if A(p,p) == A(q,q)
            theta = pi / 4;
        else
            theta = 0.5 * atan(2 * A(p,q) / (A(p,p) - A(q,q)));
        end

        c = cos(theta);
        s = sin(theta);

        % Apply Givens rotation: A_new = G' * A * G
        A_new = A;
        for i = 1:n
            if i ~= p && i ~= q
                A_new(i, p) = c * A(i, p) + s * A(i, q);
                A_new(p, i) = A_new(i, p);
                A_new(i, q) = -s * A(i, p) + c * A(i, q);
                A_new(q, i) = A_new(i, q);
            end
        end
        A_new(p, p) = c^2 * A(p,p) + 2*s*c * A(p,q) + s^2 * A(q,q);
        A_new(q, q) = s^2 * A(p,p) - 2*s*c * A(p,q) + c^2 * A(q,q);
        A_new(p, q) = 0;
        A_new(q, p) = 0;

        A = A_new;

        % Accumulate eigenvectors
        V_new = V;
        V_new(:, p) = c * V(:, p) + s * V(:, q);
        V_new(:, q) = -s * V(:, p) + c * V(:, q);
        V = V_new;

        if mod(iter, 10) == 0
            fprintf('  iter %d: max off-diag = %.2e\n', iter, max_val);
        end
    end

    eigenvalues = diag(A);
    fprintf('Eigenvalues: '); disp(sort(eigenvalues)');
    fprintf('Built-in:    '); disp(sort(eig(A))');
end
```

**Test:**
```matlab
A = [4, -30, 60, -35;
     -30, 300, -675, 420;
     60, -675, 1620, -1050;
     -35, 420, -1050, 700];

[evals, evecs] = jacobi_eigenvalue(A, 1e-10, 200);
```

---
