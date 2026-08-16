# Abstract

This project presents a lightweight end-to-end framework for human action recognition (HAR) and automated natural language captioning using Spatio-Temporal Graph Convolutional Networks (ST-GCN). By leveraging MediaPipe Pose Landmarker for 3D skeleton extraction, raw video streams are mapped into standardized COCO-17 keypoint dynamic graphs. Evaluated on the KTH dataset, the ST-GCN model achieves a test classification accuracy of 77.78%. The framework further integrates classification confidence scores with template-based text generation to produce real-time, human-readable action descriptions.

# Motivation

Traditional frame-based 3D Convolutional Neural Networks (3D-CNNs) incur heavy computational overhead and exhibit high sensitivity to ambient background noise. To resolve these limitations, utilizing lightweight pose estimation isolates human topology while completely filtering out redundant visual backgrounds. Unlike standard sequential architectures that flatten joint coordinates and forfeit intrinsic structural topology, Spatio-Temporal Graph Convolutional Networks (ST-GCN) explicitly represent human joint connections alongside their temporal motion trajectories. Building a lightweight, ST-GCN-centered pipeline achieves precise, structurally aware action recognition while integrating natural, readable text generation to facilitate enhanced Human-Robot Interaction (HRI), providing a highly scalable solution optimized for resource-constrained edge deployments.

# Model Architecture

* **ST-GCN Core Network:** The backbone leverages a Spatio-Temporal Graph Convolutional Network (ST-GCN) to process 3D skeleton joints $(17, 3)$ across a 64-frame window. Spatial graph convolutions extract joint layout features using standard physical connectivity, while 1D temporal convolutions capture motion trajectories across frames.

* **Lightweight Optimization:** The network uses lightweight spatial-temporal blocks with spatial layout partition and global average pooling to maintain a compact parameter footprint, enabling low-latency inference on CPU environments without hardware acceleration.

**Confidence-Aware Sentence Generation:** Softmax probabilities from the linear classification head are routed to a deterministic language mapping module. Based on confidence thresholds ($\ge 0.90$, $0.75\text{--}0.89$, and $0.60\text{--}0.74$), the output transitions dynamically between definitive ("The person is...") and tentative phrasing ("The person appears to be..." / "It looks like...") to enhance HRI readability.

# Evaluation & Experimental Results

**Action Recognition Performance**

**Superior Precision in Upper-Body Gestures:** The model achieved excellent performance on local upper-body movements, reaching 91%–100% accuracy across boxing, handclapping, and handwaving. This confirms the ST-GCN backbone effectively captures high-frequency spatial topology and local joint dynamics.

**Confusion in Full-Body Locomotion:** Performance dropped on full-body locomotion classes, specifically jogging (58.3%) and running (47.2%). This represents a well-known benchmark bottleneck on the KTH dataset due to subtle velocity variations between the two actions. Increasing the sampling frame rate or incorporating optical flow features would better disambiguate speed gradients.

**Impact of Early Stopping Strategy**

Training without Early Stopping achieved a higher overall accuracy than training with Early Stopping (77.78% vs. 76.39%, a +1.39% improvement):
* **Overall Convergence:** Terminating training prematurely upon reaching a $80\%$ validation accuracy threshold caused the model to stop prior to full convergence, misclassifying approximately 3 additional test samples.

* **Gesture-Specific Gains:** Fully trained models without Early Stopping demonstrated superior convergence, achieving 100% accuracy on handclapping (vs. 97.2% with Early Stopping) and higher accuracy on handwaving (94.4% vs. 91.7%).

* **Locomotion Bottlenecks:** Both setups struggled to differentiate running from jogging. Training without Early Stopping improved running classification (47.2% vs. 41.7%), whereas Early Stopping slightly boosted jogging (61.1% vs. 58.3%), indicating under-fitted lower-limb motion dynamics across both setups.

* **Invariant Actions:** Performance remained identical across both training regimes for boxing (91.7%) and walking (75.0%).

* **Patience-Based Early Stopping Recommendation:** Replacing fixed target accuracy thresholds with a patience-based mechanism (monitoring whether Validation Loss/Accuracy fails to improve over $N$ consecutive epochs) ensures the model reaches its full optimization potential without truncating training prematurely.

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
