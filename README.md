# Deep Reinforcement Learning Nanodegree - Project 2: Continuous control via DDPG

## Introduction
This project is the second of the three assigned in the Udacity course I followed about Deep Reinforcement Learning.  

Aim of the project is to solve a typical continuos control (CC) problem - controlling a (virtual) double-jointed arm - by using a suitable deep reinforcement-learning (RL) technique ([DDPG](https://arxiv.org/abs/1509.02971)). 

## Continuous control and reinforcement learning
Controlling the arm means applying good sequences of torque values to the arm to make its hand reach a target location.
In the CC framework, such kind of problems is usally solved by exploiting a well hand-crafted dynamic model of the arm in order to push it through desired behavior, i.e. reaching the target location.
In the RL framwork, solving the problem is 'learning' the desired beahviour by 'trial and error': hand-crafted rewards and penalities (i.e. negative rewards) are provided during the task in order to distinguish good sequences and bad sequences of torque values.

Basically, an important difference between the two approaches is that RL is model-free: no prior-knowledge has to be embedded in some dynamic models of the controlled object if the desired behaviour can be easily expressed by rewards and penalities.

Let me try to include some rough parallels between CC and RL. Consider them as an easy attempt to introduce people familiar with CC to RL. Feel free to jump it :D

## A bit of nomenclature: 

In CC, the arm is 'the plant/model to be controlled', the control algorithm is 'the controller', the sequence of torque-values applied by controller is 'the control action', the mismatch between desired behaviour and current behaviour is 'the control error'.
In RL, the arm is 'the environment to be explored/exploited', the control algorithm is 'the agent (or the agent's policy)', the torque-values applied according to policy are 'actions' and the mismatch between the agent's desired behaviour and current beahviour is roughly expressed by 'the rewards' provided by the environment.
'Tuning' stands to controllers like 'training' stands to agents (the first can be done by humans or other ad-hoc techniques, the latter is core-part of the RL algorithm).
In both frameworks, 'states' is the minimum set of informations needed to 'summarize' the passed dynamics of model/environment (e.g. positions and velocities of the arm joints well represent the current state of the arm).
 
## The algorithm

The environment to be solved here is populated by 20 double-jointed arms and for each arm its controlling agent is fed with a positive reward (+0.1) for each step that the arm's  hand is in the target location (the goal of the agents is to maintain the hand position at the target location as long as possible).

Using multiple arms is particularly efficient when GPU is available and the training algorithm can be parallelized. Since DDPG is an off-policy method, experience can be indipendently gathered by the 20 arms/agents and then be periodically sampled to train a common model, which is eventually used to make arms/agents act. The model basically consists of two deep neural networks: one is trained to estimate the *best* action to pick in a certain state (it's an approximate *maximizer*), the other is trained to estimate the value (expected rewards) of the action suggested by the first one; in turn, such value is used to train the maximizer in estimating which the *best* action is. Two additional clones of the models (target models) slowly track the previous ones (the parameters are slightly shifted in those directions) and are exclusively employed in the training phase to improve convergence properties of the algorithm.

The state space for a single agent consists of 33 variables corresponding to position, rotation, velocity, and angular velocities of the arm. Each action is a 4 elements vector of torque values applicable to joints. Every entry in the action vector must be a number between -1 and 1.

## The solution
Once trained, the agent can be seen in action in the Unity environment.  
#<img width="451" alt="frame" #src="https://user-images.githubusercontent.com/53077127/112690757-a0374e00-8e7c-11eb-8c99-3f55124f14ad.png">  
#[Click here to take a #look!](https://user-images.githubusercontent.com/53077127/112689969-4d10cb80-8e7b-11eb-82ce-e0cc986b2736.mp4)

## Installation
Follow the instructions in the [DRLND GitHub repository](https://github.com/udacity/deep-reinforcement-learning#dependencies) to set up your Python environment. These instructions can be found in README.md at the root of the repository. You will install PyTorch, the ML-Agents toolkit, and a few more Python packages required to run the project.

Note: the project is provided with pre-built Unity environemnt for Windows 10 (64 bit). Other operating systems need custom environment.

## Try it for yourselft!
Open the Jupyter notebook: Continuous_Control.ipynb 
Run steps from 1 to 4 to train an agent.  
Run step 5 if you want to watch the agent play. If you don't train it, a pre-trained version is available.    
