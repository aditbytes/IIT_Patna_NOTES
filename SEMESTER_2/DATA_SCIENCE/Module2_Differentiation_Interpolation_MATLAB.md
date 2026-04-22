# Module 2: Numerical Differentiation & Interpolation — MATLAB Codes
## CDA 106 | Lectures 7–8

---

## 2.1 Forward Difference Table Generator

```matlab
function T = forward_diff_table(x, y)
% FORWARD_DIFF_TABLE Build complete forward difference table
%   x - vector of equally spaced x values
%   y - vector of corresponding f(x) values
%   T - difference table matrix (T(:,1)=y, T(:,2)=Δy, T(:,3)=Δ²y, ...)

    n = length(y);
    T = zeros(n, n);
    T(:, 1) = y(:);

    for j = 2:n
        for i = 1:(n - j + 1)
            T(i, j) = T(i+1, j-1) - T(i, j-1);
        end
    end

    % Display
    fprintf('Forward Difference Table:\n');
    fprintf('%-10s', 'x', 'f(x)');
    for j = 2:n
        fprintf('%-12s', sprintf('Δ^%d f', j-1));
    end
    fprintf('\n');
    for i = 1:n
        fprintf('%-10.4f', x(i));
        for j = 1:n-i+1
            fprintf('%-12.6f', T(i,j));
        end
        fprintf('\n');
    end
end
```

**Test:**
```matlab
x = [0, 1, 2, 3, 4];
y = [1, 2, 4, 8, 16];
T = forward_diff_table(x, y);
```

---

## 2.2 Backward Difference Table Generator

```matlab
function T = backward_diff_table(x, y)
% BACKWARD_DIFF_TABLE Build complete backward difference table
%   T - table where T(i,j) = ∇^(j-1) f at x(i)

    n = length(y);
    T = zeros(n, n);
    T(:, 1) = y(:);

    for j = 2:n
        for i = j:n
            T(i, j) = T(i, j-1) - T(i-1, j-1);
        end
    end

    % Display
    fprintf('Backward Difference Table:\n');
    fprintf('%-10s', 'x', 'f(x)');
    for j = 2:n
        fprintf('%-12s', sprintf('∇^%d f', j-1));
    end
    fprintf('\n');
    for i = 1:n
        fprintf('%-10.4f', x(i));
        for j = 1:i
            fprintf('%-12.6f', T(i,j));
        end
        fprintf('\n');
    end
end
```

**Test:**
```matlab
x = [1.0, 1.1, 1.2, 1.3, 1.4];
y = [0.8415, 0.8912, 0.9320, 0.9636, 0.9854];  % sin(x)
T = backward_diff_table(x, y);
```

---

## 2.3 First Derivative — Forward Difference

```matlab
function dfdx = deriv_forward(f, x, h)
% DERIV_FORWARD First derivative using forward difference
%   O(h) formula: f'(x) ≈ (f(x+h) - f(x)) / h
    dfdx = (f(x + h) - f(x)) / h;
end

function dfdx = deriv_forward_2nd_order(f, x, h)
% Second-order accurate forward difference
%   f'(x) ≈ (-3f(x) + 4f(x+h) - f(x+2h)) / (2h)
    dfdx = (-3*f(x) + 4*f(x+h) - f(x+2*h)) / (2*h);
end
```

**Test:**
```matlab
f = @(x) sin(x);
x0 = pi/4;
exact = cos(pi/4);

h_vals = [0.1, 0.01, 0.001, 0.0001];
fprintf('Forward Difference for sin''(pi/4):\n');
fprintf('%-12s %-15s %-15s %-15s\n', 'h', 'O(h)', 'O(h^2)', 'Exact');
for h = h_vals
    d1 = deriv_forward(f, x0, h);
    d2 = deriv_forward_2nd_order(f, x0, h);
    fprintf('%-12.4e %-15.10f %-15.10f %-15.10f\n', h, d1, d2, exact);
end
```

---

## 2.4 First Derivative — Backward Difference

```matlab
function dfdx = deriv_backward(f, x, h)
% DERIV_BACKWARD First derivative using backward difference
%   O(h) formula: f'(x) ≈ (f(x) - f(x-h)) / h
    dfdx = (f(x) - f(x - h)) / h;
end

function dfdx = deriv_backward_2nd_order(f, x, h)
% Second-order accurate backward difference
%   f'(x) ≈ (3f(x) - 4f(x-h) + f(x-2h)) / (2h)
    dfdx = (3*f(x) - 4*f(x-h) + f(x-2*h)) / (2*h);
end
```

**Test:**
```matlab
f = @(x) exp(x);
x0 = 1;
exact = exp(1);

h = 0.01;
d1 = deriv_backward(f, x0, h);
d2 = deriv_backward_2nd_order(f, x0, h);
fprintf('Backward diff O(h):   %.10f, Error: %.2e\n', d1, abs(d1-exact));
fprintf('Backward diff O(h^2): %.10f, Error: %.2e\n', d2, abs(d2-exact));
```

---

## 2.5 First Derivative — Central Difference

```matlab
function dfdx = deriv_central(f, x, h)
% DERIV_CENTRAL First derivative using central difference
%   O(h^2) formula: f'(x) ≈ (f(x+h) - f(x-h)) / (2h)
    dfdx = (f(x + h) - f(x - h)) / (2 * h);
end

function dfdx = deriv_central_4th_order(f, x, h)
% Fourth-order accurate central difference
%   f'(x) ≈ (-f(x+2h) + 8f(x+h) - 8f(x-h) + f(x-2h)) / (12h)
    dfdx = (-f(x+2*h) + 8*f(x+h) - 8*f(x-h) + f(x-2*h)) / (12*h);
end
```

**Test:**
```matlab
f = @(x) sin(x);
x0 = pi/6;
exact = cos(pi/6);

fprintf('Central Difference for sin''(pi/6):\n');
for h = [0.1, 0.01, 0.001]
    d2 = deriv_central(f, x0, h);
    d4 = deriv_central_4th_order(f, x0, h);
    fprintf('h=%.3f: O(h^2)=%.10f  O(h^4)=%.10f  Exact=%.10f\n', h, d2, d4, exact);
end
```

---

## 2.6 Second Derivative — Central Difference

```matlab
function d2fdx2 = deriv2_central(f, x, h)
% DERIV2_CENTRAL Second derivative using central difference
%   O(h^2): f''(x) ≈ (f(x+h) - 2f(x) + f(x-h)) / h^2
    d2fdx2 = (f(x + h) - 2*f(x) + f(x - h)) / h^2;
end

function d2fdx2 = deriv2_central_4th_order(f, x, h)
% Fourth-order accurate second derivative
%   f''(x) ≈ (-f(x+2h) + 16f(x+h) - 30f(x) + 16f(x-h) - f(x-2h)) / (12h^2)
    d2fdx2 = (-f(x+2*h) + 16*f(x+h) - 30*f(x) + 16*f(x-h) - f(x-2*h)) / (12*h^2);
end
```

**Test:**
```matlab
f = @(x) sin(x);
x0 = pi/4;
exact = -sin(pi/4);   % f''(x) = -sin(x)

h = 0.01;
d2 = deriv2_central(f, x0, h);
d4 = deriv2_central_4th_order(f, x0, h);
fprintf('2nd deriv O(h^2): %.10f, Error: %.2e\n', d2, abs(d2-exact));
fprintf('2nd deriv O(h^4): %.10f, Error: %.2e\n', d4, abs(d4-exact));
```

---

## 2.7 Differentiation from Tabulated Data

```matlab
function [df, d2f] = diff_from_table(x, y, x0)
% DIFF_FROM_TABLE Estimate derivatives from tabulated data
%   Uses forward/backward/central difference as appropriate
%   x  - equally spaced x values
%   y  - f(x) values
%   x0 - point at which to estimate derivative

    h = x(2) - x(1);
    n = length(x);
    idx = find(x == x0);

    if isempty(idx)
        error('x0 not in the data set');
    end

    i = idx;

    % First derivative
    if i == 1
        % Forward difference
        df = (-3*y(i) + 4*y(i+1) - y(i+2)) / (2*h);
    elseif i == n
        % Backward difference
        df = (3*y(i) - 4*y(i-1) + y(i-2)) / (2*h);
    else
        % Central difference
        df = (y(i+1) - y(i-1)) / (2*h);
    end

    % Second derivative
    if i == 1
        d2f = (y(i) - 2*y(i+1) + y(i+2)) / h^2;
    elseif i == n
        d2f = (y(i) - 2*y(i-1) + y(i-2)) / h^2;
    else
        d2f = (y(i+1) - 2*y(i) + y(i-1)) / h^2;
    end

    fprintf('At x = %.4f:\n', x0);
    fprintf('  f''(x)  ≈ %.8f\n', df);
    fprintf('  f''''(x) ≈ %.8f\n', d2f);
end
```

**Test:**
```matlab
x = 0:0.1:1;
y = sin(x);
[df, d2f] = diff_from_table(x, y, 0.5);
fprintf('Exact f''(0.5)  = %.8f\n', cos(0.5));
fprintf('Exact f''''(0.5) = %.8f\n', -sin(0.5));
```

---

## 2.8 Newton-Gregory Forward Interpolation

```matlab
function P = newton_gregory_forward(x_data, y_data, x_eval)
% NEWTON_GREGORY_FORWARD Newton-Gregory Forward Interpolation
%   x_data - equally spaced data points
%   y_data - function values at x_data
%   x_eval - point(s) at which to interpolate

    n = length(x_data);
    h = x_data(2) - x_data(1);

    % Build forward difference table
    D = zeros(n, n);
    D(:, 1) = y_data(:);
    for j = 2:n
        for i = 1:(n - j + 1)
            D(i, j) = D(i+1, j-1) - D(i, j-1);
        end
    end

    % Evaluate polynomial at x_eval
    P = zeros(size(x_eval));
    for k = 1:length(x_eval)
        s = (x_eval(k) - x_data(1)) / h;
        val = D(1, 1);
        s_prod = 1;
        for j = 1:n-1
            s_prod = s_prod * (s - (j - 1)) / j;
            val = val + s_prod * D(1, j+1);
        end
        P(k) = val;
    end
end
```

**Test:**
```matlab
x_data = [0, 1, 2, 3, 4];
y_data = [1, 1, 2, 4, 8];

% Interpolate at x = 1.5
x_eval = 1.5;
P = newton_gregory_forward(x_data, y_data, x_eval);
fprintf('P(%.1f) = %.6f\n', x_eval, P);

% Plot
xx = linspace(0, 4, 100);
yy = newton_gregory_forward(x_data, y_data, xx);
figure;
plot(x_data, y_data, 'ro', 'MarkerSize', 10, 'LineWidth', 2); hold on;
plot(xx, yy, 'b-', 'LineWidth', 2);
xlabel('x'); ylabel('y');
title('Newton-Gregory Forward Interpolation');
legend('Data', 'Interpolant');
grid on;
```

---

## 2.9 Newton-Gregory Backward Interpolation

```matlab
function P = newton_gregory_backward(x_data, y_data, x_eval)
% NEWTON_GREGORY_BACKWARD Newton-Gregory Backward Interpolation
%   Best for interpolation near the END of the data table

    n = length(x_data);
    h = x_data(2) - x_data(1);

    % Build backward difference table
    D = zeros(n, n);
    D(:, 1) = y_data(:);
    for j = 2:n
        for i = j:n
            D(i, j) = D(i, j-1) - D(i-1, j-1);
        end
    end

    % Evaluate using backward formula
    P = zeros(size(x_eval));
    for k = 1:length(x_eval)
        s = (x_eval(k) - x_data(end)) / h;   % s is usually negative
        val = D(n, 1);
        s_prod = 1;
        for j = 1:n-1
            s_prod = s_prod * (s + (j - 1)) / j;
            val = val + s_prod * D(n, j+1);
        end
        P(k) = val;
    end
end
```

**Test:**
```matlab
x_data = [1.0, 1.1, 1.2, 1.3, 1.4];
y_data = log(x_data);   % ln(x)

% Interpolate near end: x = 1.35
x_eval = 1.35;
P = newton_gregory_backward(x_data, y_data, x_eval);
fprintf('P(%.2f) = %.10f\n', x_eval, P);
fprintf('Exact:    %.10f\n', log(1.35));
fprintf('Error:    %.2e\n', abs(P - log(1.35)));
```

---

## 2.10 Newton's Divided Difference Interpolation

```matlab
function [P, dd_table] = newton_divided_diff(x_data, y_data, x_eval)
% NEWTON_DIVIDED_DIFF Newton's Divided Difference Interpolation
%   Works for UNEQUALLY spaced data

    n = length(x_data);

    % Build divided difference table
    dd_table = zeros(n, n);
    dd_table(:, 1) = y_data(:);

    for j = 2:n
        for i = 1:(n - j + 1)
            dd_table(i, j) = (dd_table(i+1, j-1) - dd_table(i, j-1)) / ...
                             (x_data(i + j - 1) - x_data(i));
        end
    end

    % Display divided difference table
    fprintf('Divided Difference Table:\n');
    fprintf('%-10s %-12s', 'x', 'f[x]');
    for j = 2:n
        fprintf('%-14s', sprintf('f[%d-th]', j-1));
    end
    fprintf('\n');
    for i = 1:n
        fprintf('%-10.4f', x_data(i));
        for j = 1:n-i+1
            fprintf('%-14.8f', dd_table(i,j));
        end
        fprintf('\n');
    end

    % Evaluate P(x) = f[x0] + f[x0,x1](x-x0) + f[x0,x1,x2](x-x0)(x-x1) + ...
    P = zeros(size(x_eval));
    for k = 1:length(x_eval)
        val = dd_table(1, 1);
        prod_term = 1;
        for j = 2:n
            prod_term = prod_term * (x_eval(k) - x_data(j-1));
            val = val + dd_table(1, j) * prod_term;
        end
        P(k) = val;
    end
end
```

**Test:**
```matlab
% Unequally spaced data
x_data = [0, 1, 3, 5, 7];
y_data = [1, 3, 49, 273, 823];   % y = x^3 + 2x + 1

x_eval = 2;
[P, dd] = newton_divided_diff(x_data, y_data, x_eval);
exact = 2^3 + 2*2 + 1;
fprintf('\nP(%.1f) = %.6f\n', x_eval, P);
fprintf('Exact:   %.6f\n', exact);
fprintf('Error:   %.2e\n', abs(P - exact));
```

---

## 2.11 Lagrange Interpolation

```matlab
function P = lagrange_interp(x_data, y_data, x_eval)
% LAGRANGE_INTERP Lagrange Interpolation
%   Works for any spacing (equally or unequally spaced)
%
%   P(x) = sum_{i=0}^{n} y_i * L_i(x)
%   L_i(x) = prod_{j≠i} (x - x_j) / (x_i - x_j)

    n = length(x_data);
    P = zeros(size(x_eval));

    for k = 1:length(x_eval)
        xv = x_eval(k);
        val = 0;
        for i = 1:n
            Li = 1;
            for j = 1:n
                if j ~= i
                    Li = Li * (xv - x_data(j)) / (x_data(i) - x_data(j));
                end
            end
            val = val + y_data(i) * Li;
        end
        P(k) = val;
    end
end
```

**Test:**
```matlab
% 4 data points
x_data = [0, 1, 2, 3];
y_data = [1, 2.7183, 7.3891, 20.0855];  % e^x

x_eval = [0.5, 1.5, 2.5];
P = lagrange_interp(x_data, y_data, x_eval);
exact = exp(x_eval);

fprintf('Lagrange Interpolation:\n');
for k = 1:length(x_eval)
    fprintf('  P(%.1f) = %.6f, Exact = %.6f, Error = %.2e\n', ...
        x_eval(k), P(k), exact(k), abs(P(k) - exact(k)));
end

% Plot
xx = linspace(0, 3, 200);
yy = lagrange_interp(x_data, y_data, xx);
figure;
plot(x_data, y_data, 'ro', 'MarkerSize', 10, 'LineWidth', 2); hold on;
plot(xx, yy, 'b-', 'LineWidth', 2);
plot(xx, exp(xx), 'k--', 'LineWidth', 1.5);
xlabel('x'); ylabel('y');
title('Lagrange Interpolation vs e^x');
legend('Data', 'Lagrange', 'Exact e^x');
grid on;
```

---

## 2.12 Richardson Extrapolation for Derivatives

```matlab
function df_rich = richardson_extrapolation(f, x, h, order)
% RICHARDSON_EXTRAPOLATION Improve derivative estimate
%   Uses two central difference estimates with h and h/2
%   order - order of base method (2 for central diff)

    D1 = deriv_central(f, x, h);
    D2 = deriv_central(f, x, h/2);

    % Richardson formula: D_improved = (2^p * D2 - D1) / (2^p - 1)
    p = order;  % order of leading error term
    df_rich = (2^p * D2 - D1) / (2^p - 1);
end
```

**Test:**
```matlab
f = @(x) sin(x);
x0 = 1;
exact = cos(1);
h = 0.1;

d_central = deriv_central(f, x0, h);
d_rich = richardson_extrapolation(f, x0, h, 2);

fprintf('Central diff:   %.10f, Error: %.2e\n', d_central, abs(d_central-exact));
fprintf('Richardson:     %.10f, Error: %.2e\n', d_rich, abs(d_rich-exact));
fprintf('Exact:          %.10f\n', exact);
```

---

## 2.13 Convergence Study — All Differentiation Methods

```matlab
%% Convergence comparison for all differentiation formulas
clc; clear; close all;

f = @(x) exp(x);
x0 = 1;
exact = exp(1);

h_vals = logspace(-1, -8, 50);
err_fwd = zeros(size(h_vals));
err_bwd = zeros(size(h_vals));
err_cen = zeros(size(h_vals));
err_cen4 = zeros(size(h_vals));

for k = 1:length(h_vals)
    h = h_vals(k);
    err_fwd(k) = abs(deriv_forward(f, x0, h) - exact);
    err_bwd(k) = abs(deriv_backward(f, x0, h) - exact);
    err_cen(k) = abs(deriv_central(f, x0, h) - exact);
    err_cen4(k) = abs(deriv_central_4th_order(f, x0, h) - exact);
end

figure;
loglog(h_vals, err_fwd, 'r-', 'LineWidth', 2); hold on;
loglog(h_vals, err_bwd, 'b--', 'LineWidth', 2);
loglog(h_vals, err_cen, 'g-', 'LineWidth', 2);
loglog(h_vals, err_cen4, 'm-', 'LineWidth', 2);
xlabel('Step size h'); ylabel('|Error|');
title('Numerical Differentiation — Error vs Step Size');
legend('Forward O(h)', 'Backward O(h)', 'Central O(h^2)', 'Central O(h^4)');
grid on;
```

---
