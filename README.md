# MOSAIRE

This repository contains the official figures of the paper: **MOSAIRE:
Mitigating Route-Level Structural Drift via Evidence Compression in Continual
WSI Learning**.

## 1. Overview

Continual whole-slide image (WSI) classification requires a model to learn new
histological tasks while retaining previously acquired knowledge under limited
access to historical data. Sequential updates can disrupt how hierarchical WSI
models select, route, and aggregate tissue evidence, resulting in route-level
structural drift.

MOSAIRE is designed to mitigate this problem through semantic region
selection, redundancy-aware route compression, and training-time memory-anchor
guidance. The framework preserves compact historical evidence across tasks
while requiring no memory-anchor retrieval during inference.

## 2. MOSAIRE Framework

MOSAIRE consists of three stages:

1. **Discover:** Frozen CONCH-v1 image and text encoders score candidate tissue
   regions against fixed text prototypes. The most relevant regions are
   retained, with up to 512 regions selected for each WSI.
2. **Condense:** Redundancy-aware route compression (RaRP) condenses each
   selected 8 × 8 patch grid from 64 patch tokens into 32 representative route
   tokens for patch-to-region cross-attention.
3. **Consolidate:** Region- and slide-level memory-anchor guidance (MAG),
   together with route-relation alignment, uses replay-derived historical
   anchors to stabilize evidence aggregation during continual training.

## 3. Experimental Setting

MOSAIRE is evaluated on six sequential TCGA cohorts:

```text
NSCLC → BRCA → RCC → ESCA → TGCT → CESC
```

The benchmark contains 3,654 WSIs from 3,384 patients across 13 histological
subtypes. Experiments use frozen CONCH-v1 representations, ten fixed
patient-level splits, and replay capacities of 1,100, 2,200, and 6,600 regions.

## 4. Main Results

| Replay capacity | Final Class-IL accuracy (%) | Macro one-vs-rest AUC (%) |
|---:|---:|---:|
| 1,100 regions | 68.83 ± 2.86 | 97.82 ± 0.44 |
| 2,200 regions | 70.83 ± 4.51 | 97.99 ± 0.43 |
| 6,600 regions | 75.90 ± 2.18 | 98.23 ± 0.31 |

At a replay capacity of 6,600 regions, MOSAIRE exceeds ConSlide by 1.18
percentage points in final Class-IL accuracy and 0.91 percentage points in
macro AUC. At capacities of 1,100 and 2,200 regions, the corresponding accuracy
gains are 2.18 and 1.43 percentage points.

Five-fold ablations show that removing semantic region selection or MAG
reduces mean accuracy by 1.84 and 1.79 percentage points, respectively.
Compressing 64 patch tokens into 32 route tokens improves mean accuracy by
0.51 percentage points over the uncompressed grid while halving the
cross-attention cost. Across 90 Task 1 test slides, MAG reduces final mean
attention drift from 59.79% to 57.11%.

## 5. Main Figure

The overview figure presents the sequential WSI task stream and the Discover,
Condense, and Consolidate stages of MOSAIRE:

- [MOSAIRE overview (PDF)](fig/Fig1_MOSAIRE_overview.pdf)

## 6. Citation

```bibtex
@misc{qian2026mosaire,
  title  = {MOSAIRE: Mitigating Route-Level Structural Drift via Evidence Compression in Continual WSI Learning},
  author = {Fangji Qian and Weitao Wang and Yanyan Huang and Meng Niu and Yuanxue Gao and Zihao Zhao and Kuangyu Shi and Lequan Yu and Yu Fu and Cheng Zhuo},
  year   = {2026}
}
```

## 7. License

This repository is released under the [MIT License](LICENSE).
