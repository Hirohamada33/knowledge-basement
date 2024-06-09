*Pulse width modulation is a technique in which a signal is encoded into the pulse width of a digital signal*


A typical PWM signal is a periodic, rectangular waveform that alternates between the low and high states. 
![[PWM.png]]

PWM signal can be characterised in terms of its **frequency** and **duty cycle**. 
**Frequency** is determined by the number of cycles of the PWM waveform per second. It is mathematically expressed as $$f_{PWM} = \frac{1}{T_{PWM}}$$**Duty cycle** is a measure of the ratio of the **high** time of the signal compared to the total PWM period, ranges from 0% to 100%. It is mathematically expressed as $$D=\frac{T_{high}}{T_{PWM}}$$ ![[PWM DAC.png]]
PWM can be used as a form of **digital to analogue conversion (DAC)** by using some **modulating signal** to set the **duty cycle** of our PWM output. 
In analogue, an input signal would be encoded into PWM by comparing a triangular waveform (the **carrier**) with some input signal (the **modulating signal**). The **intersection** of these signal creates the **PWM output**.
The input signal is exactly encoded into the output PWM signal and can be recovered through a process of filtering (essentially within the average of the PWM signal)


![[digital PWM.png]]


