 
 
 
 
 
  
 
 
Project 1 
Ye Qiao 
Austin Patrick 
Instructor: Dr. Kimball 
ECE 387 
March 13th, 2020 
 
 
 
 
 
Description: 
The ultimate goal of this project is building a data logging system via FPGA and test it with Arduino ATmega328p microcontroller. It is useful when data collected is not needed in real time, but later than when collected. We implement logic with writing Verilog in Quartus software as two separate modules, one for AD/C reading and the other for EEPROM writing. 
Once we successfully write data into the EEPROM, we pull it out and hook it up with Arduino microcontroller and read data out to serial monitor for demonstration. 
 
Component used: 
1.	Arduino board, attached to a PC 
2.	EEPROM IC chip (Digi-Key Part # 24LC256-I/P-ND) 
3.	Analog to Digital converter IC chip (Digi-Key Part # MCP3002-I/P-ND) 
4.	Breadboard 
5.	Signal Generator 
6.	Jumper wire  
7.	DE-2 FPGA board 
8.	2x 5k ohm resistors (Arduino part as pull high resistors) 
9.	2x 10k ohm resistors (FPGA to EEPROM protection resistors) 
 
Results and Discussion, and conclusion: 
Referring from gist.github.com, we wrote A/DC module with alternating channel mode with 
50Khz of sub clock. State machine was configured with total 16 state since the AD/C chip need 16 clock cycle to finish 10-bit of value grabbing. We set up a latch to alternate channel to config the system with lower delay rate. We also have a 10-bit shared register to pass value to 
EEPROM module and a notify signal come from the EEPROM submodule to report if the EEPROM finish writing values. Once reported notify signal was pulled to high. The AD/C module will resume to take next value in, otherwise it will stay in previous state. We also mapped the output of AD/C to ten red led to verify the values. 
Our Verilog code for EEPROM submodule is relatively straight forward. We followed the data sheet and set up a total of 41 state machine controlling operation of every clock pulse to perform I2C protocol data transfer. The last state is using to notify AD/C module to resume and update data. Since our EEPROM module work with 50hz sub-clock rate, time period of one signal pulse is enough for AD/C module to update its data value. Moreover, we throw out two the list significant value from AD/C data because we implement our EEPROM by byte writing.  
An Arduino code was been written to read the data out form EEPROM chip and print it out on serial monitor. We used wire.h library header file to implement this part of the project after asking permission from instructor. 
As it turns out, we manage to take data from AD/C and successfully store it to EEPROM and read it out afterward. We spent a lot of time on timing control and I2C protocol implementation and finally learned how it work and finish the project task. 
