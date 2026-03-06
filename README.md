
# IFC — Italian Financial Challenge

**Challenge 2: Financial Health Classification**  
Multiclass classifier to predict `financial_health_class` (A/B/C/D) for Italian companies.  
Target metric: **Weighted F1 > 0.65**

---

## Project Overview

This project is part of the LUISS Italian Financial Data Challenge (Academic Year 2024/2025).  
We classify companies into 4 financial health categories based on balance sheet, income statement, and financial ratio data from 2018–2023.

| Class | Meaning |
|-------|---------|
| A | Excellent financial health |
| B | Good financial health |
| C | Moderate risk |
| D | High risk / distressed |

---

## Quick Setup

### Prerequisites
- Python **3.12.13** (pinned in `.python-version`)
- [uv](https://docs.astral.sh/uv/) package manager
- git

### 1. Clone the repository

```bash
git clone https://github.com/LolloPaz/ifc.git
cd ifc
```

### 2. Install dependencies

```bash
uv sync
```

> `uv` will automatically use Python 3.12.13 as declared in `.python-version` and restore the exact environment from `uv.lock`.

### 3. Install the local package in editable mode

```bash
uv pip install -e .
```

### 4. Add data files (not tracked by git)

Place the data files provided by the course in:

```
data/processed/train_data.csv       # 11,828 observations (2018–2021)
data/processed/test_features.csv    # 5,811 observations (2022–2023)
```

### 5. Launch JupyterLab

```bash
uv run jupyter lab
```

---

## Repository Structure

```
ifc/
├── pyproject.toml              # Project metadata and dependencies (uv)
├── uv.lock                     # Locked dependency tree (always commit this)
├── .python-version             # Pinned Python 3.12.13
├── .gitignore
├── README.md
│
├── data/                       # ⚠️ Not tracked by git — add manually
│   └── processed/
│       ├── train_data.csv
│       └── test_features.csv
│
├── notebooks/                  # Jupyter notebooks
│   └── 01_eda.ipynb
│
└── src/
    └── ifc/                    # Installable Python package
        ├── __init__.py
        ├── config.py           # Paths, constants, column definitions
        └── preprocessing.py    # (coming soon)
```

---

## Dependencies

Managed via `uv`. Key libraries:

| Library | Purpose |
|---------|---------|
| `pandas`, `numpy` | Data manipulation |
| `scikit-learn` | ML models, preprocessing, metrics |
| `xgboost`, `lightgbm` | Gradient boosting models |
| `imbalanced-learn` | SMOTE and imbalance handling |
| `shap` | Model interpretability |
| `matplotlib`, `seaborn`, `plotly` | Visualizations |
| `jupyterlab` | Notebook environment |

---

## Package Configuration

The `src/ifc/` folder is an installable package configured via `pyproject.toml`:

```toml
[build-system]
requires = ["setuptools>=68"]
build-backend = "setuptools.build_meta"

[tool.setuptools.packages.find]
where = ["src"]
```

Import from anywhere in the project without path hacks:

```python
from ifc.config import TRAIN_FILE, TARGET, SEED
```

---

## Key Constants (`src/ifc/config.py`)

| Constant | Value |
|----------|-------|
| `TARGET` | `"financial_health_class"` |
| `CLASSES` | `["A", "B", "C", "D"]` |
| `TRAIN_YEARS` | `[2018, 2019, 2020, 2021]` |
| `TEST_YEARS` | `[2022, 2023]` |
| `SEED` | `42` |

---

## Important Rules

- **No data leakage**: split is always temporal (`fiscal_year`), never random
- **Fit on train only**: scalers and imputers must be fit on training data and applied to test
- **Data files are never committed**: `data/` is in `.gitignore`
- **Evaluate with Weighted F1**: accuracy is misleading due to class imbalance (A≈30%, B≈40%, C≈20%, D≈10%)

---

## Evaluation Targets

| Level | Weighted F1 |
|-------|-------------|
| Minimum | > 0.60 |
| Good | 0.65 – 0.75 |
| Excellent | > 0.75 |

---

## Environment Details

| Item | Value |
|------|-------|
| Python | 3.12.13 |
| Package manager | uv |
| Install mode | Editable (`-e .`) |
| VCS | GitHub |

---

**Institution**: LUISS University — Academic Year 2024/2025  
**Repository**: [https://github.com/LolloPaz/ifc](https://github.com/LolloPaz/ifc)
