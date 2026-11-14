
## Stage 2: Neural-Network Reproducibility

### Overview

We independently reconstructed the CNN and MM
fusion neural-network experiments using a separate
Google Colab notebook.

The original development notebook and saved model
checkpoints were preserved.

### Neural-Network Architectures

| Setting | CNN Fusion NN | MM Fusion NN |
|---|---|---|
| Input features | 4,096 | 400 |
| Hidden layers | 256, 64 | 128, 32 |
| Trainable parameters | 1,065,670 | 55,654 |
| Best epoch | 5 | 28 |

Both models used AdamW optimization and
validation-based early stopping.

### Validation Verification

The reproduced models selected exactly the same
best epochs as the original experiments.

Validation macro-F1:

- CNN Fusion NN: 0.673827
- MM Fusion NN: 0.688492

For both models, the original and reproduced
checkpoints generated identical predictions
for all 1,836 validation recordings.

### Held-Out Test Verification

The reconstructed models produced the following
results on 1,792 test recordings:

| Model | Accuracy | Balanced Accuracy | Macro-F1 |
|---|---:|---:|---:|
| CNN Fusion NN | 68.58% | 67.90% | 67.97% |
| MM Fusion NN | 69.03% | 68.31% | 68.49% |

All three reported metrics matched the original
research results to two decimal places.

### Prediction-Level Verification

We compared the original saved checkpoints
with the newly trained models.

Results:

- CNN Fusion NN: 0 different test predictions
- MM Fusion NN: 0 different test predictions

Thus, every test recording received the same
predicted genre from the original and reproduced
models for both architectures.

### Execution Environment

The reconstructed experiments were executed
using Google Colab and the original project
data stored in Google Drive.

PyTorch version: 2.14.1+cpu.

New model checkpoints and experiment outputs
were saved separately under:

`finetuning_experiments/reproducible_stage2_2026/`

The original checkpoints were not overwritten.

### Interpretation

Stage 2 successfully reproduced both neural
networks at the individual-prediction level.

This provides stronger verification than
matching aggregate test metrics alone.

The verification was conducted using the
existing project data and Colab environment.
A completely independent installation and
execution from a fresh environment remains
a separate reproducibility objective.

## Remaining Verification Work

The logistic-regression and neural-network
experiments have now been reconstructed.

The remaining research analyses requiring
separate verification include:

- Visual-typicality quintiles
- Rescued and harmed predictions
- Artist-cluster bootstrap confidence intervals
- Robustness to candidate near-duplicate artwork
- Reproduction of the final research figures

