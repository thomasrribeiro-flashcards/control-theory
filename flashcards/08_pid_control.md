+++
order = 8
subject = "Math"
tags = ["math", "control-theory", "pid", "tuning", "ziegler-nichols", "integral-windup", "anti-windup"]
+++

# Control Theory — PID Control and Tuning

## 8.1 The PID Controller

Q: Write the [PID control law] in time-domain.
A: $u(t) = K_p e(t) + K_i \int_0^t e(\tau) d\tau + K_d \dot{e}(t)$, where $e(t) = r(t) - y(t)$ is the tracking error. Three terms: PROPORTIONAL ($K_p$), INTEGRAL ($K_i$), DERIVATIVE ($K_d$). In Laplace: $C(s) = K_p + K_i/s + K_d s$. Alternative form with time constants: $C(s) = K_p(1 + 1/(T_i s) + T_d s)$ where $T_i = K_p/K_i$ (integral time), $T_d = K_d/K_p$ (derivative time).

Q: Why is PID the MOST WIDELY USED control scheme in industry?
A: Because (1) three interpretable parameters — easy to tune intuitively. (2) Works well for most well-behaved plants (smooth, stable, SISO). (3) Small computational burden — easily implemented in microcontrollers. (4) Well-understood theory. (5) Decades of tuning rules (Ziegler-Nichols, etc.) available. ~90% of industrial controllers are PID or PI variants. Sometimes called "the aspirin of control" — works for many problems without deep analysis.

## 8.2 Effect of Each Term

Q: Describe the INDIVIDUAL effects of P, I, D in a PID controller.
A: (1) [Proportional]: faster response, reduces (but doesn't eliminate) steady-state error, large $K_p$ can destabilize. (2) [Integral]: eliminates steady-state error (adds integrator, raises system type), but slows response and can cause oscillations/overshoot. (3) [Derivative]: predicts future error from trend, ADDS DAMPING, improves stability, but amplifies high-frequency noise. Each term has distinct role; tuning balances them.

Q: Why does INTEGRAL action eliminate STEADY-STATE ERROR to step inputs?
A: Because at steady state, the output is constant, so $\dot{y} = 0$. If ERROR were constant and nonzero, the integral term would grow WITHOUT BOUND, driving the controller output to infinity — impossible for a bounded system. Hence steady-state error must be zero. Formally: integrator in the controller adds a POLE AT ORIGIN to $L(s)$, raising system type from 0 to 1 → zero steady-state error to step.

## 8.3 PI, PD, PID Variants

Q: When is PI sufficient, and when do you need PID?
A: [PI] suffices when the plant has ENOUGH inherent damping (slow or overdamped dynamics — thermal systems, level control, flow control). Derivative doesn't help much and adds noise sensitivity. [PID] is useful when the plant is UNDERDAMPED or requires FAST response without overshoot (motor position control, motion control, aircraft attitude). [PD] (no integral) used when plant already has an integrator or steady-state error isn't critical (e.g., position control with natural gravity offset).

## 8.4 Integral Windup

Q: What is [integral windup], and why is it problematic?
A: When the controller output saturates (hits actuator limits) and the plant can't follow the reference, the error remains large for an extended time, causing the INTEGRAL TERM to accumulate a huge value. When the reference finally changes or the plant catches up, the wound-up integral takes a long time to unwind — causing MASSIVE OVERSHOOT. Classic failure mode in real systems with saturating actuators.

Q: How do you implement [ANTI-WINDUP]?
A: Several techniques: (1) [BACK-CALCULATION]: feed the difference between saturated and unsaturated controller output back to reduce the integral state. (2) [CONDITIONAL INTEGRATION]: freeze the integral when the output is saturated. (3) [CLAMPING]: limit the integral term directly to a fixed range. (4) [INCREMENTAL / VELOCITY FORMS]: compute $\Delta u$ per step, bypassing windup by not storing integrated error. Anti-windup is MANDATORY for production-quality PID implementations.

## 8.5 Derivative Filtering

Q: Why must the DERIVATIVE term be FILTERED in practice?
A: Because pure differentiation amplifies HIGH-FREQUENCY NOISE: $\mathcal{L}\{dn/dt\} = j\omega N(j\omega)$ — gain grows with frequency. Sensor noise at 1 kHz can produce massive control spikes. Standard fix: replace $K_d s$ with $K_d s / (\tau_f s + 1)$ — a "dirty derivative" that behaves like $d/dt$ at low frequencies but rolls off at high. Typical $\tau_f = T_d / 10$. Without filtering: unusable in real applications.

## 8.6 Ziegler-Nichols Tuning

Q: Describe the ZIEGLER-NICHOLS CLOSED-LOOP TUNING method.
A: (1) Set $K_i = K_d = 0$. Start with $K_p$ small and INCREASE slowly until the closed-loop system oscillates with constant amplitude at GAIN $K_u$ (ULTIMATE GAIN) and period $T_u$ (ultimate period). (2) Then:
- P: $K_p = 0.5 K_u$.
- PI: $K_p = 0.45 K_u, T_i = T_u / 1.2$.
- PID: $K_p = 0.6 K_u, T_i = T_u / 2, T_d = T_u / 8$.
Empirical rules from 1940s, still widely used. Give reasonable STARTING POINTS; typically require fine-tuning. Gives aggressive response — 25% overshoot by design.

Q: Describe ZIEGLER-NICHOLS OPEN-LOOP (reaction curve) tuning.
A: Apply step input to open loop. Measure reaction curve: extract DEAD TIME $L$ (delay before response starts) and TIME CONSTANT $T$ (characteristic time of response) from tangent at inflection point. Then:
- P: $K_p = T / L$.
- PI: $K_p = 0.9 T / L, T_i = 3.3 L$.
- PID: $K_p = 1.2 T / L, T_i = 2 L, T_d = 0.5 L$.
For systems where closed-loop oscillation test is unsafe. Simple, quick, but limited to approximately first-order-plus-delay plants.

## 8.7 Modern Tuning Approaches

Q: What are MODERN alternatives to Ziegler-Nichols?
A: (1) [IMC (Internal Model Control)]: model-based tuning — choose closed-loop time constant directly, compute PID from plant model. (2) [AMIGO rules] (Åström-Hägglund): less aggressive than ZN, better robustness. (3) [LOOP-SHAPING] via Bode: design PID for desired bandwidth, phase margin. (4) [COMPUTER OPTIMIZATION]: numerically optimize for a cost function (rise time, overshoot, robustness). Modern tools (MATLAB PID Tuner, Python python-control) automate tuning.

## 8.8 Cascade Control

Q: What is [cascade control], and when is it useful?
A: TWO PID LOOPS nested: an INNER loop (fast) regulates a secondary variable; an OUTER loop (slow) regulates the primary variable by setting the inner loop's reference. Example: temperature control of reactor — outer loop sets temperature setpoint, inner loop controls heating power using actual temperature. ADVANTAGES: better disturbance rejection (inner loop rejects disturbances affecting intermediate variable), more flexible tuning (separate tuning per loop). Common in industry.

## 8.9 Feedforward and Setpoint Weighting

Q: What is [feedforward control]?
A: Direct MODEL-BASED compensation for MEASURABLE DISTURBANCES or reference changes, added to the feedback signal. Example: known process disturbance $d$ measured → feedforward controller $-G_d/G_p$ cancels its effect before feedback has to react. Much faster response than pure feedback for predictable inputs. Requires ACCURATE MODEL; combined with feedback for handling residual errors.

Q: What is [setpoint weighting] in PID?
A: Different weights on the reference for the P and D terms: $u = K_p(b r - y) + K_i \int e + K_d(c \dot{r} - \dot{y})$. Typical: $b < 1$, $c = 0$. Reduces "kick" from step-changes in reference (derivative kick) and smooths the response. ERROR feedback remains exact for integral (ensures zero steady-state error). Common refinement in industrial PID.

## 8.10 Limitations of PID

Q: What are the LIMITATIONS of PID control?
A: (1) ONLY for SISO systems (one input, one output). Cross-coupling in MIMO systems requires multivariable methods. (2) Struggles with LONG DEAD TIMES — Smith predictor is a better approach. (3) Can't handle severely non-minimum-phase or unstable plants optimally. (4) No explicit handling of constraints beyond anti-windup. (5) Tuning-intensive for complex dynamics. For simple loops PID is unbeatable; for complex systems, state-space methods (MPC, LQR) win.

## 8.11 Anti-Aliasing and Digital Implementation

Q: How is PID implemented DIGITALLY?
A: Sample at period $T_s$; approximate continuous operations:
- Integral: $I_k = I_{k-1} + K_i \cdot T_s \cdot e_k$ (rectangular) or trapezoidal ($T_s (e_{k-1} + e_k)/2$).
- Derivative: $D_k = K_d (e_k - e_{k-1})/T_s$ or filtered version to reduce noise.
- Total: $u_k = K_p e_k + I_k + D_k$.
Sampling period: $T_s \leq T_{10}/10$ where $T_{10}$ is the 10% rise time of the process. Too large $T_s$ → sluggishness and instability.

## 8.12 A Worked PID Design

P: Design a PI controller for the plant $G(s) = \frac{1}{(s+1)(s+5)}$ using the Ziegler-Nichols closed-loop method.
S:
**IDENTIFY**: Second-order plant. Need to find ultimate gain and period experimentally (or analytically for this simple case).

**PLAN**: Find $K_u, T_u$; apply Z-N formulas for PI.

**EXECUTE**:
With $C(s) = K$ (pure proportional): $L(s) = K/((s+1)(s+5))$. Char. poly: $s^2 + 6s + (5 + K) = 0$. For oscillation: need $s = j\omega$, so $-\omega^2 + 6j\omega + 5 + K = 0$. Real: $5 + K - \omega^2 = 0$; imaginary: $6\omega = 0 \Rightarrow \omega = 0$. But $\omega = 0$ gives $K = -5$ — not oscillatory. Wait: this 2nd-order plant is ALWAYS stable for $K > -5$, so NEVER oscillates sustainedly with pure P. No ultimate gain!

Need 3rd or higher order for ZN to apply. Revised plant: $G(s) = \frac{1}{(s+1)(s+2)(s+5)}$ (same as our root locus example).

From chapter 4/5: $K_u = 70$, $\omega_u = \sqrt{10} \approx 3.162$ rad/s, so $T_u = 2\pi/\omega_u \approx 1.987$ s.

ZN PI: $K_p = 0.45 K_u = 31.5$, $T_i = T_u / 1.2 \approx 1.656$ s.

So $K_i = K_p / T_i \approx 19.0$.

Controller: $C(s) = 31.5 + 19.0/s = (31.5 s + 19.0)/s$.

**EVALUATE**: ZN PI gives aggressive response (~25% overshoot by design). Integrates tell us: loop now has INTEGRATOR — zero steady-state error to step. The value $K_p = 31.5$ is close to $K_u/2 = 35$ — reduced from ultimate gain for stability margin. Fine-tuning: simulate step response, check overshoot/settling; if too aggressive, reduce $K_p$ (or increase $T_i$). Start with ZN, iterate — standard engineering workflow. For more sophisticated design, use loop-shaping or pole placement.
