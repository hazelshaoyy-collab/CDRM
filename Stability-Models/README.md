# Stability Stage-A Public Module

This repository provides a lightweight public module for the **Stage-A stability classification model** used in our manuscript on dual-metal 2D-COF electrocatalysts for CO2 reduction.

The goal of this repository is **lightweight transparency and reviewer-facing reproducibility**, rather than full reproduction of the complete DFT workflow, full feature-selection pipeline, or later active-learning retraining rounds.

## Scope of this public module

This module contains the minimum materials required to demonstrate that the published Stage-A stability workflow is real, executable, and internally consistent:

- the full **679-sample stability dataset** used for the Stage-A model after feature selection
- the fixed **Dev/Lockbox split** used for model development and held-out evaluation
- the frozen tuned model `LGBM_TUNED_ROC.joblib`
- the final **30 selected features** used by the public module
- a lightweight notebook for
  - coarse benchmarking on **Dev only**
  - OOF evaluation of the frozen tuned model on **Dev only**
  - SHAP feature-importance analysis on **Dev only**
  - final held-out evaluation on **Lockbox only**
- a GitHub-friendly showcase notebook with embedded key figures for direct preview

## What is intentionally not included

This repository does **not** include:

- the complete raw DFT workflow
- the full original Stage-A research notebook with all internal trial cells
- the hyperparameter-search process itself
- active-learning retraining rounds
- full external-validation workflows
- full intermediate artifacts from every internal experiment

These components can be provided separately if requested during editorial review.

## Repository structure

```text
stability-public-module/
├─ README.md
├─ requirements.txt
├─ extract_stability_public_files.py
├─ ML-Stability-StageA-GitHubShowcase.ipynb
├─ ML-Stability-StageA-PublicModule-v2.ipynb
├─ data/
│  ├─ stability_stageA_679_best30.csv
│  └─ stability_stageA_split.csv
└─ models/
   ├─ LGBM_TUNED_ROC.joblib
   ├─ stability_features.json
   └─ stability_tuned_meta.json
```

## File descriptions

### `data/stability_stageA_679_best30.csv`
Public Stage-A stability table containing all 679 samples used in this module.

Recommended columns:
- `MOFID` (or another stable sample identifier)
- `Label_Stab`
- `sample_weight` (optional but recommended)
- the final 30 selected features

### `data/stability_stageA_split.csv`
Fixed split definition used by the public module.

Required columns:
- `MOFID`
- `subset` (`Dev` or `Lockbox`)

Recommended additional column:
- `group_key`

### `models/LGBM_TUNED_ROC.joblib`
Frozen tuned LightGBM-based stability classifier used for Stage-A SHAP analysis and public-model demonstration.

### `models/stability_features.json`
Ordered list of the final 30 selected feature names.

### `models/stability_tuned_meta.json`
Compact metadata file containing the classification threshold and selected tuned-model metadata.

### `extract_stability_public_files.py`
Utility script that extracts the public-facing files from the original internal Stage-A project directory.

### `ML-Stability-StageA-PublicModule-v2.ipynb`
Runnable public notebook for:
- coarse benchmarking on Dev
- tuned-model OOF evaluation on Dev
- SHAP analysis on Dev
- Lockbox evaluation

### `ML-Stability-StageA-GitHubShowcase.ipynb`
Display-oriented notebook intended for GitHub preview. It emphasizes readability and figure presentation.

## Environment setup

Create and activate a clean Python environment, then install the required packages:

```bash
python -m pip install -r requirements.txt
```

Python 3.10 or 3.11 is recommended.

## How to generate the public files

If you already have the original internal Stage-A project directory, place `extract_stability_public_files.py` in the project root and run:

### In a terminal

```bash
python extract_stability_public_files.py
```

### In a Jupyter notebook cell

Use **one** of the following:

```python
!python extract_stability_public_files.py
```

or

```python
%run extract_stability_public_files.py
```

Do **not** type `python extract_stability_public_files.py` directly in a notebook code cell without `!` or `%run`, because that will be interpreted as Python syntax and will raise a `SyntaxError`.

If needed, first change the working directory:

```python
%cd /path/to/your/project/root
```

After successful execution, the script will generate a public export folder containing the required data and model metadata files.

## How to run the public notebook

Open:

- `ML-Stability-StageA-PublicModule-v2.ipynb`

and execute the notebook from top to bottom.

The notebook is organized into five parts:

1. Load the public 679-sample table and the fixed split file
2. Run coarse benchmarking on **Dev only**
3. Load `LGBM_TUNED_ROC.joblib` and report OOF metrics on **Dev only**
4. Compute SHAP feature-importance and dependence plots on **Dev only**
5. Report final held-out performance on **Lockbox only**

## Reproducibility rules used in this public module

To remain consistent with the manuscript workflow:

- the full 679-sample table is public
- **Dev** is used for model development and public benchmarking
- **Lockbox** is reserved for final held-out evaluation only
- the tuned model is provided directly
- the hyperparameter-search process itself is not reproduced in this public package

## Notes for reviewers and readers

This repository is intended as a concise transparency package for the published Stage-A stability model.
It is not designed as a full software distribution.

If additional intermediate artifacts or extended validation materials are required during editorial review, they can be supplied separately.
