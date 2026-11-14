
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


## Stage 3: Visual Typicality and Statistical Verification

### Overview

We reconstructed the visual-typicality analysis,
prediction-level transitions, and artist-cluster
bootstrap confidence intervals using a separate
Google Colab notebook.

The original predictions and analysis files
were preserved.

### Dataset and Typicality Verification

The analysis included:

- 1,792 test recordings
- 975 unique test artists
- Six musical genres
- Five genre-relative visual-typicality quintiles

Typicality was reconstructed from the CNN visual
embeddings using cosine distance to the corresponding
training-set genre centroid.

All 1,792 recordings received exactly the same
quintile assignment as in the original analysis.

The maximum cosine-distance difference was
approximately 5.93e-08.

True genre labels were used to define the
retrospective typicality groups. This analysis
is not a deployable prediction-time procedure.

### Rescued and Harmed Predictions

The reconstructed prediction transitions matched
the original analysis exactly.

| Representation | Rescued | Harmed | Net Gain |
|---|---:|---:|---:|
| CNN | 209 | 142 | +3.74 pp |
| MM | 190 | 124 | +3.68 pp |

All rescued and harmed counts also matched
within each of the five typicality quintiles.

### Artist-Cluster Bootstrap

We performed 3,000 bootstrap resamples of
the 975 test artists.

Overall fusion gains and 95% percentile
confidence intervals:

| Comparison | Gain | 95% CI |
|---|---:|---|
| CNN fusion vs audio | +3.74 pp | [1.62, 5.86] |
| MM fusion vs audio | +3.68 pp | [1.67, 5.72] |

The reconstructed bootstrap estimates and
confidence intervals matched the original
saved results to floating-point precision.

### Visual-Typicality Contrasts

The largest contrast was observed between
the most visually typical group (Q1) and
the least visually typical group (Q5).

| Comparison | Q1-Q5 Contrast | 95% CI |
|---|---:|---|
| CNN fusion | +37.17 pp | [30.64, 43.70] |
| MM fusion | +34.40 pp | [28.22, 40.94] |

The difference between the MM and CNN
contrasts was -2.78 percentage points,
with a 95% confidence interval of
[-8.09, 2.89].

These estimates reproduced the original
saved research results.

### Interpretation and Limitations

The results show a strong association between
genre-relative visual typicality and the
benefit of multimodal fusion.

They do not establish a causal relationship.

Because the analysis uses true test genres
and explores subgroup performance on an
already evaluated test set, the findings
should be interpreted as retrospective
and exploratory.

Independent replication on new data remains
an important direction for future research.

### Verification Status

Stage 3 successfully reproduced:

- Visual-typicality quintile assignments
- Overall rescued and harmed predictions
- Quintile-level prediction transitions
- Overall artist-cluster bootstrap intervals
- Quintile-level bootstrap intervals
- Paired Q1-Q5 statistical contrasts

The original analysis files were not overwritten.


## Remaining Verification Work

The principal classification experiments and
visual-typicality statistical analysis have
now been reconstructed.

Remaining tasks include:

- Robustness checks for candidate near-duplicate artwork
- Reproduction and verification of final research figures
- Clean-environment execution and documentation
- Final paper-to-code consistency checks
- Repository organization and team contribution history

