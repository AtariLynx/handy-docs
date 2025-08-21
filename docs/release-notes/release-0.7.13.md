HANDY Release 0.7.13

5 April 1989  
Confidential and Proprietary

This document describes the V0.7.13 release of the Handy development environment. It includes these sections:

- HandyASM V0.23
- Handycraft 1.55
- What You Must Do To Start Using This Release

## HandyAsm V0.23

`#IF`'s can be nested to 16 deep. Files can be `.IN`'cluded to 16 deep.

If you didn't know, you can have as many command-line switches per line as you want. This includes any number of `+D` declarations.

In preparation for accepting the V1.00 release of *HandyAsm*, I put together a test suite which tries all combinations of opcodes with operands. Found several little errors (some of which were also allowed by the old assembler!) which have now been fixed. Don't be surprised if this release tells you that you've been doing something naughty in your code.

Bug fixes:

- Local labels should work correctly, and should generate error messages correctly. This is in answer to many different bug reports
- These are no longer a valid instructions:
  ```
  STA #$10 
  LDX addr,X
  STX addr,X
  LDY addr,Y
  STY addr,Y
  ```
- Using `#value` as the argument to an equate is now flagged as an error
- Typos such as `<#29` should be trapped as errors now
- The nesting of `#IF` and `#ELSE` and `#ENDIF` should behave correctly

## Handycraft 1.55

One bug fix: palettes output is corrected (blue-red was screwed up in last release).

## What you must do to start using this release

The new assembler is now copied to both `HANDY:HandyAsm` and `HANDY:asm`. This means that with this release you start getting the new assembler as the default. If you want to assemble using the old assembler, first enter the CLI command `OldAsm`. After `OldAsm`, you can use NewAsm to start using the new assembler again.
