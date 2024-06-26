##### History 
The 1st industrial revolution was the physical power of machine substitute for people. 
The 2nd industrial revolution is the computational power of machine substitute for people. This includes perception, motor control, prediction, decision making, optimisation, search 

![[computation revolution.png]]

Some key progress in the history:
2011 - IBM's Watson beats best human players in Jeopardy
2012 - Deep neural networks greatly improve the state of the art in speech recognition and computer vision
2013 - Google's self-driving car becomes a plausible reality 
2014 - Deepmind's QDN learns to play Atari games at the human level, from pixels, with no game-specific knowledge
2015 - University of Alberta program solves Limit Poker
2016 ~ 2017  - Google Deepmind's AlphaGo defeats legendary Go player, vastly improving over all previous programs 
2017 - University of Alberta program also defeats professional players at No-limit Poker

**Atari games** was one of the development in RL, where the intelligent agent had no previous knowledge on the game. 
The RL and deep learning are applied to classic Atari games, it has learned to play 49 games for Atari 2600, without labels or human input, but purely from self-play and the score alone.

![[RL + deep network in Atari.png]]


##### Multi-arm bandits
The problem is the simplest reinforcement learning problem. Given a scenario that:
1. Action 1 - Reward is always 8 ($q_*(1) = 8$)
2. Action 2 - 88% chance of 0, 12% chance of 100 ($q_*(2) = 0.88 * 0 + 0.12 * 100 = 12$)
3. Action 3 - Randomly between -10 and 35, equiprobable ($q_*(3) = \frac{-10+35}{2} = 12.5$)
4. Action 4 - a third 0, a third 20 and a third from {8, 9, ..., 18} ($q_*(4) = \frac{1}{3}*0 + \frac{1}{3}*20 + \frac{1}{3}*\frac{8+18}{2} = 11.33$)

On each of infinite sequence of time steps, t = 1, 2, 3, ... there is an action $A_t$ from $k$ possibilities, and received a real-valued reward $R_t$. The reward depends only on the action taken, it is identically, independently distributed (i.i.d.): $$q_*(a) = E[R_t|A_t = a], \forall a \in {1, ..., k} $$
These true values are unknown, the distribution is unknown. Nevertheless, the total reward must be maximise. 

##### Exploration / Exploitation Dilemma 
For an action-value estimates $$Q_t(a) \approx q_a(a), \space \forall a$$The greedy action at time t is defined as $$A_t^* = argmax_aQ_t(a) $$
If $A_t = A_t^*$ , then the action is exploiting.
If $A_t \neq A_t^*$ , then the action is exploring. 

An action cannot be both exploiting and exploring, but it is required to have both. Some strategies is never stop exploring, but with time elapsed the exploration should be less (or not). 


##### $\epsilon$-greedy Action Selection
In greedy action selection, the action is always exploit. In $\epsilon$-greedy, the action is usually greedy, but with probability of $\epsilon$ that the action is instead picked at random (possibly the greedy action again). This is the simplest way to balance exploration and exploitation. 
![[simple bandit algorithm.png]]


##### Action-value methods 
Action-value method is a method that learn action-value estimates. For instance, a sample average action values gives:
$$Q_t(a) = \frac{\text{sum of rewards when a taken prior to t}}{\text{number of times a taken prior to t}} = \frac{\sum_{i=1}^{t-1}R_i * 1_{A_i = a}}{\sum_{i=1}^{t-1}1_{A_i = a}}$$
The sample-average estimates converge to the true values if the action is taken an infinite number of times: $$\lim_{N_t(a) \rightarrow \infty} = q_*(a)$$
**Action values**
Now consider for one action. Only its rewards and its estimate after n+1 rewards are considered $$Q_n \doteq \frac{R_1 + R_2 + \dots + R_{n-1}}{n-1}$$
**Running average**
The running average (or incremental update) is given by $$Q_{n+1} = Q_n + \frac{1}{n}[R_n - Q_n]$$This is a standard form for learning/update rules: 
![[sample average formula.png]]
where the step size is also referred as learning rate. 

The derivation of incremental update is given by
![[running average derivation.png]]

**Non-stationary problem**
Suppose the true action values change slowly over time, this is referred as non-stationary problem. 
In this case, sample averages are not a good idea. Instead, the better option is "exponential, recency-weighted average"$$Q_{n+1} \doteq Q_n + \alpha[R_n - Q_n] = (1-\alpha)^nQ_1 + \sum_{i=1}^{n}\alpha(1-\alpha)^{n-i}R_i$$where $\alpha$ is a constant step-size parameter, $\alpha \in (0, 1]$. 
There is bias due to $Q_1$ that becomes smaller over time. 

**Action-value functions**
An action-value function says how good it is to be in a state, take an action, and thereafter follow a policy $$q_\pi(s, a) = E[R_{t+1} + \gamma R_{t+2} + \gamma^2R_{t+3} + \dots | \space S_t =s, A_t = a, A_{t+1:\infty} \sim \pi]$$

##### Policy
**Optimality**
A policy $\pi_*$ is optimal iff it maximises the action-value function $$q_{\pi_*}(s, a) = \max_\pi q_\pi(s, a) = q_*(s, a)$$
Thus all optimal policies share the same optimal value function.
Given the optimal value function, it is easy to act optimally: $$\pi_*(s) = \arg \max_a q_*(s, a)$$
This is referred as greedification. The optimal policy is greedy with respect to the optimal value function.
There is always at least one deterministic optimal policy. 


##### Bellman equation
The basic idea of Bellman equation for a policy $\pi$:$$G_t = R_{t+1}+\gamma R_{t+2} + \gamma^2 R_{t+3} + \gamma ^3R_{t+4}+\dots = R_{t+1} + \gamma(R_{t+2} + \gamma R_{t+3}+\gamma^2R_{t+4}+\dots)$$ $$ = R_{t+1} + \gamma G_{t+1}$$
So, $$v_\pi(s) = E_\pi{G_t|S_t  = s} = $$


epsilon-greedy test


upper-confidence bound

greedification

Bellman equation



Dense reward, spars reward
considering 

gamma - confidence in looking into future / significance of the future states

Some questions 
1. Why is the reinforcement learning needed when there's supervised learning?
   Not sure about good policy / no proper training set
   AlphaGo has a new opening no human has ever played
2. Is reward in policy reinforcement same as heuristic in A*?
3. 


Difficulty:
so many layers, unstable -> batch normalisation (activation = 0)

ReLu function
sigmoid

Multi-arm bandit machine



Some cool examples:
Atari
AlphaGO

##### Greedification and policy dancing
Given a value function for any policy $\pi$, 
$q_\pi = (s, \space, a)$ for all s, a
It can always be greedified to obtain a better policy
$$\pi '(s) = \arg \max_a q_pi(s, \space a)$$
where the better means 
$$q_{\pi'}(s, \space a) \geq q_\pi(s, \space a)$$

###### Greedification Strategies

1. **ε-Greedy Strategy**:
- **Description**: This strategy chooses a random action with probability ε (exploration) and the current best action with probability 1-ε (exploitation). As learning progresses, ε can decrease, making the agent more likely to choose the optimal action.
- **Example**: In a blackjack game, the player might initially try various strategies to see which works best. Over time, the player will increasingly rely on the known effective strategies, reducing the frequency of trying new ones.
2. **Greedy in the Limit with Infinite Exploration (GLIE)**: 
- **Description**: This more stringent strategy ensures that the optimal solution is found with infinite exploration. It requires that the exploration probability ε decreases to zero over time, but the total number of explorations remains infinite.
- **Example**: In autonomous vehicle path planning, the system might broadly explore various path choices initially but focuses more on proven safe and efficient paths as time progresses.
3. **Softmax Strategy**:
- **Description**: This strategy bases action selection on estimated values and adjusts the exploration-exploitation balance using a temperature parameter. The temperature decreases over time, making the agent favor higher-value actions.
- **Example**: In stock trading strategies, the system initially trades various stocks with a high temperature parameter but gradually reduces trading in less profitable stocks, focusing more on high-return stocks over time.

A **policy dance**, is referred to a policy being greedified (policy update), then the value function found is also strictly better after the policy update, until both policy and value function eventually are both optimal. 
![[截圖 2024-06-19 下午12.43.23.png]]
Q: Is the reinforcement a **convex-problem**? Why does it says no local optima? #review_later 
##### Common algorithms
###### SARSA
![[截圖 2024-06-19 下午12.36.06.png]]
###### Q-Learning
![[截圖 2024-06-19 下午12.36.24.png]]


![[截圖 2024-06-19 下午12.47.42.png]]