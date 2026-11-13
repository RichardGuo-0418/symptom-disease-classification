# symptom-disease-classification
Reproducible pipeline for disease classification from symptom datasets
# Disease Classification from Symptoms Dataset

A reproducible pipeline for comparing classical and neural classifiers on binary
symptom data, and for measuring how much of the reported accuracy difference
survives when preprocessing, splitting and tuning are held constant.

Undergraduate project (202318020304), Software Engineering.

## Research question

Published studies on symptom-based disease classification report high accuracy,
but with different encodings, different splits and different tuning budgets.
This project asks: **once one dataset is prepared once and split once, which
classifier is actually the more reliable one — and does a neural model beat the
classical learners on binary symptom data?**

## Dataset

Public symptom–disease dataset (Kaggle, `itachi9604/disease-symptom-description-dataset`):

| Property | Value |
|---|---|
| Records | 4,920 |
| Symptom features | 132 (binary, 1 = present) |
| Disease classes | 41 |
| Class balance | imbalanced (largest ≈ 120, smallest ≈ 16 records) |
| Duplicates | identical symptom patterns collapsed before splitting |

The dataset is public and contains no patient identifiers. It is kept read-only
in `data/raw/` and is never edited in place.

## Method

1. **Validation** — reject rows without a recognised label, report non-binary entries.
2. **De-duplication** — collapse identical symptom vectors with the same label;
   duplicates are grouped so they cannot cross partitions.
3. **Split** — stratified 70 / 15 / 15 (train / validation / test), fixed seed.
4. **Training** — five classifiers, tuned on the validation partition only:
   - Decision Tree
   - Naive Bayes
   - Support Vector Machine
   - Random Forest
   - Multilayer Perceptron (132 → 128 ReLU → 64 ReLU → 41 softmax, dropout 0.3, Adam)
5. **Evaluation** — accuracy, macro-averaged precision / recall / F1, per-class
   confusion matrix, 5-fold stratified cross-validation repeated 3 times.
   Every classifier is scored on the same held-out test partition.

## Repository structure
