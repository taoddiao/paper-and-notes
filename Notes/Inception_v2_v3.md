# Inception v2, v3
## 1. Introduction
- Incpetion v1 is difficult to make changes due to the complexity of the architecture
- It does not provide a clear description about the contributing factors of architecture design decisions
- General principles and optimization ideas, which can efficiently scale up convolution networkds, needed to be described

## 2. General Design Principles
1. **avoid representational bottlenecks, especially early in the network**
	- representation size should gently decrease
	- the dimensionality merely provides a rough estimate of infromation content
2. **higher dimensional representations are easier to process locally within a netwrok **
3. **spatial aggregation can be done over lower dimensional embeddings without much or any loss in representational power **
 	- strong corrlation between adjacent unit exists
4. **balance the width and depth of the network **

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

## 6. Model Regularization via Label Smoothing
- $k$ is label, $x$ is input, $y$ is label for input $x$
- $p(k|x)$ or brevity version $p(k)$ is probabilty of label $k$
- $q(k|x)$ or $q(k)$ is ground-truth distribution of label $k$
- $\delta_{k,y}$ is Dirac delta ($\delta_{k,y}=1$ if $k=y$ else 0)

two problems of using $q(k)=\delta_{k,y}$:
- may result in over-fitting
- may encourage the difference between the largest logit and all others to become large, furthur reducing the adaptation ability of model

smoothing version:
- $u(k)$ is the fixed distribution
- $\epsilon$ is smoothing parameter or weight
$$q'(k)=(1-\epsilon)\delta_{k,y} + \epsilon u(k)$$
$$H(q',p)=(1-\epsilon)H(q,p)+\epsilon H(u,p)$$

if $u(k)$ is uniform distribution $u(k) = 1/K$
$$q'(k)=(1-\epsilon)\delta_{k,y} + \frac{\epsilon}{K}$$
use $u(k)=1/1000$ and $\epsilon=0.1$ improve about 0.2% accuracy

## 7. Inception v2/v3
the quality of the network is relative to variations if principles are matched
- in v3, factorize Conv($7\times7$, stride 2) $\longrightarrow$ Conv($3\times3$, stride 2), Conv($3\times3$), Conv($3\times3$, padded)
- Pool($3\times3$, stride 2)
- Conv($3\times3$), Conv($3\times3$, stride 2), Conv($3\times3$)
- Inception modules (3, 5, 2)
- Pool($8\times8$) (global average pool)
- linear logits
- softmax

v3 use BN, RMSProp, Label Smoothing, Factorized $(7\times7)$, BN-auxiliary classifiers