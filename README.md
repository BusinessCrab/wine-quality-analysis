# Wine Quality Prediction

A reproducible, notebook-first analysis of how physicochemical measurements predict an observed wine quality score. The analysis lives in [`wine_quality_prediction.ipynb`](wine_quality_prediction.ipynb). It uses pandas for inspection, matplotlib/seaborn for charts, and scikit-learn for regression and classification.

## Analysis guide

The notebook inspects schema, missing values, exact duplicates, and quality counts before modeling. It then:

- charts quality counts, feature distributions, relationships with quality, and a correlation matrix;
- fits a random forest regressor and reports MAE, RMSE, R², and rounded-score comparisons;
- fits a random forest classifier and reports accuracy, macro-F1, weighted-F1, and a confusion matrix;
- compares an 11-neighbor classifier before and after standardization on the same split;
- calculates one-vs-rest permutation importance for every observed quality class.

All models use the same reproducible, group-aware train/test split. Exact duplicate rows remain in the data, while identical input rows are assigned to the same partition. Missing feature values are imputed inside pipelines fitted only on training data. The notebook calculates findings at run time and avoids treating predictive importance as causality.

## Repository structure

```text
wine_quality_prediction.ipynb   # Complete analysis, at the repository root
data/                           # Downloaded input CSVs (ignored by Git)
plots/                          # Figures exported when the notebook runs
requirements.txt                # Runtime Python dependencies
README.md                       # Setup and analysis guide
```

## Data setup

1. Download the primary [Kaggle Red Wine Quality dataset](https://www.kaggle.com/datasets/uciml/red-wine-quality-cortez-et-al-2009). Extract `winequality-red.csv` into `data/`.
2. Optionally, download the companion `winequality-white.csv` from the [UCI Wine Quality dataset](https://archive.ics.uci.edu/dataset/186/wine+quality) and place it in `data/` too.
3. Keep the filenames above. The notebook also accepts `winequality_red.csv` and `winequality_white.csv`. Comma-, semicolon-, and tab-separated CSVs are supported.

With both files, the notebook concatenates red and white wines and adds `wine_type`. With only one, it analyzes that variant and names it in the output. A missing CSV produces an error with the expected location. The notebook does not download data or request credentials. `.gitignore` keeps downloaded CSVs out of version control; `data/.gitkeep` preserves the directory.

## Install and run

Use Python 3.10 or newer. From the repository root:

```bash
python -m venv .venv
```

Activate the environment with `.\.venv\Scripts\Activate.ps1` in PowerShell or `source .venv/bin/activate` on macOS/Linux, then run:

```bash
python -m pip install -r requirements.txt
python -m notebook wine_quality_prediction.ipynb
```

Select the environment's Python kernel in Jupyter and choose **Run All**. Start Jupyter from the repository root because paths resolve from the current working directory. Each run saves labeled PNG figures to `plots/`. The notebook's source contains no fabricated outputs or fixed dataset metrics. The supplied figures were generated from the Kaggle red-wine CSV; adding white wine and rerunning replaces them with combined-data figures.

## Interpretation

Quality scores are ordered but imbalanced, so read macro-F1 and individual confusion-matrix rows alongside overall accuracy. Permutation importance is computed on the held-out set for each quality class; it can vary for rare classes or correlated features. These measurements describe predictive associations in the supplied data.
