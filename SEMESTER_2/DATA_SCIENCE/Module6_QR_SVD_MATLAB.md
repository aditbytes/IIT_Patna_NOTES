# Module 6: QR Decomposition & SVD — MATLAB Codes
## CDA 106 | Lectures 17–20

---

## 6.1 Classical Gram-Schmidt Orthogonalization

```matlab
function [Q, R] = gram_schmidt_classical(A)
% GRAM_SCHMIDT_CLASSICAL Classical Gram-Schmidt Process
%   Input:  A - m x n matrix (columns are vectors to orthogonalize)
%   Output: Q - m x n orthonormal matrix
%           R - n x n upper triangular matrix
%   A = Q * R

    [m, n] = size(A);
    Q = zeros(m, n);
    R = zeros(n, n);

    fprintf('=== Classical Gram-Schmidt ===\n\n');

    for j = 1:n
        v = A(:, j);
        fprintf('--- Column %d ---\n', j);
        fprintf('a_%d = [', j); fprintf('%.4f ', A(:,j)); fprintf(']\n');

        % Subtract projections onto previous q vectors
        for i = 1:j-1
            R(i, j) = Q(:, i)' * A(:, j);
            v = v - R(i, j) * Q(:, i);
            fprintf('  <a_%d, q_%d> = %.6f\n', j, i, R(i, j));
        end

        R(j, j) = norm(v);
        Q(:, j) = v / R(j, j);

        fprintf('  ||v_%d|| = %.6f\n', j, R(j,j));
        fprintf('  q_%d = [', j); fprintf('%.6f ', Q(:,j)); fprintf(']\n\n');
    end

    fprintf('Q = \n'); disp(Q);
    fprintf('R = \n'); disp(R);

    % Verify orthonormality
    fprintf('Q''*Q (should be I):\n'); disp(Q' * Q);
    fprintf('Q*R (should be A):\n'); disp(Q * R);
    fprintf('Max error |A - QR|: %.2e\n', max(max(abs(A - Q*R))));
end
```

**Test:**
```matlab
A = [1, 1, 0;
     1, 0, 1;
     0, 1, 1];

[Q, R] = gram_schmidt_classical(A);
```

---

## 6.2 Modified Gram-Schmidt (Numerically Stable)

```matlab
function [Q, R] = gram_schmidt_modified(A)
% GRAM_SCHMIDT_MODIFIED Modified Gram-Schmidt (numerically stable version)
%   More stable than classical GS for ill-conditioned matrices

    [m, n] = size(A);
    Q = zeros(m, n);
    R = zeros(n, n);
    V = A;    % working copy

    fprintf('=== Modified Gram-Schmidt ===\n\n');

    for j = 1:n
        R(j, j) = norm(V(:, j));
        Q(:, j) = V(:, j) / R(j, j);

        fprintf('Step %d: ||v_%d|| = %.8f\n', j, j, R(j,j));

        % Update remaining columns (key difference from classical)
        for i = j+1:n
            R(j, i) = Q(:, j)' * V(:, i);
            V(:, i) = V(:, i) - R(j, i) * Q(:, j);
        end
    end

    fprintf('\nQ = \n'); disp(Q);
    fprintf('R = \n'); disp(R);
    fprintf('Max error |A - QR|: %.2e\n', max(max(abs(A - Q*R))));
    fprintf('Orthogonality check max|Q''Q - I|: %.2e\n', ...
        max(max(abs(Q'*Q - eye(n)))));
end
```

**Test — compare stability:**
```matlab
% Ill-conditioned matrix where classical GS loses orthogonality
A = [1, 1, 1;
     1e-8, 0, 0;
     0, 1e-8, 0;
     0, 0, 1e-8];

fprintf('--- Classical GS ---\n');
[Q1, R1] = gram_schmidt_classical(A);
fprintf('Orthogonality error (classical): %.2e\n', max(max(abs(Q1'*Q1 - eye(3)))));

fprintf('\n--- Modified GS ---\n');
[Q2, R2] = gram_schmidt_modified(A);
fprintf('Orthogonality error (modified):  %.2e\n', max(max(abs(Q2'*Q2 - eye(3)))));
```

---

## 6.3 QR Decomposition via Gram-Schmidt

```matlab
function [Q, R] = qr_gram_schmidt(A)
% QR_GRAM_SCHMIDT Compute QR decomposition using Modified Gram-Schmidt
%   A = Q * R

    [m, n] = size(A);
    Q = zeros(m, n);
    R = zeros(n, n);
    V = A;

    for j = 1:n
        R(j, j) = norm(V(:, j));
        if R(j, j) < eps
            warning('Column %d is (nearly) linearly dependent', j);
            Q(:, j) = zeros(m, 1);
        else
            Q(:, j) = V(:, j) / R(j, j);
        end

        for i = j+1:n
            R(j, i) = Q(:, j)' * V(:, i);
            V(:, i) = V(:, i) - R(j, i) * Q(:, j);
        end
    end
end
```

---

## 6.4 QR Decomposition via Householder Reflections

```matlab
function [Q, R] = qr_householder(A)
% QR_HOUSEHOLDER QR decomposition using Householder reflections
%   More numerically stable than Gram-Schmidt
%   Uses reflectors H_k = I - 2*v*v'/(v'*v)

    [m, n] = size(A);
    Q = eye(m);
    R = A;

    fprintf('=== QR via Householder Reflections ===\n\n');

    for k = 1:min(m-1, n)
        % Extract the column from diagonal downward
        x = R(k:m, k);

        % Compute Householder vector
        e1 = zeros(length(x), 1);
        e1(1) = 1;
        v = sign(x(1)) * norm(x) * e1 + x;
        v = v / norm(v);

        % Apply reflection: R = H * R, Q = Q * H'
        R(k:m, k:n) = R(k:m, k:n) - 2 * v * (v' * R(k:m, k:n));
        Q(:, k:m) = Q(:, k:m) - 2 * (Q(:, k:m) * v) * v';

        fprintf('Step %d: Householder vector v_%d = [', k, k);
        fprintf('%.4f ', v); fprintf(']\n');
    end

    fprintf('\nQ = \n'); disp(Q);
    fprintf('R = \n'); disp(R);
    fprintf('Max error |A - QR|: %.2e\n', max(max(abs(A - Q*R))));
    fprintf('Max |Q''Q - I|: %.2e\n', max(max(abs(Q'*Q - eye(m)))));
end
```

**Test:**
```matlab
A = [1, -1, 4;
     1, 4, -2;
     1, 4, 2;
     1, -1, 0];

fprintf('--- Householder QR ---\n');
[Q, R] = qr_householder(A);

fprintf('\n--- MATLAB built-in ---\n');
[Qm, Rm] = qr(A, 0);
fprintf('Q (built-in):\n'); disp(Qm);
fprintf('R (built-in):\n'); disp(Rm);
```

---

## 6.5 QR Decomposition via Givens Rotations

```matlab
function [Q, R] = qr_givens(A)
% QR_GIVENS QR decomposition using Givens rotations
%   Zeroes out elements one at a time using plane rotations

    [m, n] = size(A);
    Q = eye(m);
    R = A;

    fprintf('=== QR via Givens Rotations ===\n\n');

    for j = 1:n
        for i = m:-1:j+1
            if R(i, j) ~= 0
                % Compute Givens rotation to zero out R(i,j)
                a = R(i-1, j);
                b = R(i, j);
                r = sqrt(a^2 + b^2);
                c = a / r;
                s = -b / r;

                fprintf('G(%d,%d): c=%.6f, s=%.6f  (zeroing R(%d,%d))\n', ...
                    i-1, i, c, s, i, j);

                % Apply rotation to R
                G = eye(m);
                G(i-1, i-1) = c;  G(i-1, i) = -s;
                G(i, i-1) = s;    G(i, i) = c;

                R = G * R;
                Q = Q * G';
            end
        end
    end

    fprintf('\nQ = \n'); disp(Q);
    fprintf('R = \n'); disp(R);
    fprintf('Max error |A - QR|: %.2e\n', max(max(abs(A - Q*R))));
end
```

**Test:**
```matlab
A = [6, 5, 0;
     5, 1, 4;
     0, 4, 3];
[Q, R] = qr_givens(A);
```

---

## 6.6 Solve Ax = b using QR Decomposition

```matlab
function x = solve_qr(A, b)
% SOLVE_QR Solve Ax = b using QR decomposition
%   A = QR  =>  QRx = b  =>  Rx = Q'b  =>  back substitution

    fprintf('=== Solving Ax = b via QR ===\n');

    [Q, R] = qr_gram_schmidt(A);

    fprintf('Q = \n'); disp(Q);
    fprintf('R = \n'); disp(R);

    % Compute Q' * b
    Qb = Q' * b;
    fprintf('Q''*b = \n'); disp(Qb');

    % Back substitution on Rx = Q'b
    n = length(b);
    x = zeros(n, 1);
    x(n) = Qb(n) / R(n, n);
    for i = n-1:-1:1
        x(i) = (Qb(i) - R(i, i+1:n) * x(i+1:n)) / R(i, i);
    end

    fprintf('Solution x = \n'); disp(x');
    fprintf('Verification A*x = \n'); disp((A*x)');
end
```

**Test:**
```matlab
A = [1, 1, 1;
     0, 1, 2;
     1, 0, 1];
b = [6; 5; 4];
x = solve_qr(A, b);
fprintf('MATLAB A\\b = \n'); disp((A\b)');
```

---

## 6.7 QR Algorithm (Basic) for All Eigenvalues

```matlab
function [eigenvalues, iter] = qr_algorithm_basic(A, tol, max_iter)
% QR_ALGORITHM_BASIC Find ALL eigenvalues using basic QR algorithm
%   Iterate: A_k = Q_k * R_k, then A_{k+1} = R_k * Q_k
%   Converges: A_k -> upper triangular (eigenvalues on diagonal)

    n = size(A, 1);
    Ak = A;

    fprintf('=== QR Algorithm (Basic) ===\n');
    fprintf('%-6s %-20s %-15s\n', 'Iter', 'Diagonal', 'Max off-diag');

    for iter = 1:max_iter
        [Q, R] = qr_gram_schmidt(Ak);
        Ak = R * Q;

        % Check convergence: off-diagonal elements
        off_diag = Ak - diag(diag(Ak));
        max_off = max(max(abs(off_diag)));

        if mod(iter, 5) == 0 || max_off < tol
            diag_str = sprintf('%.4f ', diag(Ak));
            fprintf('%-6d [%s] %.2e\n', iter, diag_str, max_off);
        end

        if max_off < tol
            fprintf('\nConverged after %d iterations!\n', iter);
            break;
        end
    end

    eigenvalues = diag(Ak);
    fprintf('\nEigenvalues (QR algorithm): '); disp(sort(eigenvalues)');
    fprintf('Eigenvalues (built-in):    '); disp(sort(eig(A))');
end
```

**Test:**
```matlab
A = [4, 1, 0;
     1, 3, 1;
     0, 1, 2];

[evals, iter] = qr_algorithm_basic(A, 1e-10, 200);
```

---

## 6.8 QR Algorithm with Shifts (Wilkinson Shift)

```matlab
function [eigenvalues, iter] = qr_algorithm_shifted(A, tol, max_iter)
% QR_ALGORITHM_SHIFTED QR Algorithm with Wilkinson shift
%   Faster convergence by shifting: A_k - mu*I = Q*R, then A_{k+1} = R*Q + mu*I
%   Wilkinson shift: mu = eigenvalue of bottom-right 2x2 closest to a_{nn}

    n = size(A, 1);
    Ak = A;

    fprintf('=== QR Algorithm with Wilkinson Shift ===\n');

    for iter = 1:max_iter
        % Wilkinson shift: eigenvalue of bottom 2x2 block closest to a(n,n)
        if n >= 2
            a = Ak(n-1, n-1);
            b = Ak(n-1, n);
            c = Ak(n, n-1);
            d = Ak(n, n);

            % Eigenvalues of 2x2 block
            tr = a + d;
            det_val = a*d - b*c;
            disc = sqrt(tr^2 - 4*det_val);
            lam1 = (tr + disc) / 2;
            lam2 = (tr - disc) / 2;

            % Pick the one closer to d = a(n,n)
            if abs(lam1 - d) < abs(lam2 - d)
                mu = lam1;
            else
                mu = lam2;
            end
        else
            mu = 0;
        end

        % Shifted QR step
        [Q, R] = qr_gram_schmidt(Ak - mu * eye(n));
        Ak = R * Q + mu * eye(n);

        % Check convergence
        off_diag = Ak - diag(diag(Ak));
        max_off = max(max(abs(off_diag)));

        if mod(iter, 5) == 0 || max_off < tol
            fprintf('iter %d: max off-diag = %.2e, shift = %.6f\n', iter, max_off, mu);
        end

        if max_off < tol
            fprintf('Converged after %d iterations!\n', iter);
            break;
        end
    end

    eigenvalues = diag(Ak);
    fprintf('\nEigenvalues (shifted QR): '); disp(sort(eigenvalues)');
    fprintf('Built-in eig(A):         '); disp(sort(eig(A))');
end
```

**Test:**
```matlab
A = [4, 1, 0, 0;
     1, 3, 1, 0;
     0, 1, 2, 1;
     0, 0, 1, 1];

fprintf('--- Basic QR ---\n');
[ev1, it1] = qr_algorithm_basic(A, 1e-10, 500);

fprintf('\n--- Shifted QR ---\n');
[ev2, it2] = qr_algorithm_shifted(A, 1e-10, 500);

fprintf('\nBasic QR: %d iterations\n', it1);
fprintf('Shifted QR: %d iterations\n', it2);
```

---

## 6.9 SVD Computation (A = UΣV^T) — Step by Step

```matlab
function [U, S, V] = svd_manual(A)
% SVD_MANUAL Compute SVD: A = U * S * V'
%   Step 1: Compute A'A and find eigenvalues -> singular values sigma_i
%   Step 2: Find V (eigenvectors of A'A)
%   Step 3: Find U from u_i = A*v_i / sigma_i

    [m, n] = size(A);
    r = min(m, n);

    fprintf('=== Manual SVD Computation ===\n');
    fprintf('A (%d x %d):\n', m, n); disp(A);

    % Step 1: Compute A'A
    AtA = A' * A;
    fprintf('A''A = \n'); disp(AtA);

    % Step 2: Eigendecomposition of A'A
    [V_all, D] = eig(AtA);
    eigenvalues = diag(D);

    % Sort in descending order
    [eigenvalues, idx] = sort(eigenvalues, 'descend');
    V_all = V_all(:, idx);

    fprintf('Eigenvalues of A''A: '); disp(eigenvalues');

    % Singular values = sqrt of eigenvalues
    sigma = sqrt(max(eigenvalues, 0));  % avoid sqrt of tiny negatives
    fprintf('Singular values: '); disp(sigma');

    % Step 3: V matrix (eigenvectors of A'A)
    V = V_all(:, 1:r);
    fprintf('V = \n'); disp(V);

    % Step 4: U matrix — u_i = A*v_i / sigma_i
    U = zeros(m, r);
    for i = 1:r
        if sigma(i) > eps
            U(:, i) = A * V(:, i) / sigma(i);
        end
    end

    % If m > n, extend U to be m x m orthogonal
    if m > r
        % Find orthogonal complement
        [U_full, ~] = qr(U);
        U = U_full(:, 1:m);
    end
    fprintf('U = \n'); disp(U(:, 1:r));

    % Construct Sigma matrix
    S = zeros(m, n);
    for i = 1:r
        S(i, i) = sigma(i);
    end
    fprintf('Sigma = \n'); disp(S);

    % Verify
    A_reconstructed = U(:,1:r) * S(1:r, :) * V';
    fprintf('U * Sigma * V'' = \n'); disp(A_reconstructed);
    fprintf('Max error |A - USV''|: %.2e\n', max(max(abs(A - A_reconstructed))));
end
```

**Test:**
```matlab
A = [3, 2, 2;
     2, 3, -2];

fprintf('--- Manual SVD ---\n');
[U, S, V] = svd_manual(A);

fprintf('\n--- Built-in SVD ---\n');
[Um, Sm, Vm] = svd(A);
fprintf('U (built-in):\n'); disp(Um);
fprintf('S (built-in):\n'); disp(Sm);
fprintf('V (built-in):\n'); disp(Vm);
```

---

## 6.10 SVD for Square Matrix

```matlab
function [U, S, V] = svd_square(A)
% SVD_SQUARE SVD for square matrix with detailed steps
%   A = U * Sigma * V'

    [m, n] = size(A);
    assert(m == n, 'Matrix must be square');

    fprintf('=== SVD for %dx%d Matrix ===\n\n', m, n);

    % Method: Use both A'A and AA'
    AtA = A' * A;
    AAt = A * A';

    fprintf('A''A = \n'); disp(AtA);
    fprintf('AA'' = \n'); disp(AAt);

    % Eigendecomposition of A'A for V and sigma
    [V_mat, D1] = eig(AtA);
    [eig_vals, idx] = sort(diag(D1), 'descend');
    V_mat = V_mat(:, idx);

    % Eigendecomposition of AA' for U
    [U_mat, D2] = eig(AAt);
    [~, idx2] = sort(diag(D2), 'descend');
    U_mat = U_mat(:, idx2);

    sigma = sqrt(max(eig_vals, 0));

    fprintf('Eigenvalues of A''A: '); disp(eig_vals');
    fprintf('Singular values:    '); disp(sigma');

    % Fix signs: ensure U and V are consistent
    % u_i = A * v_i / sigma_i
    U = zeros(m, m);
    V = V_mat;
    for i = 1:m
        if sigma(i) > eps
            U(:, i) = A * V(:, i) / sigma(i);
        else
            U(:, i) = U_mat(:, i);
        end
    end

    S = diag(sigma);

    fprintf('U = \n'); disp(U);
    fprintf('Sigma = \n'); disp(S);
    fprintf('V = \n'); disp(V);

    fprintf('Verification: U*S*V'' = \n'); disp(U * S * V');
    fprintf('Max error: %.2e\n', max(max(abs(A - U * S * V'))));
end
```

**Test:**
```matlab
A = [1, 2;
     3, 4];
[U, S, V] = svd_square(A);
```

---

## 6.11 SVD for Rectangular Matrix

```matlab
function [U, S, V] = svd_rectangular(A)
% SVD_RECTANGULAR SVD for m x n matrix (m ≠ n)
%   A (m x n) = U (m x m) * S (m x n) * V' (n x n)

    [m, n] = size(A);
    fprintf('=== SVD for %dx%d Rectangular Matrix ===\n', m, n);
    fprintf('A = \n'); disp(A);

    AtA = A' * A;  % n x n
    AAt = A * A';  % m x m

    % Get V from A'A
    [V, D_v] = eig(AtA);
    [eig_v, idx_v] = sort(diag(D_v), 'descend');
    V = V(:, idx_v);

    % Singular values
    r = min(m, n);
    sigma = sqrt(max(eig_v(1:r), 0));

    % Get U: u_i = A*v_i / sigma_i for nonzero sigma
    U = zeros(m, m);
    for i = 1:r
        if sigma(i) > eps
            U(:, i) = A * V(:, i) / sigma(i);
        end
    end

    % Complete U to orthonormal basis if needed
    if m > r
        % Use null space of A' or QR
        null_cols = null(A');
        if ~isempty(null_cols)
            U(:, r+1:m) = null_cols;
        end
        % Ensure orthonormality
        [U, ~] = qr(U);
    end

    % Build S matrix (m x n)
    S = zeros(m, n);
    for i = 1:min(length(sigma), min(m,n))
        S(i, i) = sigma(i);
    end

    fprintf('U (%dx%d) = \n', m, m); disp(U);
    fprintf('Sigma (%dx%d) = \n', m, n); disp(S);
    fprintf('V (%dx%d) = \n', n, n); disp(V);
    fprintf('U*S*V'' = \n'); disp(U * S * V');
    fprintf('Max error: %.2e\n', max(max(abs(A - U * S * V'))));
end
```

**Test:**
```matlab
% 3 x 2 matrix
A = [1, 0;
     0, 1;
     1, 1];
[U, S, V] = svd_rectangular(A);

% 2 x 3 matrix
B = [1, 2, 3;
     4, 5, 6];
[U2, S2, V2] = svd_rectangular(B);
```

---

## 6.12 Low-Rank Approximation using SVD

```matlab
function [A_k, error_k] = svd_low_rank(A, k)
% SVD_LOW_RANK Compute rank-k approximation of A using SVD
%   A_k = sum_{i=1}^{k} sigma_i * u_i * v_i'
%   Error: ||A - A_k||_F = sqrt(sigma_{k+1}^2 + ... + sigma_r^2)

    [U, S, V] = svd(A);  % Use built-in for reliability
    sigma = diag(S);

    fprintf('=== Low-Rank Approximation (rank %d) ===\n', k);
    fprintf('Singular values: '); disp(sigma');
    fprintf('Original rank: %d\n', rank(A));

    % Rank-k approximation
    U_k = U(:, 1:k);
    S_k = S(1:k, 1:k);
    V_k = V(:, 1:k);
    A_k = U_k * S_k * V_k';

    fprintf('\nA_k (rank-%d approximation):\n', k); disp(A_k);

    % Error
    error_k = norm(A - A_k, 'fro');
    theoretical_error = norm(sigma(k+1:end));
    fprintf('||A - A_k||_F = %.8f\n', error_k);
    fprintf('Theoretical  = sqrt(sigma_%d^2 + ...) = %.8f\n', k+1, theoretical_error);

    % Energy captured
    total_energy = sum(sigma.^2);
    captured_energy = sum(sigma(1:k).^2);
    fprintf('Energy captured: %.2f%%\n', 100 * captured_energy / total_energy);
end
```

**Test:**
```matlab
A = [1, 2, 3;
     4, 5, 6;
     7, 8, 9;
     10, 11, 12];

fprintf('--- Rank 1 approximation ---\n');
[A1, e1] = svd_low_rank(A, 1);

fprintf('\n--- Rank 2 approximation ---\n');
[A2, e2] = svd_low_rank(A, 2);
```

---

## 6.13 Energy Plot — SVD Singular Values

```matlab
function svd_energy_plot(A)
% SVD_ENERGY_PLOT Plot singular values and cumulative energy

    [~, S, ~] = svd(A);
    sigma = diag(S);
    n = length(sigma);

    energy = sigma.^2;
    cum_energy = cumsum(energy) / sum(energy) * 100;

    figure;
    subplot(1, 2, 1);
    bar(1:n, sigma, 'FaceColor', [0.2, 0.6, 0.9]);
    xlabel('Index i');
    ylabel('\sigma_i');
    title('Singular Values');
    grid on;

    subplot(1, 2, 2);
    plot(1:n, cum_energy, 'r-o', 'LineWidth', 2, 'MarkerSize', 8);
    xlabel('Number of singular values (k)');
    ylabel('Cumulative energy (%)');
    title('Cumulative Energy Captured');
    yline(95, 'b--', '95%', 'LineWidth', 1.5);
    grid on;
    ylim([0, 105]);

    fprintf('Singular values: '); disp(sigma');
    fprintf('Cumulative energy (%%):\n');
    for i = 1:n
        fprintf('  k=%d: %.2f%%\n', i, cum_energy(i));
    end
end
```

**Test:**
```matlab
% Random matrix
A = randn(10, 5);
svd_energy_plot(A);

% Matrix with clear rank structure
B = [1; 2; 3; 4; 5] * [1, 2, 3] + 0.01 * randn(5, 3);
figure;
svd_energy_plot(B);
```

---

## 6.14 Pseudoinverse via SVD

```matlab
function A_pinv = pseudoinverse_svd(A)
% PSEUDOINVERSE_SVD Compute Moore-Penrose pseudoinverse using SVD
%   A^+ = V * Sigma^+ * U'
%   Sigma^+ = diag(1/sigma_1, ..., 1/sigma_r, 0, ..., 0)

    [U, S, V] = svd(A);
    sigma = diag(S);

    fprintf('=== Pseudoinverse via SVD ===\n');
    fprintf('Singular values: '); disp(sigma');

    % Invert non-zero singular values
    tol_val = max(size(A)) * eps * sigma(1);
    S_pinv = zeros(size(S'));
    for i = 1:length(sigma)
        if sigma(i) > tol_val
            S_pinv(i, i) = 1 / sigma(i);
        end
    end

    A_pinv = V * S_pinv * U';

    fprintf('A^+ = \n'); disp(A_pinv);

    % Verify 4 Moore-Penrose conditions
    fprintf('Verification (Moore-Penrose conditions):\n');
    fprintf('  ||A*A^+*A - A||   = %.2e\n', norm(A * A_pinv * A - A, 'fro'));
    fprintf('  ||A^+*A*A^+ - A^+|| = %.2e\n', norm(A_pinv * A * A_pinv - A_pinv, 'fro'));

    fprintf('\nBuilt-in pinv(A):\n'); disp(pinv(A));
    fprintf('Max difference: %.2e\n', max(max(abs(A_pinv - pinv(A)))));
end
```

**Test:**
```matlab
% Rectangular (overdetermined)
A = [1, 2; 3, 4; 5, 6];
A_pinv = pseudoinverse_svd(A);

% Least squares: solve Ax ≈ b
b = [1; 2; 3];
x_pinv = A_pinv * b;
x_backslash = A \ b;
fprintf('Least squares via A^+: '); disp(x_pinv');
fprintf('MATLAB A\\b:            '); disp(x_backslash');
```

---

## 6.15 Image Compression using SVD (Bonus Application)

```matlab
function svd_image_compression(k_values)
% SVD_IMAGE_COMPRESSION Demonstrate SVD-based image compression
%   k_values - array of ranks to try, e.g., [1, 5, 10, 20, 50]

    % Create a synthetic "image" (or load one)
    % A = double(rgb2gray(imread('cameraman.tif')));  % if available
    n = 64;
    [X, Y] = meshgrid(linspace(-2, 2, n));
    A = sin(X.^2 + Y.^2) + cos(X .* Y);  % synthetic image

    [U, S, V] = svd(A);
    sigma = diag(S);

    figure;
    num_plots = length(k_values) + 1;
    subplot(1, num_plots, 1);
    imagesc(A); colormap gray; axis image;
    title(sprintf('Original (rank %d)', rank(A)));

    for idx = 1:length(k_values)
        k = k_values(idx);
        Ak = U(:, 1:k) * S(1:k, 1:k) * V(:, 1:k)';
        compression = 100 * (1 - k*(2*n+1) / (n*n));
        error = norm(A - Ak, 'fro') / norm(A, 'fro') * 100;

        subplot(1, num_plots, idx + 1);
        imagesc(Ak); colormap gray; axis image;
        title(sprintf('Rank %d\nComp: %.1f%%, Err: %.1f%%', k, compression, error));
    end

    sgtitle('SVD Image Compression');
end
```

**Test:**
```matlab
svd_image_compression([1, 5, 10, 20]);
```

---

## 6.16 Full Comparison Script — All Decompositions

```matlab
%% Master comparison: QR methods and SVD
clc; clear; close all;

A = [12, -51, 4;
     6, 167, -68;
     -4, 24, -41];

fprintf('================================================\n');
fprintf('Matrix A:\n'); disp(A);
fprintf('================================================\n\n');

% 1. Classical Gram-Schmidt
fprintf('--- 1. Classical Gram-Schmidt ---\n');
[Q1, R1] = gram_schmidt_classical(A);
fprintf('Orthogonality error: %.2e\n\n', norm(Q1'*Q1 - eye(3), 'fro'));

% 2. Modified Gram-Schmidt
fprintf('--- 2. Modified Gram-Schmidt ---\n');
[Q2, R2] = gram_schmidt_modified(A);
fprintf('Orthogonality error: %.2e\n\n', norm(Q2'*Q2 - eye(3), 'fro'));

% 3. Householder
fprintf('--- 3. Householder ---\n');
[Q3, R3] = qr_householder(A);
fprintf('Orthogonality error: %.2e\n\n', norm(Q3'*Q3 - eye(3), 'fro'));

% 4. Givens
fprintf('--- 4. Givens ---\n');
[Q4, R4] = qr_givens(A);
fprintf('Orthogonality error: %.2e\n\n', norm(Q4'*Q4 - eye(3), 'fro'));

% 5. Built-in
fprintf('--- 5. MATLAB built-in QR ---\n');
[Q5, R5] = qr(A);
fprintf('Q:\n'); disp(Q5);
fprintf('R:\n'); disp(R5);

% 6. SVD
fprintf('\n--- 6. SVD ---\n');
[U, S, V] = svd_manual(A);

% 7. QR Algorithm for eigenvalues
fprintf('\n--- 7. Eigenvalues via QR Algorithm ---\n');
[evals, ~] = qr_algorithm_shifted(A, 1e-10, 100);

fprintf('\n================================================\n');
fprintf('Summary of decomposition errors:\n');
fprintf('  |A - Q1*R1| = %.2e (Classical GS)\n', norm(A - Q1*R1, 'fro'));
fprintf('  |A - Q2*R2| = %.2e (Modified GS)\n', norm(A - Q2*R2, 'fro'));
fprintf('  |A - Q3*R3| = %.2e (Householder)\n', norm(A - Q3*R3, 'fro'));
fprintf('  |A - Q4*R4| = %.2e (Givens)\n', norm(A - Q4*R4, 'fro'));
fprintf('  |A - USV''|  = %.2e (SVD)\n', norm(A - U*S*V', 'fro'));
fprintf('================================================\n');
```

---
