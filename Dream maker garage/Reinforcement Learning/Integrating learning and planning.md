Advantages: 
- Can efficiently learn from supervised learning
Disadvantages:
- Two sources (value function and model) of approximation error

#### Model
A model M is a representation of an MDP, parameterised by $\eta$. The model contains the information about the state transition and reward function. 
Some tactical games require to do look ahead for a complicated strategy planning. This is when model-based planning is better. 


##### Model learning
Use regression to learn the reward function, by taking supervised learning from experience. 
Use density estimation to learn the next state. 


##### Examples of models
- Table lookup model
- Linear expectation model
- Linear Gaussian model 
- Gaussian process model 
- Deep belief network model