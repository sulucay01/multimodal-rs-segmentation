# DI725 Term Project: Multimodal Fusion for Remote Sensing Land Cover Segmentation

Course project for METU DI725. This project investigates whether textual descriptions can improve semantic segmentation performance on aerial land-cover imagery.

## Overview

The project compares a vision-only segmentation baseline with text-augmented multimodal models. The main task is semantic segmentation, where each pixel is assigned to one of seven land-cover classes, with performance measured by mean Intersection-over-Union (mIoU) on the test set.

The study focuses on these questions:

1. Whether textual descriptions improve segmentation over a vision-only baseline.
2. Whether different textual description variants contribute differently.
3. Whether image-level class distribution information helps when used as a train-time auxiliary regularizer.
4. Whether a remote-sensing-pretrained text encoder (RemoteCLIP) outperforms a general CLIP encoder.
5. Whether a region-based loss outperforms weighted cross-entropy, comparing CE+Dice, CE+Tversky, and CE+Lovász.
6. Whether the gains from the selected encoder and loss add up when the two are combined in the final model.

## Dataset

The dataset contains 10,000 aerial RGB images at 256 × 256 resolution. Each image has a pixel-level segmentation mask and five textual description variants.

Land-cover classes:

- Tree
- Shrub
- Grass
- Crop
- Built-up
- Barren
- Water

The dataset is split into 70% training, 15% validation, and 15% test sets using stratified sampling based on the dominant class of each image.

## Method

The vision-only baselines are UNetFormer and Swin-UperNet.

The multimodal model extends UNetFormer with:

- a frozen text encoder, either general CLIP or remote-sensing-pretrained RemoteCLIP
- gated cross-attention modules in the decoder, which condition the prediction on the encoded caption
- a choice of loss, weighted cross-entropy or a region-based loss (CE+Dice, CE+Tversky, or CE+Lovász)

The final configuration is RemoteCLIP + CE+Dice, conditioned on the `text_qwen3-4b` caption.

A separate Phase 2 experiment added an optional KL-based train-time auxiliary regularizer that uses image-level class distribution. The regularizer is used only during training and is not provided to the model at inference. It is reported below as a Phase 2 result and is not part of the final model.

## Results

All values are mIoU on the test set. Gains are relative to the vision-only baseline.

| Configuration | mIoU | Gain vs baseline |
|---|---:|---:|
| Vision-only baseline (UNetFormer) | 0.6488 | — |
| Multimodal (CLIP, weighted CE) | 0.6970 | +7.4% |
| Multimodal (RemoteCLIP, weighted CE) | 0.7008 | +8.0% |
| Multimodal (CLIP, CE+Dice) | 0.7281 | +12.2% |
| **Multimodal (RemoteCLIP + CE+Dice), final** | **0.7347** | **+13.2%** |

The two Phase 3 changes contribute roughly additively: switching CLIP to RemoteCLIP and switching weighted CE to CE+Dice each help on their own, and combining them gives the best result.

For reference, a Phase 2 experiment with the KL-based auxiliary regularizer (on the CLIP, weighted-CE branch) reached an mIoU of 0.705. The Phase 3 directions surpassed it, so the final model does not use the KL regularizer.

## Repository Structure

```text
data/
  README.md
notebooks/
  README.md
  01_data_exploration.ipynb
  02_preprocessing.ipynb
  03_baseline_models.ipynb
  04_multimodal.ipynb
  05_phase3_ablations.ipynb
  06_qualitative_analysis.ipynb
reports/
  README.md
  term_project_phase1_*.pdf
  term_project_phase2_*.pdf
  term_project_phase3.pdf
requirements.txt
README.md
```

## Notebooks

Run the notebooks in numerical order.

1. `01_data_exploration.ipynb`  
   Explores class distributions, caption length, caption accuracy, and dataset characteristics.

2. `02_preprocessing.ipynb`  
   Converts RGB masks into class-index masks and creates the train, validation, and test splits.

3. `03_baseline_models.ipynb`  
   Trains the vision-only baselines, UNetFormer and Swin-UperNet.

4. `04_multimodal.ipynb`  
   Trains text-augmented models, caption-variant ablations, percentage-removal checks, and the KL auxiliary regularizer experiment.

5. `05_phase3_ablations.ipynb`  
   Phase 3 ablations: text encoder (CLIP vs RemoteCLIP), region loss (CE+Dice, CE+Tversky, CE+Lovász vs weighted CE), and the combined final model with an additivity check.

6. `06_qualitative_analysis.ipynb`  
   Qualitative analysis of the three-stage progression on the test set: a worked example, per-step comparisons, worst cases, and per-class IoU. Loads saved predictions only.

## Setup

The experiments are designed to run on Google Colab with an A100 GPU.

Install dependencies with:

```bash
pip install -r requirements.txt
```

## Experiment Tracking

All experiments are tracked with Weights & Biases.

W&B project: [di725_project](https://wandb.ai/selingoktas98-metu-middle-east-technical-university/di725_project?nw=nwuserselingoktas98)

## Acknowledgements

UNetFormer implementation is adapted from [WangLibo1995/GeoSeg](https://github.com/WangLibo1995/GeoSeg). CLIP text encoding is implemented using [open_clip](https://github.com/mlfoundations/open_clip). RemoteCLIP weights are from [chendelong/RemoteCLIP](https://huggingface.co/chendelong/RemoteCLIP).
