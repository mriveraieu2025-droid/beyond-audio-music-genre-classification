
# Reproducibility Verification Record

**Project:** Beyond Audio: Music Genre Classification  
**IE University — Machine Learning Final Project, November 2026**

## Overview

This document records the verification procedures and results supporting the experiments reported in *Beyond Audio: Music Genre Classification*.

The verification workflow consists of five core experimental stages and three supplementary experiments:

1. Logistic-regression and fusion verification.
2. Neural-network reproducibility.
3. Visual-typicality and statistical verification.
4. Data-integrity and artwork-overlap sensitivity.
5. Research tables and figures.
6. Class-weighted neural-network verification.
7. Alternative visual-typicality representation.
8. Neural-network stability across random seeds.

The experiments were conducted using Google Colab and the team's existing MSD-I project data and saved research artifacts.

**Important reproducibility boundary:** The repository provides documented reconstruction and verification procedures, not an entirely self-contained raw-data-to-paper pipeline. Some notebooks depend on prepared features, previously saved model predictions, checkpoints, and other intermediate files that are not publicly included.

The analyses have not been independently replicated using a different dataset.

For setup requirements and data provenance, see:

- [Reproduction Instructions](REPRODUCE.md)
- [Data and Reproducibility Guide](notebooks/DATA_AND_REPRODUCIBILITY.md)
- [Experiment Index](notebooks/EXPERIMENT_INDEX.md)

---

# Part I — Core Experiment Verification

## 1. Stage 1: Logistic Regression and Fusion

**Notebook:** [Stage 1](notebooks/beyond_audio_final_experiments_stage1.ipynb)

**Report sections:** 3.1, 3.2, 4.1, and 4.2.

### Objective

Stage 1 reconstructs the downstream logistic-regression experiments and validates the main audio-only, visual-only, and multimodal classification results.

The analysis uses the prepared CNN and MM representations and preserves the original artist-disjoint experimental partitions.

### Dataset Verification

The recorded data checks confirmed:

| Partition | Recordings |
|---|---:|
| Training | 8,242 |
| Validation | 1,836 |
| Test | 1,792 |
| **Total** | **11,870** |

The notebook checked feature dimensions, label alignment, track identifiers, and consistency between the CNN and MM representations.

Preprocessing was fitted using training-set observations only.

Model hyperparameters were selected using validation macro-F1.

### Classification Results

All values are percentages.

| Configuration | Accuracy | Balanced accuracy | Macro-F1 |
|---|---:|---:|---:|
| CNN Audio LR | 65.90 | 62.36 | 64.07 |
| CNN Visual LR | 47.60 | 44.76 | 46.34 |
| CNN Early Fusion LR | 69.64 | 67.39 | 68.68 |
| CNN Fixed Late Fusion | 68.69 | 65.02 | 67.29 |
| CNN Adaptive Late Fusion | 68.30 | 64.97 | 67.08 |
| MM Audio LR | 66.52 | 63.13 | 64.64 |
| MM Early Fusion LR | 70.20 | 67.30 | 69.10 |
| All Four LR | 69.48 | 67.51 | 68.69 |

The best overall result was MM early-fusion logistic regression, with 70.20% test accuracy.

### Fusion Parameter Verification

The validation-selected parameters were:

- Fixed late-fusion audio weight: 0.55.
- Adaptive late-fusion beta: 0.5.
- Adaptive late-fusion gamma: 1.

These parameters were selected using validation macro-F1 and then applied to the held-out test data.

### Comparison With Original Research Artifacts

The saved diagnostic comparisons showed zero differing test predictions between the reconstructed and original models for:

- CNN Early Fusion LR.
- All Four LR.

For these selected comparisons, the fitted scaler statistics and classifier coefficients also matched the original saved models.

One historical CNN checkpoint used `random_state=None`, whereas the reconstruction used `random_state=42`. The observed fitted coefficients and predictions nevertheless matched.

### Verification Limitations

The selected checkpoint comparisons do not independently establish exact reproduction of every logistic-regression configuration.

The committed Stage 1 notebook also lacks a saved execution count and output for its model-fitting cell, although subsequent verification cells contain recorded results.

Consequently, its saved outputs should not be interpreted as evidence that the entire notebook was independently executed from a completely fresh environment.

### Conclusion

Stage 1 provides numerical verification of the reported logistic-regression and fusion results using the existing experimental features and saved research artifacts.

---

## 2. Stage 2: Neural-Network Reproducibility

**Notebook:** [Stage 2](notebooks/beyond_audio_final_experiments_stage2.ipynb)

**Report sections:** 3.1, 4.1, and Appendix A.

### Objective

Stage 2 reconstructs the CNN and MM early-fusion feedforward neural networks.

Its purpose is to verify the reported training procedure, validation-selected checkpoints, and held-out test predictions.

### Model Architectures

| Setting | CNN Fusion NN | MM Fusion NN |
|---|---:|---:|
| Input dimensions | 4,096 | 400 |
| First hidden layer | 256 | 128 |
| Second hidden layer | 64 | 32 |
| Output classes | 6 | 6 |
| Trainable parameters | 1,065,670 | 55,654 |
| Original best epoch | 5 | 28 |

Both networks used:

- ReLU activation functions.
- Dropout regularization.
- Multiclass cross-entropy loss.
- AdamW optimization.
- Batch size 128.
- A maximum of 100 epochs.
- Early stopping with patience 12.
- Validation macro-F1 for checkpoint selection.

Input standardization used training-set statistics.

### Validation Verification

The successfully reproduced models selected the original best epochs:

- CNN: epoch 5.
- MM: epoch 28.

Validation macro-F1 values were:

| Model | Validation macro-F1 |
|---|---:|
| CNN Fusion NN | 0.673827 |
| MM Fusion NN | 0.688492 |

All 1,836 validation class predictions matched the original saved predictions for each model in the successful reproduction.

### Held-Out Test Verification

| Model | Accuracy | Balanced accuracy | Macro-F1 |
|---|---:|---:|---:|
| CNN Fusion NN | 68.58% | 67.90% | 67.97% |
| MM Fusion NN | 69.03% | 68.31% | 68.49% |

The successful reproduction also matched the original predictions at the individual-recording level.

| Comparison | CNN | MM |
|---|---:|---:|
| Different validation predictions | 0 | 0 |
| Different test predictions | 0 | 0 |

Thus, the reconstructed models assigned the same predicted genre to every one of the 1,792 test recordings.

### Verified GPU Environment

A successful reproduction was recorded in Google Colab with:

| Component | Recorded value |
|---|---|
| GPU | NVIDIA Tesla T4 |
| Python | 3.13.16 |
| PyTorch | 2.14.1+cu130 |
| CUDA | Available |
| Random seed | 42 |

The corresponding verified artifacts were backed up under:

`finetuning_experiments/verified_stage2_t4_2026/`

The original research checkpoints were preserved.

### CPU and GPU Execution Differences

Additional CPU executions were performed during verification.

An earlier CPU execution reproduced the original class predictions, while another fresh CPU execution produced different selected epochs, predictions, and test metrics.

Therefore, the differences cannot be attributed conclusively to CPU versus GPU hardware alone.

The precise source of this environment-dependent variation was not isolated.

The recorded Tesla T4 environment is a verified configuration for reproducing the original predictions, but exact reproducibility across arbitrary software and hardware environments is not guaranteed.

### Conclusion

Stage 2 reproduced the original CNN and MM neural-network results at the individual-prediction level under documented execution conditions.

This is stronger evidence than matching aggregate metrics alone, although it does not establish environment-independent reproducibility.

---

## 3. Stage 3: Visual Typicality and Statistical Verification

**Notebook:** [Stage 3](notebooks/beyond_audio_final_experiments_stage3.ipynb)

**Report sections:** 3.3, 3.4, 4.3, 4.4, and Appendix C.

### Objective

Stage 3 reconstructs:

- Genre-relative visual-typicality quintiles.
- Rescued and harmed predictions.
- Overall and group-specific fusion gains.
- Artist-cluster bootstrap confidence intervals.
- Comparisons between the most typical and atypical groups.

The analysis uses the original frozen model predictions.

### Visual-Typicality Verification

The reconstruction used:

- 1,792 test recordings.
- 975 test artists.
- Six musical genres.
- Five genre-conditional typicality quintiles.

Visual typicality was calculated using standardized CNN visual embeddings and cosine distance to the corresponding training-set true-genre centroid.

All 1,792 test recordings received exactly the same quintile assignment as in the original analysis.

The maximum observed difference between reconstructed and original cosine distances was approximately `5.93e-08`.

**Important:** True genre labels were required to construct the groups. The analysis is therefore retrospective, not a deployable method for determining visual reliability for new unlabeled recordings.

### Rescued and Harmed Predictions

The reconstructed prediction transitions matched the original results.

| Representation | Rescued | Harmed | Net additional correct |
|---|---:|---:|---:|
| CNN | 209 | 142 | 67 |
| MM | 190 | 124 | 66 |

The corresponding overall accuracy gains were:

- CNN: +3.74 percentage points.
- MM: +3.68 percentage points.

The quintile-specific rescued and harmed counts also matched the original analysis.

### Artist-Cluster Bootstrap

The experiment used 3,000 bootstrap resamples of the 975 test artists.

All observations belonging to a selected artist were sampled together.

The reconstructed overall accuracy-gain estimates were:

| Comparison | Gain | 95% confidence interval |
|---|---:|---|
| CNN fusion versus audio-only | +3.74 pp | [1.62, 5.86] |
| MM fusion versus audio-only | +3.68 pp | [1.67, 5.72] |

The estimates matched the original saved research results to floating-point precision.

### Q1–Q5 Contrasts

The largest differences occurred between the most visually typical (Q1) and most atypical (Q5) recordings.

| Comparison | Q1–Q5 gain contrast | 95% confidence interval |
|---|---:|---|
| CNN | 37.17 pp | [30.64, 43.70] |
| MM | 34.40 pp | [28.22, 40.94] |

The difference between MM and CNN contrasts was approximately -2.78 percentage points, with a 95% confidence interval of [-8.09, 2.89].

The interval includes zero, so the results do not establish a clear difference between the two representations in the magnitude of this association.

### Interpretation and Limitations

The analysis identifies a strong observed association between visual typicality and fusion performance.

It does not demonstrate that visual atypicality causes classification errors.

The grouping procedure was applied retrospectively using true test labels. The bootstrap confidence intervals condition on the trained classifiers and observed analysis groups.

They do not independently validate the relationship on new data or account for all prior model-development and analysis choices.

### Conclusion

Stage 3 successfully reproduced the typicality assignments, prediction transitions, bootstrap intervals, and Q1–Q5 contrasts used in the report.

---

## 4. Stage 4: Data Integrity and Artwork-Overlap Sensitivity

**Notebook:** [Stage 4](notebooks/beyond_audio_final_experiments_stage4.ipynb)

**Report sections:** 2.3 and Appendix D.1.

### Objective

Stage 4 verifies the integrity of the experimental partitions and evaluates the sensitivity of the classification results to identified candidate near-duplicate album artwork.

### Dataset Integrity

The recorded checks covered:

- 11,870 recordings.
- 6,461 artists.
- Artist-disjoint training, validation, and test partitions.
- Consistent track and feature alignment.
- Availability of album-cover metadata.

No exact album-index or image-URL overlaps were detected between experimental partitions.

### Candidate Near-Duplicate Artwork

The original perceptual-hash candidate file contained 12 image pairs.

Using the current experimental partition assignments, the verification identified:

- Five candidate training–test artwork pairs.
- Four unique affected test recordings.
- Consistency between saved and reconstructed candidate hash distances.

The verification used the previously saved candidate list.

It did not independently regenerate the full perceptual-hash search or establish that every visually similar image had been identified.

### Sensitivity Analysis

The four flagged test recordings were excluded from evaluation.

The resulting test subset contained 1,788 recordings.

The original trained classifiers, frozen predictions, and visual-typicality assignments were retained.

No models were retrained.

| Quantity | Original | After exclusion |
|---|---:|---:|
| CNN overall fusion gain | +3.74 pp | +3.64 pp |
| MM overall fusion gain | +3.68 pp | +3.58 pp |
| CNN Q1 gain | +17.45 pp | +17.04 pp |
| MM Q1 gain | +17.45 pp | +17.04 pp |
| CNN Q5 gain | -19.72 pp | -19.72 pp |
| MM Q5 gain | -16.94 pp | -16.94 pp |

Three flagged recordings belonged to Q1 and one to Q3.

None belonged to Q5, so the Q5 results remained unchanged.

### Interpretation

The identified candidate overlaps do not account for the overall multimodal accuracy gains or the main visual-typicality pattern.

However, this does not establish the absence of all possible artwork duplication or other data-quality issues.

### Conclusion

Stage 4 verified the saved candidate-artwork records and showed that excluding the four flagged recordings produced only small changes in the reported fusion gains.

---

## 5. Stage 5: Research Tables and Figures

**Notebook:** [Stage 5](notebooks/beyond_audio_final_experiments_stage5.ipynb)

**Report sections:** Section 4 and Appendices B–D.

### Objective

Stage 5 recalculates the reported performance metrics and verifies the data underlying the main tables, confusion matrices, visual-typicality figures, and qualitative examples.

It uses original saved model predictions and verified downstream research artifacts. The Stage 5 notebook's `figure1`–`figure4` references and exported filenames are historical internal identifiers; the revised manuscript can number those figures differently. The checks below are identified by their subject rather than relying on figure-number equality.

### Classification Table Verification

Metrics for all ten reported model configurations were recalculated from frozen test predictions.

These included:

- Accuracy.
- Balanced accuracy.
- Macro-F1.

The results were compared with the scientific report.

One historical reference discrepancy concerned the All Four LR balanced accuracy:

- Original reported model value: 67.51%.
- Earlier saved verification reference: 67.52%.
- Corrected notebook reference: 67.51%.

The Stage 1 and Stage 5 source references were corrected to 67.51%.

The previously saved Stage 5 output was not regenerated following that reference edit. Its cached output should therefore not be treated as evidence of a new post-correction execution.

### Confusion Matrices

Four row-normalized confusion matrices were reconstructed:

- CNN Audio LR.
- CNN Early Fusion LR.
- MM Early Fusion LR.
- MM Fusion NN.

All 24 genre-specific F1 scores matched the corresponding supplementary results in Table 7 to two decimal places.

The figures were also checked for labeling and readability.

### Fusion Gains by Visual Typicality

The visual-typicality fusion-gain figure was reconstructed using the verified artist-cluster bootstrap estimates.

The quintile-specific gains and reported 95% confidence intervals matched the original analysis.

### Absolute Accuracy by Visual Typicality

The supplementary accuracy curves were reconstructed from the original frozen model predictions.

The selected Q1 and Q5 accuracy values matched the original results.

### Supplementary Tables

The checks covered the report's supplementary tables, including:

- Fusion strategy comparisons in Q5.
- Rescued and harmed prediction counts.
- Overall prediction transitions.
- Audio–visual prediction disagreement.
- Artwork-overlap sensitivity results.

The recorded automated numerical consistency checks passed.

### Qualitative Album-Cover Examples

Six selected album-cover examples were checked against the original research metadata.

The examples comprise:

- Three visually typical recordings rescued by early fusion.
- Three visually atypical recordings harmed by early fusion.

The verification checked:

- Track identifiers.
- True genre labels.
- Typicality groups.
- Recorded model predictions.

The PDF was visually inspected, and the displayed figures and labels were readable.

The connection between the embedded images and their original source artwork URLs was not independently reverified.

The examples are illustrative, not statistically representative.

### Saved Outputs

The Stage 5 output was saved under:

`finetuning_experiments/reproducible_stage5_2026/`

Regenerated figures were saved separately within its `figures/` subdirectory.

The original research figures and predictions were not overwritten.

### Conclusion

Stage 5 established numerical consistency between the reported results and the available saved prediction-level evidence, subject to the documented cached-output and provenance limitations.

---

# Part II — Supplementary Experiment Verification

The following three experiments were performed to investigate additional questions raised during the final report review.

They supplement the five core stages and support results in Appendices B and D.

## 6. Class-Weighted Neural-Network Verification

**Notebook:** [Weighted NN Verification](notebooks/weighted_nn_verification_colab.ipynb)

**Report sections:** 4.1 and Appendix B.3.

### Objective

This experiment investigates whether class-weighted cross-entropy improves neural-network classification of underrepresented genres.

Both the CNN and MM early-fusion networks were reconstructed using the original architectures, experimental partitions, and hyperparameters.

Class weights were calculated from the training labels.

Weighted and unweighted networks were compared using seed 42, with validation macro-F1 used for checkpoint selection.

### Recorded Results

All values are percentages.

| Model | Validation macro-F1 | Test accuracy | Test balanced accuracy | Test macro-F1 |
|---|---:|---:|---:|---:|
| CNN unweighted | 67.38 | 68.58 | 67.90 | 67.97 |
| CNN weighted | 67.09 | 68.25 | 70.24 | 68.23 |
| MM unweighted | 68.85 | 69.03 | 68.31 | 68.49 |
| MM weighted | 68.14 | 68.02 | 70.93 | 68.17 |

The reconstructed experiment reproduced the reported weighted-model metrics to the displayed precision.

The unweighted control experiments also reproduced the original seed-42 test predictions in the recorded execution environment.

### Interpretation

Class weighting increased test balanced accuracy:

- CNN: 67.90% to 70.24%.
- MM: 68.31% to 70.93%.

However, overall test accuracy decreased for both networks.

CNN test macro-F1 increased slightly, while MM test macro-F1 decreased.

Both weighted networks achieved lower validation macro-F1 than their unweighted counterparts.

Therefore, the original unweighted networks were retained in the main classification comparisons.

### Limitations

The original historical weighted-training history was not available.

Verification was performed through a reconstructed training experiment using the reported architecture and settings.

Both weighted and unweighted networks were compared using one random seed.

The experiment therefore does not establish the stability of weighting effects across random initializations.

---

## 7. Alternative Visual-Typicality Representation

**Notebook:** [MM Visual-Typicality Robustness](notebooks/mm_visual_typicality_robustness_colab.ipynb)

**Report sections:** 4.3 and Appendix D.3.

### Objective

The original analysis defines visual typicality using CNN visual embeddings.

This supplementary experiment investigates whether the observed relationship between visual typicality and fusion performance persists when the groups are defined using MM visual embeddings.

### Verification Procedure

The analysis used the same 1,792 test recordings and frozen classifier predictions.

MM visual embeddings were standardized using training-set statistics.

Genre centroids were calculated from the training data.

Test recordings were ranked within their true genre using cosine distance to the corresponding centroid.

New MM-based genre-conditional quintiles were constructed.

Classifier predictions were not retrained.

Artist-cluster bootstrap resampling was used to quantify uncertainty.

### Recorded Results

All gains are percentage-point differences relative to the corresponding audio-only classifier.

| Typicality definition | Classifier | Q1 gain | Q5 gain |
|---|---|---:|---:|
| CNN visual | CNN | +17.45 | -19.72 |
| CNN visual | MM | +17.45 | -16.94 |
| MM visual | CNN | +16.62 | -18.61 |
| MM visual | MM | +19.11 | -19.72 |

Only **54.19%** of test recordings retained exactly the same quintile assignment across the CNN and MM grouping definitions.

For MM-defined quintiles, the 95% artist-cluster bootstrap confidence intervals were:

| Classifier | Q1 confidence interval | Q5 confidence interval |
|---|---|---|
| CNN | [12.80, 20.58] | [-23.06, -14.25] |
| MM | [14.93, 23.31] | [-24.50, -15.12] |

All confidence intervals are expressed in percentage points.

### Interpretation

Although the individual quintile assignments changed substantially, the main qualitative finding persisted.

Fusion gains remained positive for the most visually typical recordings and substantially negative for the most atypical recordings under both grouping definitions.

This suggests that the observed association is not restricted to the original CNN-based definition.

### Limitations

Both grouping methods require true genre labels and are defined retrospectively.

CNN and MM representations share an underlying feature-extraction history and therefore do not provide fully independent measurements.

The analysis is a robustness check, not independent validation or evidence that visual typicality causally determines classifier performance.

---

## 8. Neural-Network Stability Across Random Seeds

**Notebook:** [Multi-Seed NN Stability](notebooks/beyond_audio_multiseed_nn_stability_colab.ipynb)

**Report sections:** 4.1, 4.5, and Appendix D.4.

### Objective

This experiment assesses whether the reported neural-network results are highly sensitive to the original random initialization.

The unweighted CNN and MM early-fusion neural networks were trained using five predefined random seeds:

**42, 43, 44, 45, 46**

### Verification Procedure

All runs used:

- The original artist-disjoint data partitions.
- The original network architectures.
- The same hyperparameters and training procedure.
- Training-only feature standardization.
- Validation macro-F1 for checkpoint selection.
- The held-out test partition for final evaluation.

Every predefined seed was evaluated and included in the summary.

No seed was selected based on its test performance.

### Recorded Execution Environment

| Component | Recorded value |
|---|---|
| Platform | Google Colab |
| GPU | NVIDIA Tesla T4 |
| Python | 3.13.16 |
| PyTorch | 2.14.1+cu130 |
| CUDA | Available |

### Per-Seed Results

All values are percentages.

| Model | Seed | Test accuracy | Test balanced accuracy | Test macro-F1 |
|---|---:|---:|---:|---:|
| CNN | 42 | 68.58 | 67.90 | 67.97 |
| CNN | 43 | 69.20 | 68.14 | 68.72 |
| CNN | 44 | 68.92 | 68.32 | 68.24 |
| CNN | 45 | 69.81 | 67.58 | 68.75 |
| CNN | 46 | 68.19 | 67.94 | 67.81 |
| MM | 42 | 69.03 | 68.31 | 68.49 |
| MM | 43 | 70.20 | 68.80 | 69.23 |
| MM | 44 | 69.20 | 68.41 | 68.59 |
| MM | 45 | 69.87 | 68.59 | 69.24 |
| MM | 46 | 68.92 | 67.73 | 68.26 |

### Summary Statistics

Values are mean ± sample standard deviation across five seeds, expressed in percentage points.

| Metric | CNN | MM |
|---|---:|---:|
| Validation macro-F1 | 67.56 ± 0.38 | 68.86 ± 0.17 |
| Test accuracy | 68.94 ± 0.61 | 69.44 ± 0.56 |
| Test balanced accuracy | 67.98 ± 0.28 | 68.37 ± 0.41 |
| Test macro-F1 | 68.30 ± 0.43 | 68.76 ± 0.45 |

The original seed-42 predictions were reproduced exactly in the recorded Tesla T4 environment for both CNN and MM networks.

### Comparison With Logistic Regression

The mean neural-network accuracies remained below their corresponding logistic-regression results:

| Representation | Mean NN accuracy | LR accuracy |
|---|---:|---:|
| CNN | 68.94% | 69.64% |
| MM | 69.44% | 70.20% |

However, individual neural-network runs occasionally matched or exceeded these baselines.

### Interpretation

Performance variation across the five seeds was relatively modest.

The overall finding that logistic regression performs competitively with the tested feedforward neural networks is therefore not driven solely by the original seed-42 initialization.

### Limitations

Only five random seeds were evaluated.

All experiments used the same fixed training, validation, and test partitions.

The experiment measures sensitivity to training randomness rather than generalization across independent datasets.

It evaluates unweighted networks only and does not establish multi-seed stability for the class-weighting experiment.

---

# Part III — Verification Boundaries and Conclusions

## 9. Recorded Evidence and Availability

The core experiment notebooks contain saved verification outputs from the original Colab checks, subject to the execution-history qualifications described above.

The three supplementary notebooks currently committed to GitHub contain executable procedures but do not include saved cell outputs.

The supplementary numerical results reported here were obtained from the executed Colab experiments and checked against the scientific report.

A reader can inspect the supplementary code, but the committed notebook files alone do not independently demonstrate the recorded executions.

The underlying data and research artifacts are stored outside this public repository.

These include:

- Original MSD-I feature files and metadata.
- Prepared CNN and MM matrices.
- Saved model predictions.
- Model checkpoints.
- Original perceptual-hash candidates.
- Selected intermediate tables and figures.
- Verification output directories in Google Drive.

Access to the authorized project workspace is necessary to reproduce the data-dependent workflows.

## 10. Overall Verification Summary

| Experiment | Main verified result | Principal limitation |
|---|---|---|
| Stage 1 | Logistic-regression and fusion metrics, with selected checkpoint comparisons | Existing prepared features and historical artifacts |
| Stage 2 | Original CNN/MM neural-network class predictions reproduced | Execution-environment sensitivity |
| Stage 3 | Typicality quintiles, transitions, and bootstrap estimates reconstructed | Retrospective true-genre-defined groups |
| Stage 4 | Candidate artwork-overlap sensitivity verified | Saved candidate list, not exhaustive hash regeneration |
| Stage 5 | Main tables, figure data, and supplementary results checked | Uses saved predictions and intermediate artifacts |
| Weighted NN | Class-weighting metrics reconstructed | One seed; original historical training history unavailable |
| Alternative Typicality | Main association persisted with MM-based groups | Retrospective and related feature representations |
| Multi-Seed NN | Performance summarized across five predefined seeds | One fixed data partition |

## 11. Final Conclusion

The verification work supports the numerical consistency of the main results reported in *Beyond Audio: Music Genre Classification*.

In particular:

- The reported classification results were checked against saved predictions.
- The original CNN/MM neural-network predictions were reproduced in a documented Tesla T4 environment.
- Visual-typicality assignments and prediction transitions were reconstructed.
- Artist-cluster bootstrap estimates matched the original analyses.
- Identified candidate artwork overlaps had a small effect on the reported fusion gains.
- Alternative visual embeddings preserved the main qualitative typicality pattern.
- Five-seed experiments indicated relatively modest neural-network performance variation.

These findings strengthen confidence in the internal consistency and documented reproducibility of the experiments under the tested conditions.

However, the project has not established:

- Fully independent execution from raw source data to every final research artifact.
- Exact neural-network reproducibility across arbitrary computing environments.
- Exhaustive exclusion of near-duplicate album artwork.
- Independent validation of the retrospective visual-typicality association on new data.
- A prediction-time method for identifying visually unreliable album covers.

The project should therefore be interpreted as a verified, exploratory experimental analysis using a fixed dataset and precomputed representations, rather than an independently validated deployment-ready classification system.

For navigation and execution instructions, return to the [main README](README.md) or consult [REPRODUCE.md](REPRODUCE.md).
