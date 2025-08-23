# HANDY Release 0.6

28 December 1988  
Confidential and Proprietary

The dreaded Display-Buster release.

This document describes the V0.6 release of Handy development environment. It includes these sections:

- [`6502:` and `HANDY:` changes](#6502-and-handy-changes)
- [Handycraft V1.36](#handycraft-v136)
- [What you must do to start using this release](#what-you-must-do-to-start-using-this-release)

## `6502:` and `HANDY:` changes

There's lots of new display macros. It would be wasteful to try to itemize everything new and everything changed here. Instead, please erase your memory about Handy display creation and start anew with the [[[Creating Handy Display]]] chapter that's distributed with this release.

Please note at the end of the new [[[Creating Handy Display]]] chapter that there's an index. This is the first released chapter of the new [[[Handy Programmer's Guide]]]. The new Guide, which I'll be finishing during CES, will have a real index and table of content.

The files `6502:examples/testsprite.src` and `6502:examples/video.src` illustrate the steps required to create displays using the new body of macros. These files are tested on the emulator. `video.src` is tested on Handy hardware, while `testsprite.src` is not.

New release files:

`6502:macros/display.mac`  
`6502:macros/sprite.mac`  
`6502:include/harddefs.i`  
`6502:include/monitor.i`  
`6502:src/display.src`  
`6502:src/sprite.src`  
`6502:src/syszpage.src`  
`6502:examples/testsprite.src`  
`6502:examples/video.src`  
`HANDY:monitor.bin`

The variable `ptr` that's declared in `6502:src/sysdata.src` has been renamed to `sysptr`. If you were using `ptr`, should you be? I don't know ... you probably don't want to.

Here's a rule that was never published before but is as true as true can be: with all macros and system subroutines, don't presume that any registers are preserved, nor any of the flags `N`, `V`, `Z` or `C`.

New monitor code release, with some fine tuning (thanks to Chuck). Does nothing new in Apple (poof).

The `PSPRITE` macro has been deleted, and `SPRITES` now does what `PSPRITE` used to do, which is to render your sprites into the buffer but not invoke the `DISPLAY` macro. `SPRITES` now must be followed by `DISPLAY` or `DBUF_DISPLAY`.

I am distributing hardcopy of all the new files.

I am distributing an "I Survived The Display Buster Release" badge of honor.

What used to be `6502:src/sysdata.src` is now split into two files, `sysdata.src` and `syszpage.src`. Here's the rule: you must include `syszpage.src` before any code or macro usage. You must include `sysdata.src` before any other `6502:src` file. You must `.ORG` out of page zero before you include `sysdata.src`.

`6502:include/harddefs.i` has sprite control and audio channel definitions which it lacked before. Also, the file is rewritten so that all of the hardware registers will now appear as symbols in your assembly symbol tables.

All `6502:` files now have headers which will track changes henceforth.

As with all documentation that we release, please give it a critical read and provide feedback. We ask for this all the time, but you know RG's the only one who's ever really given feedback. And boy did he feed back, more and more and more, feeding back feeding back on and on and on until we thought, well, maybe some people could work off-site.

By the way, I have analyzed the assembler and I found two major bottle-necks: processing opcodes and accessing the file system. I can rewrite the opcode parsing routines, but speeding up the file system access time will require upgrading our Amigas to the 1.3 version of the operating system, which we won't do until after CES.

## Handycraft V1.36

*Handycraft* needs you to use the new display macros for the emulator to work correctly. Also, if you've been cheating at all with your sprites and display usage, it will probably need fixing now.

## What you must do to start using this release

1. Give me permission to install it on your machine.
2. Make honest displays.
