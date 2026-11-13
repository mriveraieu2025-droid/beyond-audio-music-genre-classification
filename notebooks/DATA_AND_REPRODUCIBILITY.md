
# Data and Reproducibility Guide

## 1. Overview

This document describes the datasets, intermediate
files, and model outputs used in our machine-learning
project, Beyond Audio: Music Genre Classification.

The experiments were developed in Google Colab
using data stored in Google Drive.

The repository contains the development notebook.
The complete experimental pipeline has not yet
been independently reproduced from a clean
environment.

## 2. Dataset

Our project uses the MSD-I dataset associated
with Oramas et al. (2018).

We evaluate six musical genres:

- Electronic
- Jazz
- Metal
- Pop
- Rap
- Rock

The experimental subset contains 11,870 recordings:

| Partition | Recordings |
|---|---:|
| Training | 8,242 |
| Validation | 1,836 |
| Test | 1,792 |

The reported experiments use the original MSD-I
archive partitions restricted to these genres.

These partitions are artist-disjoint.

## 3. Original Data Files

The development notebook references these
original dataset resources:

- `audio_features/msdi-data.tar.gz`
- `audio_features/MSD-I_dataset.tsv`

It also uses a project-specific metadata file:

- `final_modeling_dataset.csv`

The MSD-I archive contains the feature
representations and track indices used for
the experiments.

The project-specific CSV is an intermediate
file prepared during development. Its earlier
split assignments are not the final experimental
partitions used in the research paper.

Original data files are not included in this
repository. Users should obtain the dataset
from its original source and respect its
distribution conditions.

## 4. Precomputed Feature Matrices

The notebook prepares and uses four feature
representations:

- CNN audio: 2,048 features
- CNN visual: 2,048 features
- MM audio: 200 features
- MM visual: 200 features

For the CNN representations, intermediate files
include:

- `X_train_audio.npy`
- `X_val_audio.npy`
- `X_test_audio.npy`
- `X_train_visual.npy`
- `X_val_visual.npy`
- `X_test_visual.npy`

For the MM representations, corresponding files
include:

- `X_train_mm-audio.npy`
- `X_val_mm-audio.npy`
- `X_test_mm-audio.npy`
- `X_train_mm-visual.npy`
- `X_val_mm-visual.npy`
- `X_test_mm-visual.npy`

Associated label files include:

- `labels_train.csv`
- `labels_val.csv`
- `labels_test.csv`

These are intermediate experimental artifacts,
not separate original datasets.

The notebook contains procedures for extracting
and aligning the representations with the original
MSD-I partitions.

## 5. Models and Generated Results

The experiments train downstream classifiers
rather than retraining the original feature
extractors.

Model artifacts include scikit-learn models
saved as `.joblib` files and PyTorch neural
network checkpoints saved as `.pt` files.

Generated analysis files include:

- `final_test_metrics.csv`
- `final_test_predictions.csv`
- `final_test_typicality_details.csv`
- `representation_final_test_metrics.csv`
- `representation_final_test_predictions.csv`
- `neural_network_test_predictions.csv`

These files record model results and support
the analyses reported in the research paper.

Some notebook sections load results saved by
earlier cells. Therefore, the notebook should
not be assumed to execute independently from
top to bottom without first preparing the
required inputs and intermediate artifacts.

## 6. Original Colab Environment

The notebook uses a Google Drive project folder:

`/content/drive/MyDrive/MSD-I_project`

It also references experimental directories,
including:

- `finetuning_experiments/multimodal_artist_disjoint/`
- `finetuning_experiments/oramas_representation_experiment/`
- `finetuning_experiments/neural_network_experiments/`

These paths reflect the original development
environment and may need to be changed when
running the notebook elsewhere.

Python dependencies are listed in the
repository's `requirements.txt`.

Exact tested package versions and a clean
execution procedure have not yet been
established.

## 7. Development and Verification

Development initially took place in Google
Colab before the creation of this GitHub
repository.

The project includes checks for:

- Track-identifier consistency
- Original partition alignment
- Artist-disjoint evaluation
- Exact feature-vector alignment
- Candidate near-duplicate album artwork

The final experiments use 8,242 training,
1,836 validation, and 1,792 test recordings.

The research report documents the classification
results, uncertainty estimates, exploratory
analyses, and limitations.

## 8. Reproducibility Status

The repository currently provides:

- The development notebook
- Research methodology and experiment descriptions
- Python dependency names
- Documentation of the main input and output files

A fully reproducible execution from a fresh
environment is still being prepared.

This distinction is important because the
development notebook contains exploratory
experiments and depends on external dataset
files and intermediate outputs.

We will document a verified final execution
sequence as the repository is organized.

## Reference

Oramas, S., Barbieri, F., Nieto, O., & Serra, X.
(2018). Multimodal Deep Learning for Music Genre
Classification. Transactions of the International
Society for Music Information Retrieval, 1(1), 4–21.

DOI: https://doi.org/10.5334/tismir.10
