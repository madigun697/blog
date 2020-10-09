---
layout: post
title: "[CS224W Lecture 7] Graph Representation Learning"
date: 2020-04-16
categories:
- Study
excerpt: "CS224W Machine Learning with Graphs"
tags:
- CS224W
- Graph
- Machine Learning
commnets: false
---

# CS224W Machine Learning with Graphs

## Lecture 7

Title: Graph Representation Learning<br>
Date: 2020. 04. 14 (TUE) ~ 2020. 04. 16 (THU)<br>
Materials: [Slides](http://web.stanford.edu/class/cs224w/slides/07-noderepr.pdf) [YouTube](https://youtu.be/2XFMAdHa8uY)<br>
Additional Materials:[Negative Sampling(처음의 마음)](https://ddiri01.tistory.com/310), [Graph Embedding](https://medium.com/@harry_41860/%EA%B7%B8%EB%9E%98%ED%94%84-%EC%9E%84%EB%B2%A0%EB%94%A9-%EC%9A%94%EC%95%BD-a0a34527bb3f)

### Introduction

- Machine Learning in Network

  - Node classification
  - Link prediction

- Machine Learning Lifecycle

  - Raw Data -> Structured Data -> Learning Algorithm -> Model
  - "Feature Engineering" is the process between Raw Data and Structured Data
  - Task: How learn the features automatically

- Feature Learning in Graphs

  - Embedding: feature representation in the given nodes

  ![embedding](https://user-images.githubusercontent.com/8471958/79169268-ef7c9e00-7e26-11ea-9c51-b4c4de96a6fe.png)

  - Map each node in a network into a "low-dimensional space"

  ![embedding2](https://user-images.githubusercontent.com/8471958/79169323-1a66f200-7e27-11ea-9e9c-30a4973ceaa7.png)

### Embedding Nodes

- Encode nodes into embedding space using similarity($$ENC(u)$$) 
- Goal: $$similarity(u, v) \approx {z_u}^\intercal{z_v}$$
- Task
  - Define an encoder
  - Define a node similarity function
  - Optimize the parameters of the encoder

#### Encoder

- $$ENC(v) = z_v$$(**$$d$$-dimensional embedding**)

- Shallow Encoding

  - $$ENC(v) = Zv$$
  - $$Z \in \mathbb{R}^{d\times￨\mathcal{V}￨}$$, $$v \in \mathbb{I}^{￨\mathcal{V}￨}$$
    - $$Z$$ is embedding matrix(main goal)
    - Available methods: DeepWalk, node2vec, **TransE**

  ![embedding matrix](https://user-images.githubusercontent.com/8471958/79170511-8a2aac00-7e2a-11ea-908f-4cf753505382.png)

#### Similarity function

- $$similarity(u, v) \approx {z_v}^\intercal{z_u}$$(**dot product between node embeddings**)

### Random Walk Approaches to Node Embeddings

- Random Walk on the graph is the random sequence of points(nodes) 
  - typically within 5 path's long
- $${z_u}^\intercal{z_v} \approx$$ the probability of random walk path to start from $$u$$ and end to $$v$$
  - $$P_R(u￨v)$$: the probability using random walk strategy $$R$$
  - Similarity($$cos(\theta)$$) $$\varpropto P_R(u￨v)$$
- Reason why use random walk
  - Expressivity
  - Efficiency: Only use(consider) pairs that co-occur on random walks

#### Unsupervised Feature Learning (DeepWalk)

- $$N_r(u)$$: Neighborhood nodes to obtain some strategy $$R$$
  - The nodes are existed in the same path made by some strategy
- Optimization
  - Log-likelihood objective: $$max_z \sum_{ {u} \in {V}} logP(N_R(u)￨z_u)$$
- Random Walk Optimization
  - Run **short fixed-length random walks** starting from each node
  - For each node $$u$$ collect $$N_R(u)$$
  - Optimize embeddings
    - Lost function($$\mathcal{L}$$) $$= \sum_{ {u}\in{V}}\sum_{ {v}\in{N_R(u)}} - log(P(v￨z_u))$$
    - Parameterize $$P(v￨z_u)$$ using softmax
      - $$P(v￨z_u) = { {exp(z_u^\intercal z_v)} \over {\sum_{ {n}\in{V}}exp(z_u^\intercal z_v)}}$$
    - Optimizing means finding embeddings $$z_u$$ that minimize $$\mathcal{L}$$
      (Using Stochastic Gradient Descent)
- Negative Sampling
  - The concept is used in Word2Vec
  - Instead of compute all nodes, compute only some random sample nodes(only related nodes and some of unrelated nodes in Word2Vec)

#### Node2Vec

- Capture local(BFS, microscopic view) and global(DFS, macroscopic view) views of the network

- Interpolate BFS and DFS

  - $$p$$: the parameter of probability to return back to the previous node
  - $$q$$: the parameter of ratio of BFS(move outward) and DFS(move inward, move to other neighbors)

  ![biased random walk](https://user-images.githubusercontent.com/8471958/79399147-88ddb880-7fbd-11ea-97b2-883f37d692b6.png)

- Node2Vec algorithm

  1. Compute random walk probabilities
  2. Simulate $$r$$ random walks of length $$l$$ starting from each node $$u$$
  3. Optimize the node2vec objective using Stochastic Gradient Descent

### Translating Embeddings for Modeling Multi-relational Data

- TransE represented as triples, $$(h, l, t)$$
  where $$h$$ is head entity, $$l$$ is relation, $$t$$ is tail entity
- If given fact is true, $$h + l \approx t$$. However, it's false, $$h + l \neq t$$ in embedded space $$R^k$$

### Embedding Entire Graphs

1. Run a standard graph embedding on the subgraphs and sum(or average) embedded vectors
   - $$Z_G = \sum_{ {v} \in {G}}z_v$$
2. Introduce a "vritual node" to represent the subgraph and run a standard graph embedding technique
3. Using anonymous walk

![anonymous walk](https://user-images.githubusercontent.com/8471958/79400786-2aff9f80-7fc2-11ea-9188-9d10d0a4d135.png)

