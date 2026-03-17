# Activity Stage-A Public Module

This folder provides a lightweight public module for the **Stage-A activity regression model** used in our manuscript on dual-metal 2D-COF electrocatalysts for CO2 reduction.

The goal is **reviewer-facing transparency and lightweight reproducibility**, rather than full reproduction of the complete DFT workflow, the full feature-selection development history, or the later active-learning retraining rounds.

## Scope of this public module

This module contains the minimum materials required to demonstrate that the published Stage-A activity workflow is real, executable, and internally consistent:

- the full **578-sample activity dataset** used for the Stage-A activity model after feature selection
- the fixed **Dev/Lockbox split** used for model development and held-out evaluation
- the frozen tuned model `RandomForest_StageA_tuned_bestk24.joblib`
- the final **24 selected features** used by the public module
- a lightweight notebook for
  - coarse benchmarking on **Dev only**
  - OOF evaluation of the frozen tuned model on **Dev only**
  - SHAP feature-importance analysis on **Dev only**
  - final held-out evaluation on **Lockbox only**
- a GitHub-friendly showcase notebook with embedded key figures for direct preview

## What is intentionally not included

This module does **not** include:

- the complete raw DFT workflow
- the full original Stage-A research notebook with all internal trial cells
- the Optuna hyperparameter-search process itself
- active-learning retraining rounds
- full external-validation workflows
- all intermediate artifacts from every internal experiment

These components can be provided separately if requested during editorial review.

## Recommended repository layout

```text
activity-public-module/
├─ README.md
├─ requirements.txt
├─ extract_activity_public_files.py
├─ ML-activity-StageA-GitHubShowcase.ipynb
├─ ML-activity-StageA-PublicModule-v2.ipynb
├─ data/
│  ├─ activity_stageA_578_best24.csv
│  └─ activity_stageA_split.csv
└─ models/
   ├─ RandomForest_StageA_tuned_bestk24.joblib
   ├─ activity_features.json
   └─ activity_tuned_meta.json
```

## Required files

### `data/activity_stageA_578_best24.csv`
Public Stage-A activity table containing all 578 samples used in this module.

Required columns:
- `MOFID`
- `targets`
- the final 24 selected features

Optional columns:
- descriptor source columns retained from the original table
- any reviewer-facing annotation columns

### `data/activity_stageA_split.csv`
Fixed split definition used by the public module.

Required columns:
- `MOFID`
- `subset` (`Dev` or `Lockbox`)

Recommended additional columns:
- `group_key`

### `models/RandomForest_StageA_tuned_bestk24.joblib`
Frozen tuned random-forest regressor used for the Stage-A SHAP analysis and public-model demonstration.

### `models/activity_features.json`
Ordered list of the final 24 selected feature names.

### `models/activity_tuned_meta.json`
Compact metadata file containing the tuned-model metadata, post-tuning Dev metrics, and the random-forest best parameters.

## Environment setup

A single shared root-level `requirements.txt` is recommended for all three public modules.
If you prefer each module to be self-contained, keep a copy of the same `requirements.txt` inside each module folder.

Install dependencies with:

```bash
python -m pip install -r requirements.txt
```

Python 3.10 or 3.11 is recommended.

## How to generate the public files

If you already have the original internal Stage-A project directory, place `extract_activity_public_files.py` in the project root and run:

### In a terminal

```bash
python extract_activity_public_files.py
```

### In a Jupyter notebook cell

Use **one** of the following:

```python
!python extract_activity_public_files.py
```

or

```python
%run extract_activity_public_files.py
```

Do **not** type `python extract_activity_public_files.py` directly in a notebook code cell without `!` or `%run`, because that will be interpreted as Python syntax and will raise a `SyntaxError`.

## How to run the public notebook

Open:

- `ML-activity-StageA-PublicModule-v2.ipynb`

and execute the notebook from top to bottom.

The notebook is organized into five parts:

1. Load the public 578-sample table and the fixed split file
2. Run coarse benchmarking on **Dev only**
3. Load `RandomForest_StageA_tuned_bestk24.joblib` and report OOF metrics on **Dev only**
4. Compute SHAP feature-importance and dependence plots on **Dev only**
5. Report final held-out performance on **Lockbox only**

## Reproducibility rules used in this public module

To remain consistent with the manuscript workflow:

- the full 578-sample table is public
- **Dev** is used for model development and public benchmarking
- **Lockbox** is reserved for final held-out evaluation only
- the tuned model is provided directly
- the hyperparameter-search process itself is not reproduced in this public package

## Notes for reviewers and readers

This repository is intended as a concise transparency package for the published Stage-A activity model.
It is not designed as a full software distribution.

If additional intermediate artifacts or extended validation materials are required during editorial review, they can be supplied separately.
