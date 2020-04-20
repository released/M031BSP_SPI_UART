# M031BSP_SPI_UART
 M031BSP_SPI_UART

update @ 2020/04/20

1. Fix database offset without save into CONFIG issue 

-- before FMC_Open(), add SYS_UnlockReg()

-- after Write_Data , following with FMC_Close() and SYS_LockReg()

2. How to get checksum under KEILC

- get srecord from http://srecord.sourceforge.net/

- put srec_cat.exe under project
![image](https://github.com/released/M031BSP_SPI_UART/blob/master/srec_under%20project.png)


- add srecord command line into post build

.\obj\srec_cat .\obj\@L.hex -intel -Checksum_Positive_Big_Endian 0x20000 4 -o -HEX_Dump

![image](https://github.com/released/M031BSP_SPI_UART/blob/master/srec_add%20to%20post%20build.png)

![image](https://github.com/released/M031BSP_SPI_UART/blob/master/srec_add%20to%20post%20build%20result.png)

- or under same folder with srec_cat.exe, create bat file as below

**************

;srec_cat template.hex -intel -Checksum_Positive_Big_Endian 0x20000 4 -crop 0x20000 0x20004 -o -HEX_Dump

srec_cat template.hex -intel -Checksum_Positive_Big_Endian 0x20000 4 -o -HEX_Dump

pause

**************

- after build project , create hex file , execute the bat file in File manager
![image](https://github.com/released/M031BSP_SPI_UART/blob/master/srec_under%20project%20with%20bat.png)


3. explanation

- 0x20000 : MCU max flash size range

- template.hex : project file name in KEILC

- 0x20000 4 : at this address after 4 bytes, display check sum




update @ 2019/12/26

Scenario : 

1. Use UART RX (PB.12) to receive unknown length , with UART interrupt

- At UART02_IRQHandler , take (N-1) byte in UART RX buffer (UART FIFO : N byte) ,

- and at last transmit , will trigger RX timeout , then calculate this RX packet total length

- Display RX data under UART_Loop_Process

- may use teraterm with M031 EVM (VCP) , to input data as below ,
![image](https://github.com/released/M031BSP_SPI_UART/blob/master/SampleCode/Template/terminal_operation.jpg)

2. Use SPI0 (PA0 : SPI0_MOSI , PA1 : SPI0_MISO , PA2 : SPI0_CLK , PA3 : SPI0_SS) , to transmit and receive , with SPI interrupt

- SPI data length : 16 , create data at SPI_Master_PreInit and enable interrupt 

- Display RX under SPI_Master_Loop_Process

- below is SPI waveform for reference 
![image](https://github.com/released/M031BSP_SPI_UART/blob/master/SampleCode/Template/SPI_scope.jpg)

3. Use internal flash to emulate EEPROM

- with 2 pages (for backup) , refer to DATA_FLASH_PAGE

- and expected EEPROM adddress total : 16 bytes , refer to DATA_FLASH_AMOUNT

- M031FB0AE : 16K , data flash set to 0x3C00 , refer to DATA_FLASH_OFFSET

- Under Emulate_EEPROM_Process , write data in order and read back according to address and send UART string

- below is EEPROM write and read data log for reference 
![image](https://github.com/released/M031BSP_SPI_UART/blob/master/SampleCode/Template/emulate_eeprom.jpg)


