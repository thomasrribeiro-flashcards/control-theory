+++
order = 12
subject = "Mathematics"
tags = ["math", "control-theory", "optimal-control", "lqr", "riccati", "kalman-filter", "lqg", "mpc"]
+++

# Control Theory — Optimal Control

## 12.1 The Optimal Control Problem

Q: What is the general OPTIMAL CONTROL problem?
A: Find the control input $\mathbf{u}(t)$ minimizing a PERFORMANCE INDEX (cost functional) subject to system dynamics constraints: $\min_{\mathbf{u}} J = \int_0^T L(\mathbf{x}, \mathbf{u}, t) dt + \phi(\mathbf{x}(T))$ s.t. $\dot{\mathbf{x}} = f(\mathbf{x}, \mathbf{u}, t)$. $L$ = running cost; $\phi$ = terminal cost. Choice of $L, \phi$ expresses design goals (tracking error, control effort, energy). Central trade-off: "good performance" vs "low control effort."

## 12.2 The Linear Quadratic Regulator (LQR)

Q: Define the [LQR problem].
A: Linear system $\dot{\mathbf{x}} = A\mathbf{x} + B\mathbf{u}$; quadratic cost $J = \int_0^\infty (\mathbf{x}^T Q \mathbf{x} + \mathbf{u}^T R \mathbf{u}) dt$ with $Q \succeq 0$, $R \succ 0$. Find $\mathbf{u}(t)$ minimizing $J$. $Q$ penalizes state deviation; $R$ penalizes control effort. Larger $Q$: more aggressive state regulation; larger $R$: more conservative control. Trade-off encoded in weighting matrices.

Q: State the LQR SOLUTION.
A: Optimal control is STATE FEEDBACK: $\mathbf{u}^*(t) = -K\mathbf{x}(t)$ where $K = R^{-1} B^T P$ and $P$ is the positive-definite solution to the ALGEBRAIC RICCATI EQUATION: $A^T P + P A - P B R^{-1} B^T P + Q = 0$. Closed-loop: $\dot{\mathbf{x}} = (A - BK)\mathbf{x}$, always stable (if $(A, B)$ stabilizable and $(A, \sqrt{Q})$ detectable). LQR is STATE FEEDBACK with OPTIMAL gain.

## 12.3 The Riccati Equation

C: The [algebraic Riccati equation (ARE)] is $A^T P + PA - PBR^{-1}B^T P + Q = 0$, a MATRIX QUADRATIC equation for $P$.

Q: How do you SOLVE the Riccati equation?
A: (1) [Analytical]: for small systems (n = 1, 2), solve directly. (2) [HAMILTONIAN MATRIX]: form $H = \begin{pmatrix} A & -BR^{-1}B^T \\ -Q & -A^T \end{pmatrix}$; stable eigenvectors of $H$ give $P$. (3) [Iterative]: Newton's method starting from initial guess. Standard software (MATLAB's `lqr`, Python's `scipy.linalg.solve_continuous_are`) implements these robustly. Always use software for $n > 3$.

## 12.4 LQR Design Parameters

Q: How do you CHOOSE $Q$ and $R$ for LQR?
A: (1) Diagonal $Q = \text{diag}(q_i)$ with $q_i = 1/\text{max}(x_i)^2$ — state normalized to unit range. (2) Similarly $R = \text{diag}(r_i)$ with $r_i = 1/\text{max}(u_i)^2$. (3) Overall SCALE of $Q/R$: ratio controls aggressiveness (larger ratio = faster, more control effort). Tune by simulation. "BRYSON'S RULE" — standard starting point for LQR weights. Iterate based on response characteristics.

Q: What's the ROBUSTNESS of LQR controllers?
A: LQR has GUARANTEED MARGINS (Safonov-Athans): GM = $[-6 \text{ dB}, +\infty]$ (infinite upper gain margin), PM $\geq 60°$. ROBUST to gain and phase perturbations at the input. Best-known automatic-margin result in control. CATCH: margins are only at the input; output perturbations can be worse. Compound robustness via $\mu$-synthesis or $H_\infty$ handles both.

## 12.5 LQR for Tracking

Q: How do you extend LQR to TRACK a reference?
A: Add INTEGRAL STATE: $x_I = \int (r - y) dt$. Augmented system with states $[\mathbf{x}, x_I]$. Penalize $x_I^2$ in cost to drive tracking error to zero. Equivalent to adding integral action to the controller. Alternatively: FEEDFORWARD — compute input bias $u_\text{ff} = -N_u r$ from steady-state analysis to set output to reference. Combining: $u = -K\mathbf{x} + N_u r$ for accurate reference tracking without steady-state error.

## 12.6 The Kalman Filter

Q: Describe the [Kalman filter] for state estimation with noise.
A: For system $\dot{\mathbf{x}} = A\mathbf{x} + B\mathbf{u} + \mathbf{w}$, $\mathbf{y} = C\mathbf{x} + \mathbf{v}$ with zero-mean Gaussian white noise $\mathbf{w} \sim \mathcal{N}(0, W)$, $\mathbf{v} \sim \mathcal{N}(0, V)$: the Kalman filter is a Luenberger observer with OPTIMAL gain $L = PC^T V^{-1}$, where $P$ solves the DUAL Riccati equation: $AP + PA^T - PC^T V^{-1} CP + W = 0$. Minimizes expected estimation error variance — OPTIMAL observer under Gaussian noise.

Q: What does the KALMAN FILTER TRADE OFF?
A: [Process noise covariance $W$]: quantifies uncertainty in dynamics. Large $W$: trust measurements more (high $L$). [Measurement noise $V$]: quantifies sensor noise. Large $V$: trust model more (low $L$). Optimal $L$ balances these. Celebrated application: Apollo navigation (1960s) — first major success of Kalman filtering. Now ubiquitous in: GPS, radar tracking, SLAM, spacecraft, autopilots.

## 12.7 LQG Control

Q: Describe the [Linear Quadratic Gaussian (LQG)] problem and its solution.
A: Same LQR cost but state measured with noise: output $\mathbf{y} = C\mathbf{x} + \mathbf{v}$, process noise $\mathbf{w}$. Optimal control: use KALMAN FILTER to estimate $\hat{\mathbf{x}}$; apply LQR gain $\mathbf{u} = -K\hat{\mathbf{x}}$. Combines CONTROL and ESTIMATION separately via SEPARATION PRINCIPLE — still applies here. LQG is the OPTIMAL stochastic output-feedback controller for linear Gaussian systems. Celebrated; workhorse of modern control.

Q: Why is LQG ROBUSTNESS problematic?
A: LQR alone has guaranteed margins. LQG (with Kalman filter) has NO GUARANTEED MARGINS — can be fragile to model uncertainty (Doyle 1978: "Guaranteed Margins for LQG Regulators"). Remedy: [LQG/LTR (Loop Transfer Recovery)] — carefully tune Kalman weights to recover LQR-like robustness. Or: use $H_\infty$ or $\mu$-synthesis for explicitly robust design. A sobering reminder that combining optimal pieces doesn't guarantee optimal combined performance.

## 12.8 Dynamic Programming and Bellman

Q: State [Bellman's principle of optimality].
A: "An optimal policy has the property that whatever the initial state and initial decision are, the REMAINING DECISIONS must constitute an optimal policy with regard to the state resulting from the first decision." Principle underlying dynamic programming. Yields the [Hamilton-Jacobi-Bellman (HJB) equation]: $-\partial V/\partial t = \min_u \{L + (\partial V/\partial \mathbf{x})^T f\}$ for the optimal value function $V$. For LQR: HJB reduces to Riccati. For nonlinear: usually intractable — approximate via reinforcement learning, policy iteration.

## 12.9 Pontryagin's Maximum Principle

Q: State the [Pontryagin maximum principle].
A: NECESSARY conditions for optimal $\mathbf{u}^*(t)$: introduce COSTATE $\boldsymbol{\lambda}(t)$, form the HAMILTONIAN $H = L + \boldsymbol{\lambda}^T f$. Then: (1) $\dot{\boldsymbol{\lambda}} = -\partial H / \partial \mathbf{x}$. (2) $\mathbf{u}^*(t) = \arg\min_{\mathbf{u}} H$. (3) Boundary conditions on $\boldsymbol{\lambda}$ (transversality). Elegantly handles INEQUALITY CONSTRAINTS on $\mathbf{u}$ — optimal control lies on boundary when unconstrained minimum is infeasible. Gives BANG-BANG controllers for time-optimal problems.

## 12.10 Bang-Bang Control

Q: What is [BANG-BANG CONTROL]?
A: Control that switches between MINIMUM and MAXIMUM values (no intermediate) — optimal for TIME-OPTIMAL problems subject to control bounds. Example: fastest way to move a car from 0 to 100 km distance: FULL ACCELERATION then FULL BRAKING. Derived from Pontryagin. Results in bang-bang or "switching" controllers, not smooth. Useful theoretically; practically often replaced by smooth approximations due to actuator wear. Sliding-mode control extends this idea.

## 12.11 Model Predictive Control (MPC)

Q: Describe [Model Predictive Control (MPC)].
A: At each time step: solve a FINITE-HORIZON optimal control problem over prediction horizon $N$, starting from current state. Apply only the FIRST CONTROL ACTION, then re-optimize at the next step (RECEDING HORIZON). Handles CONSTRAINTS explicitly (actuator limits, state bounds, safety). Computational cost: solve a QP or NLP every sample time. DOMINANT in process industries, increasingly in automotive, robotics, aerospace. Handles what LQR cannot: hard constraints.

## 12.12 A Worked LQR Design

P: Design LQR for the double integrator $\dot{\mathbf{x}} = \begin{pmatrix} 0 & 1 \\ 0 & 0 \end{pmatrix} \mathbf{x} + \begin{pmatrix} 0 \\ 1 \end{pmatrix} u$ with $Q = I$ (identity), $R = 1$.
S:
**IDENTIFY**: Simplest LQR problem. Solve algebraic Riccati for $P$; gain $K = R^{-1} B^T P = B^T P$.

**PLAN**: Let $P = \begin{pmatrix} p_1 & p_2 \\ p_2 & p_3 \end{pmatrix}$ (symmetric). Plug into Riccati: $A^T P + PA - PB B^T P + Q = 0$. Solve 3 scalar equations.

**EXECUTE**:
$A^T P = \begin{pmatrix} 0 & 0 \\ p_1 & p_2 \end{pmatrix}$, $PA = \begin{pmatrix} 0 & p_1 \\ 0 & p_2 \end{pmatrix}$.
$A^T P + PA = \begin{pmatrix} 0 & p_1 \\ p_1 & 2p_2 \end{pmatrix}$.
$PB = \begin{pmatrix} p_2 \\ p_3 \end{pmatrix}$, $B^T P = (p_2, p_3)$, $PBB^T P = \begin{pmatrix} p_2^2 & p_2 p_3 \\ p_2 p_3 & p_3^2 \end{pmatrix}$.

Riccati: $\begin{pmatrix} 0 & p_1 \\ p_1 & 2p_2 \end{pmatrix} - \begin{pmatrix} p_2^2 & p_2 p_3 \\ p_2 p_3 & p_3^2 \end{pmatrix} + \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} = 0$.

Equations:
- (1,1): $-p_2^2 + 1 = 0 \Rightarrow p_2 = 1$ (positive).
- (1,2): $p_1 - p_2 p_3 = 0 \Rightarrow p_1 = p_3$.
- (2,2): $2p_2 - p_3^2 + 1 = 0 \Rightarrow p_3^2 = 3 \Rightarrow p_3 = \sqrt{3}$.

So $P = \begin{pmatrix} \sqrt{3} & 1 \\ 1 & \sqrt{3} \end{pmatrix}$, $K = B^T P = (1, \sqrt{3})$.

Controller: $u^* = -x_1 - \sqrt{3} x_2$.

Closed-loop: $A - BK = \begin{pmatrix} 0 & 1 \\ -1 & -\sqrt{3} \end{pmatrix}$. Char. poly: $s^2 + \sqrt{3} s + 1$. Poles: $s = (-\sqrt{3} \pm \sqrt{-1})/2 = -\sqrt{3}/2 \pm j/2$.

**EVALUATE**: Damping ratio: $\zeta = \sqrt{3}/2 \approx 0.866$ — well-damped. Natural frequency: $\omega_n = 1$. Classic LQR response: balanced aggressiveness, modest overshoot. Changing $R$ rescales the trade-off: $R = 0.1$ (cheap control) → faster poles, higher gain; $R = 10$ (expensive control) → slower, safer. Closed-loop ALWAYS stable (LQR guarantee). This $(P, K)$ gives OPTIMAL regulation for the chosen $Q, R$ — no other linear controller achieves lower cost.
