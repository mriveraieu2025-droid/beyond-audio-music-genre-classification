
# Reproducing the Experiments

**Project:** Beyond Audio: Music Genre Classification  
**Machine Learning Final Project — IE University, November 2026**

## 1. Purpose and Scope

This document explains how to execute the experiments reported in *Beyond Audio: Music Genre Classification*.

The repository contains:

- Five core experiment and verification notebooks.
- Three supplementary verification notebooks.
- One original exploratory development notebook.
- Documentation of dataset provenance, experimental results, and reproducibility limitations.

The five core notebooks cover classification, fusion, neural networks, visual typicality, statistical analysis, artwork sensitivity, and research figures.

The supplementary notebooks investigate:

1. Class-weighted neural networks.
2. Alternative MM-based visual typicality.
3. Neural-network stability across five random seeds.

**Important:** The repository provides documented reconstruction and verification workflows using the team's existing research data.

It is not a self-contained pipeline that automatically regenerates every result directly from the original MSD-I archive.

Some notebooks require original saved predictions, trained model artifacts, or intermediate files.

## 2. Software Environment

The experiments were developed using Google Colab and Python.

### Recorded Environments

| Workflow | Recorded environment |
|---|---|
| Stage 1 and Stages 3–5 | Python 3.13.18, NumPy 2.4.6, pandas 2.2.3, scikit-learn 1.6.1 |
| Successful neural-network reproduction | Python 3.13.16, PyTorch 2.14.1+cu130, NVIDIA Tesla T4 |
| Five-seed neural-network analysis | Python 3.13.16, PyTorch 2.14.1+cu130, NVIDIA Tesla T4 |

The recorded software versions describe environments used during verification. They do not constitute a fully pinned installation.

Python dependencies are listed in:

[notebooks/requirements.txt](notebooks/requirements.txt)

Install them, where appropriate, using:

```bash
python -m pip install -r notebooks/requirements.txt
```

In Google Colab, inspect the installed PyTorch and CUDA versions before modifying GPU-related packages.

Different hardware or software environments may produce different neural-network training outcomes.

The successful Tesla T4 configuration reproduced the original seed-42 neural-network class predictions, but exact reproduction across all environments is not guaranteed.

## 3. Required Data

### Dataset

The experiments use a six-genre subset of MSD-I.

The dataset contains 11,870 recordings across Electronic, Jazz, Metal, Pop, Rap, and Rock.

The original artist-disjoint experimental partitions are preserved.

| Partition | Recordings |
|---|---:|
| Training | 8,242 |
| Validation | 1,836 |
| Test | 1,792 |
| Total | 11,870 |

### Feature Representations

Four precomputed representations are used:

| Representation | Dimensions |
|---|---:|
| CNN audio | 2,048 |
| CNN visual | 2,048 |
| MM audio | 200 |
| MM visual | 200 |

The project trains downstream classifiers on these representations.

It does not train the original feature-extraction models.

### Data Availability

The repository does not include:

- Original MSD-I feature archives.
- Prepared audio and visual feature matrices.
- Original track and artist metadata.
- Saved model predictions.
- Trained model checkpoints.
- Original perceptual-hash candidate files.
- Selected intermediate analysis files.

These inputs must be obtained from authorized sources or the team's existing project workspace.

## 4. Google Drive Project Structure

The default Google Colab project directory is:

```text
/content/drive/MyDrive/MSD-I_project
```

The notebooks also support an alternative location through the environment variable:

`MSDI_PROJECT_DIR`

The main experimental directories are:

```text
MSD-I_project/
|
|-- audio_features/
|   |-- msdi-data.tar.gz
|   |-- MSD-I_dataset.tsv
|
|-- finetuning_experiments/
|   |
|   |-- multimodal_artist_disjoint/
|   |-- oramas_representation_experiment/
|   |-- neural_network_experiments/
|   |
|   |-- reproducible_stage1_2026/
|   |-- reproducible_stage2_2026/
|   |-- reproducible_stage3_2026/
|   |-- reproducible_stage4_2026/
|   |-- reproducible_stage5_2026/
|   |
|   |-- reproducible_weighted_nn_2026/
|   |-- mm_visual_typicality_robustness_2026/
|   |-- multiseed_nn_stability_2026/
|
|-- clean_album_metadata.csv
|-- album_perceptual_hashes.csv
|-- near_duplicate_candidates.csv
```

The original data and research-artifact folders must already exist.

The output directories shown above are the locations used by the corresponding notebooks. They are created during execution when necessary.

Their contents are not committed to this GitHub repository.

### Prepared CNN Features

For each partition `train`, `val`, and `test`, the following files are expected in:

`finetuning_experiments/multimodal_artist_disjoint/`

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

### Prepared MM Features

The following files are expected in:

`finetuning_experiments/oramas_representation_experiment/`

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

The feature rows and metadata must be correctly aligned.

The notebooks check expected dimensions, identifiers, partition membership, and relevant artist-disjoint conditions before performing their analyses.

Additional original prediction files and model artifacts are described in [DATA_AND_REPRODUCIBILITY.md](notebooks/DATA_AND_REPRODUCIBILITY.md).

## 5. Core Experiment Notebooks

The five organized notebooks are located in the [notebooks directory](notebooks/).

The recommended execution sequence is Stage 1 through Stage 5.

However, these stages do not form a completely automated pipeline: some downstream analyses intentionally use original frozen predictions rather than automatically consuming newly trained models.

### Stage 1 — Classification and Fusion

**Notebook:** [Stage 1](notebooks/beyond_audio_final_experiments_stage1.ipynb)

This notebook:

- Loads and checks CNN and MM features.
- Trains audio-only and multimodal logistic-regression models.
- Evaluates visual-only classification using CNN features.
- Evaluates the concatenation of all four representations.
- Tunes regularization parameters using validation macro-F1.
- Evaluates fixed and confidence-adaptive late fusion.
- Compares selected results with original saved research artifacts.

**Main inputs:** Prepared CNN/MM matrices and labels. Original predictions and checkpoints are needed for historical comparison cells.

**Output directory:**

`finetuning_experiments/reproducible_stage1_2026/`

### Stage 2 — Neural Networks

**Notebook:** [Stage 2](notebooks/beyond_audio_final_experiments_stage2.ipynb)

This notebook:

- Prepares CNN and MM early-fusion features.
- Fits feature standardization using training data only.
- Trains two feedforward neural networks.
- Selects checkpoints using validation macro-F1.
- Evaluates the held-out test data.
- Compares reproduced predictions with original saved model outputs.

**Main inputs:** Prepared features, labels, and original neural-network checkpoints or scalers for historical comparison.

**Output directory:**

`finetuning_experiments/reproducible_stage2_2026/`

The successful Tesla T4 reproduction matched the original validation and test class predictions for both neural networks.

### Stage 3 — Visual Typicality and Statistical Analysis

**Notebook:** [Stage 3](notebooks/beyond_audio_final_experiments_stage3.ipynb)

This notebook:

- Reconstructs genre-relative visual-typicality quintiles.
- Recalculates rescued and harmed predictions.
- Evaluates fusion gains across Q1–Q5.
- Computes artist-cluster bootstrap confidence intervals.
- Verifies Q1–Q5 contrasts.

**Main inputs:** CNN visual features, training/test labels, and original frozen CNN/MM model predictions.

**Output directory:**

`finetuning_experiments/reproducible_stage3_2026/`

Stage 3 intentionally uses the original saved predictions and does not automatically replace them with freshly fitted Stage 1 outputs.

The analysis requires true genre labels and is retrospective rather than a prediction-time procedure.

### Stage 4 — Artwork-Overlap Sensitivity

**Notebook:** [Stage 4](notebooks/beyond_audio_final_experiments_stage4.ipynb)

This notebook:

- Checks experimental partition integrity.
- Examines original artwork metadata.
- Reconstructs candidate training-test artwork overlaps.
- Evaluates the exclusion of four flagged test recordings.
- Compares the resulting fusion gains.

**Additional required files:**

```text
audio_features/MSD-I_dataset.tsv
clean_album_metadata.csv
album_perceptual_hashes.csv
near_duplicate_candidates.csv
```

Stage 4 also requires:

```text
finetuning_experiments/
    reproducible_stage3_2026/
        stage3_paired_test_predictions_and_typicality.csv
```

**Output directory:**

`finetuning_experiments/reproducible_stage4_2026/`

This is a sensitivity analysis based on previously saved perceptual-hash candidates.

It does not independently regenerate the full hash search or retrain the classifiers.

### Stage 5 — Research Tables and Figures

**Notebook:** [Stage 5](notebooks/beyond_audio_final_experiments_stage5.ipynb)

This notebook:

- Recalculates the ten main classification results.
- Checks genre-specific F1 scores.
- Reconstructs confusion matrices.
- Reconstructs visual-typicality performance figures.
- Verifies prediction-transition and supplementary tables.
- Checks selected qualitative album-cover examples against saved metadata.

**Main inputs:** Original frozen CNN/MM/NN predictions and relevant Stage 3–4 outputs.

**Output directory:**

`finetuning_experiments/reproducible_stage5_2026/`

The notebook checks numerical consistency with the report. It does not independently verify every original album-cover image URL.

## 6. Supplementary Verification Notebooks

The supplementary notebooks support experiments reported in the appendices.

They can be run after the necessary prepared inputs are available.

### Supplementary Experiment 1 — Class-Weighted Neural Networks

**Notebook:** [Weighted NN Verification](notebooks/weighted_nn_verification_colab.ipynb)

**Report reference:** Section 4.1 and Appendix B.3.

This notebook:

- Reconstructs the original CNN and MM neural-network architectures.
- Calculates class weights from training labels.
- Trains weighted and unweighted variants using seed 42.
- Compares validation and test metrics.
- Checks the reported class-weighting results.

**Required inputs:** Prepared train, validation, and test CNN/MM feature matrices and labels.

Stage 2 predictions may optionally be used for additional comparison checks.

**Output directory:**

`finetuning_experiments/reproducible_weighted_nn_2026/`

The experiment reconstructs the weighted-training procedure. It does not establish access to the original historical weighted-training history.

### Supplementary Experiment 2 — Alternative Visual Typicality

**Notebook:** [MM Visual-Typicality Robustness](notebooks/mm_visual_typicality_robustness_colab.ipynb)

**Report reference:** Section 4.3 and Appendix D.

This notebook:

- Constructs visual-typicality groups using MM visual embeddings.
- Uses training-set genre centroids.
- Retains original frozen classifier predictions.
- Compares CNN-based and MM-based typicality assignments.
- Computes paired artist-cluster bootstrap intervals.

**Required inputs:**

- CNN/MM training and test metadata.
- MM training and test visual embeddings.
- Verified Stage 3 paired prediction/typicality output.

In particular, the notebook expects:

```text
finetuning_experiments/
    reproducible_stage3_2026/
        stage3_paired_test_predictions_and_typicality.csv
```

**Output directory:**

`finetuning_experiments/mm_visual_typicality_robustness_2026/`

Run Stage 3 successfully before this notebook.

Both grouping definitions use true genre labels and are retrospective.

### Supplementary Experiment 3 — Multi-Seed Neural-Network Stability

**Notebook:** [Multi-Seed NN Stability](notebooks/beyond_audio_multiseed_nn_stability_colab.ipynb)

**Report reference:** Sections 4.1 and 4.5, and Appendix D.

This notebook:

- Trains unweighted CNN and MM neural networks.
- Uses seeds 42, 43, 44, 45, and 46.
- Retains the original architectures and hyperparameters.
- Selects checkpoints using validation macro-F1.
- Evaluates each predefined run on the held-out test set.
- Reports cross-seed means and sample standard deviations.

**Required inputs:** Prepared CNN/MM feature matrices and labels for all three data partitions.

Saved Stage 2 predictions are optional for the seed-42 reproduction comparison.

**Output directory:**

`finetuning_experiments/multiseed_nn_stability_2026/`

The recorded experiment was executed in a Tesla T4 environment.

The analysis measures training-initialization sensitivity using a fixed data split.

It does not establish generalization across independent datasets.

## 7. Recommended Execution Order

For a reviewer with access to the original project workspace:

| Order | Notebook | Dependency |
|---|---|---|
| 1 | Stage 1 | Prepared features and original comparison artifacts |
| 2 | Stage 2 | Prepared features and original NN artifacts |
| 3 | Stage 3 | Original frozen predictions |
| 4 | Stage 4 | Stage 3 outputs and saved artwork candidates |
| 5 | Stage 5 | Original predictions and Stage 3–4 outputs |
| 6 | Weighted NN Verification | Prepared features; Stage 2 comparison optional |
| 7 | MM Visual-Typicality Robustness | Stage 3 outputs required |
| 8 | Multi-Seed NN Stability | Prepared features; Stage 2 comparison optional |

The supplementary weighted and multi-seed experiments are computationally independent of the full five-stage sequence, apart from optional historical comparisons.

The alternative MM-typicality experiment requires a verified Stage 3 output.

Before running a notebook, inspect its input-file checks and ensure that the required external research artifacts are available.

## 8. Verification Evidence

The recorded verification results include:

| Check | Result |
|---|---|
| Dataset size | 11,870 recordings |
| Artist-disjoint partitions | Verified |
| CNN audio-only accuracy | 65.90% |
| CNN early-fusion accuracy | 69.64% |
| MM audio-only accuracy | 66.52% |
| MM early-fusion accuracy | 70.20% |
| CNN prediction transitions | 209 rescued, 142 harmed |
| MM prediction transitions | 190 rescued, 124 harmed |
| Original CNN typicality assignments | 1,792 reconstructed |
| CNN gain after artwork exclusion | +3.64 pp |
| MM gain after artwork exclusion | +3.58 pp |
| Alternative MM typicality | Main qualitative pattern persisted |
| Five-seed CNN NN mean accuracy | 68.94% |
| Five-seed MM NN mean accuracy | 69.44% |

The complete numerical evidence and its qualifications are documented in:

[REPRODUCIBILITY_VERIFICATION.md](REPRODUCIBILITY_VERIFICATION.md)

The three supplementary notebooks currently committed to GitHub contain executable procedures without saved cell outputs.

Their recorded verification results are documented separately and should not be confused with directly inspectable execution outputs in the committed notebook files.

## 9. Important Reproducibility Limitations

The following limitations apply:

1. **External data are required.** The GitHub repository does not include the full research dataset or intermediate artifacts.

2. **The workflow is not fully automated.** Some stages intentionally use frozen predictions rather than automatically chaining newly trained models.

3. **Neural-network execution conditions matter.** Successful prediction-level reproduction was obtained in a documented environment, but other executions produced different outcomes.

4. **Perceptual-hash verification is incomplete.** Stage 4 checks the saved candidate list rather than regenerating every upstream hash.

5. **The typicality analysis is retrospective.** Its groups depend on true genre labels and test-set rankings.

6. **Statistical uncertainty is conditional.** The artist-cluster bootstrap does not provide independent validation of the exploratory association.

7. **Independent data validation remains outstanding.** The experiments have not been independently repeated on a new dataset with a separate predefined analysis.

These limitations should be considered when interpreting the reported results.

## 10. Independent Verification Procedure

For an additional clean-runtime check:

1. Obtain authorized access to the original dataset and the necessary research artifacts.
2. Open a new Google Colab session.
3. Configure the project directory and required inputs.
4. Record the Python, package, GPU, and CUDA versions.
5. Execute the desired notebooks in the documented order.
6. Compare outputs with the recorded verification results.
7. Save new evidence separately, preserving previous experimental artifacts.
8. Document any discrepancies, execution failures, or environment differences.

Re-running notebooks may overwrite files in their designated output directories. Back up important results before execution.

**Final note:** The repository documents meaningful numerical verification under the tested conditions, but independent raw-data-to-paper reproduction in a completely fresh environment has not been established.
