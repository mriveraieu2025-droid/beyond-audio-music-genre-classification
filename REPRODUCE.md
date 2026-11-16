# Reproducing the Experiments

## Scope and current verification status

This repository contains five staged Google Colab notebooks that reconstruct and check the experiments reported in **Beyond Audio: Music Genre Classification**, together with the original exploratory development notebook.

The team has run the staged workflow using the **existing MSD-I project files and experimental artifacts** in Google Drive. The saved notebook outputs and [reproducibility verification record](REPRODUCIBILITY_VERIFICATION.md) document the resulting checks, including neural-network prediction-level reproduction and downstream numerical consistency.

**Important distinction:** This is a documented, data-dependent reconstruction and verification workflow, **not** a self-contained pipeline that starts with raw MSD-I data and automatically regenerates every research artifact. A fully independent execution in a newly provisioned environment remains unverified.

## 1. Environment

The experiments were developed in **Google Colab**. Recorded environments include:

| Workflow | Recorded environment |
|---|---|
| Stage 1 and Stages 3–5 | Python 3.13.18, NumPy 2.4.6, pandas 2.2.3, scikit-learn 1.6.1 |
| Successful Stage 2 Tesla T4 reproduction | Python 3.13.16, PyTorch 2.14.1+cu130, NVIDIA Tesla T4 GPU |
| Earlier Stage 2 CPU execution | PyTorch 2.14.1+cpu (the precise execution conditions differ) |

These are **recorded versions**, not a lockfile or a guarantee of identical behavior across hardware. A separate CPU run produced different neural-network training results; the cause was not isolated.

The list of dependency names is in [`notebooks/requirements.txt`](notebooks/requirements.txt). A conventional installation command is:

```bash
python -m pip install -r notebooks/requirements.txt
```

The requirements are not version-pinned. In Colab, confirm the installed PyTorch/CUDA build before changing the preinstalled GPU packages; an unrestricted installation does not necessarily reproduce the verified Tesla T4 environment.

## 2. Data access and directory setup

The repository **does not include** the original MSD-I feature archive, prepared feature matrices, labels, research checkpoints, saved predictions, artwork metadata, or perceptual-hash candidate files. These must be obtained separately from authorized sources or the team's existing research workspace, subject to the applicable distribution terms.

By default, the notebooks expect the project root:

```text
/content/drive/MyDrive/MSD-I_project
```

They also honor the environment variable `MSDI_PROJECT_DIR` if the project files are elsewhere. Within the project root, the main directories are:

```text
finetuning_experiments/
    multimodal_artist_disjoint/       # CNN representations, labels and saved CNN results
    oramas_representation_experiment/ # MM representations, labels and saved representation results
    neural_network_experiments/       # Original NN artifacts and predictions
    reproducible_stage1_2026/        # Stage 1 outputs
    reproducible_stage2_2026/        # Stage 2 outputs
    reproducible_stage3_2026/        # Stage 3 outputs
    reproducible_stage4_2026/        # Stage 4 outputs
    reproducible_stage5_2026/        # Stage 5 outputs
```

### Prepared feature and label files

For each split `train`, `val`, and `test`, Stage 1 expects the following inside `multimodal_artist_disjoint/`:

- `labels_<split>.csv`
- `X_<split>_audio.npy` (CNN audio, 2,048 features)
- `X_<split>_visual.npy` (CNN visual, 2,048 features)

Inside `oramas_representation_experiment/`, it expects:

- `labels_<split>.csv`
- `X_<split>_mm-audio.npy` (MM audio, 200 features)
- `X_<split>_mm-visual.npy` (MM visual, 200 features)

The feature rows and metadata must correspond to the original **artist-disjoint** experimental splits: 8,242 training, 1,836 validation, and 1,792 test recordings, covering six genres. The notebooks contain checks for shapes, identifiers, and alignment.

Stage-specific saved predictions, original checkpoints, additional metadata, and intermediate analysis files are described below. The [data and reproducibility guide](notebooks/DATA_AND_REPRODUCIBILITY.md) gives further provenance information.

## 3. Execution order and dependencies

Open each notebook from the [`notebooks/` directory](notebooks/) in Google Colab. Mount the authorized Google Drive workspace, check the project root, and execute its cells in order. Each notebook checks for some of its required input files before proceeding.

**Do not assume that the output of one fresh run automatically replaces the original frozen predictions used by later verification stages.** These stages intentionally preserve comparison with the original research results.

| Stage | Notebook | Main function | Important dependencies |
|---|---|---|---|
| **1 — Logistic regression** | [Stage 1](notebooks/beyond_audio_final_experiments_stage1.ipynb) | Train CNN/MM baselines and early/late fusion models; tune on validation; compare test metrics and selected saved checkpoints | Prepared CNN/MM features and labels; original prediction/checkpoint files for later comparison cells |
| **2 — Neural networks** | [Stage 2](notebooks/beyond_audio_final_experiments_stage2.ipynb) | Train CNN and MM fusion MLPs; choose checkpoints using validation macro-F1; compare validation/test predictions | Prepared CNN/MM features and labels; original neural-network checkpoints and scalers for original-vs-new comparisons |
| **3 — Visual typicality** | [Stage 3](notebooks/beyond_audio_final_experiments_stage3.ipynb) | Reconstruct typicality quintiles, rescue/harm transitions, and artist-cluster bootstrap intervals | Original saved CNN/MM test predictions, CNN visual features, labels; optional original typicality/bootstrap CSVs for cross-checking |
| **4 — Artwork robustness** | [Stage 4](notebooks/beyond_audio_final_experiments_stage4.ipynb) | Check metadata, artwork candidate overlaps, and sensitivity after excluding flagged recordings | Original MSD-I metadata, clean album metadata, saved perceptual hashes and candidate pairs, labels, and Stage 3 paired predictions |
| **5 — Tables and figures** | [Stage 5](notebooks/beyond_audio_final_experiments_stage5.ipynb) | Recalculate reported metrics, genre F1, transitions, and figure data | Original frozen CNN/MM/NN predictions plus verified Stage 3 and Stage 4 analysis outputs |

### Stage 1 — Logistic regression and fusion

- Validates the splits and all four embedding matrices.
- Selects logistic-regression regularization parameters by **validation macro-F1**.
- Selects fixed and adaptive late-fusion settings on the validation partition.
- Evaluates the held-out test set and saves new models, predictions, and metrics under `finetuning_experiments/reproducible_stage1_2026/`.
- Later diagnostic cells compare certain newly fitted models and predictions with original saved artifacts. These comparisons require the original files to be present.

### Stage 2 — Neural networks

- Uses training-only standardization and validation-selected checkpoints for CNN (4,096-dimensional early fusion) and MM (400-dimensional early fusion).
- Saves trained models, scalers, histories, predictions, and evaluation results under `finetuning_experiments/reproducible_stage2_2026/`.
- Comparison cells use original checkpoints in `finetuning_experiments/neural_network_experiments/`.
- The documented **Tesla T4 reproduction** selected epochs **5 (CNN)** and **28 (MM)** and matched original class predictions for **all 1,836 validation and 1,792 test recordings** for both models. This is prediction agreement, not a claim that every model parameter or probability must match in every environment.

### Stage 3 — Visual typicality and bootstrap

- Uses the **original saved test predictions**, not automatically the newly fitted Stage 1 predictions.
- Recomputes cosine distances to true-genre training centroids and genre-relative typicality quintiles.
- Recalculates rescued/harmed counts and artist-cluster bootstrap intervals (3,000 resamples).
- Saves outputs under `finetuning_experiments/reproducible_stage3_2026/`.

The typicality analysis uses the **true test genres**, so it is retrospective and exploratory; its grouping procedure is not a deployable inference-time method.

### Stage 4 — Data integrity and near-duplicate sensitivity

In addition to the stage directories, this notebook requires these files at the project root:

```text
audio_features/MSD-I_dataset.tsv
clean_album_metadata.csv
album_perceptual_hashes.csv
near_duplicate_candidates.csv
```

It also requires `reproducible_stage3_2026/stage3_paired_test_predictions_and_typicality.csv` under `finetuning_experiments/`.

The Stage 4 checks use **saved perceptual-hash candidates**: the original hash generation is not rerun, and the candidate list is not an exhaustive duplicate-artwork search. The recorded sensitivity excludes four flagged test recordings; it does not retrain the classifiers.

### Stage 5 — Paper tables and figures

- Reads **original frozen CNN/MM/NN prediction files**, plus Stage 3 prediction/transition/bootstrap outputs and Stage 4 sensitivity outputs.
- Recalculates metrics for the ten reported model configurations, genre-specific F1 scores, and additional fusion-analysis tables.
- Generates supplementary figure files and tables under `finetuning_experiments/reproducible_stage5_2026/`.
- Checks six illustrative examples against saved metadata; the correspondence of artwork images to original source URLs was not independently reverified.

## 4. Recorded consistency checks

The experiment record includes:

| Check | Recorded result |
|---|---|
| Split sizes | 8,242 train / 1,836 validation / 1,792 test |
| CNN audio-only test accuracy | 65.90% |
| CNN early-fusion test accuracy | 69.64% |
| MM audio-only test accuracy | 66.52% |
| MM early-fusion test accuracy | 70.20% |
| CNN audio-to-fusion transitions | 209 rescued / 142 harmed |
| MM audio-to-fusion transitions | 190 rescued / 124 harmed |
| Visual-typicality assignments reconstructed | All 1,792 matched saved assignments |
| CNN gain excluding four flagged test recordings | +3.64 percentage points |

These results are checks **against the team's existing project artifacts**, not evidence of performance on an independent dataset. Consult [`REPRODUCIBILITY_VERIFICATION.md`](REPRODUCIBILITY_VERIFICATION.md) for the documented execution environments, bootstrap comparisons, numerical checks, and caveats.

## 5. Important limitations and practical notes

1. **External inputs are essential.** Cloning the repository and installing the dependency names alone is insufficient to execute the full analyses.
2. **Stages are not a single automated pipeline.** Stage 3 and Stage 5 intentionally use original saved model predictions; Stage 4 uses saved hash candidates and Stage 3 outputs.
3. **Environment differences matter.** The Tesla T4 configuration reproduced the original neural-network class predictions, but other CPU execution conditions gave different outcomes. Exact reproducibility across hardware and package versions is not guaranteed.
4. **Existing files should be backed up before rerunning.** The notebooks save into dedicated `reproducible_stage*_2026/` directories, but running a notebook again can overwrite files in its own output directory.
5. **The audit is limited in scope.** It does not independently regenerate the original embeddings or perceptual hashes, certify all possible image overlaps, or independently validate the exploratory typicality findings.

## 6. Independent reproduction procedure

For an additional clean-runtime verification:

1. Obtain authorized access to the required **data and original research artifacts**.
2. Open the notebooks in a new Colab session and configure `MSDI_PROJECT_DIR` if needed.
3. Record Python, package, hardware, and CUDA versions before training.
4. Run the stages in order, distinguishing original saved inputs from newly generated outputs.
5. Compare the results with [`REPRODUCIBILITY_VERIFICATION.md`](REPRODUCIBILITY_VERIFICATION.md), preserving any discrepancies and full error logs.
6. Record genuine new verification work in the repository without backdating or inventing historical runs.

The existing documented verification is meaningful, but **full clean-environment, raw-data-to-paper reproduction has not been established**.
