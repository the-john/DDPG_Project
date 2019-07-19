# My Deep Deterministic Policy Gradient Project for Continuous Control

In this environment, a double-jointed arm can move to target locations. A reward of +0.1 is provided for each step that the agent's hand is in the goal location. Thus, the goal of your agent is to maintain its position at the target location for as many time steps as possible.

The observation space consists of 33 variables corresponding to position, rotation, velocity, and angular velocities of the arm. Each action is a vector with four numbers, corresponding to torque applicable to two joints. Every entry in the action vector should be a number between -1 and 1.

There is a version of the Unity environment used here which contains 20 identical agents, each with its own copy of the environment.  Here we use multiple (non-interacting, parallel) copies of the same agent to distribute the task of gathering experience.

The barrier for solving this environment must take into account the presence of many agents. In particular, our agents must get an average score of +30 (over 100 consecutive episodes, and over all agents). Specifically,

- After each episode, we add up the rewards that each agent received (without discounting), to get a score for each agent. This yields 20 (potentially different) scores. We then take the average of these 20 scores.

- This yields an average score for each episode (where the average is over all 20 agents).

The environment is considered solved, when the average (over 100 episodes) of those average scores is at least +30.

I wasn't able to get the Unity ML-Agent environment to work properly on my computer, so I had to resort to using the Udacity Workspace.  Just download everything in this repository to a Jupyter Notebook folder and run the DDPG_Control.ipynb file (here)[].  Details on how I trained the agent are included in this file.
