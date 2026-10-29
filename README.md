# Interpolation, Extrapolation & Climate Change
## MFAD Mini-Project (UE25MA242A)

This project applies linear algebra from first principles to model global climate warming using annual global temperature anomaly data (1980–2023) from NASA GISS. Rather than using black-box libraries, all mathematical transformations—Vandermonde matrix modeling, Gaussian elimination, RREF, rank analysis, Gram-Schmidt orthogonalization, orthogonal projection, normal equations, least squares fitting, and covariance eigendecomposition—are implemented and visualized directly in Python using only `numpy`, `pandas`, and `matplotlib`.

---

## 👥 Team Members & Contributions

| Member | Specific Contributions & Topics Owned |
| :--- | :--- |
| **Member 1** | Dataset curation, Vandermonde design matrix construction with year normalization, custom Gaussian elimination with partial pivoting for RREF, rank & system consistency analysis, and intermediate polynomial interpolation. |
| **Member 2** | Gram-Schmidt orthogonalization algorithm ($QR$ factorization), derivation of normal equations $A^T A \hat{x} = A^T b$, least squares parameter estimation, and coefficient of determination ($R^2$) evaluation. |
| **Member 3** | Column space ($\text{Col}(A)$) and null space ($\text{Nul}(A)$) geometric analysis, orthogonal projection matrix ($P_{\text{proj}}$) computation, residual orthogonality verification, and future temperature extrapolation (2024–2050). |
| **Member 4** | Sample covariance matrix construction, eigenvalue/eigenvector decomposition, matrix diagonalization ($C = P D P^T$), full visualization suite (fits, residuals, extrapolation, and PCA projections), project synthesis, and viva guide. |

---

## 🛠️ Setup Instructions for Teammates (Windows & Mac)

👉 **For the complete, OS-specific setup walkthrough, see [TEAMMATE_SETUP_GUIDE.md](TEAMMATE_SETUP_GUIDE.md).**

### Quick Setup (3 Steps):

1. **Clone the repo:**
   ```bash
   git clone https://github.com/YatishGowda09/climate-change-linear-algebra.git
   cd climate-change-linear-algebra
   ```

2. **Create and activate a virtual environment:**
   - **Windows (PowerShell):**
     ```powershell
     py -m venv .venv
     .\.venv\Scripts\Activate.ps1
     ```
   - **macOS / Linux:**
     ```bash
     python3 -m venv .venv
     source .venv/bin/activate
     ```

3. **Install dependencies & launch:**
   ```bash
   pip install -r requirements.txt
   jupyter notebook Climate_Change_Project.ipynb
   ```
   *(Or open in **VS Code**, select `.venv` as your Python Kernel, and click **Run All**).*

> 💡 **No setup required option:** You can also upload `Climate_Change_Project.ipynb` and `climate_data.csv` directly into **Google Colab**!

---

## 📁 Repository Structure

```
climate-change-linear-algebra/
├── climate_data.csv                 # Cleaned NASA GISS annual global temperature anomalies (1980–2023)
├── Climate_Change_Project.ipynb     # Complete, single, pre-rendered Jupyter notebook with all math, code & plots
├── presentation_walkthrough.md      # Minute-by-minute speaking guide for the 5-7 minute team demo
├── viva_cheatsheet.md               # Concept → Purpose → Outcome cheat sheet and anticipated viva Q&A
├── TEAMMATE_SETUP_GUIDE.md          # Comprehensive setup guide for Windows, macOS, Linux & Google Colab
├── requirements.txt                 # Project dependencies (numpy, pandas, matplotlib, jupyter, ipykernel)
├── .gitignore                       # Clean Git configuration (ignores .venv, caches, etc.)
└── README.md                        # Project documentation and summary
```

---

## 🗺️ Mathematical Workflow (11 Core Linear Algebra Stages)

1. **Real-World Data:** 44 annual observations of global surface temperature anomalies relative to the 1951–1980 baseline.
2. **Matrix Representation:** Bounded time coordinates $t \in [-1, 1]$ mapped into a $44 \times 4$ Vandermonde matrix $A = [1, t, t^2, t^3]$ for the system $Ax = b$.
3. **Matrix Simplification:** Custom Gaussian elimination with partial pivoting produces $\text{rref}(A)$ and $\text{rref}([A \mid b])$.
4. **Consistency & Rank Check:** $\text{rank}(A) = 4 < \text{rank}([A \mid b]) = 5$, rigorously proving by the Rouché–Capelli theorem that $b \notin \text{Col}(A)$ and the system is inconsistent (no exact cubic passes through all 44 noisy data points).
5. **Subspace Analysis:** $\dim(\text{Col}(A)) = 4 \subset \mathbb{R}^{44}$, while $\text{nullity}(A) = 4 - 4 = 0$, guaranteeing that the optimal least squares parameter solution is strictly unique.
6. **Gram-Schmidt Orthogonalization:** Orthonormal basis $\{q_1, q_2, q_3, q_4\}$ constructed with $Q^T Q = I_4$ and $A = Q R_{\text{gs}}$.
7. **Orthogonal Projection:** $\hat{p} = P_{\text{proj}} b = A(A^T A)^{-1}A^T b$, minimizing Euclidean prediction error $\|e\| = \|b - \hat{p}\| \approx 0.5971$. Residual satisfies $A^T e = \mathbf{0}$.
8. **Least Squares Parameter Estimation:** Solving normal equations $(A^T A)\hat{x} = A^T b$ gives $a_0 \approx 0.5103$, $a_1 \approx 0.4325$, $a_2 \approx 0.1151$, $a_3 \approx -0.0203$ with $R^2 \approx 0.8857$ (explaining 88.57% of temperature variance).
9. **Interpolation:** Reliable estimates for unobserved mid-years (e.g., $T(2000.5) \approx 0.4904^\circ\text{C}$) tightly bounded by empirical training data.
10. **Extrapolation:** Projections to 2050 ($+1.24^\circ\text{C}$ in 2030, $+1.54^\circ\text{C}$ in 2040, $+1.84^\circ\text{C}$ in 2050); crossing the +1.5°C Paris Agreement threshold around 2038–2040. Limitations of polynomial divergence are discussed.
11. **Eigendecomposition & Diagonalization:** $2 \times 2$ covariance matrix factored into $C = P D P^T$. Dominant eigenvalue $\lambda_1 = 0.4213$ explains **98.10%** of total variance (multi-decadal warming trend), while $\lambda_2 = 0.0081$ explains **1.90%** (short-term climate noise).

---

## 🎤 Presentation & Viva Preparation
- **Demo Walkthrough:** See [`presentation_walkthrough.md`](presentation_walkthrough.md) for speaking order and assigned minutes.
- **Viva Defense:** See [`viva_cheatsheet.md`](viva_cheatsheet.md) for concept summaries and anticipated professor Q&A.
