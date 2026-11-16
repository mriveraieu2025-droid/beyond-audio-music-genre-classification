# Experiment Index

This guide distinguishes the **original Colab development notebook** from the five final, organized verification notebooks and identifies which original research outputs support the paper.

The [development notebook](beyond_audio_colab_development.ipynb) predates the GitHub repository. It includes exploratory experiments, corrections, intermediate results, and final model evaluations. Some earlier cells were superseded; their output should not automatically be treated as evidence for the final report.

## 1. Original development work

| Research area | Corresponding development-notebook sections | Relevant saved research outputs |
|---|---|---|
| Dataset and embedding preparation | Inspection of MSD-I Metadata; Verification of Audio Feature Alignment; Feature Quality Validation; Extraction and Alignment of Audio and Visual Embeddings | Original MSD-I track indices and prepared CNN/MM feature matrices |
| Audio/visual classification and CNN fusion | Audio-Only Baseline; Visual-Only Baseline; Early Fusion; Final Evaluation on the Held-Out Test Set | `final_test_metrics.csv`, `final_test_predictions.csv` |
| CNN versus MM representations | Development sequence labelled **EXPERIMENT 2** | `representation_final_test_metrics.csv`, `representation_final_test_predictions.csv` |
| Neural-network classification | Original development sequence labelled **STAGE 6** | `neural_network_test_metrics.csv`, `neural_network_test_predictions.csv`, saved checkpoints |
| Typicality and prediction transitions | Visual-typicality subgroup analysis; paired rescued/harmed transitions | `final_test_typicality_details.csv`, `cnn_mm_paired_transitions.csv`, `cnn_mm_quintile_transitions.csv` |
| Statistical uncertainty | Artist-cluster bootstrap; Q1–Q5 contrast analysis | `overall_fusion_gain_cluster_bootstrap.csv`, `quintile_fusion_gain_cluster_bootstrap.csv`, `q1_q5_fusion_gain_contrasts.csv` |
| Artwork audit and robustness | Original partition verification; image-URL checks; perceptual-hash candidate analysis | Saved artwork metadata, `near_duplicate_candidates.csv`, sensitivity tables |
| Research figures | Confusion matrices; fusion-gain plots; accuracy-by-typicality plots; selected album-cover examples | Original figures and supporting prediction or metadata CSVs |

The file names above refer to **original research artifacts in the team's project workspace**, not necessarily files located next to this Markdown document in GitHub.

## 2. Organized final experiment notebooks

The staged notebooks give a more direct path through the final analyses. The labels **Stage 1–5** refer to this organized verification sequence and should not be confused with section or stage numbers inside the older development notebook.

| Final notebook | Verification focus | Key data boundary |
|---|---|---|
| [Stage 1: Logistic regression and fusion](beyond_audio_final_experiments_stage1.ipynb) | Refit CNN/MM and combined-representation logistic-regression models; compare early and late fusion, metrics, and selected original checkpoints | Uses prepared feature matrices and labels, with original artifacts for selected comparisons |
| [Stage 2: Neural networks](beyond_audio_final_experiments_stage2.ipynb) | Refit CNN/MM early-fusion neural networks and compare saved predictions | Uses prepared embeddings and original checkpoint/scaler files for historical comparisons |
| [Stage 3: Visual typicality and bootstrap](beyond_audio_final_experiments_stage3.ipynb) | Reconstruct quintiles, rescue/harm transitions, and artist-cluster intervals | Uses **original frozen test predictions**, not automatically the new Stage 1 predictions |
| [Stage 4: Artwork overlap sensitivity](beyond_audio_final_experiments_stage4.ipynb) | Check metadata, current split membership, and saved hash candidates; exclude flagged test tracks | Uses the **saved candidate list** and Stage 3 outputs; no independent upstream image-hash generation |
| [Stage 5: Figures and tables](beyond_audio_final_experiments_stage5.ipynb) | Recalculate ten classification model results, per-genre F1, main figure data, and supplementary tables | Uses original CNN/MM/NN predictions and verified Stage 3–4 outputs |

Each stage saves new results to a corresponding `finetuning_experiments/reproducible_stageN_2026/` directory in the **external project workspace**, not to this GitHub repository. Rerunning a stage may replace files in its own output directory, so previously verified outputs should be backed up where necessary.

## 3. How the final experiments support the paper

- **Overall classification comparisons:** Stage 1 and Stage 5 evaluate audio-only, visual-only, early-fusion, fixed/adaptive late-fusion, and all-four-representation logistic-regression results. Stage 2 covers CNN/MM neural networks.
- **Visual typicality:** Stage 3 reconstructs genre-relative quintiles using standardized CNN visual embeddings and **true genre labels**. The analysis is retrospective, exploratory, and does not provide an inference-time reliability signal.
- **Prediction-level behavior:** Stage 3 reconstructs rescued and harmed classifications for CNN and MM fusion models. Stage 5 verifies the corresponding tables and visual-only agreement values.
- **Statistical uncertainty:** Stage 3 reconstructs artist-cluster bootstrap estimates and confidence intervals using **3,000 artist-level resamples** for the main typicality analysis.
- **Robustness:** Stage 4 verifies the saved artwork candidate-pair mapping and sensitivity of reported fusion gains to excluding four flagged test recordings. The upstream perceptual hashes were not independently regenerated.
- **Figures and supplementary results:** Stage 5 checks data for confusion matrices, the typicality-gain plot, absolute-accuracy curves, genre-specific F1 scores, and selected example labels. The original artwork source URLs were not independently reverified.

## 4. Verification records and limits

The notebook outputs and [reproducibility verification record](../REPRODUCIBILITY_VERIFICATION.md) document successful numerical checks with the team's existing dataset and artifacts. In the recorded Tesla T4 environment, Stage 2 reproduced the original CNN/MM neural-network **class predictions** for all 1,836 validation and 1,792 test recordings.

These checks should **not** be described as completely independent replication on a new dataset or a standalone raw-data-to-paper build. Source embeddings, saved predictions, model checkpoints, and candidate-artwork files remain necessary external inputs.

For the required file paths, environment notes, and execution sequence, see [REPRODUCE.md](../REPRODUCE.md) and [DATA_AND_REPRODUCIBILITY.md](DATA_AND_REPRODUCIBILITY.md).

## 5. Development history and attribution

GitHub history records work committed after the initial Colab research. It does not establish the chronology of earlier experiments or attribute exact contributions by itself. The project team remains responsible for the submitted work; AI-assisted development is disclosed in the [project README](../README.md).
