# Team Presentation Walkthrough & Demo Notes
## MFAD Mini-Project: "Interpolation, Extrapolation & Climate Change"
**Format:** Live Jupyter notebook demonstration  
**Target Duration:** 5 to 7 minutes total (~1 minute per topic)  
**Style:** Internal team prep notes — conversational, direct, mathematically rigorous, and fully transparent about model limitations.

---

### Team Roles & Topic Ownership
- **Member 1:** Data loading & baseline definition, matrix representation & degree selection rationale, RREF algorithm, rank & consistency check, interpolation analysis
- **Member 2:** Classical Gram-Schmidt orthogonalization ($QR$ factorization), solving via back-substitution ($R\hat{x} = Q^T b$), normal equations derivation, polynomial degree comparison (Degrees 1 to 6)
- **Member 3:** Column space & null space geometry, orthogonal projection matrix, extrapolation trajectory (2025–2050) & baseline offset analysis (1951–1980 vs. pre-industrial)
- **Member 4:** Covariance eigendecomposition & scale-dependence nuance, correlation analysis, matrix diagonalization, visualization suite, project conclusion

---

### Minute-by-Minute Demo Flow

#### Minute 1: Introduction & Data Exploration (Presenter: Member 1)
- **What to show on screen:**
  - Section 1 title and Section 2 raw scatter plot (`NASA GISS Global Temperature Anomalies (1980–2023)`).
- **Talking points:**
  - "Hello Professor, our mini-project applies fundamental linear algebra to model historical climate change using NASA GISS global surface temperature anomaly data from 1980 through 2023."
  - "We have 44 discrete annual observations showing a visible upward trajectory of around +0.9°C over four decades."
  - "Important baseline definition: NASA GISS computes anomalies relative to the **1951–1980 base period**. This is distinct from pre-industrial levels, an essential distinction we address during extrapolation."
  - "Instead of treating curve fitting as an arbitrary optimization or using black-box libraries, we derive polynomial fitting from first principles using vector spaces, projection, and eigendecomposition."
- **Handoff:** Move directly into the matrix setup.

---

#### Minute 2: Matrix Representation, Degree Rationale & RREF (Presenter: Member 1)
- **What to show on screen:**
  - Section 3 Vandermonde matrix formulation, degree rationale, Section 4 RREF printed matrices, and Section 5 rank output.
- **Talking points:**
  - "To model temperature $T(t) = a_0 + a_1 t + a_2 t^2 + a_3 t^3$, we build an overdetermined linear system $Ax = b$ with a $44 \times 4$ Vandermonde matrix."
  - "Why degree 3? Degree 1 assumes a constant rate of warming, ignoring acceleration. Degree 2 captures acceleration, but assumes constant acceleration. Degree 3 introduces an inflection point, allowing the warming rate to change while requiring only 4 parameters for 44 points."
  - "Crucial step: We normalized calendar years to $[-1, 1]$. If we used raw years like 2023, powers like $2023^3 \approx 8 \times 10^9$ cause matrix condition numbers over $10^{15}$, destroying numerical stability. Normalization keeps condition numbers low."
  - "We implemented our own Gaussian elimination with partial pivoting. RREF of $A$ shows 4 pivots, so $\text{rank}(A) = 4$ (full column rank)."
  - "However, RREF of the augmented matrix $[A \mid b]$ has rank 5, with row 5 showing $[0 \; 0 \; 0 \; 0 \mid 1]$ — the contradiction $0 = 1$."
  - "By the Rouché–Capelli theorem, because $\text{rank}(A) < \text{rank}([A \mid b])$, the system is strictly inconsistent. No single cubic curve passes through 44 noisy points. This mathematically proves why we must find a least squares solution."
- **Handoff:** "Now Member 3 and Member 2 will explain subspace geometry and our two methods for solving least squares."

---

#### Minute 3: Column Space, Null Space & Gram-Schmidt Solver (Presenters: Member 3 + Member 2)
- **What to show on screen:**
  - Section 6 column/null space printouts, followed by Section 7 Gram-Schmidt code, verification `np.allclose(Q.T @ Q, np.eye(4))`, and the back-substitution solve output.
- **Talking points (Member 3):**
  - "Geometrically, the column space $\text{Col}(A)$ is a 4-dimensional subspace inside $\mathbb{R}^{44}$."
  - "By Rank-Nullity, $\text{nullity}(A) = 4 - 4 = 0$. Because the null space contains only the zero vector, any least squares solution is guaranteed to be unique."
  - "The observation vector $b$ lives in $\mathbb{R}^{44}$ but sticks outside $\text{Col}(A)$ due to climate noise."
- **Talking points (Member 2):**
  - "We factored $A = QR$ using Classical Gram-Schmidt (CGS). While CGS can experience loss of orthogonality on poorly conditioned matrices, here our normalized matrix is well-conditioned, and $Q^T Q = I_4$ holds to machine precision."
  - "Crucially, we actually use $QR$ to solve the system: substituting $A = QR$ into normal equations gives $R\hat{x} = Q^T b$."
  - "Because $R$ is upper triangular, we solve it directly by back-substitution without ever inverting $A^T A$, which avoids squaring the matrix condition number. The coefficients match our normal equations exactly."
- **Handoff:** "Next, Member 3 shows the projection geometry, and Member 2 compares polynomial degrees."

---

#### Minute 4: Projection, Least Squares & Degree Comparison (Presenters: Member 3 + Member 2)
- **What to show on screen:**
  - Section 8 projection vector $\hat{p}$, residual orthogonality check $A^T (b - \hat{p}) \approx 0$, Section 9 coefficients, and the **Degree 1 to 6 Comparison Table**.
- **Talking points (Member 3):**
  - "To find the closest approximation to $b$ inside $\text{Col}(A)$, we compute the orthogonal projection $\hat{p} = P_{\text{proj}} b = A(A^T A)^{-1}A^T b$."
  - "On screen, we verify that the residual $e = b - \hat{p}$ is strictly orthogonal to $\text{Col}(A)$: $A^T e \approx \mathbf{0}$ within machine precision ($10^{-15}$)."
- **Talking points (Member 2):**
  - "Solving yields coefficients: $a_0 \approx 0.5103$, $a_1 \approx 0.4325$, $a_2 \approx 0.1151$, and $a_3 \approx -0.0203$, giving $R^2 \approx 0.8857$."
  - "Look at our degree comparison table on screen:
    - Degree 1 (linear) already captures **86.74%** of variance.
    - Degree 2 jumps to **88.55%**, capturing acceleration since the 1980s.
    - Degree 3 adds a tiny marginal gain of just **+0.02%** ($R^2 = 88.57\%$), which is why $a_3$ is so small.
    - Degrees 4 through 6 show dropping Adjusted $R^2$ and rising cross-validation error, confirming that higher degrees overfit."
- **Handoff:** "Now Member 1 and Member 3 walk through interpolation versus extrapolation."

---

#### Minute 5: Interpolation & Extrapolation Analysis (Presenters: Member 1 + Member 3)
- **What to show on screen:**
  - Section 9 interpolation table, Section 10 extrapolation table, and Section 12 Plot 2 (combined trajectory).
- **Talking points (Member 1):**
  - "For interpolation, we evaluated 8 intermediate mid-years inside $[1980, 2023]$ (green diamonds). Because these points are bounded by real data on both sides, predictions are stable and reliable (e.g., $+0.49^\circ\text{C}$ in 2000.5)."
- **Talking points (Member 3):**
  - "For extrapolation (2025 to 2050), look closely at our numbers: the 5-year increments are nearly uniform at $\approx +0.15^\circ\text{C}$ per 5 years. The curve is essentially linear through 2050."
  - "Because $a_3 = -0.0203$ is negative, the warming rate actually peaks at an inflection point around **2041–2042** ($t \approx 1.89$), and the curve would eventually turn downward around year **2112** ($t \approx 5.16$). The honest limitation is not that it explodes to infinity in 2050, but that cubics turn over unphysically far out, and the model has no physics."
  - "Baseline clarification: NASA GISS uses the 1951–1980 baseline, which is $\approx 0.25^\circ\text{C}$ to $0.30^\circ\text{C}$ warmer than pre-industrial. Reaching +1.5°C above pre-industrial corresponds to $\approx +1.20^\circ\text{C}$ on NASA's scale, which our model crosses around **2028–2030**. The 2040 prediction of $+1.54^\circ\text{C}$ represents $\approx +1.80^\circ\text{C}$ above pre-industrial levels."
- **Handoff:** "Now Member 4 presents the eigendecomposition of the covariance matrix and our conclusion."

---

#### Minute 6: Eigenvalue Analysis & Scale Dependence (Presenter: Member 4)
- **What to show on screen:**
  - Section 11 covariance matrix $C$, eigenvalues, Pearson correlation output, and Section 12 Plot 4 (1×2 subplot).
- **Talking points (Member 4):**
  - "To examine bivariate structure, we computed the covariance matrix $C = \frac{1}{N-1}X_c^T X_c$ and diagonalized it as $C = P D P^T$, verified numerically on screen."
  - "Now, an essential linear algebra nuance: The unstandardized covariance matrix gives $\lambda_1 = 0.4213$ (**98.10%** of variance). But this figure is **scale-dependent**. Normalized time ($s_t^2 \approx 0.357$) and temperature in °C ($s_b^2 \approx 0.0725$) have different units, so the first component leans toward the variable with larger numerical values."
  - "The true unit-free metric is the **Pearson correlation**: $r \approx 0.9313$ ($r^2 \approx 86.74\%$). Time and temperature are strongly correlated, meaning a straight line explains about 87% of the variance."
  - "If we standardize variables to unit variance (correlation matrix PCA), $\lambda_1 = 1.9313$, explaining **96.57%** of standardized variance."
  - "Diagonalization decouples this correlated system into independent principal axes: dominant trend ($v_1$) and orthogonal climate noise ($v_2$)."

---

#### Minute 7: Visualizations, Limitations & Conclusion (Presenter: Member 4)
- **What to show on screen:**
  - Section 12 Plot 1 (fitted curve), Plot 3 (residual stem plot), and the Conclusion markdown cell.
- **Talking points (Member 4):**
  - "Our residual stem plot shows errors fluctuating evenly around zero, confirming our fit captures the primary trend without systematic bias."
  - "To conclude, linear algebra provides a transparent, rigorous framework for curve fitting from first principles:
    1. Time and temperature are strongly correlated ($r \approx 0.93$), with a cubic model explaining 88.57% of historical variance.
    2. Model comparison proves degree 3 captures acceleration without the overfitting seen in degrees 4 to 6.
    3. Solving via Gram-Schmidt back-substitution matches normal equations while preserving numerical conditioning."
  - "Our model's honest limitations are that cubics produce unphysical turnover far out (around 2112), and the model contains no physical radiative transfer or carbon cycle feedbacks."
  - "In atmospheric science, this exact mathematics is scaled to millions of dimensions as **Empirical Orthogonal Functions (EOFs)** to isolate global warming and El Niño modes."
  - "Thank you Professor! We are ready for your questions."

---

### End Note: Viva Coordination & Handoffs
> **If asked who did what, each member explains their specific sections:**
> - **Member 1:** "I handled data preprocessing and baseline definition, set up the normalized Vandermonde matrix, justified degree 3 selection, implemented Gaussian elimination with partial pivoting for RREF, verified rank inconsistency, and analyzed interpolation."
> - **Member 2:** "I implemented Classical Gram-Schmidt to factor $A = QR$, solved $R\hat{x} = Q^T b$ via back-substitution to obtain coefficients without forming $A^T A$, derived normal equations, and conducted the degree 1 to 6 comparison with 5-fold cross-validation."
> - **Member 3:** "I analyzed column space and null space geometry, derived the orthogonal projection matrix $P_{\text{proj}}$, verified residual orthogonality, and analyzed the 2025–2050 extrapolation trajectory, identifying the inflection point and pre-industrial baseline offset."
> - **Member 4:** "I formulated the covariance matrix, computed eigendecomposition and addressed scale dependence, verified matrix diagonalization $C = P D P^T$, created the visualization suite, and synthesized our scientific conclusions and model limitations."
