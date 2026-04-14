+++
order = 3
subject = "Math"
tags = ["math", "control-theory", "time-response", "transient", "steady-state", "damping", "specifications"]
+++

# Control Theory — Time-Domain Response

## 3.1 Test Signals

Q: What are the STANDARD TEST SIGNALS in control?
A: (1) [Impulse] $\delta(t)$: models sudden shocks, probes system's natural modes. (2) [Step] $u(t)$: models sudden set-point changes; common specification signal. (3) [Ramp] $t \cdot u(t)$: models constant-velocity inputs, tests tracking of linearly-increasing references. (4) [Sinusoid] $\sin(\omega t)$: probes frequency response. Standard battery of inputs lets engineers compare systems and specify performance requirements unambiguously.

## 3.2 Transient vs Steady-State Response

C: The [transient response] is the portion of the output that decays to zero as $t \to \infty$ (from initial conditions and switching). The [steady-state response] is what remains as $t \to \infty$ — long-term behavior determined by input.

Q: How do poles determine the TRANSIENT response?
A: Each pole $p_i$ contributes a term $c_i e^{p_i t}$ to the response. Modes with $\text{Re}(p_i) \ll 0$ decay QUICKLY; modes near the imaginary axis decay SLOWLY. Modes with $\text{Re}(p_i) = 0$ persist indefinitely. For fast transient: push all poles far into the left half-plane. Dominant pole (one closest to imaginary axis) often dictates the overall transient character.

## 3.3 First-Order Response

Q: Write the STEP RESPONSE of a first-order system $G(s) = K/(\tau s + 1)$.
A: $Y_\text{step}(s) = \frac{K}{s(\tau s + 1)} = \frac{K}{s} - \frac{K\tau}{\tau s + 1}$. Inverse: $y(t) = K(1 - e^{-t/\tau})$. Exponential rise to $K$. $\tau$ = [time constant]: at $t = \tau$, $y = 63\%$ of final; at $t = 4\tau$, $y \approx 98\%$ — the commonly-used SETTLING TIME. Simple and clean — pole at $-1/\tau$ completely characterizes the response.

## 3.4 Second-Order Response

Q: List the CHARACTERISTIC TIMES for an underdamped second-order step response.
A: (1) [Rise time] $t_r \approx \frac{1 - 0.4167\zeta + 2.917\zeta^2}{\omega_n}$ (10%-90%). (2) [Peak time] $t_p = \frac{\pi}{\omega_d}$ where $\omega_d = \omega_n\sqrt{1 - \zeta^2}$. (3) [Percent overshoot] $\%OS = 100 e^{-\zeta\pi/\sqrt{1-\zeta^2}}$. (4) [Settling time] $t_s \approx \frac{4}{\zeta \omega_n}$ ($\pm 2\%$), $\approx 3/(\zeta \omega_n)$ ($\pm 5\%$). Quick design: pick $\zeta$ from %OS (e.g., $\zeta = 0.707$ for ~5% overshoot), then $\omega_n$ for speed.

Q: Why does $\zeta = 0.707$ often represent a GOOD design choice?
A: Because it offers a good TRADE-OFF: (1) Reasonable overshoot ($\approx 4\%$ — not excessive). (2) Fast rise time (faster than critically damped). (3) Good gain margin and phase margin in frequency response. (4) Minimal Integral Time Absolute Error (ITAE) — a common performance index. Butterworth-style filter prototype. For aggressive designs: $\zeta \approx 0.5$; for smooth: $\zeta \approx 1$; $\zeta \approx 0.7$ is the engineer's default.

## 3.5 Overshoot

Q: Why does an UNDERDAMPED system OVERSHOOT?
A: Because insufficient damping cannot absorb all the kinetic energy before the system crosses the setpoint. The mass (or equivalent) OVERSHOOTS, decelerates, oscillates. Overshoot depends ONLY on damping ratio $\zeta$ (not $\omega_n$): $\%OS = 100 e^{-\pi\zeta/\sqrt{1-\zeta^2}}$. Monotonically decreasing in $\zeta$: $\zeta = 0.1 \to 73\%$, $\zeta = 0.5 \to 16\%$, $\zeta = 0.7 \to 4.6\%$, $\zeta = 1 \to 0$.

## 3.6 Dominant Poles Approximation

Q: What is the [dominant poles approximation]?
A: When a system has poles FAR APART in real part, the SLOWEST (closest to imaginary axis) dominate the response. Higher-order systems can be approximated by keeping only the 1-2 dominant poles. Rule of thumb: "5× RULE" — poles at least 5× further left contribute negligibly. Simplifies analysis: a 4th-order system with dominant complex pair becomes effectively 2nd-order, enabling standard $\zeta, \omega_n$ design.

## 3.7 Effect of Zeros

Q: How do ZEROS affect the time response?
A: Zeros MODIFY TRANSIENT character without changing pole-determined decay rates. A zero between pole and origin: INCREASES OVERSHOOT and speed. A zero NEAR a pole: approximately cancels that pole's mode. RHP zero (non-minimum phase): causes INITIAL INVERSE RESPONSE — output goes the "wrong way" before the right way. Limits achievable bandwidth; important in aerospace (aircraft pitch response to elevator), chemical processes, power electronics.

## 3.8 Steady-State Error and System Type

C: [System type] is the number of pure integrators ($1/s$ factors) in the open-loop transfer function $L(s)$. Type 0, 1, 2, ...

Q: How does SYSTEM TYPE determine steady-state error?
A: For a step input: type-0 has finite steady-state error; type-1+ has zero. For ramp: type-0 infinite error; type-1 finite; type-2+ zero. For parabolic: type-0, 1 infinite; type-2 finite; type-3+ zero. Pattern: ADDING INTEGRATORS to open-loop ELIMINATES steady-state error to increasingly "faster" reference signals. Why PI and PID controllers (integral action) achieve zero steady-state step error: they raise system type by 1.

## 3.9 Error Constants

Q: Define the STATIC ERROR CONSTANTS.
A: (1) [Position constant] $K_p = \lim_{s \to 0} G(s)$: steady-state error to unit step is $1/(1 + K_p)$. (2) [Velocity constant] $K_v = \lim_{s \to 0} s G(s)$: error to unit ramp is $1/K_v$. (3) [Acceleration constant] $K_a = \lim_{s \to 0} s^2 G(s)$: error to parabolic is $1/K_a$. Higher type: infinite constants, zero error to corresponding input. Elegant way to characterize tracking ability without simulation.

## 3.10 Transient Specifications in Pole Location

Q: In the $s$-PLANE, where should POLES be placed for desired transient behavior?
A: (1) [Real part]: $\sigma = -\zeta \omega_n$ — set for decay rate. Settling time $t_s \approx 4/\sigma$. (2) [Imaginary part]: $\omega_d = \omega_n \sqrt{1 - \zeta^2}$ — oscillation frequency. (3) [Angle from negative real axis]: $\beta = \arccos(\zeta)$ — damping-ratio cone. Desired region: left of $\text{Re}(s) = -\sigma_\text{min}$ (fast enough), inside cone $|\text{Im}(s)/\text{Re}(s)| \leq \tan(\beta_\text{max})$ (damped enough). Translates specs to pole-placement regions.

## 3.11 Steady-State Output

Q: For LTI systems with sinusoidal input $u(t) = A \sin(\omega t)$, the steady-state output is?
A: $y_\text{ss}(t) = A |G(j\omega)| \sin(\omega t + \angle G(j\omega))$. That is: same frequency, MAGNITUDE scaled by $|G(j\omega)|$, PHASE shifted by $\angle G(j\omega)$. This is the FREQUENCY RESPONSE — evaluating transfer function along imaginary axis gives the gain and phase for each frequency. Foundation of Bode/Nyquist methods (chapters 6-7).

## 3.12 A Worked Time Response

P: For $G(s) = \frac{25}{s^2 + 4s + 25}$, identify $\omega_n, \zeta$, and estimate rise time, overshoot, settling time for a unit step input.
S:
**IDENTIFY**: Standard second-order form.

**PLAN**: Match to $\omega_n^2/(s^2 + 2\zeta\omega_n s + \omega_n^2)$; compute specs from formulas.

**EXECUTE**:
Compare: $\omega_n^2 = 25 \Rightarrow \omega_n = 5$ rad/s. $2\zeta\omega_n = 4 \Rightarrow \zeta = 0.4$.

Since $\zeta < 1$: UNDERDAMPED. $\omega_d = \omega_n\sqrt{1 - \zeta^2} = 5\sqrt{0.84} \approx 4.58$ rad/s.

Specs:
- Rise time: $t_r \approx 1.8/\omega_n = 0.36$ s (approximation). Or from formula $(1 - 0.4167\zeta + 2.917\zeta^2)/\omega_n \approx (1 - 0.167 + 0.467)/5 \approx 0.26$ s.
- Peak time: $t_p = \pi/\omega_d \approx 0.686$ s.
- % overshoot: $100 e^{-0.4\pi/\sqrt{0.84}} \approx 100 e^{-1.37} \approx 25.4\%$.
- Settling time ($\pm 2\%$): $4/(\zeta\omega_n) = 4/2 = 2$ s.

Poles: $s = -2 \pm 4.58 j$.

**EVALUATE**: The system responds in about 2 seconds with $\sim 25\%$ overshoot — moderately aggressive, would probably improve by increasing $\zeta$ (more damping). To reduce overshoot to 5%: need $\zeta \approx 0.7$, so target $b = 2\zeta\omega_n = 7$ instead of 4. Doubling $\omega_n$ halves all time-based specs. This analytical sensitivity to $\omega_n, \zeta$ makes 2nd-order design CLEAN: specs translate directly to coefficient values, which translate to physical parameters (mass, damping, spring constant).
