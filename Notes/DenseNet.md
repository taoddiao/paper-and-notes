# DenseNet
## 1. Introduction
1. creates ***short paths*** from early layers to later layers 
2. $\frac{L(L+1)}{2}$ ( $C_{L}^{2}$ )layers in an L-yaer network 
3. requires ***fewer*** parameters
4. improved flow of iformation and gradients
5. a regularizing effect with smaller training set sizes

## 2. Related work 
1. making networkds deeper
    - **Highway Networks** use bypassing paths along with gating units
    - **ResNets** use pure identity mappings as bypassing paths
    - *stochastic depth* - randomly drop layers during training
    - *increasing in networks width*
        - GoogLeNet 
        - FractalNets
        - increasing the number of filters
2. other architecture innovations
	- Network in Network(NIN)
	- Deeply Supervied Network(DSN)
	- Ladder Networks
	- Deeply-Fused Nets(DFNs)
	- ... 

## 3. DenseNets
$H_l(\cdot)$ :== non-linear transtfromation(BN, ReLU, Pooling, Conv), ...
$x_l$ :== the output of $l^{th}$ layers

- ResNets
	- $x_l=H_l(x_{l-1})+x_{l-1}$
	- summation achieves gradient flow but impedes information flow
- Dense connectivity
	- concatenation of feature-maps : $[x_0, x_1,\cdots,x_{l-1}]$
	- $x_l = H_l([x_0, x_1,\cdots,x_{l-1}])$
- Composite function
	- $H_l(\cdot)$ a composite function of three consecutive operations: BN $\rightarrow$ ReLU $\rightarrow$ Conv(3 $\times$ 3)
- Pooling layers
	- divide network into multiple densely connected ***dense blocks***
	- ***transition layers*** are layers between blocks
	- use transiton layers to do convolution and pooling
	- transition layers :== BN $\rightarrow$ ReLU $\rightarrow$ Conv(1 $\times$ 1) $\rightarrow$ Pooling(2 $\times$ 2)
- Growth rate ( hyper-parameter k)
	- k :== the number of feature-maps that $H_l$ produces
	- k regulates how much new information (new feature-maps) each layer contributes to the global state (total feature-maps)
- Bottleneck layers
	- Conv(1 $\times$ 1) layers before Conv(3 $\times$ 3) layers 
	- aims to reduce the number of input feature-maps and thus to improve computational efficiency 
	- DenseNet-B 
		- $H_l(\cdot)$ :== BN $\rightarrow$ ReLU $\rightarrow$ Conv(1 $\times$ 1) $\rightarrow$ BN $\rightarrow$ ReLU $\rightarrow$ Conv(3 $\times$ 3) 
		- Conv(1 $\times$ 1) layers produce $4k$ feature-maps
- Compression
	- $\theta$ is compression factor ot reduce the number of feature-maps at transition layers ( $0< \theta <1$ )
	- dense block contaions $m$ feature-maps, transition layers output $\lfloor\theta m\rfloor$ feature-maps
	- DenseNet-C
		- $\theta = 0.5$
- Implementation Details

