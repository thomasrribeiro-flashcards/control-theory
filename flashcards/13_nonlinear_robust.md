+++
order = 13
subject = "Mathematics"
tags = ["math", "control-theory", "nonlinear-control", "lyapunov", "sliding-mode", "robust-control", "h-infinity", "adaptive"]
+++

# Control Theory — Nonlinear and Robust Control

## 13.1 Beyond Linear

Q: When does LINEARIZATION fail, requiring NONLINEAR CONTROL methods?
A: (1) LARGE operating ranges are required (not just near equilibrium). (2) Fundamentally nonlinear phenomena (hysteresis, saturation, deadzone) dominate. (3) The design exploits specific nonlinear dynamics (robotics with Coriolis, spacecraft with attitude constraints).

Q: Why is NONLINEAR CONTROL harder than linear control?
A: No universal frequency-domain tools — analysis and design are case-by-case.

Q: Name the KEY METHODS of nonlinear control.
A: Lyapunov analysis, feedback linearization, sliding mode, backstepping.

## 13.2 Lyapunov's Direct Method

Q: State [LYAPUNOV'S DIRECT METHOD] for stability of nonlinear systems.
A: For $\dot{\mathbf{x}} = f(\mathbf{x})$ with equilibrium $x^* = 0$: find $V(\mathbf{x})$ (Lyapunov function) with $V(0) = 0$, $V > 0$ elsewhere locally, AND $\dot{V} = \nabla V \cdot f(\mathbf{x}) \leq 0$: equilibrium is STABLE. If $\dot{V} < 0$ (strict): ASYMPTOTICALLY STABLE. If $V$ is RADIALLY UNBOUNDED ($V \to \infty$ as $|\mathbf{x}| \to \infty$) and $\dot{V} < 0$ globally: GLOBALLY ASYMPTOTICALLY STABLE. No need to solve the ODE.

Q: Why is LYAPUNOV'S METHOD so POWERFUL?
A: Because it proves stability WITHOUT SOLVING the ODE. Works for nonlinear, time-varying, uncertain systems. Physical intuition: $V$ is an "energy" — if energy monotonically decreases, system settles. Challenge: finding $V$ is often an ART. For linear systems: quadratic $V = x^T P x$ works with $P$ solving Lyapunov equation $A^T P + PA = -Q$. For nonlinear: need creativity (energy, gradient, weighted sums).

## 13.3 Feedback Linearization

Q: Describe [feedback linearization].
A: For a nonlinear system with appropriate structure: find a change of coordinates AND a feedback law that TRANSFORMS the nonlinear system into a LINEAR one. Then apply standard linear control. Two types: (1) [Input-output linearization]: linearize the input-output relation. (2) [Full-state linearization]: make the entire system linear in transformed coordinates. Conditions: RELATIVE DEGREE, involutive distributions. Exact when the system admits the transformation; approximate otherwise.

Q: Give an EXAMPLE of feedback linearization.
A: Robot arm: $\ddot{q} = -M(q)^{-1}(C(q, \dot{q})\dot{q} + g(q) - \tau)$. Choose torque $\tau = C\dot{q} + g + M \mathbf{v}$: eliminates nonlinearity, leaves $\ddot{q} = \mathbf{v}$ — a DOUBLE INTEGRATOR. Now design $\mathbf{v}$ via LQR or PD. This is the [COMPUTED TORQUE METHOD] in robotics. Requires ACCURATE MODEL — parameter errors leave residual nonlinearities. Robust variants handle this.

## 13.4 Sliding Mode Control

Q: Describe [SLIDING MODE CONTROL].
A: Design a SLIDING SURFACE $s(\mathbf{x}) = 0$ in state space. Choose control to drive $s \to 0$ quickly (REACHING PHASE) and keep it there (SLIDING PHASE). On the sliding surface, dynamics are determined by the surface equation, independent of plant uncertainty. ROBUST to matched disturbances and parameter variations. DISCONTINUOUS control law: $u = -k \cdot \text{sign}(s)$ induces chattering; smooth variants (boundary layer, super-twisting) reduce this.

Q: Why is SLIDING MODE especially ROBUST?
A: Because on the sliding surface, the system's behavior is determined ENTIRELY BY $s(\mathbf{x}) = 0$ — the plant's dynamics enter only through reaching the surface. Uncertainties within a bound are completely REJECTED once sliding. Ideal for: systems with MATCHED DISTURBANCES (disturbances entering through the same channels as control), parameter uncertainties. Applications: robotics, power electronics, aerospace. Downside: CHATTERING (high-frequency switching).

## 13.5 Backstepping

Q: Describe [BACKSTEPPING] control design.
A: Recursive design for systems in STRICT-FEEDBACK FORM: $\dot{x}_i = f_i(x_1, \ldots, x_i) + g_i(\cdot) x_{i+1}$. Design control progressively: (1) Treat $x_2$ as "virtual control" for $x_1$'s subsystem; design $x_2 = \alpha_1(x_1)$ stabilizing $x_1 \to 0$. (2) With actual $x_2$, design "next virtual control" for error $z_2 = x_2 - \alpha_1$. (3) Continue to the final actual control $u$. At each step, construct Lyapunov function. Powerful for nonlinear designs; widely used in: aerospace (missile guidance), electric motor drives.

## 13.6 Adaptive Control

Q: What is [ADAPTIVE CONTROL]?
A: Control strategies that ESTIMATE UNKNOWN PLANT PARAMETERS online and adjust the controller accordingly. Handles systems where parameters (mass, drag, inductance) are unknown or slowly changing. Structure: PARAMETER ESTIMATOR (least squares or gradient update) + CONTROL LAW using current estimates. Classes: [Model Reference Adaptive Control (MRAC)]: adapt so closed-loop matches a reference model. [Self-tuning regulators]: identify plant, design controller repeatedly.

Q: What's the PRIMARY CHALLENGE in adaptive control?
A: ROBUSTNESS — classical adaptive laws can be unstable under unmodeled dynamics, disturbances, or noise. Bursting phenomena, drift — "practical stability" issues. Modern adaptive control: use $\sigma$-modification, $e$-modification, or projection algorithms to bound parameter estimates. Recent: neural network-based and reinforcement learning approaches (deep adaptive control). Active research area with industrial importance.

## 13.7 Robust Control Introduction

Q: What is [ROBUST CONTROL]?
A: Control design ensuring specific properties (stability, performance) for ALL plants in an UNCERTAINTY SET — not just the nominal model. Model uncertainty: unstructured (frequency-domain bounds), structured (parameter ranges), unmodeled dynamics. Tools: $H_\infty$ control, $\mu$-synthesis, LMIs (linear matrix inequalities). Designed-in robustness vs adaptive's online adjustment: complementary approaches.

## 13.8 H-infinity Control

Q: What does $H_\infty$ CONTROL optimize?
A: Minimizes the $H_\infty$ norm (peak gain over frequency) of a closed-loop transfer function from disturbances/noise to errors/outputs. $||T||_\infty = \sup_\omega \bar{\sigma}(T(j\omega))$ where $\bar{\sigma}$ = largest singular value. SMALL $||T||_\infty$ → disturbances and uncertainties have BOUNDED effect for all frequencies. Solution: Riccati equations similar to LQR but with "DGKF" (Doyle-Glover-Khargonekar-Francis) equations. A worst-case design method: optimizes the worst case rather than the average.

## 13.9 Gain Scheduling

Q: What is [GAIN SCHEDULING]?
A: Design LINEAR CONTROLLERS at several OPERATING POINTS; interpolate between them based on a MEASURED SCHEDULING VARIABLE (altitude, velocity, angle of attack). Connects linear-theory tools with nonlinear plants. Widely used in aerospace (flight control throughout envelope). Formal conditions: scheduling variable should vary slowly; stability between scheduled points must be verified. Modern variant: [LPV (Linear Parameter-Varying)] systems for rigorous gain scheduling.

## 13.10 Passivity-Based Control

Q: What is a [PASSIVE] system, and why is passivity useful?
A: A system is passive if $\int_0^t u^T y \, d\tau \geq 0$ (energy "in" exceeds energy "out") — generalizes physical energy dissipation. KEY THEOREM: feedback interconnection of passive systems is stable. Many physical systems are naturally passive (mechanical with dissipation, electrical with resistors). PASSIVITY-BASED CONTROL: design controllers that PRESERVE PASSIVITY. Robust, elegant, especially in robotics (Ortega-Spong, IDA-PBC).

## 13.11 Neural Network and Learning-Based Control

Q: What role do NEURAL NETWORKS play in ADAPTIVE control?
A: NONLINEAR FUNCTION APPROXIMATORS — representing unknown dynamics or controllers.

Q: Besides function approximation in adaptive control, how is MACHINE LEARNING used for control?
A: (1) REINFORCEMENT LEARNING: learn policies by trial and error (deep RL for robotics, games). (2) Imitation learning from expert demonstrations. (3) Model-based learning: identify dynamics, then plan. Blends with classical control: robust training, stability guarantees, safe RL.

## 13.12 A Worked Lyapunov Analysis

P: Show that the origin of $\dot{x}_1 = -x_1 + x_2, \dot{x}_2 = -x_1 - x_2^3$ is asymptotically stable via Lyapunov analysis.
S:
**IDENTIFY**: Nonlinear 2D system. Linearization at origin: $A = \begin{pmatrix} -1 & 1 \\ -1 & 0 \end{pmatrix}$, eigenvalues... wait, $x_2^3$ term vanishes at origin, linearized $\dot{x}_2 = -x_1 - 0$; so $A = \begin{pmatrix} -1 & 1 \\ -1 & 0 \end{pmatrix}$, char. poly $s^2 + s + 1 = 0$, roots $(-1 \pm \sqrt{-3})/2$ — stable spiral. But that's linearization; Lyapunov gives GLOBAL info.

**PLAN**: Try quadratic $V(\mathbf{x}) = \frac{1}{2}(x_1^2 + x_2^2)$; compute $\dot{V}$.

**EXECUTE**:
$V = \frac{1}{2}(x_1^2 + x_2^2)$. Positive definite ($V > 0$ for $\mathbf{x} \neq 0$, $V(0) = 0$), radially unbounded.

$\dot{V} = x_1 \dot{x}_1 + x_2 \dot{x}_2 = x_1(-x_1 + x_2) + x_2(-x_1 - x_2^3) = -x_1^2 + x_1 x_2 - x_1 x_2 - x_2^4 = -x_1^2 - x_2^4$.

Clearly $\dot{V} \leq 0$, with equality iff $x_1 = 0$ AND $x_2 = 0$ (iff $\mathbf{x} = 0$).

$\dot{V}$ is NEGATIVE DEFINITE → GLOBALLY ASYMPTOTICALLY STABLE by Lyapunov's theorem.

**EVALUATE**: Simple quadratic Lyapunov function works because the cross-terms $x_1 x_2$ cancel exactly. The system's $x_2^4$ nonlinearity is STABILIZING (dissipates energy faster than linear damping would). Linearization alone would give asymptotic stability locally; Lyapunov shows it's GLOBAL — much stronger. Real-world use: power systems, coupled oscillators, chemical reactors. The same quadratic $V$ works for many "inherently stable" nonlinear systems; for trickier systems, Lyapunov functions require more cleverness (physical energy, entropy, weighted sums).
