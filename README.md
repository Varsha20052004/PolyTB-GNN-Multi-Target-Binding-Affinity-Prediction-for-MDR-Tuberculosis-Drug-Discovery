# PolyTB-GNN: Multi-Target Binding Affinity Prediction for MDR-Tuberculosis Drug Discovery

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-red.svg)](https://pytorch.org/)
[![PyG](https://img.shields.io/badge/PyTorch_Geometric-2.3+-orange.svg)](https://pytorch-geometric.readthedocs.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Paper: Under Review](https://img.shields.io/badge/Paper-Under%20Review-yellow.svg)]()

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

## Repository Structure
