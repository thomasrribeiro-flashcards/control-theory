+++
order = 1
subject = "Math"
tags = ["math", "control-theory", "feedback", "block-diagrams", "open-loop", "closed-loop"]
+++

# Control Theory — Feedback and the Control Paradigm

## 1.1 What Control Theory Studies

Q: What is the CENTRAL PROBLEM of control theory?
A: Given a PLANT (a dynamical system to be controlled), design a CONTROLLER that makes the plant behave in a desired way — tracking a reference, rejecting disturbances, remaining stable. Applications: robotics (trajectory tracking), aerospace (flight stabilization, autopilot), chemical process control, power grids, biomedical devices, economics. Controls THE DYNAMICS rather than predicting them — action-oriented, applied dynamical systems.

Q: Why is CONTROL THEORY its own field, distinct from pure dynamical systems?
A: Because the key question isn't "how does the system evolve?" but "how do we DESIGN INPUTS to make it evolve as we want?" Requires: (1) MODELING the plant mathematically. (2) SPECIFYING performance (stability, accuracy, speed). (3) SYNTHESIZING controllers meeting specs. (4) ANALYZING robustness to modeling errors and disturbances. Engineering focus drives a rich mathematical toolkit not found in pure analysis.

## 1.2 Open-Loop vs Closed-Loop Control

C: [Open-loop] control: controller computes input based on the REFERENCE signal alone — no feedback from the actual output. [Closed-loop] (feedback) control: controller uses the MEASURED OUTPUT to adjust the input, continuously correcting.

Q: Why does CLOSED-LOOP control usually outperform open-loop?
A: Because open-loop is VULNERABLE to (1) MODEL INACCURACIES (plant dynamics differ from assumed), (2) DISTURBANCES (unexpected inputs affecting output), (3) PARAMETER DRIFT (component aging, environment changes). Closed-loop continuously CORRECTS — error between desired and actual output drives the input. Examples: thermostats (closed), microwave timer (open). Key insight: feedback dramatically reduces sensitivity to uncertainty.

## 1.3 The Block Diagram

C: A [block diagram] represents control systems as interconnected blocks (transfer functions) with signals flowing between them — a graphical notation for ODE interconnections.

Q: Describe the CANONICAL feedback LOOP in block-diagram form.
A: Reference $r(t)$ enters a SUMMING JUNCTION where the measured output $y(t)$ is subtracted, producing error $e = r - y$. Error drives the CONTROLLER $C$, whose output is the input $u$ to the PLANT $G$. Plant output is $y$. Closed-loop transfer function from $r$ to $y$: $T = \frac{GC}{1 + GC}$ (assuming unity feedback). From disturbance to output: $S = \frac{1}{1 + GC}$ — the [sensitivity function].

## 1.4 Feedback's Beneficial Effects

Q: State the main BENEFITS of feedback.
A: (1) [Reduces sensitivity] to plant parameter variations. (2) [Rejects disturbances] automatically. (3) [Linearizes nonlinearities] around operating points. (4) [Modifies dynamics] — can stabilize unstable plants, speed up slow ones, add damping. (5) [Enables tracking] of reference signals. Feedback TRANSFORMS imperfect components into reliable systems — the engineering miracle that makes precision machines possible from imperfect parts.

Q: What is the COST or DOWNSIDE of feedback?
A: (1) [Can INDUCE instability]: high-gain feedback can make a stable plant unstable if phase margin is inadequate. (2) [Adds complexity]: more components (sensors, controllers), more tuning. (3) [Introduces noise] from sensors into the control signal. (4) [Delay concerns]: lags in measurement/actuation can trigger oscillations. Feedback is powerful but MUST BE DESIGNED CAREFULLY. Naive high gain is rarely the answer.

## 1.5 Closed-Loop Transfer Function

Q: Derive the CLOSED-LOOP TRANSFER FUNCTION for a unity-feedback loop with forward path $L(s) = G(s) C(s)$.
A: At the summing junction: $E(s) = R(s) - Y(s)$. Forward path: $Y(s) = L(s) E(s)$. Substitute: $Y(s) = L(s)(R(s) - Y(s)) \Rightarrow Y(s)(1 + L(s)) = L(s) R(s)$. Hence $T(s) = \frac{Y(s)}{R(s)} = \frac{L(s)}{1 + L(s)}$. Denominator $1 + L(s) = 0$ is the CHARACTERISTIC EQUATION — its roots are the closed-loop poles, determining stability.

## 1.6 Sensitivity and Complementary Sensitivity

C: The [sensitivity function] is $S(s) = \frac{1}{1 + L(s)}$ (disturbance → output). The [complementary sensitivity] is $T(s) = \frac{L(s)}{1 + L(s)}$ (reference or noise → output).

Q: State the FUNDAMENTAL TRADE-OFF $S + T = 1$.
A: $S(j\omega) + T(j\omega) = 1$ at every frequency. So making $T$ small (noise rejection) forces $S$ large (poor disturbance rejection), and vice versa. Classic design TRADE-OFF between NOISE REJECTION (high-frequency, favored by small $T$) and DISTURBANCE REJECTION (low-frequency, favored by small $S$). Controller shapes $S$ and $T$ with frequency-dependent gain — small $S$ at low frequencies (good tracking), small $T$ at high (noise rejection).

## 1.7 Performance Specifications

Q: What are the STANDARD specifications for a control system's step response?
A: (1) [Rise time $t_r$]: time to go from 10% to 90% of final value. (2) [Settling time $t_s$]: time to reach and stay within a $\pm 2\%$ (or $\pm 5\%$) band. (3) [Overshoot %]: peak vs final value excess. (4) [Steady-state error $e_\text{ss}$]: long-term tracking error. (5) [Damping ratio $\zeta$]: oscillation strength. These spec the "speed," "accuracy," and "smoothness" of response — quantified design targets.

## 1.8 Disturbances and Noise

Q: Distinguish DISTURBANCES from SENSOR NOISE in control systems.
A: [Disturbances] act on the PLANT — unmeasured inputs that perturb the state (wind gusts on aircraft, load changes on motors). Typically LOW-FREQUENCY. [Sensor noise] corrupts the MEASUREMENT of the output (electrical noise, quantization). Typically HIGH-FREQUENCY. Different effect: disturbances $\to$ $S \cdot d$ at output; noise $\to$ $T \cdot n$ fed back as spurious error. Good design: small $S$ at low frequencies (reject disturbances), small $T$ at high frequencies (attenuate noise).

## 1.9 Linear Time-Invariant Systems

C: A system is [linear] if it satisfies superposition ($T(ax_1 + bx_2) = aT(x_1) + bT(x_2)$); [time-invariant (LTI)] if shifting input in time shifts output by same amount.

Q: Why do we focus on LTI systems despite most real systems being nonlinear?
A: Because (1) LINEAR systems are TRACTABLE: superposition, frequency response, transfer functions apply. (2) Most nonlinear systems are APPROXIMATELY LINEAR near an operating point (linearization). (3) LTI theory gives FREQUENCY-DOMAIN tools (Laplace, Bode, Nyquist) with clear design insights. (4) Many practical controllers achieve LARGE PERFORMANCE with linear design + simple nonlinear extensions (saturation, deadzones, gain scheduling). Fundamental starting point.

## 1.10 A Brief History of Control

Q: Sketch the HISTORY of control theory.
A: (1) [1788]: Watt's centrifugal governor — first mechanical feedback, steam engine speed regulation. (2) [1920s-30s]: Nyquist, Bode at Bell Labs — frequency-domain analysis. (3) [1950s-60s]: State-space (Kalman, Bellman), optimal control (LQR, dynamic programming). (4) [1960s-70s]: Kalman filter, space-age applications (Apollo). (5) [1980s]: Robust control (H-infinity). (6) [Modern]: Nonlinear, adaptive, learning-based control. Each era driven by application demands (aerospace, industry, robotics).

## 1.11 Classical vs Modern Control

Q: Distinguish CLASSICAL and MODERN control frameworks.
A: [Classical] (frequency domain): transfer functions, Bode, Nyquist, root locus, PID. Single-input single-output (SISO), hand-tuning, intuitive. [Modern] (state-space): multi-input multi-output (MIMO), matrix-based, computer-aided design, optimal control. Classical still dominates simple applications; modern required for aerospace, complex robotics. Most engineers use BOTH — classical for design intuition, modern for rigor/MIMO.

## 1.12 A Worked Feedback Analysis

P: For a plant $G(s) = \frac{1}{s + 1}$ with proportional controller $C(s) = K$ (constant gain), derive the closed-loop transfer function and analyze the effect of $K$.
S:
**IDENTIFY**: First-order plant with proportional control. Closed-loop is the simplest feedback analysis.

**PLAN**: Compute $T(s)$; analyze pole location, steady-state gain, speed of response.

**EXECUTE**:
Forward path: $L(s) = K \cdot \frac{1}{s+1} = \frac{K}{s+1}$.

Closed-loop: $T(s) = \frac{L}{1 + L} = \frac{K/(s+1)}{1 + K/(s+1)} = \frac{K}{s + 1 + K}$.

Pole: $s = -(1 + K)$. For $K > -1$: stable (pole in left half-plane). For $K = 0$: $T(s) = 0$ — no control. For $K > 0$: pole moves FURTHER LEFT as $K$ grows — FASTER response. Steady-state gain: $T(0) = K/(1 + K) \to 1$ as $K \to \infty$ — better reference tracking.

Open-loop steady-state gain was $G(0) = 1$. With $K = 10$: $T(0) = 10/11 \approx 0.91$, slightly less than 1 (offset). Steady-state error: $1 - T(0) = 1/(1 + K) \to 0$ as $K \to \infty$. Speed: time constant $\tau = 1/(1 + K) \to 0$ as $K \to \infty$ — instantaneous response in the limit.

**EVALUATE**: Proportional feedback with a first-order plant (1) ALWAYS stable for $K > -1$, (2) speeds up response as $K$ grows, (3) REDUCES BUT DOESN'T ELIMINATE steady-state error. Key limitations: (a) error $1/(1+K)$ never reaches zero — need INTEGRAL action (PI controller). (b) High-gain idealization ignores noise amplification, actuator saturation, high-frequency plant dynamics. Realistic design balances gain against these constraints. This simple example captures the central trade-offs that motivate richer controllers.
