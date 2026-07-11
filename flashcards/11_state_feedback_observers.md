+++
order = 11
subject = "mathematics"
tags = ["math", "control-theory", "state-feedback", "pole-placement", "observer", "luenberger", "ackermann"]
+++

# Control Theory — State Feedback and Observers

## 11.1 State Feedback Law

C: A [state-feedback controller] is $\mathbf{u} = -K\mathbf{x} + r$, where $K$ is a constant gain matrix and $r$ is a reference input. Closed-loop: $\dot{\mathbf{x}} = (A - BK)\mathbf{x} + Br$.

Q: What does STATE FEEDBACK offer over output feedback?
A: ARBITRARILY placeable closed-loop poles (if controllable), direct MIMO handling, and natural formulation of optimal control (LQR) — full access to the internal state enables richer control laws.

Q: What is the main DOWNSIDE of state feedback, and its standard remedy?
A: It requires MEASURING ALL STATES — often impractical. Remedy: OBSERVERS estimate states from outputs; combined state-feedback + observer = OUTPUT FEEDBACK with state-space power.

## 11.2 Pole Placement

Q: State the POLE PLACEMENT theorem.
A: If $(A, B)$ is CONTROLLABLE, then for ANY desired set of $n$ closed-loop poles (closed under complex conjugation, since $A, B$ are real), there exists a gain $K$ such that eigenvalues of $A - BK$ match the desired poles. Controllability GUARANTEES we can place poles anywhere. Practical limits: aggressive pole placement requires large $K$ → HIGH GAIN → saturation and noise amplification. Pole placement is DESIGN ART of choosing "reasonable" targets.

## 11.3 Computing the Gain

Q: Describe [Ackermann's formula] for pole placement.
A: For SISO systems with controllable $(A, B)$ and desired characteristic polynomial $p(s) = s^n + \alpha_{n-1} s^{n-1} + \cdots + \alpha_0$: $K = [0, \ldots, 0, 1] \mathcal{C}^{-1} p(A)$, where $\mathcal{C} = [B \ AB \ \cdots \ A^{n-1}B]$ is the controllability matrix and $p(A) = A^n + \alpha_{n-1}A^{n-1} + \cdots + \alpha_0 I$. Direct formula, though numerically ill-conditioned for high order. Modern alternative: robust pole placement algorithms (Kautsky-Nichols).

## 11.4 Choosing Target Poles

Q: How do you CHOOSE desired closed-loop poles in pole placement?
A: Based on performance specs: (1) DOMINANT pair for transient behavior: $\zeta, \omega_n$ chosen for rise time, overshoot, settling time. (2) OTHER poles: place at least 5-10× further left than dominant — so they contribute negligibly. (3) Don't place poles TOO far into LHP — requires high gain, amplifies noise, saturates actuators. (4) Avoid placing poles at transmission zeros. TRADE-OFF between performance (fast poles) and robustness (gentle control).

## 11.5 State Observers

Q: Describe the [Luenberger observer].
A: Observer dynamics: $\dot{\hat{\mathbf{x}}} = A\hat{\mathbf{x}} + Bu + L(\mathbf{y} - C\hat{\mathbf{x}})$. Replicates plant dynamics ($A\hat{\mathbf{x}} + Bu$) plus a CORRECTION proportional to the output error ($y - C\hat{x}$). $L$ = observer gain. Estimation error $\mathbf{e} = \mathbf{x} - \hat{\mathbf{x}}$ satisfies $\dot{\mathbf{e}} = (A - LC)\mathbf{e}$. Stable if eigenvalues of $A - LC$ have negative real part. DUAL of state feedback: pick $L$ to make $A - LC$ have desired eigenvalues (pole placement for $(A^T, C^T)$).

Q: Why does the OBSERVER work?
A: Because the ERROR dynamics $\dot{\mathbf{e}} = (A - LC)\mathbf{e}$ are INDEPENDENT of input $u$ — the observer converges regardless of inputs. Any stable $A - LC$ (poles in LHP) makes $\mathbf{e} \to 0$, so $\hat{\mathbf{x}} \to \mathbf{x}$. Design task: place eigenvalues of $A - LC$ "fast enough" for quick convergence (typically 5-10× faster than closed-loop plant poles).

## 11.6 Separation Principle

Q: State the [separation principle] for observer-based state feedback.
A: Implementing $\mathbf{u} = -K\hat{\mathbf{x}} + r$ (state feedback with observed state instead of true state) yields closed-loop system with eigenvalues = UNION of eigenvalues of $(A - BK)$ (state-feedback poles) AND eigenvalues of $(A - LC)$ (observer poles). DESIGN EACH SEPARATELY: pole placement for $K$, observer design for $L$, combine fearlessly. Magical result enabling modular design of MIMO output-feedback controllers.

Q: Why does the separation principle HOLD?
A: Consider combined state $(\mathbf{x}, \mathbf{e})$ where $\mathbf{e} = \mathbf{x} - \hat{\mathbf{x}}$. The closed-loop dynamics in these coordinates become block-triangular: $\dot{\mathbf{x}} = (A - BK)\mathbf{x} + BK\mathbf{e}$, $\dot{\mathbf{e}} = (A - LC)\mathbf{e}$. BLOCK-TRIANGULAR → eigenvalues are UNION of block diagonals. Observer error decays independently; once $\mathbf{e} \to 0$, plant behaves as with true state feedback. Decouples design.

## 11.7 Observer Dimension and Reduced-Order Observers

Q: What are [reduced-order observers]?
A: If $C$ has FULL ROW RANK $p$ (outputs directly observe $p$ state components), the remaining $n - p$ states can be estimated by a lower-order observer. Saves computation. Construction: partition state into measured + unmeasured; use measured part directly; estimate unmeasured part with Luenberger-like observer of order $n - p$. Common in robotics where position is measured and velocity must be estimated.

## 11.8 Integral Action with State Feedback

Q: How to achieve ZERO STEADY-STATE ERROR with state feedback?
A: Augment state with the INTEGRAL of tracking error: add new state $x_I = \int e \, dt = \int(r - y) dt$. New system: $[\dot{\mathbf{x}}, \dot{x}_I]$ with $\dot{x}_I = r - C\mathbf{x}$. Apply state feedback to augmented system. The integral state forces zero steady-state tracking error by the same logic as PI controllers — integrator in the loop ensures $e_\text{ss} = 0$. Mirrors PID's role in SISO design.

## 11.9 Disturbance Rejection

Q: How does state feedback handle DISTURBANCES?
A: For constant disturbances: augment state with disturbance estimate (if disturbance is modeled). Equivalently, use integral action (previous section) to handle unknown constant disturbances. For more general disturbances (sinusoidal, time-varying): use DISTURBANCE OBSERVERS — a parallel observer estimating the disturbance from input-output mismatch. Feed-forward the disturbance estimate to cancel it — INTERNAL MODEL PRINCIPLE.

## 11.10 Output Feedback

Q: Why is OUTPUT FEEDBACK (static) harder than STATE FEEDBACK?
A: Because output feedback $\mathbf{u} = -K\mathbf{y} = -KC\mathbf{x}$ constrains $K$ to a subspace (only $KC$ effectively multiplies $\mathbf{x}$). Can't arbitrarily place poles — only place them in some achievable set. For SISO: static output feedback places 1 pole (unless specific structure); state feedback places all. Hence OBSERVER-BASED output feedback is usually superior — gets full pole placement via $\hat{\mathbf{x}}$ estimate plus state-feedback law.

## 11.11 Robustness of State Feedback

Q: What ROBUSTNESS to model uncertainty does state feedback provide?
A: LQR (chapter 12) gives GUARANTEED gain margin (infinite) and phase margin (60°) for the loop broken at the input. Pole placement alone offers no inherent robustness guarantee — aggressive placement can be fragile. RULE OF THUMB: don't place poles much faster than needed, don't cancel lightly-damped plant modes. Modern methods (LQG/LTR, H-infinity) explicitly optimize for robustness — covered in later chapters.

## 11.12 A Worked Pole Placement

P: For the plant $A = \begin{pmatrix} 0 & 1 \\ 0 & 0 \end{pmatrix}$, $B = \begin{pmatrix} 0 \\ 1 \end{pmatrix}$ (double integrator — position and velocity), design state feedback placing poles at $-2 \pm 2j$.
S:
**IDENTIFY**: Controllable pair. Desired char. poly: $(s + 2 - 2j)(s + 2 + 2j) = s^2 + 4s + 8$.

**PLAN**: Check controllability. Compute $K = [k_1, k_2]$ making $\det(sI - A + BK) = s^2 + 4s + 8$.

**EXECUTE**:
Controllability: $\mathcal{C} = [B \ AB] = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}$, rank 2 ✓.

$A - BK = \begin{pmatrix} 0 & 1 \\ -k_1 & -k_2 \end{pmatrix}$.

Char. poly: $\det(sI - (A - BK)) = \det \begin{pmatrix} s & -1 \\ k_1 & s + k_2 \end{pmatrix} = s(s + k_2) + k_1 = s^2 + k_2 s + k_1$.

Match to desired $s^2 + 4s + 8$: $k_1 = 8, k_2 = 4$.

So $K = [8, 4]$, giving control law $u = -8 x_1 - 4 x_2$ (position and velocity feedback).

**EVALUATE**: Closed-loop poles: $-2 \pm 2j$ → $\omega_n = \sqrt{8} \approx 2.83$, $\zeta = 4/(2\omega_n) = 0.707$ — textbook damping. Settling time $\approx 4/(\zeta\omega_n) = 4/2 = 2$ s. Overshoot ~4.3%. Control law: position gain 8, velocity gain 4 — think of it as a PD controller: $u = -K_p x_1 - K_d x_2$ with $K_p = 8, K_d = 4$. PD on double integrator = classical aerospace control (e.g., spacecraft attitude). State feedback achieves this with CLEAN DESIGN SEMANTICS: desired poles → gain values directly via Ackermann-like algebra.
