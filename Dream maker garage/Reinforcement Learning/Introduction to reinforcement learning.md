There are four fundamental elements in an RL problem:
- **Optimisation**: 
  Goal is to find an optimal way to make decisions (i.e., yielding best outcomes or at least very good outcomes). Explicit notion of decision utility. 
- **Delayed consequences**: 
  Decisions that impact results later. This introduces two major challenge:
  - **When planning**: decisions involve reasoning about not just immediate benefit of a decision but also its longer term ramifications
  - **When learning**: temporal credit assignment is hard (what caused later high or low rewards?)
- **Exploration**:
  Learning about the world by making decisions
- **Generalisation**:
  Policy is mapping from past experience to action

RL problem is similar to other artificial problems. Below listed a comparison:

|                            | AI planning | Supervised | Unsupervised | Reinforcement | Imitation |
| :------------------------: | :---------: | :--------: | :----------: | :-----------: | :-------: |
|      **Optimisation**      |      X      |            |              |       X       |     X     |
| **Learns from experience** |             |     X      |      X       |       X       |     X     |
|     **Generalisation**     |      X      |     X      |      X       |       X       |     X     |
|  **Delayed Consequences**  |      X      |            |              |       X       |     X     |
|      **Exploration**       |             |            |              |       X       |           |

Two scenarios where RL is particular powerful:
1. No examples of **desired behavior** (e.g. because the goal is to go beyond human performance or there is no existing data for a task)
2. Enormous search or optimization problem with **delayed outcomes**


##### Markov assumption



##### Policy
Policy determines how the agent chooses action, mapping from states to action $\pi: S \rightarrow A$ 

**Deterministic policy**:$$\pi(s) = a$$
**Stochastic policy**: $$\pi(a|s) = Pr(a_t = a|s_t = s)$$
