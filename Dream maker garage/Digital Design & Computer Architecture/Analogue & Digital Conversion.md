**Analogue to Digital conversion**, sometimes also called **ADC**. It is an electronic component or module used to **convert continuous analog signals into digital form** for **digital signal processing** and analysis. ADCs are critical in many applications including **digital signal processing**, **data acquisition**, **control systems**, and **instrumentation**.
The basic principle of an ADC involves **sampling** and **quantising** a continuous analog signal into a series of discrete digital values. These values are typically represented in binary form and can be further processed and analysed in a computer or digital signal processor.
There is also a process called **digital to analogue conversion (DAC**). 

An analogue signal:
![[analogue signal.png]]

A digital signal:
![[digital signal.png]]


#### Sampling and quantisation
Sampling typically refers to discretising in time in sampling theory. The sample rate determines the time resolution. 
On a microcontroller, the fundamental time resolution is typically the period of the CPU clock. 
![[sampling.png]]


Further on, if the amplitude is also discretised, this is referred as quantisation. 
In terms of error the code width determines the amplitude resolution and introduces quantisation error. 
![[quantisation.png]]


#### Hardware
1. **Input pin(s)**: It requires a channel of data to be read into ADC peripherals. Usually this channel should be multiplexed as it typically is on a shared port. 
2. **AVCC**: The supply voltage pin (input) for all the A/D Converter channels. If the ADC is not used, it should be externally connected to VCC. If the ADC is used, it should be connected to VCC through a low-pass filter. Usually the magnitude of AVCC should be the same as VCC. 
   The reason for using low-pass filter is to reduce the frequency noise that is usually introduced by the direct voltage, as it would affect the measurement in sampling.  
3. **AREF**: This is the analog reference pin (input) for the A/D Converter. AREF stands for Analog Reference Voltage. In the context of microcontrollers with analog-to-digital converters (ADCs), AREF is a pin or reference voltage setting used to establish the maximum voltage range for analog input signals to be converted into digital values by the ADC.
   When using an ADC, the input analog voltage is compared to a reference voltage to determine its digital representation. AREF is typically used to set this reference voltage.

#### Software setup
There are different settings and modes for ADC. Usually it is configured by writing to control registers. It generally have the following modes and settings: 

- **Voltage reference selection**
- **Clock source / prescaler select**:
  This determine the division factor between the XTAL frequency and the input clock to the ADC. If a lower resolution than amount of bits provided is needed, the input clock frequency to the ADC can be higher to get a higher sample rate.
- **Left adjustment**:
  Based on the code, the selection of left adjustment can make it **easier to read the result** of the conversion directly from the ADC's data registers **without the need for additional bit manipulation or shifting**. 
  Normally, ADC results are right-adjusted, the **MSBs are placed in the upper bits** of the data register and **LSBs are in the lower bits**. 
  In left adjustment mode, the **LSBs are placed in the upper bits** of the register, while the **MSBs are in the lower bits**. 
- **Resolution**: 
  Based on the device, the resolution of the data can typically ranges between 8 to 12 pins (so 1/256 to 1/4096). Sometimes the device offers choices to select different resolution based on the need. 
- **Modes**:
  Some typical modes are **free-running mode**, **single conversion mode**, **auto-trigger mode**, **differential mode** and **low-power mode**. 
1. **Single Conversion Mode:** 
   In this mode, the ADC performs a **single conversion** and generates a result. Typically, the conversion needs to be **manually started** by the CPU, which then waits for the conversion to complete.
2. **Free Running Mode:** 
   In this mode, the ADC **continuously performs conversions**, sampling the input signal and updating the result continuously. The first conversion may need to be **manually initiated** by the CPU, but **subsequent conversions are automatically triggered**. This allows the ADC to **continuously sample** the signal and provide new data as needed.
3. **Auto Trigger Mode:** 
   In this mode, the ADC automatically starts conversions based on **external trigger conditions or internal timer signals**. This enables the ADC to trigger conversions based on **specific events** or **time intervals** **without requiring CPU intervention**. In multiplexing of different ADC channels, this mode is not ideal, as it could continue to read-in values while a multiplexing is wanted, causing confusion of which values belong to which channel. 
4. **Differential Mode:** In this mode, the ADC can **compare two different input signals** and measure the difference between them (this can also have a **gain**). This can be used to **eliminate the effects** of **common-mode noise**, improving signal **immunity** and **accuracy**.
5. **Low Power Mode:** 
   Some ADCs also offer low-power modes, which are typically achieved by **reducing the conversion rate** or **lowering the supply voltage** to **reduce power consumption** and **extend battery life**.

In addition, several things need to be set up before execution. 
- Interrupt enable
- ADC enable
- Start conversion signal (refer to the manual initiation in ADC modes)
- Auto-trigger enable
- Left adjust enable

