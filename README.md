# COMP273_final  
Ivan, March 11:  

  I've thought about instructions length: you know we're fucked, right?  
  Unless the teacher changes the instructions, we have a single byte per instruction which means we will have to have variable length OP-codes.
  We have to use Kraft's ineq. to find codes that fit and can be distinguished.
  As such, here are the proposed codes:
  
  BEQ : 10 [R1] [R2] [A] #4 variations of R
  
  BNQ : 11 [R1] [R2] [A] #4 variations of R
  
  LD  : 010 [R] [A] #2 variations of R
  
  STR : 011 [R] [A] #2 variations of R
  
  ADD : 0010 [R1] [R2] #4 variations of R
  
  SUB : 0011 [R1] [R2] #4 variations of R
  
  PRT : 00010 [R] #2 variations of R
  
  INP : 00011 [R] #2 variations of R
  
  STOP: 00000000

  Sum: 25, 12 of which require to fit 4-bit address -> 4-bit OP-code

-------------------

Ivan, March 18:

  As explained in a lecture on control units, we're going to use the first two bits of op-codes to carry the information of "addressed instruction".
  Luckily, the plan already uses that already: the first two bits being 00 are the only ones not using addresses.
  However, whoever designs the ALU should consider changing the code/order of things for direct commands, perhaps use the ALU func code at the end as in lecture.

-------------------

Ivan, March 25:
  
  CU steps:
  BEQ: 
    R2 to L; R1 to R; ALU Sub EXEC; if Zero rewrite A
  BNQ:
    R2 to L; R1 to R; ALU Sub EXEC; if !Zero rewrite A
  LD:
    Send to RAM (Read at A); EXEC RAM; D to R
  STR:
    Send to RAM (Write R at A)
  ADD:
    R2 to L; R1 to R; ALU Add EXEC; D to R1
  SUB:
    R2 to L; R1 to R; ALU Sub EXEC; D to R1
  PRT:
    Send to OUT (Enabled, R)
  INP:
    IN to R (IE as control)
  STOP:
    ??

  Before each:
    1: IC to MAR
    2: Send to RAM (Read at MAR)
    3: Exec RAM (and INC IC)
    4: RAM D to MBR
    5: MBR to IR

  Swapped R2 and R1 in all instructions due to 2s comp on L not on R