# HANDY release 1.3.4a

18-Oct-90  
Confidential and Proprietary

## Update notes

This document contains the following sections:

- [Update notes](#update-notes)
- [Contents of Handy release 1.3.4a](#contents-of-handy-release-134a) 
- [`Handy:` changes](#handy-changes)
- [`6502:` changes](#6502-changes)
- [*TileRead* tile reader program](#tileread-tile-reader-program)
- [What to do to use this release](#what-to-do-to-use-this-release)

This is a small incremental release. Some minor changes were made to the assembler and some optimization features were added to Redeye. A couple of new useful utilities (*TileRead* and *RomSplit*) are also included.

This will be the last update note that I will be putting out, since Friday October 19 is my last day at Epyx. I am leaving to work on (hopefully) bigger and better things.

Comments and Questions can be directed to Atari Sunnyvale or to Craig Nelson at Epyx.

Have fun,  
Stephen H. Landrum

## Contents of Handy release 1.3.4a

This release contains this documentation, an updated copy of the Redeye documentation, an updated document on Handy music generation, and an Amiga disk (Quarterback v2.2 format) .

The Quarterback disk "Handy Release 1.3.4a" contains an update for *Asm* (version 1.12) and the new program *RomSplit* in the `Handy:` directory, updates for some files in the `6502:` subdirectories, the files `Release_1.3.4` (a copy of this document) and `Redeye.doc` (an updated document with some corrections) in the top level directory, and a new subdirectory *TileRead* with Chuck Sommerville's tile reader program, source and (very brief) documentation.

## `Handy:` changes

Version 1.12 of *Asm* has had some minor bug fixes and changes. A bug has been fixed in macro argument expansions. A change has also been made so +hat macro arguments can now be passed with an embedded space or tab character. The maximum number of include files has been increased from 400 to 2500 (yes, somebody couldn't live with ONLY 400 include files).

The program *RomSplit* is used to split a 256K or 512K Lynx ROM file to 2 or 4 128K files that can be used to make EPROMs for use in Mandy/Pinky setups. To split a 256K file type:

```
ROMSPLIT <infile> <outfilel> <outfile2>
```

where `<infile>` is the 256K file you want to split, and `outfilel` and `outfile2` are where the low and high parts of the split file are to be written. To split a 512K file type:

```
ROMSPLIT <infile> <outfile1> <outfile2> <outfile3> <outfile4>
```

where `infile` is the 512K file you want to split up, and `outfile1-4` are where the split pieces are to be written.

## `6502:` changes

Support for some new features were added to Redeye. Read the updated Redeye documentation for details.

A run of the Mikey chips for the Lynx had a bug in it. The chips are not supposed to be used, but just in case they are I have a workaround for the `WAITSUZY` macro. Defining the switch `NO_RICOH_PATCH` disables the workaround.

Some errors in the comments in some of the files were corrected.

The following changes were made to files in the `6502:` directory: 

- `6502:examples/redeye_glue.src`  
  Minor code improvements  
  Added `SPLIT_SEND_RECEIVE` support
- `6502:examples/testredeye.src`  
  Added `SPLIT_SEND_RECEIVE`  
  Added `REDEYE_SLOWNESS`
- `6502:include/msgmgr.i`  
  Added `REDEYE_SLOWNESS` support
- `6502:macros/sprite.mac`  
  Added code to work around RICOH bug  
  Added `NO_RICOH_PATCH` override switch support
- `6502:src/comlink.src`  
  Debug code modifications
- `6502:src/comlink_logon.src`
  Added `REDEYE_SLOWNESS` support
- `6502:src/display.src`  
  Added code to work around RICOH bug  
  Added `NO_RICOH PATCH` override switch support
- `6502:src/hmusic.src`  
  Comment corrected (no code difference)
- `6502:src/hsfx.src`  
  Comment corrected (no code difference)
- `6502:src/hprint.src`  
  Added `HprFontLo` and `HprFontHi` labels
- `6502:src/msgmgr.src`  
  Added `REDEYE_SLOWNESS` support
- `6502:src/sys.src`  
  Comment corrected (no code difference)

## *TileRead* tile reader program

Chuck Sommerville's *TileRead* program is used to read a *DPaint* file and create an array of tile numbers for inclusion in your game source. The files `TileRead` (the executable program) , `TileRead.c` (C source) , `TileRead.doc` (Chuck's very brief documentation) and `Example.pic` (a sample *DPaint* picture) have been included in the `TileRead` directory.

## What to do to use this release

Run *Quarterback*, and restore all of the files to your hard disk. If you keep `6502` or `Handy` on separate hard disks or partitions, you will have to selectively restore them separately to appropriate disks.

The *TileRead* program is in its own directory, and if you want it to be in your command path you should copy it to the `Handy:` directory.

There should be no source code changes required.
