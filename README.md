# Beyond Popularity: A Bias-Controlled Re-evaluation of HMA-GCA for circRNA–miRNA Interaction Prediction

**Authors:** Jasmin Sultana Shimu, Rokia Jannat, Tanjina Akter  
**Affiliation:** Department of Computer Science and Engineering, United International University, Dhaka, Bangladesh  
**Project:** CSE 4893 — Introduction to Bioinformatics

## 01. Project Overview

A research-based re-evaluation of HMA-GCA, a deep learning model for circRNA–miRNA interaction prediction. This study investigates whether reported predictive performance is influenced by node-degree bias, negative sampling strategies, and evaluation protocols, particularly when predicting interactions involving unseen RNAs.

## 02. Research Problem

Random negative sampling and conventional pair-level evaluation may allow prediction models to exploit RNA interaction popularity rather than learn meaningful interaction patterns. This project examines how network degree and evaluation design affect model performance and generalization under cold-start conditions.

## 03. Methodology & Experimental Approach

- **Dataset:** Merged and deduplicated three HMA-GCA datasets, producing a benchmark of 3,802 circRNAs, 1,273 miRNAs, and 20,771 unique positive interactions.
- **Sequence features:** K-mer frequencies (340 dimensions) and Composition, Transition, and Distribution (CTD) descriptors (30 dimensions).
- **Models:** Degree-only baseline, Gradient Boosting, and four HMA-GCA variants.
- **Model variants:** Full model, without node-degree features, without PCA+UMAP, and without both.
- **Evaluation:** Five-fold cross-validation using warm, cold circRNA, cold miRNA, and cold-both splits.
- **Negative sampling:** Compared random negatives against degree-matched negatives.
- **Ablation studies:** Examined the contribution of node-degree information and PCA+UMAP augmentation.

**Implementation scope:** This is a partial re-evaluation of the HMA-GCA architecture using reduced sequence features. Doc2Vec and Role2Vec features from the original study were excluded; therefore, the experiments should not be interpreted as an exact reproduction of the published results.

## 04. Key Results

AUC comparison across selected evaluation settings:

| Model | Warm / Random | Warm / Degree-Matched | Cold Both / Random | Cold Both / Degree-Matched |
|---|---:|---:|---:|---:|
| Degree-only | 0.893 | 0.494 | 0.500 | 0.500 |
| Gradient Boosting | 0.960 | 0.783 | 0.873 | 0.691 |
| HMA-GCA (Full) | 0.958 | 0.817 | 0.546 | 0.556 |
| HMA-GCA (Without Degree) | **0.967** | **0.863** | **0.809** | 0.680 |

## 05. Key Findings

- Node degree alone achieved 0.893 AUC under warm random evaluation, indicating substantial popularity-related predictive signal.
- Degree-matched negative sampling reduced the apparent advantage of popularity-based prediction.
- Removing node-degree input improved cold-both AUC from 0.546 to 0.809 under random negative sampling.
- PCA+UMAP augmentation showed no consistent performance improvement.
- Generalization to unseen RNAs, particularly unseen miRNAs, remained challenging.

## 06. My Contribution

- Conducted HMA-GCA training experiments across warm and cold-start evaluation settings.
- Executed model variants to assess the effects of node-degree features and PCA+UMAP augmentation.
- Monitored training runs and collected performance metrics for comparative analysis.

## 07. Limitations

- Doc2Vec and Role2Vec features from the original model were excluded.
- The original HMA-GCA implementation and results were not fully reproduced.
- Unlabelled interactions were treated as negatives, although some may be undiscovered positives.
- Other published circRNA–miRNA prediction models were not benchmarked.
- Cold-both evaluation involved relatively small test folds.

## 08. Tech Stack

`Python` · `PyTorch` · `scikit-learn` · `NumPy` · `Pandas` · `Google Colab`

## 09. Project Organization

- **Report:** `Final-report.pdf`
- **Code & Notebooks:** Dataset analysis, benchmark construction, baseline evaluation, and HMA-GCA experiments.
- **Data:** Three public circRNA–miRNA datasets associated with the base study.

[Add repository links or data-access instructions where available.]

## 10. Reference

Hu, Y., Wang, Y., Bai, Y., Xu, L., Zou, Q., Zhou, H., Wang, C., & Niu, M. (2026). HMA-GCA: Hybrid manifold augmentation and gated cross-attention for circRNA–miRNA interaction prediction. *Bioinformatics, 42*(8). https://doi.org/10.1093/bioinformatics/btag536