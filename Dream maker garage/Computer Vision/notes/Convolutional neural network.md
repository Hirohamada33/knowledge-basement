
##### History 
The **Mark I Perceptron machine** was the first implementation of the **perceptron** algorithm. The machine was connected to a camera that used 20×20 cadmium sulfide photocells to produce a 400-pixel image.



Zero padding


Parameters


What is the intuition of choices of stride?
It determines the resolution. Additionally, for a connected neural network, it affects the later layer as well as the number of layers. 



##### Pooling layer

Max pooling is  


##### Training 
In training overview, there is a brief overview of process listed below:
1. **One time setup** - activation functions, preprocessing, weight initialisation, regularisation, gradient checking
2. **Training dynamics** - babysitting the learning process, parameter updates, hyperparameter optimisation 
3. **Evaluation** - model ensembles

###### Activation functions
There are some different choices of activations  


**Sigmoid function** 
![[sigmoid function.png]]

Sigmoid function is historically popular since there is a nice interpretation as a saturating "firing rate" of a neuron. 
However, there are some disadvantages of sigmoid function. 
1. **Saturated neurons "kill" the gradient**: for the region that is not close around centre (for this example, say $x = 10$), which will yields the flat region where gradient is zero. This will give the multiplication of something near zero in the chain rule $\frac{\partial L}{\partial x} = \frac{\partial \sigma}{\partial x} \frac{\partial L}{\partial \sigma}$ which gives the downstream gradient of zero. 
2. **Sigmoid outputs aren't zero-centered (ranges from 0-1)**: consider the case the input to a neuron is always positive. The weight `W` will always be all positive or all negative, depending on the sign of the upstream gradient coming down. This give inefficient gradient update, looking at the below figure, the zig-zag path indicate the direction of allowed update axes, which is inefficient in hypothetical optimal `W` (indicated in blue vector)
   Hence, the input x should be zero-mean, where there allows both positive and negative. 
![[inefficient gradient update due to non-zero centered function.png]]
3. **Exp() is computationally expensive.** But this is just a minor point, considering dot products and convolution operations are also expensive. 


**Tanh function**
![[Tanh function.png]]

Tanh function has a major improvement from sigmoid function, which is that function ranges from \[-1, 1], zero-centered. However there is still flat region at saturated points, which is bad for continuous propagation flow calculation.  

**ReLU (Rectified Linear Unit)**
![[ReLU activation function.png]]
Some major improvement of ReLU, is that it does not have a saturated gradient, and computationally efficient. 
ReLU is more biological plausible than sigmoid function. 
The intuition of ReLU function, is that if the input is less than 0, it will block it. If the input is more than 0, it will let it pass through (with the magnitude of input)
ReLU starting to be used a lot when AlexNet came around. 

However, some major issues with ReLU are including:
1. **Not zero-centered**: in the negative half of the input, the gradient is killed off. 
2. **Dead ReLU**: At the negative half of the input, the ReLU will yields blocked output (0) that causes what was described as "dead ReLU" because it causes the weights never activated or updated. Some scenarios causing this issue, are that (1) weight initialisation, some weights are unlucky to be initialised to a value that is at dead ReLU region, and (2) if the learning rate is too high, the result will oscillate between activated and unactivated, which it will fall into dead ReLU region.  
![[dead ReLU problem.png]]

Below are some variations of ReLU that tried to address the issue and make improvements:

**Leaky ReLU**

![[Leaky ReLU activation function.png]]
The Leaky ReLU modifies the negative regime to be $0.01x$, it improves that there is no saturated gradient, and there will be no dead ReLU problem

**PReLU (Parametric Rectifier)**

![[PReLU activation function.png]]
PReLU, similar to leaky ReLU idea which in negative region the slope is given by $\alpha x$ that is parametric and can be determined in backpropogation. It gives a bit more flexibility 

**ELU (Exponential Linear Units)**

![[ELU activation function.png]]
The advantage of ELU is closer to zero-mean outputs. The negative region is saturation regime, adding more robustness to noise in the deactivation state. 
The function will required expensive exp() operation. 


**Maxout**
Maxout does not have the basic form of dot product -> nonlinearity process. Instead, its process is described mathematically as $$max(w_1^Tx+b_1, \space w_2^Tx+b_2)$$
This generalises the idea of ReLU and Leaky ReLU, where it is linear regime, and it has the advantage of not saturating and not dying. 
The disadvantage is it doubles the number of parameters per neuron for tuning. 

**Summary**
In practice, 
1. general standard rule is use ReLU, but be careful with learning rates
2. Try out Leaky ReLU / Maxout / ELU
3. Try out tanh (but not to expect much)
4. Don't use sigmoid (early discovered function)

##### Data preprocessing
The original data requires some preprocessing, including zero-centering the data and normalisation. 
In machine learning image processing, sometimes the process also includes PCA and whitening of the data. This is not required in CNN, is because machine learning process was to project the image pixels values onto a lower dimension (feature dimension). Convolution Neural Network works for applying the filter spatially, so physically the PCA and whitening are not required. 
Note that data preprocessing is required for training data as well as test data. 

###### Zero centering
This can be done by either:
- Subtract the mean image: The mean was taken of all training set. 
- Subtract per-channel (RGB) mean
A question about zero-centering, if the images data were zero-mean, were problem of non-zero-centered  at activation functions solved?
The answer is, it actually not sufficient and only helps at the first layer in deep network. 

Another intuition for data preprocessing (zero-centering, normalisation) is, from the below graph it can be seen that the classification is sensitive for small perturbation, which for optimisation stage it could be more difficult in updating weights (loss is sensitive to parameter vectors). While after the normalisation, it is less sensitive to the perturbation. 
![[intuition for data processing (zero centering and normalisation).png]]

###### Weight initialisation
All the updates of weight require the first run of deep network, suggesting that the weight has to have some initial values for the backprop. 
If the `W` is set to zero matrix, the neurons will all do the same operations, hence getting the same gradient thus same updates. However the ultimate goal was each neuron to learn different knowledge and perform different operation. 

Hence, that lead to the idea of using small random number. 
```python
W = 0.01 * np.random.randn(D, H)
```

However it may not work in a deeper network. The mean and standard deviation (gaussian distribution) will quickly shrink and collapse to near zero. This is due to the accumulative multiplication in later layers with small weights. 
The effect of this on backprop updates, is that due to the collapsed value it will be nearly not updating back from the upstream. Additionally, in backprop it uses the chained derivative where the upstream gradient is dot product with the local gradient, which will cause the same phenomenon of zero collapsing of upstream gradient.

![[zero collapse example.png]]


Now try to sample the standard deviation of 1 instead of 0.01. 
```python
W = 1 * np.random.randn(D, H)
```

The issue now is that since the initialised weights values are big, the output will more likely to fall in saturated regime, which it will be either 1 or -1 (in a tanh activation function case). 
So, weight initialisation could be a difficult problem - with small values it is easily to collapse to zero, with big values it is easily to get saturated output. 

**Xavier initialisation** was trying to address the issue. The idea is that in each layer there is approximately a unit gaussian at each layer. The formula was to sample the standard gaussian, then scale by the number of inputs. Basically it is to specify the variance of the input to be the same as the output. 
Intuitively, with small number of inputs, it is divided by small scale to get large weights, to get the same variance at larger spread output. 
It assumes the linear activations (activation region), hence it will broke if using ReLU (nonlinearity), which at negative region it "dies out" and the output variance becomes halved. 
This will cause the zero collapse again. 
This can solved by divided by 2 additionally from the fact that half of the neurons get killed. 

Proper initialisation actually making a big difference towards the training stage and performance. 

##### Batch normalisation 
Batch normalisation related the idea that wanting to keep activations in a gaussian distributed range. 

$$\hat{x}^{(k)} = \frac{x^{k} - E[x^{(k)}]}{\sqrt{Var[x^{(k)}]}}$$
After the Xavier weight initialisation to make the unit gaussian at each layer implicitly, the batch normalisation will explicitly make the unit gaussian. 

The exact process of batch normalisation is shown as follow:
![[batch normalisation.png]]
1. There are N training examples, each sample has dimension D. The empirical mean and variance are computed across the batch, independently for each dimension. 
2. Apply normalisation


The location of batch normalisation operation is usually before the nonlinearity and after a dense layer with activation. 
![[location of BN layer.png]]
The reason was even with proper weight initialisation, there could still be bad scaling effect, which batch normalisation layer can used to justify this (undo the effect). 
For convolutional layers, the normalisation is also applying jointly across both all feature dimensions and spatial locations, to obey convolutional property (nearby location normalised same way).

However, it is not always necessary wished to eliminate the saturation regime in activation functions. A unit gaussian input is constraining to the linear regime in nonlinearity. Sometimes some degree of saturation is better, it is hoped to have a little (controlled) saturated output. 
To address this, is adding the squashing and scaling operation after batch normalisation. 
This is given by $$y^{(k)} = \gamma ^{(k)} \hat{x}^{(k)} + \beta ^{(k)}$$
The network can learn that $\gamma ^{(k)} = \sqrt{Var[x^{(k)}]}$  and $\beta ^{(k)} = E[x^{(k)}]$ to recover the identity function mapping, as if the batch normalisation didn't exist. This creates the flexibility of doing everything between, more or less saturated for better training purpose. 

In a mini-batch normalisation, it follows the below procedures:
![[mini-batch operations.png]]

As a result, it improves gradient flow in forward and backward propagation, allowing wider range of learning rates, and reduce the dependence of initialisation. Overall it is a robust solution in data preprocessing to address the issues mentioned before. 

Because each input data is normalised by the empirical mean over that batch, it can be seen as sort of a regularisation process as well. 
Hence it is no longer deterministic, and jitters the representation of input, hence sort of gives a regularisation effect. 


##### Monitor training
After data preprocessing and choosing an architecture that meet the requirement, 
first check is to see if the loss is reasonable (first having regularision disabled)
Then turn on the regularisation, see if the loss has increased. 

Another sanity check, start with small amount of training sets. The idea is to overfit the model and make the loss close to zero (when regularisation is disabled). 
(These are sanity checks)

Next step is to start training, with small regularisation. Learning rate is determined first, and compared with the update process in each epoch to see if the learning rate was too slow, or too fast where the cost is exploding to infinity due to divergent learning rate. 

Hyperparameter optimisation is done by cross-validation strategy. This is done in stages. 
First stage: only a few epochs to get rough idea of what params work
Second stage: longer running time, finer search
So this is a coarse -> fine stages. 

A tip for detecting explosion is early exit for `cost > 3 x original cost`

![[random vs grid search.png]]
In practice the better search is random search over grid search (fixed set of values searched in a grid manner). The reason is, as can be seen above a grid layout can miss where a good region of hyperparams are, but using a random search an outline of distribution can be seen from the result. 

Some hyperparameters to play with are, 
- Network architecture
- Learning rate, decay schedule, update type 
- Regularisation (L2/Dropout strength)

In hyperparams tuning cross-validation results were compared. As can be seen below the "command center" is used for running bunch of different settings and results were compared in a tabular manner. 
![[cross-validation table.png]]

Here outlines a loss curve for tuning learning rates. Some common behaviour of learning rates were visualised. 
Notice that a steep change with a plateau, is also an indicator of learning is too high. It is taking too large jumps and not able to settle well in local optimum.  
In practice a good learning curve is a steep change, but keep updating values. 
![[behaviour of different learning rates.png]]

Sometimes the loss-time curve can be looked like this. This suggests a prime suspect of bad initialisation, that it wasn't able to learn anything until suddenly the update is towards the right direction and reflect on the loss-time curve
![[bad initialisation loss-time curve.png]]


In the accuracy-time plot, the training and validation accuracies are compared. A big gap suggest the model is now overfitting and will perform poorly on unforeseen datasets, and no gap suggest there are some capacity for further optimising still. 
![[accuracy-time plot.png]]

Just a slight note: sometimes it is a good idea to have a monitor window setup for loss-time and accuracy-time plot to babysit the process. 
![[monitor window of loss-time and accuracy-time plot.png]]

The update is also tracked in the process, by the ratio of weight updates over weight magnitudes. The ratio should be somewhere around 0.001
```python
# assume parameter vector W and its gradient vector dW
param_scale = np.linalg.norm(W.ravel())
update = -learning_rate * dW
update_scale = np.linalg.norm(update.ravel())
W += update
print(update_scale / param_scale) # hope something around 1e-3
```


The accuracy can jumped up very quickly in early stage of training even the loss is still relatively diffuse. This is due to the weights start going to the right direction in learning process, and maximum correct value is taken.


##### Optimisation
The previous optimisation gives 
```python
while True:
	weight_grad = evaluate_gradient(loss_func, data, weights)
	weights += -step_size * weights_grad
```

###### Problems with SGD optimisation
**Zig-zag behaviour**
However, with the following landscape SGD optimisation will have some issues. The landscape looks like "taco shell", where loss function is sensitive in one direction but not in others. This is referred as, the loss function having a bad condition number at this point, which is the ratio of largest and smaller singular number in Hessian matrix. 

This is referred as the characteristic zig-zagging behaviour (undesired), where the direction of the gradient does not align with the direction towards the minima. 
![[zig-zag problem.png]]

This is quite common considering the multi-dimension space is large in data. 


**Local minima and saddle point**
For a function that has a local minima or saddle point, the gradient will have a zero at turning point. 
Saddle points are much more common in high-dimension, considering for a local minima to occur the gradient must be all positive in every direction (i.e., every dimension). 

The idea extends to when the gradient is small (nearly zero), the progress will be very slow. 

![[local minima and saddle point.png]]


**Stochastic noiseness**
Considering for SGD optimiser, the gradient update comes from mini-batch (for the update through whole dataset is expensive). If the mini-batch is noisy, it will reflects on the gradient-update where as can be seen the direction isn't optimal. 
$$L(W) = \frac{1}{N}\sum_{i=1}^{N}L_i(x_i, y_i, W)$$
$$\nabla_wL(W) = \frac{1}{N}\sum_{i=1}^{N}\nabla_wL_i(x_i, y_i, W)$$
 ![[gradient noise.png]]

###### Solution 
**Momentum**
The normal SGD formula gives $$x_{t+1} = x_t-\alpha\nabla f(x_t)$$
```python
while True:
	dx = compute_gradient(x)
	x += learning_rate * dx
```

For SGD update it is stepping in the direction of the gradient. 

Adding a momentum term it becomes $v_{t+1} = \rho v_t + \nabla f(x_i)$ 
$x_{t+1} = x_t - \alpha v_{t+1}$
```python
vx = 0
while True:
	dx = compute_gradient(x)
	vx = rho * vx + dx
	x += learning_rate * vx
```

The idea is to maintain the original velocity over time, adding the gradient estimate to the velocity, then step in the direction of the velocity, instead of the gradient. 

$\rho$ is the friction, it used for decaying the velocity. The initialisation of velocity almost always set to zero. Intuitively, the velocity is sort of a weighted sum of gradients seen over time, with more recent gradients weighted heavier. 

By adding the momentum into the equation, the problems for zero-gradient, poor conditioning and gradient noise were able to be solved. 

![[Momentum effect on SGD issues.png]]
With the momentum, the update rates will be maintained by the velocity so it is robust against small gradient or gradient noises. For zig-zagging problem, the velocity in vertical direction will be cancelled after some iteration, but accelerating in horizontal axis. 

The following shows the vector form in momentum. 
The normal momentum takes the vector addition in velocity and gradient. 
The Nesterov momentum is a variation, where the gradient is measured after stepping into the point where velocity is directing. This allow more mixing in information. Say if the velocity direction was wrong, it allows incorporate the gradient information from a little bit larger parts of the objective landscape. 
For convex-problem Nesterov has some theoretical properties, but neural network is a non-convex problem. 
![[Nesterov momentum.png]]

$v_{t+1} = \rho v_t - \alpha \nabla f(x_t + \rho v_t)$ 
$x_{t+1} = x_t + v_{t+1}$

Noticed one problem with Nesterov momentum is that $x_t + \rho v_t$ the gradient and loss were not measured from the same point. 
There is a change of variable that can try to solve this issue:
$v_{t+1} = \rho v_t - \alpha \nabla f(\tilde{x_t})$ $\tilde{x}_{t+1} = \tilde{x}_t - \rho v_t + (1 + \rho)v_{t+1} = \tilde{x}_{t} + v_{t+1} + \rho(v_{t+1} - v_t)$
```python
dx = compute_gradient(x)
old_v = v
v = rho * v - learning_rate * dx
x += -rho * old_v + (1+rho) * v
```
After #review_later 


Below is the comparison of SGD, added momentum and Nesterov momentum. Here it can be seen that SGD progresses slow in updating, and momentum and Nesterov will have some "overshoot" effect as there is a velocity term. 

![[overshooting for momentum and Nesterov.png]]

One interesting question, is that if the minima is "sharp" or "small" enough, will the overshooting simply go over the minima? 

Comparing the sharp and flat minima, flat minima is actually more robust. The small minima may due to the size of the data size, and sometimes it disappears if the amount of data size is increased, and the landscape is changed. 
Additionally, these sharp minima might be bad minima where it causes the overfitting. 
Hence, it is a feature instead of a bug when the SGD skips over the sharp minima. 

**AdaGrad**
```python
grad_squared = 0
while True:
	dx = compute_gradient(x)
	grad_squared += dx * dx
	x -= learning_rate * dx / (np.sqrt(grad_squared) + 1e-7)
```
The idea of AdaGrad is to have a running sum of squared gradient term, and in the update, the parameter vector is divided by the grad squared term. 
Notice that there is a `1e-7` term, which is to ensure that there is always a small number (no zero-division). 
Recall the problem with the zig-zag problem, the squared grad term ensures that when the gradient is small, it will accelerate the learning process in the dimension (since divide by small number) but in the large gradient, it will slow down the process of updating the parameter, preventing the wiggling in some dimensions that have great gradient.  
![[zig-zag problem.png]]
However, the problem is that as time grows, the stepping size becomes smaller as the grad squared term is a running sum term. 
In a convex problem, this is fine; but in a non-convex problem (where there is saddle points and local minima), then the step could be stuck. 

**RMSProp** 
```python
grad_squared = 0
while True:
	dx = compute_gradient(x)
	grad_squared = decay_rate * grad_squared + (1 - decay_rate) * dx * dx
	x -= learning_rate * dx / (np.sqrt(grad_squared) + 1e-7)
```
**RMSProp** is a variation of AdaGrad. Instead, the squared term will be decaying, that is sort of like the momentum update, except it is the momentum of the squared gradient, rather than the momentum of the actual gradient.  
These estimates are leaky, it sort of addresses the problem of always slowing down when it is not desired in AdaGrad. 

Below is the comparison of SGD, SGD with momentum, and RMSProp.
![[截圖 2024-06-17 下午2.58.15.png]]


**Adam optimiser**
Adam optimiser incorporates the advantages of (1) momentum, a weighted sum of velocity (past gradients) and gradient and (2) division of squared grad term that adjust the update speed based on the dimension gradient.

```python
# Almost Adam optimiser
first_moment = 0
second_moment = 0
while True:
	dx = compute_gradient(x)
	first_moment = beta1 * first_moment + (1 - beta1) * dx
	# Momentum
	 
	second_moment = beta2 * second_moment + (1 - beta2) * dx * dx
	x -= learning_rate * first_moment / (np.sqrt(second_moment) + 1e-7)
	# AdaGrad / RMSProp
```
However the problem with this is that, if the initialisation of `second_moment` is set to a small value or zero, then in the first update the step is divided by a small value and causing a big step. This is rather than a geometric setting of the problem but an artifact from the algorithm setup. 
It could have been the case when the initialisation is bad, then the position is ending up in a bad position from large step in the objective landscape that is unable to converge. 

The complete definition of Adam optimiser is:
```python
first_moment = 0
second_moment = 0
for t in range(num_iterations):
	dx = compute_gradient(x)
	first_moment = beta1 * first_moment + (1 - beta1) * dx
	second_moment = beta2 * second_moment + (1 - beta2) * dx * dx
```


##### Learning rates
![[截圖 2024-06-21 下午10.56.53.png]]

For a learning rate definition, it does not have to be a constant. Sometimes it could be a non-stationary term:
- **Step decay**: decay learning rate by half every few epochs
- **Exponential decay**: $\alpha = \alpha_0 e^{-kt}$ 
- **1/t decay**: $\alpha = \alpha_0 / (1+kt)$
Learning rate is more common in SGD momentum and less common in Adam optimiser. 
Notice that learning rate decay is a second-order hyperparameter. Typically the learning rate is initially set to non-decaying otherwise the training process could get confusing. 

##### Second-order approximation
A first-order approximation is a Taylor linear approximation. The point only incorporates the information of first-derivative of the function, stepping a small step (as the approximation does not converges in a large region, the step size cannot be too large). 
![[截圖 2024-06-21 下午11.03.20.png]]

With the second-order Taylor approximation, the step size can be larger as it can directly go to the minimum. This is the idea of second-order optimisation.
![[截圖 2024-06-21 下午11.05.41.png]]

The second-order Taylor expansion gives
$$J(\theta) \approx J(\theta_0) + (\theta - \theta_0)^T \nabla_\theta J(\theta_0) + \frac{1}{2}(\theta - \theta_0)^TH(\theta - \theta_0)$$
Solving for the critical point the Newton parameter update is 
$$\theta^* = \theta_0 - H^{-1} \nabla_\theta J(\theta_0)$$
However, the Hessian matrix $H$ has $O(n^2)$ elements and inverting matrix $H^{-1}$ requires $O(n^3)$, where $n$ is the number of parameters (usually tens to hundreds of millions). 

An approximation of Hessian matrix os Quasi-Newton methods (**BGFS** is the most popular). Instead of inverting the Hessian ($O(n^3)$), approximate inverse Hessian only takes $O(n^2)$
**L-BFGS (limited memory BFGS)** does not form / store the full inverse Hessian. It usually works very well in full batch, deterministic mode, but not in mini-batch settings, neither in large-scale, stochastic setting nor non-convex problems. 

Genrally:
- Adam is good default choice
- If can afford for full-batch update, try L-BFGS

##### Model ensembles
Model ensembles is a trick where instead of training independent models, use multiple snapshots of a single model during training. 
**Polyak averaging**

##### Regularisation
**Dropout** is a common strategy in regularisation of the neural network. In each forward pass, randomly set some neurons to zero (activations) (usually 0.5 for the probability of dropping, hyperparameter). More common in fully connected layer. 
![[截圖 2024-06-22 下午1.40.25.png]]

```python
def train_step(X):
	H1 = np.maximum(0, np.dot(W1, X) + b1)
	U1 = np.random.rand(*H1.shape) < p # first dropout mask
	H1 *= U1 # drop
	H2 = np.maximum(0, np.dot(W2, H1) + b2)
	U2 = np.random.rand(*H2.shape) < p # second dropout mask
	H2 *= U2 # drop 
	out = np.dot(W3, H2) + b3
```

The idea of dropout is, for a classifier to identify the features it rely across many different features. 
![[截圖 2024-06-22 下午1.46.04.png]]

