# PSAU Internship — Edge AI Deployment Pipeline

A collection of the research, experiments, implementations, and documentation developed during my internship at the **Research & Development Center, Prince Sattam bin Abdulaziz University (PSAU)**.

The internship focused on investigating and developing a practical **Edge AI deployment pipeline for resource-constrained hardware**, covering the process from model training and optimization to conversion, deployment, and on-device inference.

The work included experiments with **ESP32, AMB82-Mini, and Raspberry Pi Zero 2 W**, along with different machine-learning models, runtimes, optimization techniques, and deployment environments.

## Overview

The broader objective of the internship was to understand the practical requirements and challenges involved in deploying machine-learning models on constrained edge devices.

The work followed a general pipeline:

```text
Problem Definition
       ↓
Dataset Acquisition
       ↓
Model Training
       ↓
Model Evaluation
       ↓
Model Optimization
       ↓
Model Conversion
       ↓
Hardware Deployment
       ↓
On-Device Inference
       ↓
Performance Evaluation
       ↓
Optimization / Iteration
```

The experiments progressed from a small **TinyML classification workload on ESP32** to more demanding **computer-vision object detection workloads** on the AMB82-Mini and Raspberry Pi Zero 2 W.

---

## Internship Work

The internship consisted of several interconnected experiments.

### 1. ESP32 MNIST — TensorFlow Lite Micro

The initial experiment focused on deploying a small neural network to an **ESP32 microcontroller** using **TensorFlow Lite Micro (TFLM)** and **ESP-IDF**.

The MNIST handwritten-digit dataset was used to train a small CNN, which was then converted to TensorFlow Lite and fully quantized to **INT8** before being embedded into the ESP32 firmware.

The complete pipeline was:

```text
MNIST Dataset
      ↓
Train Small CNN
      ↓
Full INT8 Quantization
      ↓
TensorFlow Lite Model
      ↓
C/C++ Model Representation
      ↓
ESP32 + TensorFlow Lite Micro
      ↓
On-Device Inference
```

The experiment demonstrated:

* Training a small neural network
* TensorFlow Lite conversion
* Full INT8 quantization
* Embedded model packaging
* TensorFlow Lite Micro integration
* ESP-IDF development
* Quantized input and output handling
* On-device inference
* Embedded inference timing and benchmarking

The deployment was intentionally **camera-free**, using embedded MNIST test images to validate the ML deployment pipeline independently of camera and sensor integration.

The complete implementation is maintained separately:

**[PSAU Internship — ESP32 MNIST TFLite Micro](https://github.com/SheikhXAdil/PSAU-Internship-ESP32-MNIST-TFLiteMicro)**

---

# 2. YOLO Drone Detection

The second major part of the internship focused on **computer-vision-based drone detection**.

A custom YOLO-based object detector was trained using a publicly available drone detection dataset and subsequently deployed across different edge platforms.

The work progressed through several iterations:

```text
Drone Detection Dataset
          ↓
      YOLOv7 Training
          ↓
   YOLOv7-tiny Deployment
          ↓
       AMB82-Mini
          ↓
 Raspberry Pi Zero 2 W
       + Ubuntu
          ↓
  Performance Investigation
          ↓
      YOLO11n Training
          ↓
     NCNN Conversion
          ↓
 Raspberry Pi Optimization
```

## Dataset

The drone detection experiments used the **YOLO Drone Detection Dataset** by `muki2003`:

**[YOLO Drone Detection Dataset — Kaggle](https://www.kaggle.com/datasets/muki2003/yolo-drone-detection-dataset)**

The same dataset was used for the YOLOv7 and YOLO11n experiments to provide consistent conditions when investigating different model approaches.

---

## 3. YOLOv7 Training

The initial drone detection model was based on **YOLOv7**.

The model was trained using the Kaggle drone detection dataset and subsequently prepared for deployment on the target edge hardware.

The training notebook is preserved as part of the YOLO project:

```text
yolo-drone-detection-amb82-mini/
└── training/
    └── yolo-training.ipynb
```

The training work involved:

* Dataset preparation
* YOLOv7 training
* Model evaluation
* Model export
* Preparing the model for edge deployment

The trained model was then taken through separate deployment paths for the AMB82-Mini and Raspberry Pi Zero 2 W.

---

# 4. YOLOv7-tiny on AMB82-Mini

The first computer-vision hardware deployment target was the **AMB82-Mini**.

The deployment was based on the **provided `ObjectDetectionLoop` example sketch**. Instead of developing a complete embedded object-detection application from scratch, the example was customized to run the **YOLOv7-tiny** model for the drone-detection task.

This involved adapting the existing embedded AI example to work with the custom-trained model and testing camera-based object detection directly on the AMB82-Mini.

The working implementation is maintained under:

```text
yolo-drone-detection-amb82-mini/
└── arduino_amb82/
    └── ObjectDetectionLoop/
```

### AMB82-Mini Benchmarking Experiment

A separate implementation called:

```text
ObjectDetectionLoopWithBenchmark/
```

was explored to add additional benchmarking functionality.

However, this implementation caused problems on the AMB82-Mini and did **not** become the successful deployment path.

The final working deployment therefore remained the customized:

```text
ObjectDetectionLoop/
```

example running the YOLOv7-tiny model.

This experiment was still useful for understanding the practical limitations of modifying an embedded AI example and the challenges involved in adding additional measurement functionality to a resource-constrained device.

### AMB82-Mini Testing

Sample drone images used during testing are preserved in:

```text
yolo-drone-detection-amb82-mini/
└── test_images/
```

The AMB82-Mini stage provided practical experience with:

* Embedded object detection
* Camera-based inference
* YOLOv7-tiny deployment
* Adapting hardware-specific AI examples
* Embedded resource constraints
* Real-time inference evaluation

---

# 5. YOLOv7 on Raspberry Pi Zero 2 W

The next stage moved the drone-detection workload to a **Raspberry Pi Zero 2 W running Ubuntu**.

This provided a different type of edge-computing environment from the AMB82-Mini.

Instead of a microcontroller-oriented environment, the Raspberry Pi provided a Linux-based system where the model could be integrated with Python-based computer-vision tools and an optimized inference runtime.

The Raspberry Pi implementation uses **NCNN** for model inference.

The implementation is maintained under:

```text
yolo-drone-detection-raspberry-pi/
```

with the main components including:

```text
best.pt
best.ncnn.param
best.ncnn.bin
detector.py
```

The deployment involved:

* Converting the trained YOLOv7 model to NCNN
* Running NCNN inference on the Raspberry Pi Zero 2 W
* Capturing camera frames
* Image preprocessing
* Object detection
* Bounding-box processing
* Confidence filtering
* Non-Maximum Suppression
* Measuring inference performance
* Streaming annotated detection output

The deployment provided practical experience with running a custom object detector on a **resource-constrained Linux-based edge computer**.

---

# 6. YOLO11n Raspberry Pi Optimization

The YOLOv7 Raspberry Pi deployment demonstrated that model complexity has a significant effect on inference performance when working with constrained hardware.

To investigate a more efficient approach, the next iteration used **YOLO11n**, a smaller YOLO model.

The model was trained using the same drone detection dataset and converted to **NCNN** for deployment on the Raspberry Pi Zero 2 W.

The experiment is maintained under:

```text
yolov11n-drone-detection/
```

and contains:

```text
metadata.yaml
model.ncnn.bin
model.ncnn.param
model_ncnn.py
yolo11n_detector.py
yolo11n_training.ipynb
```

The optimization experiment investigated:

* Smaller model architectures
* Model conversion
* NCNN inference
* Camera-based inference
* Inference latency
* FPS
* Preprocessing overhead
* Postprocessing overhead
* Frame processing
* Hardware limitations
* Model efficiency

The purpose was not simply to replace one model with another, but to investigate how **model selection affects the feasibility of real-time inference on constrained edge hardware**.

---

# 7. Edge AI Performance Investigation

Performance evaluation was a major component of the internship.

The experiments investigated the practical behavior of ML workloads on different edge platforms rather than evaluating models only in a conventional training environment.

Important factors included:

* Model architecture
* Model size
* Hardware compute capability
* Available memory
* Runtime compatibility
* Input resolution
* Quantization
* Model conversion
* Preprocessing
* Inference latency
* Postprocessing
* Camera I/O
* FPS
* Frame skipping
* CPU limitations

The experiments demonstrated that **successful model deployment does not necessarily mean successful real-time deployment**.

A model may produce correct detections while still being impractical because of its inference latency, memory requirements, preprocessing cost, or other hardware limitations.

This was particularly evident during the Raspberry Pi experiments, where the computational cost of the YOLOv7 model motivated the investigation of the smaller YOLO11n model.

---

# 8. Hardware Platforms

The internship involved three main classes of edge hardware.

## ESP32

Used for the initial TinyML experiment:

* MNIST classification
* Small CNN
* TensorFlow Lite Micro
* Full INT8 quantization
* ESP-IDF
* Embedded inference

## AMB82-Mini

Used for embedded computer vision:

* YOLOv7-tiny
* Custom drone detection
* Camera-based inference
* Customized `ObjectDetectionLoop`
* Embedded AI deployment

## Raspberry Pi Zero 2 W

Used for Linux-based edge inference:

* Ubuntu
* YOLOv7
* YOLO11n
* NCNN
* Python
* OpenCV
* Camera-based inference
* Performance investigation

The different platforms provided useful insight into how the same general Edge AI problem changes depending on the capabilities and software environment of the target hardware.

---

# 9. Technologies & Tools

### Machine Learning

* **YOLOv7**
* **YOLOv7-tiny**
* **YOLO11n**
* PyTorch
* TensorFlow / Keras
* TensorFlow Lite
* TensorFlow Lite Micro

### Edge AI

* **NCNN**
* TensorFlow Lite Micro
* Model conversion
* Quantization
* Embedded inference
* Performance benchmarking

### Computer Vision

* **OpenCV**
* Image preprocessing
* Letterbox resizing
* Bounding-box processing
* Non-Maximum Suppression
* Camera-based inference

### Embedded Development

* **ESP32**
* **AMB82-Mini**
* ESP-IDF
* Arduino
* C/C++

### Edge Computing

* **Raspberry Pi Zero 2 W**
* **Ubuntu**
* Python
* Linux-based deployment

### Development & Training

* Python
* Jupyter Notebook
* Kaggle
* PyTorch
* TensorFlow
* C/C++

---

# 10. Key Engineering Lessons

The internship provided practical experience with the complete path from an ML model to deployment on constrained hardware.

Some of the major lessons included:

### Model selection matters

A model that performs well on a conventional computer may not be suitable for real-time inference on constrained hardware.

Smaller and more efficient architectures can be more appropriate when latency and compute resources are limited.

### Model conversion is part of deployment

Training a model is only one stage of the process.

The model may need to be converted into a format supported by the target inference runtime, such as:

```text
PyTorch
   ↓
Export / Conversion
   ↓
NCNN / TensorFlow Lite
   ↓
Target Hardware
```

### Quantization can be important

The ESP32 experiment demonstrated the use of **full INT8 quantization** to make a neural network more suitable for microcontroller deployment.

### Hardware-specific constraints matter

Different edge devices require different deployment approaches.

The ESP32, AMB82-Mini, and Raspberry Pi Zero 2 W provide very different environments in terms of memory, compute resources, operating systems, available runtimes, and development workflows.

### Real-time performance requires more than inference speed

The practical performance of an application depends on the entire pipeline:

```text
Camera
  ↓
Frame Capture
  ↓
Preprocessing
  ↓
Model Inference
  ↓
Postprocessing
  ↓
Display / Output
```

Optimizing only the neural-network inference stage does not necessarily optimize the complete application.

### Existing examples can accelerate embedded deployment

The AMB82-Mini deployment demonstrated the usefulness of starting from a hardware-provided AI example and adapting it to a custom model.

The customized `ObjectDetectionLoop` provided the working foundation for the YOLOv7-tiny deployment.

---

# 11. Overall Deployment Pipeline

The different experiments can be viewed as parts of a generalized Edge AI deployment pipeline:

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
          Target Runtime Selection
                     ↓
              Hardware Deployment
                     ↓
             On-Device Inference
                     ↓
             Performance Testing
                     ↓
           Optimization / Iteration
```

The internship provided practical exposure to each of these stages through different hardware and ML workloads.

---

# 12. Repository Structure

This archive contains the main implementations and documentation from the internship:

```text
PSAU-Internship-Archive/
│
├── esp32_mnist_tflm/
│   └── ESP32 MNIST TensorFlow Lite Micro experiment
│
├── yolo-drone-detection-amb82-mini/
│   └── YOLOv7-tiny AMB82-Mini deployment
│
├── yolo-drone-detection-raspberry-pi/
│   └── YOLOv7 Raspberry Pi Zero 2 W deployment
│
├── yolov11n-drone-detection/
│   └── YOLO11n Raspberry Pi optimization experiment
│
├── Edge AI Deployment Pipeline Documentation.pdf
│   └── Detailed internship documentation
│
└── Edge AI Deployment Pipeline Presentation.pptx
    └── Internship presentation
```

The individual implementations are also maintained in dedicated repositories where appropriate.

---

# 13. Documentation

The complete internship research, methodology, experiments, findings, and conclusions are documented in the accompanying files:

### Technical Documentation

**`Edge AI Deployment Pipeline Documentation.pdf`**

Contains the detailed research and technical documentation covering the Edge AI deployment pipeline, hardware, model deployment, optimization, and experimental findings.

### Presentation

**`Edge AI Deployment Pipeline Presentation.pptx`**

Contains the presentation prepared to summarize the internship work, experiments, deployment process, and findings.

These documents provide the broader context behind the implementations preserved in this repository.

---

# 14. Related Repositories

### ESP32 MNIST

**[PSAU Internship — ESP32 MNIST TFLite Micro](https://github.com/SheikhXAdil/PSAU-Internship-ESP32-MNIST-TFLiteMicro)**

Contains the complete ESP32 TinyML deployment experiment using TensorFlow Lite Micro and INT8 quantization.

### YOLO Drone Detection

**[PSAU Internship — YOLO Drone Detection: AMB82-Mini & Raspberry Pi](https://github.com/SheikhXAdil/PSAU-Internship-Yolo-Drone-Detection-AMB82-Mini-Raspberry-Pi)**

Contains the detailed YOLOv7, YOLOv7-tiny, and YOLO11n training and deployment work across the AMB82-Mini and Raspberry Pi Zero 2 W.

---

# 15. Internship Outcome

The internship provided hands-on experience with the complete **Edge AI deployment lifecycle**, from training machine-learning models to running inference directly on constrained hardware.

The work progressed from a controlled TinyML experiment:

```text
MNIST
  ↓
Small CNN
  ↓
INT8 Quantization
  ↓
TensorFlow Lite Micro
  ↓
ESP32
```

to increasingly demanding computer-vision workloads:

```text
Drone Dataset
      ↓
   YOLOv7
      ↓
YOLOv7-tiny
      ↓
 AMB82-Mini
      ↓
Raspberry Pi Zero 2 W
      ↓
    YOLO11n
      ↓
 Raspberry Pi Optimization
```

This progression provided practical insight into the challenges involved in deploying AI models outside conventional computing environments.

The main outcome was the development of a **generalized understanding of an Edge AI deployment pipeline**, including:

* Dataset acquisition
* Model training
* Model evaluation
* Model selection
* Quantization
* Model conversion
* Runtime selection
* Hardware-specific deployment
* Camera and sensor integration
* On-device inference
* Performance measurement
* Model optimization

The internship demonstrated that successful Edge AI deployment requires consideration of the **entire system**, rather than the machine-learning model alone.

---

# 16. Status

**Completed internship archive.**

This repository preserves the research, experiments, implementations, and documentation developed during my internship at the **Research & Development Center, Prince Sattam bin Abdulaziz University (PSAU)**.

It serves as the central archive for my work on **Edge AI deployment for resource-constrained hardware**, including the ESP32 TinyML experiment and the YOLO-based drone-detection deployments on the AMB82-Mini and Raspberry Pi Zero 2 W.
