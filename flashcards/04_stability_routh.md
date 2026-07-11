+++
order = 4
subject = "mathematics"
tags = ["math", "control-theory", "stability", "routh-hurwitz", "characteristic-equation", "poles"]
+++

# Control Theory — Stability and the Routh-Hurwitz Criterion

## 4.1 BIBO Stability

Q: Define BIBO (bounded-input bounded-output) STABILITY for an LTI system.
A: Every bounded input produces a bounded output.

Q: State the BIBO STABILITY condition for transfer functions.
A: An LTI system with transfer function $G(s)$ is BIBO stable iff ALL POLES of $G$ have NEGATIVE REAL PART (lie in the OPEN LEFT HALF-PLANE). Equivalent forms: (1) the impulse response is absolutely integrable: $\int_0^\infty |g(t)| dt < \infty$. (2) All natural modes $e^{p_i t}$ decay to zero. A pole on the imaginary axis ($\text{Re}(p) = 0$) gives MARGINAL stability — bounded but non-decaying response (single poles) or unbounded (repeated poles).

Q: Why is stability the FIRST requirement of any control design?
A: Because UNSTABLE systems blow up — any disturbance, noise, or small error grows unbounded, making the system useless or dangerous. All subsequent design goals (tracking, disturbance rejection, robustness) PRESUPPOSE stability. Stability is often the HARDEST constraint: adding more gain for better tracking can destabilize; careful design balances stability against performance. "Stability first, then performance."

## 4.2 The Characteristic Equation

C: The [characteristic equation] of a closed-loop system is $1 + L(s) = 0$ where $L(s)$ is the loop transfer function. Equivalently: roots of the denominator of $T(s) = L/(1+L)$.

Q: What's the CONNECTION between characteristic equation and stability?
A: The CLOSED-LOOP POLES are the ROOTS of the characteristic equation. System is stable iff ALL roots have negative real part. Direct root-finding is tedious for high orders; indirect methods (Routh-Hurwitz, root locus, Nyquist) enable stability analysis WITHOUT solving the polynomial. Each method provides different insights (Routh: counting unstable poles; root locus: parameter dependence; Nyquist: frequency-domain).

## 4.3 Routh-Hurwitz Criterion

Q: What does the [Routh-Hurwitz criterion] determine?
A: Whether a polynomial $P(s) = a_n s^n + a_{n-1} s^{n-1} + \cdots + a_0$ has all roots in the OPEN LEFT HALF-PLANE (i.e., whether the corresponding system is stable), WITHOUT actually finding the roots. Constructs the ROUTH TABLE from polynomial coefficients and checks signs. If all entries in the first column are POSITIVE: stable. Sign changes: number of RHP poles = number of sign changes.

## 4.4 Building the Routh Table

Q: Construct the [Routh table] for $P(s) = a_n s^n + a_{n-1} s^{n-1} + \cdots + a_0$.
A: First two rows:
$s^n$: $a_n, a_{n-2}, a_{n-4}, \ldots$
$s^{n-1}$: $a_{n-1}, a_{n-3}, a_{n-5}, \ldots$
Subsequent rows computed as $2 \times 2$ determinants divided by the leading element of the row above:
$s^{n-2}$: $b_1 = -\frac{1}{a_{n-1}} \det\begin{pmatrix} a_n & a_{n-2} \\ a_{n-1} & a_{n-3} \end{pmatrix} = \frac{a_{n-1} a_{n-2} - a_n a_{n-3}}{a_{n-1}}$, and so on.
Continue until only one non-zero entry in a row. $n+1$ rows total.

## 4.5 Necessary Condition

Q: State the NECESSARY CONDITION for stability from polynomial coefficients.
A: All coefficients of $P(s)$ must be PRESENT (nonzero) and POSITIVE (assuming leading coefficient positive). If any coefficient is zero or negative: system is UNSTABLE. Easy first check before Routh table. Not SUFFICIENT: a polynomial with all-positive coefficients can still have complex RHP roots (e.g., $s^3 + s^2 + s + 12$ has positive coefficients but unstable roots). Still, quick rejection of obviously-unstable systems.

## 4.6 Interpretation of the Routh Table

Q: How do you READ the Routh table for stability?
A: Examine the FIRST COLUMN. If all entries are STRICTLY POSITIVE: system is stable (all roots in open LHP). If there are SIGN CHANGES: number of sign changes = number of unstable (RHP) roots. If any entry is zero: special case requiring modifications (epsilon method or auxiliary polynomial). Zero rows: indicate pairs of purely imaginary roots (marginal stability) — can extract them via auxiliary polynomial.

## 4.7 Special Cases

Q: What's the [epsilon trick] for Routh tables with zero in the first column?
A: Replace zero with small $\epsilon > 0$; complete the table; examine signs as $\epsilon \to 0^+$ and $\epsilon \to 0^-$. If sign changes depend on $\epsilon$'s sign: imaginary axis poles or degenerate cases. Alternative: substitute $s \to 1/s$ and reconstruct — gives same stability info without zeros in first column. These tricks handle edge cases where plain Routh table fails.

Q: What does a COMPLETELY ZERO ROW in the Routh table indicate?
A: Roots SYMMETRIC about the origin — typically pairs of pure imaginary roots $\pm j\omega$ or opposite-sign real pairs $\pm \alpha$. Recover these by forming the AUXILIARY POLYNOMIAL from the previous (non-zero) row's entries; its roots are the symmetric pairs. Differentiate auxiliary polynomial; use coefficients in place of the zero row to continue. Zero rows → system is MARGINALLY STABLE at best.

## 4.8 Stability Margins via Routh

Q: How does Routh-Hurwitz determine the GAIN MARGIN (range of stable $K$ in $1 + KG(s) = 0$)?
A: Write characteristic polynomial with $K$ as a parameter: $P(s, K) = 0$. Build Routh table with $K$-dependent entries. Determine range of $K$ making ALL first-column entries positive. Boundaries: $K$ values where first-column entries become zero → marginal stability (imaginary-axis poles). Gives a PRECISE stability range analytically — useful for design. Example: for which $K$ is $s^3 + 2s^2 + s + K$ stable? Routh gives $0 < K < 2$.

## 4.9 Root Properties

Q: What does the number of SIGN CHANGES in the Routh table's first column equal?
A: The exact number of roots of the characteristic polynomial with POSITIVE REAL PART (RHP roots). Stability: zero sign changes. If table gives 2 sign changes: 2 unstable poles, system unstable. This quantitative info is useful for diagnosis and verification beyond binary stable/unstable answer. Combined with ROOT LOCUS: predict where unstable poles are as design parameter varies.

## 4.10 Examples of Stability Analysis

Q: Analyze stability of $P(s) = s^4 + 2s^3 + 3s^2 + 4s + 5$ via Routh-Hurwitz.
A: All coefficients positive (necessary condition). Build table:
$s^4$: 1, 3, 5
$s^3$: 2, 4, 0
$s^2$: $(2 \cdot 3 - 1 \cdot 4)/2 = 1$, $(2 \cdot 5 - 1 \cdot 0)/2 = 5$
$s^1$: $(1 \cdot 4 - 2 \cdot 5)/1 = -6$
$s^0$: $(-6 \cdot 5 - 1 \cdot 0)/(-6) = 5$
First column: $1, 2, 1, -6, 5$ — TWO sign changes ($1 \to -6$ and $-6 \to 5$). So 2 UNSTABLE roots. System unstable.

## 4.11 Lyapunov Stability (Preview)

Q: What's the LYAPUNOV approach to stability (for linear systems)?
A: For $\dot{\mathbf{x}} = A\mathbf{x}$: system is STABLE iff there exist positive definite matrices $P, Q$ satisfying $A^T P + PA = -Q$ (LYAPUNOV EQUATION). Intuition: $V(x) = x^T P x$ is a positive-definite "energy," and $\dot{V} = -x^T Q x < 0$ shows energy dissipates. Equivalent to Routh-Hurwitz for LTI systems but GENERALIZES to nonlinear systems — foundation of modern robust/nonlinear control (chapter 13).

## 4.12 A Worked Stability Range

P: Find the range of $K$ for which the unity-feedback system with forward path $L(s) = \frac{K}{s(s+2)(s+5)}$ is stable.
S:
**IDENTIFY**: Characteristic equation $1 + L(s) = 0$; expand; apply Routh-Hurwitz with $K$-dependent entry.

**PLAN**: Compute characteristic polynomial; build Routh table; determine signs as function of $K$.

**EXECUTE**:
Char. poly: $s(s+2)(s+5) + K = s^3 + 7s^2 + 10s + K = 0$.

Routh table:
$s^3$: 1, 10
$s^2$: 7, $K$
$s^1$: $(7 \cdot 10 - 1 \cdot K)/7 = (70 - K)/7$
$s^0$: $K$

First-column entries: $1, 7, (70 - K)/7, K$.

All positive iff $(70 - K)/7 > 0$ AND $K > 0$, i.e., $0 < K < 70$.

At $K = 70$: $(70 - K)/7 = 0$ (marginal stability — imaginary-axis poles). Find them: auxiliary polynomial $7s^2 + K = 0 \Rightarrow s^2 = -K/7 = -10 \Rightarrow s = \pm j\sqrt{10} \approx \pm 3.16j$. These are the frequencies of sustained oscillation at stability boundary.

**EVALUATE**: Stable for $0 < K < 70$. At $K = 70$: system is on the verge, oscillating at $\omega \approx 3.16$ rad/s. For $K > 70$: two RHP poles — system unstable. Designer can choose $K < 70$ for stability; smaller $K$ → more stability margin, but poorer tracking. Typical design: $K \approx 20$–$30$ for balance of stability margin and performance. The same system analyzed via ROOT LOCUS (chapter 5) shows the graphical equivalent: branches cross into the RHP at $K = 70$.
