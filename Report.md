# Deep Deterministic Policy Gradient Project for Continuous Control

## Repository Files
The heart of the file structure on this repository starts with the Jupyter Notebook Continuous_DDPG.ipynb file [here]( https://github.com/the-john/DDPG_Project/blob/master/Continuous_DDPG.ipynb).  This file drives both the training and the running of this project.  The model.py file, [here]( https://github.com/the-john/DDPG_Project/blob/master/model.py), defines both the Actor and the Critic model architectures, while the ddpg_agent.py file, [here]( https://github.com/the-john/DDPG_Project/blob/master/ddpg_agent.py), not only defines the details of how the Actor and Critic networks are setup, but other necessary functions like soft updates, the creation of noise, and the structure of the replay buffer.  After training, the weights where saved in the checkpoint_actor.pth file, [here]( https://github.com/the-john/DDPG_Project/blob/master/checkpoint_actor.pth), for the Actor, and in the checkpoint_critic.pth file , [here]( https://github.com/the-john/DDPG_Project/blob/master/checkpoint_critic.pth), for the Critic.  There is a workspace_utils.py file, [here]( https://github.com/the-john/DDPG_Project/blob/master/workspace_utils.py) in there that enables you to keep running the training session in the Udacity Workspace without it timing out.

## Implementation Description
In this environment, a double-jointed arm can move to target locations. A reward of +0.1 is provided for each step that the agent's hand is in the goal location. Thus, the goal of your agent is to maintain its position at the target location for as many time steps as possible.

The observation space consists of 33 variables corresponding to position, rotation, velocity, and angular velocities of the arm. Each action is a vector with four numbers, corresponding to torque applicable to two joints. Every entry in the action vector should be a number between -1 and 1.

There is a version of the Unity environment used here which contains 20 identical agents, each with its own copy of the environment.  Here we use multiple (non-interacting, parallel) copies of the same agent to distribute the task of gathering experience.

The barrier for solving this environment must take into account the presence of many agents. In particular, our agents must get an average score of +30 (over 100 consecutive episodes, and over all agents). Specifically,
- After each episode, we add up the rewards that each agent received (without discounting), to get a score for each agent. This yields 20 (potentially different) scores. We then take the average of these 20 scores.
- This yields an average score for each episode (where the average is over all 20 agents).

The environment is considered solved, when the average (over 100 episodes) of those average scores is at least +30.

I wasn't able to get the Unity ML-Agent environment to work properly on my computer, so I had to resort to using the Udacity Workspace.  Just download everything in this repository to a Jupyter Notebook folder and run the Continuous_DDPG.ipynb file [here]( https://github.com/the-john/DDPG_Project/blob/master/Continuous_DDPG.ipynb).  Details on how I trained the agent are included in this file.

## Learning Algorithm
For this project I chose the Deep Deterministic Policy Gradient (DDPG) methodology to teach the double-jointed robot arm how to behave.  DDPG was chosen because if its proven history in solving continuous action spaces.

DDPG uses two Deep Neural Networks, one called the Actor and the other the Critic.  The Actor is used to approximate the optimal policy deterministically (unlike a stochastic policy).  We want the believed BEST action every time we query the Actor network (which is a deterministic policy).  The Actor is learning the best Action.  The Critic learns to evaluate the optimum Action-Value function by using the Actor’s best believed action.  We use the Actor, which is an approximate Maximizer, to calculate the new target value for training the Action-Value function.  They refer to this as the “Actor-Critic Method”.

A DDPG network uses a Replay Buffer and two copies of the network weights:
-	A Regular for the Actor and another Regular for the Critic
-	A Target for the Actor and another Target for the Critic

A Soft Update strategy is used to update the Target networks because it provides faster convergence.

The Soft Update strategy slowly blends your Regular network weights with the Target network weights.  So at every time-step your Target network weights are 99.99% the Target network weights and only 0.01% the Regular network weights.  You slowly mix in your Regular network weights into your Target network weights.  The Regular network is the most up to date network because it is the one we are training.  The Target network is the one we use for prediction to stabilize training.

For both the Actor and the Critic I made a model architecture that consists of a Fully Connected Layer, followed by some batch normalization, and finished up with two more Fully Connected Layers.  For the Actor I do a ReLU function on the first and second Fully Connected Layers, and use a tanh function on the last Fully Connected Layer.  For the Critic I do a ReLU function on first Fully Connected Layer, I then do a concatenation, then another ReLU function on the second Fully Connected Layer.

Some of the key hyperparameters used are the Tau for the soft update (which controls the mixture for the Target updates), the size of the replay buffer, the batch size, a discount factor, the learning rate for both the actor and the critic, a couple of noise parameters, explore/exploit noise process called epsilon, and a decay rate for epsilon.  I used all the initial parameter values that I pulled from the training materials and the first project for this nano degree.

## Rewards Per Episode Plot
It took 108 episodes before I was able to get an average score of 30.  I went ahead and pushed the learning out to an average score of 32 before I saved my Actor and Critic weights, just to ensure that in the future I’m able to get a 30-point average.

**(Note; for the life of me I don’t know why my “DDPG Score” label repeated itself so much.  I ended up loosing the scores and would have to spend another 6 hours training again to recreate the data … so I’m using the graph below instead.)**

![](https://github.com/the-john/DDPG_Project/blob/master/P2_Plot.png)
 
## Ideas for the Future
I think that I’d like to try something like the Trusted Region Policy Optimization (TRPO).  Or I could get really ambitious and actually dig into the Distributed Distributional Deterministic Policy Gradient (D4PG) and give that a go.
