
# Beyond Audio: Music Genre Classification

**Machine Learning Final Project — November 2026**  
**IE University | Bachelor in Applied Mathematics**

**Authors:** Carlos Saldaña, Ana Santos, María Rivera

## 1. Project Overview

This project investigates how album-cover images influence music genre classification, and when combining audio and visual information improves or worsens predictions.

We compare audio-only, visual-only, and multimodal classifiers using precomputed audio and album-cover embeddings from the MSD-I dataset.

Beyond comparing overall classification performance, we investigate individual predictions to understand when adding visual information corrects existing errors and when it introduces new ones.

**Research question:**

> How do album-cover images influence music genre classification, and when does combining visual and audio information improve or worsen predictions?

Our main contribution is an experimental analysis of the relationship between multimodal fusion performance and album-cover visual typicality.

## 2. Dataset and Representations

We use a subset of the MSD-I dataset introduced by Oramas et al. (2018), containing **11,870 recordings** across six genres:

Electronic, Jazz, Metal, Pop, Rap, and Rock.

The original artist-disjoint experimental partitions are preserved.

| Partition | Recordings |
|---|---:|
| Training | 8,242 |
| Validation | 1,836 |
| Test | 1,792 |
| **Total** | **11,870** |

The test partition contains recordings from 975 distinct artists.

### Feature Representations

We use two families of precomputed embeddings:

| Representation | Audio dimensions | Visual dimensions | Combined |
|---|---:|---:|---:|
| CNN | 2,048 | 2,048 | 4,096 |
| MM | 200 | 200 | 400 |

The MM representations were learned within the original multimodal framework and are not independent of the CNN feature-extraction history.

**Important:** We train downstream classification models using these embeddings. We do not retrain the original CNN or MM feature extractors.

## 3. Methodology

### Classification Models

We compare:

- Multiclass logistic regression
- Feedforward neural networks
- Audio-only classification
- Visual-only classification using CNN features
- CNN and MM early fusion
- Concatenation of all four representations

We additionally evaluate two late-fusion strategies using CNN representations:

- Fixed late fusion
- Confidence-adaptive late fusion

### Training and Model Selection

All preprocessing statistics are estimated from the training set.

Model hyperparameters and neural-network checkpoints are selected using validation macro-F1.

Final evaluation uses the held-out test partition.

### Evaluation Metrics

We report:

- Overall accuracy
- Balanced accuracy
- Macro-F1
- Genre-specific classification performance
- Rescued and harmed predictions
- Artist-cluster bootstrap confidence intervals

### Visual-Typicality Analysis

We investigate whether the effect of multimodal fusion varies with the visual typicality of album covers.

Visual typicality is measured using the cosine distance between a recording's visual embedding and the training-set centroid of its true genre.

Test recordings are ranked within each genre and divided into five groups:

- Q1: most visually typical
- Q5: most visually atypical

The original analysis uses CNN visual embeddings.

**Important limitation:** Quintile membership requires the true genre label and is constructed retrospectively using the test distribution. It therefore cannot directly be used to select a fusion strategy for a new unlabeled recording.

## 4. Main Results

### Overall Classification Performance

| Model | Test accuracy | Macro-F1 |
|---|---:|---:|
| CNN Audio LR | 65.90% | 64.07% |
| CNN Visual LR | 47.60% | 46.34% |
| CNN Early Fusion LR | 69.64% | 68.68% |
| MM Audio LR | 66.52% | 64.64% |
| MM Early Fusion LR | **70.20%** | **69.10%** |
| CNN Fusion NN | 68.58% | 67.97% |
| MM Fusion NN | 69.03% | 68.49% |

The best overall classification accuracy is **70.20%**, achieved by MM early-fusion logistic regression.

Compared with the corresponding audio-only classifiers:

- CNN early fusion improves accuracy by 3.74 percentage points.
- MM early fusion improves accuracy by 3.68 percentage points.

Both improvements have positive 95% artist-cluster bootstrap confidence intervals.

### Visual Typicality and Fusion Performance

The overall improvements are not distributed equally across recordings.

| Typicality group | CNN fusion gain | MM fusion gain |
|---|---:|---:|
| Q1 (most typical) | +17.45 pp | +17.45 pp |
| Q2 | +12.32 pp | +12.89 pp |
| Q3 | +10.64 pp | +8.40 pp |
| Q4 | -1.96 pp | -3.36 pp |
| Q5 (most atypical) | -19.72 pp | -16.94 pp |

Early fusion is associated with substantial improvements for visually typical covers and substantial deterioration for visually atypical covers.

### Prediction-Level Analysis

Adding visual information changes individual predictions in both beneficial and harmful ways.

| Representation | Rescued | Harmed | Net improvement |
|---|---:|---:|---:|
| CNN early fusion | 209 | 142 | +67 |
| MM early fusion | 190 | 124 | +66 |

These findings demonstrate that overall classification improvements can coexist with new errors in particular groups of recordings.

The analysis identifies associations rather than causal effects.

## 5. Supplementary Experiments and Robustness

We conducted additional experiments to evaluate the reliability and limitations of the main results.

### Class-Weighted Neural Networks

We compared weighted and unweighted cross-entropy training using the original CNN and MM architectures.

Class weighting improved balanced accuracy:

| Representation | Unweighted | Weighted |
|---|---:|---:|
| CNN NN | 67.90% | 70.24% |
| MM NN | 68.31% | 70.93% |

However, the weighted models achieved lower validation macro-F1 than their unweighted counterparts.

The original class-weighting comparison uses random seed 42. A separate verification notebook reconstructed the experiment and reproduced the reported metrics.

### Candidate Duplicate-Artwork Sensitivity

A perceptual-hash analysis identified five candidate near-duplicate training-test artwork pairs involving four unique test recordings.

After excluding these four test recordings without retraining:

| Representation | Original gain | Gain after exclusion |
|---|---:|---:|
| CNN | +3.74 pp | +3.64 pp |
| MM | +3.68 pp | +3.58 pp |

The main visual-typicality pattern remained present.

This check is not an exhaustive search for every possible visual overlap.

### Alternative Visual-Typicality Representation

To assess sensitivity to the original CNN-based grouping, we repeated the visual-typicality analysis using MM visual embeddings.

The qualitative pattern persisted for both classifiers: positive gains in the most typical group and substantial negative gains in the most atypical group.

Only 54.19% of test recordings retained exactly the same quintile assignment across the two definitions.

Both analyses remain retrospective and use true genre labels. The representations also share an underlying feature-extraction history.

### Neural-Network Stability Across Seeds

We evaluated the unweighted CNN and MM neural networks using five predefined seeds: 42, 43, 44, 45, and 46.

| Test metric | CNN mean ± SD | MM mean ± SD |
|---|---:|---:|
| Accuracy | 68.94 ± 0.61% | 69.44 ± 0.56% |
| Balanced accuracy | 67.98 ± 0.28% | 68.37 ± 0.41% |
| Macro-F1 | 68.30 ± 0.43% | 68.76 ± 0.45% |

The experiments showed relatively modest performance variation.

The original seed-42 CNN and MM predictions were reproduced exactly in the recorded Tesla T4 environment.

These results measure sensitivity to training randomness using one fixed experimental partition. They do not establish generalization across independent datasets.

## 6. Repository Structure

### Core Experiment Notebooks

The five organized experiment notebooks cover the main research workflow.

| Notebook | Purpose |
|---|---|
| [Stage 1 — Classification and Fusion](notebooks/beyond_audio_final_experiments_stage1.ipynb) | Logistic regression, audio-only baselines, early fusion, fixed late fusion, and adaptive late fusion |
| [Stage 2 — Neural Networks](notebooks/beyond_audio_final_experiments_stage2.ipynb) | CNN/MM neural-network training and evaluation |
| [Stage 3 — Visual Typicality](notebooks/beyond_audio_final_experiments_stage3.ipynb) | Quintile construction, rescued/harmed predictions, and artist-cluster bootstrap |
| [Stage 4 — Artwork Robustness](notebooks/beyond_audio_final_experiments_stage4.ipynb) | Partition-integrity and candidate duplicate-artwork sensitivity checks |
| [Stage 5 — Results Verification](notebooks/beyond_audio_final_experiments_stage5.ipynb) | Verification of reported classification results, supplementary tables, and figures |

### Supplementary Verification Notebooks

These notebooks support additional experiments reported in the appendices.

| Notebook | Purpose |
|---|---|
| [Weighted Neural-Network Verification](notebooks/weighted_nn_verification_colab.ipynb) | Reconstructed class-weighted cross-entropy experiment; supports Appendix B |
| [Alternative Visual-Typicality Analysis](notebooks/mm_visual_typicality_robustness_colab.ipynb) | MM-embedding-based typicality robustness; supports Appendix D |
| [Multi-Seed Neural-Network Stability](notebooks/beyond_audio_multiseed_nn_stability_colab.ipynb) | Five-seed analysis of the unweighted CNN and MM neural networks; supports Appendix D |

### Original Development Notebook

[Original Colab Development](notebooks/beyond_audio_colab_development.ipynb)

This notebook documents the exploratory development of the project.

It includes preliminary experiments, intermediate analyses, and approaches that were subsequently revised or superseded.

**For the final reported experiments, use the organized notebooks listed above.**

The development notebook is retained to preserve research history and should not be interpreted as a standalone final reproduction pipeline.

### Documentation

| File | Purpose |
|---|---|
| [REPRODUCE.md](REPRODUCE.md) | Required inputs, execution order, and environment guidance |
| [REPRODUCIBILITY_VERIFICATION.md](REPRODUCIBILITY_VERIFICATION.md) | Recorded verification checks and their limitations |
| [Experiment Index](notebooks/EXPERIMENT_INDEX.md) | Relationship between the development work, final notebooks, and report results |
| [Data and Reproducibility Guide](notebooks/DATA_AND_REPRODUCIBILITY.md) | Dataset provenance and intermediate-file descriptions |
| [Notebook README](notebooks/README.md) | Notebook organization and usage |
| [requirements.txt](notebooks/requirements.txt) | Python dependencies; versions are not pinned |

## 7. Running the Experiments

The experiments were developed using Google Colab and project data stored in Google Drive.

### Required Data

The repository does not contain:

- The original MSD-I feature archive
- Prepared CNN and MM feature matrices
- Original metadata and saved predictions
- Model checkpoints
- Perceptual-hash candidate files
- Selected intermediate research outputs

These files must be obtained separately from authorized sources or the team's existing project workspace.

The default project location in Google Colab is:

`/content/drive/MyDrive/MSD-I_project`

This can be changed through the `MSDI_PROJECT_DIR` environment variable.

### Execution Instructions

1. Read [REPRODUCE.md](REPRODUCE.md) to identify the required external files and directories.
2. Obtain authorized access to the prepared experimental data and relevant saved research artifacts.
3. Open the required notebook in Google Colab and mount Google Drive.
4. Verify the input paths and installed dependencies.
5. Run the five organized experiment notebooks, respecting their documented dependencies.
6. Run the supplementary verification notebooks for the weighted neural networks, alternative visual typicality, and multi-seed stability analyses.
7. Compare the resulting metrics with the report and the recorded verification evidence.

The notebooks save new experimental outputs to designated directories within the external project workspace.

**Reproducibility boundary:** The repository provides documented, data-dependent reconstruction and verification workflows. It is not a self-contained, automated raw-data-to-paper pipeline.

Stages 3–5 rely on original saved predictions and other intermediate artifacts. The artwork-overlap experiment uses saved perceptual-hash candidates rather than independently regenerating all hashes.

The three supplementary notebooks contain executable code, but their GitHub copies do not currently include saved execution outputs.

## 8. Verification Status and Limitations

The main verification work includes:

- Checking dataset partition integrity and feature alignment
- Recalculating classification metrics
- Reproducing original neural-network predictions
- Reconstructing CNN-based visual-typicality quintiles
- Recalculating rescued/harmed transitions
- Computing artist-cluster bootstrap estimates
- Checking identified artwork-overlap candidates
- Verifying figure data and supplementary tables
- Reconstructing weighted neural-network results
- Testing alternative MM-based visual-typicality groupings
- Evaluating neural-network sensitivity across five seeds

The verification experiments use the existing research dataset and artifacts.

They do not constitute independent evaluation on a new dataset.

A fully independent clean-environment execution from the original source archive to every final research output has not been established.

In addition, the visual-typicality findings remain exploratory because their grouping definitions require true genre labels and test-set rankings.

For detailed evidence concerning the five original stages, see [REPRODUCIBILITY_VERIFICATION.md](REPRODUCIBILITY_VERIFICATION.md). The supplementary notebooks document the additional experiment procedures.

## 9. Development History and Attribution

Project development began in Google Colab before the GitHub repository was created.

Generative AI tools, including ChatGPT, assisted with aspects of coding, debugging, analysis, and documentation.

## 10. References

Oramas, S., Barbieri, F., Nieto, O., & Serra, X. (2018).

*Multimodal Deep Learning for Music Genre Classification.*

Transactions of the International Society for Music Information Retrieval, 1(1), 4–21.

https://doi.org/10.5334/tismir.10


## Reference

Oramas, S., Barbieri, F., Nieto, O., & Serra, X. (2018). *Multimodal Deep Learning for Music Genre Classification*. *Transactions of the International Society for Music Information Retrieval*, 1(1), 4–21. https://doi.org/10.5334/tismir.10
