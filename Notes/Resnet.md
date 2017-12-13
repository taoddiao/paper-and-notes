# ResNet
## 1. Introduction
- ***degradation*** problem: with the network depth increasing, accuracy gets saturated and then degrades rapidly
- solution: each few stacked layers fit a **residual mapping** rather than a desired underlying mapping
- resnet is easy to optimizee and accuracy increases when depth increases

## 2.Related Work
- Residual Representations
	- VLAD (residual vectors with respect to a dictionary), Fisher Vector (probabilistic version of VLAD) in image recognition
	- encoding residual vectors in vector quantization
	- Multigrid method and hierarchical basis preconditioning in partial differential equations

- shortcut connections
	- GoogleNet
	- Highway networks (gating fuctions, data-dependent, have parameters) 
    - addressing vanishing and exploading gradients

## 3. Deep Residual Learning
- Residual Learning
	- Hypothesis : multiple nonlinear layers can asymptotically approximate complicated functions **?**
	- $\mathcal{H}(x)$ is unserlying mapping to be fit by a few stacked layers (complicated function)
	- $\mathcal{F}(x) :== \mathcal{H}(x) -x$ is residual functions
	-  solvers might have difficulties in approximating identity mappings by multiple nonlinear layers (degradation problem)

- Identity Mapping by Shortcuts
	- a building block defined as:
	     $$y = \mathcal{F}(x,\{W_i\}) + W_sx$$
    $x$, $y$ are the input and output vectors of the layers, $\mathcal{F}(x,\{W_i\})$ is residual mapping to be learned, $W_s$ is a linear projection to match the dimensions of input and output layers, $+$( element-wise addition) is a shortcut connection
    - projection shortcuts are **not essential** for addressing the degradation problem (sightly better than identity shorcuts)
    - Identity shortcus is good enough and more efficient 

- Deeper Bottleneck Architectures
	- using a stack of 3 layers instead of 2, ie $1 \times 1$, $3 \times 3$, $1 \times 1$ convolutions
	- $1 \times 1$ convolutions are responsible for reducing and then increasing dimensions
	- using identity shortcuts instead of projection shortcuts (efficient in time complexity and model size)

- Implementation
	- resize image to [256, 480] for scale augmentation
	- randomly crop [224, 224], horizontal flip, per-pixel mean sbutracted, color augmentation
	- BN after convolution and before activation
	- initialize the weights (derivation initialization in arXiv:1502.01852v1 )
	- SGD, lr = 0.1, decay factor = 0.1 (when error plateaus), momentum = 0.9
	- weight decay = 0.0001
	- no dropout