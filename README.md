# Channel Strength, Field-Force Behavior, and Customer Readiness

Code for the paper **"Channel Strength, Field-Force Behavior, and Customer Readiness: An Explainable Machine Learning Analysis of Sales Conversion Performance in an Emerging Market"** (manuscript under review).

The study links a survey of 222 field sales executives to system-recorded conversion performance for 194 sales territories of a building-materials firm in Bangladesh. It compares penalised logistic regression, random forest and XGBoost under area-grouped cross-validation, explains the models with SHAP values and accumulated local effects, and checks every hypothesis against cluster-robust regressions.

## Repository structure

```
src/
  run_analysis.py    Main analysis: models, validation, block importance, SHAP, regressions, robustness, archetypes
  extra_checks.py    System vs self-reported predictors, random-split validation, within-territory agreement
  shape_tests.py     Curvature of SHAP dependence (slope below vs above the median, bootstrap intervals)
  make_figures.py    Figures 2-7
  fig_framework.py   Figure 1 (conceptual framework)
results/             Summary results used in the paper (no individual-level data)
figures/             Figures as submitted (600 dpi)
data/                Empty: see data/README.md
```

## Data

The survey and performance data are proprietary to the partner firm and are **not** included. They contain information that could identify employees and the firm. See [data/README.md](data/README.md) for the files and variables the code expects. The data are available from the author on reasonable request, subject to the firm's approval.

## How to run

Python 3.11 or later.

```bash
pip install -r requirements.txt
```

Put the three data files in `data/`, then run from the repository root:

```bash
python src/run_analysis.py data results
python src/extra_checks.py data results
python src/shape_tests.py data results
python src/make_figures.py data results figures
python src/fig_framework.py figures/Figure1_framework.png
```

`run_analysis.py` takes about 30–40 minutes on a laptop (10 repetitions of five-fold cross-validation grouped by sales area, with nested tuning). Set the environment variable `NREP=1` for a quick test run.

## Main settings

| Setting | Value |
|---|---|
| Unit of analysis | Sales territory (n = 194) |
| Primary outcome | Conversion achievement ≥ 91% of target (firm's top band); continuous outcome ln(1 + achievement %) |
| Primary feature set | 27 predictors: channel strength (8), field-force behaviour (9), customer readiness (10) |
| Validation | Stratified five-fold cross-validation grouped by sales area (34 areas), repeated 10 times; tuning in an inner grouped three-fold loop |
| Preprocessing | Median imputation and scaling fitted inside training folds only |
| Excluded from primary models | Conversion target (outcome denominator), carpenter-target achievement, leave-one-out zone averages |
| Explanation | TreeSHAP (out-of-fold), SHAP interaction values, accumulated local effects, block permutation importance |
| Econometric checks | OLS and logit with zone fixed effects and area-clustered standard errors; ordered logit; Gaussian copula |

## Licence

MIT. See [LICENSE](LICENSE).

## Author

Md Habibul Islam, United International University, Dhaka, Bangladesh. ORCID: [0009-0005-7333-2254](https://orcid.org/0009-0005-7333-2254)
