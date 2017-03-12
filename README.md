# COMP273_final  
Ivan, March 11:  

  I've thought about instructions length: you know we're fucked, right?  
  Unless the teacher changes the instructions, we have a single byte per instruction which means we will have to have variable length OP-codes.
  We have to use Kraft's ineq. to find codes that fit and can be distinguished.
  As such, here are the proposed codes:
  
  BEQ : 10 [R1] [R2] [A]
  
  BNQ : 11 [R1] [R2] [A]
  
  LD  : 010 [R] [A]
  
  STR : 011 [R] [A]
  
  ADD : 0010 [R1] [R2]
  
  SUB : 0011 [R1] [R2]
  
  PRT : 00010 [R]
  
  INP : 00011 [R]
  
  STOP: 00000000
