# MFAD Mini-Project: Viva Preparation Guide
## Topic: Interpolation, Extrapolation & Climate Change

This cheatsheet provides a quick-reference summary of all mathematical concepts implemented in our mini-project, followed by anticipated viva questions and student-level explanations.

---

### Core Concepts Summary Table

| Concept | What We Did | Why | Result |
| :--- | :--- | :--- | :--- |
| **Matrix representation** | Built a $44 \times 4$ Vandermonde matrix $A = [1, t, t^2, t^3]$ using normalized time coordinates $t \in [-1, 1]$ and target vector $b$ with NASA GISS temperature anomalies. | Formulated the cubic polynomial curve fitting task as a linear system $Ax = b$ to solve for coefficients $[a_0, a_1, a_2, a_3]^T$. | An overdetermined $44 \times 4$ system. Normalization bounded entries in $[-1, 1]$ to avoid numerical instability from huge powers ($1980^3$). |
| **RREF** | Programmed Gaussian elimination with partial pivoting from scratch in NumPy to compute $\text{rref}(A)$ and $\text{rref}([A \mid b])$. | To locate pivot positions, check column linear independence, and test whether an exact polynomial solution exists. | Found 4 pivots in $A$ (full column rank). In $[A \mid b]$, the 5th column has a pivot yielding row $[0 \; 0 \; 0 \; 0 \mid 1]$, proving the system is inconsistent. |
| **Rank** | Evaluated rank by counting non-zero pivot rows: $\text{rank}(A) = 4$ and $\text{rank}([A \mid b]) = 5$. Verified via `np.linalg.matrix_rank`. | To apply the Rouché–Capelli theorem to rigorously prove why an exact interpolating cubic cannot fit all 44 noisy data points. | $\text{rank}(A) < \text{rank}([A \mid b])$, confirming $b \notin \text{Col}(A)$. This mathematically justifies moving to least squares. |
| **Column/null space** | Analyzed fundamental subspaces of $A$: $\dim(\text{Col}(A)) = 4 \subset \mathbb{R}^{44}$ and $\text{Null}(A) = \{\mathbf{0}\}$ (nullity = 0). | To determine what temperature behaviors the model can represent and verify parameter uniqueness. | 4D column space captures smooth cubic trends; nullity = 0 guarantees a unique least squares coefficient solution $\hat{x}$. |
| **Gram-Schmidt** | Implemented Gram-Schmidt orthogonalization from scratch to decompose $A$ into $Q \in \mathbb{R}^{44 \times 4}$ with orthonormal columns and upper-triangular $R \in \mathbb{R}^{4 \times 4}$. | To compute an orthonormal basis for $\text{Col}(A)$ and solve normal equations stably without directly inverting $A^T A$. | Verified $Q^T Q = I_4$; enables solving $R\hat{x} = Q^T b$ via simple back-substitution, avoiding numerical ill-conditioning. |
| **Projection** | Computed the orthogonal projection $\hat{p} = A\hat{x} = Q Q^T b$ of temperature vector $b$ onto $\text{Col}(A)$, and residual $e = b - \hat{p}$. | The projection $\hat{p}$ is the geometrically closest vector in $\text{Col}(A)$ to $b$, minimizing the Euclidean distance $\|b - \hat{p}\|_2$. | $\hat{p}$ gives our fitted values. Verified orthogonality $A^T e \approx \mathbf{0}$ ($< 10^{-14}$) and minimum error $\|e\|_2 \approx 0.5971$. |
| **Least squares** | Solved the normal equations $(A^T A)\hat{x} = A^T b$ to find optimal cubic coefficients $\hat{x} = [a_0, a_1, a_2, a_3]^T$. | To minimize the sum of squared prediction errors $S = \|Ax - b\|_2^2$. | Optimal coefficients: $a_0 \approx 0.5103$, $a_1 \approx 0.4325$, $a_2 \approx 0.1151$, $a_3 \approx -0.0203$. Achieved $R^2 \approx 0.8857$. |
| **Interpolation** | Evaluated the fitted polynomial at 8 unobserved intermediate mid-years (e.g., 1983.5, 1995.5, 2020.5) inside $[1980, 2023]$. | To estimate historical temperature anomalies between annual records. | Produced smooth, realistic values closely bounded by empirical data (e.g., $T(2000.5) \approx 0.51^\circ\text{C}$). High mathematical reliability. |
| **Extrapolation** | Evaluated the cubic model beyond the data boundary (2023) up to 2050 at 5-year intervals (2025, 2030, ..., 2050). | To forecast future warming trajectories and explore the mathematical risks of polynomial projection. | Projected $+1.24^\circ\text{C}$ by 2030, $+1.54^\circ\text{C}$ by 2040 (passing Paris Agreement limit), and $+1.84^\circ\text{C}$ by 2050; highlighted divergence risk. |
| **Eigenvalues** | Formed sample covariance matrix $C = \frac{1}{N-1}X_c^T X_c$ for centered $[t_{\text{norm}}, b]$ and computed eigenvalues using `np.linalg.eigh`. | To identify the principal directions of data variance and quantify how much variation is trend versus noise. | $\lambda_1 = 0.4213$ (98.10% variance, dominant warming trend) and $\lambda_2 = 0.0081$ (1.90% variance, year-to-year climate noise). |
| **Diagonalization** | Factored covariance matrix into $C = P D P^T$, where $P$ has orthonormal eigenvectors and $D = \text{diag}(\lambda_1, \lambda_2)$. | To decouple correlated time and temperature coordinates into independent orthogonal components. | Verified $C = P D P^T$ and $P^T P = I_2$ via `np.allclose`. Confirmed data variance aligns almost entirely along the first principal axis. |

---

### Sample Viva Questions & Student Answers

#### 1. Why did you normalize years to $[-1, 1]$ instead of using raw years like 1980 to 2023?
> **Student Answer:** If we plug raw calendar years into the Vandermonde matrix, the cubic column contains numbers like $2000^3 \approx 8 \times 10^9$. When we multiply $A^T A$, terms reach $t^6 \approx 6.4 \times 10^{19}$. That massive range of scales blows up the condition number past $10^{15}$, which causes catastrophic floating-point cancellation during Gaussian elimination. By mapping the years to $[-1, 1]$, every entry in $A$ stays between $-1$ and $1$, keeping the condition number small and the math numerically stable.

#### 2. Why write your own RREF and Gram-Schmidt functions instead of just calling `np.polyfit`?
> **Student Answer:** `np.polyfit` is a black box that hides the linear algebra. The whole objective of this mini-project is to show how polynomial fitting actually works from first principles—building the Vandermonde matrix, using RREF to show why an exact solution doesn't exist, and using Gram-Schmidt or normal equations to project $b$ onto the column space. Implementing them ourselves proves we understand the geometric mechanics behind the algorithms.

#### 3. What does your $R^2 \approx 0.886$ value actually mean?
> **Student Answer:** $R^2$ is the coefficient of determination. It tells us that about 88.6% of the total variance in observed temperature anomalies around their mean is captured by our cubic curve over time. The remaining ~11.4% corresponds to the squared residual norm $\|e\|^2$, which represents natural short-term climate noise like El Niño events or volcanic cooling that a smooth polynomial cannot capture.

#### 4. Can we actually trust your model's prediction of ~1.84°C in 2050?
> **Student Answer:** Mathematically, that's what the polynomial evaluates to, but physically, we shouldn't rely on it for long-term policy. Cubic polynomials are unconstrained outside the data domain, so they tend to diverge sharply toward infinity. Also, our model only considers time—it doesn't include physical drivers like greenhouse gas emissions, radiative forcing, or ocean heat uptake. It's an illustrative baseline, but real climate projections require physics-based Earth System Models.

#### 5. What do the eigenvalues of the covariance matrix tell us about climate change?
> **Student Answer:** The covariance matrix of centered normalized year and temperature has two eigenvalues: $\lambda_1 \approx 0.4213$ and $\lambda_2 \approx 0.0081$. The first eigenvalue accounts for 98.10% of total variance, and its eigenvector points along the positive diagonal (increasing time, increasing temperature). This mathematically proves that almost all variation in the data is driven by a single dominant direction—the multi-decadal warming trend. The remaining 1.90% is just orthogonal scatter, representing year-to-year climate noise.

#### 6. What is the fundamental difference between interpolation and extrapolation?
> **Student Answer:** Interpolation is estimating values *inside* the range of our training data ($1980 \le \text{year} \le 2023$), where the polynomial is anchored on both sides by real observations, making predictions stable and reliable. Extrapolation is evaluating *outside* that window ($\text{year} > 2023$). Because there are no future observations to anchor the curve, the cubic term takes over and small errors in the coefficients get magnified, making extrapolation far less reliable.

#### 7. Why did $\text{rank}(A) = 4$ while $\text{rank}([A \mid b]) = 5$?
> **Student Answer:** Matrix $A$ has 4 columns, so its column space is at most 4-dimensional. Because real temperature measurements contain noise and natural fluctuations, the 44 points do not lie on any single cubic curve. That means the observation vector $b$ doesn't lie in the 4D subspace $\text{Col}(A)$. When we augment $b$ into the matrix, it adds a 5th linearly independent column. By the Rouché–Capelli theorem, because $\text{rank}(A) < \text{rank}([A \mid b])$, the system $Ax = b$ is inconsistent and has no exact solution, which is why we must project $b$ onto $\text{Col}(A)$ using least squares.
