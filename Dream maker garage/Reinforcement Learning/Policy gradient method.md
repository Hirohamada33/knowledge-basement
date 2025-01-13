Policy Gradient (PG) method is a technique for directly optimising a policy function to maximise accumulated rewards. The core idea is to estimate the "probability distribution of actions" rather than estimating "action values", enabling the agent to learn a policy that selects optimal actions based on the state of the environment.
A key feature of the policy gradient method is that it directly updates policy parameters, avoiding the exploration-exploitation dilemma found in value-based methods, as policy gradients directly learn a definite behavior distribution and do not depend on ϵ\epsilonϵ-greedy or softmax policies for action selection.


![[截圖 2024-10-31 下午6.27.38.png]]
The diagram illustrates the comparison between value-based and policy-based RL.

**Value function**:
- Learnt value function
- Implicit policy (e.g. ε-greedy)
**Policy function**:
- No Value Function
- Learnt policy
**Actor critic**:
- Learnt value function
- Learnt policy

**Differences between policy gradient method and typical model-free methods**:
1. **Policy-Based Learning vs. Value-Based Learning**:
- Policy gradient methods directly learn the probability distribution of behaviors instead of relying on learning the "value" of those behaviors. This makes policy gradients suitable for continuous or high-dimensional action spaces.
- Traditional model-free methods, like Q-learning or SARSA, rely on learning an action-value function $Q(s,a)$ to estimate the reward of each action in a particular state, then choose actions based on these values. These methods are stable but can struggle with high-dimensional problems.
2. **Continuous and Discrete Action Spaces**:    
- Policy gradient methods naturally handle continuous action spaces, as the behavior distribution can be represented by parameterised functions (like Gaussian distributions).
- In contrast, typical model-free methods usually require discrete action spaces for effective learning. Continuous action spaces can be handled by binning actions, but this is often computationally intensive and less effective.
3. **Exploration-Exploitation Tradeoff**:
- Policy gradient methods automatically balance exploration and exploitation by directly using a behaviour distribution during the learning process.
- Typical model-free methods generally need manually set exploration parameters (e.g., ϵ\epsilonϵ-greedy), which may require tuning across different scenarios.
4. **Objective**:
- The objective of policy gradient is to maximise the expected return $J(θ)$, updating policy parameters directly along the gradient of the objective function.
- The goal of typical model-free methods is to learn an action-value function $Q(s,a)$that gradually converges to optimal values, from which the best action is chosen.

Some discussion about using the policy-based RL:
**Advantages**:  
- Better convergence properties  
- Effective in high-dimensional or continuous action spaces 
- Can learn stochastic policies

**Disadvantages**:  
- Typically converge to a local rather than global optimum 
- Evaluating a policy is typically inefficient and high variance


Some discussion about why deterministic policy sometimes isn't a good idea:
1. **Stochasticity is better then deterministic state** in game nature: 
   Consider policies for iterated rock-paper-scissors. 
   A deterministic policy is easily exploited; a uniform random policy is optimal (i.e. Nash equilibrium). 
2. **Aliased state**:
   ![[截圖 2024-10-31 下午6.36.40.png]]
   Consider the above grid world, where the feature is represented by $$φ(s,a) = 1(\text{wall to N},a = \text{move E})$$
Under aliasing, an optimal deterministic policy will either move W or E in both grey states, it can get stuck and never reach the money. 
Value-based RL learns a near-deterministic policy (e.g. greedy or ε-greedy), so it will traverse the corridor for a long time. 
An optimal stochastic policy will randomly move E or W in grey states (probability = 0.5), where it will reach the goal state in a few steps with high probability. 
Policy-based RL can learn the optimal stochastic policy. 

