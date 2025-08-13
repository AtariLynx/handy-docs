# HANDY release 0.5.1 

16 November 1988  
Confidential and Proprietary

This document describes the V0.5.1 bug fix release of Handy development environment. It includes these sections:

- [*Handebug* V1.34](#handebug-v134)
- [*Handycraft* V1.29](#handycraft-v129)
- [Revisions to `6502:` Directory](#revisions-to-6502-directory)
- [What you must do to start using this release](#what-you-must-do-to-start-using-this-release)

## *Handebug* V1.34

Bug fixes:
- Low memory isn't trashed
- Structure display updated correctly

## *Handycraft* V1.29

Bug fixes:
- Sprites might appear for you now. I dunno, maybe

## Revisions to `6502:` Directory

Handy math macros are slightly modified. Now, you setup up all your registers, writing `MATHA` or `MATHE` last (which on real hardware will trigger the math operation) and then use the `MULTIPLY` or `DIVIDE` macro depending on which you want to do. On real hardware, these macros will do nothing as the act of writing to the math triger register starts the math for you. In the emulator, these macros do a `JSR` to the appropriate routine.

To use the math you must do this: 

- Call `RESETMATH` to initialize the registers
- If you're doing a multiply
  - If you want to accumulate the results of multiplies, you can either preset the accumulator or use the `CLEARACCUM` macro to reset the accumulator
  - Set any of the registers, and set `MATHA` last 
  - `MULTIPLY`
- If you're doing a divide
  - Set any of the registers, and set `MATHE` last 
  - `DIVIDE`
- You might want to call `WAITMATH` to wait for the results, or you might want to go off and do other stuff while the math hardware churns

`6502:examples/testmath.src` reflects the above changes.

## What you must do to start using this release

If you have started using the math macros, you'll need to make the above changes.
