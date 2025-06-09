# 🧠 YOLOv11 + Hailo-8L Real-Time Object Detection

This project demonstrates how to use **Ultralytics YOLOv11** to train an object detection model with a **Roboflow-annotated dataset**, convert it to **ONNX**, compile it for the **Hailo-8L AI accelerator**, and run **real-time inference**.

---

## 📁 Dataset

- **Source**: Roboflow  
- **Format**: YOLO (compatible with Ultralytics)
### 🏷️ Classes:
```

0: Trash

````

---

## 🏋️ Training with Ultralytics YOLOv11

### 1. Setup Environment

```bash
python3 -m venv python_yolo
source python_yolo/bin/activate
````

### 2. Install YOLOv11

```bash
pip install ultralytics
```

> **Requirements**: Python 3.8+, PyTorch 1.9+, and a CUDA-compatible GPU for training.

### 3. Train Your Model

```bash
yolo train model=yolov11s.pt data=data.yaml epochs=100 imgsz=640
```

### Dataset Structure

```
dataset/
├── train/
│   ├── images/
│   │   ├── image1.jpg
│   └── labels/
│       ├── image1.txt
├── valid/
│   ├── images/
│   └── labels/
├── test/
│   ├── images/
│   └── labels/
└── data.yaml
```

* `data.yaml` includes paths and class names.
* Output is saved in: `runs/detect/train/`

---

## 🔄 Export to ONNX

After training is complete, export the best weights to ONNX format:

```bash
yolo export model=runs/detect/train/weights/best.pt format=onnx
```

> This creates `best.onnx`.

---

## ⚙️ Compile ONNX to Hailo .hef

Use the provided `run.sh` script to compile `best.onnx` into a `.hef` file compatible with Hailo-8L.

---

## 🎯 Run Inference on Hailo-8L

### 🖼️ Image Input

1. Install HailoRT on Raspberry Pi:

```bash
sudo dpkg -i hailoRT<version>.deb
```

(Current version: RPI 4.20)

2. Clone and install Hailo examples:

```bash
git clone https://github.com/hailo-ai/hailo-rpi5-examples.git
cd hailo-rpi5-examples
./install.sh
source setup_env.sh
```

---

### 📷 Webcam Input

Run the detection pipeline using:

```bash
python basic_pipelines/detection.py --input rpi --hef-path /path/to/yolov5st.hef
```

---

## 📸 Outputs

* Bounding Boxes
* Class Label: `Trash`
* Confidence Scores

---

## 📌 Notes

* **Hailo-8L** requires models in `.hef` format.
* **Ultralytics** supports easy ONNX export.
* **Roboflow** is used for dataset preparation and annotation.

---

## 🧩 Dependencies Summary

* Python 3.8+
* PyTorch 1.9+
* Ultralytics (YOLOv11)
* HailoRT
* ONNX Runtime (for testing ONNX before compiling)

```

Let me know if you face any issue
```
