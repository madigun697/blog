---
layout: post
title: "[CS224W Lecture 8] Graph Neural Networks"
date: 2020-04-20
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

## Lecture 8

Title: Graph Neural Networks<br>
Date: 2020. 04. 17 (FRI) ~ 2020. 04. 20 (MON)<br>
Materials: [Slides](http://web.stanford.edu/class/cs224w/slides/08-GNN.pdf) [YouTube](https://youtu.be/6izOYKGuXTQ)<br>
Additional Materials: [Pooling](https://medium.com/@hobinjeong/cnn%EC%97%90%EC%84%9C-pooling%EC%9D%B4%EB%9E%80-c4e01aa83c83), [GNN](https://newsight.tistory.com/313)

### Shallow Encoders(Node Embedding)

- Map nodes in original graph to $$d$$-dimensional embedding space
  - $$ENC(v) = z_v$$
- Goal is that the the doc product of two node points in the embedding vector close to similarity of two nodes
  - $$similarity(u, v) \approx z_v^\intercal z_u$$
- Limitation
  - Every node has its own uniques embedding
  - Cannot generate embedding for nodes that are not seen during training
  - Do not incorporate node features

#### Deep Graph Encoders

- Using neural networks to encode and combining with node similarity functions
- **The challenge** is the Modern ML/DL Toolboxes **specialized the simple data type**(sequences or grids)
  - Graph doesn't have reference point(where to move)
- Naive Approach
  - Using Adjacency matrix and feature to apply neural net
  - Issues
    - too many parameters(# of the nodes and # of features)
    - It's not applicable to graphs of different sizes
    - It's not invariant to node ordering

### Basics of Deep Learning for Graphs

- **GCN (Graph Convolution Networks)**

- Key concepts
  - Understanding local network neighborhoods
  - Stacking multiple layers
  
- Graph $$G$$
  - $$V$$: vertex set
  - $$A$$: adjacency matrix
  - $$X \in \mathbb{R}^{m \times ￨V￨}$$: matrix of node features

- Aggregate Neighbors
  - Generate embeddings based on local network neighborhoods
  - Using neural networks, nodes aggregate information from their neighbors(within certain steps)
  - Compute every nodes, Every nodes have the different neural networks
  
  ![Aggregate Neighbors](https://user-images.githubusercontent.com/8471958/79516102-57c8bb00-8085-11ea-8de2-5746a99afcf0.png)
  
  - Aggregation
    - Basically, the average information from neighbors to apply a neural network
  - The Math: Deep Encoder
    - $$h_v^0 = x_v$$: 0-th layer of node $$v$$ equals to the features of node $$v$$
    - $$z_v = h_v^K$$: The final result(embedding) is the result of last layer($$K$$)
    - $$h_v^k$$: The result of every layer(except last layer)
      $$h_v^k = \sigma (W_k \sum_{u \in N(v)} { {h_u^{k-1}} \over {￨N(v)}} + B_kh_v^{k-1}), \forall k \in \{1,...,K\} $$
      - $$\sigma$$ is the Non-linearity like ReLU
      - $$W_k, B_k$$ is the parameter matrix
        - These two parameter is trade-off relationship
        - If $$W_k$$ is high, it means the node $$u$$ determine from more its neighbors
        - If $$B_k$$ is high, it means the node $$u$$ determine from itself features
      - $$N(v)$$ is the set of neighbors of node $$u$$
      - $$\sum_{u \in N(v)} { {h_u^{k-1}} \over {￨N(v)}}$$ is the average of the results in previous layer($$k-1$$)
      - $$h_v^{k-1}$$ is the message of node $$v$$ self
  
- Unsupervised Training

  - Using Random walks, Graph factorization, Node proximity in the graph

- Supervised Training

  - Using loss function($$\mathcal{L}$$), train the way toward minimization of loss

- Inductive Capability

  - The parameters $$W_k, B_k$$ can be used in other graphs(unseen nodes)
  - In the huge graph, we can train with snapshot and apply same parameters to other part of graph

### Graph Convolutional Networks and GraphSAGE

- Apply a different aggregation function instead of average
- $$h_v^k = \sigma ([W_k \cdot AGG(\{h_u^{k-1}, \forall_u\in{N(v)}\}), B_kh_v^{k-1}])$$
- $$AGG(\{h_u^{k-1}, \forall_u\in{N(v)}\})$$
  - Mean<br>
    $$AGG = \sum_{u\in{N(v)}}{ {h_u^{k-1}}\over{￨N(v)￨}}$$
  - Pool: Pooling strategy like mean pooling or max pooling<br>
    $$AGG = \gamma(\{ Qh_u^{k-1}, \forall_u \in {N(v)} \})$$
    ![max pooling](https://user-images.githubusercontent.com/8471958/79702105-33671b80-82dd-11ea-97c3-122aee9adfaa.png)
  - LSTM<br>
    $$AGG = LSTM([h_u^{k-1}, \forall_u \in {\pi(N(v))}])$$
- Efficient Implementation
  - Using sparse matrix operations
  - $$\sum_{u \in N(v)} { {h_u^{k-1}} \over {￨N(v)}}$$ can be calculated $$H^k = D^{-1}AH^{k-1}$$
    where, $$H^k = [H_1^k, ..., H_n^k]$$, $$D$$ is degree matrix, $$A$$ is adjacency matrix

### Graph Attention Networks (GAT)

- Simple neighborhood aggregation has every neighbors have same weight(importance)
  - $$\alpha_{vu} = 1 / ￨N(v)￨$$ is the weighting factor of node $$u$$'s message to node $$v$$ in the average aggregation
- Attention machanism
  - $$e_{vu} = a(W_kh_u^{k-1}, W_kh_v^{k-1})$$
  - Normalize coefficients using the softmax function
    - $$\alpha_{vu} = { {exp(e_{vu})}\over{\sum_{k \in {N(v)} exp(e_{vu})}} }$$
      $$h_v^k = \sigma(\sum_{u \in {N(v)}} \alpha_{vu} W_kh_u^{k-1})$$
