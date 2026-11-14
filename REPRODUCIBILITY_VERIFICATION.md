
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


## Additional Stage 2 GPU Reproduction Verification

### Recorded Execution Environment

The neural-network experiments were executed
in Google Colab using:

- GPU: NVIDIA Tesla T4
- Python: 3.13.16
- PyTorch: 2.14.1+cu130
- CUDA: Available
- Random seed: 42

### Reproduction Results

Both neural networks reproduced the original
validation-selected checkpoints at the
individual-prediction level.

| Verification | CNN Fusion NN | MM Fusion NN |
|---|---:|---:|
| Best epoch | 5 | 28 |
| Validation prediction differences | 0 | 0 |
| Test prediction differences | 0 | 0 |
| Test accuracy | 68.58% | 69.03% |
| Test macro-F1 | 67.97% | 68.49% |

All 1,836 validation predictions and all
1,792 test predictions matched the original
saved models for each architecture.

### CPU and GPU Comparison

A separate fresh CPU execution produced
different selected epochs, model predictions,
and test metrics.

This demonstrates sensitivity to execution
conditions, although the precise cause
has not been isolated.

An earlier CPU execution also reproduced
the original predictions, so GPU hardware
alone cannot be established as the cause.

The Tesla T4 environment above is a
verified configuration for reproducing
the original neural-network predictions.

Exact reproducibility across arbitrary
hardware and software environments is
not guaranteed.

### Preserved Artifacts

The successful GPU verification outputs
were backed up to:

finetuning_experiments/verified_stage2_t4_2026/

The backup includes the reproduced model
artifacts and recorded execution environment.

The original research checkpoints and
reported paper results remain unchanged.


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

## Stage 4: Data Integrity and Robustness Verification

### Overview

We reconstructed the data-integrity checks and near-duplicate album-artwork sensitivity analysis using a separate Google Colab notebook.

The original datasets, model checkpoints, predictions, and audit files were preserved without modification.

### Dataset Integrity

The verification covered 11,870 recordings from 6,461 unique artists.

The original training, validation, and test partitions were confirmed to be artist-disjoint.

All experimental recordings were matched to the dataset metadata, with no missing image URLs.

No exact album-index or image-URL overlaps were found between the experimental partitions.

### Near-Duplicate Artwork Audit

The original perceptual-hash candidate file contained 12 pairs.

Using the current experimental split assignments, we identified:

- 5 candidate train–test artwork pairs.
- 4 unique test recordings involved in those pairs.
- No inconsistencies between the stored and reconstructed candidate hash distances.

The verification used the original candidate file. It did not independently regenerate perceptual hashes or establish that all possible near-duplicates had been identified.

### Sensitivity Analysis

We excluded the four flagged test recordings without retraining the models or changing their


## Stage 5: Research Figures and Paper Consistency

### Overview

We reconstructed the numerical tables and research
figures using a separate Google Colab notebook.

The original saved predictions and verified
Stage 3–4 analysis outputs were used.

The original report figures were not overwritten.

### Main Model Results

All ten reported model configurations were
independently evaluated from their saved predictions.

The reconstructed accuracy, balanced accuracy,
and macro-F1 scores matched Table 2 of the
research paper to its reported precision.

The highest-performing model remained
MM Early Fusion LR:

- Accuracy: 70.20%
- Balanced accuracy: 67.30%
- Macro-F1: 69.10%

### Figure 1: Confusion Matrices

Four row-normalized confusion matrices were
reconstructed:

- CNN Audio LR
- CNN Early Fusion LR
- MM Early Fusion LR
- MM Fusion NN

All 24 genre-specific F1 scores matched
Table 7 to two decimal places.

The figure was also reviewed for readability,
consistent labels, and color scaling.

### Figure 2: Fusion Gains by Visual Typicality

The figure was reconstructed using the verified
artist-cluster bootstrap estimates.

All quintile-level fusion gains and their
95% confidence intervals matched the original
research outputs.

The figure illustrates the strong positive
fusion gains in Q1 and negative gains in Q5.

### Figure 3: Accuracy by Visual Typicality

The supplementary accuracy curves were regenerated
from the original frozen model predictions.

The selected Q1/Q5 reference values matched
the original analysis.

### Supplementary Tables

Tables 3, 4, 8, 9, and 10 were verified
against the corresponding predictions and
saved analysis results.

These cover:

- Fusion strategies within Q5
- Rescued and harmed predictions
- Overall prediction transitions
- Audio–visual prediction disagreement
- Near-duplicate artwork sensitivity

All automated consistency checks passed.

### Figure 4: Qualitative Album-Cover Examples

Six selected examples were checked against the
original research metadata:

- Three visually typical examples rescued by fusion
- Three visually atypical examples harmed by fusion

Track identifiers, true genres, quintile assignments,
and model predictions were verified.

The PDF was visually inspected. The six images,
displayed labels, and two-row layout were readable
and consistent with the intended examples.

The correspondence between embedded artwork and
original source URLs was not independently reverified.

The examples are illustrative and should not be
interpreted as a representative sample.

### Saved Outputs

The Stage 5 audit and regenerated figures were
saved separately under:

`finetuning_experiments/reproducible_stage5_2026/`

New figure PDFs and PNGs are located in the
`figures/` subfolder.

No existing research outputs were overwritten.

### Verification Status

The five-stage reconstruction now covers the
principal classification models, statistical
analyses, robustness checks, and research figures.

These results were reproduced using the existing
project data and Colab environment.

Independent execution from a fresh environment
remains outstanding.


## Final Fresh-Runtime Verification: Stages 3–5

Following the successful Tesla T4 reproduction of
Stage 2, Stages 3–5 were executed again in
Google Colab to verify the downstream analyses.

### Stage 3: Visual Typicality and Bootstrap

- All 1,792 visual-typicality quintile assignments matched.
- CNN rescued/harmed counts: 209/142.
- MM rescued/harmed counts: 190/124.
- All quintile-level fusion gains matched the report.
- Artist-cluster bootstrap estimates and confidence
  intervals matched the original saved analysis
  to floating-point precision.
- The analysis uses original saved test predictions
  and is retrospective, not an independent replication.

### Stage 4: Artwork-Overlap Sensitivity

- All 12 saved perceptual-hash candidate pairs verified.
- Five train-test candidate pairs identified.
- Four unique test recordings flagged.
- CNN fusion gain after exclusions: +3.64 pp.
- MM fusion gain after exclusions: +3.58 pp.
- Perceptual hashes were not independently regenerated;
  verification used the saved candidate list.

### Stage 5: Final Tables and Figures

- All 10 Table 2 model results matched the report
  at its stated two-decimal precision.
- Genre-specific F1 scores in Table 7 verified.
- Figure 1 reconstructed from saved predictions.
- Figure 2 bootstrap estimates and intervals verified.
- Figure 3 accuracy values verified.
- Tables 3, 4, 8, 9 and 10 verified.
- Figure 4 track IDs, genres and predictions verified.
- Figure 4 artwork and visual layout still require
  human inspection; image URLs were not independently
  retrieved.

### Conclusion

The five-stage workflow has been checked against
the original experimental artifacts.

Stage 2 reproduced all original validation and
test predictions on a Tesla T4 GPU.

The remaining limitations include small fresh-fit
variation in Stage 1, environment sensitivity during
neural-network retraining, reliance on saved
near-duplicate candidates, and the retrospective
nature of the visual-typicality analysis.



