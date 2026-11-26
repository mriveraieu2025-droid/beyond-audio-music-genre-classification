
# Beyond Audio — Experiment Notebooks

This directory contains the eight core and supplementary Python notebooks supporting the experiments in *Beyond Audio: Music Genre Classification*. The original exploratory development notebook is preserved separately in [`archive/`](../archive/).

The repository separates the final experiments from the original exploratory development work.

## 1. Core Experiments

The five core notebooks cover the main research workflow.

| Notebook | Purpose |
|---|---|
| [Stage 1 — Classification and Fusion](beyond_audio_final_experiments_stage1.ipynb) | Logistic regression, audio-only baselines, early fusion, fixed late fusion, and confidence-adaptive late fusion |
| [Stage 2 — Neural Networks](beyond_audio_final_experiments_stage2.ipynb) | CNN and MM early-fusion neural-network training and evaluation |
| [Stage 3 — Visual Typicality](beyond_audio_final_experiments_stage3.ipynb) | Genre-relative typicality quintiles, rescued/harmed predictions, and artist-cluster bootstrap |
| [Stage 4 — Artwork Sensitivity](beyond_audio_final_experiments_stage4.ipynb) | Dataset-integrity checks and candidate near-duplicate artwork sensitivity |
| [Stage 5 — Results Verification](beyond_audio_final_experiments_stage5.ipynb) | Verification of classification metrics, supplementary tables, and research figures |

These notebooks use the original artist-disjoint dataset partitions and precomputed CNN/MM representations.

The recommended execution sequence is Stage 1 through Stage 5, respecting their documented dependencies.

## 2. Supplementary Experiments

Three additional notebooks support analyses reported in the appendices.

| Notebook | Purpose |
|---|---|
| [Weighted Neural-Network Verification](weighted_nn_verification_colab.ipynb) | Reconstructs class-weighted cross-entropy experiments using seed 42 |
| [Alternative Visual Typicality](mm_visual_typicality_robustness_colab.ipynb) | Evaluates robustness when visual typicality is defined using MM instead of CNN embeddings |
| [Multi-Seed NN Stability](beyond_audio_multiseed_nn_stability_colab.ipynb) | Evaluates unweighted CNN/MM neural networks across five predefined random seeds |

These experiments provide additional evidence concerning class imbalance, representation sensitivity, and training randomness.

They do not constitute independent validation on a new dataset.

The three supplementary notebooks currently committed to GitHub contain executable code without saved cell outputs. Their recorded numerical results are documented in the project's verification record.

## 3. Original Development Notebook

[Original Colab Development](../archive/beyond_audio_colab_development.ipynb)

This notebook preserves the original exploratory research.

It includes preliminary experiments, intermediate analyses, and approaches that were later corrected or superseded.

The public notebook does not contain its historical saved cell outputs.

**For the final reported experiments, use the eight organized notebooks above rather than the exploratory development notebook.**

The development notebook is retained to preserve research history.

## 4. Data and Execution

The experiments were developed using Google Colab and project data stored separately in Google Drive.

The default project directory is:

`/content/drive/MyDrive/MSD-I_project`

An alternative location can be specified using the `MSDI_PROJECT_DIR` environment variable.

The repository does not include the full research dataset, prepared feature matrices, original checkpoints, saved predictions, or selected intermediate artifacts.

These must be obtained separately from authorized sources or the team's existing project workspace.

Some core notebooks intentionally use original frozen predictions rather than automatically consuming newly trained models.

The notebooks are therefore documented, data-dependent verification workflows rather than a fully self-contained raw-data-to-paper pipeline.

### Experiment Output Directories

The core notebooks write results to:

- `reproducible_stage1_2026/`
- `reproducible_stage2_2026/`
- `reproducible_stage3_2026/`
- `reproducible_stage4_2026/`
- `reproducible_stage5_2026/`

The supplementary notebooks write results to:

- `reproducible_weighted_nn_2026/`
- `mm_visual_typicality_robustness_2026/`
- `multiseed_nn_stability_2026/`

These directories are located within `finetuning_experiments/` in the external project workspace.

Their contents are not committed to this repository.

## 5. Documentation

For more information:

- [Main Project README](../README.md) — Project overview and main findings.
- [REPRODUCE.md](../REPRODUCE.md) — Complete execution instructions and data requirements.
- [EXPERIMENT_INDEX.md](EXPERIMENT_INDEX.md) — Maps each experiment to its corresponding report results.
- [REPRODUCIBILITY_VERIFICATION.md](../REPRODUCIBILITY_VERIFICATION.md) — Numerical verification evidence and limitations.
- [DATA_AND_REPRODUCIBILITY.md](DATA_AND_REPRODUCIBILITY.md) — Dataset provenance and intermediate research artifacts.
- [requirements.txt](requirements.txt) — Python dependencies.

The project has not established fully independent reproduction from the original raw data in a new computing environment.
