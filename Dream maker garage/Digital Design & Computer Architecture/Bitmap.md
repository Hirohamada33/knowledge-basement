**Bitmap** is a commonly-seen image format in the context of microcontroller. 
Generally when it comes to formatting, there are several components to be considered:
1. **ColoUr Depth**: Determine the colour depth of the display, which specifies the number of bits used to represent each pixel. Common colour depths include 16-bit (RGB565), 18-bit (RGB666), 24-bit (RGB888), and others.

2. **Pixel Arrangement**: Understand the arrangement of colour components for each pixel (e.g., RGB, RGBW). This affects how pixel data is organized and transmitted.

3. **Pixel Data Format**: Format pixel data according to the display's requirements. Each pixel's colour information may need to be packed into a specific number of bits in a particular order (e.g., Red, Green, Blue).

4. **Endianness**: Consider the endianness (byte order) of the display controller and adjust the byte order of pixel data if necessary to ensure correct interpretation.

5. **Interface Protocol**: If using a serial interface like SPI, data may need to be sent in a specific format, such as bytes or words, along with any necessary control signals (e.g., RS, CS).

6. **Row and Column Addressing**: Determine the method for addressing individual pixels on the display (e.g., row-major or column-major addressing) and format pixel data accordingly.

7. **Data Transmission**: Transmit pixel data to the display controller using the selected interface protocol and appropriate commands or signals to specify the start of pixel data transmission, row/column addressing, etc.

8. **Refresh Rate**: Consider the display's refresh rate and update pixel data accordingly to achieve smooth animations or video playback, if applicable.


##### Colour depth 
There are slightly different ways in terms of colour depth. 
