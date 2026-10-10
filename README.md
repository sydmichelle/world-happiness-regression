# world-happiness-regression
Regression in Python

A data analysis project that combines three years of World Happiness Report data and uses multiple linear regression to find which country characteristics best predict a country's happiness score.

**Tools:** Python, pandas, NumPy, scikit-learn, matplotlib, seaborn, Jupyter

## Question

Which measurable factors (economic output, health, family support, freedom, trust in government, generosity) best predict how happy a country's residents report being?

## Data

World Happiness Report country-level data for **2015, 2016 and 2017** (about 470 country-year rows in total). Each row has a happiness score and six explanatory variables. See [`data/README.md`](data/README.md) for how to download the files.

## My role

This began as a team project. I defined the research question, sourced and merged the multi-year datasets, led the data cleaning and visualization, and built the original regression model. This repository is a cleaned-up version of that work.

## Method

1. **Merge:** stack the three yearly files and standardize the column names (the 2017 file is formatted differently).
2. **Clean:** keep the columns shared by all years and drop the ones that exist only in some years (standard error, confidence intervals, whiskers, dystopia residual). Check for missing values and duplicates.
3. **Explore:** histograms, boxplots and a correlation heatmap.
4. **Transform:** square-root transform on the right-skewed Trust and Generosity variables.
5. **Split and scale:** 80/20 train/test split, then center and scale the predictors using the **training set only**, to avoid data leakage.
6. **Check assumptions:** linearity, multicollinearity (VIF), and residual plots.
7. **Model:** fit four linear regression models and compare them on the held-out test set with RÂ² and RMSE.

## Results

All four models were evaluated on the same 20% test set (`random_state=1`).

| Model | Predictors | Test R² | Test RMSE |
|---|---|---|---|
| 1. All predictors | Economy, Family, Health, Freedom, Trust, Generosity | 0.774 | 0.578 |
| **2. Main model** | **Economy, Health, Family, Freedom** | **0.756** | **0.601** |
| 3. Without Freedom | Economy, Family, Health, Trust, Generosity | 0.754 | 0.603 |
| 4. Without Economy | Family, Health, Freedom, Trust, Generosity | 0.769 | 0.584 |

**Main model:** the four-predictor model explains about **76% of the variance** in happiness scores on unseen data, with a typical error of about **0.6 points** (scores range from roughly 2.7 to 7.6).

**Why Model 2 is the main model:** adding Trust and Generosity (Model 1) raises RÂ² by less than 0.02 and their coefficients are small, so the simpler four-predictor model was preferred.

**Findings:**
- Economic output (GDP per capita) had the largest standardized coefficient, followed by Freedom, Health and Family.
- Generosity added almost nothing to the model.
- Economy and Health are strongly correlated (about 0.79), so their individual coefficients should be read with care.

> Run the notebook to reproduce these numbers. The table is from my original run. Centering and scaling fitted on the training set does not change a linear model's test predictions, so R² and RMSE should match, but standardized coefficients may differ slightly from the original.

## Limitations

- **Repeated countries.** Each country appears in up to three years, so the rows are not independent, and the same country can appear in both the training and test sets. Test performance may be somewhat optimistic compared with predicting for entirely new countries. A next step would be a split grouped by country.
- **Association, not causation.** This is observational data. The model shows which factors move together with happiness, not what causes it.
- **Overlapping predictors.** Multicollinearity (checked with VIF) makes individual coefficients less stable.
- **Survey-based target.** Happiness scores come from survey responses and carry measurement error and cultural differences in how people answer.

## How to run

```bash
git clone <this-repo-url>
cd world-happiness-regression
pip install -r requirements.txt
# download 2015.csv, 2016.csv, 2017.csv into the data/ folder (see data/README.md)
jupyter notebook World_Happiness_Analysis.ipynb
```

## Repository contents

```
World_Happiness_Analysis.ipynb   main analysis
requirements.txt                 Python dependencies
data/README.md                   where to get the data
```

## Contact

Sydney Smith
sydneysmith0121@gmail.com
