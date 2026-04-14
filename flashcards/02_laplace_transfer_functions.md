+++
order = 2
subject = "Math"
tags = ["math", "control-theory", "laplace-transform", "transfer-function", "poles", "zeros", "impulse-response"]
+++

# Control Theory — Laplace Transforms and Transfer Functions

## 2.1 The Laplace Transform

C: The [Laplace transform] of a signal $f(t)$ for $t \geq 0$ is $F(s) = \mathcal{L}\{f(t)\} = \int_0^\infty f(t) e^{-st} \, dt$, where $s = \sigma + j\omega$ is a complex variable.

Q: Why is the LAPLACE TRANSFORM central to control theory?
A: Because it converts LINEAR ODEs with constant coefficients into ALGEBRAIC equations: $\dot{y} \to sY(s) - y(0)$ replaces differentiation with multiplication by $s$. Complex dynamics become rational functions — easier to analyze, factor, combine, and invert. Provides the FREQUENCY-DOMAIN perspective: substituting $s = j\omega$ gives the Fourier transform (steady-state sinusoidal response).

## 2.2 Key Laplace Transform Pairs

Q: State the LAPLACE TRANSFORM of COMMON signals.
A: (1) $\delta(t) \to 1$ (impulse). (2) $1 \text{ (step)} \to 1/s$. (3) $t \to 1/s^2$ (ramp). (4) $e^{-at} \to 1/(s + a)$ (exponential decay). (5) $\sin(\omega t) \to \omega/(s^2 + \omega^2)$. (6) $\cos(\omega t) \to s/(s^2 + \omega^2)$. (7) $e^{-at} \sin(\omega t) \to \omega/((s+a)^2 + \omega^2)$. Each standard signal has a simple rational Laplace representation — the bedrock of frequency-domain methods.

Q: Why does $\mathcal{L}\{e^{-at}\} = 1/(s+a)$?
A: $\int_0^\infty e^{-at} e^{-st} dt = \int_0^\infty e^{-(s+a)t} dt = \left[-\frac{1}{s+a} e^{-(s+a)t}\right]_0^\infty = \frac{1}{s+a}$ (valid for $\text{Re}(s) > -a$). The REGION OF CONVERGENCE: $\text{Re}(s) > -a$. For stable (decaying) signals: ROC includes the right half-plane. For unstable signals: LT may not exist or has restricted ROC. Control theory focuses on stable signals.

## 2.3 Properties of the Laplace Transform

Q: State the key PROPERTIES of the Laplace transform.
A: (1) [Linearity]: $\mathcal{L}\{af + bg\} = aF + bG$. (2) [Derivative]: $\mathcal{L}\{\dot{f}\} = sF(s) - f(0)$. (3) [Integral]: $\mathcal{L}\{\int_0^t f d\tau\} = F(s)/s$. (4) [Time shift]: $\mathcal{L}\{f(t - \tau) u(t - \tau)\} = e^{-s\tau} F(s)$. (5) [Frequency shift]: $\mathcal{L}\{e^{-at} f(t)\} = F(s + a)$. (6) [Convolution]: $\mathcal{L}\{f * g\} = F \cdot G$. Each property enables mechanical conversion of time-domain operations to algebraic ones.

## 2.4 The Transfer Function

C: The [transfer function] $G(s)$ of an LTI system is the ratio of output Laplace transform to input Laplace transform, ASSUMING ZERO INITIAL CONDITIONS: $G(s) = Y(s)/U(s)$.

Q: Why are TRANSFER FUNCTIONS the central object in classical control?
A: Because they COMPLETELY CHARACTERIZE LTI input-output behavior (with zero initial conditions). Series connection: $G_1 G_2$. Parallel: $G_1 + G_2$. Feedback: $G / (1 + GH)$. Enable GRAPHICAL analysis via block diagrams, computation of frequency response, stability analysis via poles. Every classical design method (root locus, Bode, Nyquist) is transfer-function-based.

## 2.5 From ODE to Transfer Function

Q: Convert the ODE $\ddot{y} + 3\dot{y} + 2y = u(t)$ with zero initial conditions to its transfer function.
A: Apply Laplace transform: $s^2 Y + 3sY + 2Y = U$, so $Y(s^2 + 3s + 2) = U$. Transfer function: $G(s) = Y(s)/U(s) = \frac{1}{s^2 + 3s + 2} = \frac{1}{(s+1)(s+2)}$. Factored: poles at $s = -1, -2$ (both negative real → stable, non-oscillatory decay). Each pole corresponds to an exponential mode in the time-domain response.

## 2.6 Poles and Zeros

C: [Poles] of $G(s) = N(s)/D(s)$ are the roots of $D(s) = 0$; [zeros] are the roots of $N(s) = 0$.

Q: What do POLES tell us about a system's behavior?
A: Poles are the EIGENVALUES of the system — each pole $s = p$ gives a natural mode $e^{pt}$ in the time-domain response. (1) [Real negative pole]: exponential decay. (2) [Real positive pole]: UNSTABLE exponential growth. (3) [Complex pair $\sigma \pm j\omega$, $\sigma < 0$]: damped oscillation. (4) [Pure imaginary $\pm j\omega$]: sustained oscillation (marginally stable). System STABLE iff ALL poles in open left half-plane ($\text{Re}(p) < 0$).

Q: What do ZEROS tell us about a system's response?
A: Zeros affect the SHAPE of the response (not its stability directly). (1) [RHP zero]: "non-minimum phase" — inverse response (going the wrong way before going the right way). (2) [Slow zeros near poles]: can cancel or reduce certain modes. Zeros at frequencies in the input signal completely BLOCK those frequencies (zero frequency response at zero location). Cannot "cancel" RHP poles with RHP zeros safely — leads to internal instability.

## 2.7 Impulse and Step Responses

Q: What is the [impulse response], and how does it relate to the transfer function?
A: The impulse response $g(t)$ is the output when input is a unit impulse $\delta(t)$. Since $\mathcal{L}\{\delta\} = 1$: $g(t) = \mathcal{L}^{-1}\{G(s)\}$. For any other input $u(t)$: $y(t) = g * u = \int_0^t g(\tau) u(t - \tau) d\tau$ (convolution). So $g(t)$ COMPLETELY characterizes the LTI system in the TIME DOMAIN — the time-domain dual of the transfer function.

Q: What is the [step response]?
A: The output when input is a unit step $u(t) = 1$ for $t \geq 0$. $Y_\text{step}(s) = G(s) / s$, and $y_\text{step}(t) = \mathcal{L}^{-1}\{G(s)/s\}$. Most common test signal in experimental identification and specifications. Step response encodes: rise time, overshoot, settling time, steady-state value — ALL the time-domain performance metrics. Integral of impulse response: $y_\text{step}(t) = \int_0^t g(\tau) d\tau$.

## 2.8 First-Order Systems

Q: Describe the RESPONSE of the first-order system $G(s) = K/(\tau s + 1)$.
A: DC gain $K = G(0)$. [Time constant $\tau$] = time to reach $63\%$ of final value. Impulse response: $(K/\tau) e^{-t/\tau}$. Step response: $K(1 - e^{-t/\tau})$ — exponential rise to $K$. Settling time $\approx 4\tau$ ($\pm 2\%$). No overshoot. Canonical first-order examples: RC circuit, single-tank level, thermal systems. SIMPLE design: adjust $K$ for steady-state, $\tau$ for speed.

## 2.9 Second-Order Systems

Q: Write the STANDARD FORM of a second-order transfer function.
A: $G(s) = \frac{\omega_n^2}{s^2 + 2\zeta \omega_n s + \omega_n^2}$. $\omega_n$ = [natural frequency] (undamped oscillation rate). $\zeta$ = [damping ratio] (0 to 1 = underdamped; 1 = critically damped; >1 = overdamped). Poles: $s = -\zeta \omega_n \pm \omega_n \sqrt{\zeta^2 - 1}$. Canonical model for: mass-spring-damper, RLC circuit, pendulum near equilibrium, cruise control, pitch control.

Q: What's the STEP RESPONSE of a second-order UNDERDAMPED system?
A: $y(t) = 1 - \frac{e^{-\zeta\omega_n t}}{\sqrt{1 - \zeta^2}} \sin(\omega_d t + \phi)$ where $\omega_d = \omega_n \sqrt{1 - \zeta^2}$ (damped frequency) and $\phi = \arctan(\sqrt{1-\zeta^2}/\zeta)$. Characteristics: (1) Overshoot: $\%OS = 100 e^{-\zeta\pi/\sqrt{1-\zeta^2}}$. (2) Peak time: $t_p = \pi/\omega_d$. (3) Settling time: $t_s \approx 4/(\zeta \omega_n)$ for $\pm 2\%$. Provides simple FORMULAS for design — pick $\zeta, \omega_n$ to meet specs.

## 2.10 Inverse Laplace Transform

Q: How do you compute the INVERSE LAPLACE TRANSFORM of a rational function?
A: [Partial fraction expansion]. Factor $G(s) = N(s)/D(s)$; express as sum of simple terms: $G(s) = \sum_i \frac{A_i}{s - p_i}$ (distinct poles) or $\sum_i \frac{A_{ij}}{(s - p_i)^{k_{ij}}}$ (repeated poles). Each term has a known inverse: $A/(s - p) \to A e^{pt}$. Sum gives $g(t)$. Complex conjugate pole pairs combine to give damped sinusoids. Standard method for computing time responses analytically.

## 2.11 Initial and Final Value Theorems

Q: State the [initial value theorem] and [final value theorem].
A: [IVT]: $f(0^+) = \lim_{s \to \infty} s F(s)$. Gives the initial value directly from the Laplace transform. [FVT]: $\lim_{t \to \infty} f(t) = \lim_{s \to 0} s F(s)$, VALID ONLY if $f(\infty)$ exists (stable pole-zero structure). Useful shortcut for steady-state analysis without inverting: e.g., steady-state error $e_\text{ss} = \lim_{s \to 0} s E(s)$. Misapplying to unstable systems gives nonsense — check stability first.

## 2.12 A Worked Transfer Function

P: Derive the transfer function of a MASS-SPRING-DAMPER: $m\ddot{x} + b\dot{x} + kx = F(t)$, where $x$ is displacement and $F$ is applied force.
S:
**IDENTIFY**: Second-order LTI. Input: $F$. Output: $x$. Zero initial conditions.

**PLAN**: Laplace transform the ODE; solve for $X(s)/F(s)$.

**EXECUTE**:
$m s^2 X(s) + b s X(s) + k X(s) = F(s)$
$X(s) (m s^2 + b s + k) = F(s)$
$G(s) = \frac{X(s)}{F(s)} = \frac{1}{ms^2 + bs + k} = \frac{1/m}{s^2 + (b/m)s + k/m}$

Normalized: $\omega_n = \sqrt{k/m}$, $\zeta = \frac{b}{2\sqrt{km}}$. So $G(s) = \frac{1/m}{s^2 + 2\zeta \omega_n s + \omega_n^2}$.

DC gain: $G(0) = 1/k$ (static compliance — displacement per unit force with zero damping).

Poles: $s = -\zeta\omega_n \pm \omega_n \sqrt{\zeta^2 - 1}$.

**EVALUATE**: Cases: (1) $b = 0$: $\zeta = 0$, poles pure imaginary $\pm j\omega_n$ — undamped oscillator. (2) $0 < \zeta < 1$: UNDERDAMPED, oscillates with decay. (3) $\zeta = 1$: CRITICAL damping, fastest non-oscillatory response. (4) $\zeta > 1$: OVERDAMPED, slow monotonic decay. Designing a suspension: pick $m$ (car), $k$ (spring stiffness — comfort/stiffness trade-off), $b$ (damper — usually target $\zeta \approx 0.7$ for balanced speed/smoothness). This transfer function is the prototype for ALL second-order systems — a single template models RLC circuits, cantilever beams, aircraft pitch dynamics.
