+++
order = 10
subject = "mathematics"
tags = ["math", "control-theory", "controllability", "observability", "duality", "kalman-decomposition", "minimal-realization"]
+++

# Control Theory — Controllability and Observability

## 10.1 Controllability

C: A system $\dot{\mathbf{x}} = A\mathbf{x} + B\mathbf{u}$ is [controllable] if for any initial state $\mathbf{x}_0$ and target state $\mathbf{x}_f$, there exists an input $\mathbf{u}(t)$ on some finite interval $\lbrack 0, T\rbrack $ steering $\mathbf{x}(0) = \mathbf{x}_0$ to $\mathbf{x}(T) = \mathbf{x}_f$.

Q: Why is CONTROLLABILITY fundamental?
A: Because it answers: "Can the input INFLUENCE every state variable?" If not controllable, some state components are INDEPENDENT of input — uncontrollable modes cannot be affected. Controllability is PREREQUISITE for: pole placement (chapter 11), optimal control (LQR), any design that requires steering the system. Uncontrollable stable modes are acceptable; uncontrollable UNSTABLE modes → system cannot be stabilized.

## 10.2 The Controllability Matrix

Q: State the CONTROLLABILITY matrix test.
A: The [controllability matrix] is $\mathcal{C} = [B \ AB \ A^2 B \ \cdots \ A^{n-1} B]$ — an $n \times (nm)$ matrix for $n$ states, $m$ inputs. System is CONTROLLABLE iff $\mathcal{C}$ has FULL ROW RANK ($= n$). Proof idea: $\mathbf{x}(T) = e^{AT}\mathbf{x}_0 + \int_0^T e^{A(T-\tau)} B \mathbf{u}(\tau) d\tau$; reaching arbitrary $\mathbf{x}_f$ requires the RANGE of the integral term to span $\mathbb{R}^n$ — equivalent to $\mathcal{C}$ being full rank.

Q: Apply the controllability test to $A = \begin{pmatrix} 0 & 1 \\ -2 & -3 \end{pmatrix}$, $B = \begin{pmatrix} 0 \\ 1 \end{pmatrix}$.
A: $\mathcal{C} = [B \ AB] = \begin{pmatrix} 0 & 1 \\ 1 & -3 \end{pmatrix}$. $\det(\mathcal{C}) = 0 \cdot (-3) - 1 \cdot 1 = -1 \neq 0$. Full rank ($=2$) → CONTROLLABLE. Makes sense: $B$ directly affects $\dot{x}_2$, and $x_2$ drives $\dot{x}_1$ via $A$, so both states are influenced.

## 10.3 Observability

C: A system $\dot{\mathbf{x}} = A\mathbf{x}$, $\mathbf{y} = C\mathbf{x}$ is [observable] if the INITIAL STATE $\mathbf{x}(0)$ can be DETERMINED from observations $\mathbf{y}(t)$ on some finite interval $\lbrack 0, T\rbrack $.

Q: Why does OBSERVABILITY matter?
A: Because many control schemes (state-feedback) need to know the STATE — but often only OUTPUTS are directly measurable. Observability ensures we can RECONSTRUCT the state from outputs. OBSERVERS / KALMAN FILTERS (chapter 11) exploit observability to estimate the state. Unobservable modes: effects not visible at the output; present but hidden. Like controllability, observability is DESIGN PREREQUISITE.

## 10.4 The Observability Matrix

Q: State the OBSERVABILITY matrix test.
A: The [observability matrix] is $\mathcal{O} = \begin{pmatrix} C \\ CA \\ CA^2 \\ \vdots \\ CA^{n-1} \end{pmatrix}$ — an $(np) \times n$ matrix. System is OBSERVABLE iff $\mathcal{O}$ has FULL COLUMN RANK ($= n$). Proof: outputs and derivatives at time 0: $\mathbf{y}(0), \dot{\mathbf{y}}(0), \ldots$ = $C\mathbf{x}_0, CA\mathbf{x}_0, CA^2\mathbf{x}_0, \ldots$ Stacking: $\mathcal{O} \mathbf{x}_0$. Recovering $\mathbf{x}_0$ needs $\mathcal{O}$ full rank.

## 10.5 Duality

Q: State the [duality] between controllability and observability.
A: $(A, B)$ is controllable iff $(A^T, B^T)$ is observable. $(A, C)$ is observable iff $(A^T, C^T)$ is controllable. Swapping the role of $B$ and $C^T$, and transposing $A$, exchanges the two properties. CONSEQUENCE: any result about controllability has a DUAL result about observability (and vice versa). Algorithmic consequence: state feedback design and observer design are essentially SAME problem in dual form.

## 10.6 The PBH Test

Q: State the [Popov-Belevitch-Hautus (PBH) test] for controllability.
A: System is CONTROLLABLE iff for every eigenvalue $\lambda$ of $A$, the matrix $[\lambda I - A \ B]$ has FULL ROW RANK. Equivalently: there's NO left eigenvector $v$ of $A$ with $v^T B = 0$. Dual test for observability: $\begin{pmatrix} \lambda I - A \\ C \end{pmatrix}$ has full column rank for every eigenvalue. Useful for identifying WHICH MODES are uncontrollable/unobservable (those corresponding to the deficient eigenvalue).

## 10.7 Kalman Decomposition

Q: Describe the [Kalman decomposition].
A: Any state-space system can be transformed (via similarity) into a block-triangular form decomposing the state into four subspaces: (1) CONTROLLABLE and OBSERVABLE (CO), (2) controllable but UNOBSERVABLE (CU), (3) UNCONTROLLABLE but OBSERVABLE (UO), (4) uncontrollable and unobservable (UU). ONLY the CO part appears in the transfer function — the other parts are "hidden dynamics." Essential: the $TF = $ only CO part; other modes may be unstable without showing in $TF$.

## 10.8 Minimal Realization

C: A state-space model is a [minimal realization] of a transfer function $G(s)$ if it has the SMALLEST possible state dimension $n$ — equivalently, if it is both CONTROLLABLE and OBSERVABLE.

Q: Why are MINIMAL REALIZATIONS IMPORTANT?
A: Because non-minimal realizations hide cancelled pole-zero pairs as UNCONTROLLABLE OR UNOBSERVABLE MODES. These hidden modes may be STABLE (fine) or UNSTABLE (disastrous — stable transfer function but unstable internal dynamics). Minimal realization gives the TRUE DIMENSION of the dynamics and exposes any problematic modes. All good design starts from minimal (or at least controllable + observable) realizations.

## 10.9 Pole-Zero Cancellation and Hidden Modes

Q: What's the danger of POLE-ZERO CANCELLATION in controllers?
A: Cancelling an UNSTABLE plant pole with a controller zero creates a HIDDEN UNSTABLE MODE: transfer function looks stable, but state-space reveals an UNCONTROLLABLE (or UNOBSERVABLE) unstable pole. Any disturbance excites this hidden mode → state blows up. NEVER cancel unstable poles/zeros in practice. Cancelling STABLE pole-zero pairs is OK (common in design to simplify).

## 10.10 Stabilizability and Detectability

C: A system is [stabilizable] if all UNCONTROLLABLE MODES are STABLE. A system is [detectable] if all UNOBSERVABLE MODES are stable.

Q: Why are STABILIZABILITY and DETECTABILITY weaker but sufficient conditions?
A: Because for stabilizing control: we only need to CONTROL unstable modes — uncontrollable STABLE modes decay on their own, no need to influence them. Similarly for observation: only unstable modes must be OBSERVED (for feedback); stable unobservable modes are fine. Practical control design often requires only stabilizability + detectability, not full controllability + observability. Weaker than full rank conditions but sufficient for stabilization.

## 10.11 Gramians

C: The [controllability Gramian] is $W_c(t) = \int_0^t e^{A\tau} BB^T e^{A^T \tau} d\tau$. The [observability Gramian] is $W_o(t) = \int_0^t e^{A^T \tau} C^T C e^{A\tau} d\tau$.

Q: What do GRAMIANS MEASURE?
A: Controllability Gramian $W_c$: QUANTIFIES how easy it is to steer to each direction. Steering to direction $\mathbf{v}$ requires energy $\mathbf{v}^T W_c^{-1} \mathbf{v}$. Directions with small Gramian eigenvalues are HARD to reach. Observability Gramian: quantifies how much each direction CONTRIBUTES to output energy. Used in model reduction (BALANCED TRUNCATION): keep directions that are both easy to control AND easily observed. LYAPUNOV equations compute finite-horizon Gramians.

## 10.12 A Worked Controllability Analysis

P: For the system $A = \begin{pmatrix} 1 & 0 \\ 0 & 2 \end{pmatrix}$, $B = \begin{pmatrix} 1 \\ 0 \end{pmatrix}$, $C = \begin{pmatrix} 1 & 1 \end{pmatrix}$: test controllability and observability. Is there a hidden mode?
S:
**IDENTIFY**: 2D diagonal system with eigenvalues $\lambda_1 = 1, \lambda_2 = 2$ (BOTH UNSTABLE in LHP sense — positive real part). Input affects only first state; output sums both.

**PLAN**: Compute $\mathcal{C}$ and $\mathcal{O}$; check ranks.

**EXECUTE**:
Controllability matrix: $\mathcal{C} = [B \ AB] = \begin{pmatrix} 1 & 1 \\ 0 & 0 \end{pmatrix}$. $\det = 0$ → RANK 1, NOT FULL. UNCONTROLLABLE.

The uncontrollable mode: $x_2$ (second state) — input $u$ doesn't appear in $\dot{x}_2 = 2 x_2$. Input can never influence $x_2$. Since $\lambda_2 = 2 > 0$: UNSTABLE uncontrollable mode. System is NOT STABILIZABLE.

Observability matrix: $\mathcal{O} = \begin{pmatrix} C \\ CA \end{pmatrix} = \begin{pmatrix} 1 & 1 \\ 1 & 2 \end{pmatrix}$. $\det = 2 - 1 = 1 \neq 0$ → rank 2, FULL. OBSERVABLE.

Transfer function: $G(s) = C(sI - A)^{-1} B + D = \begin{pmatrix} 1 & 1 \end{pmatrix} \begin{pmatrix} 1/(s-1) & 0 \\ 0 & 1/(s-2) \end{pmatrix} \begin{pmatrix} 1 \\ 0 \end{pmatrix} = \frac{1}{s-1}$.

**EVALUATE**: Transfer function has ONLY ONE POLE at $s = 1$ (unstable). The pole at $s = 2$ is HIDDEN — not in $G(s)$ because state 2 is uncontrollable (cannot be excited by input). But $x_2$ grows unboundedly from any nonzero initial condition. If we try to stabilize the transfer function by feedback, the closed-loop might appear stable — yet $x_2$ still blows up from nonzero IC! CLASSIC EXAMPLE of why transfer-function analysis can miss dangerous hidden modes. State-space analysis REVEALS the problem. Before designing controllers: always check controllability (or at least stabilizability).
