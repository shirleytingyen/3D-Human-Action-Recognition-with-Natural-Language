# Abstract

This project presents a lightweight end-to-end framework for human action recognition (HAR) and automated natural language captioning using Spatio-Temporal Graph Convolutional Networks (ST-GCN). By leveraging MediaPipe Pose Landmarker for 3D skeleton extraction, raw video streams are mapped into standardized COCO-17 keypoint dynamic graphs. Evaluated on the KTH dataset, the ST-GCN model achieves a test classification accuracy of 77.78%. The framework further integrates classification confidence scores with template-based text generation to produce real-time, human-readable action descriptions.

# Motivation

Traditional frame-based 3D Convolutional Neural Networks (3D-CNNs) incur heavy computational overhead and exhibit high sensitivity to ambient background noise. To resolve these limitations, utilizing lightweight pose estimation isolates human topology while completely filtering out redundant visual backgrounds. Unlike standard sequential architectures that flatten joint coordinates and forfeit intrinsic structural topology, Spatio-Temporal Graph Convolutional Networks (ST-GCN) explicitly represent human joint connections alongside their temporal motion trajectories. Building a lightweight, ST-GCN-centered pipeline achieves precise, structurally aware action recognition while integrating natural, readable text generation to facilitate enhanced Human-Robot Interaction (HRI), providing a highly scalable solution optimized for resource-constrained edge deployments.


---
## 💾 Dataset Setup

This project uses the **KTH Action Dataset** for human action recognition and skeleton-guided text generation.

### Manual Download

You can download the dataset directly from Kaggle:

* **Official Dataset Link**: [Kaggle - KTH Dataset Complete](https://www.kaggle.com/datasets/rishita26/kth-dataset-complete?resource=download)

### Expected Directory Structure

After downloading, extract the files and place them into the `data/kth/` directory as follows:

```text
Skeleton-Guided-Text-Generation/
├── data/
│   └── kth/
│       ├── boxing/
│       ├── handclapping/
│       ├── handwaving/
│       ├── jogging/
│       ├── running/
│       └── walking/
└── ...
