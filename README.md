# PolyTB-GNN: Multi-Target Binding Affinity Prediction for MDR-Tuberculosis Drug Discovery

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-red.svg)](https://pytorch.org/)
[![PyG](https://img.shields.io/badge/PyTorch_Geometric-2.3+-orange.svg)](https://pytorch-geometric.readthedocs.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

> **Official implementation of PolyTB-GNN**, a dual-branch cross-attention 
> Graph Neural Network for simultaneous multi-target binding affinity 
> prediction across InhA, KasA, and DprE1 of *Mycobacterium tuberculosis*.

---

## Overview

Multi-Drug Resistant Tuberculosis (MDR-TB) is driven by mutations in key 
enzyme binding pockets, rendering single-target drugs ineffective. 
**PolyTB-GNN** addresses this by jointly predicting binding affinities across 
**three distinct TB enzyme targets** simultaneously, identifying drug candidates 
with balanced multi-target potency — a property that imposes near-insurmountable 
evolutionary barriers on *M. tuberculosis*.

### Key Contributions

- **First curated multi-target TB bioactivity dataset** — 4,295 standardized 
  drug-like molecules with experimental pKᵢ values across InhA, KasA, and DprE1 
  from ChEMBL
- **Novel dual-branch architecture** — GAT molecular graph encoder fused with 
  ECFP4 + MACCS pharmacophoric fingerprint encoder via 4-head cross-attention
- **Inverse-frequency-weighted masked multi-task loss** — handles severely 
  imbalanced target sample sizes (124 to 2,969) without discarding any data
- **Multi-Target Resilience Score (MTRS)** — novel harmonic-mean ranking metric 
  that penalizes weakness on any single target
- **Consistent improvement across all three targets** — outperforms GCN and GAT 
  baselines simultaneously (InhA: +9.6%, KasA: +68.5%, DprE1: +32.8%)

---

## Results

| Model | InhA R | KasA R | DprE1 R | Mean R |
|---|---|---|---|---|
| GCN Baseline | 0.753 | 0.251 | 0.670 | 0.558 |
| GAT Baseline | 0.527 | 0.084 | 0.438 | 0.350 |
| PolyTB-GNN v1 (E3-Equivariant) | 0.711 | 0.096 | 0.338 | 0.382 |
| PolyTB-GNN v2 (Stabilized E3) | 0.762 | 0.129 | 0.335 | 0.409 |
| PolyTB-GNN v3 (Dual-Branch) | 0.837 | 0.398 | 0.753 | 0.663 |
| **PolyTB-GNN v3-ft (Ours)** | **0.825** | **0.423** | **0.890** | **0.713** |

---

## Installation

### Option 1 — Google Colab (Recommended)

Open the notebook directly in Colab and run all cells in order. 
All dependencies install automatically within the notebook.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](YOUR_COLAB_LINK_HERE)

### Option 2 — Local Installation

```bash
# Clone the repository
git clone https://github.com/Varsha20052004/PolyTB-GNN.git
cd PolyTB-GNN

# Create virtual environment
python -m venv polytb_env
source polytb_env/bin/activate  # On Windows: polytb_env\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

---

## Requirements
torch>=2.0.0
torch-geometric>=2.3.0
torch-scatter>=2.1.0
torch-sparse>=0.6.17
rdkit>=2023.3.1
chembl-webresource-client>=0.10.9
pandas>=2.0.0
numpy>=1.24.0
scikit-learn>=1.3.0
scipy>=1.11.0
matplotlib>=3.7.0
seaborn>=0.12.0
tqdm>=4.65.0

---

## Notebook Pipeline

The notebook `PolyTB_GNN.ipynb` is organized into the following modules:

| Module | Description |
|---|---|
| **Module 1** | ChEMBL data collection and curation for InhA, KasA, DprE1 |
| **Module 2** | Molecular featurization — 3D graph construction + atom/bond features |
| **Module 3** | Baseline models — GCN and GAT training and evaluation |
| **Module 4** | PolyTB-GNN — architecture, training, ablation study, fine-tuning |
| **Module 5** | Results and figures — all publication-ready plots |

---

## Dataset

The curated dataset was sourced from the ChEMBL database (v33) using the 
following target ChEMBL IDs:

| Target | ChEMBL ID | Role in MDR-TB | Molecules |
|---|---|---|---|
| InhA | CHEMBL2366517 | FAS-II enoyl reductase | 1,241 |
| KasA | CHEMBL3192 | FAS-II β-ketoacyl synthase | 2,969 |
| DprE1 | CHEMBL3804751 | Arabinogalactan biosynthesis | 124 |

**Preprocessing pipeline:**
- Fragment removal (largest fragment heuristic)
- Charge neutralization (RDKit Uncharger)
- Deduplication by InChIKey (median pChEMBL for duplicates)
- Activity labeling: active (pChEMBL ≥ 6.0), intermediate (5–6), inactive (< 5)

---

## Multi-Target Resilience Score (MTRS)

The MTRS is a novel metric introduced in this work to rank drug candidates 
for MDR-resilience. It is defined as the harmonic mean of predicted pKᵢ 
values across all three targets:

$$\text{MTRS} = \frac{3}{\frac{1}{\hat{y}_{\text{InhA}}} + \frac{1}{\hat{y}_{\text{KasA}}} + \frac{1}{\hat{y}_{\text{DprE1}}}}$$

The harmonic mean penalizes weakness on any single target — a molecule 
scoring 9/9/2 receives MTRS ≈ 3.6, while a molecule scoring 7/7/7 
receives MTRS = 7.0 — directly operationalizing the biological requirement 
for multi-pathway inhibition.

---

## Citation

If you use PolyTB-GNN, the dataset, or any code from this repository in 
your research, please cite:

```bibtex
@article{polytbgnn2025,
  title   = {PolyTB-GNN: A Dual-Branch Cross-Attention Graph Neural Network 
             for Multi-Target Binding Affinity Prediction in MDR-Tuberculosis 
             Drug Discovery},
  author  = {[Your Name] and [Co-Author Name]},
  journal = {Computers in Biology and Medicine},
  year    = {2025},
  note    = {Under Review}
}
```

---

## License

This project is licensed under the MIT License. 

---

## Acknowledgements

Bioactivity data sourced from ChEMBL (EMBL-EBI). 
Molecular processing performed using RDKit (Greg Landrum et al.). 
Graph neural network implementation built on PyTorch Geometric (Fey & Lenssen, 2019).

---

