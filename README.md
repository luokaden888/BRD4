AI tools were used for code documentation and clarity, but all modeling, experiments, and analysis were conducted by the author.

# Protein–Ligand Binding Affinity Prediction with a Hybrid GNN–Transformer

This repository contains code for an independent research project on **protein–ligand binding classification**, using deep learning models trained on molecular SMILES data. The project focuses on improving generalization to novel chemical scaffolds by integrating **graph, fingerprint, and physicochemical representations** within a hybrid GNN–Transformer architecture.

## Motivation
Protein–ligand binding affinity prediction is a central problem in drug discovery. Traditional experimental and simulation-based methods (e.g., docking, molecular dynamics) are computationally expensive and time-consuming, while many machine learning approaches suffer from poor generalization or reliance on noisy regression targets.

This project explores **binary binding classification** as a robust alternative and investigates whether combining multiple molecular representations can improve performance across structurally diverse protein targets.

## Dataset
- Source: **BELKA (Big Encoded Library for Chemical Assessment)** dataset
- Ligand representation: molecular SMILES
- Targets: **BRD4, HSA, sEH**
- Labels: binary binding/non-binding
- Sampling:
  - Balanced datasets (binding vs non-binding)
  - **Scaffold-based splitting** to evaluate out-of-distribution generalization
  - 75% train / 25% test with stratified validation split

## Molecular Representations
- **ECFP (Morgan) fingerprints** (1024-bit, radius 2)
- **Graph representations** (atom-level features + bond connectivity)
- **Physicochemical descriptors** (molecular weight, logP, TPSA)

Descriptors and fingerprints were generated using **RDKit**, and graph data was processed using **PyTorch Geometric**.

## Models Evaluated
Baseline:
- Logistic Regression
- Random Forest
- Decision Tree

Deep Learning:
- 1D CNN (ECFP-based)
- 2D CNN (RDKit molecular images)
- Graph Neural Network (GNN)
- Transformer (ECFP + metadata)
- **Hybrid GNN–Transformer (proposed)**

## Hybrid GNN–Transformer Architecture
The proposed model processes three input streams **in parallel**:
1. **GNN branch** for local structural information
2. **Transformer branch** for global feature interactions across ECFP bits
3. **MLP branch** for physicochemical descriptors

The Transformer uses **multi-head self-attention** to model higher-order dependencies between fingerprint dimensions (treated as a set, not a sequence). Outputs from all branches are fused via feature concatenation and passed to a final classifier.

## Training Strategy
- Loss: `BCEWithLogitsLoss`
- Optimizer: Adam
- Batch balancing for class imbalance
- Threshold tuning based on precision–recall trade-offs
- Regularization: dropout, early stopping
- Cosine learning rate scheduler with warmup

## Evaluation Metrics
- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC
- **Average Precision (AP)**

Threshold-independent metrics (AUC, AP) were emphasized due to class imbalance and scaffold-based evaluation.

## Results (Approximate Averages)
Across all three proteins, the hybrid GNN–Transformer achieved:
- **Accuracy / Precision / Recall:** ~70–80%
- **ROC-AUC:** ~0.80–0.82
- **Average Precision:** ~0.68–0.70

Compared to baseline models (~50% accuracy), the hybrid model improved performance by **20–30% across all major metrics**. Under scaffold splits, accuracy and precision decreased slightly, while AUC and AP remained stable—indicating preserved ranking ability on unseen scaffolds.

## Key Takeaways
- Multi-representational models outperform single-representation baselines
- Scaffold splits reveal generalization limits masked by random splits
- AUC and Average Precision are more reliable indicators than accuracy for drug discovery
- Hybrid GNN–Transformer models show promise across structurally diverse proteins

## Limitations & Future Work
- Extend evaluation to additional protein families
- Incorporate protein sequence or pocket-level structural features
- Explore regression-based affinity prediction (e.g., Kd values)
- Investigate equivariant GNNs for 3D molecular interactions

## Author
Kaden Luo

## Acknowledgements
Mentorship and resources provided by Inspirit AI.

AI tools were used for code documentation and clarity, but all modeling, experiments, and analysis were conducted by the author.
