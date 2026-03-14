# AHS 2024 — Poverty & Demographics Analysis
### Group 1 | Economic and Social Statistics (AST3336) | Year 3, Semester 2 | AY 2025–2026
**University of Rwanda — College of Business and Economics | Department of Applied Statistics**

---

## 📌 Research Question
> **Which demographic and livelihood characteristics are associated with poverty among agricultural households in Rwanda?**

---

## 📊 Dataset
| Item | Detail |
|------|--------|
| **Survey** | Rwanda Agriculture Household Survey (AHS) 2024 |
| **Reference ID** | RWA-NISR-RAHS-2024-V0.1 |
| **Source** | National Institute of Statistics of Rwanda (NISR) |
| **Portal** | https://microdata.statistics.gov.rw |
| **Coverage** | National — 600 EAs across 30 districts |
| **Sample** | 3,724 agricultural households |
| **Files used** | Section 0, Section 1, Section 2 |

> ⚠️ **Raw data is not included in this repository** due to NISR data licensing. Download from the NISR microdata portal using the reference ID above.

---

## 🔑 Key Findings
| Finding | Result |
|---------|--------|
| National poverty rate | **23.87%** of agricultural households |
| Highest poverty province | **West (31.65%)** |
| Lowest poverty province | **Kigali (13.64%)** |
| Household size effect | Each additional member **+5.47pp** poverty probability |
| Land effect | Each log-unit of land **−11.25pp** poverty probability |
| Agriculture livelihood | Dramatically **reduces** poverty odds (OR = 0.008) |
| OLS R² | **0.1497** |
| Logit Pseudo-R² | **0.0893** |

---

## 🗂️ Repository Structure

```
AHS2024-Group1/
│
├── README.md
├── LICENSE
├── .gitignore
│
├── data/                          ← local only, not pushed to GitHub
│   ├── ahs2024_merged.csv         ← merged dataset (Notebooks 1 output)
│   └── ahs2024_analysis.csv       ← analysis dataset (Notebook 2 output)
│
├── notebooks/
│   ├── 01_data_merging.ipynb      ← Load & merge Section 0, 1, 2
│   ├── 02_variable_construction.ipynb  ← Construct all analysis variables
│   ├── 03_descriptive_analysis.ipynb   ← Tables & figures by province/sex
│   └── 04_regression_analysis.ipynb    ← Logit, Probit, Marginal Effects, OLS
│
├── output/
│   ├── tables/                    ← 11 CSV tables
│   │   ├── table1_summary_statistics.csv
│   │   ├── table2_by_province.csv
│   │   ├── table3_by_sex_of_head.csv
│   │   ├── table4_poverty_province_sex.csv
│   │   ├── table5_quintile_by_province.csv
│   │   ├── table6_ag_activity_poverty.csv
│   │   ├── table7_logit_results.csv
│   │   ├── table8_probit_results.csv
│   │   ├── table9_marginal_effects.csv
│   │   ├── table10_ols_results.csv
│   │   └── table11_model_fit.csv
│   │
│   └── figures/                   ← 8 publication-quality figures
│       ├── fig1_poverty_by_province.png
│       ├── fig2_poverty_by_sex.png
│       ├── fig3_quintile_by_province.png
│       ├── fig4_poverty_province_sex.png
│       ├── fig5_land_depratio_by_poverty.png
│       ├── fig6_marginal_effects.png
│       ├── fig7_ols_coefficients.png
│       └── fig8_predicted_probabilities.png
│
└── report/
    ├── main.tex                   ← Master LaTeX document
    ├── references.bib
    └── chapters/
        ├── chapter3_methodology.tex
        ├── chapter4_results.tex
        └── ...
```

---

## 🛠️ Tools & Technology
| Tool | Purpose |
|------|---------|
| **Python 3.13** | Primary analysis language |
| **pandas** | Data loading, merging, manipulation |
| **numpy** | Numerical computations |
| **statsmodels** | Logit, Probit, OLS regression |
| **matplotlib / seaborn** | Data visualization |
| **Jupyter Notebooks** | Reproducible analysis workflow |
| **VS Code** | Development environment |
| **LaTeX (XeLaTeX)** | Professional report typesetting |
| **Git / GitHub** | Version control & reproducibility |

---

## 📓 Notebooks Overview

### `01_data_merging.ipynb`
- Loads Section 0, Section 1, Section 2 from raw `.dta` Stata files
- Collapses individual-level Section 1 to household level
- Extracts household head characteristics (sex, age)
- Computes household size and dependency ratio
- Merges all three files on `hhid`
- Saves `data/ahs2024_merged.csv`

### `02_variable_construction.ipynb`
- Constructs binary poverty variable (`poor`)
- Constructs welfare quintile (`welfare_quintile`)
- Constructs livelihood dummy (`livelihood_ag`)
- Creates province dummy variables (Kigali = reference)
- Log-transforms land area
- Saves `data/ahs2024_analysis.csv`

### `03_descriptive_analysis.ipynb`
- Table 1: Overall summary statistics
- Table 2: Key indicators by province
- Table 3: Key indicators by sex of HH head
- Table 4: Poverty rate by province × sex of head
- Table 5: Welfare quintile distribution by province
- Table 6: Agricultural activity by poverty status
- 5 publication-quality figures

### `04_regression_analysis.ipynb`
- Logit model with odds ratios
- Probit model
- Average marginal effects (Logit vs Probit)
- OLS baseline model (robust SE)
- Model fit statistics (AIC, BIC, Pseudo-R²)
- 3 regression figures

---

## 🚀 How to Reproduce

### 1. Clone the repository
```bash
git clone https://github.com/Pierre-2/AHS2024-Group1.git
cd AHS2024-Group1
```

### 2. Install required packages
```bash
pip install pandas numpy matplotlib seaborn statsmodels scipy jupyter
```

### 3. Download the data
- Go to: https://microdata.statistics.gov.rw
- Search Reference ID: `RWA-NISR-RAHS-2024-V0.1`
- Download and extract `.dta` files to:
```
AHS2024-Group1/data/raw/
```

### 4. Update the data path
In `notebooks/01_data_merging.ipynb`, update:
```python
RAW = r"path\to\your\Microdata\folder"
```

### 5. Run notebooks in order
```
01_data_merging.ipynb
02_variable_construction.ipynb
03_descriptive_analysis.ipynb
04_regression_analysis.ipynb
```

---

## 📐 Model Specification

$$\text{poor}_i = f(\text{head\_sex}_i,\ \text{head\_age}_i,\ \text{head\_age}^2_i,\ \text{hh\_size}_i,\ \text{dep\_ratio}_i,\ \text{livelihood\_ag}_i,\ \ln(\text{land\_ha})_i,\ \mathbf{d\_province}_i)$$

**Three models estimated:**
- **Logit** — binary poverty status (DV)
- **Probit** — binary poverty status (DV), for robustness
- **OLS** — welfare quintile (DV), baseline model

---

## 👥 Group Members
| Name | Student REG No |
|------|-----------|
| ABIKUNDA RUHANGARWA Fides |222006152  |
| MUVUNYI NDAHIRO Lorenzo | 221022916 |
| NIYOMUGABO Jean Pierre | 222008736 |
| AHISHAKIYE Faustin | 2222015260  |
| SINGIZWA Simplice | 222019503 |
| ISHIMWE Barbeline |221020301|
| ISHIMWE Aline | 222020230|

**Supervisor:** *Dr. Jules NGANGO*

---

## 📄 License
This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.

---

*University of Rwanda | College of Business and Economics | AST3336 | 2025–2026*
