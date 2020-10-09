---
layout: post
title: "[CS224W Lecture 10] Deep Generative Models for Graphs"
date: 2020-05-04
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

## Lecture 10

Title: Deep Generative Models for Graphs<br>
Date: 2020. 04. 28(TUE) ~ 2020. 05. 04(MON)<br>
Materials: [Slides](http://web.stanford.edu/class/cs224w/slides/10-graph-gen.pdf) [YouTube](https://youtu.be/18iKx9YSNHk)
Additional Materials:

### Deep Graph Encoders

- Generate node embeddings based on local network neighborhoods and neural networks
  - Graph Convolutional Neural Networks(GCN): average neighborhood information
  - GraphSAGE: generalized neighborhood aggregation

### Problem of Graph Generation

#### Graph Generation Tasks

- Realistic graph generation
- Goal-directed graph generation

#### Why is it hard?

- Large and variable output space
  - We need to $$n^2$$ values to express $$n$$ nodes (adjacency matrix)
  - Different graphs need to different sizes
- Non-Uniqueness
  - $$n$$-node graph can be represented in $$n!$$ ways
- Complex dependencies
  - To form the edges, we need have long-range dependencies(more memory)

### ML Basics for Graph Generation

#### Graph Generative Models

- Given: $$p_{data}(G)$$, the set of graphs
- Goal: Distribution $$p_{model}(G)$$ and sample from $$p_{model}(G)$$
  - $$p_{model}(G)$$ means the new set of graphs using learning
- Generate the new set of graphs using assumed(learned) distribution with input set(observation) of graphs
  - $$p_{data}(x)$$ is the **data distribution** in the real world. $$x_i$$ is the sampled in the $$p_{data}(x)$$
  - $$p_{model}(x;\theta)$$ is the **model** that we use to approximate $$p_{data}(x)$$
  - The goal is the making $$p_{model}(x;\theta)$$ close to $$p_{data}(x)$$ and getting sample from $$p_{model}(x;\theta)$$

#### Make $$p_{model}(x;\theta)$$ close to $$p_{data}(x)$$ 
- Find $$\theta$$ to maximize likelihood
- $$\theta^* = argmax_\theta \mathbb{E}_{x \sim p_{data}} log_{p_{model}}(x | \theta)$$

#### Sample from $$p_{model}(x;\theta)$$

- To sample from a complex distribution, using noise(seed) from a simple noise distribution
  - Using the noise from a noise distribution as parameter of function($$f( \cdot )$$), we can make that the output($$x_i$$) follows a complex distribution
  - Sample from a simple noise distribution
    $$z_i \sim N(0,1)$$
  - Transform the noise $$z_i$$ vis $$f(\cdot)$$
    $$x_i = f(z_i;\theta)$$
  - $$f( \cdot )$$ is designed by Deep Neural Networks

#### Auto-regressive models

- Auto-regressive means that every next action depends on the previous actions
- In the graph case, the next action($$t$$-th action, $$x_t$$) will be adding node or adding edge
- Apply chain rule
  - $$p_{model}(x;\theta) = \prod_{t=1}^n p_{model}(x_t|x_1,...,x_{t-1};\theta)$$

### GraphRNN

- Recurrent: Every output feedback into input (Auto regressive)
- Graph $$G$$ is made by Generation process $$S^\pi$$(recipe of drawing graph),
  where $$\pi$$ is node ordering

![Generation Process](https://user-images.githubusercontent.com/8471958/80545274-e9251f00-89ed-11ea-803f-7ba62d3d70c8.png)

- The sequence $$S^\pi$$ has two levels
  - Node-level: At each step, a new node is added
    - Node-level $$S^\pi$$ is the order of adding node
    - $$S^\pi = (S_1^\pi, S_2^\pi, S_3^\pi, S_4^\pi, S_5^\pi)$$
  - Edge-level: At each step, a new edge is added
    - Edge-level $$S^\pi$$ is the set of whether or not connect previous nodes
    - $$S_4^\pi = (S_{4,1}^\pi, S_{4,2}^\pi, S_{4,3}^\pi) = (0, 1, 1)$$
- GraphRNN has a node-level RNN and edge-level RNN
  - Using these two-level sequence, we make the graph and adjacency matrix(the yellow area)

![GraphRNN](https://user-images.githubusercontent.com/8471958/80545940-6735f580-89ef-11ea-8138-dba66b7c9966.png)

![adjacency matrix](https://user-images.githubusercontent.com/8471958/80545975-79179880-89ef-11ea-96e4-98d7faec84e6.png)

#### Recurrent Neural Network

![RNN](https://user-images.githubusercontent.com/8471958/80546089-c431ab80-89ef-11ea-9a19-f945f404e4a1.png)

![RNN](https://user-images.githubusercontent.com/8471958/80546480-ae70b600-89f0-11ea-9f5d-ee838c638986.png)

- RNN cell calculates the output and the state using input and state of previous step
  - $$s_t$$: State of RNN after time $$t$$
  - $$x_t$$: Input of RNN at time $$t$$, Output of RNN at time $$t-1$$
  - $$y_t$$: Output of RNN at time $$t$$
  - SOS($$s_0$$) and EOS($$y_t$$) use zero vector
- In the generation(stochastic), output is the probability vector and input is the sample based on the probability

##### Training phase

- At training time, use an observed sequence $$y^*$$ and "Teacher Forcing"
- Teacher Forcing means model use real sequence($$y^*$$) output (not model's output) as a input
- Loss $$L$$ is the **Binary cross entropy**
  $$L = -[y_1^* log(y_1) + (1-y_1^*) log (1-y_1)]$$
- Using loss and back propagation, adjust RNN parameters

#### Tractability

- Too many steps for edge generation because of that any node can connect to any prior node(random ordering)
- Using BFS ordering, reduce steps for edge generation

![BFS Ordering](https://user-images.githubusercontent.com/8471958/80928165-dcce0700-8ddd-11ea-96b7-82162fe6daea.png)


#### Evaluating Generated Graphs

- Define similarity metric for graphs
  - Visual similarity
  - Graph statistics similarity

### Applications and Open Questions

- Generating Graph's Goal
  - High scores(Optimization)
  - Valid(Obey underlying rules)
  - Realistic(imitating an original graph)

- Graph Convolutional Policy Network(GCPN)
  - Combine graph representation + RL
  - Graph Neural Network(Valid), Reinforcement learning(High scores), Adversarial training(Realistic)