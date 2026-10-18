# FN6809 Linear Financial Models — Group 10

## Project Title

**Exploring the Drivers of U.S. Domestic Airline Fares**  
*An Exploratory Multiple Regression Analysis*

This repository contains the data-processing code, statistical analysis, generated outputs, and LaTeX report for the FN6809 Linear Financial Models group assignment at Nanyang Technological University.

The project investigates which observable itinerary, carrier, purchase, and geographic characteristics are most useful in explaining variation in U.S. domestic airline ticket fares.

---

## Group Members

- Fu Yuxuan
- Koh Chen Yang
- Ma Qimin
- Verma Aditya Aryan
- Yin Qiuhao
- Zhu Jingyu

**Course:** FN6809 Linear Financial Models  
**Lecturer:** Assoc Prof Wu Yuan  
**Submission Date:** 31 October 2026

---

## Project Overview

The analysis uses the June 2026 U.S. Bureau of Transportation Statistics DB1C Ticket dataset.

A reproducible random sample of 100,000 ticket records is drawn from the raw dataset. After data-quality screening, 90,086 regular positive-fare observations are retained for analysis.

The modelling workflow is:

1. Data preparation and variable screening
2. Exploratory data analysis
3. Initial broad multiple regression
4. Variable reduction and candidate-model comparison
5. Regression diagnostics
6. Model refinement and remedial measures
7. Out-of-sample validation and final model selection
8. Economic interpretation and fare benchmarking

The final model uses a log-transformed fare response and empirical weighted least squares, with White heteroskedasticity-robust OLS used as a robustness benchmark.

---

## Repository Structure

```text
FN6809-Linear-Financial-Models/
│
├── data/
│   └── Processed datasets used by the analysis
│
├── notebooks/
│   ├── 01_data_preparation.ipynb
│   ├── 02_exploratory_analysis.ipynb
│   ├── 03_initial_broad_model.ipynb
│   ├── 04_variable_reduction.ipynb
│   ├── 05_model_diagnostics.ipynb
│   ├── 06_model_refinement.ipynb
│   ├── 07_model_validation.ipynb
│   └── 08_business_application.ipynb
│
├── outputs/
│   └── Tables, figures, and intermediate model outputs
│
├── report/
│   ├── FN6809_Group 10.tex
│   ├── FN6809_Group 10.pdf
│   └── nbs_logo.jpg
│
├── requirements.txt
├── .gitignore
└── README.md
```

---

## Python Environment

The analysis is implemented in Python.

To reproduce the project, create a virtual environment and install the packages listed in `requirements.txt`.

### Using Conda

```bash
conda create -n fn6809 python=3.11
conda activate fn6809
pip install -r requirements.txt
```

### Using Python venv

Windows:

```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

macOS/Linux:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

If the notebooks are opened in VS Code, select the newly created Python environment as the notebook kernel.

---

## Notebook Workflow

The notebooks are intended to be run sequentially because later stages use datasets, modelling decisions, and outputs produced in earlier stages.

### Notebook 01 — Data Preparation

`01_data_preparation.ipynb`

- Reads and samples the raw DB1C Ticket dataset
- Performs fare-quality screening
- Screens variables for redundancy and suitability
- Constructs the prepared analysis dataset

### Notebook 02 — Exploratory Data Analysis

`02_exploratory_analysis.ipynb`

- Examines the fare distribution
- Analyses distance and itinerary complexity
- Compares fares across carriers, purchase groups, and geography
- Produces descriptive tables and figures

### Notebook 03 — Initial Broad Regression Model

`03_initial_broad_model.ipynb`

- Creates the 50/50 model-building and validation split
- Groups low-frequency categorical levels
- Estimates the initial eight-block OLS model
- Examines coefficient patterns
- Assesses multicollinearity

### Notebook 04 — Variable Reduction

`04_variable_reduction.ipynb`

- Performs backward-selection partial F-tests
- Estimates nearby six-, seven-, and eight-block specifications
- Compares candidate models using:
  - R²
  - adjusted R²
  - Mallows' Cp
  - AIC
  - BIC
  - PRESS

### Notebook 05 — Model Diagnostics

`05_model_diagnostics.ipynb`

- Examines residual behaviour
- Tests for heteroskedasticity
- Assesses response outliers
- Examines leverage
- Calculates DFFITS
- Calculates Cook's distance
- Examines DFBETAS

### Notebook 06 — Model Refinement

`06_model_refinement.ipynb`

- Applies a logarithmic transformation to ticket fare
- Estimates empirical weighted least squares
- Develops and refines the variance function
- Reassesses residual behaviour
- Compares WLS results with White heteroskedasticity-robust OLS

### Notebook 07 — Model Validation

`07_model_validation.ipynb`

- Evaluates out-of-sample prediction performance
- Compares model-building and validation fit
- Examines coefficient stability
- Compares the final candidate specifications
- Selects the final model

### Notebook 08 — Economic Interpretation and Fare Benchmarking

`08_business_application.ipynb`

- Re-estimates the selected specification using the full sample
- Converts log-model coefficients into percentage fare differences
- Interprets itinerary, carrier, purchase, and geographic effects
- Constructs model-implied fare benchmarks
- Produces the final benchmarking tables

---

## Data

The raw dataset is obtained from:

**U.S. Department of Transportation, Bureau of Transportation Statistics — Airline Origin and Destination Survey (DB1C).**

The analysis uses the **June 2026 DB1C Ticket dataset**.

The raw source file contains approximately 13.8 million observations and is approximately 2.5 GB in size. It is therefore **not included in this GitHub repository**.

Only processed datasets required by the subsequent analysis should be stored in the repository.

The raw DB1C file should be placed locally in the location expected by `01_data_preparation.ipynb` before reproducing the complete workflow from the beginning.

---

## Reproducing the Analysis

After installing the Python dependencies, run the notebooks in the following order:

```text
01_data_preparation.ipynb
02_exploratory_analysis.ipynb
03_initial_broad_model.ipynb
04_variable_reduction.ipynb
05_model_diagnostics.ipynb
06_model_refinement.ipynb
07_model_validation.ipynb
08_business_application.ipynb
```

The notebooks write processed data and generated figures/tables to the corresponding project folders.

The model-building and validation workflow should be preserved when reproducing or modifying the analysis. In particular, the validation sample should not be used to make model-specification or refinement decisions.

---

## Report

The main report is written in LaTeX.

Source:

```text
report/FN6809_Group 10.tex
```

Latest compiled report:

```text
report/FN6809_Group 10.pdf
```

Figures and tables referenced by the report are generated from the analysis notebooks and stored under `outputs/`.

When modifying the report, compile the `.tex` file from the `report/` directory so that the relative paths to figures in `../outputs/` remain valid.

---

## Collaboration Workflow

### 1. Get the latest version

Before starting work:

```bash
git checkout main
git pull
```

### 2. Create a working branch

Create a separate branch for your changes:

```bash
git checkout -b your-name-edits
```

For example:

```bash
git checkout -b qimin-report-edits
```

### 3. Make and review your changes

Check which files have changed:

```bash
git status
```

### 4. Commit the changes

```bash
git add .
git commit -m "Describe your changes"
```

For example:

```bash
git commit -m "Revise model diagnostics section"
```

### 5. Push the branch

```bash
git push -u origin your-name-edits
```

### 6. Merge through GitHub

Open a pull request on GitHub and merge the branch into `main` after reviewing the changes.

To minimise merge conflicts, avoid having multiple group members edit the same part of `FN6809_Group 10.tex` simultaneously.

---

## Important Collaboration Notes

- Pull the latest `main` branch before beginning new work.
- Use separate branches for substantial edits.
- Do not commit the full raw DB1C dataset.
- Do not commit temporary LaTeX compilation files.
- Do not commit Python virtual environments or Conda environments.
- Do not commit Jupyter checkpoint folders.
- Keep generated figures and tables that are required by the report in `outputs/`.
- Keep the latest compiled report PDF so that group members can review the current document without compiling LaTeX themselves.
- Run the notebooks in numerical order when reproducing the full analysis.
- Do not use the held-out validation sample to make model-specification or refinement decisions.

---

## References

Kutner, M. H., Nachtsheim, C. J., Neter, J., & Li, W. (2005). *Applied Linear Statistical Models* (5th ed.). McGraw-Hill/Irwin.

U.S. Department of Transportation, Bureau of Transportation Statistics. (2026). *Airline Origin and Destination Survey (DB1C): Ticket Data*. Bureau of Transportation Statistics.