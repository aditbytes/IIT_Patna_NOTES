# Module 1: Numerical Integration — MATLAB Codes
## CDA 106 | Lectures 1–6

---

## 1.1 Trapezoidal Rule (Single Interval)

```matlab
function I = trapezoidal(f, a, b)
% TRAPEZOIDAL Single-interval Trapezoidal Rule
% I = trapezoidal(f, a, b)
%   f - function handle
%   a - lower limit
%   b - upper limit
    h = b - a;
    I = (h / 2) * (f(a) + f(b));
end
```

**Test:**
```matlab
f = @(x) x.^2;
I = trapezoidal(f, 0, 1);
fprintf('Trapezoidal: %.6f\n', I);       % 0.500000
fprintf('Exact:       %.6f\n', 1/3);     % 0.333333
fprintf('Error:       %.6f\n', abs(I - 1/3));
```

---

## 1.2 Composite Trapezoidal Rule

```matlab
function I = composite_trapezoidal(f, a, b, n)
% COMPOSITE_TRAPEZOIDAL Composite Trapezoidal Rule
% I = composite_trapezoidal(f, a, b, n)
%   f - function handle
%   a - lower limit
%   b - upper limit
%   n - number of subintervals
    h = (b - a) / n;
    x = a:h:b;         % n+1 points
    y = f(x);
    I = (h / 2) * (y(1) + 2 * sum(y(2:end-1)) + y(end));
end
```

**Test:**
```matlab
f = @(x) sin(x);
I = composite_trapezoidal(f, 0, pi, 10);
fprintf('Composite Trapezoidal (n=10): %.10f\n', I);
fprintf('Exact:                        %.10f\n', 2.0);
fprintf('Error:                        %.2e\n', abs(I - 2.0));

% Error vs n plot
n_vals = [2, 4, 8, 16, 32, 64, 128];
errors = zeros(size(n_vals));
for k = 1:length(n_vals)
    I_approx = composite_trapezoidal(f, 0, pi, n_vals(k));
    errors(k) = abs(I_approx - 2.0);
end
loglog(n_vals, errors, 'b-o', 'LineWidth', 2);
xlabel('n'); ylabel('|Error|');
title('Composite Trapezoidal Rule — Error vs n');
grid on;
```

---

## 1.3 Simpson's 1/3 Rule (Single Interval)

```matlab
function I = simpson13(f, a, b)
% SIMPSON13 Simpson's 1/3 Rule (single interval, 3 points)
% I = simpson13(f, a, b)
    h = (b - a) / 2;
    x0 = a;
    x1 = a + h;
    x2 = b;
    I = (h / 3) * (f(x0) + 4 * f(x1) + f(x2));
end
```

**Test:**
```matlab
f = @(x) exp(x);
I = simpson13(f, 0, 1);
exact = exp(1) - 1;
fprintf('Simpson 1/3: %.10f\n', I);
fprintf('Exact:       %.10f\n', exact);
fprintf('Error:       %.2e\n', abs(I - exact));
```

---

## 1.4 Composite Simpson's 1/3 Rule

```matlab
function I = composite_simpson13(f, a, b, n)
% COMPOSITE_SIMPSON13 Composite Simpson's 1/3 Rule
% I = composite_simpson13(f, a, b, n)
%   n must be EVEN
    if mod(n, 2) ~= 0
        error('n must be even for Simpson''s 1/3 rule');
    end
    h = (b - a) / n;
    x = a:h:b;
    y = f(x);

    % Odd-indexed interior points (1-based: indices 2,4,6,...)
    odd_sum = sum(y(2:2:end-1));
    % Even-indexed interior points (1-based: indices 3,5,7,...)
    even_sum = sum(y(3:2:end-2));

    I = (h / 3) * (y(1) + 4 * odd_sum + 2 * even_sum + y(end));
end
```

**Test:**
```matlab
f = @(x) 1 ./ x;
I = composite_simpson13(f, 1, 2, 10);
exact = log(2);
fprintf('Composite Simpson 1/3 (n=10): %.10f\n', I);
fprintf('Exact (ln 2):                 %.10f\n', exact);
fprintf('Error:                        %.2e\n', abs(I - exact));
```

---

## 1.5 Simpson's 3/8 Rule (Single Interval)

```matlab
function I = simpson38(f, a, b)
% SIMPSON38 Simpson's 3/8 Rule (single interval, 4 points)
% I = simpson38(f, a, b)
    h = (b - a) / 3;
    x0 = a;
    x1 = a + h;
    x2 = a + 2*h;
    x3 = b;
    I = (3 * h / 8) * (f(x0) + 3*f(x1) + 3*f(x2) + f(x3));
end
```

**Test:**
```matlab
f = @(x) x.^3;
I = simpson38(f, 0, 1);
exact = 0.25;
fprintf('Simpson 3/8: %.10f\n', I);
fprintf('Exact:       %.10f\n', exact);
fprintf('Error:       %.2e\n', abs(I - exact));
```

---

## 1.6 Composite Simpson's 3/8 Rule

```matlab
function I = composite_simpson38(f, a, b, n)
% COMPOSITE_SIMPSON38 Composite Simpson's 3/8 Rule
% n must be a multiple of 3
    if mod(n, 3) ~= 0
        error('n must be a multiple of 3 for Simpson''s 3/8 rule');
    end
    h = (b - a) / n;
    x = a:h:b;
    y = f(x);

    I = 0;
    for i = 1:3:n
        I = I + (3*h/8) * (y(i) + 3*y(i+1) + 3*y(i+2) + y(i+3));
    end
end
```

**Test:**
```matlab
f = @(x) exp(-x.^2);
I = composite_simpson38(f, 0, 1, 9);
exact = 0.746824132812427;  % from integral tables
fprintf('Composite Simpson 3/8 (n=9): %.10f\n', I);
fprintf('Reference:                   %.10f\n', exact);
```

---

## 1.7 Error Comparison — All Newton-Cotes Methods

```matlab
%% Compare Trapezoidal, Simpson 1/3, Simpson 3/8
f = @(x) sin(x);
a = 0; b = pi;
exact = 2.0;

n_vals = 6:6:60;   % multiples of 6 so all methods work
err_trap = zeros(size(n_vals));
err_s13  = zeros(size(n_vals));
err_s38  = zeros(size(n_vals));

for k = 1:length(n_vals)
    n = n_vals(k);
    err_trap(k) = abs(composite_trapezoidal(f, a, b, n) - exact);
    err_s13(k)  = abs(composite_simpson13(f, a, b, n) - exact);
    err_s38(k)  = abs(composite_simpson38(f, a, b, n) - exact);
end

figure;
loglog(n_vals, err_trap, 'r-o', 'LineWidth', 2); hold on;
loglog(n_vals, err_s13,  'b-s', 'LineWidth', 2);
loglog(n_vals, err_s38,  'g-^', 'LineWidth', 2);
xlabel('Number of subintervals (n)');
ylabel('|Error|');
title('Newton-Cotes Methods — Error Comparison');
legend('Trapezoidal', 'Simpson 1/3', 'Simpson 3/8');
grid on;
```

---

## 1.8 Gauss-Legendre 2-Point Quadrature

```matlab
function I = gauss_legendre_2pt(f, a, b)
% GAUSS_LEGENDRE_2PT 2-point Gauss-Legendre Quadrature on [a,b]
%   Exact for polynomials up to degree 3

    % Nodes and weights on [-1, 1]
    t = [-1/sqrt(3), 1/sqrt(3)];
    w = [1, 1];

    % Transform from [-1,1] to [a,b]
    % x = ((b-a)*t + (b+a)) / 2
    % dx = (b-a)/2 * dt
    x = ((b - a) * t + (b + a)) / 2;
    I = (b - a) / 2 * sum(w .* f(x));
end
```

**Test:**
```matlab
f = @(x) x.^2 + 1;
I = gauss_legendre_2pt(f, 0, 1);
exact = 1/3 + 1;   % = 4/3
fprintf('GL 2-pt: %.10f\n', I);
fprintf('Exact:   %.10f\n', exact);
fprintf('Error:   %.2e\n', abs(I - exact));
```

---

## 1.9 Gauss-Legendre 3-Point Quadrature

```matlab
function I = gauss_legendre_3pt(f, a, b)
% GAUSS_LEGENDRE_3PT 3-point Gauss-Legendre Quadrature on [a,b]
%   Exact for polynomials up to degree 5

    % Nodes and weights on [-1, 1]
    t = [-sqrt(3/5), 0, sqrt(3/5)];
    w = [5/9, 8/9, 5/9];

    % Transform to [a, b]
    x = ((b - a) * t + (b + a)) / 2;
    I = (b - a) / 2 * sum(w .* f(x));
end
```

**Test:**
```matlab
f = @(x) x.^4;
I = gauss_legendre_3pt(f, -1, 1);
exact = 2/5;
fprintf('GL 3-pt: %.10f\n', I);
fprintf('Exact:   %.10f\n', exact);
fprintf('Error:   %.2e\n', abs(I - exact));

% Test on [0, pi]
f2 = @(x) sin(x);
I2 = gauss_legendre_3pt(f2, 0, pi);
fprintf('sin(x) on [0,pi]: GL3=%.10f, Exact=%.10f\n', I2, 2.0);
```

---

## 1.10 Gauss-Legendre n-Point (General)

```matlab
function I = gauss_legendre_npt(f, a, b, n)
% GAUSS_LEGENDRE_NPT n-point Gauss-Legendre Quadrature on [a,b]
%   Uses the Golub-Welsch algorithm to find nodes and weights

    % Compute nodes and weights on [-1, 1]
    [t, w] = gauss_legendre_nodes_weights(n);

    % Transform to [a, b]
    x = ((b - a) * t + (b + a)) / 2;
    I = (b - a) / 2 * sum(w .* f(x));
end

function [x, w] = gauss_legendre_nodes_weights(n)
% Compute Gauss-Legendre nodes and weights using eigenvalue method
    beta = (1:n-1) ./ sqrt(4*(1:n-1).^2 - 1);
    J = diag(beta, 1) + diag(beta, -1);   % Jacobi matrix
    [V, D] = eig(J);
    x = diag(D)';       % nodes
    w = 2 * V(1,:).^2;  % weights

    % Sort by node position
    [x, idx] = sort(x);
    w = w(idx);
end
```

**Test:**
```matlab
f = @(x) exp(x);
exact = exp(1) - 1;

fprintf('Gauss-Legendre on [0,1] for e^x:\n');
for n = 2:6
    I = gauss_legendre_npt(f, 0, 1, n);
    fprintf('  n=%d: I=%.12f, Error=%.2e\n', n, I, abs(I - exact));
end
```

---

## 1.11 Gauss-Hermite 2-Point Quadrature

```matlab
function I = gauss_hermite_2pt(f)
% GAUSS_HERMITE_2PT 2-point Gauss-Hermite Quadrature
%   Approximates integral of f(x)*exp(-x^2) from -inf to +inf
%   Exact for polynomials up to degree 3

    % Nodes and weights
    x = [-1/sqrt(2), 1/sqrt(2)];
    w = [sqrt(pi)/2, sqrt(pi)/2];

    I = sum(w .* f(x));
end
```

**Test:**
```matlab
% Integral of x^2 * exp(-x^2) from -inf to +inf = sqrt(pi)/2
f = @(x) x.^2;
I = gauss_hermite_2pt(f);
exact = sqrt(pi) / 2;
fprintf('GH 2-pt for x^2: %.10f\n', I);
fprintf('Exact:           %.10f\n', exact);
fprintf('Error:           %.2e\n', abs(I - exact));
```

---

## 1.12 Gauss-Hermite 3-Point Quadrature

```matlab
function I = gauss_hermite_3pt(f)
% GAUSS_HERMITE_3PT 3-point Gauss-Hermite Quadrature
%   Approximates integral of f(x)*exp(-x^2) from -inf to +inf
%   Exact for polynomials up to degree 5

    % Nodes and weights
    x = [-sqrt(3/2), 0, sqrt(3/2)];
    w = [sqrt(pi)/6, 2*sqrt(pi)/3, sqrt(pi)/6];

    I = sum(w .* f(x));
end
```

**Test:**
```matlab
% Integral of x^4 * exp(-x^2) from -inf to +inf = 3*sqrt(pi)/4
f = @(x) x.^4;
I = gauss_hermite_3pt(f);
exact = 3 * sqrt(pi) / 4;
fprintf('GH 3-pt for x^4: %.10f\n', I);
fprintf('Exact:           %.10f\n', exact);
fprintf('Error:           %.2e\n', abs(I - exact));
```

---

## 1.13 Gauss-Hermite n-Point (General)

```matlab
function I = gauss_hermite_npt(f, n)
% GAUSS_HERMITE_NPT n-point Gauss-Hermite Quadrature
%   Approximates integral of f(x)*exp(-x^2) from -inf to +inf

    [x, w] = gauss_hermite_nodes_weights(n);
    I = sum(w .* f(x));
end

function [x, w] = gauss_hermite_nodes_weights(n)
% Compute Gauss-Hermite nodes and weights using eigenvalue method
    beta = sqrt((1:n-1) / 2);
    J = diag(beta, 1) + diag(beta, -1);
    [V, D] = eig(J);
    x = diag(D)';
    w = sqrt(pi) * V(1,:).^2;

    [x, idx] = sort(x);
    w = w(idx);
end
```

**Test:**
```matlab
f = @(x) x.^6;
% Exact: integral of x^6 * exp(-x^2) = 15*sqrt(pi)/8
exact = 15 * sqrt(pi) / 8;
for n = 2:5
    I = gauss_hermite_npt(f, n);
    fprintf('GH n=%d: I=%.10f, Error=%.2e\n', n, I, abs(I - exact));
end
```

---

## 1.14 Gauss-Laguerre Quadrature

```matlab
function I = gauss_laguerre_npt(f, n)
% GAUSS_LAGUERRE_NPT n-point Gauss-Laguerre Quadrature
%   Approximates integral of f(x)*exp(-x) from 0 to +inf

    [x, w] = gauss_laguerre_nodes_weights(n);
    I = sum(w .* f(x));
end

function [x, w] = gauss_laguerre_nodes_weights(n)
% Nodes and weights via eigenvalue method
    i = 1:n-1;
    a = 2*(1:n) - 1;   % diagonal
    b = i;              % off-diagonal
    J = diag(a) + diag(sqrt(b), 1) + diag(sqrt(b), -1);
    [V, D] = eig(J);
    x = diag(D)';
    w = V(1,:).^2;

    [x, idx] = sort(x);
    w = w(idx);
end
```

**Test:**
```matlab
% Integral of x^2 * exp(-x) from 0 to inf = 2
f = @(x) x.^2;
I = gauss_laguerre_npt(f, 3);
fprintf('Gauss-Laguerre (n=3) for x^2: %.10f (exact: 2)\n', I);
```

---

## 1.15 Interval Transformation Utility

```matlab
function [x_new, jacobian] = transform_interval(t, a, b)
% TRANSFORM_INTERVAL Map from [-1,1] to [a,b]
%   t       - points in [-1, 1]
%   a, b    - target interval
%   x_new   - transformed points in [a, b]
%   jacobian - dx/dt = (b-a)/2

    x_new = ((b - a) * t + (b + a)) / 2;
    jacobian = (b - a) / 2;
end
```

**Example usage:**
```matlab
% Evaluate integral of cos(x) on [0, pi/2] using GL 3-pt
[t, w] = gauss_legendre_nodes_weights(3);
[x, jac] = transform_interval(t, 0, pi/2);

f = @(x) cos(x);
I = jac * sum(w .* f(x));
fprintf('cos(x) on [0,pi/2]: %.10f (exact: 1)\n', I);
```

---

## 1.16 Composite Gauss-Legendre Quadrature

```matlab
function I = composite_gauss_legendre(f, a, b, n_sub, n_pts)
% COMPOSITE_GAUSS_LEGENDRE Composite Gauss-Legendre
%   n_sub - number of subintervals
%   n_pts - number of GL points per subinterval

    h = (b - a) / n_sub;
    [t, w] = gauss_legendre_nodes_weights(n_pts);
    I = 0;
    for k = 0:n_sub-1
        ak = a + k * h;
        bk = a + (k + 1) * h;
        x = ((bk - ak) * t + (bk + ak)) / 2;
        I = I + (bk - ak) / 2 * sum(w .* f(x));
    end
end
```

**Test:**
```matlab
f = @(x) exp(-x.^2);
I = composite_gauss_legendre(f, 0, 3, 10, 3);
exact = 0.886226925452758;  % erf(3)*sqrt(pi)/2
fprintf('Composite GL: %.10f\n', I);
```

---

## 1.17 Full Comparison Script — All Methods

```matlab
%% MASTER COMPARISON: All Numerical Integration Methods
clc; clear; close all;

f = @(x) exp(-x.^2);
a = 0; b = 2;
exact = 0.882081390762421;  % integral(exp(-x^2), 0, 2)

fprintf('=== Numerical Integration Comparison ===\n');
fprintf('Integral of exp(-x^2) from 0 to 2\n');
fprintf('Exact value: %.12f\n\n', exact);

% Single-interval methods
I1 = trapezoidal(f, a, b);
I2 = simpson13(f, a, b);
I3 = simpson38(f, a, b);
I4 = gauss_legendre_2pt(f, a, b);
I5 = gauss_legendre_3pt(f, a, b);

fprintf('--- Single Interval ---\n');
fprintf('Trapezoidal:  %.10f  Error: %.2e\n', I1, abs(I1-exact));
fprintf('Simpson 1/3:  %.10f  Error: %.2e\n', I2, abs(I2-exact));
fprintf('Simpson 3/8:  %.10f  Error: %.2e\n', I3, abs(I3-exact));
fprintf('GL 2-pt:      %.10f  Error: %.2e\n', I4, abs(I4-exact));
fprintf('GL 3-pt:      %.10f  Error: %.2e\n', I5, abs(I5-exact));

% Composite methods (n = 10)
n = 10;
I6 = composite_trapezoidal(f, a, b, n);
I7 = composite_simpson13(f, a, b, n);
I8 = gauss_legendre_npt(f, a, b, 5);

fprintf('\n--- Composite (n=10) ---\n');
fprintf('Comp. Trapezoidal: %.10f  Error: %.2e\n', I6, abs(I6-exact));
fprintf('Comp. Simpson 1/3: %.10f  Error: %.2e\n', I7, abs(I7-exact));

% GL with increasing points
fprintf('\n--- Gauss-Legendre (increasing n) ---\n');
for np = 2:8
    I = gauss_legendre_npt(f, a, b, np);
    fprintf('  GL n=%d: %.12f  Error: %.2e\n', np, I, abs(I-exact));
end
```

---
