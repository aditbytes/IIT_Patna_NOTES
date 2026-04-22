# Module 3: Ordinary Differential Equations — MATLAB Codes
## CDA 106 | Lecture 9

---

## 3.1 Euler's Method

```matlab
function [x, y] = euler_method(f, x0, y0, h, xn)
% EULER_METHOD Solve ODE y' = f(x,y) using Euler's method
%   f  - function handle f(x, y)
%   x0 - initial x
%   y0 - initial y (y(x0) = y0)
%   h  - step size
%   xn - final x value
%
%   Formula: y_{n+1} = y_n + h * f(x_n, y_n)

    n = round((xn - x0) / h);
    x = zeros(1, n+1);
    y = zeros(1, n+1);
    x(1) = x0;
    y(1) = y0;

    for i = 1:n
        y(i+1) = y(i) + h * f(x(i), y(i));
        x(i+1) = x(i) + h;
    end

    % Display step-by-step
    fprintf('Euler''s Method (h = %.4f):\n', h);
    fprintf('%-6s %-14s %-14s\n', 'Step', 'x_n', 'y_n');
    for i = 1:n+1
        fprintf('%-6d %-14.8f %-14.8f\n', i-1, x(i), y(i));
    end
end
```

**Test:**
```matlab
% dy/dx = x + y, y(0) = 1, solve from x=0 to x=1
f = @(x, y) x + y;
[x, y] = euler_method(f, 0, 1, 0.1, 1.0);

% Exact solution: y = 2*exp(x) - x - 1
exact = @(x) 2*exp(x) - x - 1;
fprintf('\nFinal: y(1) = %.8f\n', y(end));
fprintf('Exact: y(1) = %.8f\n', exact(1));
fprintf('Error:       %.2e\n', abs(y(end) - exact(1)));

% Plot
figure;
plot(x, y, 'b-o', 'LineWidth', 2); hold on;
xx = linspace(0, 1, 100);
plot(xx, exact(xx), 'r-', 'LineWidth', 2);
xlabel('x'); ylabel('y');
title('Euler''s Method');
legend('Euler', 'Exact');
grid on;
```

---

## 3.2 Modified Euler's Method (Heun's Method)

```matlab
function [x, y] = modified_euler(f, x0, y0, h, xn)
% MODIFIED_EULER Solve ODE y' = f(x,y) using Modified Euler / Heun's method
%
%   Predictor: y_pred = y_n + h * f(x_n, y_n)
%   Corrector: y_{n+1} = y_n + (h/2) * [f(x_n, y_n) + f(x_{n+1}, y_pred)]

    n = round((xn - x0) / h);
    x = zeros(1, n+1);
    y = zeros(1, n+1);
    x(1) = x0;
    y(1) = y0;

    for i = 1:n
        x(i+1) = x(i) + h;

        % Predictor (Euler step)
        y_pred = y(i) + h * f(x(i), y(i));

        % Corrector (average of slopes)
        y(i+1) = y(i) + (h / 2) * (f(x(i), y(i)) + f(x(i+1), y_pred));
    end

    % Display
    fprintf('Modified Euler''s Method (h = %.4f):\n', h);
    fprintf('%-6s %-14s %-14s\n', 'Step', 'x_n', 'y_n');
    for i = 1:n+1
        fprintf('%-6d %-14.8f %-14.8f\n', i-1, x(i), y(i));
    end
end
```

**Test:**
```matlab
f = @(x, y) x + y;
[x, y] = modified_euler(f, 0, 1, 0.1, 1.0);

exact = @(x) 2*exp(x) - x - 1;
fprintf('\nFinal: y(1) = %.8f\n', y(end));
fprintf('Exact: y(1) = %.8f\n', exact(1));
fprintf('Error:       %.2e\n', abs(y(end) - exact(1)));
```

---

## 3.3 Midpoint Method (RK2 Variant)

```matlab
function [x, y] = midpoint_method(f, x0, y0, h, xn)
% MIDPOINT_METHOD Solve ODE using Midpoint Method (a Runge-Kutta 2nd order variant)
%
%   k1 = h * f(x_n, y_n)
%   k2 = h * f(x_n + h/2, y_n + k1/2)
%   y_{n+1} = y_n + k2

    n = round((xn - x0) / h);
    x = zeros(1, n+1);
    y = zeros(1, n+1);
    x(1) = x0;
    y(1) = y0;

    for i = 1:n
        k1 = h * f(x(i), y(i));
        k2 = h * f(x(i) + h/2, y(i) + k1/2);
        y(i+1) = y(i) + k2;
        x(i+1) = x(i) + h;
    end

    fprintf('Midpoint Method (h = %.4f):\n', h);
    fprintf('%-6s %-14s %-14s\n', 'Step', 'x_n', 'y_n');
    for i = 1:n+1
        fprintf('%-6d %-14.8f %-14.8f\n', i-1, x(i), y(i));
    end
end
```

---

## 3.4 Runge-Kutta 2nd Order (RK2)

```matlab
function [x, y] = rk2(f, x0, y0, h, xn)
% RK2 Runge-Kutta 2nd Order Method
%
%   k1 = h * f(x_n, y_n)
%   k2 = h * f(x_n + h, y_n + k1)
%   y_{n+1} = y_n + (k1 + k2) / 2

    n = round((xn - x0) / h);
    x = zeros(1, n+1);
    y = zeros(1, n+1);
    x(1) = x0;
    y(1) = y0;

    for i = 1:n
        k1 = h * f(x(i), y(i));
        k2 = h * f(x(i) + h, y(i) + k1);
        y(i+1) = y(i) + (k1 + k2) / 2;
        x(i+1) = x(i) + h;
    end

    fprintf('RK2 Method (h = %.4f):\n', h);
    fprintf('%-6s %-14s %-14s %-14s %-14s\n', 'Step', 'x_n', 'y_n', 'k1', 'k2');
    % Recompute for display
    xi = x0; yi = y0;
    fprintf('%-6d %-14.8f %-14.8f\n', 0, xi, yi);
    for i = 1:n
        k1 = h * f(x(i), y(i));
        k2 = h * f(x(i) + h, y(i) + k1);
        fprintf('%-6d %-14.8f %-14.8f %-14.8f %-14.8f\n', i, x(i+1), y(i+1), k1, k2);
    end
end
```

**Test:**
```matlab
f = @(x, y) -2*x*y;
% Exact: y = exp(-x^2)
[x, y] = rk2(f, 0, 1, 0.1, 1.0);

exact = @(x) exp(-x.^2);
fprintf('\nFinal: y(1) = %.8f\n', y(end));
fprintf('Exact: y(1) = %.8f\n', exact(1));
```

---

## 3.5 Runge-Kutta 4th Order (RK4) — The Classic

```matlab
function [x, y] = rk4(f, x0, y0, h, xn)
% RK4 Classical Runge-Kutta 4th Order Method
%
%   k1 = h * f(x_n, y_n)
%   k2 = h * f(x_n + h/2, y_n + k1/2)
%   k3 = h * f(x_n + h/2, y_n + k2/2)
%   k4 = h * f(x_n + h, y_n + k3)
%   y_{n+1} = y_n + (k1 + 2*k2 + 2*k3 + k4) / 6

    n = round((xn - x0) / h);
    x = zeros(1, n+1);
    y = zeros(1, n+1);
    x(1) = x0;
    y(1) = y0;

    fprintf('RK4 Method (h = %.4f):\n', h);
    fprintf('%-5s %-12s %-12s %-12s %-12s %-12s %-12s\n', ...
        'Step', 'x_n', 'k1', 'k2', 'k3', 'k4', 'y_{n+1}');

    for i = 1:n
        k1 = h * f(x(i), y(i));
        k2 = h * f(x(i) + h/2, y(i) + k1/2);
        k3 = h * f(x(i) + h/2, y(i) + k2/2);
        k4 = h * f(x(i) + h, y(i) + k3);

        y(i+1) = y(i) + (k1 + 2*k2 + 2*k3 + k4) / 6;
        x(i+1) = x(i) + h;

        fprintf('%-5d %-12.6f %-12.6f %-12.6f %-12.6f %-12.6f %-12.6f\n', ...
            i, x(i), k1, k2, k3, k4, y(i+1));
    end
end
```

**Test:**
```matlab
% dy/dx = x + y, y(0) = 1
f = @(x, y) x + y;
[x, y] = rk4(f, 0, 1, 0.1, 1.0);

exact = @(x) 2*exp(x) - x - 1;
fprintf('\nFinal: y(1) = %.10f\n', y(end));
fprintf('Exact: y(1) = %.10f\n', exact(1));
fprintf('Error:       %.2e\n', abs(y(end) - exact(1)));
```

---

## 3.6 RK4 for Systems of ODEs

```matlab
function [t, Y] = rk4_system(F, t0, Y0, h, tn)
% RK4_SYSTEM RK4 for a system of ODEs
%   F  - function handle F(t, Y) returning column vector
%   Y0 - initial conditions (column vector)
%   Y  - matrix where each row is solution at one time step

    n = round((tn - t0) / h);
    m = length(Y0);
    t = zeros(1, n+1);
    Y = zeros(n+1, m);
    t(1) = t0;
    Y(1, :) = Y0(:)';

    for i = 1:n
        K1 = h * F(t(i), Y(i,:)');
        K2 = h * F(t(i) + h/2, Y(i,:)' + K1/2);
        K3 = h * F(t(i) + h/2, Y(i,:)' + K2/2);
        K4 = h * F(t(i) + h, Y(i,:)' + K3);

        Y(i+1, :) = Y(i, :) + (K1 + 2*K2 + 2*K3 + K4)' / 6;
        t(i+1) = t(i) + h;
    end
end
```

**Test:**
```matlab
% Convert 2nd order ODE to system:
% y'' + y = 0,  y(0) = 0, y'(0) = 1
% Let y1 = y, y2 = y'
% y1' = y2
% y2' = -y1
F = @(t, Y) [Y(2); -Y(1)];

[t, Y] = rk4_system(F, 0, [0; 1], 0.1, 2*pi);

figure;
plot(t, Y(:,1), 'b-', 'LineWidth', 2); hold on;
plot(t, sin(t), 'r--', 'LineWidth', 2);
xlabel('t'); ylabel('y');
title('RK4 System: y'''' + y = 0');
legend('RK4', 'Exact sin(t)');
grid on;
```

---

## 3.7 Convergence Comparison — All Methods

```matlab
%% Compare Euler, Modified Euler, RK2, RK4
clc; clear; close all;

f = @(x, y) x + y;
exact = @(x) 2*exp(x) - x - 1;

x0 = 0; y0 = 1; xn = 1;

h_vals = [0.2, 0.1, 0.05, 0.025, 0.0125, 0.00625];
err_euler = zeros(size(h_vals));
err_mod   = zeros(size(h_vals));
err_rk2   = zeros(size(h_vals));
err_rk4   = zeros(size(h_vals));

for k = 1:length(h_vals)
    h = h_vals(k);
    [~, y1] = euler_method(f, x0, y0, h, xn);
    [~, y2] = modified_euler(f, x0, y0, h, xn);
    [~, y3] = rk2(f, x0, y0, h, xn);
    [~, y4] = rk4(f, x0, y0, h, xn);

    err_euler(k) = abs(y1(end) - exact(xn));
    err_mod(k)   = abs(y2(end) - exact(xn));
    err_rk2(k)   = abs(y3(end) - exact(xn));
    err_rk4(k)   = abs(y4(end) - exact(xn));
end

figure;
loglog(h_vals, err_euler, 'r-o', 'LineWidth', 2); hold on;
loglog(h_vals, err_mod,   'b-s', 'LineWidth', 2);
loglog(h_vals, err_rk2,   'g-^', 'LineWidth', 2);
loglog(h_vals, err_rk4,   'm-d', 'LineWidth', 2);
xlabel('Step size h'); ylabel('|Error at x=1|');
title('ODE Solver Comparison');
legend('Euler O(h)', 'Modified Euler O(h^2)', 'RK2 O(h^2)', 'RK4 O(h^4)', ...
    'Location', 'southeast');
grid on;

fprintf('\n=== Error at x = 1 ===\n');
fprintf('%-10s %-14s %-14s %-14s %-14s\n', 'h', 'Euler', 'Mod.Euler', 'RK2', 'RK4');
for k = 1:length(h_vals)
    fprintf('%-10.5f %-14.2e %-14.2e %-14.2e %-14.2e\n', ...
        h_vals(k), err_euler(k), err_mod(k), err_rk2(k), err_rk4(k));
end
```

---

## 3.8 Step-by-Step Worked Example

```matlab
%% Detailed step-by-step: dy/dx = y - x^2 + 1, y(0)=0.5, h=0.2, find y(0.2)
clc;

f = @(x, y) y - x^2 + 1;
x0 = 0; y0 = 0.5; h = 0.2;

fprintf('===== STEP-BY-STEP: RK4 =====\n');
fprintf('dy/dx = y - x^2 + 1, y(0) = 0.5, h = 0.2\n\n');

% Step 1: Compute k1
k1 = h * f(x0, y0);
fprintf('k1 = h * f(x0, y0) = %.4f * f(%.4f, %.4f) = %.4f * %.4f = %.8f\n', ...
    h, x0, y0, h, f(x0, y0), k1);

% Step 2: Compute k2
k2 = h * f(x0 + h/2, y0 + k1/2);
fprintf('k2 = h * f(x0+h/2, y0+k1/2) = %.4f * f(%.4f, %.4f) = %.8f\n', ...
    h, x0+h/2, y0+k1/2, k2);

% Step 3: Compute k3
k3 = h * f(x0 + h/2, y0 + k2/2);
fprintf('k3 = h * f(x0+h/2, y0+k2/2) = %.4f * f(%.4f, %.4f) = %.8f\n', ...
    h, x0+h/2, y0+k2/2, k3);

% Step 4: Compute k4
k4 = h * f(x0 + h, y0 + k3);
fprintf('k4 = h * f(x0+h, y0+k3) = %.4f * f(%.4f, %.4f) = %.8f\n', ...
    h, x0+h, y0+k3, k4);

% Step 5: Compute y1
y1 = y0 + (k1 + 2*k2 + 2*k3 + k4) / 6;
fprintf('\ny(0.2) = y0 + (k1 + 2k2 + 2k3 + k4)/6\n');
fprintf('       = %.4f + (%.8f + 2*%.8f + 2*%.8f + %.8f)/6\n', y0, k1, k2, k3, k4);
fprintf('       = %.10f\n', y1);

% Exact: y = (x+1)^2 - 0.5*exp(x)
exact = (0.2+1)^2 - 0.5*exp(0.2);
fprintf('\nExact: y(0.2) = %.10f\n', exact);
fprintf('Error:         %.2e\n', abs(y1 - exact));
```

---

## 3.9 Adaptive Step Size RK4 (Bonus)

```matlab
function [x, y] = rk4_adaptive(f, x0, y0, xn, tol)
% RK4_ADAPTIVE RK4 with adaptive step size control
%   Uses step doubling to estimate error

    h = (xn - x0) / 10;   % initial step
    x = x0;
    y = y0;
    xc = x0;
    yc = y0;

    while xc < xn
        if xc + h > xn
            h = xn - xc;
        end

        % Full step
        [~, y_full] = rk4_single_step(f, xc, yc, h);

        % Two half steps
        [~, y_half] = rk4_single_step(f, xc, yc, h/2);
        [~, y_two]  = rk4_single_step(f, xc + h/2, y_half, h/2);

        % Error estimate
        err = abs(y_two - y_full) / 15;  % Richardson

        if err < tol
            xc = xc + h;
            yc = y_two + (y_two - y_full) / 15;  % local extrapolation
            x = [x, xc];
            y = [y, yc];

            % Increase step if error is very small
            if err < tol / 10
                h = h * 2;
            end
        else
            % Reduce step
            h = h / 2;
        end
    end

    fprintf('Adaptive RK4: %d steps taken\n', length(x)-1);
end

function [x_new, y_new] = rk4_single_step(f, x, y, h)
    k1 = h * f(x, y);
    k2 = h * f(x + h/2, y + k1/2);
    k3 = h * f(x + h/2, y + k2/2);
    k4 = h * f(x + h, y + k3);
    y_new = y + (k1 + 2*k2 + 2*k3 + k4) / 6;
    x_new = x + h;
end
```

**Test:**
```matlab
f = @(x, y) -20 * (y - x^2) + 2*x;  % stiff-ish problem
[x, y] = rk4_adaptive(f, 0, 0, 1, 1e-8);

figure;
plot(x, y, 'b-o', 'LineWidth', 2);
xlabel('x'); ylabel('y');
title('Adaptive RK4');
grid on;
```

---
