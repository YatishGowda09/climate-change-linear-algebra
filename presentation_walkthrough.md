# Team Presentation Walkthrough & Demo Notes
## MFAD Mini-Project: "Interpolation, Extrapolation & Climate Change"
**Format:** Live Jupyter notebook demonstration  
**Target Duration:** 5 to 7 minutes total (~1 minute per topic)  
**Style:** Internal team prep notes — keep it conversational, direct, and focused on the linear algebra concepts.

---

### Team Roles & Topic Ownership
- **Member 1:** Data loading, matrix representation, RREF algorithm, rank & consistency check, interpolation
- **Member 2:** Gram-Schmidt orthogonalization ($QR$ factorization), least squares derivation & solution
- **Member 3:** Column space & null space geometry, orthogonal projection matrix, extrapolation analysis
- **Member 4:** Covariance eigendecomposition, matrix diagonalization, visualization suite, project conclusion

---

### Minute-by-Minute Demo Flow

#### Minute 1: Introduction & Data Exploration (Presenter: Member 1)
- **What to show on screen:**
  - Section 1 markdown title and Section 2 raw scatter plot (`NASA GISS Global Temperature Anomalies (1980–2023)`).
- **Talking points:**
  - "Hello Professor, our mini-project applies fundamental linear algebra to model historical climate change using NASA GISS global surface temperature anomalies from 1980 through 2023."
  - "We have 44 discrete annual observations showing a visible upward trajectory of around +0.9°C over four decades."
  - "Instead of treating curve fitting as an arbitrary optimization or using high-level libraries, our goal is to derive polynomial fitting from first principles using vector spaces, projection, and eigendecomposition."
- **Handoff:** Move directly into the matrix setup.

---

#### Minute 2: Matrix Representation, RREF & Rank Analysis (Presenter: Member 1)
- **What to show on screen:**
  - Section 3 printed Vandermonde matrix $A$ ($44 \times 4$), Section 4 RREF printed matrices, and Section 5 rank output.
- **Talking points:**
  - "To fit a cubic polynomial $T(t) = a_0 + a_1 t + a_2 t^2 + a_3 t^3$, we build an overdetermined linear system $Ax = b$ with a $44 \times 4$ Vandermonde matrix."
  - "Crucial step: We normalized calendar years to $[-1, 1]$. If we used raw years like 2023, terms like $2023^3 \approx 8 \times 10^9$ cause matrix condition numbers over $10^{15}$, destroying numerical stability. Normalization keeps condition numbers low."
  - "We implemented our own Gaussian elimination with partial pivoting. RREF of $A$ shows 4 pivots, so $\text{rank}(A) = 4$ (full column rank)."
  - "However, RREF of the augmented matrix $[A \mid b]$ has rank 5, with row 5 showing $[0 \; 0 \; 0 \; 0 \mid 1]$ — the contradiction $0 = 1$."
  - "By the Rouché–Capelli theorem, because $\text{rank}(A) < \text{rank}([A \mid b])$, the system is strictly inconsistent. No single cubic curve can pass through 44 noisy data points. That proves why we must find a least squares solution."
- **Handoff:** "Now Member 3 and Member 2 will explain the geometry of subspaces and how we project into the column space."

---

#### Minute 3: Column Space, Null Space & Gram-Schmidt (Presenters: Member 3 + Member 2)
- **What to show on screen:**
  - Section 6 column/null space printouts, followed by Section 7 Gram-Schmidt code and verification `np.allclose(Q.T @ Q, np.eye(4))`.
- **Talking points (Member 3):**
  - "Geometrically, the column space $\text{Col}(A)$ is a 4-dimensional subspace inside $\mathbb{R}^{44}$."
  - "By Rank-Nullity, $\text{nullity}(A) = 4 - 4 = 0$. Because the null space contains only the zero vector, any least squares solution is guaranteed to be unique."
  - "The observation vector $b$ lives in $\mathbb{R}^{44}$ but sticks outside $\text{Col}(A)$ due to climate noise."
- **Talking points (Member 2):**
  - "To solve the system stably without inverting the potentially sensitive normal matrix $A^T A$, we implemented Gram-Schmidt orthogonalization from scratch to decompose $A = QR$."
  - "Notice our verification output on screen: $Q^T Q = I_4$. The columns of $Q$ provide a clean orthonormal basis for $\text{Col}(A)$, allowing us to solve $R\hat{x} = Q^T b$ by simple back-substitution."
- **Handoff:** "Next, we compute the projection and find our optimal polynomial coefficients."

---

#### Minute 4: Projection & Least Squares Fit (Presenters: Member 3 + Member 2)
- **What to show on screen:**
  - Section 8 projection vector $\hat{p}$, residual orthogonality check $A^T (b - \hat{p}) \approx 0$, and least squares coefficients with $R^2$.
- **Talking points (Member 3):**
  - "To find the closest approximation to $b$ inside $\text{Col}(A)$, we compute the orthogonal projection $\hat{p} = P_{\text{proj}} b = A(A^T A)^{-1}A^T b$."
  - "The residual vector $e = b - \hat{p}$ represents the geometric error perpendicular to $\text{Col}(A)$."
  - "On screen, we verify that $A^T e \approx \mathbf{0}$ within machine precision ($10^{-15}$). The residual norm $\|e\|_2 \approx 0.5971$ is the minimum possible Euclidean distance."
- **Talking points (Member 2):**
  - "The projection $\hat{p}$ exactly equals $A\hat{x}$, where $\hat{x} = [a_0, a_1, a_2, a_3]^T$ are our least squares coefficients."
  - "We get $a_0 \approx 0.5103$, $a_1 \approx 0.4325$, $a_2 \approx 0.1151$, and $a_3 \approx -0.0203$."
  - "Our model achieves an $R^2 \approx 0.8857$, meaning over 88.5% of total temperature variance is captured by our cubic polynomial."
- **Handoff:** "Now Member 1 and Member 3 will walk through our model evaluation: interpolation versus extrapolation."

---

#### Minute 5: Interpolation & Extrapolation Analysis (Presenters: Member 1 + Member 3)
- **What to show on screen:**
  - Section 9 interpolation table, Section 10 extrapolation table, and Section 12 Plot 2 (combined curve with green diamonds and red stars).
- **Talking points (Member 1):**
  - "For interpolation, we evaluated 8 unobserved mid-years inside the historical window $[1980, 2023]$ (shown as green diamonds)."
  - "Because these points are flanked on both sides by real data, the polynomial is tightly constrained. For example, in 2000.5 we predict $+0.51^\circ\text{C}$, which matches the historical decadal trend."
- **Talking points (Member 3):**
  - "For extrapolation, we projected the model forward to 2050 at 5-year intervals (shown as red stars in the light red shaded region)."
  - "The model predicts anomalies of $+1.24^\circ\text{C}$ by 2030, $+1.54^\circ\text{C}$ by 2040, and $+1.84^\circ\text{C}$ by 2050."
  - "Crucially, our polynomial crosses the Paris Agreement target of +1.5°C around year 2038–2040."
  - "However, as linear algebra students, we know polynomials are unstable outside their domain. As $t > 1$, the $t^3$ term dominates and small coefficient errors blow up. Predictions past 2035 have high mathematical uncertainty."
- **Handoff:** "Now Member 4 will present the eigendecomposition of the covariance matrix, our visualizations, and the conclusion."

---

#### Minute 6: Eigenvalue Analysis & Diagonalization (Presenter: Member 4)
- **What to show on screen:**
  - Section 11 covariance matrix $C$, eigenvalues, diagonalization check `np.allclose(C, P @ D @ P.T)`, and Section 12 Plot 4 (1×2 subplot).
- **Talking points (Member 4):**
  - "To understand the bivariate structure between normalized time and temperature, we constructed the $2 \times 2$ covariance matrix $C = \frac{1}{N-1}X_c^T X_c$ and computed its eigendecomposition."
  - "Because $C$ is symmetric, its eigenvectors are orthogonal. We diagonalize it as $C = P D P^T$ and verify it numerically on screen with `np.allclose` returning `True`."
  - "Look at the eigenvalues: $\lambda_1 = 0.4213$ and $\lambda_2 = 0.0081$."
  - "In the bar chart on the right, $\lambda_1$ accounts for **98.10%** of total variance! Its eigenvector $v_1$ points diagonally upwards in our scatter plot on the left, capturing the dominant long-term warming trend."
  - "The second eigenvalue $\lambda_2$ accounts for only **1.90%** of variance. Its eigenvector $v_2$ points perpendicular to the trend, representing short-term climate noise like annual weather variations."
  - "Diagonalization shows that global warming is almost entirely a 1-dimensional principal component signal."

---

#### Minute 7: Visualizations, Limitations & Conclusion (Presenter: Member 4)
- **What to show on screen:**
  - Section 12 Plot 1 (fitted curve), Plot 3 (residual stem plot), and the Conclusion markdown cell.
- **Talking points (Member 4):**
  - "Looking at our residual stem plot (Plot 3), the residuals are centered around zero with no severe skew, confirming our cubic fit is well-behaved."
  - "To conclude, our least squares cubic model captures 88.6% of historical temperature variance, and eigendecomposition confirms 98.1% of bivariate variance aligns with the warming trend."
  - "However, our model has clear limitations:
    1. Cubic polynomial extrapolation diverges to infinity long-term.
    2. We only modeled time, omitting greenhouse gas emissions (CO₂), aerosols, and solar cycles.
    3. We used empirical linear algebra rather than physics-based thermodynamic equations."
  - "In real-world climate science, this exact linear algebra is scaled up to millions of dimensions. Climate scientists use covariance eigendecomposition—known as **Empirical Orthogonal Functions (EOFs)**—to discover modes like El Niño, and use least squares data assimilation to calibrate climate models."
  - "Thank you Professor! We are ready for your questions."

---

### End Note: Viva Coordination & Handoffs
> **If asked who did what, each member explains their specific sections:**
> - **Member 1:** "I handled the dataset preprocessing, formulated the Vandermonde matrix with normalized time, implemented Gaussian elimination with partial pivoting for RREF, verified matrix rank and system inconsistency, and evaluated interpolation."
> - **Member 2:** "I implemented Gram-Schmidt orthogonalization from scratch to factor $A = QR$, solved the normal equations via back-substitution to obtain the least squares coefficients, and evaluated model goodness-of-fit ($R^2$)."
> - **Member 3:** "I analyzed the geometry of the column space and null space, derived the orthogonal projection matrix $P_{\text{proj}}$, demonstrated residual orthogonality, and computed the future extrapolations to 2050."
> - **Member 4:** "I formulated the covariance matrix, computed its eigenvalues and eigenvectors, verified matrix diagonalization $C = P D P^T$, created the visualization suite (raw fit, extrapolation, residuals, PCA plots), and synthesized the scientific conclusion."

---

### Quick Emergency Tips for Demo Day
- **If the professor asks you to re-run a cell:** All code cells are self-contained and run in under 0.5 seconds.
- **If asked about packages:** "We used only standard `numpy`, `pandas`, and `matplotlib` as required. No `scipy` or `sklearn` black boxes were used for RREF, QR, or projection."
- **If the projector is low resolution:** Plots use 11-13pt fonts and high-contrast color palettes so they remain readable from the back of the classroom.
