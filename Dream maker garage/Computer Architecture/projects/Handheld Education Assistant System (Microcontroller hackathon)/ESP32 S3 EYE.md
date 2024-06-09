
In-built features:
 - LCD
 - 8 MB flash
 - PSRAM
 - Accelerometer
 - USB Serial/JTAG interface
 - Antenna

![[ESP32-S3-EYE_MB-annotated-photo.png]]

Key components:
1. **Camera**: The camera [OV2640](https://github.com/espressif/esp32-camera) with 2 million pixels has a 66.5° field of view and a maximum resolution of 1600x1200. You can change the resolution when developing applications.
2. **Module Power LED**: The LED (green) turns on when USB power is connected to the board. If it is not turned on, it indicates either the USB power is not supplied, or the 5 V to 3.3 V LDO is broken. Software can configure GPIO3 to set different LED statuses (turned on/off, flashing) for different statuses of the board. Note that GPIO3 must be set up in open-drain mode. Pulling GPIO3 up may burn the LED.
3. **Pin Headers**: Connect the female headers on the sub board.
4. **5 V to 3.3 V LDO**: Power regulator that converts a 5 V supply into a 3.3 V output for the module.
5. **Digital Microphone**: The digital I2S MEMS microphone features 61 dB SNR and –26 dBFS sensitivity, working at 3.3 V. 
6. **FPC Connector**: Connects the main board and the sub board.
7. **Function Button**: There are six function buttons on the board. Users can configure any functions as needed except for the RST button.
8. **ESP32-S3-WROOM-1**: The ESP32-S3-WROOM-1 module embeds the ESP32-S3R8 chip variant that provides Wi-Fi and Bluetooth 5 (LE) connectivity, as well as dedicated vector instructions for accelerating neural network computing and signal processing. On top of the integrated 8 MB Octal SPI PSRAM offered by the SoC, the module also comes with 8 MB flash, allowing for fast data access. ESP32-S3-WROOM-1U module is also supported.
9. **MicroSD Card Slot**: Used for inserting a MicroSD card to expand memory capacity.
10. **3.3 V to 1.5 V LDO**: Power regulator that converts a 3.3 V supply into a 1.5 V output for the camera.
11. **3.3 V to 2.8 V LDO**: Power regulator that converts a 3.3 V supply into a 2.8 V output for the camera.
12. **USB Port**: A Micro-USB port used for 5 V power supply to the board, as well as for communication with the chip via GPIO19 and GPIO20.
13. **Battery Soldering Points**: Used for soldering a battery socket to connect an external Li-ion battery that can serve as an alternative power supply to the board. If you use an external battery, make sure it has built-in protection circuit and fuse. The recommended specifications of the battery: capacity > 1000 mAh, output voltage 3.7 V, input voltage 4.2 V – 5 V.
14. **Battery Charger Chip**: 1 A linear Li-ion battery charger (ME4054BM5G-N) in ThinSOT package. The power source for charging is the **USB Port**.
15. **Battery Red LED**: When the USB power is connected to the board and a battery is not connected, the red LED blinks. If a battery is connected and being charged, the red LED turns on. When the battery is fully charged, it turns off.
16. **Accelerometer**: Three-axis accelerometer (QMA7981) for screen rotation, etc.


Sub-components: 

1. **LCD Display**: 1.3” LCD display, connected to ESP32-S3 over the SPI bus.
2. **Strapping Pins**: Four strapping pins led out from the main board. They can be used as testing points.
3. **Female Headers**: Used for mounting onto the pin headers on the main board.
4. **LCD FPC Connector**: Connects the sub board and the LCD display.
5. **LCD_RST**: LCD_RST testing point. You can use it to reset the LCD display with control signals.



![[ESP who functionalities.png]]


#### Pinout
- **PWDN_GPIO** stands for "Power-Down GPIO." It is a pin used to control the power-down state of a module or device in embedded systems, particularly in the context of Espressif's ESP32 microcontroller.
- **SIOD_GPIO** stands for Synchronous Input/Output Data GPIO in SPI (Serial Peripheral Interface) devices. In microcontrollers like the ESP32 or others, it is typically used to define the GPIO pin for synchronous data input/output in the SPI interface. This pin is used to define the GPIO pin for synchronous data input/output in the SPI interface, facilitating synchronous data transfer.
- **SIOC_GPIO** is something to do with SPI too. 
- **VSYNC_GPIO** stands for "Vertical Synchronization GPIO." This term is commonly used in the context of display interfaces, particularly when working with TFT (Thin-Film Transistor) LCD (Liquid Crystal Display) screens or similar display technologies.
- **HREF_GPIO** stands for "Horizontal Reference GPIO" and is commonly used in display devices, particularly those using parallel interfaces like TFT LCD (Thin Film Transistor Liquid Crystal Display) screens.
  In display devices, HREF_GPIO is used to indicate the validity of data, specifically whether data is validly transmitted to the rows (or horizontally) of the display device. When the HREF_GPIO signal is high, it indicates that the transmitted data is valid, while a low signal indicates that the transmitted data is invalid.
- **PCLK_GPIO** stands for "Pixel Clock GPIO" and is commonly used in display devices, particularly those utilizing parallel interfaces like TFT LCD (Thin Film Transistor Liquid Crystal Display) screens.
  In display devices, PCLK_GPIO is used to synchronize the output of pixel data with the pixel clock signal. The pixel clock signal determines the rate at which pixel data is transmitted from the display controller to the display panel. PCLK_GPIO generates pulses corresponding to each pixel clock cycle, ensuring that pixel data is transmitted at the correct timing.



In ESP32-based modules, such as Wi-Fi or Bluetooth modules, the PWDN_GPIO pin is often used to control the power state of the module. When this pin is set to a certain level (usually low or high), it triggers the module to enter a low-power or power-down state, effectively turning off the module to conserve power.


#### Question lists encountered:
1. What is a pytest?
   Pytest is a popular testing framework in Python used for writing and running unit tests, integration tests, and functional tests.
2. 