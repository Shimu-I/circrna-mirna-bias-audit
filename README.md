

# Beyond Popularity: A Bias-Controlled Re-evaluation of HMA-GCA for circRNA–miRNA Interaction Prediction

Jasmin Sultana Shimu, Rokia Jannat, Tanjina Akter
Department of Computer Science and Engineering, United International University, Dhaka, Bangladesh

Research project for CSE 4893: Introduction to Bioinformatics, United International University.

## Summary
 
Deep learning models for circRNA–miRNA interaction prediction report AUC above 0.96, but much of that score can come from RNA popularity rather than real pair-level learning. We audited HMA-GCA on a merged benchmark of 20,771 pairs with leak-free cold-start splits and degree-matched negatives. A degree-only rule reached 0.893 AUC under the paper-style setting, and HMA-GCA fell from 0.967 to 0.680 under the strictest conditions. Using node degree as a model input hurt prediction for unseen RNAs, and predicting for new miRNAs remains the main open problem.

## The problem

Circular RNAs (circRNAs) can bind microRNAs (miRNAs) and change how genes are regulated. Testing a circRNA–miRNA pair in the lab is slow and costly, so deep learning models are used to rank candidate pairs first. HMA-GCA [1], a recent model, reports AUC of 0.963–0.973 on three datasets.

These scores come from random pair splits with random negative pairs. The interaction network is very uneven: a few RNAs have many known partners and most have very few. In this setting, a model can score high just by learning which RNAs are popular, without learning anything about a specific pair. The question is how much of the reported performance is real pair-level learning, and how much is a popularity shortcut.

## What we did

- Merged the three datasets used by HMA-GCA into one benchmark: 3,802 circRNAs, 1,273 miRNAs, 20,771 unique positive pairs.
- Built warm and cold-start splits (new circRNA, new miRNA, both new) with two kinds of test negatives: random and degree-matched.
- Checked every split for data leakage (all 20 folds clean) and re-derived the benchmark independently from the raw data.
- Compared a degree-only rule, gradient boosting, and four HMA-GCA variants on exactly the same stored splits, with three random seeds for the main variants.
- Compared validation protocols, since a random validation slice can hide cold-start failure.

## What we found

| Model (warm split, random negatives) | AUC |
|---|---|
| Degree-only rule (no learning) | 0.893 |
| HMA-GCA (full) | 0.958 |
| HMA-GCA (without degree input) | 0.967 |
| Gradient boosting (sequence only) | 0.960 |

- **Popularity explains a lot.** A rule that only uses node degree already reaches 0.893 under the paper-style setting. With degree-matched negatives it falls to chance (0.494), while HMA-GCA still reaches 0.817, so the model does learn something real.
- **Scores drop under stricter evaluation.** For HMA-GCA without degree, AUC falls from 0.967 (warm, random negatives) to 0.863 (degree-matched negatives) and to 0.680 when both the circRNA and the miRNA are new.
- **The node-degree input hurts new RNAs.** When both RNAs are new, AUC is 0.546 with the degree input and 0.809 without it, because degree exists for known nodes and disappears for new ones.
- **New miRNAs are the main open problem.** Gradient boosting on sequence features performs as well as or better than HMA-GCA there.
- **PCA+UMAP shows no consistent effect** (AUC change between −0.011 and +0.014).
- **Validation must match the test condition.** A random validation slice gave validation AUC of about 0.97 where the cold-start test AUC was 0.54. Holding out whole nodes removed this gap.

## Recommendations for future studies

Report a degree-only baseline, cold-start splits, degree-matched negatives and node-held-out validation, and use several seeds with paired differences.

## Scope

This is not an exact reproduction of the published HMA-GCA numbers. Only K-mer and CTD features are used, and the splits and negatives differ from the original study. Only the HMA-GCA architecture was re-run.

## Tools

Python, PyTorch, scikit-learn, umap-learn, NumPy, pandas. Experiments were run in Kaggle notebooks with a GPU.

## Repository structure

```
circrna-mirna-bias-audit/
├── README.md
├── .gitignore
├── requirements.txt
├── notebooks/
│   ├── 01_dataset_analysis.ipynb
│   ├── 02_benchmark_audit.ipynb
│   └── 03_hmagca_on_benchmark.ipynb
├── data/
│   └── processed/
│       ├── pairs_merged.csv
│       ├── circ_ids.csv
│       ├── mirna_ids.csv
│       ├── splits.csv.gz
│       ├── seq_features.npz
│       └── leakage_checks.csv
├── results/
│   ├── dataset_analysis/
│   ├── benchmark_audit/
│   ├── hmagca_v1/
│   └── hmagca_v2/
├── figures/
├── paper/
│   ├── manuscript.md
│   ├── manuscript_latex/
│   └── Final-report.pdf
```

## Reference

[1] Hu, Y., Wang, Y., Bai, Y., Xu, L., Zou, Q., Zhou, H., Wang, C., Niu, M. HMA-GCA: hybrid manifold augmentation and gated cross-attention for circRNA–miRNA interaction prediction. *Bioinformatics* 42(8), 2026. https://doi.org/10.1093/bioinformatics/btag536