# HANDY release 1.3.3

28-Jun-90  
Confidential and Proprietary

## Update notes

This document contains the following sections:

- [Update notes](#update-notes)
- [Contents of Handy release 1.3.3](#contents-of-handy-release-133)
- [Assembler changes](#assembler-changes)
- [`6502:` changes](#6502-changes)
- [Cartridge boot](#cartridge-boot)
- [New RAM cartridge macros](#new-ram-cartridge-macros)
- [What to do to use this release](#what-to-do-to-use-this-release)

This is a small incremental release. A minor bug was fixed in the assembler, and support for battery backed up RAM cartridges was added to the system code.

Coming up to CES, as several games were made into cartridges to be demo'ed, it became obvious that a good description of the title screen file for cartridge boot was never released. A note on how to get your game to properly boot with a title screen has been included with this release.

As always, let me know if you have any problems or questions.

Have fun,
  Stephen H. Landrum  
  c/o Epyx, Inc.  
  600 Allerton Dr.  
  Redwood City, CA 94063  
  (415) 368-3200 x14

## Contents of Handy release 1.3.3

This release contains this documentation, an updated copy of the assembler documentation, and an Amiga disk (Quarterback v2.2 format).

The Quarterback disk "Handy Release 1.3.3" contains an update for `Asm` (version 1.11) in the `Handy:` directory, updates for some files in the `6502:src` and `6502:macros` directories, and the files `Release_1.3.3` (a copy of this document) and `HandyAsm.doc` (the assembler documentation) in the top level directory.

## Assembler changes

The assembler had a bug if the user specified an output buffer size of larger than 72K (the default), and used the `.PC` or `.PD` directives. The symptom of the bug was that the files included with `.PC` or `.PD` would not be included sometimes. The bug has been corrected in version 1.11 of the assembler.

In addition several typos were corrected in the documentation for the assembler. The most notable errors in the document were the omission of the description of the `.SY` (include symbol file) directive, and the `.PC` and `.PD` directives had their descriptions swapped.

## `6502:` changes

`RAMCART_USER` support has been added to the system code. Macros have been added to enable reading and writing to the battery backed up RAM cartridge.

The following changes were made to files in the `6502:` directory:

- `6502:include/harddefs.i`  
  Removed the `MAGxx` names for timers  
  Added `READ_ENABLE` bit definition for `IODAT`
- `6502:macros/cart.mac`  
  Added `RAMCART_USER` support  
  Added `READRAMCART` and `WRITERAMCART` macros
- `6502:macros/sys.mac`  
  Added `RAMCART_USER` support
- `6502:src/cart.src`  
  Added `RAMCART_USER` support

## Cartridge Boot

The first "file" (file `0`) in the Lynx ROM is expected to be a Title screen (to give the user something to look at while the cartridge is booting). The file consists of 32 bytes of palette information (which will be copied directly to the hardware registers in order, the first byte going to `GREEN0`) followed by a sprite SCB. The sprite SCB can (and probably will) be the first SCB in a chain that will direct Suzy on how to construct the
title screen. The title screen should cover the entire screen frequently done by making the first sprite in the list one which erases the whole screen). The title screen will be displayed until control is handed off to the game code or until the user presses the A or B button. If the user presses the A or B button, the screen will be left in memory intact, but the colors of the palette will all be set to `$00` (black). The title screen file must be loaded to memory starting at or after location `$2400` and must end before location `$FC00`. The title screen will be constructed and displayed at location `$0400`.

The second file (file `1`) is the initial game code. This file will be loaded and then branched to (at its load address) after the cartridge security has been verified. This file must load starting at or after location `$0400` and must end before location `$FC00`.

The title screen can be tested in development (assuming that the system has been updated with Handy Release 1.3a) by using the `CART LOAD` and `BOOT CART` functions of `Handebug`. The title screen should display for a few seconds before jumping to the game.

## New RAM cartridge macros

If you are developing a game that uses the battery backed up RAM cartridge, define the switch `RAMCART_USER` in your source code before including the system includes.The macros `READRAMCART` and `WRITERAMCART` have been added to `cart.mac` to make it easier to access the RAM in the cartridge. 

The following descriptions of the macros are copied from the comments in the file `cart.mac` in the `6502:macros` directory:

```
* The READRAMCART macro is used to read in one or more pages from 8K of
* battery backed up RAM
*
* Regardless of the ROM size, the RAM is organized as 32 pages of 256 bytes
* each
*
* On entry:
*	X,Y = (low,high) Address in memory to read data into
*	A   = number of pages to be read in
* NOTE: Before calling READRAMCART, the SETCARTPAGE macro should be used to
*  set the hardware to the page to start reading from
*
* On exit:
*	A,X,Y are trashed
*	Cart power is disabled
*	The cartridge is NOT set up for subsequent reads or writes
*
* NOTE: This macro generates a significant amount of code, and if used
*  several times, a subroutine that calls this macro should be considered

* The WRITERAMCART macro is used to write one or more pages to 8K of
* battery backed up RAM
*
* Regardless of the ROM size, the RAM is organized as 32 pages of 256 bytes
* each
*
* On entry:
*	X,Y = (low,high) Address in memory of data to write to cartridge
*	A   = number of pages to be written
* NOTE: Before calling WRITERAMCART, the SETCARTPAGE macro should be used to
*  set the hardware to the page to start writing to
*
* On exit:
*	A,X,Y are trashed
*	Cart power is disabled
*	The cartridge is NOT set up for subsequent reads or writes
*
* NOTE: This macro generates a significant amount of code, and if used several
*  times, a subroutine that calls this macro should be considered
```

## What to do to use this release

Run Quarterback, and restore all of the files to your hard disk. If you keep `6502` or `Handy` on separate hard disks or partitions, you will have to selectively restore them separately to appropriate disks.

There should be no source code changes required.
