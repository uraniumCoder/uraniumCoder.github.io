---
title: Rethinking robotics with natural language
categories:
- General
- Robotics
- Natural Language Processing
excerpt: |
    As the capabilities of ML continue to grow, so does our ability to build intelligent robots which can do more than precise, hard-coded movements. Today's robots are able to make plans for complex tasks such as brushing teeth or cleaning up a counter. In this blog, we investigate how to make robots with such human-like intelligence.
feature_image: "/assets/background.jpeg"
---

As the capabilities of machine learning continue to grow, so does our ability to build intelligent robots which can do more than precise, hard-coded movements. Today's robots are able to make plans for complex tasks such as brushing teeth or cleaning up a counter. In this blog, we investigate how to make robots with such human-like intelligence.

## How robots learn
Robots learning is typically formulated through the language of reinforcement learning (RL). In short, the RL framework consists of an agent interacting with an environment which tells the agent its current state and possible actions. The environment also gives the agent rewards and a new state after taking each action, which the agent must learn on its own. 

The agent seeks to maximize its cumulative rewards over time, and RL studies the learning algorithms and techniques that can be used to train agents that achieve high rewards. Popular algorithms for RL include the Deep Q-Network (DQN), Cross-Entropy Method (CEM), Behavioral Cloning (BC), and Proximal Policy Optimization (PPO).

## Training in simulation
Sometimes, obtaining data to train robots can be prohibitively slow and expensive due to the physical hardware needed to run robots. A common technique is to train the robot in simulation instead. A simulation is an approximate replication of the real environment, where the robot can still move its parts and interacts with other objects through the laws of physics. With rapidly increasing computing power and new advances in computer vision such as DALLE-2, it is now possible to make photorealistic simulations where the robot can learn just as well as in the real world.

{% include figure.html image="/assets/posts/2022-09-22/simulation.png" caption="The Playground environment. Image by Lynch et al. via <a href='https://learning-from-play.github.io/'>learning-from-play.github.io</a>." width="500" height="1000" %}

## Hierarchical Learning
Direct RL algorithms such as DQN and PPO are great for small problems but do not scale well with the environment. Consider the problem of filling a water bottle. A human will probably approach this problem by first looking for a fountain, but a naive RL algorithm would tell you which direction it will take its first step in. 

Hierarchical learning overcomes this complexity by mimicking the human thought process and splitting up the task into high-level planning and low-level execution. In hierarchical learning, an agent has access to multiple low-level skills that it can execute at will, such as picking up a water bottle or navigating to a water fountain. Then, a high-level planner algorithm treats low-level skills as atomic actions and devises a sequence of low-level skills which achieves the desired task when executed sequentially.

{% include figure.html image="/assets/posts/2022-09-22/feudal.png" caption="Hierarchical learning. Image by Dayan et al. via <a href='https://proceedings.neurips.cc/paper/1992/file/d14220ee66aeec73c49038385428ec4c-Paper.pdf'>Feudal Reinforcement Learning</a>." width="500" height="1000" %}

## Language controlled robotics
With hierarchical learning, the high-level planner no longer thinks about concrete physical movements, it instead thinks about abstract ideas which are described by natural language. Thus, a robot's reasoning process can be guided through a natural language prior. The language-based robot would also be able to follow natural language commands such as:

```I want to fill my water bottle. Can you please do this for me?```

As a result, using natural language knowledge as a prior accelerates training, increases data efficiency, increases usability, and produces high-performance robots capable of generalizing beyond their training set through their prior knowledge about the world. 

In practice, we use natural language priors by asking a large language model about what actions the robot should perform. Large language models, such as a fine-tuned GPT-2 or GPT-3, are models which capture the contiguous flow of ideas found in human-written texts. This can be done by either querying the language model for all the actions at once or through a prompt like:
```
Task: Tell me how to fill a water bottle. 
Instructions: 
```
or by querying one action at a time, given the previous actions, through a prompt like
```
Task: Tell me how to fill a water bottle. 
Instructions: 
1. find a water bottle
2. pick up the water bottle
3. 
```
One problem that arises is that the language model is unaware of the robot's surroundings and consequently might give instructions which are unachievable by the robot such as "find a kettle" or "go to a nearby Starbucks". A recent paper by a team at Google introduces the [SayCan method](https://say-can.github.io/ "SayCan method"), in which a fixed list of the planner's possible actions (i.e. the low-level skills of the robot) are queried for their probabilities of being an output of the language model, and the action with the highest probability is chosen. This guarantees that the robot's plans always consist of achievable actions.

## Possible applications
There is a wide range of applications for language-controlled robots with general intelligence, ranging from the automation of routine tasks such as "pack these Amazon packages" to driving cutting-edge research such as "find the optimal way to fold this protein". 

Work has also been done on creating languaged-based logic units for reacting to critical circumstances in real-time, such as fall and stroke detection in an elder care setting.

{% include figure.html image="/assets/posts/2022-09-22/logic unit.png" caption="Sample layout of a natural language based logic unit" width="500" height="1000" %}