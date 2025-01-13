Multiplexing is a technique used to share **limited resources** in a system to achieve **more efficient resource utilization and functionality expansion**. In embedded systems and communication systems, multiplexing is commonly applied in the following two aspects:

1. **Resource Sharing**: Multiplexing technology can be used to share hardware resources such as **sensors**, **input/output pins**, **communication interfaces**, etc., among multiple devices. This approach can significantly save **costs** and **space** while improving the overall performance of the system.

2. **Time Division**: In communication systems, multiplexing can be used to allocate a single communication channel to multiple users or multiple devices, enabling simultaneous communication. This includes techniques such as Time Division Multiplexing (TDM), Frequency Division Multiplexing (FDM), Space Division Multiplexing (SDM), etc.

In the context of microcontroller multiplexing, usually it referred to a pin that can be used for different purposes, such as I/O port, ADC conversion, serial communication, etc. 
There will be an enable signal for a multiplexing signal, so it will turn the pin into the specific usage. 

Here provides an example of multiplexing in GPIO ports. 

![[multiplexing.png]]

- **EXTINT (External interrupt)**: 
   A common feature in embedded systems. External interrupts allow external events, such as changes in pin states, to trigger the execution of **Interrupt Service Routines (ISR)** in microcontrollers or microprocessors, enabling **timely responses** to external events and performing appropriate actions. When the external event meets the trigger condition, it stops the main function and **priortises** the pre-defined external **interrupt service routine**. By using external interrupts, the system can **respond immediately** to these events **without continuously polling** the pin states, which helps conserve resources and improve system efficiency.
- **PCINT (Pin change interrupt)**: 
   It allows the microcontroller to trigger an ISR when a **change in the state** of a **specified pin** occurs. PCINT is primarily used in applications where timely handling of pin change events is required, such as button presses, sensor triggers, etc. Similar to EXTINT in terms of priortising the ISR and prevent polling method. 
- **PTC (Peripheral touch controller)**:
   It is used for **capacitive touch sensing** applications, such as touch buttons, touchpads, and other touch-sensitive interfaces. The PTC operates by **detecting changes in capacitance** caused by human touch. By analyzing these capacitance variations, the PTC can determine the **position or gesture of the touch**.
   They are widely used in various **consumer electronics**, **industrial control systems**, and **human-machine interface** (HMI) applications to provide **intuitive** and **responsive** touch interfaces. 
- **OSC (Oscillator)**: 
   In embedded systems, oscillators are commonly used to provide the **clock signal** for microcontrollers or microprocessors, determining the **operational frequency** of the system. These clock signals are used to **synchronize the operations** of the processor and to **synchronize data transfer** with external devices. Oscillators can be implemented in various ways, such as **crystal oscillators**, **ceramic resonators**, or **RC oscillators**, depending on the requirements of the application and cost considerations.
   Sometimes an external oscillator will be employed, instead of using internal clock of microcontroller, usually is due to the following advantages:
   - Accuracy and stability
   - Frequency flexibility
   - Noise immunity
   - Power consumption
   - Clock synchronisation (used in several devices usually)

- **T/C (Timer / counter)**
- **ADC (Analogue to digital conversion)**
- **USART / I2C / SPI (Serial communication)**
