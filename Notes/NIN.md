# Network in Network
## 1. Introduction
- The convolution filter is GLM ( generalized linear model ), which has ***low*** level of **abstraction** ( the ablitity to maintain invariant to the variants of the same concpet )
- GLM assumes that the samples of latent concepts are **linearly separable**
- NIN relpaces GML with a micro network structure —— MLP (multilayer perceptron), a general nonlinear function approximator
- mlpconv uses MLP to generate feature-maps and MLP is **shared** among all local receptive fields
- **GAP** (global average pooling) layer is used instead of FC (fully connected) layers when being fed into softmax layer
- GAP is more meaningful and interpretable than FC
- GAP acts as a structural regularizer to **prevent overfitting**

## 2. Convolutional Neural Networks
$$f_{i,j,k} = \max(w_k^Tx_{i,j},0)$$
$(i,j)$ is the pixel index in the feature map, $k$ is the index of channels of feature map, $x_{i,j}$ is the input patch centered at location $(i,j)$
## 3. Network In Network
### 3.1 MLP Convolution Layers
$$f_{i,j,k_1}^1= \max({w^1_{k_1}}^Tx_{i,j}+b_{k_1},0)$$
$$\vdots$$
$$f_{i,j,k_n}^n= \max({w^n_{k_n}}^Tf_{i,j}^{n-1}+b_{k_n},0)$$
$n$ is the number of layers in the MLP

- a cascaded cross channel parametric polling structure
- a convolution layer with 1x1 convolution kernel
- maxout layers use convex fuction approximator, $f_{i,j,k}=\max\limits_m(w^T_{k_m}x_{i,j})$
- compared to maxout layers, mlpconv uses a universal function approximator and has grater capability in modeling various distributions of latent concepts

### 3.2 Global Average Pooling
- GAP enables the feature maps being categories confidence maps
- avoid overfitting
- more robust to spatial translations

### 3.3 Network In Network Structure
- three mlpconv layers
- three-layer perceptron within each mlpconv layer

