# Boston Housing Analysis

Exploratory data analysis and statistical hypothesis testing on the classic Boston Housing dataset, done in a Python notebook (JupyterLite / Pyodide).

## Overview

The project looks at what is associated with the median value of owner-occupied homes (`MEDV`) in Boston suburbs. It covers data inspection, cleaning checks, visualization, and four hypothesis tests, followed by a simple linear regression.

## Dataset

- **Source:** [boston_housing.csv](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-ST0151EN-SkillsNetwork/labs/boston_housing.csv) (IBM Developer Skills Network)
- **Size:** 506 rows, 14 columns (plus an `AGE_Group` column)
- **Missing values:** none
- **Duplicates:** none

| Column | Description |
|--------|-------------|
| `CRIM` | Per capita crime rate by town |
| `ZN` | Proportion of residential land zoned for lots over 25,000 sq. ft. |
| `INDUS` | Proportion of non-retail business acres per town |
| `CHAS` | Charles River dummy variable (1 = tract bounds the river, 0 = otherwise) |
| `NOX` | Nitric oxides concentration (parts per 10 million) |
| `RM` | Average number of rooms per dwelling |
| `AGE` | Proportion of owner-occupied units built before 1940 |
| `DIS` | Weighted distance to five Boston employment centres |
| `RAD` | Index of accessibility to radial highways |
| `TAX` | Property-tax rate per $10,000 |
| `PTRATIO` | Pupil-teacher ratio by town |
| `LSTAT` | Percentage of lower-status population |
| `MEDV` | Median value of owner-occupied homes in $1000s (target) |
| `AGE_Group` | Derived category: Young (0–35), Middle-aged (35–70), Old (70–100) |

## Tools and Libraries

- Python 3 (JupyterLite with `piplite`)
- `pandas`, `numpy`
- `matplotlib`, `seaborn`
- `scipy.stats`
- `statsmodels`

## Analysis Steps

1. **Data loading:** the CSV is fetched with `js.fetch` and read into a DataFrame.
2. **Data inspection:** `head()`, `info()`, missing-value and duplicate checks.
3. **Descriptive statistics:** `describe()` and a correlation heatmap.
4. **Visualizations:**
   - Boxplot of `MEDV`
   - Bar chart of `CHAS` counts
   - Boxplot of `MEDV` by `AGE_Group`
   - Scatter plot of `NOX` vs `INDUS`
   - Histogram of `PTRATIO`
5. **Hypothesis tests** (significance level α = 0.05).
6. **Linear regression:** `MEDV ~ DIS` using OLS.

## Results

| Test | Method | Result |
|------|--------|--------|
| Equal variance of `MEDV` for river vs. non-river tracts | Levene's test | Variances differ significantly |
| Difference in mean `MEDV` for river vs. non-river tracts | Welch's t-test | Significant difference |
| Difference in `MEDV` across age groups | One-way ANOVA | See note below |
| Correlation between `NOX` and `INDUS` | Pearson | Significant, r = 0.76 |

**Regression (`MEDV` ~ `DIS`):**

- Intercept: 18.39
- `DIS` coefficient: 1.09 (p < 0.001), so each additional unit of distance to employment centres is associated with about $1,090 higher median home value
- R² = 0.062, so distance to employment centres alone explains only about 6% of the variation in `MEDV`
- Residuals are right-skewed (skew 1.47) and show autocorrelation (Durbin-Watson 0.57), so the model assumptions are not fully met

## Known Issues

- **ANOVA by age group:** the test filters on the labels `'Group 1'`, `'Group 2'` and `'Group 3'`, but the actual labels are `'Young'`, `'Middle-aged'` and `'Old'`. The groups come back empty (hence the `DegenerateDataWarning`), so the reported "no significant difference" is **not valid**. Changing the filters to the real labels fixes it.
- The `Unnamed: 0` column is a leftover index and can be dropped.
- `pandas` is imported twice in the setup cell.

## How to Run

1. Open the notebook in JupyterLite or any Jupyter environment.
2. Run the install cell (skip if the packages are already available):
   ```python
   import piplite
   await piplite.install(['numpy', 'pandas'])
   await piplite.install(['seaborn'])
   ```
3. Run the remaining cells in order. Note that data loading via `js.fetch` only works in a browser-based (Pyodide) environment. Elsewhere, read the CSV directly with `pd.read_csv(URL)`.

## Author

**Abdujabborov Abdurasul**

I hope you liked this :)
