
# Reproducing the Experiments

## Overview

This repository contains five organized experimental
notebooks and the original Google Colab development
notebook.

The five stages reconstruct the main classification
experiments, statistical analyses, robustness checks,
and research figures.

The experiments have been verified using the
project's existing data and Google Colab environment.

An independent clean-environment reproduction
is the next verification objective.

## Environment

The verified Colab environment included:

- Python 3.13.18
- NumPy 2.4.6
- pandas 2.2.3
- scikit-learn 1.6.1
- PyTorch 2.14.1+cpu

Additional dependencies are listed in
`requirements.txt`.

These are recorded versions, not a fully
pinned or independently validated environment.

## Required Data

The notebooks expect a project directory
containing the prepared MSD-I features and
metadata.

The main input directories are:

- `finetuning_experiments/multimodal_artist_disjoint/`
- `finetuning_experiments/oramas_representation_experiment/`

These contain artist-disjoint labels and
precomputed audio/visual feature matrices.

The repository does not publicly distribute
the original dataset or feature archive.

Access to external data must be arranged
separately, respecting its distribution terms.

## Execution Order

### Stage 1: Logistic Regression

Notebook:
`notebooks/beyond_audio_final_experiments_stage1.ipynb`

Loads the CNN and MM feature matrices,
selects regularization parameters using
validation data, and evaluates logistic
regression and fusion strategies.

### Stage 2: Neural Networks

Notebook:
`notebooks/beyond_audio_final_experiments_stage2.ipynb`

Trains CNN and MM fusion neural networks
with validation-based checkpoint selection.

Original checkpoints are needed for the
additional checkpoint-comparison sections.

### Stage 3: Visual Typicality

Notebook:
`notebooks/beyond_audio_final_experiments_stage3.ipynb`

Reconstructs visual-typicality quintiles,
prediction transitions, and artist-cluster
bootstrap confidence intervals.

This stage uses original saved prediction
files and is not yet connected directly to
the newly generated Stage 1 predictions.

### Stage 4: Robustness

Notebook:
`notebooks/beyond_audio_final_experiments_stage4.ipynb`

Checks metadata consistency, saved
near-duplicate candidates, and sensitivity
to excluding flagged test recordings.

Requires the original metadata and
perceptual-hash candidate files, as well
as the verified Stage 3 outputs.

### Stage 5: Figures and Tables

Notebook:
`notebooks/beyond_audio_final_experiments_stage5.ipynb`

Reconstructs the main research figures
and verifies reported model and
supplementary results.

Requires original saved model predictions
and selected Stage 3 and Stage 4 outputs.

## Important Limitations

The five notebooks do not yet constitute
a fully automated pipeline running from
raw MSD-I data to the final paper.

Some stages depend on original saved
predictions, extracted embeddings, and
intermediate analysis files.

The original feature-extraction process
and upstream perceptual-hash generation
have not been independently reconstructed
in these five notebooks.

The numerical verification results must
not be interpreted as independent
validation on a new dataset.

## Independent Reproduction

A team member should:

1. Open the notebooks from GitHub in a
   new Google Colab environment.
2. Obtain authorized access to the
   required project input files.
3. Configure the project root directory.
4. Execute the notebooks in order,
   documenting any missing dependencies.
5. Compare the produced results with
   `REPRODUCIBILITY_VERIFICATION.md`.
6. Record the Python package versions,
   execution issues, and any differences.

The independent test should be documented
using genuine GitHub commits.

No historical commits should be recreated
or backdated.
