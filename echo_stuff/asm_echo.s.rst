
.. Copyright (c) 2013 Bryan A. Jones, Robert B. Reese, and J. W. Bruce ("AUTHORS")
   All rights reserved.
   (B. A. Jones, bjones_AT_ece.msstate.edu, Mississippi State University)
   (R. Reese, reese_AT_ece.msstate.edu, Mississippi State University)
   (J. W. Bruce, jwbruce_AT_ece.msstate.edu, Mississippi State University)

   Permission to use, copy, modify, and distribute this software and its
   documentation for any purpose, without fee, and without written agreement
   is hereby granted, provided that the above copyright notice, the following
   two paragraphs and the authors appear in all copies of this software.

   IN NO EVENT SHALL THE "AUTHORS" BE LIABLE TO ANY PARTY FOR DIRECT,
   INDIRECT, SPECIAL, INCIDENTAL, OR CONSEQUENTIAL DAMAGES ARISING OUT OF THE
   USE OF THIS SOFTWARE AND ITS DOCUMENTATION, EVEN IF THE "AUTHORS" HAS BEEN
   ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

   THE "AUTHORS" SPECIFICALLY DISCLAIMS ANY WARRANTIES, INCLUDING, BUT NOT
   LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A
   PARTICULAR PURPOSE.  THE SOFTWARE PROVIDED HEREUNDER IS ON AN "AS IS"
   BASIS, AND THE "AUTHORS" HAS NO OBLIGATION TO PROVIDE MAINTENANCE, SUPPORT,
   UPDATES, ENHANCEMENTS, OR MODIFICATIONS.

   Please maintain this header in its entirety when copying/modifying these
   files.

.. highlight:: nasm

**************************************************
asm_echo.s -- Demonstrates calling C from assembly
**************************************************
Demonstrates how to call C functions from assembly lanaguage.
Implements a UART1 character echo, and calls the configBasic, inChar, and outChar functions.

::

 ; wokifvzohtdlm
 
 ; wokifvzohtdlm

Include processor-specific headers

::

 ; wokifvzohtdlm
 .include "xc.inc"
 
 
 ; wokifvzohtdlm

Data section
============
This directive causes the assember to place the following statements in PSV space (constant memory).

::

 ; wokifvzohtdlm
            .section psv psv
 ; wokifvzohtdlm

Define a null-terminated ASCII string.

::

 ; wokifvzohtdlm
 HELLO_MSG: .asciz "asm_echo.s ready!\n"
 
 
 ; wokifvzohtdlm

Code Section
============
This directive causes the assembler to place the following statements in program memory.

::

 ; wokifvzohtdlm
 .text
 
 ;; int main() {
 ; wokifvzohtdlm

Make ``main`` visible outside this file so that *C* startup code can call it.

::

 ; wokifvzohtdlm
 .global _main
 _main:
 
   ;; char c;
   ;;
   ;;               W0
   ;; configBasic(HELLO_MSG)
   mov #HELLO_MSG, W0
   call _configBasic
 
   ;; while (1) {
   while_top:
 
     ;; W0
     ;; c  = inChar();
     call _inChar
 
     ;; c++;
     inc W0, W0
 
     ;;         W0
     ;; outChar(c);
     call _outChar
 
   bra while_top
   ;; }
