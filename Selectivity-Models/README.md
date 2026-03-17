# Selectivity Stage-A Public Module

This directory provides a lightweight public module for the **Stage-A selectivity classification model** used in our manuscript on dual-metal 2D-COF electrocatalysts for selective CO2 reduction.

The purpose of this module is **reviewer-facing transparency and lightweight reproducibility**, rather than full reproduction of the original DFT workflow, the entire internal feature-selection workflow, or the later active-learning retraining rounds.

## Scope of this public module

This package contains the minimum materials required to show that the published Stage-A selectivity workflow is real, executable, and internally consistent:

- the full **200-sample selectivity table** used by the public module after feature selection
- the fixed **Dev / Lockbox** split used for model development and held-out evaluation
- the frozen tuned model `RandomForest_TUNED_ROC.joblib`
- the final **27 selected features**
- a runnable public notebook for:
  - coarse benchmarking on **Dev only**
  - OOF validation of the frozen tuned model on **Dev only**
  - SHAP analysis on **Dev only**
  - final held-out evaluation on **Lockbox only**
- a GitHub showcase notebook with embedded figures for direct preview

## What is intentionally not included

This public module does **not** include:

- the complete raw DFT workflow
- the full internal research notebook with all intermediate trial cells
- the hyperparameter-search process itself
- later active-learning retraining rounds
- full external-validation workflows
- every internal artifact generated during development

These can be supplied separately if needed during editorial review.

## Repository structure

```text
selectivity-public-module/
├─ README.md
├─ requirements.txt
├─ extract_selectivity_public_files.py
├─ ML-selectivity-StageA-GitHubShowcase.ipynb
├─ ML-selectivity-StageA-PublicModule-v2.ipynb
├─ data/
│  ├─ selectivity_stageA_200_best27.csv
│  └─ selectivity_stageA_split.csv
└─ models/
   ├─ RandomForest_TUNED_ROC.joblib
   ├─ selectivity_features.json
   └─ selectivity_tuned_meta.json
```

## File descriptions

### `data/selectivity_stageA_200_best27.csv`
Public Stage-A selectivity table containing all 200 samples used in this public module.

Required columns:
- `MOFID`
- `label`
- the final 27 selected features

Optional column:
- `sample_weight`

### `data/selectivity_stageA_split.csv`
Fixed split definition used by the public module.

Required columns:
- `MOFID`
- `subset` (`Dev` or `Lockbox`)

Recommended additional column:
- `group_key`

### `models/RandomForest_TUNED_ROC.joblib`
Frozen tuned RandomForest-based selectivity classifier used for Stage-A SHAP analysis and public-model demonstration.

### `models/selectivity_features.json`
Ordered list of the final 27 selected feature names.

### `models/selectivity_tuned_meta.json`
Compact metadata file containing the public threshold and selected tuned-model metadata.

### `extract_selectivity_public_files.py`
Utility script that extracts the public-facing files from the original internal Stage-A project directory.

### `ML-selectivity-StageA-PublicModule-v2.ipynb`
Runnable public notebook for:
- coarse benchmarking on Dev
- tuned-model OOF evaluation on Dev
- SHAP analysis on Dev
- Lockbox evaluation

### `ML-selectivity-StageA-GitHubShowcase.ipynb`
Display-oriented notebook intended for GitHub preview. It emphasizes readability and figure presentation.

## Environment setup

Create and activate a clean Python environment, then install the required packages:

```bash
python -m pip install -r requirements.txt
```

Python 3.10 or 3.11 is recommended.

## How to generate the public files

If you already have the original internal Stage-A selectivity project directory, place `extract_selectivity_public_files.py` in the project root and run:

### In a terminal

```bash
python extract_selectivity_public_files.py
```

### In a Jupyter notebook cell

Use **one** of the following:

```python
!python extract_selectivity_public_files.py
```

or

```python
%run extract_selectivity_public_files.py
```

Do **not** type `python extract_selectivity_public_files.py` directly in a notebook code cell without `!` or `%run`, because that will be interpreted as Python syntax and will raise a `SyntaxError`.

If needed, first change the working directory:

```python
%cd /path/to/your/project/root
```

After successful execution, the script will generate a public export folder containing the required data and metadata files.

## How to run the public notebook

Open:

- `ML-selectivity-StageA-PublicModule-v2.ipynb`

and execute it from top to bottom.

The notebook is organized into five parts:

1. Load the full 200-sample public table and the fixed split file
2. Run coarse benchmarking on **Dev only**
3. Load `RandomForest_TUNED_ROC.joblib` and report OOF metrics on **Dev only**
4. Compute SHAP feature-importance and dependence plots on **Dev only**
5. Report final held-out performance on **Lockbox only**

## Reproducibility rules used in this public module

To remain consistent with the manuscript workflow:

- the full 200-sample table is public
- **Dev** is used for model development and public benchmarking
- **Lockbox** is reserved for final held-out evaluation only
- the tuned model is provided directly
- the hyperparameter-search process itself is not reproduced in this public package

## Notes

The correct public selectivity filenames should reflect the actual dataset size and feature count:

- `selectivity_stageA_200_best27.csv`
- not `selectivity_stageA_578_best24.csv`

This package is intended as a concise transparency module for the published Stage-A selectivity model rather than a full software distribution.