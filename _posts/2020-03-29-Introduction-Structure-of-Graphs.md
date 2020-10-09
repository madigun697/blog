---
layout: post
title: "[CS224W Lecture 1] Introduction; Structure of Graphs "
date: 2020-03-29
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

## Lecture 1

Title: Introduction; Structure of Graphs <br>
Date: 2020. 03. 29 (Sun)<br>
Materials: [Slides](http://web.stanford.edu/class/cs224w/slides/01-intro.pdf)

### Introduction of Network

![Network](https://user-images.githubusercontent.com/8471958/77970881-4dd65680-7328-11ea-9da2-8d1f8b1f0102.png)

- Natural Graphs
- General language for describing complex systems of interacting entities
- the interactions between the components
- Why?
  - Universal language for describing complex data
  - Shared vocabulary between fields
  - Data availability & computational challenges
  - Impact

### Networks and Applications

####  Sort of Networks analysis
  - Node classification (Predict the attributes(type/color) of a given node)
  - Link prediction (Predict whether two nodes are linked)
  - Community detection (Identify densely linked clusters of nodes)
  - Network similarity (Measure similarity of two nodes or networks)

#### Applications

  1. Social Networks
     - e.g., Social Circle Detection(Community detection)
  2. Infrastructure

  3. Knowledge

     - e.g., Content recommendation(Link prediction)
     - Type of networks
       - Knowledge Graphs
       - Heterogeneous Graphs
       - Multimodal Graphs
       
       ![Type of Networks](https://user-images.githubusercontent.com/8471958/77970944-6e061580-7328-11ea-949e-6a4e26ced38f.png)

     - Embedding Nodes
       - Map nodes to d-dimensional embeddings such that **nodes with similar network neighborhoods** are **embedded close together**
       - Transfer nodes to vectors for computation
       
       ![Embedding Nodes](https://user-images.githubusercontent.com/8471958/77970981-89712080-7328-11ea-932f-dc889ff9543a.png)

  4. Online Media

     - e.g., Hoax article detection (Community detection?), Prediction Virality, Product Adoption

  5. Biomedicine
     - e.g., Protein-protein interaction(PPI) networks, Metabolic networks, Side effects detection(Predict adverse when taking a pair of drugs simultaneously)
     - Biomedical Graph(Heterogeneous graph, Link prediction)

#### Network Analysis Tools
  - SNAP.PY
  - SNAP C++
  - NetworksX
  - graph-tool

### Structure of Graphs

- Network: a collection of objects where some pairs of objects are connected by links
- Objects($$N$$): nodes, vertices
- Interactions($$E$$): links, edges
- System($$G(N,E)$$): networks, graph

![Structure_of_Graphs](https://user-images.githubusercontent.com/8471958/77970765-f59f5480-7327-11ea-89e6-565c95b6baf8.png)

- Difference between Network and Graph
  - **Network** refers to **real-world systems**
  - **Graph** is **mathematical representation** of a network

### Choice of Network Representation

#### Type of Graphs

  - Undirected Graph: symmetrical, reciprocal, without direction

  ![Undirected Graph](https://user-images.githubusercontent.com/8471958/77971090-ce955280-7328-11ea-884e-15b3fd82230f.png)

  - Directed Graph: with direction(arcs)

  ![Directed Graph](https://user-images.githubusercontent.com/8471958/77971096-d3f29d00-7328-11ea-8308-b5fab4107400.png)

  - Subtypes of graphs

    - Unweighted(undirected)

      ![Unweighted](https://user-images.githubusercontent.com/8471958/77972405-3ef1a300-732c-11ea-9e05-c4d11820c61d.png)

    - Weighted(undirected)

      ![Weighted](https://user-images.githubusercontent.com/8471958/77972408-40bb6680-732c-11ea-9f99-cfc3e514f3b0.png)

    - Self-edges(self-loops, undirected)

      ![Self edges](https://user-images.githubusercontent.com/8471958/77972499-93951e00-732c-11ea-97ef-b1bbd914e2f8.png)

    - Multigraph(undirected)

      ![Multigraph](https://user-images.githubusercontent.com/8471958/77972518-a4459400-732c-11ea-9e28-838fd812fc16.png)

#### **Node Degrees**($$k_i$$)

  - \# of edges adjacent to node $$i$$
    - $$k_A = 4$$

  ![Undirected Graph Node Degrees](https://user-images.githubusercontent.com/8471958/77971253-2df36280-7329-11ea-8642-4d6ba59ae837.png)

  - In directed networks node degree separates **in-degree** and **out-degree**
    - In-degree: # of the edges to node $$i$$
    - Out-degree: # of the edges from node $$i$$
    - $$k^{in}_C=2$$, $$k^{out}_C = 1$$, $$k_C=3$$
    
    ![Directed Graph Node Degrees](https://user-images.githubusercontent.com/8471958/77971422-94788080-7329-11ea-8cee-c48fc58dc225.png)

#### Complete Graph
  - Maximum # of edges($$E_{max} = \{\{N(N-1)\}\over\{2\}\}$$)
  - If the graph is $$E = E_{max}$$, it's called a **complete graph**
    - Average degree $$= N-1$$
  - However, most real-world networks are **sparse graph**
    - $$E << E_{max}$$ or $$\bar{k} << N-1$$

#### Bipartite Graph

  - The graph whose nodes can be divided into two disjoint sets $$U$$ and $$V$$

    - e.g., Authors-to-Papers, Actors-to-Movies

    ![Bipartite Graph](https://user-images.githubusercontent.com/8471958/77971671-44e68480-732a-11ea-86d4-9b98652d9c21.png)

#### Representing Graph

  - Adjacency Matrix
    - Representing(Mapping) graph to n-dimensional vector(matrix)

    ![Adjacency Matrix](https://user-images.githubusercontent.com/8471958/77971800-9d1d8680-732a-11ea-940b-ded865c19eb9.png)

  - Edge List

    - Edges represent a pair of nodes combination ($$(Node_{From}, Node_{To})$$)

    ![Edge List](https://user-images.githubusercontent.com/8471958/77971898-ee2d7a80-732a-11ea-89de-c11bb0603d66.png)

  - Adjacency list
  
    - It's an easier way to represent graph
    
    
    ![Adjacency list](https://user-images.githubusercontent.com/8471958/77972000-2c2a9e80-732b-11ea-97a9-f3a35e959465.png)

#### Edge Attributes
  - Properties depending on the structure of the rest of the graph
  - Possible options: Weight, Ranking, Type, Sign(Flag, Boolean)

#### Connectivity of Undirected Graphs

- Connected Graph means that any two vertices can be joined by a path
- Disconnected Graph is made up of two or more connected components
- Connected Graph can be disconnected by Bridge edge and Articulation nodes

![Connectivity](https://user-images.githubusercontent.com/8471958/77973034-0eab0400-732e-11ea-8549-7dda31d468bb.png)

- Strongly connected and Weakly connected
  - Strongly connected graph has every path from each node to every other node
  - Weakly connected graph has more than one missing paths from each node to every other node
  - Strongly connected components(SCCs) is sub-graph which can make strongly connected graph
