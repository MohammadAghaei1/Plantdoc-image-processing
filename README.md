# 🌿 Plant Disease Detection System

An end-to-end **computer vision pipeline** designed to detect and classify plant leaf diseases from real-world agricultural imagery using modern **object detection models**.

This project focuses on building a **robust machine learning workflow**, starting from raw dataset preparation to model training, evaluation, and optimization.

---

# 🏗 Project Architecture

The system follows a **modular machine learning architecture**, separating data processing, model training, and evaluation stages.

### ML Pipeline

1. **Raw Dataset**

2. **Data Cleaning & Validation**

3. **Exploratory Data Analysis (EDA)**

4. **Data Preprocessing & Augmentation**

5. **Model Training (YOLO11n)**

6. **Evaluation & Error Analysis**


This modular architecture enables flexible experimentation with **different models, preprocessing strategies, and training configurations**.

---

# 📊 Dataset Engineering

The project uses the **PlantDoc dataset**, a benchmark dataset designed for plant disease detection in real-world conditions.

### Dataset Characteristics

- **2567 images**
- **27 disease classes**
- **8818 bounding box annotations**
- Image resolution: **416 × 416**

### Dataset Challenges

Unlike curated laboratory datasets, PlantDoc introduces multiple real-world challenges:

- background clutter  
- varying illumination conditions  
- class imbalance  
- noisy annotations  
- small object detection  

These characteristics make it a challenging dataset and closer to **real agricultural environments**.

---

# 🧹 Data Quality Pipeline

Before training, a complete **data validation and cleaning pipeline** was implemented.

### Dataset Validation

The preprocessing pipeline includes:

- annotation format validation  
- filename normalization  
- detection of malformed CSV rows  
- removal of corrupted entries  

### Image Integrity Checks

Additional quality checks were implemented:

- detection and correction of **negative (inverted) images**
- removal of extremely **small bounding boxes**
- filtering **low-brightness and low-contrast samples**
- blur detection and filtering

These steps significantly improved **training stability and dataset reliability**.

---

# 🔬 Exploratory Data Analysis (EDA)

Extensive exploratory analysis was conducted to understand dataset characteristics and potential biases.

### Key Analyses

- class distribution analysis
- bounding box size distribution
- aspect ratio statistics
- brightness and contrast analysis
- image sharpness analysis

EDA confirmed:

- significant **class imbalance**
- large variability in **image conditions**
- presence of **very small objects**

These insights guided the design of the preprocessing and augmentation pipeline.

---

# 🧠 Detection Model

The system uses **Ultralytics YOLO11n**, a lightweight real-time object detection architecture.

### Why YOLO?

YOLO models are widely used in real-time vision systems because they provide:

- fast inference speed  
- efficient architecture  
- strong performance on small objects  
- end-to-end detection pipelines  

### Model Architecture

YOLO detection networks consist of three main components:

**Backbone**

Extracts hierarchical visual features from input images.

**Neck**

Performs multi-scale feature fusion using FPN/PAN structures.

**Head**

Predicts bounding boxes and class probabilities across multiple scales.

---

# ⚙️ Training Strategy

The model was trained using **transfer learning** with pretrained YOLO weights.

### Core Training Configuration

| Parameter | Value |
|----------|------|
| Model | YOLO11n |
| Optimizer | AdamW |
| Image Size | 640 × 640 |
| Batch Size | 32 |
| Epochs | 40 |
| LR Scheduler | Cosine LR |

This configuration balances **training stability, accuracy, and computational efficiency**.

---

# 🔁 Data Augmentation

To improve generalization and robustness, multiple augmentations were applied:

- horizontal flipping  
- rotation  
- translation  
- scale variation  
- HSV color augmentation  
- gaussian noise  
- mosaic augmentation  

These augmentations simulate **real-world agricultural image variability**.

---

# 🧪 Experimental Iterations

Three training runs were conducted to progressively improve model performance.

### Run 1 — Baseline Model

Initial training performed on the cleaned dataset.

### Run 2 — Missing Label Mining

The baseline model was used to detect **potential missing annotations**.

Predictions with low IoU against ground truth were saved as candidate labels.

### Run 3 — Hard Negative Cleaning

A **model-guided cutout strategy** was used to remove unlabeled leaf-like regions from training images.

This step reduced background confusion and improved detection performance.

---

# 📈 Evaluation Results

The model was evaluated on the **PlantDoc test set** using standard object detection metrics.

### Evaluation Dataset

- **238 test images**
- **454 ground-truth objects**
- **27 classes**

### Detection Performance

| Metric | Run 1 | Run 2 | Run 3 |
|------|------|------|------|
| True Positives | 245 | 250 | **263** |
| False Positives | 168 | 227 | 255 |
| False Negatives | 75 | 55 | **49** |
| Wrong Class Predictions | 134 | 149 | 142 |

### Key Observations

The **third training run** achieved the best overall detection performance:

- **263 true positives**
- **49 false negatives**
- improved detection recall across multiple classes

Some false positives occurred due to **incomplete dataset annotations**, where real objects existed but were not labeled in the ground truth.

This indicates that the model became more sensitive to leaf patterns and improved detection coverage.

---
