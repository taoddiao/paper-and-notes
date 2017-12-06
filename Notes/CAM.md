# Learning Deep Features for Discriminative Localiztion
## 1. Introduction

- **fully-connected layers** are ***unable*** to localize objects but **convolutional units** have this ability
- NIN and GooLeNet avoid the use of FC (parameters $\downarrow$, but high performance)
- **global average pooling** is a structural regularizer (preventing overfitting) in NIN
- **GAP** combined with **CAM** have localiztion ability

### 1.1 Related Work
1. Weakly-superviesed object localization
	- a technique for self-taught object localization involving masking out image regions to iden- tify the regions causing the maximal activations in order to localize objects
	- combine multiple-instance learning with CNN features to localize objects
2. Visualizing CNNs
	- deconvolutional networks
	- CNNs learn object detectors while being trained to recognize scenes
	- analyze the visual encoding of CNNs by inverting deep features at different layers

## 2. Class Activation Mapping
- take *categoriztion* as example
- loss fuction is *softmax*
- ignore the *bias* as it little to no impact on classification performance

$f_k(x,y):==$ the activation of unit $k$ in the last convolutional layer at spatial location $(x,y)$

$F^k=\sum_{x,y}f_k(x,y) :==$ the result of performing global average pooling for unit $k$

$w_k^c :==$ the weitht corresponding to class $c$ for unit $k$, or the ***importance*** of $F_k$ for class $c$

$S_c=\sum_kw_k^cF_k :==$ the input to softmax

$P_c=\frac{e^{S_c}}{\sum_ce^{S_c}}:==$  the output of softmax for class $c$

$M_c(x,y)=\sum_kw_k^cf_k(x,y) :==$ the ***class activation map*** for class $c$

$$S_c=\sum_kw_k^cF_k=\sum_kw_k^c\sum_{x,y}f_k(x,y)=\sum_{x,y}\sum_kw_k^cf_k(x,y)=\sum_{x,y}M_c(x,y)$$

- $M_c(x,y)$ indicates the importance of the activation at spatial grid $(x, y)$ leading to the classification of an image to class c.
- the discriminative regions for different categories are ***different*** even for a given image
- GAP vs GMP
	- GAP can identity **the extent** of the object, while GMP identifies **just one** discriminative part
	- GMP ignores the low scores for image regions
	- similar classification performance but GAP outperforms GMP for localization

