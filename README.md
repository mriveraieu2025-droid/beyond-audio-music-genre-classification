# Beyond Audio: Music Genre Classification

**Machine Learning Final Project — November 2026**  
**Authors:** Carlos Saldaña, Ana Santos, and Maria Rivera

## Overview

This project investigates how album-cover images influence music genre classification, and when combining visual and audio information improves or worsens predictions.

We compare audio-only, visual-only, and multimodal classifiers using precomputed audio and album-cover embeddings from the MSD-I dataset. Alongside overall performance, we examine prediction-level changes, genre-relative visual typicality, uncertainty, and potential artwork overlap.

**Research question:** How do album-cover images influence music genre classification, and when does combining visual and audio information improve or worsen predictions?

## Dataset and representations

The experiments use **11,870 recordings** across six genres: Electronic, Jazz, Metal, Pop, Rap, and Rock. We preserve the original artist-disjoint partitions.

| Partition | Recordings |
|---|---:|
| Training | 8,242 |
| Validation | 1,836 |
| Test | 1,792 |
| **Total** | **11,870** |

We use four **precomputed** representations associated with Oramas et al. (2018):

| Representation | Audio dimensions | Visual dimensions |
|---|---:|---:|
| CNN | 2,048 | 2,048 |
| MM | 200 | 200 |

The project trains downstream classifiers on these embeddings; it does **not** train the original feature-extraction networks.

## Methods

- **Classifiers:** multiclass logistic regression and feedforward neural networks.
- **Inputs:** audio-only, visual-only, CNN early fusion, MM early fusion, and a combination of all four embeddings.
- **Additional fusion methods:** fixed late fusion and confidence-adaptive late fusion.
- **Model selection:** validation macro-F1, with preprocessing fitted on training data only.
- **Evaluation:** held-out test accuracy, balanced accuracy, macro-F1, prediction transitions, and artist-cluster bootstrap confidence intervals.
- **Exploratory analysis:** genre-relative visual typicality, computed using true genre labels and training-set visual centroids.
- **Robustness:** split-integrity checks and sensitivity to identified candidate near-duplicate artwork.

## Main findings

| Comparison | Audio-only accuracy | Early-fusion accuracy | Gain |
|---|---:|---:|---:|
| CNN logistic regression | 65.90% | 69.64% | +3.74 percentage points |
| MM logistic regression | 66.52% | **70.20%** | +3.68 percentage points |

The highest observed test accuracy is **70.20%**, achieved by MM early-fusion logistic regression.

Fusion performance differs substantially across retrospective visual-typicality groups. In the most visually typical group (Q1), both CNN and MM early fusion improve accuracy by **17.45 percentage points** over their corresponding audio-only baselines. In the least visually typical group (Q5), the respective changes are **−19.72** and **−16.94 percentage points**.

**Interpretation:** Visual typicality is defined using the *true* genre, so this subgroup analysis is retrospective, exploratory, and not directly available as an inference-time reliability signal. These associations do not establish that specific artwork characteristics cause classification outcomes.

## Repository contents

| Path | Purpose |
|---|---|
| [`notebooks/beyond_audio_final_experiments_stage1.ipynb`](notebooks/beyond_audio_final_experiments_stage1.ipynb) | Logistic regression, baseline models, and fusion comparisons |
| [`notebooks/beyond_audio_final_experiments_stage2.ipynb`](notebooks/beyond_audio_final_experiments_stage2.ipynb) | CNN and MM neural-network training and checkpoint comparisons |
| [`notebooks/beyond_audio_final_experiments_stage3.ipynb`](notebooks/beyond_audio_final_experiments_stage3.ipynb) | Visual typicality, prediction transitions, and artist-cluster bootstrap |
| [`notebooks/beyond_audio_final_experiments_stage4.ipynb`](notebooks/beyond_audio_final_experiments_stage4.ipynb) | Data-integrity and candidate duplicate-artwork sensitivity checks |
| [`notebooks/beyond_audio_final_experiments_stage5.ipynb`](notebooks/beyond_audio_final_experiments_stage5.ipynb) | Verification of paper tables and reconstruction of figures |
| [`notebooks/beyond_audio_colab_development.ipynb`](notebooks/beyond_audio_colab_development.ipynb) | Original development record, including exploratory and superseded experiments |
| [`notebooks/requirements.txt`](notebooks/requirements.txt) | Python dependency names (not a fully pinned environment) |
| [`REPRODUCE.md`](REPRODUCE.md) | Required inputs, environment notes, execution order, and limitations |
| [`REPRODUCIBILITY_VERIFICATION.md`](REPRODUCIBILITY_VERIFICATION.md) | Recorded checks and reproduction results for the five stages |
| [`notebooks/DATA_AND_REPRODUCIBILITY.md`](notebooks/DATA_AND_REPRODUCIBILITY.md) | Dataset and intermediate-file provenance |
| [`notebooks/EXPERIMENT_INDEX.md`](notebooks/EXPERIMENT_INDEX.md) | Guide from original development work to final experiments |

## Running the experiments

The notebooks were developed and verified in **Google Colab**, using data stored separately in Google Drive. The original MSD-I feature archive, prepared matrices, metadata, saved model predictions, and selected intermediate outputs are **not included** in this repository.

1. Read [`REPRODUCE.md`](REPRODUCE.md) for the required files and stage dependencies.
2. Obtain authorized access to the MSD-I inputs and the project-specific experimental artifacts.
3. Configure `MSDI_PROJECT_DIR` or use the notebooks' default Google Drive project directory (`/content/drive/MyDrive/MSD-I_project`).
4. Install appropriate dependencies from [`notebooks/requirements.txt`](notebooks/requirements.txt), noting that package names are not version-pinned.
5. Run the five staged notebooks in order and compare outputs with [`REPRODUCIBILITY_VERIFICATION.md`](REPRODUCIBILITY_VERIFICATION.md).

**Important:** This is a documented reconstruction and verification workflow, **not** a fully self-contained, automated raw-data-to-paper pipeline. In particular, Stages 3–5 depend on original saved predictions and other intermediate artifacts; perceptual hashes were not regenerated independently.

## Verification status and limitations

The team's recorded Google Colab checks include:

- Recalculation and comparison of the reported classification metrics.
- Reproduction of the CNN and MM neural-network **class predictions** for all 1,836 validation and 1,792 test recordings in a documented NVIDIA Tesla T4 environment.
- Reconstruction of all 1,792 visual-typicality assignments, prediction transitions, and artist-cluster bootstrap estimates.
- Checks of dataset partition integrity and sensitivity to four test recordings flagged by the saved near-duplicate candidate list.
- Numerical consistency checks for the main results tables and research figures using saved predictions.

These checks use the **existing research dataset and artifacts**. They do not constitute independent validation on new data, and a fully independent clean-environment execution remains outstanding. Neural-network training results can vary with execution conditions. The underlying artwork URLs for the selected qualitative examples were not independently reverified.

For the detailed evidence and boundaries of each claim, see [`REPRODUCIBILITY_VERIFICATION.md`](REPRODUCIBILITY_VERIFICATION.md).

## Development history and AI assistance

The work began in Google Colab before this repository was created. The development notebook includes exploratory experiments alongside the final analyses; GitHub commit history documents subsequent repository changes, not the full earlier research timeline.

Generative AI tools, including ChatGPT, assisted with aspects of coding, debugging, analysis, and documentation. The project team is responsible for understanding, reviewing, verifying, and explaining the submitted methods, code, and results.

## Reference

Oramas, S., Barbieri, F., Nieto, O., & Serra, X. (2018). *Multimodal Deep Learning for Music Genre Classification*. *Transactions of the International Society for Music Information Retrieval*, 1(1), 4–21. https://doi.org/10.5334/tismir.10
