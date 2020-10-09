---
layout: post
title: "[CS224W Lecture 4] Community Structure in Networks"
date: 2020-04-06
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

## Lecture 4

Title: Community Structure in Networks<br>
Date: 2020. 04. 03 (FRI) ~ 2020. 04. 06 (MON)<br>
Materials: [Slides](http://web.stanford.edu/class/cs224w/slides/04-communities.pdf) [YouTube](https://youtu.be/pnYwvN8TCio)

### Networks and Communities

- Roles(RoIX) based on the feature, Communities(Fast Modularity) based on cluster

#### Flow of Information

- Questions
  - How does information flow through the network?
  - How do people find out about new job?
    - Contacts were often acquaintances(less than friends)[^1] rather than close friends

- Granovetter's Answer

  - Link perspectives: Structural perspective and Interpersonal perspective
  - Structural role(**Triadic Closure**)
    - Structure: Well-embedded edges are socially strong
    - Information: Well-embedded heavily redundant in terms of[^2] information access
      - Through the weak connection, the node can get newer information which can't get the well-embedded parts
      - So, in the job seeking problem, people contact to acquaintances rather than friends
    - Strong structure(Socially Strong) means each neighbors are also neighbors of others
      - Weight of edge means interpersonal strength
    - Triadic Closure means High clustering coefficient 
  
  ![Granovetter Structure](https://user-images.githubusercontent.com/8471958/78308676-0ba95100-7584-11ea-98d6-05809e4f7611.png)
  
- Edge overlap

  - The ratio of the # of mutual friends and # of union friends
  - Overlap = 0 means an edge is a local bridge

- Edge Removal

  - By Strength: The network structure are decomposed faster when remove low # of weighted edge than high
  - By Overlap: The network structure are decomposed faster when remove low edge overlap than high

![Network](https://user-images.githubusercontent.com/8471958/78310115-e0c0fc00-7587-11ea-84f8-214e5fdd21d1.png)

### Network Communities

- Definition
  - Sets of tightly connected nodes
  - Sets of nodes with **lots of internal** connections and **few external** ones
- Communities can exchange clusters, groups and modules

- Zachary's Karate club network(Social Network Data Example)
  - In the network, there are two different karate club
  - Split could be explained by a minimum cut in the network

#### Modularity $$Q$$

- How well as network is partitioned into communities
- $$Q \propto \sum_{\{s\}\in{S}}[($$ # edges within group $$s)$$ - $$($$expected # edges within group $$s)]$$
  - To calculated expected # edges with group, need a null model
  - **Null Model(Configuration Model)**
    - The expected # of edges between nodes $$i$$ and $$j$$
    - $${k_i \cdot k_j} \over {2m}$$ ($$k_i, K_j$$ means degrees $$i$$ and $$j$$)
- Modularity of partitioning $$S$$ of graph $$G$$
  - $$Q(G,S) = {\{1\}\over{2m}}\sum_{\{s\}\in{S}}\sum_{\{i\}\in{s}}\sum_{\{j\}\in{s}}(A_{ij} - {\{k_ik_j\}\over{2m}})$$
  - where $$A_{ij}$$ = 1 if $$i$$ -> j 0 otherwise
- Modularity values take range [-1, 1]
  - $$Q$$ greater than **0.3 - 0.7** means **significant community structure**
  - Negative $$Q$$ means anti-communities
    - Positive $$Q$$ means that nodes in the same communities are connected stronger than the null model (More connection than the null model)
    - If $$Q = 0$$, this community doesn't a difference with random connection. However, if $$Q > 0$$, this community have more connections(edges) than the random model.
- We can identify communities by maximizing modularity

![Modularity](https://user-images.githubusercontent.com/8471958/78512186-6b4e6900-77dd-11ea-8c87-7020f533fcf0.png)

### Louvain Algorithm

- Using **Greedy algorithm** for community detection(**Greedily maximizes modularity**)

  - Using to study large networks
  - Fast, Rapid convergence, High modularity output

- Steps

  1. Optimize modularity by local changes to node-communities memberships
     - Changing community of each node, calculate modularity delta($$\Delta{Q}$$)
       - Possible communities to change are only neighbor of node 
       - $$\Delta{Q} = \Delta{Q({i}\to{C})} + \Delta{Q({D}\to{i})}$$ 
       - Modularity delta means the aggregation of change of modularity when node $$i$$ put into community $$C$$ and change of modularity when node $$i$$ pull from community $$D$$
     - The nodes' community determine the community with maximum modularity
     - Repeat until no increase modularity
  2. Aggregate nodes to super-nodes to build a new network
  3. Repeat these two steps until no increase of modularity

  ![Louvain Algorithm](https://user-images.githubusercontent.com/8471958/78512454-8f12ae80-77df-11ea-82ef-49c7fd30a02a.png)

### Detecting Overlapping Communities: BigCLAM

1. Define generative model for graphs

   - Community Affiliation Graph Model(AGM)

2. Discover communities

   - Given graph G, make the assumption that G was generated by AGM

   - Find the best AGM = discover communities

#### Community Affiliation Graph Model(AGM)

- $$V, C, M, \{p_c\}$$

- $$V$$: Nodes

- $$C$$: Communities

- $$M$$: Memberships, The link between nodes and communities

- $$P_c$$: The probability that Node $$v$$ has Membership $$m$$ to Community $$c$$

##### To detect communities with AGM

- Given a Graph, find the model $$F$$

- Terms
    - $$M$$: Affiliation graph
    - $$C$$: Number of communities like $$k$$ in k-means
    - $$p_c$$: Parameters
- Maximum likelihood estimation
  - $$arg_F max P(G|F)$$
  - $$P(G|F) = \prod_{\{(u,v)\}\in{G}}P(u,v)\prod_{\{(u,v)\notin{G}\}}(1-P(u, v))$$
- Membership has strengths ($$F_{uA}$$)
  - How strong connect node to community
  - $$F_{uA} = 0$$ means that node $$u$$ is not a member of community $$A$$













[^1]: Acquaintance: 지인(친구보다는 덜 친분이 있는)
[^2]: In terms of: ~면에서, ~관점에서
