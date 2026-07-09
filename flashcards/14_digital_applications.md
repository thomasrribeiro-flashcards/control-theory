+++
order = 14
subject = "Math"
tags = ["math", "control-theory", "digital-control", "z-transform", "sampling", "robotics", "aerospace", "applications"]
+++

# Control Theory — Digital Control and Applications

## 14.1 Why Digital Control

Q: Why is MOST MODERN CONTROL implemented DIGITALLY?
A: Because (1) MICROCONTROLLERS and computers are cheap and ubiquitous. (2) DIGITAL implementations are FLEXIBLE (change algorithm via software). (3) PRECISION: floating-point arithmetic beats analog component tolerances. (4) COMPLEX ALGORITHMS (MPC, Kalman filters, adaptive control) are naturally digital. (5) DIAGNOSTICS and logging are straightforward. Even analog-era techniques (PID) are implemented digitally today. Exceptions: very-high-frequency (RF), safety-critical analog backups.

## 14.2 Sampling

C: [Sampling] converts a continuous-time signal $x(t)$ to discrete samples $x\lbrack k\rbrack  = x(kT_s)$, where $T_s$ is the SAMPLING PERIOD.

Q: State the [NYQUIST-SHANNON SAMPLING THEOREM].
A: A signal containing no frequencies above $f_N$ Hz is COMPLETELY determined by samples taken at rate $f_s \geq 2 f_N$ (the NYQUIST RATE). Below this rate: ALIASING — high frequencies "fold down" indistinguishably into lower frequencies. Practical rule: sample at 10-20× the bandwidth of interest; use anti-aliasing filters before the ADC. Control systems: typical $f_s$ = 10-20× closed-loop bandwidth.

Q: What is [ALIASING]?
A: High-frequency content appearing as low-frequency content after sampling. A sine wave at $f > f_s/2$ is INDISTINGUISHABLE from a sine at $|f_s - f|$ in the sampled signal. Loss of information. Mitigation: [anti-aliasing filter] (low-pass analog filter before ADC) removes frequencies above $f_s/2$. Critical in measurement and control — otherwise noise at $f > f_s/2$ corrupts slow control dynamics.

## 14.3 The Z-Transform

C: The [z-transform] of a discrete-time sequence $x\lbrack k\rbrack $ is $X(z) = \sum_{k=0}^\infty x\lbrack k\rbrack  z^{-k}$. Discrete-time analog of Laplace transform.

Q: Why use the Z-TRANSFORM in digital control?
A: Because it transforms DIFFERENCE EQUATIONS into ALGEBRAIC equations in $z$ — parallel to Laplace for ODEs. Shift operator: $x[k - 1] \to z^{-1} X(z)$. TRANSFER FUNCTION of a discrete system: $G(z) = Y(z)/U(z)$. All classical control tools (root locus, Bode, Nyquist) translate to the $z$-domain with modifications. $z = e^{sT_s}$ maps continuous $s$-plane to $z$-plane.

## 14.4 Stability in the Z-Plane

Q: State the STABILITY CRITERION for discrete-time systems.
A: A discrete LTI system is BIBO STABLE iff ALL POLES $p_i$ lie INSIDE THE UNIT CIRCLE of the $z$-plane ($|p_i| < 1$). Contrast with continuous systems (LHP). Mapping: continuous stable LHP ↔ discrete stable unit disk via $z = e^{sT_s}$. Imaginary axis → unit circle. Origin of $s$-plane → $z = 1$. Pole on unit circle → marginal stability. Same ideas; different geometric boundary.

## 14.5 Discretization Methods

Q: Describe COMMON DISCRETIZATION methods.
A: (1) [FORWARD EULER]: $s \to (z - 1)/T_s$. Simple but can destabilize. (2) [BACKWARD EULER]: $s \to (z - 1)/(T_s z)$. Always stable. (3) [TUSTIN / BILINEAR]: $s \to \frac{2}{T_s} \frac{z - 1}{z + 1}$. Maps LHP to unit disk (preserves stability), matches magnitudes well; warps frequencies (pre-warping fixes). (4) [ZERO-ORDER HOLD (ZOH)]: $G(z) = (1 - z^{-1}) \mathcal{Z}\{G(s)/s\}$. Exact for sampled systems with ZOH at input. Different methods preserve different properties.

Q: Compare FORWARD EULER and TUSTIN discretization.
A: Forward Euler: fast, simple, but can destabilize stable analog systems if $T_s$ is too large (maps only part of LHP to disk). Tustin: preserves stability (LHP ↔ disk), preserves DC gain, matches frequency response best. Both: polynomials $\to$ polynomials. Tustin is the DEFAULT for discretizing analog controllers for digital implementation. Pre-warping Tustin matches a specific frequency exactly — critical for notch filters.

## 14.6 Discrete PID

Q: Write the DISCRETE PID algorithm.
A: Using backward differences:
- $u[k] = K_p e[k] + K_i \cdot T_s \sum_{i=0}^k e[i] + K_d (e[k] - e[k-1])/T_s$
Or incremental form (avoid windup):
- $\Delta u[k] = K_p(e[k] - e[k-1]) + K_i T_s e[k] + K_d (e[k] - 2e[k-1] + e[k-2])/T_s$
- $u[k] = u[k-1] + \Delta u[k]$
Incremental form is numerically stable and avoids integral windup naturally. Standard implementation in industrial controllers.

## 14.7 Applications: Aerospace

Q: How is CONTROL THEORY used in AEROSPACE?
A: (1) [Autopilots]: PID + gain scheduling (altitude, pitch, roll loops). (2) [Guidance, Navigation, Control (GNC)]: Kalman filters (sensor fusion from GPS, IMU, star trackers), LQR for trajectory tracking. (3) [Attitude control]: quaternion-based controllers for satellites/rockets. (4) [Flight envelope protection]: ensure aircraft stays within safe flight conditions — constrained control. (5) [Reentry, landing]: model predictive control handling nonlinear dynamics + constraints. Aerospace is a CLASSIC control driver — many techniques invented for flight.

Q: What's the INVERTED PENDULUM problem's relationship to aerospace?
A: The inverted pendulum is a PROTOTYPICAL UNSTABLE PLANT — model for rocket launch stability. The Saturn V rocket was essentially an inverted pendulum stabilized by gimballed thrusters. SpaceX's landing relies on PRECISE CONTROL of an inherently unstable body (narrow, top-heavy). Same dynamics: $\ddot\theta = (g/l)\sin\theta - \tau/(ml^2)$. Control challenges (needed high-bandwidth, robust, saturation-aware feedback) drove aerospace control innovations.

## 14.8 Applications: Robotics

Q: How is CONTROL THEORY used in ROBOTICS?
A: (1) [Joint control]: PID or computed torque for each joint — fast position/velocity tracking. (2) [Trajectory tracking]: feedforward + PD + adaptive compensation for dynamics. (3) [Force control]: impedance/admittance control for physical interaction (grasping, assembly). (4) [Motion planning + control]: split problems into planner (reference) + tracker (controller). (5) [Localization/mapping]: Kalman/particle filters for SLAM. Modern extensions: RL for manipulation, visual servoing, humanoid walking via zero moment point control.

## 14.9 Applications: Process Control

Q: How does CONTROL THEORY apply to PROCESS CONTROL (chemical/industrial)?
A: (1) [Level, temperature, flow loops]: PI controllers with gain scheduling or MPC. (2) [Distillation columns, reactors]: MIMO MPC handling constraints. (3) [Long dead times]: Smith predictors, model predictive control. (4) [Batch processes]: iterative learning control. DCS (Distributed Control Systems) coordinate thousands of PIDs in a plant. Reliability and safety critical — conservative tuning, redundant sensors, extensive alarm systems. Field dominated by advanced MPC in recent decades.

## 14.10 Applications: Power Systems

Q: How is CONTROL THEORY used in POWER SYSTEMS?
A: (1) [Voltage regulation]: automatic voltage regulators (AVR) on generators. (2) [Frequency control]: load-frequency control maintaining 50/60 Hz despite load changes — droop control + secondary PI loop. (3) [Power system stabilizers]: damp electromechanical oscillations via supplementary control signals. (4) [FACTS devices]: power-electronic control of reactive power, voltage. (5) [Microgrids and renewables]: fast power electronic controllers for inverter-based generation. Modern grid is a HUGE dynamical control system.

## 14.11 Applications: Biomedical

Q: Give BIOMEDICAL applications of control theory.
A: (1) [Artificial pancreas]: closed-loop insulin delivery based on continuous glucose monitoring — MPC handles glucose constraints, meal disturbances. (2) [Cardiac pacemakers]: control of rate/rhythm. (3) [Drug dosing]: PK/PD models + control to optimize therapeutic levels (anesthesia, chemotherapy). (4) [Brain-machine interfaces]: decoding neural signals to control prosthetics. (5) [Ventilators]: pressure/volume control with lung dynamics. (6) [Deep brain stimulation]: closed-loop Parkinson's therapy. High-stakes applications requiring robust, safe control.

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
