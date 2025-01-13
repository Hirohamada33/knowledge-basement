When the model of reinforcement learning isn't given, model-free prediction is used to estimate the value function of an unknown MDP. 
A known MDP is where, the dynamics and the reward functions are all known henceforth the dynamic programming can directly be used to iteratively run through and update according to the optimal Bellman equation. 
On the contrary, model-free is when the agent have no knowledge of the environment dynamics, where transfer the information from directly the interaction with the environment to form a value function. 

##### Monte-Carlo learning
- Learn directly from **episodes of experience** (i.e., no need for knowledge of MDP transitions / rewards). 
- Learns from **complete episodes**: (i.e., no bootstrapping)
- The value function is determined by the simplest idea (value = mean return)
- Only works for **episodic MDPs** (i.e., must have **terminate state**)
- Uses **sampling** instead of the full sweeps (like in dynamic programming), hence break the dependence of running time to the size of the state space (as long as the interested states have been visited for a certain amount of time).  

###### First-visit Monte-Carlo
###### Every-visit Monte-Carlo


##### Temporal difference
##### TD


##### Question
1. Does MC has a dependency between states (so a good state following by a bad state, the good state is affected). 