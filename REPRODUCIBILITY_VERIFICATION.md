
# Reproducibility Verification

## Overview

We reconstructed the logistic-regression and fusion
experiments in a separate Google Colab notebook.

The original development notebook was preserved
without modification.

## Environment

The verification run used:

- Python 3.13.18
- NumPy 2.4.6
- pandas 2.2.3
- scikit-learn 1.6.1

## Dataset Verification

The notebook successfully loaded and validated
all four feature representations.

The original artist-disjoint partitions were
preserved:

| Split | Recordings |
|---|---:|
| Training | 8,242 |
| Validation | 1,836 |
| Test | 1,792 |

## Model Selection

Logistic-regression regularization was selected
using validation macro-F1.

The reconstructed experiments selected the same
late-fusion parameters as the original analysis:

- Fixed audio weight: 0.55
- Adaptive beta: 0.50
- Adaptive gamma: 1

## Test Results

Six of the eight reported model configurations
reproduced all three metrics to two decimal
places.

Two models exhibited small differences:

| Model | Original accuracy | Reproduced accuracy |
|---|---:|---:|
| CNN Early Fusion LR | 69.64% | 69.59% |
| All Four LR | 69.48% | 69.53% |

Each discrepancy was traced to exactly one
different test prediction.

The original saved model checkpoints reproduced
their corresponding original prediction CSV
files with zero disagreements.

## Numerical Differences

The original and reconstructed models used the
same regularization strength, solver, tolerance,
maximum iterations, and feature-scaling means.

Their fitted coefficients differed slightly.

For CNN Early Fusion, both fitting runs took
130 iterations.

For All Four LR, the original model took 163
iterations, while the reconstructed model
took 151 iterations.

The models differed on recordings for which
the two leading class probabilities were
extremely close.

The exact cause of the numerical differences
has not been conclusively established.

No reported results were modified to conceal
these discrepancies.

## Main Finding

The highest-performing logistic-regression
configuration, MM Early Fusion, reproduced:

- Test accuracy: 70.20%
- Balanced accuracy: 67.30%
- Macro-F1: 69.10%

These findings support the reproducibility of
the main logistic-regression results in the
tested Colab environment.

## Scope

This verification covers the Stage 1
logistic-regression and fusion experiments.

Neural-network experiments, visual-typicality
analysis, bootstrap uncertainty estimates,
and robustness checks require separate
verification.

The original dataset and intermediate inputs
remain external to the public GitHub repository.
