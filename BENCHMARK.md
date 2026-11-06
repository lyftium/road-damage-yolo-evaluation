# Road-damage object-detection benchmark

## Method

| Axis | Recorded condition |
|---|---|
| Model | COCO-pretrained YOLO11n |
| Data | public Road Damage Dataset: Potholes, Cracks and Manholes, COCO annotations |
| Split | 80/20 by image after shuffling with seed 0 |
| Training | 640 px, batch 16, maximum 60 epochs, patience 15, time cap 0.33 hours |
| Hardware/logged libraries | Tesla T4; Ultralytics 8.4.194; torch 2.11.0+cu128 |
| AP evaluation | 640 px, confidence 0.001, IoU 0.6 |
| Threshold sweep | 0.05–0.90 in increments of 0.05; same-class greedy confidence matching at IoU ≥ 0.5 |

The experiment code and exact Kaggle command sequence are in [`notebooks/yolo-road-damage-honest-evaluation.ipynb`](notebooks/yolo-road-damage-honest-evaluation.ipynb). To reproduce, use **Run All** in the public [Kaggle notebook](https://www.kaggle.com/code/lyftium/yolo-road-damage-honest-evaluation) after enabling GPU and reviewing its package/model downloads. The notebook writes the raw [`summary.json`](results/summary.json).

## Measured results

| Metric | Result |
|---|---:|
| Held-out images | 402 |
| mAP@50 | 0.526 |
| mAP@50–95 | 0.222 |
| Missed boxes at selected best-F1 thresholds | 564 |
| False alarms at selected best-F1 thresholds | 296 |

| Class | AP@50 | Best-F1 threshold | F1 | Precision | Recall |
|---|---:|---:|---:|---:|---:|
| Crack | 0.371 | 0.35 | 0.404 | 0.506 | 0.336 |
| Manhole | 0.731 | 0.45 | 0.727 | 0.820 | 0.653 |
| Pothole | 0.476 | 0.25 | 0.483 | 0.541 | 0.435 |

All values are from `results/summary.json` at three-decimal recorded precision. Class F1 values are shown separately; the file contains no macro-F1 field, so none is claimed.

## Interpretation

The same model behaves differently across classes: manholes score highest on AP and cracks lowest. A single confidence threshold is not optimal for all three. Error counts are evaluated at each class's best validation-F1 threshold, not at Ultralytics's default threshold.

## Limitations

One recorded run in the raw summary; short time-capped training; no confidence intervals. The held-out split is used for both model selection and threshold tuning, so these are optimistic validation results without an independent test set. Random image splitting may permit near-duplicate frames across train and validation; it does not test geographic or temporal generalization. Exact rerun behavior can differ with time-capped GPU training and unpinned package installation. The complete dataset and model artifacts are not redistributed.
