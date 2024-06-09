TFT stands for **Thin-Film Transistor**. It is a type of transistor that is commonly used in flat-panel displays, such as liquid-crystal displays (LCDs) and organic light-emitting diode (OLED) displays. TFTs are essential components for controlling individual pixels in these displays, allowing for precise control of brightness and color. They are called "thin-film" transistors because they are typically manufactured using thin layers of semiconductor materials deposited onto a substrate, such as glass or plastic. This technology enables the creation of high-resolution and high-quality display screens used in various electronic devices, including smartphones, televisions, computer monitors, and tablets.

Several data pins required to drive LCD:
- RS (mode select):
  The register select (RS) signal is a control input used to specify whether the data being sent to the display is a command or pixel data. This signal determines whether the display controller should interpret incoming data as a command to change settings (e.g., display configuration, cursor position) or as pixel data to be displayed on the screen.
  1. **Command Mode**: When the RS signal is set to indicate command mode (usually logic `LOW`), the display controller expects incoming data to be commands. These commands can include instructions to initialise the display, set the display parameters (e.g., resolution, orientation), clear the screen, or perform other control functions.

2. **Data Mode**: When the RS signal is set to indicate data mode (usually logic `HIGH`), the display controller interprets incoming data as pixel data to be displayed on the screen. Pixel data can represent images, text, graphics, or any other content that you want to show on the display.

- CS (chip select)
- SCK (SPI clock)
- SDA (SPI data bus)
- RESET (reset):

