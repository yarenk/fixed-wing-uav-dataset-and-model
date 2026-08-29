# Fixed-Wing UAV Detection — Dataset & Model

![Model](https://img.shields.io/badge/model-YOLOv5s-blue)
![Task](https://img.shields.io/badge/task-object%20detection-orange)
![License](https://img.shields.io/badge/code-MIT-green)
![Dataset](https://img.shields.io/badge/dataset-CC%20BY%204.0-lightgrey)

An open-source labeled image dataset and a trained YOLOv5s detector for **fixed-wing UAV recognition**.
Built for the Teknofest Fighting UAV Competition, where the model had to detect opposing fixed-wing
aircraft from an onboard camera in real time.

🇹🇷 Türkçe için: [README_TR.md](README_TR.md)

---

## Contents

| File | Description |
|---|---|
| `best.pt` | Trained YOLOv5s weights |
| [Dataset (Release v1)](https://github.com/yarenk/fixed-wing-uav-dataset-and-model/releases/tag/version1) | Labeled images + YOLO-format annotations |
| `metrics.png` | Training curves and evaluation metrics |
| `Example.png` | Sample detections |

---

## Dataset

| | |
|---|---|
| **Total images** | TODO_IMAGE_COUNT |
| **Classes** | 1 (`iha` — fixed-wing UAV) |
| **Image format** | JPEG / PNG |
| **Annotation format** | YOLO (one `.txt` per image: class + normalized bounding box) |
| **Split** | TODO_SPLIT_RATIO (train / val) |
| **Annotation tool** | [Roboflow](https://roboflow.com), manual bounding boxes |

### Directory structure

```
dataset/
├── train/
│   ├── images/     # Training images
│   └── labels/     # YOLO-format annotations for training images
└── val/
    ├── images/     # Validation images
    └── labels/     # YOLO-format annotations for validation images
```

### `dataset.yaml`

```yaml
path: ./dataset
train: train/images
val: val/images

nc: 1
names: ['iha']
```

---

## Results

Evaluated on the validation split:

| Metric | Score |
|---|---|
| Precision | 99.2% |
| Recall | 98.6% |
| mAP@0.5 | 98.4% |

Training curves are in `metrics.png`.

> **Note on these numbers.** They come from the validation split, not a separately held-out
> test set, and the task is single-class detection on a relatively homogeneous set of scenes.
> They should be read as "the model learned this dataset well", not as a guarantee of
> performance on arbitrary footage. See [Limitations](#limitations).

---

## Quick start

### Inference (PyTorch Hub)

```python
import torch

model = torch.hub.load('ultralytics/yolov5', 'custom', path='best.pt')
model.conf = 0.25  # confidence threshold

results = model('your_image.jpg')
results.print()
results.show()
results.pandas().xyxy[0]  # detections as a DataFrame
```

### Inference (YOLOv5 CLI)

```bash
git clone https://github.com/ultralytics/yolov5
cd yolov5 && pip install -r requirements.txt

python detect.py \
  --weights ../best.pt \
  --source ../your_images/ \
  --img 640 \
  --conf 0.25
```

### Reproduce training

```bash
python train.py \
  --img 640 \
  --batch 16 \
  --epochs 100 \
  --data dataset.yaml \
  --weights yolov5s.pt
```

---

## Training configuration

| Setting | Value |
|---|---|
| Base model | YOLOv5s |
| Image size | 640 × 640 |
| Batch size | 16 |
| Epochs | 100 |
| Optimizer | SGD with learning-rate scheduling |
| Hardware | GPU |

---

## Limitations

Worth knowing before you use this:

- **Single class.** The model only distinguishes "fixed-wing UAV" from background. It will not
  separate UAV types, and it has not been tested against rotary-wing aircraft or birds.
- **Competition-specific imagery.** The data reflects the conditions of the Teknofest Fighting
  UAV Competition — daylight, open sky, a limited set of airframes. Expect degradation on
  cluttered backgrounds, low light, or unusual viewing angles.
- **Validation-set metrics.** See the note under [Results](#results).
- **Small-object performance is untested.** Detection quality at long range was not measured
  separately.

Pull requests that extend the dataset with harder cases are welcome.

---

## License

- **Code and trained weights:** MIT License
- **Dataset:** CC BY 4.0 — free to use with attribution

## Citation

If this dataset or model is useful in your work, please cite it:

```bibtex
@misc{koc_uav_dataset,
  author       = {Yaren Koç},
  title        = {Fixed-Wing UAV Detection Dataset and Model},
  year         = {2024},
  publisher    = {GitHub},
  howpublished = {\url{https://github.com/yarenk/fixed-wing-uav-dataset-and-model}}
}
```

## Acknowledgements

- [YOLOv5](https://github.com/ultralytics/yolov5) by Ultralytics
- [Roboflow](https://roboflow.com) for annotation tooling
- HAVTEK Student Club, Erciyes University

## Contact

Questions or suggestions — open an issue, or reach me at **TODO_EMAIL**.
