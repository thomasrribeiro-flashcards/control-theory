+++
order = 5
subject = "Math"
tags = ["math", "control-theory", "root-locus", "closed-loop-poles", "compensator-design"]
+++

# Control Theory — Root Locus

## 5.1 What a Root Locus Is

C: The [root locus] is the plot in the complex $s$-plane of the CLOSED-LOOP POLES as a parameter (usually the gain $K$) varies from 0 to $\infty$.

Q: Why is the ROOT LOCUS useful?
A: Because it shows GRAPHICALLY how closed-loop behavior CHANGES with design parameter. At a glance: does the system stay stable? For what $K$ range? What are transient characteristics (damping, speed)? Where to add compensator poles/zeros to shape the locus? Invaluable design tool: visual, intuitive, works for high-order systems. Developed by Evans (1948) — still the backbone of classical design.

## 5.2 Root Locus Equation

Q: Derive the ROOT LOCUS equation.
A: Closed-loop char. equation: $1 + K G(s) = 0 \Rightarrow G(s) = -1/K$. In polar: $|G(s)| = 1/K$ AND $\angle G(s) = \pm(2k+1) \cdot 180°$ for $k = 0, 1, 2, \ldots$. The [angle condition] $\angle G(s) = 180°$ (mod $360°$) identifies points on the locus. The [magnitude condition] gives the $K$ value at each point. Plot is traced out by finding all $s$ satisfying the angle condition.

## 5.3 Basic Rules

Q: State the FIRST FEW root locus rules.
A: (1) Locus has $n$ BRANCHES where $n$ = number of poles of $G$. (2) Branches START at OPEN-LOOP POLES (when $K = 0$). (3) Branches END at OPEN-LOOP ZEROS or at infinity (when $K = \infty$). If $G$ has $m$ zeros, $n - m$ branches go to infinity. (4) Locus is SYMMETRIC about the real axis (complex poles come in conjugate pairs). These rules shape the skeleton of any root locus.

## 5.4 Real Axis Segments

Q: When is a point on the REAL AXIS on the root locus?
A: When the TOTAL NUMBER OF REAL POLES AND ZEROS TO ITS RIGHT is ODD. Easy graphical check: scan real axis, count poles (×) and zeros (○) to the right of each segment. Segments with odd count are on locus; even count segments are off. Quick way to identify real-axis branches before computing asymptotes or intersections.

## 5.5 Asymptotes

Q: How many ASYMPTOTES does a root locus have, and at what ANGLES?
A: If $G$ has $n$ poles and $m$ zeros ($n > m$): there are $n - m$ asymptotes going to infinity. Angles: $\theta_k = \frac{(2k + 1) \cdot 180°}{n - m}$ for $k = 0, 1, \ldots, n - m - 1$. Examples: $n - m = 1$: 1 asymptote at $180°$. $n - m = 2$: 2 asymptotes at $\pm 90°$ (vertical). $n - m = 3$: 3 asymptotes at $\pm 60°, 180°$. Tells you where branches go for large $K$.

Q: Where do asymptotes ORIGINATE (centroid)?
A: All asymptotes emanate from a single point on the real axis: $\sigma_c = \frac{\sum p_i - \sum z_i}{n - m}$ — the CENTROID, computed as (sum of open-loop poles minus sum of open-loop zeros) divided by (pole excess). Combined with the asymptote angles, this fully locates far-field branch behavior. Essential for sketching.

## 5.6 Breakaway and Break-In Points

Q: What are [breakaway] and [break-in] points on the real axis?
A: Points where TWO OR MORE BRANCHES meet (from opposite sides) then leave the real axis (breakaway — moving toward complex plane) or return to real axis (break-in — coming from complex plane). Location: solve $dK/ds = 0$ or equivalently $G(s) \cdot G'(s) = 0$ for real $s$. At a breakaway, the multiple roots give BORDERLINE OSCILLATORY behavior — often the transition from overdamped to underdamped.

## 5.7 Imaginary-Axis Crossings

Q: How do you find where the root locus CROSSES the imaginary axis?
A: These are points of MARGINAL STABILITY. Use ROUTH-HURWITZ: build Routh table for $1 + KG = 0$ polynomial; find $K$ where a first-column entry goes to zero; auxiliary polynomial's roots give the imaginary-axis frequencies. Equivalently: substitute $s = j\omega$ into characteristic equation, equate real and imaginary parts to zero, solve for $K$ and $\omega$. Critical design info: this $K$ is the stability threshold.

## 5.8 Angle of Departure / Arrival

Q: Compute ANGLE OF DEPARTURE from a complex pole.
A: $\theta_\text{dep} = 180° - \sum \theta_p + \sum \theta_z$, where $\theta_p$ = angles from OTHER poles to the complex pole of interest, $\theta_z$ = angles from zeros. Similarly, angle of arrival at a complex zero: $\theta_\text{arr} = 180° + \sum \theta_p - \sum \theta_z$. Tells you the INITIAL DIRECTION each complex branch moves as $K$ grows from zero. Necessary for accurate sketching near complex poles/zeros.

## 5.9 Design via Root Locus

Q: How do you DESIGN a compensator using root locus?
A: Add POLES and ZEROS of the compensator $C(s)$ to SHAPE the overall $L(s) = C(s) G(s)$ locus so that desired closed-loop poles lie on it for some $K$. Typical additions: (1) LEAD compensator ($s + z$ near origin, $s + p$ further left, $z < p$): pulls locus LEFT, improves stability and speed. (2) LAG compensator ($s + z$, $s + p$, $z > p$): REDUCES steady-state error at low $K$. (3) PID = combination. Manipulate pole-zero placement to achieve desired branches.

## 5.10 Sketching the Locus

Q: You're given an open-loop $L(s)$ and asked for the root locus. What's your first move?
A: Plot poles (×) and zeros (○) on the $s$-plane, then mark real-axis segments (odd-count rule). Asymptotes and breakaway come next.

Q: OUTLINE the steps to sketch a root locus.
A: (1) Plot open-loop poles (×) and zeros (○). (2) Identify real-axis segments (odd count rule). (3) Compute and draw asymptotes (number, angle, centroid). (4) Find breakaway/break-in points. (5) Compute angles of departure/arrival at complex poles/zeros. (6) Find imaginary-axis crossings (Routh or $s = j\omega$). (7) Sketch branches from poles to zeros, respecting rules and symmetry. Practice builds intuition rapidly.

## 5.11 Gain from the Locus

Q: Given a DESIRED pole location on the root locus, find the GAIN $K$.
A: Use the MAGNITUDE CONDITION: $K = 1/|G(s_d)|$ where $s_d$ is the desired pole. Geometrically: $K = $ (product of distances from $s_d$ to open-loop poles) / (product of distances to zeros). Just measure lengths on the plot — directly gives the gain to use. Combined with desired pole specifications ($\zeta, \omega_n$): picks specific $K$ for your design.

## 5.12 A Worked Root Locus

P: Sketch the root locus for $L(s) = K \cdot \frac{1}{s(s + 2)(s + 5)}$.
S:
**IDENTIFY**: 3 poles at $0, -2, -5$; no zeros. $n = 3, m = 0, n - m = 3$.

**PLAN**: Apply the sketching rules systematically.

**EXECUTE**:
Poles: $s = 0, -2, -5$. No zeros.

Real-axis segments: To the right of $s = 0$: 0 p/z → NOT on locus. Between $-2$ and $0$: 1 pole to right → ON LOCUS. Between $-5$ and $-2$: 2 poles to right → NOT. Left of $-5$: 3 poles to right → ON LOCUS.

Asymptotes: 3 asymptotes at $\theta = 60°, 180°, 300°$. Centroid: $\sigma_c = (0 - 2 - 5)/3 = -7/3 \approx -2.33$. The 3 asymptotes meet at $(-2.33, 0)$, shooting out at $\pm 60°$ and $180°$.

Breakaway on real axis: between $0$ and $-2$. Solve $\frac{d}{ds}\left[\frac{-1}{s(s+2)(s+5)}\right] = 0$: leads to $3s^2 + 14s + 10 = 0$, giving $s \approx -0.88$ or $-3.79$. Only $s = -0.88$ is on the locus (the other is between $-5$ and $-2$, not on locus). Breakaway at $-0.88$.

Imaginary-axis crossings: char. poly $s^3 + 7s^2 + 10s + K = 0$. From Chapter 4 analysis: $K = 70$, $\omega = \sqrt{10} \approx 3.16$.

**EVALUATE**: Root locus structure: two branches start at $s = 0, -2$, move toward each other along the real axis, meet at breakaway $s = -0.88$ (forming double pole at $K = 8.2$), split into complex-conjugate pair going vertically; third branch starts at $s = -5$, moves LEFT along real axis to $-\infty$. The complex pair CROSSES the imaginary axis at $K = 70$, $\omega = 3.16$ — stability boundary. Designer reads: for any $K < 70$, system stable; dominant closed-loop behavior changes from overdamped (small $K$) to underdamped (past breakaway) to unstable (past imaginary crossing). Enables choosing $K$ for desired damping — say $K$ giving $\zeta = 0.5$ can be found geometrically on the locus.
