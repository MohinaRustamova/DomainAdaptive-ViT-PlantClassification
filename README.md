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

├── Ablations_PVD_PD.ipynb               # Proposed method + ablation study (A1–A6)
├── model1_vit_base.ipynb                # Baseline: ViT-Base (no adaptation)
├── model2_api_net.ipynb                 # Baseline: API-Net
├── model3_ensemble.ipynb                # Baseline: Ensemble
├── model4_yolov11x.ipynb               # Baseline: YOLOv11x
├── model5_unet.ipynb                    # Baseline: U-Net
├── model6_dinov2.ipynb                  # Baseline: DINOv2
