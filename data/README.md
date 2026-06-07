# Data

The dataset used in this project is not publicly available. It was provided for the METU DI725 course project only and cannot be redistributed. This folder is therefore empty in the repository, and the files below are not committed.

## Provided files

```text
data/
  images/        # true-color RGB images, 256 x 256
  masks/         # RGB segmentation maps, one per image
  captions.csv   # per-image descriptions and class composition percentages
```

There are 7 classes in the segmentation maps. Each class has a fixed RGB color in `masks/`:

| Class | R | G | B |
|---|---:|---:|---:|
| Tree | 0 | 100 | 0 |
| Shrub | 255 | 182 | 193 |
| Grass | 154 | 205 | 50 |
| Crop | 255 | 215 | 0 |
| Built-up | 139 | 69 | 19 |
| Barren | 211 | 211 | 211 |
| Water | 0 | 0 | 255 |

`captions.csv` has five description columns, `hybrid_gemma3-4b`, `hybrid_qwen3-vl-8b`, `text_qwen3-4b`, `vision_gemma3-4b`, `vision_qwen3-vl-8b`, and seven composition-percentage columns, `Tree`, `Shrub`, `Grass`, `Crop`, `Built-up`, `Barren`, `Water`, giving each class's share of the image derived from the segmentation masks.

## Produced by preprocessing

`02_preprocessing.ipynb` converts the RGB maps in `masks/` into class-index masks (values 0-6) and adds the train/val/test split, writing:

```text
data/
  masks_class/               # class-index masks, values 0-6
  captions_with_splits.csv   # captions plus the train/val/test split column
```

The class index order is: 0 Tree, 1 Shrub, 2 Grass, 3 Crop, 4 Built-up, 5 Barren, 6 Water.
