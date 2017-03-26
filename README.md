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
  
  STOP: 00000000  //READ MARCH 26

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

-------------------

Ivan, March 26:
  Here is the program we need to write
    LD D0, 0xE (LOOP) =>  Load counter
    LD D1, 0xD        =>  Load number 1
    SUB D0, D1        =>  counter -= 1
    SUB D1, D1        =>  D1 = 0
    BEQ D0, D1, 0xA   =>  if counter == 0, jump to END
    STR D0, 0xE       =>  Store counter
    LD D0, 0xF        =>  Load sum
    INP D1            =>  Load input
    ADD D0, D1        =>  sum+=input
    STR D0, 0xF       =>  Store sum
    BEQ D1, D1, 0x0   =>  Jump to LOOP
    LD D0, 0xF  (END) =>  Load sum
    PRT D0            =>  Display sum
    STOP              =>  STOP
    0x01              =>  1
    0x??              =>  The term to multiply
    0x00              =>  Space for sum

  This is 17 lines but my solution is this:
  Change the STOP code to 00000001. We can then use it both as instruction and as data!
  The final 16 bytes would then look like this:

  0x0:  01001110
  0x1:  01011101
  0x2:  001101xx
  0x3:  001111xx
  0x4:  10011010
  0x5:  01101110
  0x6:  01001111
  0x7:  000111xx
  0x8:  001001xx
  0x9:  01101111
  0xA:  10110000
  0xB:  01001111
  0xC:  000100xx
  0xD:  00000001
  0xE:  ????????
  0xF:  00000000

  Alternative solution: do-while loop starting at instruction 0x4, with RAM starting 1 above its value.
    LD D0, 0xF (LOOP) =>  Load sum
    INP D1            =>  Load input
    ADD D0, D1        =>  sum+=input
    STR D0, 0xF       =>  Store sum
    LD D0, 0xE        =>  Load counter
    LD D1, 0xD        =>  Load number 1
    SUB D0, D1        =>  counter -= 1
    STR D0, 0xE       =>  Store counter
    SUB D1, D1        =>  D1 = 0
    BNQ D0, D1, 0x0   =>  if counter != 0, jump to LOOP
    LD D0, 0xF        =>  Load sum
    PRT D0            =>  Display sum
    STOP              =>  STOP
    0x01              =>  1
    0x??              =>  The term to multiply
    0x00              =>  Space for sum