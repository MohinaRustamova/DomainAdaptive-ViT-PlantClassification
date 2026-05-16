# CORAL-SSL-ViT-PlantDisease

Code for the paper:
**"Domain-Adaptive Vision Transformer With CORAL and Self-Supervised Learning for Robust Plant Disease Classification"**
Baimukashev R., Rustamova M., Kadyrov S. — IEEE Access, 2026.

---

## Overview

This repository contains the full implementation for reproducing all experiments
in the paper. We propose a domain-adaptive ViT-based framework that bridges the
laboratory-to-field gap in plant disease classification by combining:

- **CORAL** — second-order feature alignment between source (PlantVillage) and target (PlantDoc) domains
- **FixMatch-style consistency regularization** — strong/weak augmentation pairs for pseudo-label training
- **EMA teacher** — exponential moving average model for stable pseudo-label generation

On PlantDoc, the proposed method achieves **71.96% accuracy** and **59.04% macro-F1**,
outperforming all six baselines by a margin of +20.49 pp macro-F1.

---

## Repository Structure

```
├── Ablations_PVD_PD.ipynb               # Proposed method + ablation study (A1–A6)
├── model1_vit_base.ipynb                # Baseline: ViT-Base (no adaptation)
├── model2_api_net.ipynb                 # Baseline: API-Net
├── model3_ensemble.ipynb                # Baseline: Ensemble
├── model4_yolov11x.ipynb                # Baseline: YOLOv11x
├── model5_unet.ipynb                    # Baseline: U-Net
└── model6_dinov2.ipynb                  # Baseline: DINOv2
```

---

## Datasets

| Dataset | Role | Size | Access |
|---|---|---|---|
| PlantVillage | Source domain (labeled) | ~54,000 images | [Kaggle](https://www.kaggle.com/datasets/abdallahalidev/plantvillage-dataset) |
| PlantDoc | Target domain (unlabeled in training) | ~2,600 images | [GitHub](https://github.com/pratikkayal/PlantDoc-Dataset) |

Both datasets are downloaded automatically inside the notebooks via `opendatasets`.
Labels are harmonized to a shared **15-class** disease-level taxonomy as described in Section III-B of the paper.

---

## Setup

All notebooks are designed to run on **Google Colab** (GPU recommended).
Dependencies are installed automatically in the first cell of each notebook.

Core dependencies:
```
torch, timm, albumentations, kornia, scikit-learn, opendatasets
```

---

## Reproducing Results

### Proposed Method + Ablation Study
Open and run `Ablations_PVD_PD.ipynb` end-to-end.
This notebook covers all six ablation configurations (A1–A6) from Table 3 of the paper,
using fixed splits and seeds for reproducibility.

Key hyperparameters (consistent with the paper):

| Parameter | Value |
|---|---|
| Backbone | `vit_base_patch16_224.augreg_in21k_ft_in1k` |
| Unfrozen blocks | Last 4 transformer blocks |
| Batch size | 32 |
| Learning rate | 1e-4 |
| Weight decay | 0.05 |
| Optimizer | AdamW |
| EMA decay (α) | 0.999 |
| Warmup epochs | 1 |
| Ramp-up epochs | 5 |
| Ablation epochs | 5 |
| Pseudo-label threshold (A5) | 0.95 → 0.85 (annealed) |
| Pseudo-label threshold (A6) | 0.90 (fixed) |

### Baselines
Each `model*.ipynb` file is self-contained and reproduces one baseline from Table 2.
Run them independently; no cross-dependencies between notebooks.

---

## Results Summary

| Model | Test Acc (%) | Macro-F1 (%) |
|---|---|---|
| U-Net | 52.91 | 30.29 |
| YOLOv11x | 53.44 | 32.97 |
| Ensemble | 56.61 | 34.83 |
| ViT-Base | 56.08 | 35.42 |
| API-Net | 56.61 | 36.05 |
| DINOv2 | 58.20 | 38.55 |
| **Ours** | **71.96** | **59.04** |

Statistical significance confirmed via Cochran's Q (Q = 52.93, p = 1.21 × 10⁻⁹)
and Holm-corrected McNemar posthoc tests (all p ≤ 1.34 × 10⁻²).

---

## Citation

If you use this code, please cite:

```bibtex
@article{baimukashev2026domain,
  title={Domain-Adaptive Vision Transformer With CORAL and Self-Supervised Learning
         for Robust Plant Disease Classification},
  author={Baimukashev, Rashid and Rustamova, Mohina and Kadyrov, Shirali},
  journal={IEEE Access},
  doi={10.1109/ACCESS.2026.XXXXXXX},
  year={2026}
}
```

---

## Acknowledgment

This research was funded by the Ministry of Science and Higher Education
of the Republic of Kazakhstan, project AP23487777.
