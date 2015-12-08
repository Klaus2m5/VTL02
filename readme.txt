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

-----------------------------------------------------
 VTL02sg for the 2m5 emulated 6502 SBC

   spaces in expressions are allowed on input but are 
     removed from the stored program and listing.

   added a timer variable {/} with 10ms increments.

   the {?} input variable no longer accepts an
     expression as input. Only a number is accepted. 

   added braces as shift operators.
     A}B shifts A by B bits to the right.
     A{B shifts A by B bits to the left.
     result is unpredictable if B > 16

   an expression missing the initial {=} operator
     is converted by duplicating the leftmost variable
     and inserting a {=}. {N+1} becomes {N=N+1}.

   added a statement delimiter {;} allowing multi
     statement lines.
     branch to same line is now allowed.
     {?="..."} & unmatched {)} (used for comments) can
     not be continued. 

   added load and save facility to user call {>}
     "=0;>=13  loads program 13 from EEPROM
     "=1;>=42  saves current program to EEPROM as 42
     requires emulator version >= 0.83c 

   line numbers >= 65280 are now reserved for the
     following fast return & goto features.
   added a gosub stack, depth = 16 address words.
     {==...} is a gosub and pushes the return address
     of the next line.
     {#==} is a return and pops the address when the
     result is the special line numer asigned to {=}.
   added a 31 line addresses acronym label array.
     lowercase characters and symbols in the $60-$7e
     range are used to address the array. the array
     is populated with the address of a line when a
     character in the allowed range preceeds the line
     number.

   example (prints the first 1000 prime numbers):
       10 /=0;Q=d;V=5;U=25;X=1000
       20 N=2;==b
       30 N+1;==b
       40 N+2;==b
      a100 N+2;==b
       120 N+4;==b
       150 #=a
      b200 #=N<U[Q;Q=c;V+2;U=V*V
      c300 D=5
      e310 A=N/D;#=%]=;D+2;#=D>V[d
       320 A=N/D;#=%]=;D+4;#=D<V[e
      d400 ?=N;?=""
       420 X=X-1;#=X[=
       435 ?="Execution time: ";
       445 ?=//100;$=46;#=%>10[465;?=0
       465 ?=%;?=" seconds"

   added message service including error messages
     runtime errors:
       233 EEPROM file corrupted
       234 EEPROM file has incompatible format
       237 EEPROM not responding
       238 EEPROM full - file not saved
       239 EEPROM file not found
       240 array pointer exceeds reserved VTL RAM
       241 user call pointer inside reserved VTL RAM
       248 duplicate label
       249 undefined label or empty return stack
     errors during program line input:
       242 invalid or missing operator
       243 invalid or missing target variable 
       244 value or variable missing after operator
       245 missing closing parenthesis
       246 out of memory (*-&)

 internal changes:
   added required atomic variable fetch & store.
   replaced some jsr calls with inline code
     for skpbyte:, getbyte:, plus:, minus:.
   replaced cvbin calls to mul: & plus: with custom
     inline multiply by 10 & digit adder.
   removed simulation from startup of eval:.
   mainloop uses inline code to advance to next
     sequential program line.
     find: is now only used for true branches.
   added decimal to binary conversion on line entry
     avoiding the runtime conversion.
   abbreviated getting a simple variable in getval:.
   bypassed setting a simple variable in exec:.
   added inline divide by 10 to prnum:.
   fixed statement delimiter not overriding mismatched
     parentheses.
   merged oper: into getval: and progr: into exec:
   added a check for ctrl-c & ctrl-z during goto to
     allow user escape from a loop.

