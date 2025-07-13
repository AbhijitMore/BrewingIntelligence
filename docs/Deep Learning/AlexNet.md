# 2. AlexNet

## Table of Contents

1. [Introduction](#1-introduction)
2. [The Landscape Before AlexNet](#2-the-landscape-before-alexnet)
3. [Overview of AlexNet Architecture](#3-overview-of-alexnet-architecture)
4. [Innovations and Key Features](#4-innovations-and-key-features)
5. [Detailed Layer-wise Architecture](#5-detailed-layer-wise-architecture)
6. [Training Details](#6-training-details)
7. [Performance and Results](#7-performance-and-results)
8. [Impact on the Deep Learning Field](#8-impact-on-the-deep-learning-field)
9. [Criticisms and Limitations](#9-criticisms-and-limitations)
10. [Conclusion](#10-conclusion)
11. [Further Reading](#11-further-reading)

---

## 1. Introduction

In 2012, a seismic shift occurred in the field of computer vision. At the ImageNet Large Scale Visual Recognition Challenge (ILSVRC), a deep neural network named **AlexNet** outperformed all other competitors by a significant margin. Created by **Alex Krizhevsky**, **Ilya Sutskever**, and **Geoffrey Hinton**, this deep convolutional neural network set new standards for accuracy, efficiency, and scalability, triggering a deep learning revolution.

---

## 2. The Landscape Before AlexNet

Before AlexNet, traditional computer vision relied heavily on handcrafted features (SIFT, HOG, etc.) combined with classical machine learning classifiers (SVMs, Random Forests). Deep learning models existed but were limited in performance due to:

* Lack of large datasets
* Limited computational resources
* Vanishing/exploding gradient issues in deep networks

The **ImageNet** dataset changed this by providing **over 1.2 million labeled images across 1000 categories**, offering the scale needed for training deep models.

---

## 3. Overview of AlexNet Architecture

AlexNet is a deep convolutional neural network designed to classify images into 1000 different classes. Inspired by **LeNet-5**, it was scaled massively to handle high-resolution input and deeper layers.

**High-Level Structure:**

* Input: RGB image of size **227x227x3**
* 5 Convolutional Layers
* 3 Fully Connected Layers
* ReLU Activations
* Dropout Regularization
* Max-Pooling
* Final Softmax Classifier

**Stats:** ~60 million parameters and ~650,000 neurons

---

## 4. Innovations and Key Features

### ✅ ReLU Activation Function

* $f(x) = \max(0, x)$
* Faster convergence than sigmoid or tanh
* Mitigates vanishing gradients

### ✅ GPU Acceleration

* Trained using **2 NVIDIA GTX 580 GPUs**
* Parallel training across GPUs

### ✅ Dropout

* Applied in fully connected layers
* Reduces overfitting by randomly deactivating neurons

### ✅ Data Augmentation

* Horizontal flips, translations, color jittering
* Increases dataset variability

---

## 5. Detailed Layer-wise Architecture

| Layer   | Type            | Details                                  |
| ------- | --------------- | ---------------------------------------- |
| Input   | -               | 227x227x3 RGB Image                      |
| Conv1   | Convolution     | 96 filters of 11x11, stride 4 → 55x55x96 |
| ReLU1   | Activation      | ReLU                                     |
| LRN1    | Normalization   | Local Response Normalization             |
| Pool1   | Max Pooling     | 3x3, stride 2 → 27x27x96                 |
| Conv2   | Convolution     | 256 filters of 5x5, padding = 2          |
| ReLU2   | Activation      | ReLU                                     |
| LRN2    | Normalization   | Local Response Normalization             |
| Pool2   | Max Pooling     | 3x3, stride 2 → 13x13x256                |
| Conv3   | Convolution     | 384 filters of 3x3                       |
| ReLU3   | Activation      | ReLU                                     |
| Conv4   | Convolution     | 384 filters of 3x3                       |
| ReLU4   | Activation      | ReLU                                     |
| Conv5   | Convolution     | 256 filters of 3x3                       |
| ReLU5   | Activation      | ReLU                                     |
| Pool5   | Max Pooling     | 3x3, stride 2 → 6x6x256                  |
| FC6     | Fully Connected | 4096 neurons                             |
| Drop6   | Dropout         | 50%                                      |
| FC7     | Fully Connected | 4096 neurons                             |
| Drop7   | Dropout         | 50%                                      |
| FC8     | Fully Connected | 1000 neurons (classes)                   |
| Softmax | Classification  | Output probability distribution          |

---

## 6. Training Details

* **Dataset**: ImageNet ILSVRC 2012 (1.2M training, 50K validation)
* **Optimizer**: SGD with momentum = 0.9
* **Learning Rate**: Initial = 0.01, manually decreased
* **Batch Size**: 128
* **Weight Decay**: 0.0005 (L2 regularization)
* **Training Time**: ~5–6 days on dual GPUs

---

## 7. Performance and Results

AlexNet **won ILSVRC 2012** with a **Top-5 error rate of 15.3%**, significantly outperforming the runner-up.

| Model                     | Top-5 Error Rate |
| ------------------------- | ---------------- |
| **AlexNet (2012)**        | **15.3%**        |
| Second-best (handcrafted) | 26.2%            |

---

## 8. Impact on the Deep Learning Field

AlexNet's success led to:

* A surge in **CNN-based computer vision**
* Development of deeper models: **VGG, GoogLeNet, ResNet**
* Widespread **GPU acceleration**
* Rebirth of neural networks across domains (NLP, speech, RL)
* Boom in frameworks: **TensorFlow, PyTorch, Caffe**

AlexNet became the **launchpad for modern deep learning**.

---

## 9. Criticisms and Limitations

Despite its importance, AlexNet had limitations:

* **Shallow** by modern standards (8 layers)
* **Hard-coded GPU split** reduces flexibility
* LRN layers later considered unnecessary
* **Large parameter count** (60M)
* Not as modular or extensible as later models

---

## 10. Conclusion

AlexNet was a pivotal breakthrough that proved deep learning's practical value. It overcame longstanding bottlenecks in neural networks using a clever combination of architectural choices, GPU computing, and data augmentation. Nearly every modern vision model traces its lineage to the ideas crystallized in AlexNet.

---

## 11. Further Reading

* 📄 [Original Paper (2012)](http://www.cs.toronto.edu/~fritz/absps/imagenet.pdf)
* 📚 *Deep Learning* by Ian Goodfellow et al. (Chapter 9)
* 🎓 Stanford [CS231n: CNNs for Visual Recognition](http://cs231n.stanford.edu/)

---
