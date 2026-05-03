# DI725 Term Project: Multimodal Fusion for Remote Sensing Land Cover Segmentation

Course project for METU DI725. This project investigates whether textual descriptions can improve semantic segmentation performance on aerial land-cover imagery.

## Overview

The project compares a vision-only segmentation baseline with text-augmented multimodal models. The main task is semantic segmentation, where each pixel is assigned to one of seven land-cover classes.

The study focuses on three points:

1. Whether textual descriptions improve segmentation over a vision-only baseline.
2. Whether different textual description variants contribute differently.
3. Whether image-level class distribution information helps when used as a train-time auxiliary regularizer.

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

The vision-only baseline is UNetFormer. It uses a CNN backbone with a transformer-style decoder.

The multimodal model extends UNetFormer with:

- a frozen CLIP text encoder
- gated cross-attention modules in the decoder
- five textual description variants
- an optional KL-based train-time auxiliary regularizer

The KL regularizer is used only during training and is not provided to the model during inference.

## Results

| Configuration | Test mIoU | Gain vs baseline |
|---|---:|---:|
| UNetFormer baseline | 0.649 | 0.0% |
| Best text-augmented model (`text_qwen3-4b`) | 0.697 | +7.4% |
| Text-augmented model + KL regularizer | **0.705** | **+8.6%** |

The largest final relative gains over the vision-only baseline are observed for Shrub, Barren, and Crop. This suggests that textual information is most useful for classes where the visual baseline is weaker.

## Repository Structure

```text
data/
notebooks/
  01_data_exploration.ipynb
  02_preprocessing.ipynb
  03_baseline_models.ipynb
  04_multimodal.ipynb
reports/
  term_project_phase1_*.pdf
  term_project_phase2_*.pdf
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
   Trains vision-only baselines, including UNetFormer and Swin-UperNet.

4. `04_multimodal.ipynb`  
   Trains text-augmented models, caption-variant ablations, percentage-removal checks, and the KL auxiliary regularizer experiment.

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

UNetFormer implementation is adapted from [WangLibo1995/GeoSeg](https://github.com/WangLibo1995/GeoSeg). CLIP text encoding is implemented using [open_clip](https://github.com/mlfoundations/open_clip).
