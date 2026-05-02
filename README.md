# DI725 Term Project: Multimodal Fusion for Remote Sensing Land Cover Segmentation

Course project for METU DI725. Investigates whether textual descriptions can improve semantic segmentation on aerial land cover imagery.

## Approach

Vision-only baseline: **UNetFormer** (CNN-transformer hybrid). Multimodal variant extends UNetFormer with a frozen CLIP text encoder and gated cross-attention modules between visual features and the caption embedding at each decoder stage.

Dataset: 10,000 aerial RGB images (256x256), 7 land cover classes, 5 caption variants per image. Stratified 70/15/15 split. Pixel-level mIoU on the test set is the primary metric.

## Results

| Configuration | Test mIoU | Δ vs baseline |
|---|---|---|
| UNetFormer baseline (vision-only) | 0.6488 | --- |
| Multimodal, best caption (`text_qwen3-4b`) | 0.6970 | +7.4% |
| Multimodal + KL auxiliary regularizer | **0.7046** | **+8.6%** |

Largest per-class gains: Shrub +42.0%, Barren +19.3%, Crop +10.3%.

## Repository Structure

```
data/                 
notebooks/
  01_data_exploration.ipynb
  02_preprocessing.ipynb
  03_baseline_models.ipynb
  04_multimodal.ipynb
reports/              
requirements.txt
README.md
```

## Setup

Designed for Google Colab (A100 GPU). Open the notebooks in numerical order. All experiments are logged to Weights & Biases under `di725_project`.

```bash
pip install -r requirements.txt
```

## Acknowledgements

UNetFormer from [WangLibo1995/GeoSeg](https://github.com/WangLibo1995/GeoSeg). CLIP from [open_clip](https://github.com/mlfoundations/open_clip).
