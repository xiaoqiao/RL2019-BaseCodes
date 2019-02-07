# Task 4 - Asynchronous Q-Learning with Function Approximation in HFO

In this task, you are required to implement an attacking agent in the HFO domain that learns using the Asynchronous Q-Learning  algorithm([**Mnih et al., 2016**](https://arxiv.org/pdf/1602.01783.pdf)). Unlike previous tasks, we want you to design your own reward functions to train the agent. In general, choosing a reward function that enables easier learning for agents is a major part of deep reinforcement learning research. You are also given freedom to decide on the state representations used to train your agents. Just like rewards, designing appropriate state representation is a major part of deep reinforcement learning research.

At the end, we will require you to store the parameters of your agents every 1 million global timesteps. We would then test the performance of your agents based on these submitted parameters. The performance of the agent will be measured based on average time to goal in each episode. In the event where your agent unsuccessfully scores a goal in an episode, we define the time to goal at that episode as the maximum allowed number of timesteps for that episode.

The interface between agent and environment will be similar with previous tasks. You can check this inside `Environment.py`. Since asynchronous training requires running several environments at once, each of your learning process needs to initialize a single environment at the beginning. To start a new instance of an environment, initialize a HFO server using `startEnv()` and establish connections to the HF0 server using `connectToServer()`. In addition to this, don't forget to initialize each server with different connection ports because you might not initialize two different servers that use the same ports for connections with agents. Also, since you want each environment to present agents with different states, don't forget to initialize different environments with different seeds.

## Implemented Files (**Contains functions to be implemented**)
1. `Networks.py` 
   - Define the architecture of the **torch** neural network that you're using inside `__init__.py`
   - Define the forward computation used in your neural network inside `forward(inputs)`
2. `Environment.py` 
   - Implement your reward calculations inside `get_reward(status, nextState)`
   - Implement your state preprocessing function inside `preprocessState(state)`
3. `Worker.py`
   - Implement all the necessary computations required to train a single process of an agent inside `train()`
   - Implement the target value computation for Q-Learning inside `computeTargets(reward, nextObservation, discountFactor, done, targetNetwork)`. As you might see, this function should be usable for any target network architecture that you're using. **This function will be used in marking the correction of your implementation**.
   - Implement a single call for the forward computation of your Q-Network inside `computePrediction(state, action, valueNetwork)`. Note that this should be agnostic to any Q-Network architecture that you're using.
4. `main.py` 
   - This script should contain the necessary processes to run your asynchronous agent. We've provided an code snippet on how to asynchronously call multiple instances of the training function `train()` in `Worker.py` in the `__main__` function. 

## Additional Scripts (**Might be useful for training**)
1. `SharedAdam.py`
   -. We provide an example implementation of how to share pytorch optimizer statistics across multiple workers inside this function. In the original A3C paper, this reportedly produced better performance. You are free to use this file or even create another shared optimizer implementation of the optimizer you've chosen.

## Environment Files
1. `Environment.py`
   - File to establish connections with HFO, define rewards for agents(**to be implemented by you**)
 and preprocess state representations(**to be implemented by you**) gathered from the HFO domain. Also, you are allowed to change the state representation used by switching from `LOW_LEVEL_FEATURE_SET` to `HIGH_LEVEL_FEATURE_SET` in line 56 of `Environment.py`. Other functions inside `Environment.py` **should not be modified**.
2. `Goalkeeper.py` (**Should not be modified**)
   - File to control an NPC Goalkeeper in the environment. This NPC just runs around the goalposts throughout the episode.
   
## State Space
HFO provides two different possible state spaces, the `LOW_LEVEL_FEATURE_SET` and the `HIGH_LEVEL_FEATURE_SET`. You are free to use either one of them. Refer to the original HFO repository to see the features provided in each feature set. In our case, we are going to use noiseless observations of the feature space.

## Action Spaces
You are to use the discrete actions provided by the HFO domain. These actions include `SHOOT`, `DRIBBLE`, `MOVE`, and `GO_TO_BALL`. You can find the exact specification on what each action does on the original HFO repository. 

## Reward Functions
You are allowed to define your own reward functions for this task. Make sure that your reward function enables learning advantageous behaviour for agents to score goals.

## Setup and Requirements

The codes can be executed in your own DICE machines. However, you need to first download the necessary packages before running them. In this task, you are going to implement an agent in the Half Field Offense (HFO) domain. Full installation intructions and documentation of this environment can be seen [in this repository](https://github.com/LARG/HFO). Use the following commands to install the dependencies for HFO in your DICE machines:

```
conda create --name <Environment Name> numpy python=3.5
source activate <Environment Name>
conda install -c anaconda boost
conda install boost
conda install qt=4
```  

Finally, clone this repository into the `example` directory in the `HFO` folder. Also, don't forget to read the documentations of the original HFO repository. It would give additional understanding on what the feature spaces for this environment are.

