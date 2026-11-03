# 🌾 Maji Ndogo Agricultural Data Pipeline & Statistical Audit

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg?style=flat&logo=python&logoColor=white)](https://www.python.org/)
[![Marimo](https://img.shields.io/badge/Notebook-Marimo%20Reactive-orange.svg?style=flat)](https://marimo.io/)
[![SciPy](https://img.shields.io/badge/Statistics-SciPy%20%7C%20Inferential-teal.svg?style=flat)](https://scipy.org/)
[![SQLAlchemy](https://img.shields.io/badge/ORM-SQLAlchemy-red.svg?style=flat)](https://www.sqlalchemy.org/)
[![ALX](https://img.shields.io/badge/Program-ALX%20Data%20Science-gradient.svg?style=flat)](https://www.alxafrica.com/)

An end-to-end data science and engineering case study conducted for the **Ministry of Agriculture of Maji Ndogo**. The project transitions raw exploratory analyses under emergency government audit scrutiny into a production-grade, modular, and statistically validated agricultural data pipeline.

---

## 📌 Executive Summary & Context

The Ministry of Agriculture faced an immediate funding freeze audit requiring empirical validation of **5,654 surveyed farm records** against **independent IoT weather station readings**. 

This repository documents the three-stage engineering and analytics journey:
1. **Exploratory Data Profiling & Audit Preparation (Module 1):** Auditing multi-table relational databases, correcting data entry inversions, standardizing categorical features, and diagnosing crop yield correlations.
2. **Inferential Statistics & Hypothesis Testing (Module 2):** Applying Bessel's correction, simulating the Central Limit Theorem (CLT), constructing 95% confidence intervals, and automating one-sample hypothesis testing ($H_0$ rejection analysis).
3. **Production Pipeline Engineering & Two-Sample Distributional Validation (Module 3):** Refactoring script logic into PEP 257 documented object-oriented modules (`FieldDataProcessor`, `WeatherDataProcessor`), configuring single-source-of-truth architectures, and deploying two-sample Welch’s t-tests.

---

## 🏗️ Repository Architecture

```text
├── module_1/                               # Exploratory Data Analysis & Relational Audit
│   ├── Maji_Ndogo_farm_survey_small.db     # SQLite 4-table relational database
│   └── notebook.py                         # EDA profiling, distributions, yield drivers
│
├── module_2/                               # Inferential Statistics & Parametric Testing
│   ├── Maji_Ndogo_farm_survey_small.db     # Relational survey database
│   ├── Weather_data_field_mapping.csv      # Field-to-station topological map
│   └── notebook.py                         # CLT simulations, 95% CIs, One-sample t-tests
│
├── module_3/                               # Production Engineering & Pipeline Architecture
│   ├── data_ingestion.py                   # Reusable SQL engine & remote streaming module
│   ├── field_data_processor.py             # FieldDataProcessor pipeline class
│   ├── weather_data_processor.py           # WeatherDataProcessor IoT extraction class
│   ├── Maji_Ndogo_farm_survey_small.db     # Master agricultural database
│   ├── Weather_data_field_mapping.csv      # Geographic mapping dataset
│   ├── Weather_station_data.csv            # Raw semi-structured IoT messages
│   └── notebook.py                         # Pipeline execution & Welch's t-test validation
│
├── .gitignore                              # Git exclusion rules (caches, binaries)
├── requirements.txt                        # Dependency manifest
└── README.md                               # Project documentation & recruiter briefing
```

---

## 🔬 Core Methodologies & Engineering Highlights

### 1. Relational Database Ingestion & Anomaly Rectification (Module 1)
- **Four-Table Left Join:** Extracted complete survey features (`geographic_features`, `weather_features`, `soil_and_crop_features`, and `farm_management_features`) across 5,654 observation fields using SQL queries via SQLAlchemy.
- **Symmetric Schema Repair:** Solved an inversion flaw where `Crop_type` and `Annual_yield` column headers were swapped in the database.
- **Categorical Harmonization:** Cleaned typo variations (`cassaval` $\to$ `cassava`, `wheatn` $\to$ `wheat`, `teaa` $\to$ `tea`).
- **Feature Correlation:** Identified that `Pollution_level` is the strongest negative linear driver of standardized crop yield ($r = -0.286$).

### 2. Inferential Rigor & The Scientific Shift (Module 2)
- **Finite Sample Bias Correction:** Verified Bessel’s correction where sample variance ($n-1$, $\text{ddof}=1$) produces an unbiased estimate of population variance ($\text{ddof}=0$).
- **Central Limit Theorem Mapping:** Simulated 1,000 independent samples ($n=30$), proving that the empirical standard deviation ($0.0207$) converges tightly to the theoretical standard error ($\frac{\sigma}{\sqrt{n}} = 0.0204$).
- **Parametric Interval Estimation:** Computed 95% Student's t-distribution confidence intervals for field rainfall ($[1499.83\text{ mm}, 1545.96\text{ mm}]$ at Station 0).
- **Hypothesis Testing Divergence Discovery:** A one-sample t-test comparing Station 0 field data against published stationary averages ($1575.95\text{ mm}$) returned $p = 0.00001$ ($H_0$ rejected), triggering an investigation into whether station sensors had drifted or whether fixed single-point averages were flawed.

### 3. Object-Oriented Pipeline & Distributional Validation (Module 3)
- **Decoupled Architecture:** Replaced 30+ lines of duplicate notebook setup code with modular, PEP 257-compliant classes:
  - `data_ingestion.py`: Safe DB engine connection pooling and web CSV streaming.
  - `FieldDataProcessor`: Encapsulates SQL ingestion, column swapping, cleaning, and spatial joins.
  - `WeatherDataProcessor`: Ingests unstructured IoT text logs, utilizing regular expressions to parse continuous values for `Temperature` ($^\circ\text{C}$), `Rainfall` ($\text{mm}$), and `Pollution_level` (AQI).
- **Two-Sample Welch’s t-Test Validation:**
  - Evaluated field observations vs. real-time station distributions across all 5 stations $\times$ 3 measurements (15 independent tests).
  - Configured `equal_var=False` to handle severe sample size disparities ($n_1 = 1{,}375$ vs. $n_2 = 93$).
  - **Audit Outcome:** When comparing *empirical distributions* rather than static point-averages, all 15 tests returned $p > 0.05$ (**$H_0$ Not Rejected**), scientifically proving that the field data aligns with weather station measurements.

---

## 💡 Key Learnings & Skills Demonstrated

| Area | Demonstrated Competencies |
|---|---|
| **Software Architecture** | Refactoring notebook exploration into reusable, modular Python scripts; single source of truth configuration management (`config_params`). |
| **Statistical Inference** | Central Limit Theorem simulation, Bessel's correction, confidence interval derivation, Welch's t-tests vs. Student's t-tests. |
| **Data Engineering** | Relational SQL schema joins via SQLAlchemy, regex-based feature extraction from IoT message streams, defensive missing-value imputation. |
| **Code Quality & Auditing** | PEP 257 docstring compliance, boundary testing, type-safe operations, and reactive DAG notebook state management (Marimo). |

---

## 🚀 Getting Started

### Prerequisites
- Python 3.10 or higher installed.

### Installation
1. Clone this repository:
   ```bash
   git clone https://github.com/YonInsights/Maji-Ndogo-Farm-Survey-Audit.git
   cd Maji-Ndogo-Farm-Survey-Audit
   ```

2. Create and activate a virtual environment:
   ```bash
   python -m venv .venv
   # Windows:
   .venv\Scripts\activate
   # macOS/Linux:
   source .venv/bin/activate
   ```

3. Install project dependencies:
   ```bash
   pip install -r requirements.txt
   ```

### Running the Pipelines in Marimo
Navigate into any module directory and start the reactive notebook server:

```bash
# To run Module 1 (Exploratory Data Analysis):
cd module_1
marimo edit notebook.py

# To run Module 2 (Inferential Statistics):
cd ../module_2
marimo edit notebook.py

# To run Module 3 (Production Pipeline):
cd ../module_3
marimo edit notebook.py
```

---

## 👨‍💻 Author & Acknowledgements
- **Author:** Yonatan A. ([YonInsights](https://github.com/YonInsights))
- **Program:** ALX Africa — Data Science Career Track
- **Scenario:** Maji Ndogo Agricultural Survey Audit
