
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
![[截圖 2024-06-09 下午3.01.53.png]]
Some major improvement of ReLU, is that it does not have a saturated gradient, and computationally efficient. 
ReLU is more biological plausible than sigmoid function. 
The intuition of ReLU function, is that if the input is less than 0, it will block it. If the input is more than 0, it will let it pass through (with the magnitude of input)
ReLU starting to be used a lot when AlexNet came around. 

However, some major issues with ReLU are including:
1. **Not zero-centered**: in the negative half of the input, the gradient is killed off. 
2. **Dead ReLU**: At the negative half of the input, the ReLU will yields blocked output (0) that causes what was described as "dead ReLU" because it causes the weights never activated or updated. Some scenarios causing this issue, are that (1) weight initialisation, some weights are unlucky to be initialised to a value that is at dead ReLU region, and (2) if the learning rate is too high, the result will oscillate between activated and unactivated, which it will fall into dead ReLU region.  
![[截圖 2024-06-09 下午3.22.40.png]]

Below are some variations of ReLU that tried to address the issue and make improvements:

**Leaky ReLU**

![[截圖 2024-06-09 下午3.32.50.png]]
The Leaky ReLU modifies the negative regime to be $0.01x$, it improves that there is no saturated gradient, and there will be no dead ReLU problem

**PReLU (Parametric Rectifier)**

![[截圖 2024-06-09 下午7.45.42.png]]
PReLU, similar to leaky ReLU idea which in negative region the slope is given by $\alpha x$ that is parametric and can be determined in backpropogation. It gives a bit more flexibility 

**ELU (Exponential Linear Units)**

![[截圖 2024-06-09 下午7.49.53.png]]
The advantage of ELU is closer to zero-mean outputs. The negative region is saturation regime, adding more robustness to noise in the deactivation state. 
The function will required expensive exp() operation. 


**Maxout**
Maxout does not have the basic form of dot product -> nonlinearity process. Instead, its process is described mathematically as $$max(w_1^Tx+b_1, \space w_2^Tx+b_2)$$
