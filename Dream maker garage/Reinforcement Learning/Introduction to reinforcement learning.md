
![[截圖 2024-08-15 下午1.34.17.png]]

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

There are some scenarios for reinforcement learning application:
- Fly stunt manoeuvres in a helicopter
- Defeat the world champion at Backgammon Manage an investment portfolio
- Control a power station
- Make a humanoid robot walk
- Play many different Atari games better than humans

##### Rewards
A reward $R_t$ is a scalar feedback signal, it indicates how well agent is doing at step $t$. The agent’s job is to maximise cumulative reward. 
A very important assumption (Reward Hypothesis) is that:
All goals can be described by the maximisation of expected cumulative reward. 



##### Agent
At each step t the agent: 
1. Executes action $A_t$
2. Receives observation $O_t$ 
3. Receives scalar reward $R_t$

The environment: 
1. Receives action $A_t$
2. Emits observation $O_{t+1}$ 
3. Emits scalar reward $R_{t+1}$

$t$ increments at env. step


##### Markov assumption



##### Policy
Policy determines how the agent chooses action, mapping from states to action $\pi: S \rightarrow A$ 

**Deterministic policy**:$$\pi(s) = a$$
**Stochastic policy**: $$\pi(a|s) = Pr(a_t = a|s_t = s)$$
