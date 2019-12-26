# M031BSP_SPI_UART
 M031BSP_SPI_UART

update @ 2019/12/26

Scenario : 

1. Use UART RX (PB.12) to receive unknown length , with UART interrupt

- At UART02_IRQHandler , take (N-1) byte in UART RX buffer (UART FIFO : N byte) ,

- and at last transmit , will trigger RX timeout , then calculate this RX packet total length

- Display RX data under UART_Loop_Process

- may use teraterm with M031 EVM (VCP) , to input data as below ,
https://github.com/released/M031BSP_SPI_UART/SampleCode/Template/terminal_operation.jpg

2. Use SPI0 (PA0 : SPI0_MOSI , PA1 : SPI0_MISO , PA2 : SPI0_CLK , PA3 : SPI0_SS) , to transmit and receive , with SPI interrupt

- SPI data length : 16 , create data at SPI_Master_PreInit and enable interrupt 

- Display RX under SPI_Master_Loop_Process

- below is SPI waveform for reference 
https://github.com/released/M031BSP_SPI_UART/SampleCode/Template/SPI_scope.jpg

3. Use internal flash to emulate EEPROM

- with 2 pages (for backup) , refer to DATA_FLASH_PAGE

- and expected EEPROM adddress total : 16 bytes , refer to DATA_FLASH_AMOUNT

- M031FB0AE : 16K , data flash set to 0x3C00 , refer to DATA_FLASH_OFFSET

- Under Emulate_EEPROM_Process , write data in order and read back according to address and send UART string

- below is EEPROM write and read data log for reference 
https://github.com/released/M031BSP_SPI_UART/SampleCode/Template/emulate_eeprom.jpg


