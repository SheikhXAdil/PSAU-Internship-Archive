# PSAU Internship — Edge AI Deployment Pipeline

A collection of the **research, experiments, implementations, documentation, and presentation** produced during my internship at the **Research & Development Center, Prince Sattam bin Abdulaziz University (PSAU)**.

The internship focused on investigating and developing a practical **Edge AI deployment pipeline for resource-constrained embedded systems**, with particular emphasis on model optimization, conversion, deployment, and performance evaluation.

This repository serves as the **central archive for the internship work**. The larger implementations are also maintained in separate repositories on my GitHub profile.

## Internship Focus

The primary objective of the internship was to investigate how AI models can be prepared and deployed on resource-constrained edge hardware.

The work covered the complete path from:

```text
Dataset
   ↓
Model Training
   ↓
Model Evaluation
   ↓
Model Optimization
   ↓
Model Conversion
   ↓
Embedded Deployment
   ↓
On-Device Inference
   ↓
Performance Evaluation
```

The work explored different hardware platforms and deployment approaches, including **ESP32, AMB82-Mini, and Raspberry Pi**.

## Projects & Experiments

### 1. ESP32 MNIST — TensorFlow Lite Micro

An experimental **TinyML deployment pipeline** for running a small neural network on an ESP32.

The experiment used the MNIST dataset to train a lightweight CNN, perform **full INT8 quantization**, convert the model to TensorFlow Lite, embed the model into a C/C++ source file, and run inference directly on the ESP32 using **TensorFlow Lite Micro** and **ESP-IDF**.

The experiment demonstrated:

* Training a lightweight neural network
* TensorFlow Lite model conversion
* Full INT8 quantization
* Embedded model packaging
* TensorFlow Lite Micro inference
* On-device inference
* Input quantization and output dequantization
* Embedded inference timing

This experiment was primarily a **working proof of concept** and was not part of the main internship documentation.

**Implementation:**
[ESP32 MNIST TensorFlow Lite Micro](./esp32_mnist_tflm)

The experiment is also maintained as part of the internship archive and the larger ESP32 work can be found in its dedicated repository on my GitHub profile.

---

### 2. YOLOv7 Drone Detection — AMB82-Mini

The main computer-vision project of the internship was the development and deployment of a **custom drone detection model** on the **AMB82-Mini** edge device.

A YOLOv7-based object detection model was trained using the **YOLO Drone Detection Dataset** and subsequently deployed on the AMB82-Mini.

The work involved:

* Dataset preparation
* YOLOv7 model training
* Model evaluation
* Model conversion for embedded deployment
* Deployment to AMB82-Mini
* Camera-based inference
* On-device performance evaluation
* FPS and inference benchmarking

**Dataset:**
[YOLO Drone Detection Dataset — Kaggle](https://www.kaggle.com/datasets/muki2003/yolo-drone-detection-dataset)

**Implementation:**
[YOLOv7 Drone Detection — AMB82-Mini](./yolo-drone-detection-amb82-mini)

The deployment findings, performance observations, and technical analysis are documented in the internship documentation included in this repository.

---

### 3. YOLOv7 Drone Detection — Raspberry Pi

The next stage of the project extended the drone detection pipeline to a **Raspberry Pi**.

The trained YOLOv7 model was adapted for deployment on the Raspberry Pi, with the implementation focusing on running inference under the considerably more constrained computational environment of the device.

The work involved:

* Model conversion for Raspberry Pi deployment
* NCNN-based inference
* Camera integration
* Image preprocessing
* Object detection and postprocessing
* On-device inference
* FPS and latency measurement
* Resource and performance investigation

This stage provided practical insight into the difference between deploying an optimized model on a dedicated embedded AI platform and running the same detection workload on a low-resource general-purpose edge computer.

**Implementation:**
[YOLOv7 Drone Detection — Raspberry Pi](./yolo-drone-detection-raspberry-pi)

The Raspberry Pi implementation and its performance findings are documented in the internship documentation.

---

### 4. YOLO11n Drone Detection — Raspberry Pi Optimization

To improve the performance of the Raspberry Pi deployment, the next iteration moved from YOLOv7 to the significantly smaller **YOLO11n** model.

The model was trained using the **same drone detection dataset** and evaluated as a potential replacement for the earlier YOLOv7 model.

The objective was to investigate whether a smaller and more efficient model could provide better real-time performance on the Raspberry Pi while maintaining useful detection capability.

The work involved:

* Training YOLO11n on the drone dataset
* Model evaluation
* Model conversion to NCNN
* Raspberry Pi deployment
* Camera-based inference
* Performance benchmarking
* Comparison with the YOLOv7 deployment
* Investigation of inference latency and FPS

The experiment demonstrated the practical importance of **model selection and optimization for edge deployment**, where computational resources are significantly more limited than on the training machine.

**Implementation:**
[YOLO11n Drone Detection](./yolov11n-drone-detection)

---

## Dataset

The primary dataset used for the drone detection experiments was the **YOLO Drone Detection Dataset** by `muki2003`.

**Dataset:**
[YOLO Drone Detection Dataset — Kaggle](https://www.kaggle.com/datasets/muki2003/yolo-drone-detection-dataset)

The dataset was used for both the YOLOv7 and YOLO11n training experiments, allowing the different model approaches to be evaluated under the same dataset conditions.

## Documentation

The repository contains the complete internship documentation describing the research, methodology, experiments, deployment process, results, and findings.

### Technical Documentation

[Edge AI Deployment Pipeline Documentation.pdf](./Edge%20AI%20Deployment%20Pipeline%20Documentation.pdf)

The documentation covers the broader investigation into:

* Edge AI deployment requirements
* Model evaluation criteria
* Dataset considerations
* Model optimization
* Quantization
* Model conversion
* Embedded deployment
* Raspberry Pi deployment
* ESP32 experimentation
* AMB82-Mini deployment
* Performance evaluation
* Deployment challenges and findings

### Presentation

[Edge AI Deployment Pipeline Presentation.pptx](./Edge%20AI%20Deployment%20Pipeline%20Presentation.pptx)

The presentation summarizes the internship work, methodology, implementations, results, and key findings.

## Repository Structure

```text
PSAU-Internship-Archive/
│
├── esp32_mnist_tflm/
│   └── ESP32 TensorFlow Lite Micro experiment
│
├── yolo-drone-detection-amb82-mini/
│   └── YOLOv7 drone detection on AMB82-Mini
│
├── yolo-drone-detection-raspberry-pi/
│   └── YOLOv7 drone detection on Raspberry Pi
│
├── yolov11n-drone-detection/
│   └── YOLO11n drone detection and Raspberry Pi optimization
│
├── Edge AI Deployment Pipeline Documentation.pdf
│   └── Complete technical internship documentation
│
└── Edge AI Deployment Pipeline Presentation.pptx
    └── Internship presentation
```

The repository structure reflects the progression of the work from an initial embedded ML experiment to increasingly practical **object detection deployment and optimization**.

## Main Technical Areas

Throughout the internship, I worked with:

### Machine Learning & Computer Vision

* YOLOv7
* YOLO11n
* Object detection
* Model training
* Model evaluation
* Precision, mAP, IoU, and related metrics
* Dataset preparation

### Edge AI & Model Deployment

* TensorFlow Lite
* TensorFlow Lite Micro
* NCNN
* INT8 quantization
* Model conversion
* Embedded model packaging
* On-device inference

### Hardware

* ESP32
* AMB82-Mini
* Raspberry Pi

### Development Environment

* Python
* C/C++
* ESP-IDF
* OpenCV
* PyTorch
* Kaggle
* Raspberry Pi / Linux

## Key Findings

One of the major lessons from this work was that **successful model deployment on edge hardware is not simply a matter of converting a trained model**.

Practical deployment requires consideration of:

* Hardware computational capability
* Available memory
* Model size and complexity
* Inference runtime compatibility
* Input resolution
* Quantization
* Preprocessing overhead
* Postprocessing overhead
* Camera and I/O overhead
* Inference latency
* Real-world FPS

The YOLOv7 → Raspberry Pi → YOLO11n progression provided a practical demonstration of how **model architecture and optimization directly affect real-world edge inference performance**.

The detailed measurements and findings from these experiments are available in the technical documentation and presentation included in this repository.

## Internship Outcome

The internship resulted in a reusable conceptual pipeline for taking an AI model from **training to deployment on resource-constrained hardware**.

The work provided hands-on experience across the complete deployment lifecycle:

```text
Problem
  ↓
Dataset
  ↓
Model Selection
  ↓
Training
  ↓
Evaluation
  ↓
Optimization
  ↓
Conversion
  ↓
Hardware Deployment
  ↓
Inference
  ↓
Benchmarking
```

Rather than focusing only on model accuracy, the internship emphasized the practical trade-offs involved in deploying AI models where **memory, compute, latency, and runtime compatibility** are constrained.

## Related Repositories

The individual implementations from this internship are maintained separately where appropriate.

This repository acts as the **central archive containing the documentation, presentation, and internship work**, while the dedicated project repositories contain the corresponding implementation code.

## Internship

**Organization:** Research & Development Center, Prince Sattam bin Abdulaziz University (PSAU)

**Focus:** Edge AI, Embedded AI, Computer Vision, Model Optimization, and AI Deployment

**Primary Hardware:** ESP32, AMB82-Mini, Raspberry Pi

## Status

**Completed internship project archive.**

This repository is preserved as a complete record of the technical work, experiments, implementations, documentation, and presentation produced during the internship.

The individual experiments and deployment implementations are maintained alongside this archive to make the work easier to explore independently.
