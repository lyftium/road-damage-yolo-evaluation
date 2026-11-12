# Road-damage YOLO evaluation

Supporting methods exercise (not the industrial drawing-review line).

I evaluated a small YOLO11n detector on public road-damage images, focusing on per-class AP, confidence-threshold selection, and error analysis rather than reporting one aggregate score.

**Saved result:** mAP@50 **0.526** and mAP@50–95 **0.222** on **402 held-out images**. Manhole AP@50 was 0.731, compared with 0.476 for potholes and 0.371 for cracks.

[Benchmark report](BENCHMARK.md) · [Raw summary](results/summary.json) · [Source notebook](notebooks/yolo-road-damage-honest-evaluation.ipynb) · [Public Kaggle notebook](https://www.kaggle.com/code/lyftium/yolo-road-damage-honest-evaluation) · [Public dataset](https://www.kaggle.com/datasets/lorenzoarcioni/road-damage-dataset-potholes-cracks-and-manholes)

## Reproduce

Open the public Kaggle notebook with GPU enabled and inspect the notebook before choosing **Run All**. It downloads the configured public dataset, pretrained YOLO weights, and Ultralytics packages. The notebook performs the image split, time-capped training, held-out evaluation, per-class threshold sweep, and writes `summary.json`.

Only the notebook and measured JSON summary are included here: no dataset images, copied labels, model weights, or image-drawing outputs are redistributed.

## Limits

The split is random by image with seed 0 rather than by location/date. The same held-out split is used for model selection and threshold tuning, so those threshold results are optimistic; there is no independent test set. Training is short and time-capped, with one recorded run in the raw summary. The small probe is a methods exercise on public road images, not a technical-drawing or production benchmark.
