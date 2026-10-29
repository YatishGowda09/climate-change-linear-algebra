# 🛠️ Teammate Setup Guide: Climate Change Linear Algebra Project

Welcome to the project repository! This guide provides step-by-step instructions to get the project running on your local machine (**Windows**, **macOS**, or **Linux**), as well as a zero-installation **Google Colab** fallback.

---

## 📌 Project Overview
- **Project Name:** Interpolation, Extrapolation & Climate Change
- **Course:** UE25MA242A — Mathematics for AI/Data (MFAD)
- **Primary Deliverable:** [`Climate_Change_Project.ipynb`](Climate_Change_Project.ipynb) (Jupyter Notebook)
- **Dataset:** [`climate_data.csv`](climate_data.csv)
- **Required Libraries:** `numpy`, `pandas`, `matplotlib`, `jupyter` / `ipykernel`

---

## 🪟 Setup for Windows

### Step 1: Prerequisites
1. **Python 3.10+**:
   - If not installed, download from [python.org](https://www.python.org/downloads/) or the Microsoft Store.
   - ⚠️ **CRITICAL:** During installation, check the box that says **"Add python.exe to PATH"**.
2. **Git**: Download and install from [git-scm.com](https://git-scm.com/) (or use GitHub Desktop).
3. **Editor (Recommended):** [VS Code](https://code.visualstudio.com/) with the **Python** and **Jupyter** extensions installed.

### Step 2: Clone the Repository
Open **PowerShell** or **Command Prompt** and run:
```powershell
git clone https://github.com/YatishGowda09/climate-change-linear-algebra.git
cd climate-change-linear-algebra
```

### Step 3: Create a Virtual Environment
In PowerShell or Command Prompt:
```powershell
py -m venv .venv
```
*(If `py` is not recognized, use `python -m venv .venv`)*

### Step 4: Activate the Virtual Environment
- **In PowerShell:**
  ```powershell
  .\.venv\Scripts\Activate.ps1
  ```
  > 💡 **PowerShell Fix:** If you get a script execution policy error (`running scripts is disabled on this system`), run:
  > ```powershell
  > Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
  > ```
  > Then run `.\.venv\Scripts\Activate.ps1` again.

- **In Command Prompt (CMD):**
  ```cmd
  .\.venv\Scripts\activate.bat
  ```

*(You will see `(.venv)` appear at the beginning of your terminal line).*

### Step 5: Install Dependencies
```powershell
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### Step 6: Run the Notebook
You can run the notebook in either **VS Code** or your **browser**:

- **Option A (VS Code - Recommended):**
  1. Open the project folder in VS Code (`code .`).
  2. Open `Climate_Change_Project.ipynb`.
  3. In the top-right corner of the notebook editor, click **"Select Kernel"** -> **"Python Environments"** -> select `.venv (Python 3.x)`.
  4. Click **"Run All"**.

- **Option B (Browser via Jupyter):**
  ```powershell
  jupyter notebook Climate_Change_Project.ipynb
  ```

---

## 🍎 Setup for macOS

### Step 1: Prerequisites
1. **Python 3.10+**: Check if installed by opening **Terminal** and typing `python3 --version`. If needed, install via Homebrew (`brew install python`) or download from [python.org](https://www.python.org/downloads/).
2. **Git**: Included with Xcode Command Line Tools. Run `git --version` in Terminal (macOS will prompt to install if missing).
3. **Editor (Recommended):** [VS Code](https://code.visualstudio.com/) with the **Python** and **Jupyter** extensions.

### Step 2: Clone the Repository
Open **Terminal** and run:
```bash
git clone https://github.com/YatishGowda09/climate-change-linear-algebra.git
cd climate-change-linear-algebra
```

### Step 3: Create a Virtual Environment
```bash
python3 -m venv .venv
```

### Step 4: Activate the Virtual Environment
```bash
source .venv/bin/activate
```
*(You will see `(.venv)` in your prompt).*

### Step 5: Install Dependencies
```bash
python3 -m pip install --upgrade pip
pip install -r requirements.txt
```

### Step 6: Run the Notebook
- **Option A (VS Code):**
  1. Open folder in VS Code (`code .`).
  2. Open `Climate_Change_Project.ipynb`.
  3. Click **"Select Kernel"** (top right) -> choose `.venv`.
  4. Click **"Run All"**.

- **Option B (Browser via Jupyter):**
  ```bash
  jupyter notebook Climate_Change_Project.ipynb
  ```

---

## 🐧 Setup for Linux (Ubuntu / Debian / Arch / Fedora)

```bash
git clone https://github.com/YatishGowda09/climate-change-linear-algebra.git
cd climate-change-linear-algebra

# Create & activate venv
python3 -m venv .venv
source .venv/bin/activate

# Install dependencies
pip install --upgrade pip
pip install -r requirements.txt

# Launch notebook
jupyter notebook Climate_Change_Project.ipynb
```

---

## ☁️ Zero-Installation Alternative: Google Colab

If someone's local Python environment is giving trouble, you can run the entire project in **Google Colab** in under 30 seconds:

1. Go to [colab.research.google.com](https://colab.research.google.com/).
2. Click **File** -> **Open notebook** -> select the **GitHub** tab.
3. Paste the repository URL: `https://github.com/YatishGowda09/climate-change-linear-algebra`.
4. Select `Climate_Change_Project.ipynb`.
5. Upload `climate_data.csv` to the Colab session files (folder icon on the left sidebar).
6. Click **Runtime** -> **Run all** (`Ctrl + F9`).

---

## ✅ Verifying Your Setup
Once opened, click **Restart Kernel & Run All Cells**. Verify that:
- [x] All 17 code cells execute without any error.
- [x] The 5 plots render cleanly:
  1. Raw data scatter plot
  2. Least squares cubic polynomial fit
  3. Interpolation & extrapolation trajectory extending to 2050
  4. Residual stem plot
  5. Centered data cloud with principal eigenvector arrows & variance bar chart
- [x] Summary printouts display:
  - System consistency: $\text{rank}(A) = 4, \text{rank}([A \mid b]) = 5$ (Inconsistent)
  - Fit quality: $R^2 \approx 0.8857$ (88.57% variance explained)
  - Dominant eigenvalue: $\lambda_1 = 0.4213$ (98.10% variance)

---

## 🎤 Presentation & Viva Preparation
Please study the accompanying documents before our demo:

- 📋 [**`presentation_walkthrough.md`**](presentation_walkthrough.md):
  - Minute-by-minute speaking script for our **5 to 7 minute presentation**.
  - Review your assigned speaking minute and the exact notebook cells/plots you need to present.
- 💡 [**`viva_cheatsheet.md`**](viva_cheatsheet.md):
  - Concept $\rightarrow$ Purpose $\rightarrow$ Outcome breakdown for all 11 linear algebra topics.
  - Anticipated professor viva questions with student-friendly answers.

---

## ❓ Common Troubleshooting

| Issue | Cause | Solution |
| :--- | :--- | :--- |
| `ModuleNotFoundError: No module named 'numpy'` (or pandas/matplotlib) | Notebook is using the global Python instead of the virtual environment | In VS Code, click **Select Kernel** at top-right and choose `.venv`. In terminal, make sure `source .venv/bin/activate` or `.\.venv\Scripts\Activate.ps1` was executed before launching Jupyter. |
| `FileNotFoundError: [Errno 2] No such file or directory: 'climate_data.csv'` | The notebook's working directory is not the project root | Make sure you launched Jupyter/VS Code from the `climate-change-linear-algebra` folder where `climate_data.csv` resides. |
| PowerShell script execution error | Windows execution policy blocks unsigned scripts | Run `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser` in PowerShell, then re-activate. |
| In VS Code, `.venv` does not show in kernel list | VS Code hasn't detected the new folder yet | Press `Ctrl+Shift+P` (or `Cmd+Shift+P` on Mac), type `Python: Select Interpreter`, and select `.venv/bin/python` (or `.\.venv\Scripts\python.exe`). |

---
*Ready to rock the presentation! Reach out on our group chat if you hit any setup roadblocks.*
