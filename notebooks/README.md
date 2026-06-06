# Notebooks

Run in numerical order. Each notebook reads the data and writes its outputs (predictions, result JSONs, checkpoints) to the project folder on Google Drive.

1. `01_data_exploration.ipynb` — dataset and class-distribution exploration
2. `02_preprocessing.ipynb` — RGB masks to class-index masks, train/val/test split
3. `03_baseline_models.ipynb` — vision-only baselines (UNetFormer, Swin-UperNet)
4. `04_multimodal.ipynb` — text-augmented models, caption ablations, KL regularizer
5. `05_phase3_ablations.ipynb` — text encoder and region-loss ablations, combined final model
6. `06_qualitative_analysis.ipynb` — qualitative and per-class analysis of the final model

See the root `README.md` for full descriptions, results, and setup.
