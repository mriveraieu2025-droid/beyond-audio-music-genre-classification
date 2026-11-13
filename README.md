
# Beyond Audio: Music Genre Classification

**Machine Learning Final Project — November 2026**

**Authors:** Carlos Saldaña, Ana Santos, and Maria Rivera

## Project Overview

This project investigates whether album-cover artwork
can improve automatic music genre classification when
combined with audio information.

Although album covers may contain visual characteristics
associated with particular musical genres, this
information is not always reliable.

Our central research question is:

**When does combining album-cover artwork with audio
improve music genre classification, and when can it
introduce additional errors?**

## Dataset

We use a six-genre subset of the MSD-I dataset,
associated with Oramas et al. (2018).

The dataset contains 11,870 recordings from six genres:

- Electronic
- Jazz
- Metal
- Pop
- Rap
- Rock

We preserve the original experimental partitions:

| Partition | Recordings |
|---|---:|
| Training | 8,242 |
| Validation | 1,836 |
| Test | 1,792 |

The partitions are artist-disjoint.

## Machine Learning Methods

We evaluate two families of precomputed audio and
visual representations: CNN and MM embeddings.

The experiments include:

- Multiclass logistic regression
- Feedforward neural networks
- Audio-only and visual-only classification
- Early fusion
- Fixed late fusion
- Confidence-adaptive late fusion

Model selection is based on validation macro-F1.

Performance is evaluated using accuracy,
balanced accuracy, and macro-F1.

## Main Findings

Early multimodal fusion improves test accuracy
by approximately 3.7 percentage points compared
with the corresponding audio-only baselines.

The highest observed test accuracy is 70.20%.

However, our exploratory analysis reveals that
the benefit of fusion varies considerably with
genre-relative album-cover visual typicality.

For the most visually typical recordings, fusion
improves accuracy by 17.45 percentage points.

For the most visually atypical recordings, fusion
reduces accuracy by 19.72 and 16.94 percentage
points for CNN and MM representations, respectively.

These results suggest that evaluating multimodal
models requires examining individual prediction
outcomes, not only aggregate accuracy.

The visual-typicality analysis is retrospective
and uses true genre labels. Its findings describe
associations rather than causal relationships
or a deployable reliability measure.

## Development and Reproducibility

The project was initially developed in Google
Colab before the creation of this GitHub repository.

The development process includes dataset exploration,
feature alignment, classifier training, fusion
experiments, prediction-level analysis, statistical
evaluation, and robustness checks.

The repository is being organized to document
these experiments and their development.

The original MSD-I feature archive and associated
data files are required to execute the experiments.
The existing Colab notebook uses Google Drive paths,
which will need to be configured for reproduction.

Complete setup and execution instructions will be
added as the repository is organized.

## AI Assistance

Generative AI tools, including ChatGPT, have been
used to assist with aspects of coding, debugging,
analysis, and documentation.

The project team is responsible for reviewing,
understanding, verifying, and explaining the
submitted code, methods, and results.

## Reference

Oramas, S., Barbieri, F., Nieto, O., & Serra, X.
(2018). Multimodal Deep Learning for Music Genre
Classification. Transactions of the International
Society for Music Information Retrieval, 1(1), 4–21.

https://doi.org/10.5334/tismir.10
