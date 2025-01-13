
![[PID control.png]]
**PID control**, stands for **proportion**, **integral**, **derivative** control method. It uses information from **present**, **past** and **future** (accordingly) to account for the **error** term (i.e., the distance or value from a current position to a destination). 
It is a control method that the **speed / rate of change is based on the error**. The further the destination is, the quicker the speed goes. 
It is a **closed-loop feedback**, meaning its error term will be **updated** for each loop, and the **speed** will also be updated accordingly. 

##### Proportional factor (P):
Proportional factor, as suggested by the name, is the proportional factor between and **present error** and **destination**. 

![[P control.png]]
The simplest type is proportional control - the speed is linear to the difference in distance. However, considering a **drone** case that a proportional control is insufficient. 
Scenarios: A drone is aiming to go to a certain height, then hover at that height. 
Some considerations for a drone case:
- Usually when an error is zero, the speed will be zero too. This will cause the drone to stop and fall back down. 
- Instead, **hover** is an action that the drone will stop at the height. A certain RPM will make the lifting force equal to gravitational force. So, there is a certain RPM for hovering. 
Imagine a 100 RPM is when the drone is hovering. Based on different errors, the gain (P factor) will change. A gain of 2 will produce 50 metres error (and hover there), a gain of 5 for 20 metres error, a gain of 100 for 1 metre error. 
This is referred as a **steady state error** that no matter what gain has been set, there is always an error presented. 

##### Integrating factor (I):
It has a memory of the past information. It sums up the input signal over time, keeping a running total. 

When a steady state error presented, the integrating factor will have a increasing total sum, that cause the drone to keep rising. At the certain height, it will keep summing and subtracting until it reaches 100 RPM. 
This way, it mitigate or eliminate the steady state error presented.  

![[I control.png]]

However, the path to 100 RPM may not be ideal. 
There might be a case happening: integrating factor is over 100 RPM (so it will keep rising) while the error is really small. 
An overshooting could occur, since going over a goal to have a negative error is required, in order to lower the output of integrator, and slows the drone down. 


##### Differentiating factor (D):
It can predict the error in future. It produces a measure of the rate of the change of error. 
When an error is quickly decreasing, it has a negative rate of change. This negative value will be considered in the loop and slow the drone, preventing it from overshooting. 

![[D control.png]]


##### Tuning constants:
For each control, a weighting factor should be added to specify the significance of that factor in the entire control system. 
There is $K_p , K_i$ and $K_d$ for three weighting factors. 
Based on different application or project, the values could vary. 

![[Tuning constant.png]]



#### Anti-windup
Actuator: a machine that generating forces or energy to the system. 
Process: the actuator trying to go against or trying to affect in some way. 

Although the PID control seems ideal, there are real life constraints that doesn't allow some operations. 
For instance, a linear system is an ideal case that most real world cases doesn't apply to. Constraints such as saturation, backlash, rate constraints will occur

Considering the following example: an error has been constant in the beginning, and the integrating factor has gone over the constraint (1000 rpm). The problem isn't about if the motor will break due to going over the constraint, but the anti-winding when the error starts to decrease. 
Notice the red region is the integral windup that doesn't account for the saturation happening in the motor, causing an extra movement in the system and delay when error starts to be mitigated. 

![[Anti-windup.png]]

Anti-windup is a strategy to reduce this difference between mathematical calculation with the physical constraints and saturation. 
Namely, there are several methods that can be chose from:
- Clamping
- Back-calculation
- Observer approach
Basically, each of the idea is to keep the integrated value from increasing past some specified limit. 

Clamping method is basically introducing a "clamping saturation limit" and "saturation check"
Firstly, there will be a limit defined as saturation limit, it is used as a guideline of the constraints. 
Secondly, there will be a saturation check. The saturation check is evaluating two parts: 
1. If the input = output: if not equal, meaning that the integrating value has passed the saturation limit
2. If the signs are the same: if yes, it means it is making system become more positive / negative
If both condition are met, (i.e., input is not equal to output and both signs are the same), it is indicating that it will have a windup effect. So the clamping method will turn off the integrating path, until one of the conditions is false again. 
Also referred as conditional integration. 

Additionally, a reminder is to set a safety factor of saturation limit (i.e., always lower than the physical limit). 

![[clamping method.png]]

#### Noise filtering

Noises are the generic problem occurring in a system. They can come from different sources: 
- Environment - the background / surrounding noises
- Implementation - the errors in setup or operation
- Defects - the manufacture / hardware defects

Generally, we regard the noises to be disturbing the true signal in terms of their amplitude. So only the loud noises could possibly affect the signal. However, it is not the case for PID control, as the derivative term will take the high frequency (even with small amplitude) and amplify this noise. 

![[frequency derivatives.png]]

To reduce this greater derivative, a way is to reduce the amplitude of the high frequency noise so its slope is not as steep. This can be achieved by attenuation or blockage. 

Since the higher the frequency is, the greater the magnitude of the derivative is, it is important to find a cutoff frequency to block out the further frequencies. 
However, it is also important not to block out the main information, so it is crucial to choose the point of the cutoff frequency. 

![[cut-off frequency setting.png]]

Ideally, a process of signal should be like the following image. With a noise that is widely spread from low to high frequency with similar amplitude, the signal, which is the derivative, should have a cutoff frequency so that the amplitude of signal at high frequency is low / less impact. Note that the amplitude at low frequency may be high, but it wouldn't affect much. 
![[ideal signal process with low-pass filter.png]]


A way to do this is first-order low pass filter. It will allow the low frequency to pass through mostly unchanged, but block off / attenuate the high frequency.

The concept behind it is Laplace transfer function. 

![[Laplace transfer function.png]]

There are two approach from Laplace transfer: 
The first approach is to add a low pass filter before the derivative term. 
![[filtering D term.png]]

Another approach is to have a integral in the feedback path. Simplify the block diagram, it can be expressed mathematically as $$ \frac{N}{1+N\frac{1}{s}}$$
So it will be the same as the first approach.
![[integral feedback.png]]



#### Tuning





##### Rules of thumb
Increasing Kp:
- Decrease in rise time
- 


##### Ziegler-Nichols method



