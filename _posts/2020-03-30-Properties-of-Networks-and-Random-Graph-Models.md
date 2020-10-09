---
layout: post
title: "[CS224W Lecture 2] Properties of Networks and Random Graph Models"
date: 2020-03-30
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

## Lecture 2

Title: Properties of Networks and Random Graph Models<br>
Date: 2020. 03. 30 (Mon)<br>
Materials: [Slides](http://web.stanford.edu/class/cs224w/slides/02-gnp-smallworld.pdf) [YouTube](https://youtu.be/dD6LRgw_2mQ)

### What are the properties of networks

- Degree distribution($$P(k)$$)
  - Degree means that how many path start a node
- Probability that a randomly chosen node has degree $$k$$($$N_k = $$ nodes with degree k)
- Path length($$h$$)
  - Path: a sequence of nodes in which each node is linked to the next one
  - Distance (shortest path, geodesic, $$h_{B,D} = 2$$)
    - If the two nodes are not connected, the distance is usually defined as infinite or zero
    - However, in directed graphs, distance between two nodes is able to different
  - Diameter (how big is the graph)
    - The maximum distance(shortest path between each two nodes) between any pair of nodes in a graph
    - Average path length (ignore 'infinite' length)
      - $$\bar{h} = {\{1\}\over{2E_{max}}}\sum_{i,j\ne{i}} h_{ij}$$
      - $$h_{ij}$$ is distance from node i to node j
      - $$E_{max}$$ is the max number of edges

- Clustering coefficient($$c$$)
  - only undirected graph
  - how connected are i's neighbors to each others
  - node i with degree ki
  - Ci (= [0,1])
  - Ci = 2e_i over k_i(k_i - 1)
    - e_i is the number of edges between the neighbors of node i
  - C = 1/N sum_i^N C_i
- Connected components($$s$$)
  - Size of the largest connected component
  - Largest set where any two vertices can be joined by a path
- The graphs which are in so different field are similar each other
  - So we need Model

### What kind of models can be used to generate realistic networks

#### Erdos-Renyi Random Graph Model

* Simplest way to generate graph
  * G_np: undirected graph on n nodes (p is probability)
  * G_nm: undirected graph with n nodes (m is edges at random)
* Degree distribution follows binomial
* Clustering coefficient is same to p(bark / n)
* Expansion
* **Real networks are not random!**

#### The Small-World Model

- **The Watts Strogatz Model**
  - Still unsolved degree distribution
- Real world has high clustering coefficient and small shortest path
  - High CC and High diameter: Grid Network
  - Low CC and Low diameter: Random Graph
  - **How can get both High CC and Low diameter?**
- Controversy
- Full connected network and rewire randomness(shortcut)

#### Kronecker Graph Model

- The main assumption is that the real world's big graph construct to small graph sets
- The Kronecker product is the recursive expansion of initial small graph
- Stochastic Kronecker Graphs
  - Initial Kronecker Graph has only 0 or 1
  - However, we apply stochastic aspect(range like weight)

### Homework

