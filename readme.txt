-----------------------------------------------------
             VTL-2 for the 6502 (VTL02C)             
           Original Altair 680b version by           
          Frank McCoy and Gary Shannon 1977          
    2012: Adapted to the 6502 by Michael T. Barry
        see source code for copyright notice    
 Thanks to sbprojects.com for a very nice assembler! 
-----------------------------------------------------

 The basic concepts of VTL-2 (Very Tiny Language):
 http://www.altair680kit.com/manuals/Altair_680-VTL-2%20Manual-05-Beta_1-Searchable.pdf

 The files:
   Original source code from Mike:
     VTL02C for the apple II & the sbprojects.com assembler
        vtl02ca2.asm
   Modified versions (Syntax, I/O & user RAM size):
     VTL02C for the Kowalski 6502 simulator 
     http://www.exifpro.com/downloads/6502_1.2.12.zip
        vtl02ca2.65s
     VTL02C for my emulator & the Kingswood AS65 assembler
        vtl02ca2.a65
     VTL02C codename "Speedy Gonzales"
        vtl02sg.a65     
        this is a work in progress version and is my attempt
        to trade performance versus codesize

  2015: Revision B included some space optimizations
         (suggested by dclxvi) and enhancements
         (suggested by mkl0815 and Klaus2m5):

 * Bit-wise operators & | ^ (and, or, xor)
   Example:  A=$|128) Get a char and set hi-bit

 * Absolute addressed 8-bit memory load and store
   via the {< @} facility:
   Example:  <=P) Point to the I/O port at P
             @=@&254^128) Clear low-bit & flip hi-bit

 * Starting with VTL02B, the space character is no
     longer a valid user variable nor a "valid" binary
     operator.  It's now only significant as a numeric
     constant terminator and as a place-holder in
     strings and program listings, where it may be
     used to improve human readability (at a slight
     cost in execution speed and memory consumption).
   Example:
   *              (VTL-2)
       1000 A=1)         Init loop index
       1010 ?=A)           Print index
       1020 ?="")          Newline
       1030 A=A+1)         Update index
       1040 #=A<10*1010) Loop until done

   *              (VTL02B)
       1000 A = 1             ) Init loop index
       1010     ? = A         )   Print index
       1020     ? = ""        )   Newline
       1030     A = A + 1     )   Update index
       1040 # = A < 10 * 1010 ) Loop until done

 2015: Revision C includes further enhancements
   (suggested by Klaus2m5):

 * "THEN" and "ELSE" operators [ ]
     A[B returns 0 if A is 0, otherwise returns B.
     A]B returns B if A is 0, otherwise returns 0.

 * Some effort was made to balance interpreter code
     density with interpreter performance, while
     remaining within the 1KB constraint.  Structured
     programming principles remained at low priority.
