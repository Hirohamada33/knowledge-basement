2Tabular method fails at solving real world problem when search tree is large, "t**he curse of dimensionality**", or even in continuous space. 

**Tabular method**:
- Every state has an entry $V(s)$
- Or every state-action pair $s, a$ has an entry $Q(s, a)$
**Problem with large MDPs**:
- Too many states, memory may be insufficient 
- Too slow to learn the value of each state individually 

This has bring the motivation to the solution of solving large / continuous-space MDPs, **value function approximation**. 

$\hat{v}(s, \textbf{w}) \approx v_\pi(s)$ or $\hat{q}(s, a, \textbf{w}) \approx q_\pi(s,a)$ 
The idea is to have a small / compact set of **weights** ($w$) to represent the value function to replace the need for a large look-up table. 

![[截圖 2024-10-21 下午9.20.33.png]]

There are many function approximators, including: 
- Linear combinations of features (differentiable)
- Neural network (differentiable)
- Decision tree
- Nearest neighbour 
- Fourier / wavelet bases
Furthermore, we require a training method that is suitable for **non-stationary**, **non-iid** data.

##### Gradient descent (Incremental method) 
Let $J(w)$ to be a differentiable function of parameter vector $w$. Define the gradient of $J(w)$ to be: $$\nabla_wJ(w) = (\frac{\partial J(w)}{\partial w_1}, \dots, \frac{\partial J(w)}{\partial w_n})$$To find a local minimum of $J(w)$, adjust the weight $w$ in direction of negative gradient $$\Delta w=-\frac{1}{2}\alpha\nabla_wJ(w)$$ (where $\alpha$ is the learning rate)

###### Mean-squared error
Now the goal is to find the parameter vector $w$ to minimise the mean-squared error. between approximate value function $\hat{v}(s,w)$ and true value function $v_\pi(s)$ 
$$J(w) = E_\pi[(v_\pi(S) − \hat{v}(S, w))^2]$$
Gradient descent finds a local minimum $$∆w = −\frac{1}{2}\alpha\nabla wJ(w)  
= αE_π [(v_π(S) − \hat{v}(S, w))∇_w\hat{v}(S, w)]$$
Stochastic gradient descent samples the gradient $$∆w = \alpha(v_π(S) − \hat{v}(S, w))\nabla _w\hat{v}(S, w)$$
And expected update is equal to full gradient update. 

###### Feature vectors 
Now introduce the **feature vector** to be some sort of the state representation (such as distance of robot from landmarks, trends in the stock market, piece and pawn configurations in chess). $$x(s) = (x_1(s), x_2(s), \dots, x_n(s))$$
###### Update rule
Here we discuss the linear combinations of features. $$\hat{v}(S, w) = x(S)^⊤w = \sum_{j=1}^{n}x_j(S)w(j)$$One advantages of using linear combination is that the geometry of the least-square errors will be a bowl, i.e., the global optima exists and guaranteed for the convergence (no local optima). $$J(w) = E_π[(v_π(S) − x(S)^T w)^2]$$The update rule is simply $$∇ _w \hat{v} ( S , w ) = x ( S )$$ So *update = step-size × prediction error × feature value*
$$∆w = α(v_π(S) − \hat{v}(S, w))x(S)$$

###### Table look-up as special case
Here, table look-up is a special case of value function approximation (discretised). $$x^{table}(S) = (1_{(S=s_1)}, 1_{(S=s_2)}, \dots, 1_{(S=s_n)})$$ where parameter vector $w$ gives value of each individual state $$\hat{v}(S, w) =(1_{(S=s_1)}, 1_{(S=s_2)}, \dots, 1_{(S=s_n)}) \space\cdot\space (w_1, w_2, \dots, w_n) $$ 
###### Algorithm 
The true value $v_\pi(S, w)$ in fact does not exist. For the nature of RL, it does not have a supervisor, only rewards. Hence from the previous methods, the **target** is replacing $v_\pi(S, w)$. 
- **Monte-Carlo**: $∆w=α(Gt −\hat{v}(S_t,w))∇_w\hat{v}(S_t,w) = ∆w=α(Gt −\hat{v}(S_t,w))x(S)$
  The return $G_t$ is an unbiased, noisy sample of true value $v_π(S_t)$. Monte-Carlo evaluation converges to a local optimum, even when using non-linear value function approximation. 
- **TD(0)**: $∆w=α(R_{t+1} +γ\hat{v}(S_{t+1},w)−\hat{v}(S_t,w))∇_w\hat{v}(S_t,w) =αδx(S)$
  The TD-target $R_{t+1} + γ\hat{v}(S_{t+1}, w)$ is a biased sample of true value $v_π(S_t)$. But we can still apply supervised learning to “training data”:  $⟨S_1, R_2 + γ\hat{v}(S_2, w)⟩, ⟨S_2, R_3 + γ\hat{v}(S_3, w)⟩, ..., ⟨S_{T −1}, R_T ⟩$ 
  Linear TD(0) converges (close) to global optimum. 
- **TD(λ)**: $∆w=α(G_t^λ −\hat{v}(S_t,w))∇_w\hat{v}(S_t,w) =α(G_t^λ −\hat{v}(S_t,w))x(S)$
  The $λ$-return $G_tλ$ is also a biased sample of true value $v_π(s)$. Can again apply supervised learning to “training data”: $⟨S_1, G_1^\lambda⟩, ⟨S_2, G_2^\lambda⟩, \dots, ⟨S_{T-1}, G_{T-1}^\lambda⟩$. 
  Forward view linear TD(λ) is given by: $∆w=α(G_tλ −\hat{v}(S_t,w))∇_w\hat{v}(S_t,w) = α(G_tλ − \hat{v}(S_t , w))x(S_t)$ 
  Backward view linear TD(λ) is given by: 
  $δ_t =R_{t+1}+γ\hat{v}(S_{t+1},w)−\hat{v}(S_t,w)$ 
  $E_t =γλE_t−1+x(S_t)$
  $∆w = αδ_tE_t$
  Forward view and backward view linear TD(λ) are equivalent. 

###### Control
![[截圖 2024-10-23 上午10.54.33.png]]
Policy evaluation: **Approximate** policy evaluation, $\hat{q}(S, A, w) ≈ q_π(S, A)$
Policy improvement: ε-greedy policy improvement

The goal is to minimise mean-squared error between approximate action-value function $\hat{q}(S,A,w)$ and true action-value function $q_π(S,A)$, given 
$$J(w) = E_π(q_π(S, A) − \hat{q}(S, A, w))^2$$
And use stochastic gradient descent to find a local minimum:
$$−\frac{1}{2} ∇w J (w) = (q_π (S , A) − \hat{q}(S , A, w))∇_w\hat{q}(S , A, w) ∆w$$
$$= α(q_π(S, A) − \hat{q}(S, A, w))∇_w\hat{q}(S, A, w)$$
#### Batch method
Gradient descent is simple and appealing, however it also has a downside - not sample efficient. Batch methods seek to find the best fitting value function Given the agent’s experience (“training data”). 
Some key concepts:
- **Storing past experiences**: The agent stores transitions of state-action-reward-next state $(s_t, a_t, r_t, s_{t+1})$ in a memory buffer called a **replay buffer** as it interacts with the environment.
- **Random sampling**: Instead of updating the model **immediately** after each transition, the agent samples **a mini-batch of transitions** randomly from the replay buffer and updates the value function or policy based on this batch. This **breaks the temporal correlation** between consecutive transitions.
- **Reusing data**: Experience replay allows the agent to reuse past transitions multiple times, improving data efficiency, which is crucial in environments with high-dimensional state spaces or where data is expensive to gather.

This has the following advantages: 
- **Reduces correlation between samples**: Random sampling makes each update more independent and stable by breaking the dependency between consecutive transitions.
- **Improves data efficiency**: The agent can reuse stored experiences multiple times, making the learning process more efficient.
- **Faster learning**: Experience replay allows the agent to learn from past mistakes, accelerating policy improvements.
###### Experience replay
**Experience Replay** is a technique in reinforcement learning designed to improve learning efficiency and stability, especially when using deep neural networks for value function approximation (e.g., in Deep Q-Learning, DQN).


Given experience consisting of $⟨\text{state}, \text{value}⟩$ pairs $D = {⟨s_1, v_1^π⟩, ⟨s_2, v_2^π⟩, ..., ⟨s_T , v_T^π ⟩}$. The process is a repetition of the following steps:
1. Sample state, value from experience: $⟨s,v_π⟩ ∼ D$
2. Apply stochastic gradient descent update: $∆w=α(v_π −\hat{v}(s,w))∇_w\hat{v}(s,w)$ 
It will converge to least squares solution: $w_π = \arg\min_w LS(w)$

The Least Squares solution is given by: $$LS(w) = \sum_{t=1}^T(v_t^π −\hat{v}(s_t,w))^2$$
The key question is, which parameters $w$ give the best fitting value function $\hat{v}(s,w)$? The $LS$ algorithms find parameter vector $w$ minimising sum-squared error between $\hat{v}(s_t,w)$ and target values $v_t^π$

##### Application in DQN 
DQN uses experience replay and fixed Q-targets. 
1. Take action at according to ε-greedy policy  
2. Store transition $(s_t,a_t,r_{t+1},s_{t+1})$ in replay memory $D$ 
3. Sample random mini-batch of transitions $(s,a,r,s')$ from $D$
4. Compute Q-learning targets w.r.t. old, fixed parameters $w^−$
5. Optimise MSE between Q-network and Q-learning targets
$$L_i(w_i)=E_{s,a,r,s'∼D_i} [(r+γ \max_{a'} Q(s',a';w_i^−)−Q(s,a;w_i))^2]$$

In Atari game, the CNN architecture is shown as below: 
- End-to-end learning of values $Q(s,a)$ from pixels $s$ 
- Input state $s$ is stack of raw pixels from last 4 frames 
- Output is $Q(s,a)$ for 18 joystick/button positions
- Reward is change in score for that step

![[截圖 2024-10-25 下午5.06.45.png]]

The effect of using experience replay and fixed Q-targets in several Atari game has been shown below: 
![[截圖 2024-10-25 下午5.08.10.png]]

##### Linear Least Squares Prediction
Instead of going through some iterations using experience replay method, a linear least squares prediction can directly find the solution. 
Given the optimisation state has $$α\sum_{t=1}^{T} x(s_t)(v_t^π −x(s_t)^Tw)=0$$
Then  $$\sum_{t=1}^{T} x(s_t)v_t^π = \sum_{t=1}^{T}x(s_t)x(s_t)^Tw=0$$
Hence the optimised solution can be found: $$w=(\sum_{t=1}^{T}x(s_t)x(s_t)^T)^{-1} \sum_{t=1}^{T} x(s_t)v_t^π$$
For $N$ features, direct solution time is $O (N^3)$. Incremental solution time is $O(N^2)$ using Shermann-Morrison. 
The reason this method is feasible is due to the reduction of space from large tabular form to a compact feature state representation. 
As discussed before, the "oracle" $v_t^π$ can be replaced by:
- **LSMC** (Least Squares Monte-Carlo) uses return: $v_t^π ≈ G_t$
- **LSTD** (Least Squares Temporal-Difference) uses TD target $v_t^π ≈ R_{t+1}+γ\hat{v}(S_{t+1},w)$
- **LSTD(λ**) (Least Squares TD(λ)) uses λ-return $v_t^π ≈ G_t^λ$


###### Least Squares Policy Iteration
![[截圖 2024-10-31 下午6.10.23.png]]
Again the GPI is used. The policy evaluation is given by least squares Q-learning and improvement is by greedy policy improvement. 

State-action pair is approximated by $\hat{q}(s, a, w) = x(s, a)^⊤w ≈ q_π(s, a)$ again from experience generated by $D = {⟨(s_1, a_1), v_1^π⟩, ⟨(s_2, a_2), v_2^π⟩, ..., ⟨(s_T , a_T), v_T^π ⟩}$. 

For policy evaluation, we want to efficiently use all experience. For control, we also want to improve the policy. This experience is generated from many policies, so to evaluate $q_π(S,A)$ we must learn **off-policy** 
1. Use experience generated by old policy $S_t,A_t,R_{t+1},S_{t+1} ∼ π_\text{old}$  
2. Consider alternative successor action $A' = π_\text{new} (S_{t+1})$
3. Update $\hat{q}(S_t , A_t , w)$ towards value of alternative action $R_{t+1} + γ\hat{q}(S_{t+1}, A', w))$ 

Using the linear Q-value update:
$$δ = R_{t+1} + γ\hat{q}(S_{t+1}, π(S_{t+1}), w) − \hat{q}(S_t , A_t , w)$$
$$∆w = αδx(S_t,A_t)$$
LSTDQ algorithm: (solve for total update = zero)
$$0 = \sum_{t=1}^T α(R_{t+1} + γ\hat{q}(S_{t+1}, π(S_{t+1}, w) − \hat{q}(S_t , A_t , w))x(S_t , A_t )$$
Hence we have $$w = (\sum_{t=1}^{T} x(S_t , A_t )(x(S_t , A_t ) − γx(S_{t+1}, π(S_{t+1})))^⊤)^{-1} \sum_{t=1}^T x(S_t , A_t )R_{t+1}$$


The process is described as below. 
![[截圖 2024-10-31 下午6.18.24.png]]

##### Convergence guarantees
![[截圖 2024-10-25 下午5.20.07.png]]

For 

##### Performance
![[截圖 2024-10-31 下午6.05.02.png]]
Consider the above scenario "Chain walk". Reward +1 in states *10* and *41*, 0 elsewhere. Ideal situation the agent should yields the optimal policy: R(1-9), L(10-25), R(26-41), L(42, 50). 
Features: 10 evenly spaced Gaussian ($\sigma = 4$) for each action. 
Experience: 10,000 steps from random walk policy. 

![[截圖 2024-10-31 下午6.08.34.png]]
After 7 iterations the agent was able to approximate the true value function. 

##### Questions 

Q1: for the SGD update, we are updating essentially to the direction of feature value, whether it is "active" or not. However, essentially by doing so we are dictating the rule, actively telling the model to move to a direction before it finds the optimal goal. Isn't that going to be (1) violates the rule for setting up suboptimal goals in learning and (2) RL became supervised and the agent learning is affected by the human decision of "what's a good feature"? 

A: 
1. **Does this violate the rule of setting suboptimal goals?**: In reinforcement learning, the goal is for the agent to find a policy that maximises long-term rewards, rather than setting suboptimal or local goals. When using stochastic gradient descent (SGD), the feature vector update is in the direction of the observed reward. While this might seem like forcing the model to move in a certain direction, it's actually based on the agent's current interaction with the environment. The update is not a pre-designed target but rather the result of what the agent discovers while exploring and exploiting the environment.
   This doesn't violate the rule of setting suboptimal goals because the feature vector updates are still based on the agent-environment interaction. Suboptimal goals naturally emerge during the RL process, and the agent can eventually improve its policy through exploration. The role of SGD is to help the agent fine-tune its policy based on its observed state and the feedback it receives from its actions.

2. **Does RL become supervised learning?**: While feature selection can indeed influence the model's performance, this doesn't necessarily mean that RL turns into supervised learning. In supervised learning, we rely on human-labeled "good features," but in RL, features describe the state of the environment rather than labeled input-output pairs. The agent still needs to learn independently how to act to maximise rewards.
   In other words, feature selection in RL can influence the learning process, but the agent isn't told what constitutes a "good" feature or what the optimal action is. The agent must still explore and learn a policy on its own. So, RL doesn't become supervised learning, though feature design might guide or constrain the learning, the agent ultimately explores independently.

Q: Why is TD biased? 