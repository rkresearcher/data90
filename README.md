# 🧠 YOLOv11 + Hailo-8L Real-Time Object Detection

This project demonstrates how to use **Ultralytics YOLOv11** to train an object detection model with a Roboflow-annotated dataset, convert it to ONNX, compile it for **Hailo-8L** AI accelerator, and run real-time inference.

---

## 📁 Dataset: Roboflow + YOLO Format

- Annotated using: [Roboflow](https://roboflow.com)
- Export format: YOLO (compatible with Ultralytics)
- Label format per line:
```

\<class\_id> \<x\_center> \<y\_center> <width> <height>  # all values normalized

```

### 🏷️ Classes:
```

0: Trash


## 🏋️ Training with Ultralytics YOLOv11
python3 -m venv python_yolo
source python_yolo/bin/activate
### 1. Install YOLOv11

```bash
pip install ultralytics
````

> Requires Python 3.8+, PyTorch 1.9+, and a CUDA-compatible GPU for training.

### 2. Train Your Model
   
```bash
yolo train model=yolov11s.pt data=data.yaml epochs=100 imgsz=640
```
dataset/
├── train/
│ ├── images/
│ │ ├── image1.jpg
│ │ ├── image2.jpg
│ └── labels/
│ ├── image1.txt
│ ├── image2.txt
├── valid/
│ ├── images/
│ └── labels/
├── test/
│ ├── images/
│ └── labels/
└── data.yaml

* `data.yaml` contains paths and class names.
* Output will be saved in `runs/detect/train/`.

---

## 🔄 Export to ONNX

After training completes:

```bash
yolo export model=runs/detect/train/weights/best.pt format=onnx
```

This creates `best.onnx`.

---

## ⚙️ Compile ONNX to Hailo `.hef`

use run.sh

## 🎯 Run Inference on Hailo-8L

### Image Input:
  install HailoRT .deb on the RPI.
  sudo dpkg -i hailoRT<>.deb            In current RPI 4.20 version is installed
  git clone https://github.com/hailo-ai/hailo-rpi5-examples.git
  cd hailo-rpi5-examples
  ./install.sh
  source setup_env.sh
### Webcam Input:

```bash
 python basic_pipelines/detection.py --input rpi --hef-path /path/to/yolov5st.hef
```

📸 Outputs:

* Bounding boxes
* Class labels ("Trash")
* Confidence scores

---
---

## 📌 Notes

* Hailo-8L requires `.hef` format models.
* Ultralytics makes ONNX export straightforward.
* Roboflow is used for dataset preparation.
