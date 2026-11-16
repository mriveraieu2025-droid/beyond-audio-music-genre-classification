# Data and Reproducibility Guide

This document describes the **dataset provenance, prepared features, intermediate files, and verification status** of the *Beyond Audio: Music Genre Classification* project.

For practical setup and execution instructions, see [REPRODUCE.md](../REPRODUCE.md). For recorded numerical checks, see [REPRODUCIBILITY_VERIFICATION.md](../REPRODUCIBILITY_VERIFICATION.md). The [experiment index](EXPERIMENT_INDEX.md) explains the relationship between exploratory development work and the final experiments.

## 1. Dataset and experimental partitions

The experiments use a six-genre subset of **MSD-I**, associated with Oramas et al. (2018). The genres are Electronic, Jazz, Metal, Pop, Rap, and Rock.

| Partition | Recordings |
|---|---:|
| Training | 8,242 |
| Validation | 1,836 |
| Test | 1,792 |
| **Total** | **11,870** |

The original experimental partitions, restricted to these six genres, are **artist-disjoint**. The recorded verification covered 11,870 recordings from 6,461 unique artists, including 975 artists in the held-out test partition.

The development process also produced a project-specific file, `final_modeling_dataset.csv`. **Its earlier split assignments are not the final experimental partitions** used for the reported analyses. The final notebooks use the labels derived from the original MSD-I partitions.

## 2. Original dataset files and availability

The original Google Drive workspace includes these source resources:

```text
audio_features/msdi-data.tar.gz
audio_features/MSD-I_dataset.tsv
```

The archive provides the precomputed representations and track indices used in the experiments. The metadata TSV supports identification, genre alignment, and integrity checks.

Additional project-specific files were created or retained during development. They include `final_modeling_dataset.csv` and, for the artwork audit:

```text
clean_album_metadata.csv
album_perceptual_hashes.csv
near_duplicate_candidates.csv
```

**These dataset files and research artifacts are not distributed in this GitHub repository.** Authorized access to the original files or the team's prepared workspace is required. The user of the files is responsible for following the original dataset's distribution terms.

The five staged notebooks train **downstream classifiers on precomputed embeddings**. They do not retrain the original audio or visual feature-extraction networks.

## 3. Prepared representation files

Four representations are used:

| Representation | Dimensions per recording |
|---|---:|
| CNN audio | 2,048 |
| CNN visual | 2,048 |
| MM audio | 200 |
| MM visual | 200 |

The default project root in Google Colab is:

```text
/content/drive/MyDrive/MSD-I_project
```

It can be overridden by setting the environment variable `MSDI_PROJECT_DIR`, as documented in [REPRODUCE.md](../REPRODUCE.md).

For each partition name `train`, `val`, and `test`, the prepared files are organized as follows:

**CNN representations** — `finetuning_experiments/multimodal_artist_disjoint/`

```text
labels_train.csv
labels_val.csv
labels_test.csv
X_train_audio.npy
X_val_audio.npy
X_test_audio.npy
X_train_visual.npy
X_val_visual.npy
X_test_visual.npy
```

**MM representations** — `finetuning_experiments/oramas_representation_experiment/`

```text
labels_train.csv
labels_val.csv
labels_test.csv
X_train_mm-audio.npy
X_val_mm-audio.npy
X_test_mm-audio.npy
X_train_mm-visual.npy
X_val_mm-visual.npy
X_test_mm-visual.npy
```

These are **prepared experimental artifacts**, not additional source datasets. Matching track identifiers, label order, matrix shapes, and feature-row alignment are essential; the Stage 1 and Stage 2 notebooks check these conditions. The original MSD-I split assignments, not the earlier development CSV assignments, are authoritative for the reported experiments.

## 4. Original models and analysis artifacts

The original research workflow saved downstream models as scikit-learn `.joblib` objects and neural-network `.pt` checkpoints, along with prediction and metric tables.

Files referenced by the final notebooks include:

| Location | Examples | Purpose |
|---|---|---|
| `multimodal_artist_disjoint/` | `final_test_metrics.csv`, `final_test_predictions.csv`, `final_test_typicality_details.csv` | CNN classification and original typicality comparisons |
| `oramas_representation_experiment/` | `representation_final_test_metrics.csv`, `representation_final_test_predictions.csv` | MM and combined-representation classification comparisons |
| `neural_network_experiments/` | `neural_network_test_predictions.csv`, original `.pt` checkpoints and scalers | Neural-network comparison and reproduction |
| `phase3_uncertainty/` | `overall_fusion_gain_cluster_bootstrap.csv`, `quintile_fusion_gain_cluster_bootstrap.csv`, `q1_q5_fusion_gain_contrasts.csv` | Original uncertainty estimates for cross-checking |
| `phase4_album_examples/` | `final_report_album_examples.csv` | Six illustrative recording selections |
| `final_report_figures/` | `figure3_album_cover_examples.pdf` | Existing album-cover example figure |

All paths in this table are relative to `finetuning_experiments/` under the Google Drive project root. Some original files serve as comparison references and may be optional in specific notebook cells; other saved inputs are required for the later stages.

The original exploratory [development notebook](beyond_audio_colab_development.ipynb) contains preliminary, corrected, and final experiment sequences. It should not be treated as a guaranteed clean, top-to-bottom production pipeline.

## 5. Five-stage reconstruction and generated outputs

The repository includes five organized experiment notebooks. Their outputs are written to dedicated directories under `finetuning_experiments/` rather than intentionally replacing the original research artifacts.

| Notebook | Output directory | Main reconstructed or verified results |
|---|---|---|
| [Stage 1 — Logistic regression and fusion](beyond_audio_final_experiments_stage1.ipynb) | `reproducible_stage1_2026/` | Models, validation grids, test metrics and predictions |
| [Stage 2 — Neural networks](beyond_audio_final_experiments_stage2.ipynb) | `reproducible_stage2_2026/` | CNN/MM checkpoints, scalers, training histories, test metrics and predictions |
| [Stage 3 — Visual typicality](beyond_audio_final_experiments_stage3.ipynb) | `reproducible_stage3_2026/` | Reconstructed typicality assignments, prediction transitions and artist-cluster bootstrap estimates |
| [Stage 4 — Artwork robustness](beyond_audio_final_experiments_stage4.ipynb) | `reproducible_stage4_2026/` | Split-integrity checks, candidate artwork-pair mapping and exclusion sensitivity |
| [Stage 5 — Figures and tables](beyond_audio_final_experiments_stage5.ipynb) | `reproducible_stage5_2026/` | Paper-to-prediction metric comparisons, genre F1 and regenerated numerical figures |

**Data dependency boundary:** Stage 3 uses **original saved CNN/MM test predictions**, not automatically the newly generated Stage 1 predictions. Stage 4 uses the saved perceptual-hash candidates and Stage 3 outputs. Stage 5 uses original frozen model predictions plus verified Stage 3 and Stage 4 outputs. Thus, the stages document reproducibility and internal consistency but do not form an entirely self-contained raw-data-to-paper pipeline.

Rerunning the notebooks can overwrite files in their own `reproducible_stage*_2026/` output directories. Back up verified outputs before re-execution if they need to be preserved.

## 6. Recorded data-integrity and result verification

The team's documented Google Colab executions verified the following against the **existing project data and saved artifacts**:

- Original artist-disjoint split membership and recording counts, with no exact album-index or image-URL overlaps across the evaluated partitions.
- Consistent CNN/MM feature alignment and test prediction identifiers.
- The CNN and MM neural-network reproduction in a documented **NVIDIA Tesla T4** environment, matching the original **class predictions** for all 1,836 validation and 1,792 test recordings.
- Reconstruction of all 1,792 retrospective visual-typicality assignments and the CNN/MM rescued and harmed prediction counts.
- Reconstruction of artist-cluster bootstrap estimates and confidence intervals using 3,000 artist-level resamples.
- Identification of five candidate cross-partition near-duplicate artwork pairs involving four test recordings, based on the **saved** perceptual-hash candidate list. Excluding the four flagged test recordings changed the CNN overall early-fusion gain from +3.74 to +3.64 percentage points.
- Numerical comparisons of the ten model configurations, genre-specific F1 values, supplementary tables, and figure data against the original research predictions.

These statements describe **recorded verification**, not an independently executed audit on a new dataset. The visual-typicality analysis uses *true test genres* and is retrospective and exploratory. The candidate hash list was not independently regenerated or shown to identify every possible visually similar artwork pair. The selected album-cover examples were checked against stored track metadata; their correspondence to original image URLs was not independently reverified.

More detailed checks and execution environments are recorded in [REPRODUCIBILITY_VERIFICATION.md](../REPRODUCIBILITY_VERIFICATION.md).

## 7. Software environment and reproduction limits

The working environment was Google Colab with data in Google Drive. Recorded versions for Stage 1 and Stages 3–5 include Python 3.13.18, NumPy 2.4.6, pandas 2.2.3, and scikit-learn 1.6.1. A successful Stage 2 GPU reproduction recorded Python 3.13.16, PyTorch 2.14.1+cu130, and an NVIDIA Tesla T4.

Dependency names are listed in [`requirements.txt`](requirements.txt); **versions are not pinned**. Other hardware and software combinations may not reproduce neural-network training exactly. In particular, separate CPU executions produced different training outcomes, although their precise cause was not isolated.

An **independent clean-environment execution with all authorized original inputs** remains outstanding. The upstream feature-extraction models and original perceptual-hash generation are not reconstructed by the five staged notebooks.

For step-by-step execution guidance, including which saved files must be available, follow [REPRODUCE.md](../REPRODUCE.md).

## 8. Development history and attribution

The project was initially developed in Google Colab before the GitHub repository was created. The original development notebook is preserved as a record of exploratory experiments, including work that was later corrected or superseded. Repository commits document subsequent GitHub changes, not the full earlier research timeline.

Generative AI tools, including ChatGPT, assisted with aspects of coding, debugging, analysis, and documentation. The project team is responsible for reviewing, understanding, and verifying the submitted work.

## Reference

Oramas, S., Barbieri, F., Nieto, O., & Serra, X. (2018). *Multimodal Deep Learning for Music Genre Classification*. *Transactions of the International Society for Music Information Retrieval*, 1(1), 4–21. https://doi.org/10.5334/tismir.10
