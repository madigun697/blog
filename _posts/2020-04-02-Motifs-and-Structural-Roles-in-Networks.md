---
layout: post
title: "[CS224W Lecture 3] Motifs and Structural Roles in Networks"
date: 2020-04-02
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

## Lecture 3

Title: Motifs and Structural Roles in Networks<br>
Date: 2020. 04. 01 (WED) ~ 04. 02 (THU)<br>
Materials: [Slides](http://web.stanford.edu/class/cs224w/slides/03-motifs.pdf) [YouTube](https://youtu.be/FnLDqNBgvS8)

### Subnetworks

- Decomposition of a big network

- Isomorphic: the same graph with the same edges size and the same directions

- Significant is a metric capable of classifying the subgraph

  - under-representation(negative significant)
  - over-representation(positive significant)
  - Significant is different by networks (the same subgraph is positive in Language network and negative in Neurons network)

  ![Significant](https://user-images.githubusercontent.com/8471958/78083335-1af79580-73f0-11ea-937f-1424416463a0.png)

### Subgraphs, Motifs, and Graphlets

#### Network Motifs

- Significant recurring patterns of interconnections in the network

  - **Pattern**: Small induced[^1] subgraph
    - 'Induced subgraph' means that one or more subgraphs which have the same structure of the subgraph of interest(Motif) can find in the full graph
  - **Recurring**: High frequency
    - 'Recurrence' means how many find induced subgraphs(occurrence)
  - **Significant**: More frequent than expected
    - It's based on z-score
    - $$Z_i = (N^{real}_i-\bar{N}^{rand}_i) / std(N^{rand}_i)$$
    - $$N^{real}_i$$ is #(subgraphs of type $$i$$) in network $$G^{real}$$
    - $$N^{rand}_i$$ is #(subgraphs of type $$i$$) in randomized network $$G^{rand}$$
    - Network significant profile(SP) is a vector of normalized Z-scores
      - $$SP_i = Z_i /\sqrt{\sum_jZ^2_j} $$
      - Usually, $$G^rand$$ generated 10,000~100,000 to estimated $$\bar{N}^{rand}_i$$ and $$std(N^{rand}_i)$$. It depends on the size of the real graph

- understanding how networks work and prediction operation and reaction of the network in a given situation

  - Feed-forward loop: Move toward A to B
  - Parallel loop: Move toward A to B without skipped nodes
  - Sing-input modules: Single input and multiple outputs

  ![Motifs](https://user-images.githubusercontent.com/8471958/78084072-3a8fbd80-73f2-11ea-8cf3-fa14ecee21c1.png)
  
- Generation random model

  - Configuration Model

    - Null model(random model) which is compared with the real graph with the degree sequence of the real graph
    - Using spokes

    ![Configuration model](https://user-images.githubusercontent.com/8471958/78085173-239e9a80-73f5-11ea-801f-c2ccb69f4792.png)

    - Why connect directly --> it's not random!

  - Switching Model

    - Repeat exchange endpoints from the given Graph $$G$$ again and again (randomize!) 

  #### Graphlets: Node feature vectors

  - Graphlets: possible subgraphs which are made by n-nodes
    - use graphlets to obtain node-level subgraph metric
  - Graphlet degree vector(GDV): # of non-isomorphic position in graphlet
    - non-isomorphic position means that unique nodes in the graphlet(node touches)
    - In the input graph, GDV means a vector is constructed by the number of graphlets that a node touches at a particular orbit

  ![Automorphism Orbits](https://user-images.githubusercontent.com/8471958/78193890-902d9e00-74b6-11ea-8265-2ed63afb2eb0.png)

  - Considering graphlets on 2 to 5 nodes(73 coordinates)
    - It means that graphs usually make within 4 hops

  #### Finding Motifs and Graphlets

  - Finding size-k motifs/graphlets is a hard computational problem(NP-complete)
  - There are two big two challenges
      - **Enumerating**(To obtain all size-k connected subgraphs)
    - **Counting**(To obtain occurrences of each subgraph type)
    - Computation time grows exponentially as the size of the motif/graphlet increase
- Algorithms
  
    - Exact subgraph enumeration (ESU) [Wernicke 2006]
    - Kavosh[Kashaniet al. 2009]
    - Subgraph sampling [Kashtanet al. 2004]

#### Exact subgraph enumeration (ESU)

  - Terms
    - $$V_{subgraph}$$: currently constructed subgraph(motif)
    - $$V_{extension}$$: set of candidate nodes to extend the motif
    
  1. Enumerating(ESU-Tree)

       - ESU-Tree: Implementation as a recursive function

       - 1st Step: Add Nodes($$v$$) in $$V_{subgraph}$$ and add nodes($$w$$) which touched from $$v$$ in $$V_{extension}$$
       - 2nd Step: Add a pair of $$V_{subgraph}$$ & $$V_{extension}$$(new $$v$$) in $$V_{subgraph}$$ and add nodes which touched from $$v$$ in $$V_{extension}$$ (Only add $$w$$ which have greater node id the node id of $$v$$)
           - Try again 2nd step until the depth attain the $$k$$

     ![ESU](https://user-images.githubusercontent.com/8471958/78194629-a76d8b00-74b8-11ea-8856-36ea0d317c9e.png)

  2. Counting (McKay’s nauty algorithm [McKay 1981])

     - Determine which subgraphs in ESU-Tree leaves are topologically equivalent(isomorphic)
     - Graph Isomorphism: The condition that given two graphs are the same structure

#### Structural Roles in Networks

- Meaning of Roles
  - The function of nodes in a network(e.g., species in ecosystems, individuals in companies)
  - A collection of nodes which have a similar position in a network(**do not need to connect**)
  - Communities/Groups: A collection of nodes which are **well-connected** to each other
- How to find roles
  - Structural equivalence(same relationships to all other nodes) [Lorrain & White 1971]
  - In the adjacency matrix, nodes have the same role have the same vector

#### Discovering Structural Roles in Networks

- RolX: Automatic discovery of nodes’ structural roles in networks [Henderson, et al. 2011b]
  - Unsupervised learning approach
  - No prior knowledge required(automated and behavioral)

![RoIX](https://user-images.githubusercontent.com/8471958/78196900-cf5fed00-74be-11ea-895d-fd598c4bdeef.png)

- Recursive Feature Extraction

  - The matrix of nodes and columns(features)

  - Ego means center node and egonet means the network of nodes are connected ego directly

    - Features that describe the node and its neighbors
    - Characterize the structure of egonet

    ![Recursive feature extraction](https://user-images.githubusercontent.com/8471958/78196963-059d6c80-74bf-11ea-9b03-3a96352c90d4.png)

    - Local features: All measure of the node degree (in-out degree, total degree, weighted feature versions)

    - Egonet features: # of within-egonet edges, edges entering/leaving egonet

  - Additional feature: aggregate functions(mean, sum)
  
    - The number of possible recursive features grows exponentially with each recursive iteration
    - Reduce the number of features using a pruning technique
  
- Role Extraction

  - Cluster nodes based on extracted features using non-negative matrix factorization














[^1]: 유발하다, 유도하다
