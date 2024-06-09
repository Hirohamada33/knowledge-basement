```c
void uart_init(void) {  
	PORTB.DIRSET = PIN2_bm; // Enable PB2 as output (USART0 TXD)  
	USART0.BAUD = 1389; // 9600 baud @ 3.3 MHz  
	USART0.CTRLA = USART_RXCIE_bm | USART_DREIE_bm; // Enable DRE/RX interrupts
	USART0.CTRLB = USART_RXEN_bm | USART_TXEN_bm; // Enable Tx/Rx

}

ISR(USART0_RXC_vect) {  
	*(rxbuf++) = USART0.RXDATAL; // Read new data into buffer  
	if (rxbuf == RXBUFEND) USART0.CTRLA &= ~USART_RXCIE_bm; // Disable interrupt

}

ISR(USART0_DRE_vect) {  
	USART0.TXDATAL = *(txbuf++); // Write data out to UART  
	if (txbuf == TXBUFEND) USART0.CTRLA &= ~USART_DREIE_bm; // Disable interrupt

}
```