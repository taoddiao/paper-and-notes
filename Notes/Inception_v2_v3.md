# Inception v2, v3
## 1. Introduction
- Incpetion v1 is difficult to make changes due to the complexity of the architecture
- It does not provide a clear description about the contributing factors of architecture design decisions
- General principles and optimization ideas, which can efficiently scale up convolution networkds, needed to be described

## 2. General Design Principles
##### 1. avoid representational bottlenecks, especially early in the network
- representation size should gently decrease
- the dimensionality merely provides a rough estimate of infromation content 

##### 2. higher dimensional representations are easier to process locally within a netwrok
##### 3. spatial aggregation can be done over lower dimensional embeddings without much or any loss in representational power
- strong corrlation between adjacent unit exists 

##### 4. balance the width and depth of the network

## 3. Factorizing Convolutions with Large Filter Size
- the outputs of near-by activations are highly correlated, so activations can be reduced before aggregation
- Conv$(5\times5) \longrightarrow$ Conv$(3\times3) + $ Conv$(3\times3)$
	- $\frac{9 + 9}{25} \times$ reduction of computation
- Asymmetric factorization (better): Conv$(n\times n) \longrightarrow$ Conv$(1\times n) + $ Conv$(n\times 1)$
	- does not work well on early laters
	- good results on medium grid-sizes ($m \times m, m \in [12,\dots, 20]$) and $n=7$

## 4. Utility of Auxiliary Classifiers
- auxiliary classifiers improve the convergence, promote more stable learning and better convergence
- but auxiliary classifiers does not improve convergence early in the training
- it may be wrong that auxiliary branches help evolving the low-level features
- auxiliary calssifiers act as regularizer

## 5. Efficient Grid Size Reduction
- traditionally, activation (convolution) dimension of the network filters is expanded before applying pooling
- cannot switch convolution and pooling (will create a representational bottlenecks)
- use two parallel stride 2 blocks: pooling layer and convolution, then concat feature maps

## 6 Inception v2