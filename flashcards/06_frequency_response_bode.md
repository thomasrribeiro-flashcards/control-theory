+++
order = 6
subject = "Math"
tags = ["math", "control-theory", "frequency-response", "bode-plot", "magnitude", "phase", "decibels"]
+++

# Control Theory — Frequency Response and Bode Plots

## 6.1 Frequency Response

Q: What is the [frequency response] of an LTI system?
A: The steady-state output to a sinusoidal input $A \sin(\omega t)$ is $A |G(j\omega)| \sin(\omega t + \angle G(j\omega))$ — same frequency, amplitude scaled, phase shifted. The complex function $G(j\omega)$ — evaluating the transfer function along the imaginary axis — ENCAPSULATES the frequency response. $|G(j\omega)|$: MAGNITUDE (gain); $\angle G(j\omega)$: PHASE. Tests at various $\omega$ characterize the system fully in the frequency domain.

Q: Why is FREQUENCY RESPONSE useful for analysis AND DESIGN?
A: Because (1) MEASURABLE experimentally with sine-wave generators. (2) Reveals BANDWIDTH, RESONANCE, ROLLOFF. (3) Directly shows FILTERING behavior. (4) Enables stability analysis via NYQUIST, margins, Bode plots. (5) Superposition for complex signals: decompose into sines (Fourier), apply gain/phase, reassemble. Frequency-domain design is intuitive and widely used in classical control.

## 6.2 Bode Plots

C: A [Bode plot] is a pair of LOG-LOG (or semi-log) plots: (1) magnitude in DECIBELS ($20 \log_{10} |G(j\omega)|$) vs. $\omega$ (log scale), and (2) phase (degrees) vs. $\omega$ (log scale).

Q: Why use DECIBELS and LOG FREQUENCY?
A: (1) Dec convert MULTIPLICATIVE gains to ADDITIVE: $|G_1 G_2|$ in dB = $|G_1|$ dB + $|G_2|$ dB. Series/parallel combinations become sums. (2) Log frequency spans orders of magnitude (mHz to MHz) in one plot. (3) Polynomial transfer functions produce STRAIGHT-LINE ASYMPTOTIC Bode plots — trivial to sketch by hand. (4) Matches human perception of sound/light logarithmic scales. Standard in all engineering disciplines.

## 6.3 Bode Plot of First-Order System

Q: Sketch the Bode plot of $G(s) = \frac{1}{\tau s + 1}$.
A: $G(j\omega) = 1/(j\omega\tau + 1)$. Magnitude: $|G| = 1/\sqrt{\omega^2 \tau^2 + 1}$. Phase: $\angle G = -\arctan(\omega\tau)$.
Asymptotic Bode:
- Magnitude: 0 dB for $\omega \ll 1/\tau$; $-20$ dB/dec for $\omega \gg 1/\tau$. Corner frequency: $\omega_c = 1/\tau$.
- Phase: $0°$ at low $\omega$; $-90°$ at high $\omega$; $-45°$ at corner.
Simple single pole: LOW-PASS filter. The CORNER (or BREAK) frequency $1/\tau$ separates passband from attenuation region. Characteristic "knee" shape.

## 6.4 Bode Plot of Second-Order System

Q: Describe the Bode plot of $G(s) = \frac{\omega_n^2}{s^2 + 2\zeta\omega_n s + \omega_n^2}$.
A: Asymptotic magnitude: 0 dB for $\omega \ll \omega_n$; $-40$ dB/dec for $\omega \gg \omega_n$. Corner at $\omega_n$. For SMALL $\zeta$: a RESONANT PEAK of height $1/(2\zeta)$ near $\omega_n$ (exact peak frequency: $\omega_r = \omega_n\sqrt{1 - 2\zeta^2}$). Phase: $0°$ low, $-180°$ high, $-90°$ at $\omega_n$, steep transition for small $\zeta$. Resonance disappears for $\zeta > 1/\sqrt{2}$. CRITICAL case for oscillatory systems.

## 6.5 Constructing Bode Plots from Factors

Q: How do you CONSTRUCT a Bode plot for a complicated transfer function?
A: FACTOR $G(s)$ into simple terms: gain $K$, integrators $1/s^k$, differentiators $s^k$, first-order $1/(\tau s + 1)$, second-order $\omega_n^2 / (s^2 + 2\zeta\omega_n s + \omega_n^2)$. Each factor's Bode plot is a known simple shape. SUM them (in dB for magnitude, degrees for phase). Straight-line approximations give a QUICK SKETCH; refine at corners for accuracy. Works for any rational $G(s)$ once factored.

## 6.6 Integrators and Differentiators

Q: What are the BODE PLOTS of pure integrators $1/s$ and differentiators $s$?
A: Integrator $1/s$: $|1/(j\omega)| = 1/\omega$. In dB: $-20 \log \omega$ — a straight line of slope $-20$ dB/decade, crossing 0 dB at $\omega = 1$. Phase: $-90°$ at all frequencies. Differentiator $s$: slope $+20$ dB/decade, phase $+90°$. Each extra pole (or zero) at origin adds $-20$ (or $+20$) dB/decade and shifts phase by $-90°$ (or $+90°$). Fundamental building blocks.

## 6.7 Bandwidth

C: The [bandwidth] $\omega_B$ of a system is the frequency at which $|G(j\omega)| = |G(0)| / \sqrt{2}$ ($-3$ dB).

Q: Why is BANDWIDTH a KEY performance metric?
A: Because it measures the system's SPEED OF RESPONSE in frequency terms. Larger bandwidth → can track higher-frequency inputs → faster time response. For 2nd-order: $\omega_B \approx \omega_n \sqrt{1 - 2\zeta^2 + \sqrt{4\zeta^4 - 4\zeta^2 + 2}}$, usually $\sim \omega_n$. Rule of thumb: bandwidth $\times$ rise time $\approx 0.35$ (for first-order) or $0.5$ (for second-order with moderate damping). Bandwidth relates directly to sensor/actuator choice.

## 6.8 Gain and Phase Margins

C: [Gain margin (GM)]: how much the gain can be increased before instability. [Phase margin (PM)]: additional phase lag at gain-crossover that would cause instability.

Q: Compute GAIN and PHASE MARGINS from Bode plots.
A: (1) GAIN CROSSOVER frequency $\omega_{gc}$: where $|L(j\omega)| = 1$ (0 dB). (2) PHASE CROSSOVER frequency $\omega_{pc}$: where $\angle L(j\omega) = -180°$. Then:
- $PM = 180° + \angle L(j\omega_{gc})$ (should be positive for stability).
- $GM = -20 \log_{10} |L(j\omega_{pc})|$ (in dB; positive for stability).
Typical design targets: $PM \geq 45°$ (often $60°$), $GM \geq 6$ dB (often $> 10$ dB). Provides ROBUSTNESS to uncertainties.

## 6.9 Design via Bode Shaping

Q: How do you DESIGN compensators using Bode plots ([LOOP SHAPING])?
A: Specify desired LOOP TRANSFER FUNCTION $L(j\omega)$ characteristics: (1) HIGH GAIN at low frequencies (good tracking, disturbance rejection). (2) LOW GAIN at high frequencies (noise rejection). (3) Adequate PM at gain crossover (stability margin). (4) Appropriate CROSSOVER frequency (bandwidth). Add compensator $C(s)$ to reshape $L = CG$ — LEAD for phase boost, LAG for low-frequency gain, PID for both. Design is iterative: sketch, adjust, verify.

## 6.10 Minimum-Phase vs Non-Minimum-Phase Systems

Q: What's a [minimum-phase] system?
A: A stable system with all ZEROS in the LEFT HALF-PLANE. For minimum-phase systems: magnitude plot UNIQUELY determines phase plot (via Bode's gain-phase relationship). Non-minimum-phase systems (RHP zeros or time delays): EXTRA PHASE LAG that limits achievable bandwidth. Challenging to control: RHP zero causes INITIAL INVERSE RESPONSE. Aircraft pitch (elevator → pitch rate), some chemical processes, power converters — all non-minimum-phase.

## 6.11 Time Delay and Phase

Q: How does a PURE TIME DELAY $e^{-\tau s}$ affect the Bode plot?
A: Magnitude: $|e^{-j\omega\tau}| = 1$ (unchanged, 0 dB). Phase: $\angle e^{-j\omega\tau} = -\omega\tau$ (LINEAR phase lag, growing with frequency). Time delays ADD phase lag WITHOUT attenuation — strongly degrade phase margin at higher frequencies. Common in: networked control (communication delays), thermal systems (heat diffusion), biological feedback loops. Limits bandwidth: cannot push crossover beyond $\sim 1/\tau$ without destabilizing.

## 6.12 A Worked Bode Plot

P: Sketch the Bode magnitude plot for $G(s) = \frac{100}{s(s + 10)(s + 100)}$.
S:
**IDENTIFY**: Three factors. Break frequencies at 0 (integrator), 10, 100. Constant: 100.

**PLAN**: Convert to standard form, identify breaks, sum asymptotic contributions.

**EXECUTE**:
Rewrite: $G(s) = \frac{100}{s \cdot 10(1 + s/10) \cdot 100(1 + s/100)} = \frac{0.1}{s(1 + s/10)(1 + s/100)}$.

DC gain scale: $|G| \to 0.1/\omega$ as $\omega \to 0$ (integrator dominates). In dB: $20 \log(0.1) - 20 \log \omega = -20 - 20 \log \omega$.

At $\omega = 0.1$: magnitude $= 20 \log(0.1/0.1) = 0$ dB. At $\omega = 1$: $-20$ dB. So the $-20$ dB/decade integrator line crosses 0 dB at $\omega = 0.1$.

At $\omega = 10$: first break — add $-20$ dB/dec. Now slope is $-40$ dB/dec.
Magnitude at $\omega = 10$ (still integrator regime): $-20 - 20 \log 10 = -40$ dB. From here, slope $-40$ dB/dec.

At $\omega = 100$: second break — add $-20$ dB/dec. Slope becomes $-60$ dB/dec.
Magnitude at $\omega = 100$: $-40 - 40 \log(100/10) = -40 - 40 = -80$ dB. From there, $-60$ dB/dec.

**EVALUATE**: Asymptotic magnitude plot: slope $-20$ dB/dec up to $\omega = 10$, then $-40$ dB/dec to $\omega = 100$, then $-60$ dB/dec beyond. Crosses 0 dB at $\omega = 0.1$. Bandwidth (approximate, where $|G| = -3$ dB) is where the line reaches 0 dB — here at $\omega_{gc}$ found where integrator line hits 0, so $\omega_{gc} \approx 0.1$ rad/s. Very low bandwidth due to the low DC gain. Compensation could ADD GAIN (shift whole plot up) to increase bandwidth, while adding a LEAD term to preserve phase margin at the new crossover. This is the essence of Bode-based design.
