---
layout: post
title: "[CS224W Lecture 6] Message Passing and Node Classification"
date: 2020-04-13
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

## Lecture 6

Title: Message Passing and Node Classification<br>
Date: 2020. 04. 09 (THU) ~ 2020. 04. 13 (MON)<br>
Materials: [Slides](http://web.stanford.edu/class/cs224w/slides/06-collective.pdf) [YouTube](https://youtu.be/8szDxEU9u8g)

### Node Classification

- Main question: How to predict labels of unlabeled nodes with some labeled nodes
- Collective classification
  - Intuition: Correlations exist in networks
  - Relational classification, Iterative classification, Belief propagation

#### Correlations in Networks

- Homophily
  - Characteristic of nodes determine the edge(connection)
  - Individuals to associate and bond with similar others
- Influence
  - Connections determine characteristic of nodes
  - Social connections can influence the individual characteristics
- Confounding
  - Other factor(environment) determines both of characteristic of nodes and connections

##### Classification with Network Data(Binary Classification / Homophily network)

- Similar nodes are typically close together or directly connected

- Guilt -by-association

  - Using Feature of $$O$$(object in network), Label of the $$O$$'s neighborhood, Feature of $$O$$'s neighborhood

- Markov Assumption

  - the label $$Y_i$$ depends on the labels of its neighbors $$N_i$$
  - $$P(Y_i|i) = P(Y_i|N_i)$$

  

### Collective classification

  - Steps
    1. Local classifier: Assign initial labels
       - Standard classification task(only use node attributes)
    2. Relational classifier: Capture correlations
    3. Collective inference: Propagate correlations through network
       - Iterate until the inconsistency between neighboring labels is minimized
    
  - Relational classifier

      - Probability of $$Y_i$$(class/label of $$i$$) is a weighted average of probability of its neighbors
      - Labeled nodes initialize true label $$Y$$

    ![Given Graph](https://user-images.githubusercontent.com/8471958/78842737-d5fce000-7a3b-11ea-8511-b98afd88b637.png)

      - Unlabeled nodes initialize $$Y$$ uniformly

    ![Initialize uniformly](https://user-images.githubusercontent.com/8471958/78842766-ead97380-7a3b-11ea-8767-ae8191435d20.png)

      - Update all nodes in a random order until convergence(or until maximum # of iterations)
          - $$P(Y_i = c) = { {1}\over{\left\vert N_i \right\vert}}\sum_{(i, j)\in{E}}W(i, j)P(Y_i = c) = { {1}\over{\sum_{\{(i,j)\}\in{E}}W(i,j)}}\sum_{(i, j)\in{E}}W(i, j)P(Y_i = c)$$
            where $$c$$ is label, $$\left\vert{N_i}\right\vert$$ is the # of neighbors $$W(i, j)$$ is the edge strength(weight)
    
    ![1st Iteration](https://user-images.githubusercontent.com/8471958/78842989-7b17b880-7a3c-11ea-8f8e-20d695246d5a.png)
    
    ![End Iteration](https://user-images.githubusercontent.com/8471958/78843059-b31efb80-7a3c-11ea-92c2-13432f46480a.png)
    
    - Doesn't determine the label of node 4. It's a bridge node of the two groups
    
### Iterative classification

- Using attributes and label both
- Train a classifier using Attributes vector($$a_i$$)
- Steps
    1. Training
         - Covert each node $$i$$ to a flat vector $$a_i$$
         - Train the classifiers
         - There are two different classifier
           - Classifier trained only attributes vector
           - Classifier trained attributes and link vector
    2. Bootstrap phase
         - Use local classifier $$f(a_i)$$ to compute best value for $$Y_i$$
    3. Iteration phase
         - Update node vector $$a_i$$ (Typically update information about link)
         - Update label $$Y_i$$ to $$f(a_i)$$
         - Continue this step until convergence

#### Application of iterative classification framework

- fake reviewer/review detection
- Easy to fake: Behavioral analysis(individual behaviors), Language analysis(content of review)
- Hard to fake: graph structure(relationships between reviewers, reviews, stores)
- Bipartite rating graph(positive review is +1 rating, negative review is -1 rating)
- Fairness($$F(u)$$), Goodness($$G(p)$$), Reliability($$R(u, p)$$)
  - $$F(u) = { {\sum_{ {(u,p)}\in{Out(u)}} R(u, p)} \over {|out(u)|}}$$
  - $$G(p) = { {\sum_{ {(u, p)} \in {In(p)}} \cdot score(u, p)}\over{|In(p)|}}$$
  - $$R(u, p) = { { {1} \over {y_1+y_2}} (y_1 \cdot F(u)) + y_2 \cdot (1- { {|score(u, p) - G(p)|} \over {2}}) }$$

![Fake Review](https://user-images.githubusercontent.com/8471958/79081986-3f8f2e00-7d5d-11ea-8c94-5624fcd65a3d.png)

### Collective Classification: Belief Propagation

- Receive the message(state, attribute, etc) from neighbors and update, then pass toward other neighbors
- After receive from and pass toward every neighbors, node can calculate their own state
- However, it doesn't work loopy(cyclic) graph

![Belief Propagation](https://user-images.githubusercontent.com/8471958/79082287-8bdb6d80-7d5f-11ea-88ce-26e7a2ae80b7.png)

#### Loopy BP algorithm

##### Notation

- Label-label potential matrix($$\psi$$): $$\psi(Y_i, Y_j)$$ is probability of a node $$j$$ being in the state $$Y_j$$ given that it has a $$i$$ neighbor in state $$Y_i$$
- Prior belief($$\phi$$): Probability $$\phi_i(Y_i)$$ of node $$i$$ being in the state $$Y_i$$
- $$m_{i \to j}(Y_j)$$ is $$i$$'s estimate of $$j$$ being in the state $$Y_j$$
- $$\mathcal{L}$$ is the set of all states

##### Loopy BP algorithm

- Initialize all message to 1 and repeat to calculate for each node
- After convergence, label-label potential is 1

![Loopy BP](https://user-images.githubusercontent.com/8471958/79082492-55065700-7d61-11ea-8569-7b110c8bb24d.png)

![Convergence](https://user-images.githubusercontent.com/8471958/79082527-a1ea2d80-7d61-11ea-9981-b5af66ec76a8.png)

#### Application of Belief Propagation

- Online Auction Fraud(especially, Non-delivery fraud)

- Fraudsters try to form near-bipartite cores (2 roles)

  - Accomplices: trades with honest
  - Fraudsters: trades with accomplice and fraud with honest
    - These actions maintain their feedback score over than fraudsters who fraud with honest only

  ![fraud actions](https://user-images.githubusercontent.com/8471958/79082674-fa6dfa80-7d62-11ea-968c-09a88af209aa.png)

- Three groups have different provabilities

  ![fraud detection](https://user-images.githubusercontent.com/8471958/79082685-28ebd580-7d63-11ea-9afb-8b6f0c160303.png)















