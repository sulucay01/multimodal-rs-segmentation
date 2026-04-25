# Multimodal Fusion for Remote Sensing Land Cover Segmentation

DI725 Term Project — Selin Uluçay (2218147)
Middle East Technical University, Data Informatics

## Overview

This project investigates whether incorporating textual descriptions into a remote sensing semantic segmentation pipeline can improve land cover classification, particularly under severe class imbalance. Visual features from a UNetFormer / SegFormer backbone are combined with textual embeddings from a pretrained vision-language model (CLIP) through cross-attention modules in the decoder.

## Research Questions

The study is structured around one main research question and two ablation sub-questions:

**Main RQ.** Does incorporating textual descriptions into a remote sensing segmentation model improve performance compared to a vision-only baseline?

**Sub-question 1 (caption source ablation).** When the textual input is varied across text-only, vision-based, and hybrid caption types, which configuration produces the largest improvement over the vision-only baseline in terms of mIoU and per-class IoU?

**Sub-question 2 (auxiliary supervision ablation).** Does adding a KL-divergence auxiliary loss between predicted and ground truth class distributions improve mIoU over the best text-augmented configuration from Sub-question 1?

## Repository Structure

\`\`\`
multimodal-rs-segmentation/
├── README.md
├── requirements.txt
├── .gitignore
├── configs/                  # YAML experiment configs
├── data/                     # gitignored; expected layout below
├── notebooks/
├── src/
│   ├── data/                 # dataset, transforms, augmentation
│   ├── models/               # UNetFormer, SegFormer, multimodal wrappers
│   ├── losses/               # weighted CE, KL-divergence aux loss
│   ├── train.py
│   ├── evaluate.py
│   └── utils.py
└── reports/
\`\`\`

## Setup

\`\`\`bash
git clone https://github.com/<username>/multimodal-rs-segmentation.git
cd multimodal-rs-segmentation
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
\`\`\`

## Data

The dataset is not committed. Place it under \`data/\` with this layout:

\`\`\`
data/
├── images/
├── masks/
└── captions.csv
\`\`\`

## Reproducibility

- Fixed seeds across data splitting, model initialization, and dataloaders
- All hyperparameters in YAML configs (no magic numbers in code)
- Experiments tracked on Weights & Biases
- Train/val/test split is deterministic (stratified by dominant class)

## Phase Status

- **Phase 1 (Literature, Proposal, PoC)** — submitted
- **Phase 2 (Benchmarking, Ablations)** — in progress
- **Phase 3 (Final results, report)** — upcoming