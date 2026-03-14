# MLOps Logging Lab — ML Pipeline with Python Logging

> **Course:** IE-7374 MLOps  
> **Lab:** Experiment Tracking — Logging Lab (Modified)  
> **Based on:** [raminmohammadi/MLOps](https://github.com/raminmohammadi/MLOps/tree/main/Labs/Experiment_Tracking_Labs/Logging_Labs)

---

## Overview

This lab demonstrates how Python's built-in `logging` module is applied to a **real machine learning pipeline**, rather than a generic hello-world logging demo. Every stage of the pipeline — from data loading to model evaluation to prediction — is instrumented with structured, leveled log messages written to both the console and a persistent log file.

The lab is built on the **Iris classification dataset** using a `RandomForestClassifier` from scikit-learn.

---

## Modifications from Original Lab

The original lab only demonstrates basic Python logging syntax with generic messages (e.g., `"This is a debug message"`). This version makes the following meaningful changes:

| Area | Original Lab | This Lab |
|---|---|---|
| Dataset | None | Iris classification dataset (150 samples, 4 features) |
| Model | None | `RandomForestClassifier` (n_estimators=100, max_depth=4) |
| Logger | Root logger via `basicConfig` | Named logger `ml_pipeline` — production-style |
| Handlers | Single (console or file) | Dual: `StreamHandler` (console) + `FileHandler` (`ml_pipeline.log`) |
| Log content | Generic strings | Real metrics: accuracy, classification report, feature importances, class probabilities |
| WARNING triggers | None | Low accuracy threshold, small test set, low-confidence prediction |
| Exception handling | Simple division-by-zero | Try/except blocks around every pipeline stage |

---

## Pipeline Steps Logged

Logger Setup → INFO: "Logger initialized. Dual output: console + ml_pipeline.log"

Data Loading → INFO: Dataset shape, DEBUG: feature names, class distribution

Train/Test Split → INFO: Train/test sizes, WARNING: if test set < 20 samples

Model Training → INFO: Training start/complete, DEBUG: hyperparameters

Evaluation → INFO: Test accuracy, WARNING: if accuracy < 90%
DEBUG: Full classification report

Feature Importances → DEBUG: Ranked feature importance scores

New Prediction → INFO: Input sample, predicted class, confidence
DEBUG: Per-class probabilities
WARNING: If confidence < 70%

Log File Verification → INFO: Pipeline complete, reads and prints ml_pipeline.log


---

## Results

| Metric | Value |
|---|---|
| Train size | 120 samples |
| Test size | 30 samples |
| Test Accuracy | **96.67%** |
| Top Feature | petal length (cm) — 0.4377 importance |
| Sample Prediction | setosa — 100% confidence |

### Classification Report

          precision    recall  f1-score   support

  setosa       1.00      1.00      1.00        10

versicolor 1.00 0.90 0.95 10
virginica 0.91 1.00 0.95 10

accuracy                           0.97        30

macro avg 0.97 0.97 0.97 30
weighted avg 0.97 0.97 0.97 30


---

## Project Structure

logging_lab_modified/
│
├── ml_logging_lab.ipynb # Main notebook — all logging + ML pipeline code
├── ml_pipeline.log # Auto-generated log file (created on notebook run)
├── requirements.txt # Python dependencies
├── .gitignore # Excludes venv, pycache, checkpoints
└── README.md # This file


---

## How to Run

### 1. Clone the repo

git clone https://github.com/vraj1507/mlops-logging-lab.git
cd mlops-logging-lab

python3 -m venv venv
source venv/bin/activate        # Mac/Linux
# venv\Scripts\activate         # Windows

3. Install dependencies
pip install -r requirements.txt

4. Launch Jupyter and run the notebook
jupyter notebook ml_logging_lab.ipynb
Run all cells: Kernel → Restart & Run All

After running, a ml_pipeline.log file will be created in the same directory containing the full structured log output from the pipeline.

| Concept           | Implementation                                                      |
| ----------------- | ------------------------------------------------------------------- |
| Named logger      | logging.getLogger("ml_pipeline")                                    |
| Dual handlers     | StreamHandler + FileHandler                                         |
| Custom formatter  | %(asctime)s - %(name)s - %(levelname)s - %(message)s                |
| DEBUG level       | Hyperparameters, feature names, class distribution, probabilities   |
| INFO level        | Dataset shape, split sizes, accuracy, training complete             |
| WARNING level     | Small test set, accuracy below threshold, low-confidence prediction |
| CRITICAL level    | Model training crash handler                                        |
| Exception logging | logger.exception(...) in all try/except blocks                      |
| Log file output   | ml_pipeline.log — persistent record of every pipeline run           |

Dependencies

jupyter
scikit-learn
pandas
numpy
See requirements.txt for exact pinned versions.

Course Reference
Course: IE-7374 — MLOps

Original Lab Repo: raminmohammadi/MLOps

Lab Section: Labs/Experiment_Tracking_Labs/Logging_Labs


***

