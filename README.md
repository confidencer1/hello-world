# SVM and CNN on Fashion MNIST Dataset
[![License](https://img.shields.io/github/license/nuclearczy/Gaussian-Bayes_and_KNN_on_Fashion_MNIST_Dataset)](https://github.com/nuclearczy/Gaussian-Bayes_and_KNN_on_Fashion_MNIST_Dataset/blob/master/LICENSE)
[![Pyorch 1.3.1](https://img.shields.io/badge/pytorch-1.3.1-blue)](https://pytorch.org/)
[![cuda 10.2](https://img.shields.io/badge/CUDA-10.2-blue)](https://developer.nvidia.com/cuda-toolkit)
[![Python 3.7](https://img.shields.io/badge/python-3.7-blue.svg)](https://www.python.org/downloads/release/python-370/)

**Author: Zuyang Cao**

## Overview
This project employed two types of machine learning methods to classify the fashion MNIST dataset:

- Support Vector Machine
- Convolutional Neural Network
  - Resnet
  - VGGnet
  - Alexnet

Two dimensionality reduction techniques are applied on SVM: 
 
- PCA (Principal Component Analysis)
- LDA (Linear Discriminant Analysis)

## Dataset
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

[Fashion-MNIST](https://github.com/zalandoresearch/fashion-mnist) is a dataset of Zalando's article images—consisting of a training set of 60,000 examples and a test set of 10,000 examples. Each example is a 28x28 grayscale image, associated with a label from 10 classes.

![Dataset Visualized](visualization/fashion-mnist-sprite.png "Dataset Visualized")

*Figure 1. Visualized Dataset*

## Tunable Parameters

### PCA Parameters
- **pca_target_dim:** Using PCA to reduce the data dimension to this number.

### LDA Parameters
- **components_number:** Number of components (< n_classes - 1) for dimensionality reduction.

### SVM Parameters
- **kernel:** Kernel of Support Vector Machine.

## Results

###  Dimensionality Reduction Visualization

- PCA and LDA Visualization
![LDA and PCA Visualization](visualization/PCA_LDA_viz.png)
*Figure 2. LDA and PCA Visualization*

For more visualization images, please refer to visualization folder.

### SVM with Different Kernels

- SVM Poly vs RBF

Dataset | SVM Poly | SVM RBF
-------- | -------------- | ------------ 
LDA Training set | 84.47% | 85.19%
PCA Training set | 87.87% | 88.35%
LDA Testing set | 82.49 % | 83.41 %
PCA Testing set | 86.03 % | **86.59 %**

- SVM Time Cost

Dataset | SVM Poly | SVM RBF
-------- | -------------- | ------------ 
Training set | 148.41s | 172.30s
Testing set | **73.51s** | 81.74s

From accuracy perspective, SVM with RBF kernel is better but it takes longer training time than Ploy kernel.
And SVM with Linear kernel spent too long to take into account. Thus, only RBF and Poly kernel are tested here.

### CNN

#### Existing CNN Models
Three kinds of Convolutional Neural Network (Resnet, VGGnet, Alexnet) are applied to classify the dataset.

![CNN Diagram](https://missinglink.ai/wp-content/uploads/2019/03/resnet-identity-shortcut-connections.png)

*Figure 3. Structural diagram for VGG and Resnet (source from link)*

- Different CNN Models

Model | Accuracy | Epoch | Training Time
----- | -------- | ----- | -------------
Resnet18 | 84.19% | 2 | 18.37 mins
Resnet18 | 89.91% | 10 | 91.41 mins
Resnet50 | 78.24% | 4 | 82.88 mins
VGGnet16 | 89.71 % | 2 | 63.65 mins
Alexnet | 84.38 % | 2 | 22.60 mins

Time consumption based on NVIDIA GTX 1050 (CUDA 10.2).

#### Custom CNN Model

- Convolution Structure

Figure 4 is an example of convolution structure used in one custom CNN model, more visualization image can be found
in the visualization folder.

![convolution structure](visualization/CNN/structure/convolution_structure.png)

*Figure 4. Convolution Structure for Custom CNN*

- CNN Accuracy vs Epochs

![Resnet18 Accuracy](visualization/Resnet18vsEpoch.png)

*Figure 5. Resnet18 Accuracy vs Epochs*


![Resnet18 Accuracy](visualization/CustomCNNvsEpoch.png)

*Figure 6. Custom CNN Accuracy vs Epochs*

Epoch | Resnet18 | Custom CNN (2Conv 3Hidden)
----- | -------- | ----------
1 | 82.51 % | 82.13%
2 | 84.81 % | 85.40%
3 | 86.33 % | 86.58%
4 | 86.82 % | 87.92%
5 | 86.54 % | 86.91%
6 | 88.67 % | 88.10%
7 | 89.66 % | 87.99%
8 | 87.37 % | 88.38%
9 | 89.73 % | 88.68%
10 | 90.07 % | 89.00%
11 | 89.87 % | 88.57%
12 | 89.65 % | 88.90%
13 | 89.83 % | 89.36%
14 | **90.75 %** | 89.54%
15 | 90.11 % | 88.86%
16 | 90.23 % | 89.48%
17 | 90.46 % | 88.31%
18 | 90.12 % | 89.35%
19 | 90.35 % | 88.81%
20 | 90.61 % | **89.66%**
**Total Time** | 110.11mins | 19.77mins

Time consumption based on NVIDIA GTX 1080 (CUDA 10.2). The accuracy start to converge after approximately 15 epochs.
The common parameters for CNN models are set to be same in both conditions (for example mini-batches) in order to 
compare. 

- Different Custom CNN Models

In these cases, the convolution layers and hidden layers are modified to test the effect on accuracy.

C stands for convolutional layer and H stands for hidden layer (excluding input layer):

Epoch | 1C 3H | 1C 5H | 1C 7H | 2C 2H | 2C 3H
----- | ----- | ----- | ----- | ----- | -----
1 | 82.57% | 82.29% | 79.38% | 80.66% | 82.13%
2 | 84.99% | 86.76% | 86.10% | 83.52% | 85.40%
3 | 87.00% | 86.15% | 87.88% | 86.61% | 86.58%
4 | 87.77% | 88.75% | 89.52% | 87.18% | 87.92%
5 | 88.20% | 89.40% | 88.41% | 87.11% | 86.91%
6 | 88.64% | 90.07% | 90.22% | 87.71% | 88.10%
7 | 88.94% | 89.80% | 90.24% | 88.38% | 87.99%
8 | 89.41% | 89.91% | 90.05% | 87.54% | 88.38%
9 | 89.06% | 90.17% | **90.66%** | 88.81% | 88.68%
10| 89.38% | 90.36% | 89.83% | 89.03% | 89.00%


"# hello-world" 
