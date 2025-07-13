# 5. ResNet

## Table of Contents

1. [Introduction](#1-introduction)
2. [The Problem of Depth in Neural Networks](#2-the-problem-of-depth-in-neural-networks)
3. [What is Residual Learning?](#3-what-is-residual-learning)
4. [The ResNet Architecture](#4-the-resnet-architecture)
5. [Residual Blocks Explained](#5-residual-blocks-explained)
6. [ResNet Variants and Depths](#6-resnet-variants-and-depths)
7. [Design Principles and Insights](#7-design-principles-and-insights)
8. [Training Details](#8-training-details)
9. [Performance and Impact](#9-performance-and-impact)
10. [Limitations and Later Evolutions](#10-limitations-and-later-evolutions)
11. [Conclusion](#11-conclusion)
12. [Further Reading and Resources](#12-further-reading-and-resources)

---

## 1. Introduction

**ResNet**, short for **Residual Network**, was introduced by Microsoft Research in the landmark paper *"Deep Residual Learning for Image Recognition"* (He et al., 2015). It solved a long-standing problem in deep learning — training very deep networks.

With architectures like **ResNet-50**, **ResNet-101**, and **ResNet-152**, it achieved unprecedented depth and accuracy on ImageNet, while still being **easy to optimize**.

> ✨ **Key Innovation**: Skip connections (or residual connections) that allow gradients to flow directly across layers.

---

## 2. The Problem of Depth in Neural Networks

As researchers attempted to build **deeper convolutional neural networks**, they ran into issues like:

* **Vanishing gradients** during backpropagation
* **Degradation problem**: Accuracy got worse as layers increased beyond a point, even with batch normalization

This was surprising — deeper networks should be strictly more expressive. But in practice, deeper vanilla CNNs were harder to train effectively.

> 🧐 Observation: Simply adding more layers degraded performance rather than improved it.

---

## 3. What is Residual Learning?

Instead of each layer learning an entirely new representation, ResNet proposes to **learn residual functions**.

### Formally:

If the desired underlying mapping is $H(x)$, we let the stacked layers approximate:

$F(x) = H(x) - x \quad \Rightarrow \quad H(x) = F(x) + x$

This is implemented by adding the input $x$ to the output of a few stacked layers:

```text
Output = Layer(x) + x
```

This simple change allows:

* Better gradient flow
* Easier optimization
* Use of identity mappings to preserve information

---

## 4. The ResNet Architecture

ResNet is built using **Residual Blocks** stacked together. The standard version for ImageNet classification includes:

* Initial 7x7 Conv + MaxPooling
* 4 Stages of Residual Blocks (conv2\_x to conv5\_x)
* Global Average Pooling
* Fully Connected Layer (1000-way softmax)

### Example: ResNet-50

| Stage    | Layers                 | Output Size  |
| -------- | ---------------------- | ------------ |
| Conv1    | 7x7 conv, 64, stride 2 | 112x112x64   |
| Pool1    | 3x3 max pool, stride 2 | 56x56x64     |
| Conv2\_x | 1x1, 3x3, 1x1 (x3)     | 56x56x256    |
| Conv3\_x | 1x1, 3x3, 1x1 (x4)     | 28x28x512    |
| Conv4\_x | 1x1, 3x3, 1x1 (x6)     | 14x14x1024   |
| Conv5\_x | 1x1, 3x3, 1x1 (x3)     | 7x7x2048     |
| Pool     | Avg pool, 1x1          | 1x1x2048     |
| FC       | Fully connected        | 1000 classes |

---

## 5. Residual Blocks Explained

There are two main types:

### a. Basic Block (used in ResNet-18/34)

```text
Input --> Conv (3x3) --> BN --> ReLU --> Conv (3x3) --> BN --> + --> ReLU --> Output
                                 ^                         |
                                 |-------------------------|
```

### b. Bottleneck Block (used in ResNet-50/101/152)

```text
Input --> Conv (1x1) --> BN --> ReLU --> Conv (3x3) --> BN --> ReLU --> Conv (1x1) --> BN --> + --> ReLU
                                                                        ^                         |
                                                                        |-------------------------|
```

Why bottlenecks?

* Reduce computational cost (via 1x1 conv)
* Enable training deeper models

If input and output shapes differ, a **projection (1x1 conv)** is used in the skip path.

---

## 6. ResNet Variants and Depths

| Model      | Depth | Parameters | Top-5 Error (ILSVRC) |
| ---------- | ----- | ---------- | -------------------- |
| ResNet-18  | 18    | ~11M      | ~7.5%               |
| ResNet-34  | 34    | ~21M      | ~7.0%               |
| ResNet-50  | 50    | ~25M      | ~6.8%               |
| ResNet-101 | 101   | ~44M      | ~6.5%               |
| ResNet-152 | 152   | ~60M      | ~6.2%               |

---

## 7. Design Principles and Insights

### ✅ Identity Shortcuts:

* Let the network preserve signal across layers
* Require no extra parameters if dimensions match

### ✅ Deep Supervision (implicitly):

* Gradients can directly flow through residual connections
* Improves convergence

### ✅ Feature Reuse:

* Encourages layers to refine, not relearn, features

### ✅ Avoids Overfitting:

* Despite greater depth, ResNet generalizes better

---

## 8. Training Details

* **Dataset**: ImageNet (1.28M images)
* **Data Augmentation**:

  * Random resized crop to 224x224
  * Horizontal flips
* **Optimization**:

  * SGD with momentum (0.9)
  * Weight decay: 1e-4
  * Batch size: 256
  * LR schedule: Step decay (e.g., divide by 10 every 30 epochs)
* **Epochs**: 90–120
* **Initialization**: MSRA (He et al., 2015) weight init for ReLU

---

## 9. Performance and Impact

* **Won 1st place** in ILSVRC 2015 (Image classification)
* Enabled very deep CNNs to be trained effectively (up to 1000+ layers)
* Inspired many successor models:

  * DenseNet (feature concatenation instead of addition)
  * ResNeXt (grouped convolutions)
  * EfficientNet (compound scaling)
  * Vision Transformers still use ResNet as backbone

---

## 10. Limitations and Later Evolutions

| Issue                  | Solution in later models                         |
| ---------------------- | ------------------------------------------------ |
| Computationally heavy  | ResNeXt, MobileNet, ShuffleNet                   |
| Static design (manual) | NASNet, EfficientNet                             |
| Only additive identity | DenseNet uses concatenation                      |
| Diminishing returns    | Efficient architectures (e.g., Swin Transformer) |

---

## 11. Conclusion

ResNet revolutionized the way we design deep networks. It solved the degradation problem using a simple, elegant idea: **learning residuals instead of direct mappings**.

Its skip connections became a **blueprint** for almost all modern CNNs, and its legacy continues in hybrid models and transformers alike.

> ✨ "Sometimes, the best way forward is to remember where you started."

---

## 12. Further Reading and Resources

* 📄 [Original Paper](https://arxiv.org/abs/1512.03385): He et al., 2015
