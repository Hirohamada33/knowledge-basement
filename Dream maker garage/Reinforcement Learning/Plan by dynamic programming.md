The term **dynamic programming** have the following definition:
**Dynamic**: sequential or temporal component to the problem
**Programming**: optimising a "program", i.e. a policy 
Refer to [[Dynamic programming]] for more. 

Dynamic programming is a very general solution method for problems which have two properties:
1. **Optimal substructure**
   - Principle of optimality applies
   - Optimal solution can be decomposed into subproblems
2. **Overlapping subproblem**
   - Subproblems recur many times
   - Solutions can be cached and reused
3. Markov decision processes satisfy both properties 
   - Bellman equation gives recursive decomposition
   - Value function stores and reuses solutions (**cache** is the one central idea in reinforcement learning)

The occasion where dynamic programming is used, is in
1. **Prediction**:
   Given an input $\text{MDP}(S, A, P, R, \gamma)$ or $\text{MRP}(S, P^\pi, R^\pi, \gamma)$, it generates the value function $v_\pi$ 
2. **Control**:
   Given an input $\text{MDP}(S, A, P, R, \gamma)$, outputs the optimal value function $v_\pi$ 

##### Policy evaluation
The context is to evaluate a given policy $\pi$. Solution is iterative application of Bellman expectation backup.
The process of **synchronous backup**, is
- At each iteration $k + 1$ 
- For all states $s \in S'$ 
- Update $v_{k+1}(s)$ from $v_k(s)$ where $s'$ is a successor state of s
![[截圖 2024-07-09 上午6.50.45.png]]
Every state has a go of being the leaf, which is why it is called *synchronous*. The *backup* refers the one-step look ahead in this example. 


##### Policy iteration
Given a policy π:
1. Evaluate the policy $π$
   $v_π(s)=E[Rt+1 +γRt+2 +...|St =s]$ 
2. Improve the policy by acting greedily with respect to $v_π$
   $π' = \text{greedy}(v_π)$
   In many iterations, the policy will converges to optimal policy $\pi \rightarrow \pi_*$

The **policy dance** involves with two key element: **policy evaluation** and **policy improvement**. ![[policy dance.png]]

One example of the policy iteration in Jack's car rental:
![[Jack's rental car problem.png]]

###### Policy improvement
Consider a deterministic policy, $a = \pi(s)$, the new policy can be found by acting greedily $\pi' (s) = \arg \max_{a\in A} q_\pi(s, a)$. 
 This will improve the value from any state s over one step, $q_π(s, π'(s)) = max q_π(s, a) ≥ q_π(s, π(s)) = v_π(s)$
 which improves the value function $v_π'(s) ≥ v_π(s)$. 

The proof is done by
$v_π(s)≤q_π(s,π'(s))=E_{π'}[R_{t+1}+γv_π(S_{t+1})|S_t =s]$
$\space\space\space\space\space\space\space\space\space≤E_{π'}[R_{t+1}+γq_π(S_{t+1},π'(S_{t+1}))|S_t =s]$
$\space\space\space\space\space\space\space\space\space≤E_{π'}[ R_{t+1}+γR_{t+2}+γ^2q_π(S_{t+2},π'(S_{t+2}))|S_t =s]$
$\space\space\space\space\space\space\space\space\space≤E_{π'} [R_{t+1}+γR_{t+2}+...|S_t =s]=v_{π'}(s)$


If the improvements stopped, 
$q_π(s, π'(s)) = \max q_π(s, a) = q_π(s, π(s)) = v_π(s)$
Then the Bellman optimality equation has been satisfied
$v_π(s) = \max q_π(s, a), a∈A$
Therefore $v_π(s) = v_∗(s)$ for all $s ∈ S$ so $π$ is an optimal policy
##### Value iteration
Any optimal policy can be subdivided into two components:
- An optimal first action $A_∗$ 
- Followed by an optimal policy from successor state $S'$

The principle of optimality gives:
![[principle of optimality.png]]


For policy iteration, each value function update has a corresponding policy, there is an intermediate step of "building the policy" (greedification in small grid world). But in value iteration, there is no explicit policy. Intermediate value functions may not correspond to any policy. 


A final remark is the table of comparison

|  Problem   |                     Bellman equation                     |          Algorithm          |
| :--------: | :------------------------------------------------------: | :-------------------------: |
| Prediction |               Bellman Expectation Equation               | Iterative Policy Evaluation |
|  Control   | Bellman Expectation Equation + Greedy Policy Improvement |      Policy Iteration       |
|  Control   |               Bellman Optimaility Equation               |       Value Iteration       |

##### Extensions to dynamic programming
In-place
Real-time

##### Contraction mapping