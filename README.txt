MILOSLAVOV Ivan 260740074
AYRINHAC Theophile
Stalwart CPU

Instructions for initializing the required program:
	Load into the respective registers of RAM the bytes (within each reg, left-most byte goes into R_7, right-most into R_0)
		R_0:  01001111
		R_1:  00011100
		R_2:  00100100
		R_3:  01101111
		R_4:  01001110
		R_5:  01011101
		R_6:  00110100
		R_7:  01101110
		R_8:  01011110
		R_9:  00110100
		R_10: 11010000
		R_11: 01001111
		R_12: 00010000
		R_13: 00000001
		R_14: (RAM input,  you choose)
		R_15: 00000000

	Load into the CPU IC the nibble 0x8 (only right-most flip-flop on)

	Load into the CPU CU the first counter step (only first T FF on)

	Click the appropriate buttons on the input (supports 2 digits, with first one entered being multiplied by 16; entering 1-1 will place 17 into the input reg.)
----------
	If trying again without closing Logisim, make sure to zero everything else as well (especially RAM D and all of CPU)
—---------

Instructions Coding:
  
  BEQ : 10 [R1] [R2] [A] #4 variations of R
  
  BNQ : 11 [R1] [R2] [A] #4 variations of R
  
  LD  : 010 [R] [A] #2 variations of R
  
  STR : 011 [R] [A] #2 variations of R
  
  ADD : 0010 [R1] [R2] #4 variations of R
  
  SUB : 0011 [R1] [R2] #4 variations of R
  
  PRT : 00010 [R] #2 variations of R
  
  INP : 00011 [R] #2 variations of R
  
  STOP: 00000001

—————

Description of the components

	main: main interface to interact with the PC
	Button_Handler: Buffer responsible for loading the input from the buttons into the expected form
	Display_Handler: Buffer responsible for turning the output to the correct format and then displaying it
	System_Bus: Bus responsible for the proper connection between every components
	Stalwart_CPU: Maint interface of the CPU composed of the two registers, the ALU, the control Unit, the IC, and the registers for RAM intercation
	Control_Unit: circuit responsible for opening the correct gates depending on the instruction ben executed
	Binary_to_BCD: circuit for turning a binary input into BCD
	BCD_to_7Segments: circuit for turning a BCD input into a 7 segments output
	RAM_16x8: Main interface of the RAM, containing the registers for interaction with the CPU
	ALU: circuit responsible for executing math operations
	Byte_Register: register for a byte
	Nibble_register: register for a nibble
	Nibble_counter: specific register with an function for incrementing its content
	Bit_Adder: adder with carry and carry out
	Complement: circuit for 2’s complement
	


