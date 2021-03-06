---
layout: post
title: "[Deep Reinforcement Learning Nanodegree Chapter 1] Foundations of Reinforcement Learning"
date: 2020-11-20
categories:
- Study
excerpt: "Foundations of Reinforcement Learning"
tags:
- Nanodegree
- Deep Reinforcement Learning
commnets: false
---

# 1. Foundations of Reinforcement Learning

**Official course description**

*The first part begins with a simple introduction to reinforcement learning.  You'll learn how to define real-world problems as **Markov Decision Processes (MDPs)**, so that they can be solved with reinforcement learning.*  



*![How might we use reinforcement learning to teach a robot to walk?](https://video.udacity-data.com/topher/2018/May/5af5ba38_mjg2mzcyma/mjg2mzcyma.gif)*

*How might we use [reinforcement learning](https://arxiv.org/pdf/1803.05580.pdf) to teach a robot to walk? ([Source](https://spectrum.ieee.org/automaton/robotics/industrial-robots/agility-robotics-introduces-cassie-a-dynamic-and-talented-robot-delivery-ostrich))*

*Then, you'll implement classical methods such as **SARSA** and **Q-learning** to solve several environments in OpenAI Gym.  You'll then explore how to use techniques such as **tile coding** and **coarse coding** to expand the size of the problems that can be solved with traditional reinforcement learning algorithms.*



*![Train a car to navigate a steep hill using Q-learning.](https://video.udacity-data.com/topher/2018/June/5b1721df_mountain-car-cts/mountain-car-cts.gif)*

*Train a car to navigate a steep hill using Q-learning.*

[toc]

## Foundations of Reinforcement Learning

### Reinforcement learning(RL)

- Building code that can learn to perform complex tasks by itself

#### Applications

- Games: AlphaGo, Atari breakout, DOTA
- Self-driving: Car(Uber, Google), Ship, Airplane
- Robotics: Walking robots

#### Terminologies

- **Agent**: learner or decision-maker, born into the world w/o any understanding of how anything works
- The agent learned to interact with the environment
- **Feedback**: Rewards(Positive feedback) or discoursing feedback
- **Goal**: Maximize rewards

#### Exploration-Exploitation Dilemma

- **Exploration**: Exploring potential hypotheses for how to choose actions

- **Exploitation**: Exploiting limited knowledge about what is already known should work well

- Balancing these competing requirements

### Reinforcement learning framework

#### Setting

![image](https://user-images.githubusercontent.com/8471958/98735523-59770a80-2358-11eb-911b-23bbceafb418.png)

- State(Observation, <img src="https://render.githubusercontent.com/render/math?math=S_t">): the environment presents a situation to the agent
- Action(<img src="https://render.githubusercontent.com/render/math?math=A_t">): appropriate actions in response
- Reward(<img src="https://render.githubusercontent.com/render/math?math=R_t">): One-time step later, the agent receives
- **The goal of the Agent: Maximize expected cumulative reward**

#### Episodic task & Continuing task

- Task: an instance of the reinforcement learning problem
- Episodic task
  - Tasks with a well-defined starting and ending point
  - There is a specific ending point(**Terminal state**)
  - An Episode means that interaction ends at some time step <img src="https://render.githubusercontent.com/render/math?math=T">
  - The reward is given at the ending point
- Continuing task
  - Tasks that continue forever, without end
  - Interaction continues without limit

#### Rewards

-  Reward Hypothesis: All goals can be framed as the maximization of expected cumulative reward

- Cumulative reward(return): <img src="https://render.githubusercontent.com/render/math?math=G_t = R_{t%2B1}%2BR_{t%2B2}%2BR_{t%2B3}%2B\R_{t%2B4}%2B\cdots">
  - At the time step <img src="https://render.githubusercontent.com/render/math?math=t">, the agent picks <img src="https://render.githubusercontent.com/render/math?math=A_t">to maximize (expected) <img src="https://render.githubusercontent.com/render/math?math=G_t">
- Discounted reward(return): <img src="https://render.githubusercontent.com/render/math?math=G_t = R_{t%2B1}%2B\gamma R_{t%2B2}%2B\gamma^2 R_{t%2B3}%2B\gamma^3 R_{t%2B4}%2B\cdots">
  - discount rate <img src="https://render.githubusercontent.com/render/math?math=\gamma \in [0, 1]">

#### Markov Decision Process (MDP)

![img](https://video.udacity-data.com/topher/2017/September/59c3f51a_screen-shot-2017-09-21-at-12.20.30-pm/screen-shot-2017-09-21-at-12.20.30-pm.png)

- A (finite) MDP is defined by
  - a (finite) set of states <img src="https://render.githubusercontent.com/render/math?math=S">
  - a (finite) set of actions <img src="https://render.githubusercontent.com/render/math?math=A">
  - a (finite) set of rewards <img src="https://render.githubusercontent.com/render/math?math=R">
  - the one-step dynamics of the environment
    <img src="https://render.githubusercontent.com/render/math?math=p(s',r|s,a) \doteq \mathbb{P}(S_{t%2B1}=s', R_{t%2B1}=r|S_t = s, A_t=a)"> for all <img src="https://render.githubusercontent.com/render/math?math=s, s`, a and r">
  - a discount rate <img src="https://render.githubusercontent.com/render/math?math=\gamma \in [0, 1]">

#### Policies

##### Policies

- A policy determines how an agent chooses an action in response to the current state
- It specifies how the agent responds to situations that the environment has presented.

- Deterministic policy(<img src="https://render.githubusercontent.com/render/math?math=\pi:\mathcal{S} \rightarrow \mathcal{A}">)
  - Return a specific action by state
- Stochastic policy(<img src="https://render.githubusercontent.com/render/math?math=\pi:\mathcal{S} \times \mathcal{A} \rightarrow [0,1])">
  - <img src="https://render.githubusercontent.com/render/math?math=\pi(a|s) = \mathbb{P}(A_t = a | S_t = s)"> 
  - Return probabilities of action set by state and action set

##### Optimal Policies

1. State-Value Function
   - The value of state <img src="https://render.githubusercontent.com/render/math?math=s"> under a policy <img src="https://render.githubusercontent.com/render/math?math=\pi">
   - <img src="https://render.githubusercontent.com/render/math?math=v_\pi(s) = \mathbb{E}_\pi[G_t|S_t=s]"> 
   - For each state <img src="https://render.githubusercontent.com/render/math?math=s">, it yields the expected return(<img src="https://render.githubusercontent.com/render/math?math=G_t">) if the agent starts in state s(<img src="https://render.githubusercontent.com/render/math?math=S_t=s">) and then uses policy(<img src="https://render.githubusercontent.com/render/math?math=\pi">) to choose its actions for all time steps.

   - Bellman Expectation Equation
     - The equation to calculate the value of any state is the sum of the immediate reward and the discounted value of the state that follows.
     - <img src="https://render.githubusercontent.com/render/math?math=v_\pi(s) = \mathbb{E}_\pi[R_{t%2B1} %2B \gamma v_\pi(S_{t%2B1}|S_t = s)]"> 

2. Action-Value Function

   - The value of taking action <img src="https://render.githubusercontent.com/render/math?math=a"> in the state <img src="https://render.githubusercontent.com/render/math?math=s"> under a policy <img src="https://render.githubusercontent.com/render/math?math=\pi">
   - <img src="https://render.githubusercontent.com/render/math?math=q_\pi(s, a) = \mathbb{E}_\pi[G_t|S_t=s,A_t=a]"> 
   - <img src="https://render.githubusercontent.com/render/math?math=v_\pi(s) = q_\pi(s, \pi(s))$ if $\pi$ is a deterministic policy and all $s \in \mathcal{S}"> 

3. Optimality
   - Compare with two policies
     - <img src="https://render.githubusercontent.com/render/math?math=\pi' \ge \pi"> if and only if <img src="https://render.githubusercontent.com/render/math?math=v_{\pi'}(s) \ge v_\pi(s)\text{ for all }s \in \mathcal{S}">
   - The optimal policy <img src="https://render.githubusercontent.com/render/math?math=\pi_\star"> satisfies <img src="https://render.githubusercontent.com/render/math?math=\pi_\star \ge \pi \text{ for all }\pi">
   - Once the agent determines the optimal action-value function <img src="https://render.githubusercontent.com/render/math?math=q_*">, it can quickly obtain an optimal policy <img src="https://render.githubusercontent.com/render/math?math=\pi_*"> by setting <img src="https://render.githubusercontent.com/render/math?math=\pi_*(s) = \arg\max_{a\in\mathcal{A}(s)} q_*(s,a)">.

### [Monte Carlo Methods](https://github.com/madigun697/udacity-nanodegree/tree/master/Deep%20Reinforcement%20Learning%20Nano%20Degree/1.%20Foundations%20of%20Reinforcement%20Learning/Lesson%208.%20Monte%20Carlo%20Methods)

- Equiprobable random policy: the agent chooses an action in the action set with the same probabilities.

- The action-value function with a Q-table
  - To find an optimal policy, the agent tries many episodes.
  - Q-table is the expected value matrix by states and actions based on the results of episodes.
    - Each episode creates a matrix, and the final Q-table has average values of these matrixes.
- MC Prediction
  - Every-visit MC Prediction: Fill out the Q-table with the average value of observations
  - First-visit MC Prediction: Fill out the Q-table with the value of the first observation

#### MC Control

- Control Problem: Estimate the optimal policy
- MC Control Method: a solution for the control problem
  - Step 1: Using the policy <img src="https://render.githubusercontent.com/render/math?math=\pi"> to construct the Q-table
  - Step 2: improving the policy by changing it to be <img src="https://render.githubusercontent.com/render/math?math=\epsilon">-greedy with respect to the Q-table (<img src="https://render.githubusercontent.com/render/math?math=\pi' \leftarrow \epsilon\text{-greedy}(Q), \pi \leftarrow \pi'">)
  - Eventually, obtain the optimal policy <img src="https://render.githubusercontent.com/render/math?math=\pi_*">

#### Greedy Policies

- Greedy Policies
  - Collect episodes with <img src="https://render.githubusercontent.com/render/math?math=\pi"> and estimate the Q-table
  - Initial policy is the equiprobable random policy
  - Use the Q-table to find a better policy <img src="https://render.githubusercontent.com/render/math?math=\pi'"> and set <img src="https://render.githubusercontent.com/render/math?math=\pi \leftarrow \pi'">
  - Iterate these steps

- Epsilon-Greedy Policies
  - Greedy policy always selects the greedy action
  - Epsilon-Greedy policy is most likely selects the greedy action
  - Set a specific value <img src="https://render.githubusercontent.com/render/math?math=\epsilon (0\le\epsilon\le1)">
  - <img src="https://render.githubusercontent.com/render/math?math=\epsilon"> is a probability that the agent selects random actions instead of the greedy action

#### Exploration-Exploitation Dilemma

- Exploration: Prefer to select randomly
- Exploitation: Prefer to select the greedy action
- Greedy in the Limit with Infinite Exploration(GLIE)
  - The conditions to guarantee that MC control converges to the optimal policy <img src="https://render.githubusercontent.com/render/math?math=\pi_*">
  - Every state-action pair <img src="https://render.githubusercontent.com/render/math?math=s, a">(for all <img src="https://render.githubusercontent.com/render/math?math=s \in \mathcal{S}"> and <img src="https://render.githubusercontent.com/render/math?math=a \in \mathcal{A}(s)">) is visited infinitely many times
    - The agent continues to explore (never stop to explore)
    - <img src="https://render.githubusercontent.com/render/math?math=\epsilon_t > 0"> 
  - The policy converges to a policy that is greedy with respect to the action-value function estimate <img src="https://render.githubusercontent.com/render/math?math=\Q">
    - The agent gradually exploits more and explores less
    - <img src="https://render.githubusercontent.com/render/math?math=\lim_{t\rightarrow \infty}\epsilon_t = 0"> 

####  Incremental Mean

- To reduce time, update Q-table after every episode.
- Update Q-table could be used to improve the policy
  - <img src="https://render.githubusercontent.com/render/math?math=Q \leftarrow Q %2B {1 \over N}(G - Q)"> 

- Problem
  - <img src="https://render.githubusercontent.com/render/math?math=N">(The number of steps in the episode) is bigger and bigger, the effect of denotation(<img src="https://render.githubusercontent.com/render/math?math=G-Q">) is smaller and smaller
  - Every state-action pair is not effected to <img src="https://render.githubusercontent.com/render/math?math=Q"> evenly

#### Constant-alpha

- To solve the problem of Incremental Mean, uses constant value, <img src="https://render.githubusercontent.com/render/math?math=\alpha"> instead of <img src="https://render.githubusercontent.com/render/math?math=1 \over N">
  - <img src="https://render.githubusercontent.com/render/math?math=Q \leftarrow Q %2B \alpha(G-Q)">
    <img src="https://render.githubusercontent.com/render/math?math=Q \leftarrow (1-\alpha)Q %2B \alpha G">
  - if <img src="https://render.githubusercontent.com/render/math?math=\alpha = 0">, then the Q-table is never updated → **Exploitation**
    if <img src="https://render.githubusercontent.com/render/math?math=\alpha = 1">, then the previous result(previous $Q$) is always ignored → **Exploration**

### [Temporal-Difference Methods](https://github.com/madigun697/udacity-nanodegree/tree/master/Deep%20Reinforcement%20Learning%20Nano%20Degree/1.%20Foundations%20of%20Reinforcement%20Learning/Lesson%209.%20Temporal-Defference%20Methods)

- Monte Carlo methods need to end the interaction
  - In the self-driving car, MC methods update the policy after the crash
- Temporal-Difference methods update the policy every step
- On-policy TD control methods (like Expected Sarsa and Sarsa) have  better online performance than off-policy TD control methods (like  Q-learning). 
- Expected Sarsa generally achieves better performance than Sarsa.

#### TD Control

- It's similar to MC constant-alpha
- The value is updated in every step instead of after the episode ends
- **In the MC, Q-value is updated to sum of future rewards when the episode is ended. But in the TD, Q-value is udpated to sum of current rewards and next Q-value(expected sum of future reward after <img src="https://render.githubusercontent.com/render/math?math=t%2B1">) when every single stemp**
  - **It means that the agent considers next action's Q-value as the sum of future reward**
  - MC: Update the effect of denotation <img src="https://render.githubusercontent.com/render/math?math=G-Q">, where <img src="https://render.githubusercontent.com/render/math?math=G"> is the cumulated rewards after time <img src="https://render.githubusercontent.com/render/math?math=t">
  - TD (**Sarsa**): Using <img src="https://render.githubusercontent.com/render/math?math=R_{t%2B1} %2B \gamma Q_{t%2B1}"> instead of <img src="https://render.githubusercontent.com/render/math?math=G">

#### Sarsa (Sarsa(0))

- <img src="https://render.githubusercontent.com/render/math?math=Q(S_t, A_t) \leftarrow Q(S_t, A_t) %2B \alpha(R_{t%2B1} %2B \gamma Q(S_{t%2B1}, A_{t%2B1}) - Q(S_t, A_t))"> 

#### Q-Learning(Sarsamax)

- In the Sarsa, uses <img src="https://render.githubusercontent.com/render/math?math=Q(S_{t%2B1}, A_{t%2B1})"> to estimate future rewards
- In the Q-Learning, uses <img src="https://render.githubusercontent.com/render/math?math=max_{a \in \mathcal{A}} Q(S_{t%2B1}, a)">
- <img src="https://render.githubusercontent.com/render/math?math=Q(S_t, A_t) \leftarrow Q(S_t, A_t) %2B \alpha(R_{t%2B1} %2B \gamma max_{a \in \mathcal{A}} Q(S_{t%2B1}, a) - Q(S_t, A_t))"> 

#### Expected Sarsa

- Using expected rewards (probability of actions x expected rewards) instead of maximum rewards in the Q-learning
- <img src="https://render.githubusercontent.com/render/math?math=Q(S_t, A_t) \leftarrow Q(S_t, A_t) %2B \alpha(R_{t%2B1} %2B \gamma \sum_{a \in \mathcal{A}} \pi (a|S_{t%2B1}) Q(S_{t%2B1}, a) - Q(S_t, A_t))"> 

#### [Mini-Project(OpenAI Gym Taxi)](https://github.com/madigun697/udacity-nanodegree/tree/master/Deep%20Reinforcement%20Learning%20Nano%20Degree/1.%20Foundations%20of%20Reinforcement%20Learning/Lesson%2010.%20Solve%20OpenAI%20Gym's%20Taxi-v2%20Task)

### Deep Reinforcement Learning

#### Overview of Reinforcement Learning in Discrete spaces

- Finite Markov Decision Processes (MDPs), reinforcement learning environments where the number of states and actions is limited, is possible to represent the action-value function with a table, dictionary, or other finite structure.

- But what about MDPs with much larger spaces?  Consider that the Q-table must have a row for each state. For instance, if there are 10 million possible states, the Q-table must have 10 million rows.  Furthermore, if the state space is the set  of continuous real-valued numbers (an infinite set!), it becomes impossible to represent the action values in a finite structure! 

- Markov Decision Processes (MDPs): <img src="https://render.githubusercontent.com/render/math?math=\mathcal{S}, \mathcal{A}, \mathcal{P}, \mathcal{R}, \gamma">
  - State Transition and Reward Model: <img src="https://render.githubusercontent.com/render/math?math=\mathbb{P}(S_{t%2B1}, R_{t%2B1}|S_t, A_t)">
  - State Value Function: <img src="https://render.githubusercontent.com/render/math?math=V(S)">
  - Action Value Function: <img src="https://render.githubusercontent.com/render/math?math=Q(S,A)">
  - Goal: Find an optimal policy <img src="https://render.githubusercontent.com/render/math?math=\pi_*"> that maximizes the total *expected* reward

- Reinforcement Learning Algorithms
  - Model-Based Learning (Dynamic Programming)
    - Policy Iteration
    - Value Iteration
  - Model-Free Learning
    - Monte Carlo Methods
    - Temporal-Difference Methods

- Deep Reinforcement Learning
  - RL in Continuous Spaces
  - Deep Q-Learning
  - Policy Gradients
  - Actor-Critic Methods

#### Continuous Spaces

- Discretization
  - [Continuous spaces → Discrete spaces](https://github.com/madigun697/udacity-nanodegree/tree/master/Deep%20Reinforcement%20Learning%20Nano%20Degree/1.%20Foundations%20of%20Reinforcement%20Learning/Lesson%2011.%20RL%20in%20Continuous%20Spaces/Discretization)
  - Non-Uniform Discretization
  - [Tile Coding](https://github.com/madigun697/udacity-nanodegree/tree/master/Deep%20Reinforcement%20Learning%20Nano%20Degree/1.%20Foundations%20of%20Reinforcement%20Learning/Lesson%2011.%20RL%20in%20Continuous%20Spaces/Tile%20Coding), Coarse Coding
    - Using Multiple Q-tables
    - Greedy action is the action has a maximum average Q-value
    - Tile Coding using multiple layers by rectangles, Coarse Coding using multiple layers by circles
- Function Approximation
  - Linear Function Approximation
  - Non-Linear Function Approximation

