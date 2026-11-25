# Privacy-Preserving Training of a PIN-Code Digit Recogniser

## Project Overview

This project implements a handwritten digit recognition system using a Convolutional Neural Network (CNN) trained on the MNIST dataset.

The main objective is to explore how **Differential Privacy (DP)** can protect individual training samples while maintaining good model accuracy.

The project uses PyTorch and Opacus to train privacy-preserving models and evaluates the trade-off between model accuracy and privacy protection.

## Problem Statement

A national postal service wants to automate mail sorting by recognising handwritten PIN codes on envelopes.

However, training on citizens' handwriting samples may expose sensitive information through membership-inference attacks.

This project addresses the problem by applying Differential Privacy during model training to reduce information leakage about individual training samples.

## Objectives

- Build a CNN-based handwritten digit recognition model.
- Train a non-private baseline model.
- Train DP models with target privacy budgets ε = 1, 5 and 10.
- Apply gradient clipping and noise addition using Opacus.
- Measure privacy expenditure after each epoch.
- Perform a basic membership-inference confidence check.
- Visualise the privacy-utility trade-off.

## Technologies Used

| Technology | Purpose |
|---|---|
| Python 3 | Programming language |
| PyTorch | Deep learning and CNN |
| Torchvision | MNIST dataset |
| Opacus | Differential Privacy |
| NumPy | Numerical operations |
| Pandas | Result tables |
| Matplotlib | Data visualization |
| Seaborn | Advanced statistical visualization |
| Google Colab | Development environment |

## Dataset

**MNIST Handwritten Digits Dataset**

- Training subset: 20,000 images
- Testing dataset: 10,000 images
- Image dimensions: 28 × 28 pixels
- Number of classes: 10 (digits 0–9)
- Image type: Grayscale

Dataset: [MNIST – Torchvision](https://pytorch.org/vision/stable/generated/torchvision.datasets.MNIST.html)

## Model Architecture

A small CNN is used with Group Normalization instead of Batch Normalization to ensure compatibility with Opacus.

Architecture:

1. Convolutional Layer (1 → 16 channels)
2. Group Normalization
3. ReLU Activation
4. Max Pooling
5. Convolutional Layer (16 → 32 channels)
6. Group Normalization
7. ReLU Activation
8. Max Pooling
9. Flatten Layer
10. Fully Connected Layer (128 neurons)
11. Output Layer (10 classes)

## Experimental Configuration

| Parameter | Value |
|---|---|
| Dataset | MNIST |
| Training samples | 20,000 |
| Batch size | 256 |
| Epochs | 3 |
| Optimizer | Adam |
| Learning rate | 0.001 |
| Device | CPU |
| Random seed | 42 |
| Maximum gradient norm | 1.0 |
| Delta (δ) | 1e-5 |
| Target epsilon | 1, 5, 10 |
| Fixed noise multiplier | 1.1 |

## Experimental Results

### 1. Baseline Model

The non-private CNN achieved:

**Test Accuracy: 98.02%**

### 2. Differentially Private Models

| Target ε | Test Accuracy | Actual ε Spent |
|---:|---:|---:|
| 10 | 93.35% | 9.99 |
| 5 | 93.42% | 5.00 |
| 1 | 92.41% | 1.00 |

The ε = 1 model provides the strongest privacy protection among the tested configurations.

The accuracy difference between the baseline and ε = 1 model is 5.61 percentage points.

### 3. Privacy Accounting

Using a fixed noise multiplier of 1.1:

| Epoch | Privacy Budget (ε) |
|---|---:|
| 1 | 0.66 |
| 2 | 0.85 |
| 3 | 1.01 |

Privacy expenditure increases with each training epoch.

### 4. Membership-Inference Check

Mean maximum softmax confidence:

| Model | Training | Testing | Gap |
|---|---:|---:|---:|
| Baseline | 0.9729 | 0.9734 | 0.0004 |
| DP (ε = 1) | 0.9211 | 0.9248 | 0.0037 |

The confidence gap provides a basic indication of differences in model behaviour on training and testing samples. It is not a complete membership-inference attack.

## Visualizations

The project generates three graphs:

1. **Privacy–Utility Trade-off:** Compares test accuracy across different privacy budgets.
2. **Privacy Budget vs Epoch:** Shows how privacy expenditure increases during training.
3. **Membership-Inference Confidence Comparison:** Compares mean training and testing confidence for baseline and DP models.

Generated graph files:

- `accuracy_vs_epsilon.png`
- `epsilon_vs_epoch.png`
- `membership_confidence.png`

## How to Run the Project

1. Clone or download this repository.
2. Open the `.ipynb` notebook in Google Colab.
3. Install the required libraries.
4. Run all cells sequentially.
5. Observe the accuracy results, privacy accounting, membership check, and generated graphs.

Install dependencies:

```bash
pip install torch torchvision opacus numpy pandas matplotlib seaborn
```

Note: The notebook is configured to run on CPU.

## Conclusion

This project demonstrates the application of Differential Privacy in deep learning for handwritten digit recognition.

By comparing a non-private baseline with DP models trained under different privacy budgets, the experiment illustrates the trade-off between model accuracy and privacy protection.

The experiment shows that privacy-preserving training can maintain reasonable classification performance while providing formal protection against individual training-data disclosure.

## Limitations

- Differential Privacy can reduce model accuracy due to gradient clipping and noise addition.
- The experiment uses a subset of MNIST rather than actual postal handwriting samples.
- The membership-inference check uses confidence differences rather than a complete attack evaluation.
- The experiment is intended for educational purposes and is not a production-ready postal sorting system.

## Future Improvements

- Evaluate the model on real-world handwritten PIN-code images.
- Explore different CNN architectures.
- Compare additional privacy budgets and noise multipliers.
- Implement stronger membership-inference evaluations.
- Optimise model accuracy while maintaining privacy guarantees.

## Author

**Veda Popat**

B.Tech Information Technology

Project: Privacy-Preserving Training of a PIN-Code Digit Recogniser

---

*Developed using Python, PyTorch, Torchvision and Opacus.*
