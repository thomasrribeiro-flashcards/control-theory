+++
order = 9
subject = "Math"
tags = ["math", "control-theory", "state-space", "mimo", "modern-control", "similarity-transform", "canonical-forms"]
+++

# Control Theory — State-Space Representation

## 9.1 What State-Space Representation Is

C: The [state-space model] of an LTI system is $\dot{\mathbf{x}} = A\mathbf{x} + B\mathbf{u}$, $\mathbf{y} = C\mathbf{x} + D\mathbf{u}$, where $\mathbf{x} \in \mathbb{R}^n$ is the [state], $\mathbf{u} \in \mathbb{R}^m$ is the [input], $\mathbf{y} \in \mathbb{R}^p$ is the [output], and $A, B, C, D$ are constant matrices.

Q: Why is STATE-SPACE more powerful than transfer functions?
A: (1) [MIMO NATIVE]: handles multi-input/output directly; transfer function generalizes awkwardly to a matrix $G(s) = C(sI - A)^{-1}B + D$. (2) [INTERNAL VIEW]: tracks state variables (physical quantities — positions, velocities, currents) rather than just input-output. (3) [Modern tools]: optimal control (LQR), observers (Kalman), nonlinear extensions naturally. (4) [Numerical]: better conditioned for computation. Transfer functions are INPUT-OUTPUT; state-space is INTERNAL + input-output.

## 9.2 Choosing State Variables

Q: How do you CHOOSE STATE VARIABLES for a physical system?
A: State variables = enough information to predict FUTURE behavior given future inputs. For MECHANICAL systems: positions and velocities (or momenta). For ELECTRICAL: voltages on capacitors, currents through inductors (energy-storing elements). For THERMAL: temperatures. Generally: the MINIMUM set of variables whose INITIAL VALUES determine all future trajectories. Different choices lead to SIMILAR (via similarity transform) state-space models.

## 9.3 From ODE to State-Space

Q: Convert the 2nd-order ODE $\ddot{y} + 3\dot{y} + 2y = u$ to state-space form.
A: Choose states: $x_1 = y, x_2 = \dot{y}$. Then:
$\dot{x}_1 = x_2$
$\dot{x}_2 = \ddot{y} = -3\dot{y} - 2y + u = -3x_2 - 2x_1 + u$
In matrix form: $\dot{\mathbf{x}} = \begin{pmatrix} 0 & 1 \\ -2 & -3 \end{pmatrix} \mathbf{x} + \begin{pmatrix} 0 \\ 1 \end{pmatrix} u$, $y = \begin{pmatrix} 1 & 0 \end{pmatrix} \mathbf{x}$.
This is the CONTROLLABLE CANONICAL FORM. General rule for $n$-th order ODE: $n$ states (derivatives of $y$ up to order $n - 1$); $A$ matrix has companion form.

## 9.4 From State-Space to Transfer Function

Q: Derive the TRANSFER FUNCTION from state-space matrices.
A: Take Laplace of $\dot{\mathbf{x}} = A\mathbf{x} + B\mathbf{u}$, $\mathbf{y} = C\mathbf{x} + D\mathbf{u}$ with zero initial conditions: $s\mathbf{X}(s) = A\mathbf{X}(s) + B\mathbf{U}(s)$, so $\mathbf{X}(s) = (sI - A)^{-1} B\mathbf{U}(s)$. Output: $\mathbf{Y}(s) = C(sI - A)^{-1} B\mathbf{U}(s) + D\mathbf{U}(s)$.
So $G(s) = C(sI - A)^{-1}B + D$ — an $p \times m$ matrix of transfer functions (MIMO). Poles are EIGENVALUES of $A$; zeros come from numerator polynomials.

## 9.5 Similarity Transformations

C: A [similarity transformation] $\mathbf{x} = T\bar{\mathbf{x}}$ (for invertible $T$) gives $\dot{\bar{\mathbf{x}}} = \bar{A}\bar{\mathbf{x}} + \bar{B}\mathbf{u}$ with $\bar{A} = T^{-1}AT$, $\bar{B} = T^{-1}B$, $\bar{C} = CT$.

Q: What INVARIANTS are preserved under similarity transformations?
A: EIGENVALUES of $A$ (→ poles of transfer function), TRANSFER FUNCTION $G(s)$ itself, stability, controllability, observability. What CHANGES: numerical values of matrices, individual state variable meanings. Non-uniqueness: MANY state-space realizations of the same transfer function — choose the one that's convenient (canonical form for analysis, physical states for interpretation).

## 9.6 Canonical Forms

Q: Name COMMON canonical state-space forms.
A: (1) [CONTROLLABLE CANONICAL FORM]: $A$ has companion-matrix structure; $B = [0, \ldots, 0, 1]^T$. Used for state-feedback design. (2) [OBSERVABLE CANONICAL FORM]: dual; $A^T$ is companion; $C = [1, 0, \ldots, 0]$. Used for observer design. (3) [MODAL FORM] (diagonal): $A = \text{diag}(\lambda_i)$ — each state is a mode. Decouples dynamics into independent scalar equations. (4) [JORDAN FORM]: for repeated eigenvalues. Each form highlights DIFFERENT PROPERTIES.

## 9.7 Solution of State-Space Equations

Q: Write the SOLUTION of $\dot{\mathbf{x}} = A\mathbf{x} + B\mathbf{u}$, $\mathbf{x}(0) = \mathbf{x}_0$.
A: $\mathbf{x}(t) = e^{At} \mathbf{x}_0 + \int_0^t e^{A(t - \tau)} B \mathbf{u}(\tau) d\tau$. First term: ZERO-INPUT response (natural modes from initial conditions). Second term: ZERO-STATE response (convolution with input). Generalizes the scalar solution $x(t) = e^{at}x_0 + \int e^{a(t-\tau)} b u(\tau) d\tau$. Matrix exponential computed via eigendecomposition or numerically.

## 9.8 Stability in State-Space

Q: State the STABILITY criterion for LTI state-space systems.
A: System $\dot{\mathbf{x}} = A\mathbf{x}$ is ASYMPTOTICALLY STABLE iff ALL EIGENVALUES of $A$ have NEGATIVE REAL PART. Equivalent: $e^{At} \to 0$ as $t \to \infty$. For stability of input-output behavior, also need controllable/observable modes — otherwise hidden unstable modes may not appear at output but destabilize internal states. Minimal realization of TF: poles are ALL eigenvalues.

## 9.9 Modal Decomposition

Q: Describe the [modal decomposition] of a state-space system.
A: If $A$ has distinct eigenvalues: diagonalize $A = PDP^{-1}$ where $D = \text{diag}(\lambda_i)$ and columns of $P$ are eigenvectors. Transform: $\mathbf{z} = P^{-1}\mathbf{x}$, so $\dot{z}_i = \lambda_i z_i + (P^{-1}B)_i \mathbf{u}$. Each mode $z_i$ EVOLVES INDEPENDENTLY. Solution: $z_i(t) = e^{\lambda_i t} z_i(0) + \int e^{\lambda_i(t - \tau)}(P^{-1}B)_i \mathbf{u}(\tau) d\tau$. Cleanly separates the dynamics.

## 9.10 MIMO Transfer Function Matrices

Q: Describe the structure of a MIMO TRANSFER FUNCTION MATRIX.
A: $G(s) = C(sI - A)^{-1}B + D$ is $p \times m$. Entry $G_{ij}(s)$ = transfer from input $j$ to output $i$. Denominator of each entry: CHARACTERISTIC POLYNOMIAL of $A$ (common). Numerators: specific to each input-output pair. Coupling between loops: off-diagonal entries. DIAGONAL $G$: decoupled (each input affects only its output) — desirable but rare. DECOUPLING control aims to invert $G$ to achieve diagonal closed-loop.

## 9.11 Equilibrium Points

C: An [equilibrium] of $\dot{\mathbf{x}} = A\mathbf{x} + B\mathbf{u}$ with constant input $\mathbf{u}^*$ is a state $\mathbf{x}^*$ satisfying $A\mathbf{x}^* + B\mathbf{u}^* = 0$.

Q: Solve for the EQUILIBRIUM given constant input.
A: $\mathbf{x}^* = -A^{-1} B \mathbf{u}^*$, VALID when $A$ is invertible (no zero eigenvalues; no pure integrators). If $A$ is singular: integrators make equilibrium depend on initial conditions (LTI integrators accept any constant as equilibrium when input is zero). For stable linear systems: equilibrium is unique and globally stable. Output at equilibrium: $\mathbf{y}^* = -CA^{-1}B \mathbf{u}^* + D\mathbf{u}^* = G(0)\mathbf{u}^*$.

## 9.12 A Worked State-Space Model

P: Derive the state-space model of a simple PENDULUM of mass $m$, length $l$, linearized about the downward equilibrium: $ml\ddot\theta + mg\sin\theta = \tau$ (input torque $\tau$).
S:
**IDENTIFY**: Nonlinear 2nd-order ODE; linearize about $\theta = 0$ where $\sin\theta \approx \theta$.

**PLAN**: Choose states $(x_1, x_2) = (\theta, \dot\theta)$; express $\dot{x}_1, \dot{x}_2$.

**EXECUTE**:
Linearize: $ml\ddot\theta + mg\theta = \tau$ near $\theta = 0$.

$\dot{x}_1 = x_2$
$\dot{x}_2 = \ddot\theta = -\frac{g}{l} \theta + \frac{\tau}{ml^2} = -\frac{g}{l} x_1 + \frac{\tau}{ml^2}$

Assuming units $m l^2 = 1$ for simplicity (or include appropriate scaling):

$\dot{\mathbf{x}} = \begin{pmatrix} 0 & 1 \\ -g/l & 0 \end{pmatrix} \mathbf{x} + \begin{pmatrix} 0 \\ 1/(ml^2) \end{pmatrix} \tau$

Output (angle): $y = \theta = \begin{pmatrix} 1 & 0 \end{pmatrix} \mathbf{x}$.

Eigenvalues of $A$: $\det(\lambda I - A) = \lambda^2 + g/l = 0 \Rightarrow \lambda = \pm j\sqrt{g/l}$ — pure imaginary, MARGINALLY STABLE. Corresponds to undamped oscillator at natural frequency $\omega_n = \sqrt{g/l}$ — the familiar pendulum frequency.

Transfer function: $G(s) = C(sI - A)^{-1} B = \frac{1}{s^2 + g/l} \cdot \frac{1}{ml^2}$.

**EVALUATE**: The linearized pendulum is a MARGINALLY STABLE oscillator — no damping in the model. Real pendulums have friction, giving $-c \dot\theta$ term → stable spiral. Pole placement via state feedback (chapter 11) could ARBITRARILY relocate these poles. For the INVERTED pendulum (unstable equilibrium at $\theta = \pi$): same analysis near $\theta = \pi$, get $A$ with POSITIVE eigenvalue — UNSTABLE. Controlled via feedback — classic control problem. State-space representation enables straightforward extension to nonlinear analysis, stabilization of unstable equilibria, optimal control design.
