# Experiment Index — Beyond Audio

This document maps the experiments reported in *Beyond Audio: Music Genre Classification* to their corresponding Google Colab notebooks.

It distinguishes the five organized core experiment notebooks, three supplementary verification notebooks, and the original exploratory development notebook.

**The recommended starting point is the five core notebooks.** The supplementary notebooks support additional analyses reported in Appendices B and D.

## 1. Core Experiment Notebooks

The five organized notebooks cover the main classification experiments, prediction-level analyses, and numerical verification procedures.

| Notebook | Main purpose | Relevant report content |
|---|---|---|
| [Stage 1 — Classification and Fusion](beyond_audio_final_experiments_stage1.ipynb) | Trains and evaluates logistic-regression models using CNN and MM representations; compares early, fixed late, and confidence-adaptive late fusion | Section 3.1, Section 3.2, Section 4.1, Section 4.2 |
| [Stage 2 — Neural Networks](beyond_audio_final_experiments_stage2.ipynb) | Trains CNN/MM early-fusion neural networks using validation-selected checkpoints; compares reconstructed predictions with original saved outputs | Section 3.1, Section 4.1, Appendix A |
| [Stage 3 — Visual Typicality and Bootstrap](beyond_audio_final_experiments_stage3.ipynb) | Reconstructs genre-relative visual-typicality quintiles, rescued/harmed predictions, and artist-cluster bootstrap confidence intervals | Section 3.3, Section 3.4, Section 4.3, Section 4.4, Appendix C |
| [Stage 4 — Artwork-Overlap Sensitivity](beyond_audio_final_experiments_stage4.ipynb) | Checks experimental partitions and candidate near-duplicate artwork; evaluates sensitivity to excluding flagged test recordings | Section 2.3, Appendix D |
| [Stage 5 — Tables and Figures](beyond_audio_final_experiments_stage5.ipynb) | Recalculates reported metrics, checks genre-specific results, and reconstructs data and figures used in the report | Section 4, Appendices B–D |

The Stage 1–5 labels refer to this organized verification workflow. They should not be confused with the stage numbers in the original development notebook.

Each notebook saves its outputs to a separate directory within the external Google Drive project workspace.

## 2. Supplementary Verification Notebooks

Three additional notebooks support analyses performed after the original core experiments.

### Weighted Neural-Network Verification

**Notebook:** [weighted_nn_verification_colab.ipynb](weighted_nn_verification_colab.ipynb)

**Report location:** Section 4.1 and Appendix B.

This notebook reconstructs the weighted cross-entropy experiments using the original CNN and MM neural-network architectures.

It compares weighted and unweighted networks using identical data partitions, hyperparameters, and validation-based checkpoint selection.

The reconstruction reproduced the reported weighted-model metrics and genre-specific recall values to the reported precision. Its unweighted controls also reproduced the original predictions in the recorded execution environment.

The experiment compares weighting effects using seed 42. It does not evaluate class-weighting stability across multiple seeds.

**Important distinction:** The notebook is a reconstructed verification experiment, not the original historical weighted-training notebook.

### Alternative Visual-Typicality Representation

**Notebook:** [mm_visual_typicality_robustness_colab.ipynb](mm_visual_typicality_robustness_colab.ipynb)

**Report location:** Section 4.3 and Appendix D.

This experiment tests the sensitivity of the main visual-typicality findings to the choice of visual representation.

The original analysis defines visual typicality using CNN visual embeddings. The supplementary experiment repeats the procedure using MM visual embeddings.

Training-set genre centroids and genre-conditional test-set quintiles are recalculated using the alternative representation. Original classifier predictions are retained without retraining.

The experiment found that only 54.19% of recordings retained the same quintile assignment across the two definitions.

Nevertheless, both definitions produced positive fusion gains among the most visually typical recordings and substantial negative gains among the most atypical.

The experiment also evaluates uncertainty using paired artist-cluster bootstrap resampling.

**Limitation:** Both analyses remain retrospective, require true genre labels, and use representations with related feature-extraction histories. This is a sensitivity analysis rather than independent validation.

### Multi-Seed Neural-Network Stability

**Notebook:** [beyond_audio_multiseed_nn_stability_colab.ipynb](beyond_audio_multiseed_nn_stability_colab.ipynb)

**Report location:** Section 4.1, Section 4.5, and Appendix D.

This notebook evaluates the stability of the unweighted CNN and MM early-fusion neural networks across five predefined random seeds:

**42, 43, 44, 45, 46**

It preserves the original architectures, hyperparameters, data partitions, and early-stopping procedure.

Checkpoints are selected using validation macro-F1. All five seeds are reported rather than selecting the best-performing seed on the test set.

The recorded Tesla T4 experiment produced:

| Test metric | CNN mean ± SD | MM mean ± SD |
|---|---:|---:|
| Accuracy | 68.94 ± 0.61% | 69.44 ± 0.56% |
| Balanced accuracy | 67.98 ± 0.28% | 68.37 ± 0.41% |
| Macro-F1 | 68.30 ± 0.43% | 68.76 ± 0.45% |

The original seed-42 test predictions were reproduced exactly in the recorded GPU environment.

**Limitation:** This experiment measures sensitivity to training randomness using one fixed experimental partition. It does not test generalization to independent datasets or establish the stability of the weighted-loss comparison.

## 3. Mapping Report Results to Experiments

| Report result or analysis | Supporting notebook(s) |
|---|---|
| Dataset and feature-alignment checks | Stage 1, Stage 2, Stage 4 |
| Audio-only and visual-only logistic-regression results | Stage 1, Stage 5 |
| CNN and MM early-fusion logistic regression | Stage 1, Stage 5 |
| Fixed and confidence-adaptive late fusion | Stage 1, Stage 5 |
| CNN/MM neural-network classification | Stage 2, Stage 5 |
| Weighted cross-entropy experiment | Weighted NN Verification |
| Genre-specific F1 and confusion matrices | Stage 5 |
| Visual-typicality quintiles | Stage 3 |
| Rescued and harmed predictions | Stage 3, Stage 5 |
| Artist-cluster bootstrap confidence intervals | Stage 3 |
| Candidate duplicate-artwork sensitivity | Stage 4 |
| Alternative MM-based visual typicality | MM Visual-Typicality Robustness |
| Five-seed neural-network stability | Multi-Seed NN Stability |
| Supplementary accuracy plots and qualitative-example checks | Stage 5 |

This table identifies the primary verification source for each finding. It does not imply that every original research artifact can be regenerated independently from the public repository.

## 4. Data Dependencies and Reproducibility Boundaries

The notebooks rely on the original project workspace.

The required source archive, prepared feature matrices, labels, model checkpoints, saved predictions, and some intermediate analysis files are not included in this repository.

The default Google Drive project root is:

`/content/drive/MyDrive/MSD-I_project`

The project root may also be specified using the `MSDI_PROJECT_DIR` environment variable.

### Important dependencies

- **Stage 1** uses the prepared CNN/MM feature matrices and metadata, together with selected historical artifacts for comparison.
- **Stage 2** trains neural networks using prepared embeddings and compares them with original saved model outputs.
- **Stage 3** reads original frozen test predictions, rather than automatically consuming newly generated Stage 1 predictions.
- **Stage 4** uses saved perceptual-hash candidates. The original artwork hashes are not independently regenerated by this stage.
- **Stage 5** uses existing predictions and relevant Stage 3–4 outputs to check reported results.
- **Supplementary notebooks** use the corresponding prepared features, metadata, or frozen predictions, depending on the analysis.

The notebooks constitute documented, data-dependent reconstruction and verification workflows.

They are **not a fully automated raw-data-to-paper pipeline**, and the analyses have not been independently validated on a new dataset.

The three supplementary notebooks currently stored on GitHub contain executable procedures but do not include saved execution outputs. Their recorded numerical results should be distinguished from evidence that can be inspected directly in those notebook files.

## 5. Original Development Notebook

**Notebook:** [beyond_audio_colab_development.ipynb](../archive/beyond_audio_colab_development.ipynb)

This notebook preserves the original exploratory development of the project.

It includes dataset inspection, metadata alignment, baseline modeling, fusion experiments, neural networks, visual-typicality analyses, and earlier research figures.

Some exploratory procedures were subsequently corrected, reorganized, or superseded.

The original notebook is retained to preserve research history, but **the eight organized core and supplementary notebooks are the preferred references for the final report**.

Saved outputs from earlier exploratory cells should not automatically be treated as evidence for the final reported results.

## 6. Documentation and Verification Records

For further information, consult:

- [Main Project README](../README.md) — research overview, findings, and repository navigation.
- [REPRODUCE.md](../REPRODUCE.md) — required files, execution order, environment information, and reproduction instructions.
- [REPRODUCIBILITY_VERIFICATION.md](../REPRODUCIBILITY_VERIFICATION.md) — recorded verification evidence for all five core stages and three supplementary experiments.
- [DATA_AND_REPRODUCIBILITY.md](DATA_AND_REPRODUCIBILITY.md) — data provenance, prepared features, and intermediate artifacts.
- [requirements.txt](requirements.txt) — Python dependencies, currently without pinned versions.

The supplementary experiment results are included in the updated [reproducibility verification record](../REPRODUCIBILITY_VERIFICATION.md). The public copies of the three supplementary notebooks contain source code without saved cell execution outputs.

## 7. Recommended Reading Order

For someone reviewing the project for the first time:

1. Read the main project README and scientific report.
2. Consult Stage 1 and Stage 2 for the classification experiments.
3. Review Stage 3 for the main visual-typicality finding and statistical analysis.
4. Review Stage 4 and Stage 5 for robustness checks, figures, and numerical verification.
5. Inspect the three supplementary notebooks for the additional Appendix B and D experiments.
6. Consult the original development notebook only when investigating the earlier research process.

This sequence separates the final experimental evidence from exploratory work and helps readers trace the main scientific claims to their supporting code.
