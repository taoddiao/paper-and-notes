# Inception(v1)
- Increasing in networks's size can improve their performance
- but have two major drawbacks:
	- overfitting (a larger number of parameters require a larger high quality training sets)
	- dramatically increase the use of comptational resources
- solution: change fully connected to sparsely connected architectures
- however, computers have inefficient numerical calculation on non-uniform sparse data structures

## Inception v1
- concatenate feature-maps generated from convolutions with different kernal sizes and pooling(s)
- naive version: $1\times1$ Conv, $3\times3$ Conv, $5\times5$ Conv, $3\times3$ max pooling (decision was based more on convenience rather than necessity)
- dimension reductions: use $1\times1$ Conv to reduece dimension before $3\times3$ Conv, $5\times5$ Conv, after $3\times3$ max pooling
- the ratio of $3\times3$ Conv, $5\times5$ Conv should increase in higher layers