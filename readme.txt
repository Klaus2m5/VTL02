-----------------------------------------------------
             VTL-2 for the 6502 (VTL02B)             
           Original Altair 680b version by           
          Frank McCoy and Gary Shannon 1977          
    2012: Adapted to the 6502 by Michael T. Barry
        see source code for copyright notice    
 Thanks to sbprojects.com for a very nice assembler! 
-----------------------------------------------------
 2015: Revision B, with several space optimizations
   (suggested by dclxvi) and enhancements (suggested
   by mkl0815 and Klaus2m5).

 The basic concepts of VTL-2 (Very Tiny Language):
 http://www.altair680kit.com/manuals/Altair_680-VTL-2%20Manual-05-Beta_1-Searchable.pdf

 The files:
   VTL02B for the apple II & the sbprojects.com assembler:
      vtl02ba2.asm
   VTL02B for the Kowalski 6502 simulator 
   http://www.exifpro.com/downloads/6502_1.2.12.zip:
      vtl02b_for_Kowalski.asm
 
 New features in Revision B:
 * Bit-wise operators & | ^ (and, or, xor)
   Example:  A=$|128) Get a char and set hi-bit

 * Absolute addressed 8-bit memory load and store
   via the {< @} facility:
   Example:  <=P) Point to the I/O port at P
             @=@&254^128) Clear low-bit & flip hi-bit

 * The space character is no longer a valid user
     variable nor a "valid" binary operator.  It is
     now only significant as a numeric constant
     terminator, and as a place-holder in strings and
     program listings, where it may be used to improve
     human readability, at a slight cost in execution
     speed and memory consumption.
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
