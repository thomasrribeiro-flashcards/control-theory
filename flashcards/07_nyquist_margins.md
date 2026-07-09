+++
order = 7
subject = "Mathematics"
tags = ["math", "control-theory", "nyquist", "stability", "encirclement", "phase-margin", "robustness"]
+++

# Control Theory — Nyquist Stability and Margins

## 7.1 The Nyquist Plot

C: The [Nyquist plot] is the parametric plot of $G(j\omega)$ in the COMPLEX PLANE as $\omega$ varies from $-\infty$ to $+\infty$ — a single closed curve encoding both magnitude and phase.

Q: Why is the NYQUIST plot useful alongside Bode?
A: Because it gives a GEOMETRIC, GLOBAL stability criterion — Bode is easier for design intuition but Nyquist handles UNSTABLE OPEN-LOOP systems, systems with RHP POLES, and complex encirclement questions where Bode is ambiguous. Particularly powerful for MIMO systems and ROBUSTNESS analysis. Different visualization of same frequency-response data.

## 7.2 The Nyquist Stability Criterion

Q: State the [Nyquist stability criterion].
A: Let $L(s) = G(s)C(s)$ be the loop transfer function, with $P$ RIGHT-HALF-PLANE open-loop poles. Plot $L(j\omega)$ for $\omega \in (-\infty, +\infty)$. Count $N$ = number of CLOCKWISE encirclements of the point $-1 + 0j$. Then: number of closed-loop RHP poles $= N + P$. System is STABLE iff $N + P = 0$. So: if open-loop has no RHP poles ($P = 0$), we need NO encirclements of $-1$; if $P = 1$, we need ONE COUNTERCLOCKWISE encirclement; etc.

Q: Why $-1$ as the critical point?
A: Because the closed-loop characteristic equation is $1 + L(s) = 0$, i.e., $L(s) = -1$. At any $s$ on the locus $L(s) = -1$: closed-loop has a pole there. For stability, no zeros of $1 + L$ can be in RHP. By the ARGUMENT PRINCIPLE: encirclements of $-1$ by $L(j\omega)$ count net RHP zeros of $1 + L$ (closed-loop poles) minus open-loop RHP poles $P$. Hence the criterion.

## 7.3 Encirclement Rules

Q: How do you COUNT ENCIRCLEMENTS of the $-1$ point?
A: Draw a RAY from $-1$ to infinity (any direction). Count CROSSINGS of this ray by the Nyquist curve: clockwise crossing = $+1$, counterclockwise = $-1$. Algebraic sum = net clockwise encirclements $N$. Equivalently: draw closed curve, count HOW MANY TIMES it winds around $-1$ — positive for clockwise. For simple plots: visual inspection suffices.

## 7.4 Nyquist Path

Q: What is the NYQUIST PATH in the $s$-plane?
A: A closed contour in the $s$-plane enclosing the entire RHP: the imaginary axis from $-j\infty$ to $+j\infty$, plus a semicircle of infinite radius in the right half-plane. If $L(s)$ has poles on the imaginary axis (e.g., integrators), DETOUR around them with small semicircles. The Nyquist PLOT is the IMAGE of this path under $L$. Encirclements of $-1$ by the image = argument principle applied to $1 + L(s)$.

## 7.5 Phase and Gain Margins Revisited

Q: How do GAIN and PHASE MARGINS appear on the NYQUIST PLOT?
A: [Phase margin]: angle from negative real axis to where $|L(j\omega)| = 1$ (intersection with unit circle). Measure counterclockwise from $-1$ direction. [Gain margin]: $1/|L(j\omega_{pc})|$ where $\omega_{pc}$ = phase crossover (Nyquist crosses negative real axis). Gain margin tells how much $L$ can be scaled up before touching $-1$. Visual, geometric interpretation matches Bode.

## 7.6 Relative Stability

Q: Why do STABILITY MARGINS matter beyond binary stable/unstable?
A: Because real systems have MODEL UNCERTAINTY, PARAMETER DRIFT, and UNMODELED DYNAMICS. A barely-stable system fails easily. Margins quantify ROBUSTNESS: (1) GM = how much gain can grow (or shrink) before instability. (2) PM = how much extra phase lag tolerated. Typical targets: $GM \geq 6$ dB, $PM \geq 45°$. LARGER margins → more robust but slower response. Trade-off.

## 7.7 Sensitivity Interpretation

Q: How do MARGINS relate to the SENSITIVITY PEAK?
A: Maximum of $|S(j\omega)| = |1/(1 + L(j\omega))|$ — call it $M_S$ — measures WORST-CASE amplification of disturbances. If $L(j\omega)$ comes close to $-1$ on Nyquist plot, $1 + L$ is small, $|S|$ is large. Guaranteed: $M_S \leq 1/(1 - 1/\text{GM})$ and $M_S \leq 1/(2 \sin(PM/2))$ approximately. Bounding $M_S$ is a MODERN robustness criterion — cleaner than GM/PM individually.

## 7.8 Non-Minimum-Phase Effects

Q: How does NON-MINIMUM PHASE limit performance?
A: RHP zeros contribute EXTRA phase lag at high frequencies. Can't be canceled (unstable cancellation). Limits achievable bandwidth: closed-loop bandwidth $\omega_B$ must be well below the RHP zero location (rule of thumb: $\omega_B < z_\text{RHP}/2$). TIME DELAYS $e^{-\tau s}$ add $-\omega\tau$ phase lag — analogous effect, limits bandwidth to $\sim 1/\tau$. Fundamental limitations: can't beat physics with cleverer design.

## 7.9 Nyquist for Unstable Open-Loop

Q: How does NYQUIST analysis handle UNSTABLE open-loop plants?
A: Count RHP poles $P$ of $L(s)$. For closed-loop stability: need $N = -P$ (i.e., $P$ COUNTERCLOCKWISE encirclements of $-1$). Example: inverted pendulum has 1 RHP pole ($P = 1$); Nyquist of $L = G \cdot C$ must encircle $-1$ ONCE counterclockwise for stability. Feedback STABILIZES the unstable plant — cannot be determined from Bode alone (which assumes open-loop stability).

## 7.10 Circle Criterion and Absolute Stability

Q: What is the [circle criterion] for nonlinear feedback systems?
A: Generalizes Nyquist to systems with a NONLINEAR element whose characteristic lies in a SECTOR $[k_1, k_2]$ (slope bounded). System is stable if Nyquist plot of $G(j\omega)$ avoids a certain disk determined by the sector bounds. Trades conservatism (it is only a sufficient condition) for the ability to handle nonlinearities. Useful for: saturation nonlinearities, dead zones, hysteresis (with care). Complement to LYAPUNOV methods.

## 7.11 Sketching Nyquist Plots

Q: When sketching a NYQUIST PLOT, at which KEY points do you evaluate $L(j\omega)$ first, and how do you connect them?
A: Compute $L(j\omega)$ at $\omega = 0^+$, $\omega = \infty$, and the phase crossover(s); plot these and connect smoothly using Bode data (monotonic magnitude).

Q: After plotting $L(j\omega)$ for $\omega > 0$, how do you COMPLETE the Nyquist plot and read off stability?
A: Reflect to the CONJUGATE for $\omega < 0$, close with appropriate infinite arcs (integrator poles need semicircles in the complex plane), then count encirclements of $-1$. Sanity-check: Bode plot and Nyquist plot should agree on crossings and encirclements.

## 7.12 A Worked Nyquist Example

P: Apply the Nyquist criterion to $L(s) = \frac{K}{s(s+1)}$. Find range of $K$ for stability.
S:
**IDENTIFY**: Open-loop has pole at origin (integrator — detour in Nyquist path) and at $s = -1$. No RHP poles, so $P = 0$. Need $N = 0$ for stability.

**PLAN**: Compute $L(j\omega)$, sketch plot, count encirclements as function of $K$.

**EXECUTE**:
$L(j\omega) = \frac{K}{j\omega(j\omega + 1)} = \frac{K}{-\omega^2 + j\omega} = \frac{K(-\omega^2 - j\omega)}{\omega^4 + \omega^2} = \frac{-K}{\omega^2 + 1} - j \frac{K}{\omega(\omega^2 + 1)}$.

Behavior:
- $\omega \to 0^+$: real part $\to -K$, imaginary part $\to -\infty$. Curve approaches vertical line $\text{Re} = -K$ from below.
- $\omega \to \infty$: $L \to 0$ with angle $-180°$ (due to 2 poles). Approaches origin from negative real direction.
- At $\omega = 1$: $L(j) = K/(j(1+j)) = K/(-1 + j) = K(-1 - j)/2$. So $L(j) = -K/2 - jK/2$.

Nyquist contour: from origin along negative real side up to $\omega = 0^+$, loops around origin via the small semicircle detour (large magnitude), comes back and traces the lower half-plane. For $\omega < 0$, mirror: upper half-plane.

Critical point: $\angle L(j\omega) = -90° - \arctan(\omega)$ lies strictly between $-90°$ and $-180°$ for all finite $\omega > 0$, reaching $-180°$ only as $\omega \to \infty$ where $|L| \to 0$. So the curve never touches the negative real axis at any finite distance, and the infinite closure arc from the origin detour sweeps through the RIGHT half-plane. Hence no encirclement of $-1$: $N = 0$ for EVERY $K > 0$.

Phase margin: gain crossover where $|L(j\omega_{gc})| = 1$; $PM = 90° - \arctan(\omega_{gc}) > 0$ always — it shrinks as $K$ grows but never reaches zero. So STABLE for ALL $K > 0$ for this specific system.

**EVALUATE**: For $L(s) = K/[s(s+1)]$: CLOSED-LOOP STABLE for all $K > 0$ — confirmed by noting the closed-loop char. poly $s^2 + s + K$ has roots with NEGATIVE REAL PART for all $K > 0$ (by Routh: all coefficients positive, 2nd-order → stable). Nyquist analysis agrees: no encirclements of $-1$ for any $K > 0$. Simple plants often stable for large parameter ranges — richer systems (like the 3rd-order example in Chapter 4) have finite stability ranges.
