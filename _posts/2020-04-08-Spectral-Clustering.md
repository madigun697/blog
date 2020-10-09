---
layout: post
title: "[CS224W Lecture 5] Spectral Clustering "
date: 2020-04-08
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

## Lecture 5

Title: Spectral Clustering <br>
Date: 2020. 04. 07 (TUE) ~ 2020. 04. 08 (WED)<br>
Materials: [Slides](http://web.stanford.edu/class/cs224w/slides/05-spectral.pdf) [YouTube](https://youtu.be/0eNQnc0eOB4)<br>
Additional Materials: [ratsgo's blog](https://ratsgo.github.io/machine%20learning/2017/04/27/spectral/)

### Spectral Clustering

- Using eigenvectors, eigenvalue, and Linear algebra, 
- Algorithms
  1. Pre-processing
  
     - Build Laplacian matrix $$L$$
  
  2. Decomposition(Compute eigenvalues and eigenvectors)
  
     - Find eigenvalues $$\lambda $$ and eigenvectors $$x$$ of the matrix $$L$$
     - Find the second smallest eigenvalue $$\lambda_2 $$ and corresponding eigenvector $$x_2$$
  
  3. Grouping
  
     - Sort $$x_2$$ and identify clusters by splitting point
     - Split at 0 or median value
  
     ![Spectral Partitioning](https://user-images.githubusercontent.com/8471958/78726258-78e62900-796c-11ea-867e-d21daaf7c839.png)
  
     ![Spectral Partitioning 2](https://user-images.githubusercontent.com/8471958/78726333-a632d700-796c-11ea-987a-418179555a1c.png)

#### Graph Partitioning

- Using Network modularity, good partitioning means maximizing # of within-group connections and minimizing # of between-group connections

#### Graph Cuts

- Set of edges with one endpoint in each group
- $$cut(A, B) = \sum_{\{i\}\in{A}, {j}\in{B}}w_{ij}$$

![Graph Cuts](https://user-images.githubusercontent.com/8471958/78612368-20e3ef80-78a4-11ea-9612-22b5677f773c.png)

- Minimum-cut ($$arg min_{A,B}cut(A,B)$$)
  - However, this criterion doesn't mean optimal cut (Degenerate[^1] case)
  - Because of that doesn't consider internal cluster connectivity (only consider external cluster connectivity)
- Conductance (Smaller is better)
  - $$\varnothing(A,B) = {\{cut(A,B)\}\over{min(vol(A),vol(B))}}$$
  - where, $$vol(A)$$ means total weighted degree of the nodes in A
    $$vol(A) = \sum_{\{i\}\in{A}}k_i$$
  - It's more balanced partitions.
  - However, its computing is NP-hard

#### Spectral Graph Partitioning

- $$A$$(Adjacency matrix of undirected $$G$$)
- $$x$$ is vector of dimensionality($$\mathfrak{R}^n$$): vector of nodes(?)
- $$y_i$$ is a sum of labels $$x_j$$ of neighbors of $$i$$()$$y = A \cdot x$$)
- $${A}\cdot{x} = {\lambda}\cdot{x} $$
  where $$x$$ is **eigenvector** and $$\lambda$$ is **eigenvalue**
- If there is a **$$d$$-regular graph**(it means every node connect by same degree), its **eigenvector is {1,1,...,1,1}** and **eigenvalue is $$d$$**
- We can split graph when $$x_n$$ and $$x_{n-1}$$ are different
  In the $$d$$-regular graph, $$x_n$$ and $$x_{n-1}$$ are always same. However, if these two values are different, it means that there are weaker connection and it is the split point
- So, $$x_{n-1}$$ splits the nodes into two group, when $$x_{n-1}[i] > 0$$ or $$x_{n-1}[i] < 0$$ (positive and negative means left side group or right side group)

#### Matrix Representations

- Adjacency matrix ($$A$$)
- Degree matrix ($$D$$)
- Laplacian matrix ($$L = D - A$$)
  - If $$x = (1,...,1)$$ then $${L}\cdot{x} = 0$$ and so $$\lambda = \lambda_1 = 0$$

#### $$\lambda_2$$ Optimization

- $$lambda_2$$ means the second smallest eigenvalue (The smallest eigenvalue is always zero)
- $$\lambda_2 = min{\{ \sum_{\{(i,j)\in{E}\}}(x_i - x_j)^2 \}\over{ \sum_i x_i^2 }}$$
- $$\sum_{\{(i,j)\}\in{E}}(x_i-x_j)^2$$: We can find split point where each value in sigma is not zero
- $$\sum_i x_I^2$$: This is just 1
- So, minimum $$\lambda_2$$ means the optimization of graph partitioning

![Lambda2](https://user-images.githubusercontent.com/8471958/78615411-686e7980-78ac-11ea-8ba2-685259545ed2.png)

#### Rayleigh Theorem

- Find Optimal Cut [Fiedler'73]
  - $${y_i} = \begin{cases} +1 & if\ {i} \in {A} \\ -1 & if\ {i} \in {B}\end{cases}$$
  - Minimize $$\sum_iy_i$$, it's the optimal cut in partition (A, B)

#### k-Way Spectral Clustering

- Recursive bi-partitioning

- Cluster multiple eigenvectors

- ##### Cluster multiple eigenvectors

  - Advantages
    - Approximates the optimal cut
    - Emphasizes cohesive clusters
    - Well-separated space
  - Eigengap
    - The difference between two consecutive[^2] eigenvalues
    - $$\Delta_k = \left| \lambda_k - \lambda_{k-1} \right|$$
    - $$k$$ is the value which maximizes eigengap

### Motif-Based Spectral Clustering

- Find specific motif in the graph and cluster

  - Different motifs reveal different modular structures

  ![Motif-Based Spectral Clustering](https://user-images.githubusercontent.com/8471958/78727474-ba2c0800-796f-11ea-8b84-740e63233fad.png)

- Motifs cut and conductance

  - Motif conductance($$\varnothing(S) = {\{\#(motifs cut)\} \over {vol_m(S)}}$$)
  - \# of motif cut means how many motif cut $$S$$
  - $$vol_M(S)$$ means how many end points in the subgraph $$S$$
    - $$vol_m(S)$$ include the end points of cutting motif

  ![motif cut](https://user-images.githubusercontent.com/8471958/78729340-c1094980-7974-11ea-8bd9-3f91273c6acb.png)

  - Finding set $$S$$ with the minimal motif conductance is NP-hard

- Motif Spectral Clustering

  - Create a new weighted graph $$W^{(M)}$$ and Apply spectral clustering on $$W^{(M)}$$

  - Algorithm

    1. Pre-processing

       - Create weighted graph $$W^{(M)}$$
         - Remove irrelevant edges
         - Add weights how many times participates in the motif $$M$$

    2. Decomposition

    3. Grouping(Sweep Procedure)

       - The select to best cut using motif conductance

       ![Motif Sepctral Clustering](https://user-images.githubusercontent.com/8471958/78730089-c8315700-7976-11ea-92a7-98f9e1dd989c.png)

  - Try to multiple motifs and select one of them
    - Best motif decrease conductance quickly and less global minimum conductance











[^1]: Degenerate: 퇴보하다, 퇴화하다
[^2]: Consecutive: 연속적인
