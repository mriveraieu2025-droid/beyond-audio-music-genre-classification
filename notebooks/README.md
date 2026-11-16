# Experiment Notebooks

This directory contains the **five organized experiment and verification notebooks** for *Beyond Audio: Music Genre Classification*, plus the original Google Colab development notebook.

The final notebooks were prepared from the team's prior Colab research work. They have recorded runs using the team's **existing project data and saved artifacts**. They are not a fully self-contained raw-data-to-paper workflow.

## Final experiment notebooks

| Notebook | What it does | Saved output folder in Google Drive |
|---|---|---|
| [Stage 1 — Logistic regression and fusion](beyond_audio_final_experiments_stage1.ipynb) | Aligns CNN/MM embeddings; trains and evaluates logistic-regression baselines, early fusion, and fixed/adaptive late fusion | `finetuning_experiments/reproducible_stage1_2026/` |
| [Stage 2 — Neural networks](beyond_audio_final_experiments_stage2.ipynb) | Trains CNN and MM early-fusion neural networks with validation-based checkpoint selection and compares saved original predictions | `finetuning_experiments/reproducible_stage2_2026/` |
| [Stage 3 — Visual typicality and uncertainty](beyond_audio_final_experiments_stage3.ipynb) | Reconstructs retrospective typicality quintiles, paired prediction transitions, and artist-cluster bootstrap intervals | `finetuning_experiments/reproducible_stage3_2026/` |
| [Stage 4 — Artwork sensitivity](beyond_audio_final_experiments_stage4.ipynb) | Checks original experimental splits and saved perceptual-hash candidates; evaluates sensitivity to flagged test recordings | `finetuning_experiments/reproducible_stage4_2026/` |
| [Stage 5 — Figures and tables](beyond_audio_final_experiments_stage5.ipynb) | Recalculates reported metrics and genre-specific F1 values, reconstructs figure data, and checks supplementary results | `finetuning_experiments/reproducible_stage5_2026/` |

Output folders above are relative to the Google Drive project root `/content/drive/MyDrive/MSD-I_project` (or the `MSDI_PROJECT_DIR` override). Their contents are **not committed to GitHub**.

## Original development notebook

The [original development notebook](beyond_audio_colab_development.ipynb) records exploratory work that preceded the GitHub repository, including:

- MSD-I metadata inspection, feature alignment, and data integrity
- Audio-only and visual-only logistic-regression baselines
- CNN and MM early-fusion classification
- Fixed and confidence-adaptive late fusion
- Feedforward neural networks
- Prediction-level analysis and visual-typicality groups
- Bootstrap uncertainty, robustness checks, and report figures

Some earlier experiments were superseded or corrected by later work. Saved cell outputs were removed from the **public development notebook** to reduce size; the original Colab work and its corresponding research artifacts were retained separately by the team.

**Use the staged notebooks for the final verified workflow**, rather than assuming the development notebook will execute independently from top to bottom.

## Data, dependencies, and verification

The source feature archives, prepared embeddings, metadata, prediction CSVs, checkpoints, and original perceptual-hash records reside outside this public repository. They must be available in the authorized research workspace to execute the notebooks.

The five stages are intentionally **data-dependent**. In particular, Stage 3 reads original frozen model predictions, Stage 4 reads previously generated candidate-artwork files, and Stage 5 reads original predictions plus verified Stage 3–4 outputs. The original feature-extractor networks are **not** trained by this project.

For setup, provenance, and recorded verification, refer to:

- [Reproduction and execution instructions](../REPRODUCE.md)
- [Data and reproducibility guide](DATA_AND_REPRODUCIBILITY.md)
- [Experiment index](EXPERIMENT_INDEX.md)
- [Recorded reproduction checks](../REPRODUCIBILITY_VERIFICATION.md)
- [Python dependencies](requirements.txt) (names only; not pinned versions)

An independent clean-environment execution from all original data to the final figures has **not** been established.

## Development history

Research and notebook development began in Google Colab before this repository was created. GitHub commits document the subsequent repository history and should not be interpreted as a complete chronology of the earlier Colab work. Generative AI assistance is disclosed in the [main README](../README.md).
