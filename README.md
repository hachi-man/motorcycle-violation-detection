# Motorcycle Traffic Violation Detection System

A computer vision system that detects motorcycle traffic violations from road footage using YOLOv8. Built as part of the PITB AIRI Team AI Internship Task 1, this project addresses two violation types on Punjab's e-challan enforcement roadmap: helmet non-compliance and motorcycle overloading (3+ riders).

## System Architecture

```
Input Image / Video Frame
        ↓
YOLOv8n Detection Model (best.pt)
        ↓
┌─────────────────────────────────────┐
│  helmet             → compliant     │
│  no_helmet          → VIOLATION     │
│  motorcycle_normal  → compliant     │
│  motorcycle_overloaded → VIOLATION  │
└─────────────────────────────────────┘
        ↓
Annotated Output with Bounding Boxes,
Class Labels, and Confidence Scores
```

## Results — Best Model (YOLOv8n, 40 epochs, batch=16)

| Class | Precision | Recall | mAP@0.5 | mAP@0.5:0.95 |
|---|---|---|---|---|
| **All classes** | **0.869** | **0.890** | **0.926** | **0.688** |
| helmet | 0.837 | 0.886 | 0.900 | 0.644 |
| motorcycle_normal | 0.953 | 0.918 | 0.968 | 0.791 |
| motorcycle_overloaded | 0.862 | 0.895 | 0.928 | 0.775 |
| no_helmet | 0.823 | 0.860 | 0.910 | 0.541 |

## Dataset

| Split | Images |
|---|---|
| Train | 405 |
| Validation | 116 |
| Test | 57 |
| **Total** | **578** |

- **Sources:** Multiple public datasets from Roboflow Universe and Kaggle (no annotations used — all 578 images manually re-annotated from scratch using Roboflow)
- **Annotation tool:** Roboflow Annotate
- **Format:** YOLOv8 (YOLO txt format, normalized coordinates)
- **Classes:** 4 — helmet, motorcycle_normal, motorcycle_overloaded, no_helmet

## Training — 6 Experiments

| # | Model | Epochs | Batch | cls | mAP@0.5 | Notes |
|---|---|---|---|---|---|---|
| 1 | YOLOv8n | 30 | 8 | default | 0.919 | Baseline |
| 2 | **YOLOv8n** | **40** | **16** | **default** | **0.926** | **Best** |
| 3 | YOLOv8n | 40 | 8 | default | 0.923 | |
| 4 | YOLOv8n | 75 | 8 | 1.5 | 0.916 | Overfitting begins |
| 5 | YOLOv8n | 75 | 16 | default | 0.916 | |
| 6 | YOLOv8s | 75 | 8 | 1.5 | 0.906 | Worst — overfitting |

**Key finding:** YOLOv8s (11M parameters) performed worse than YOLOv8n (3M parameters). With 578 images, the larger model overfits while the smaller model generalizes better. Model complexity must match dataset size.

## Installation

```bash
git clone https://github.com/hachi-man/motorcycle-violation-detection
cd motorcycle-violation-detection
pip install -r requirements.txt
```

## Run Inference

**On an image:**
```python
from ultralytics import YOLO
model = YOLO("models/best.pt")
results = model.predict(
    source="your_image.jpg",
    conf=0.25,
    iou=0.4,
    agnostic_nms=True,
    save=True
)
```

**On a video:**
```python
model = YOLO("models/best.pt")
results = model.track(
    source="your_video.mp4",
    tracker="bytetrack.yaml",
    conf=0.25,
    save=True
)
```

## Project Structure

```
motorcycle-violation-detection/
│
├── dataset/
│   ├── images/
│   │   ├── train/          # 405 training images
│   │   ├── val/            # 116 validation images
│   │   └── test/           # 57 test images
│   ├── labels/
│   │   ├── train/
│   │   ├── val/
│   │   └── test/
│   └── data.yaml
│
├── notebooks/
│   └── training_notebook.ipynb
│
├── outputs/
│   ├── training_results/   # All 6 experiment results
│   ├── predictions/        # Test set inference images
│   └── demo_results/       # Video inference output
│
├── models/
│   └── best.pt             # Final model weights
│
├── report/
│   ├── final_report.pdf
│   └── problem_definition.md
│
├── error_analysis/
│   ├── error_analysis.md
│   └── error_analysis_images/
│
├── README.md
├── requirements.txt
├── .gitignore
├── annotation_guidelines.txt
└── data_sources.txt
```

## PITB E-Challan Alignment

Punjab's Safe City Authority (PSCA) and PITB are actively developing AI-based e-challan features. This project prototypes two violations on their planned deployment roadmap:
- **Helmet non-compliance** — explicitly listed as a planned AI detection feature by PSCA
- **Motorcycle overloading** — common Punjab road violation currently monitored manually

## Tools and Technologies

Python · Google Colab (Tesla T4 GPU) · YOLOv8 · Ultralytics · Roboflow · OpenCV · PyTorch · Matplotlib

## Dataset and Model Weights

Full dataset and model weights available on Google Drive:
`[Add your Google Drive link here]`

## License

Built for educational and research purposes as part of the PITB AIRI Team AI Internship.
