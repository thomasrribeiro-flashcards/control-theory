+++
order = 14
subject = "Mathematics"
tags = ["math", "control-theory", "digital-control", "z-transform", "sampling", "robotics", "aerospace", "applications"]
+++

# Control Theory — Digital Control and Applications

## 14.1 Why Digital Control

Q: Why are DIGITAL controller implementations FLEXIBLE?
A: The algorithm changes via SOFTWARE — no hardware rework. Even analog-era techniques (PID) are implemented digitally today.

Q: Why does DIGITAL control beat analog on PRECISION?
A: Floating-point arithmetic beats analog component tolerances.

Q: Which COMPLEX control algorithms are naturally digital?
A: MPC, Kalman filters, adaptive control.

Q: Besides flexibility, precision, and algorithm complexity, what PRACTICAL factors drive digital control?
A: Cheap, ubiquitous microcontrollers; straightforward diagnostics and logging.

Q: Where is control still implemented in ANALOG despite the digital norm?
A: Very-high-frequency (RF) applications and safety-critical analog backups.

## 14.2 Sampling

C: Sampling converts a continuous-time signal $x(t)$ to discrete samples [$x\lbrack k\rbrack  = x(kT_s)$], where $T_s$ is the sampling period.

Q: State the [NYQUIST-SHANNON SAMPLING THEOREM].
A: A signal containing no frequencies above $f_N$ Hz is COMPLETELY determined by samples taken at rate $f_s \geq 2 f_N$ (the NYQUIST RATE). Below this rate: ALIASING — high frequencies "fold down" indistinguishably into lower frequencies. Practical rule: sample at 10-20× the bandwidth of interest; use anti-aliasing filters before the ADC. Control systems: typical $f_s$ = 10-20× closed-loop bandwidth.

Q: What is [ALIASING]?
A: High-frequency content appearing as low-frequency content after sampling. A sine wave at $f > f_s/2$ is INDISTINGUISHABLE from a sine at $|f_s - f|$ in the sampled signal. Loss of information. Mitigation: [anti-aliasing filter] (low-pass analog filter before ADC) removes frequencies above $f_s/2$. Critical in measurement and control — otherwise noise at $f > f_s/2$ corrupts slow control dynamics.

## 14.3 The Z-Transform

C: The z-transform of a discrete-time sequence $x\lbrack k\rbrack $ is [$X(z) = \sum_{k=0}^\infty x\lbrack k\rbrack  z^{-k}$]. Discrete-time analog of Laplace transform.

Q: Why use the Z-TRANSFORM in digital control?
A: Because it transforms DIFFERENCE EQUATIONS into ALGEBRAIC equations in $z$ — parallel to Laplace for ODEs. Shift operator: $x[k - 1] \to z^{-1} X(z)$. TRANSFER FUNCTION of a discrete system: $G(z) = Y(z)/U(z)$. All classical control tools (root locus, Bode, Nyquist) translate to the $z$-domain with modifications. $z = e^{sT_s}$ maps continuous $s$-plane to $z$-plane.

## 14.4 Stability in the Z-Plane

Q: State the STABILITY CRITERION for discrete-time systems.
A: A discrete LTI system is BIBO STABLE iff ALL POLES $p_i$ lie INSIDE THE UNIT CIRCLE of the $z$-plane ($|p_i| < 1$). Contrast with continuous systems (LHP). Mapping: continuous stable LHP ↔ discrete stable unit disk via $z = e^{sT_s}$. Imaginary axis → unit circle. Origin of $s$-plane → $z = 1$. Pole on unit circle → marginal stability. Same ideas; different geometric boundary.

## 14.5 Discretization Methods

Q: Give the FORWARD EULER discretization substitution and its main weakness.
A: $s \to (z - 1)/T_s$. Simple but can DESTABILIZE a stable analog design.

Q: Give the BACKWARD EULER discretization substitution and its main strength.
A: $s \to (z - 1)/(T_s z)$. Always preserves stability.

Q: Give the TUSTIN (BILINEAR) discretization substitution and its key properties.
A: $s \to \frac{2}{T_s} \frac{z - 1}{z + 1}$. Maps LHP to unit disk (preserves stability), matches magnitudes well; warps frequencies (pre-warping fixes).

Q: Give the ZERO-ORDER HOLD (ZOH) discretization formula, and when it is exact.
A: $G(z) = (1 - z^{-1}) \mathcal{Z}\{G(s)/s\}$. Exact for sampled systems with a ZOH at the input.

Q: Compare FORWARD EULER and TUSTIN discretization.
A: Forward Euler: fast, simple, but can destabilize stable analog systems if $T_s$ is too large (maps only part of LHP to disk). Tustin: preserves stability (LHP ↔ disk), preserves DC gain, matches frequency response best. Both: polynomials $\to$ polynomials. Tustin is the DEFAULT for discretizing analog controllers for digital implementation. Pre-warping Tustin matches a specific frequency exactly — critical for notch filters.

## 14.6 Discrete PID

Q: Write the POSITIONAL form of the DISCRETE PID algorithm (backward differences).
A: $u[k] = K_p e[k] + K_i \cdot T_s \sum_{i=0}^k e[i] + K_d (e[k] - e[k-1])/T_s$

Q: Write the INCREMENTAL form of the DISCRETE PID algorithm.
A: $\Delta u[k] = K_p(e[k] - e[k-1]) + K_i T_s e[k] + K_d (e[k] - 2e[k-1] + e[k-2])/T_s$, then $u[k] = u[k-1] + \Delta u[k]$.

Q: Why do industrial controllers prefer the INCREMENTAL form of discrete PID?
A: It is numerically stable and avoids integral windup naturally — clamping $u[k]$ to the actuator range bounds the implicit integral.

## 14.7 Applications: Aerospace

Q: Which control techniques run aircraft AUTOPILOTS?
A: PID + gain scheduling across the altitude, pitch, and roll loops.

Q: Which two techniques are the core of aerospace GNC (Guidance, Navigation, Control)?
A: Kalman filters for sensor fusion (GPS, IMU, star trackers) + LQR for trajectory tracking.

Q: What controller parametrization handles satellite/rocket ATTITUDE CONTROL?
A: Quaternion-based attitude controllers.

Q: What is FLIGHT ENVELOPE PROTECTION, in control terms?
A: Constrained control that keeps the aircraft within safe flight conditions.

Q: Which control method handles REENTRY and LANDING, with nonlinear dynamics plus constraints?
A: Model predictive control.

Q: What's the INVERTED PENDULUM problem's relationship to aerospace?
A: The inverted pendulum is a PROTOTYPICAL UNSTABLE PLANT — model for rocket launch stability. The Saturn V rocket was essentially an inverted pendulum stabilized by gimballed thrusters. SpaceX's landing relies on PRECISE CONTROL of an inherently unstable body (narrow, top-heavy). Same dynamics: $\ddot\theta = (g/l)\sin\theta - \tau/(ml^2)$. Control challenges (needed high-bandwidth, robust, saturation-aware feedback) drove aerospace control innovations.

## 14.8 Applications: Robotics

Q: Which control methods handle robot JOINT CONTROL?
A: PID or computed torque for each joint — fast position/velocity tracking.

Q: What control structure achieves robot TRAJECTORY TRACKING?
A: Feedforward + PD + adaptive compensation for dynamics.

Q: Which control approaches enable robot FORCE CONTROL for physical interaction (grasping, assembly)?
A: Impedance/admittance control.

Q: How do robotic systems split MOTION PLANNING from CONTROL?
A: A planner produces the reference; a tracking controller follows it.

Q: Which estimators power robot LOCALIZATION and MAPPING (SLAM)?
A: Kalman and particle filters.

Q: Name MODERN extensions of classical robot control.
A: RL for manipulation, visual servoing, humanoid walking via zero moment point control.

## 14.9 Applications: Process Control

Q: Which controllers run the basic PROCESS loops (level, temperature, flow)?
A: PI controllers, with gain scheduling or MPC where needed.

Q: Which control method handles MIMO process units (distillation columns, reactors) with constraints?
A: MIMO model predictive control (MPC) — the dominant advanced method in recent decades.

Q: Which techniques handle LONG DEAD TIMES in process control?
A: Smith predictors and model predictive control.

Q: Which control technique suits repetitive BATCH processes?
A: Iterative learning control.

Q: How are the thousands of PID loops in a chemical plant coordinated, given reliability and safety demands?
A: DCS (Distributed Control Systems) — with conservative tuning, redundant sensors, extensive alarm systems.

## 14.10 Applications: Power Systems

Q: What regulates generator VOLTAGE in power systems?
A: Automatic voltage regulators (AVR).

Q: How is GRID FREQUENCY held at 50/60 Hz despite load changes?
A: Load-frequency control: droop control + a secondary PI loop.

Q: What do POWER SYSTEM STABILIZERS do?
A: Damp electromechanical oscillations via supplementary control signals.

Q: What do FACTS devices control in the power grid?
A: Reactive power and voltage, via power electronics.

Q: How are MICROGRIDS and renewables controlled?
A: Fast power-electronic controllers for inverter-based generation.

## 14.11 Applications: Biomedical

Q: How does the ARTIFICIAL PANCREAS use control theory?
A: Closed-loop insulin delivery driven by continuous glucose monitoring — MPC handles glucose constraints and meal disturbances.

Q: How is control theory used for DRUG DOSING (anesthesia, chemotherapy)?
A: PK/PD models + feedback control to optimize therapeutic drug levels.

Q: How is control used in VENTILATORS?
A: Pressure/volume control against lung dynamics.

Q: Name BIOMEDICAL closed-loop applications in neural and cardiac devices.
A: Cardiac pacemakers (rate/rhythm control), deep brain stimulation (closed-loop Parkinson's therapy), brain-machine interfaces (decoding neural signals to control prosthetics).

## 14.12 A Worked Digital Controller

P: Discretize a continuous PI controller $C(s) = K_p + K_i/s$ with $K_p = 2, K_i = 0.5$, using backward Euler at sampling period $T_s = 0.01$ s.
S:
**IDENTIFY**: Convert $C(s) \to C(z)$ via $s = (z - 1)/(T_s z)$.

**PLAN**: Substitute and simplify; convert to difference equation.

**EXECUTE**:
$C(z) = K_p + K_i \cdot \frac{T_s z}{z - 1} = 2 + \frac{0.5 \cdot 0.01 \cdot z}{z - 1} = 2 + \frac{0.005 z}{z - 1}$.

Combine: $C(z) = \frac{2(z - 1) + 0.005 z}{z - 1} = \frac{2.005 z - 2}{z - 1}$.

So $(z - 1) U(z) = (2.005 z - 2) E(z)$, i.e., $U(z) z - U(z) = 2.005 z E(z) - 2 E(z)$.

Difference equation: $u[k+1] - u[k] = 2.005 e[k+1] - 2 e[k]$.

Rearranged: $u[k] = u[k-1] + 2.005 e[k] - 2 e[k-1]$.

**EVALUATE**: Implementation: at each sample, compute new error $e[k]$, update $u[k]$ using previous input and errors. The factor $2.005$ ≈ $K_p + K_i T_s$ = $2 + 0.005 = 2.005$ ✓. Factor $2$ = $K_p$. Essentially: proportional-on-error with a corrective increment from the integral accumulation. Numerically identical to backward-Euler PID at this sampling rate. Compare to incremental form: $\Delta u[k] = 2.005 e[k] - 2 e[k-1]$; $u[k] = u[k-1] + \Delta u[k]$ — same thing. Real implementations: add ANTI-WINDUP (limit $u[k]$ to actuator range) and DERIVATIVE TERM (filtered difference of $e$). A complete digital PID is ~15 lines of C code — enabled by this discretization workflow.
