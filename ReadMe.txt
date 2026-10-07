# Software Quality Prediction

Predicting software quality from source-code metrics of open-source Java projects using Python and machine learning.

## Overview

This project loads software metric data from large open-source projects, cleans and encodes it, and prepares it for training a quality prediction model.

## Dataset

Data is organised by project, with one CSV per year:

```
Quality Prediction/
├── 1 spring-framework/   (2015-6.csv ... 2019-6.csv)
├── 2 junit-5/
├── 3 kafka-trunk/
└── 4 lucene-solr-master/
```

The notebook currently uses `1 spring-framework/2015-6.csv`:

- **Rows:** 36,928 (classes or methods)
- **Columns:** 40 (39 input features after removing the target)

| Group | Columns |
|---|---|
| Identifiers | `QualifiedName`, `Name` |
| Quality categories | `Complexity`, `Coupling`, `Size`, `Lack of Cohesion` |
| OO metrics | `CBO`, `RFC`, `SRFC`, `DIT`, `NOC`, `WMC`, `LCOM`, `LCAM`, `LTCC` |
| Size / structure | `LOC`, `CMLOC`, `NOF`, `NOSF`, `NOM`, `NOSM`, `NORM`, `#Pa`, `#MC`, `#AF` |
| Other | `ATFD`, `SI`, `#(C&I)`, `#C`, `#I`, `AC`, `EC`, `Abs`, `Ins`, `ND`, `MCC`, `NBD` |

## Requirements

- Python 3.8+
- JupyterLab or Jupyter Notebook
- pandas, numpy, matplotlib, seaborn, scikit-learn

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyterlab
```

## How to Run

1. Clone the repository:
```bash
   git clone https://github.com/<your-username>/<your-repo>.git
   cd <your-repo>
```
2. Start Jupyter:
```bash
   jupyter lab
```
3. Open the notebook and run the cells from top to bottom (**Shift + Enter**).

The notebook reads the data with a relative path, so keep it in the same folder as the dataset folders:

```python
dataset = pd.read_csv('1 spring-framework/2015-6.csv')
```

Expected output of the preprocessing cell:

```
(36928, 39) (36928,)
```

## What the Code Does

1. **Load data** into a pandas DataFrame.
2. **Explore** with `plotPerColumnDistribution()`, which plots columns that have 2 to 49 unique values (commented out by default).
3. **Handle missing values**: numeric gaps filled with `0`, text gaps with an empty string.
4. **Encode** `QualifiedName` and `Name` with `LabelEncoder`.
5. **Prepare** features `X` (all columns except `Complexity`) and target `Y` (`Complexity`).

## Known Issues and Recommendations

- `QualifiedName` and `Name` are unique identifiers and add noise, so consider dropping them from `X`.
- `Complexity` may be derived from other metrics such as `WMC` or `MCC`. Check correlations to avoid target leakage.
- Columns like `LOC`, `LOC.1`, `LOC.2` may be redundant duplicates.
- Decide between classification and regression by inspecting `Complexity`.

## Next Steps

- [ ] Train/test split
- [ ] Feature scaling
- [ ] Train models (Random Forest, Decision Tree, Logistic Regression, SVM)
- [ ] Evaluate (accuracy, F1, confusion matrix, or MAE/RMSE/R²)
- [ ] Repeat for other years and projects
- [ ] Compare results across projects and time

