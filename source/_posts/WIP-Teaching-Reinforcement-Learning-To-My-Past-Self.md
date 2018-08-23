---
title: '[WIP] Teaching Reinforcement Learning To My Past Self' 
date: 2018-08-23 15:44:15
tags:
---

*This post is an incomplete lession, written for past me - the me that existed less than a week ago:*
 
*I had experience working on both supervised and unsupervised learning problems using machine learning models (using the sklearn library), and some deep learning experience for supervised learning problems both in NLP and image labeling (using keras). I'd also messed around using deep learning for image manipulation (my favourite being turning personal photos into the images in the style of The Simpsons). And I had completed the first part of the fast.ai course and some of the second.*

*If you have roughly the same level of experience then this post is for you too. What follows is an explanation to my past self of how (Deep) Reinforcement Learning works.* 

My introduction to this topic came from a [Stanford University lecture](https://www.youtube.com/watch?v=lvoHnicueoE) posted on YouTube. I've been working on a write-up of the content covered (slide for slide) to give myself a reference for reading through at a slower pace and more time to grasp some of the functions and ideas, or to scan through in future when my memory of this topic gets hazy. You can find it in it's partially finished state [here](/pdf/stanford-lecture-14-reinforcement-learning.pdf).

#  What Is Reinforcement Learning?

First let's get the basic definition written down using some of the key lingo we'll be using for the rest of the explanation:

![The Reinforcement Learning Loop](/images/reinforcement/reinforcement-loop.gif "Weeee")

A reinforcement learning setup consists of an **agent** and an **environment**. The environment gives the agent a **state**. In turn, the agent is going to take an **action**, and then the environment is going to give back a **reward**, as well as the next state. This is going to keep going on in this loop, until the environment returns a **terminal state**, which then ends the **episode**. 

The goal is to learn *which series of actions will maximize its overall reward*. 

# Examples of Reinforcement Learning Problems
There are plenty of examples in which Reinforcement Learning is used, especially in robotics as machines learn to move, stay upright, and grasp things, etc. But my favourite is using it to learn to play Atari games.

[![Atari Breakout](/images/reinforcement/atari.gif)](https://www.youtube.com/watch?v=V1eYniJ0Rnk)


In this instance the definitions of our keywords are as follows:

- **Environment:** The game itself, i.e. the states and actions that can be performed within it
- **Agent:**	The player of the game who inputs the actions
- **State:**	The raw pixel inputs of the game state at a given point in time
- **Action:** 	Input controls - movement left or right at a given point in time
- **Reward:**	The score increase at a given point in time
- **Terminal state:** When the player either wins or dies
- **Episode:**	A full game, from start to completion

Here the goal is obviously to get the maximum score possible. Google's DeepMind have used reinforcement learning to solve this exact problem, you can view their agent playing with different levels of training in <a href="https://www.youtube.com/watch?v=V1eYniJ0Rnk" target="_blank">this video</a>.

It's pretty cool!

# Mathematically Formalising the Problem
To solve the problem we need to define the problem! The animated loop shown above can be written using a mathematical formulation known as the **Markov decision process** (MDP). 

An MDP satisfies the **Markov property**, which is that *the current state completely characterizes the state of the world*. Bear with me..
## MDP Object Definitions
An MDP is defined by tuple of objects, consisting of:
- $\mathcal{S}$: the *set of possible states*
- $\mathcal{A}$: the *set of possible actions*
- $\mathbb{R}$: the *distribution of possible rewards*, given a state-action pair, i.e. a function mapping from state-action $\mapsto$ reward
- $\mathbb{P}$: a *probability distribution* of the next state, given a state-action pair
- $\gamma$: a *discount factor*, used to signal *how much we value rewards coming up soon versus later on*

## MDP Process
- At time step $t=0$, the environment samples the initial state $s_0$ ~ $p(s_0)$
- For $t=0$ until done (terminal state):
    - Agent selects an action, $a_t$
    - Environment samples a reward, $r_t$ ~ $\mathbb{R}( . | s_t, a_t)$
    - Environment samples the next state, $s_{t+1}$ ~ $\mathbb{P}( . | s_t, a_t)$
    - Agent receives the reward, $r_t$, and next state, $s_{t+1}$

🚨 New lingo:
A **policy**, **$\pi$**, is a function that specifies what action to take in each state. 

So our objective can be redefined as:
*Find the **optimal policy**, **$\pi$&ast;**, that maximises the cumulative (discounted) reward.*

The term "discounted" is included here as we want to take into account our discount factor, $\gamma$, which balances how we value immediate vs. long term rewards.

This objective can be formulated as:
$$\sum\limits_{t \ge 0} \gamma^t r_t$$



