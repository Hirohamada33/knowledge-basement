##### Register
A register is a digital electronic device capable of storing several bits of data. Normally made from D-type flip-flops with asynchronous `RESET`input. 
Operates on the bits of the data word in parallel (parallel in / out)
![[截圖 2024-12-02 上午11.46.34.png]]
The operation process is:
- Data in each data input is stored in flip-flops on the rising edge of `CLK`
- The data can be read from the Q outputs
- New data can be reloaded by another clock cycle of the register
- The register can be cleared (zeroed) by asserting the `CLEAR` inputs

where it has the circuit symbol of![[截圖 2024-12-02 上午11.47.05.png]]

##### Shift register
When the Q outputs are connected to the successive input, this is referred as a shift register. 
![[截圖 2024-12-02 上午11.49.47.png]]
The operation process is:
- Each bit shift by 1 flip-flop on each clock cycle
- All flip-flops can be asynchronously reset
- Parallel data can be asynchronously loaded into flip-flop using the P signals
- 

The common applications are:
- Multiplication and division by integer power of 2
- Conversion of data between parallel formats and bit-serial format


##### Register file