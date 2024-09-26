##### Markov process 
Markov property has a definition: 
"The future is independent of the past given"
Mathematically it is expressed as
$P[S_{t+1} \space|\space S_t] = P[S_{t+1}\space |\space S_1, S_2, \dots, S_t]$ 

###### State transition matrix
A state transition matrix $P$ defines transition probabilities from all
states s to all successor states $s'$ 
![[截圖 2024-08-15 下午5.11.30.png]]
where each row of the matrix sums to 1. 
The state transition probability is defined by
$$P_{ss'} = P[ S_{t+1} = s' \space |\space S_t = s]$$

A Markov process is a **memoryless** random process, i.e. a sequence of random states S1, S2, ... with the Markov property.
A Markov Process (or Markov Chain) is a tuple $⟨S,P⟩$
- $S$ is a (finite) set of states
- $P$ is a state transition probability matrix, $P{ss'} =P[S_{t+1}=s'\space|\space S_t=s]$ 

An example of a Markov process diagram and state transition probability

![[截圖 2024-08-15 下午5.19.20.png]]


##### Markov reward process
A Markov reward process is a Markov chain with values. 
A Markov Reward Process is a tuple $⟨S, P, R, γ⟩$
- $S$ is a finite set of states
- $P$ is a state transition probability matrix, $P_{ss'} =P[S_{t+1}=s'\space|\space S_t=s]$
- $R$ is a reward function, $Rs = E[R_{t+1} | S_t = s]$
- $γ$ is a discount factor, $γ ∈ [0, 1]$ 

###### Return
A return $G_t$ is the total discounted reward from time-step $t$.
$$G_t = R_{t+1} +\gamma R_{t+2} + \dots =\sum_{k=0}^\infty \gamma^kR_{t+k+1}$$
- The discount $γ ∈ [0, 1]$ is the present value of future rewards. $γ$ close to 0 leads to ”myopic” evaluation, $γ$ close to 1 leads to ”far-sighted” evaluation
- The value of receiving reward $R$ after $k + 1$ time-steps is $γ^k R$ . 
- This values immediate reward above delayed reward.

There are several intuition for using a discounted factor. Including: 
- Mathematically convenient to discount rewards
- Avoids infinite returns in cyclic Markov processes
- Uncertainty about the future may not be fully represented
- If the reward is financial, immediate rewards may earn more interest than delayed rewards
- Animal/human behaviour shows preference for immediate reward
- It is sometimes possible to use undiscounted Markov reward processes (i.e. $γ = 1$), e.g. if all sequences terminate.

###### Value function
The state value function $v(s)$ of an MRP is the expected return starting from state $s$. It gives the long-term value of state $s$.
$$v(s) = E[G_t \space|\space S_t = s]$$

###### Bellman equation
The value function can be decomposed into two parts: 
1. immediate reward $R_{t+1}$
2. discounted value of successor state $γv(S_{t+1})$ 
![[截圖 2024-08-15 下午5.45.07.png]]

The Bellman equation is expressed as
$$v(s)=E[R_{t+1}+γv(S_{t+1})|S_t =s]$$
or 
$$v(s)=R_s +γ\sum_{s' \in S}P_{ss'}v(s')$$

![[截圖 2024-08-15 下午7.10.32.png]]

![[截圖 2024-08-15 下午7.10.49.png]]

In matrix form, the Bellman equation is described as  
![[截圖 2024-08-15 下午9.07.26.png]]

To solve the Bellman equation, it can be mathematically solved by
![[截圖 2024-08-15 下午9.07.14.png]]
with the computation complexity of $O(n^3)$, hence the direct solutions are only possible for small MRPs. 
For larger MRPs, there are many iterative methods including **Dynamic programming**, **Monte-Carlo evaluation** and **Temporal-Difference learning**. 

##### Markov decision process
A Markov decision process (MDP) is a Markov **reward process with decisions**. It is an environment in which all states are Markov. 
A Markov Decision Process is a tuple $⟨S, A, P, R, γ⟩$ 
- $S$ is a finite set of states  
- $A$ is a finite set of actions 
- $P$ is a state transition probability matrix, $P^{a}_{ss'} =P[S_{t+1}=s'|S_t=s,A_t=a]$
- $R$ is a reward function, $R^a_s = E[R_{t+1}\space|\space S_t = s,A_t = a]$ 
- $γ$ is a discount factor $γ ∈ [0, 1]$

![[截圖 2024-08-16 下午12.16.00.png]]

###### Policy
A policy $π$ is a distribution over actions given states, $$π(a|s)=P[A_t =a|S_t =s]$$
A policy fully defines the behaviour of an agent. 
For a MDP, the policies depend on the current state (not the history), i.e. Policies are stationary (time-independent), $A_t ∼π(·|St),∀t>0$ 
Given an MDP $M = ⟨S,A,P,R,γ⟩$ and a policy $π$,  the state sequence S1, S2, ... is a Markov process $⟨S, P^π⟩$, and the state and reward sequence $S1, R2, S2, ...$ is a Markov reward process $⟨S, P^π, R^π, γ⟩$, where
$$P^π_{ss'} =\sum_{a \in A}π(a|s)P^a_{ss'}$$$$ R_{s}^{\pi}=\sum_{a\in A}\pi(a|s)R^a_s$$
###### Value function
The state-value function $v_π(s)$ of an MDP is the expected return starting from state $s$, and then following policy $π$ $$v_π(s)=E_π[G_t |S_t =s]$$The action-value function $q_π(s,a$) is the expected return starting from state $s$, taking action $a$, and then following policy $π$. $$q_π(s,a)=E_π[G_t |S_t =s,A_t =a]$$ 
###### Bellman expectation equation
The state-value function can again be decomposed into immediate reward plus discounted value of successor state 
$$v_π(s) = E_π [R_{t+1} + γv_π(S_{t+1}) | S_t = s]$$
The action-value function can similarly be decomposed, $$q_π(s,a)=E_π[R_{t+1}+γq_π(S_{t+1},A_{t+1})|S_t =s,A_t =a]$$ 