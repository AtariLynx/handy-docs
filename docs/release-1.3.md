# HANDY release 1.3a

02-May-90  
Confidential and Proprietary

## Update notes

This document contains the following sections:

- Update notes
- Contents of Handy release 1.3a
- `6502:` changes
- `Handy:` changes
- What to do to use this release
- Tips and Gotchas

Since not too many files were changed in this update, and no files were removed, this is an incremental update (files that were not changed are not included).

The cartridge code and tools have been modified to support ROMs larger than 128K, and >128K ROM emulation on the Howard board (Remember to install more RAM). Support for >128K ROM emulation will be coming soon for the Mandy/Pinky systems.

A new version of the assembler and `Handyrom` are included with this release. I have fixed several bugs in the assembler, two of which could explain many of the mysterious guru's that people have been experiencing all the time. If you still have problems with the assembler, and can reproduce the conditions under which it fails, please get the information to me so that I have a chance to fix it.

Let me know if you have any problems, questions or comments.

Have fun,  
  Stephen H. Landrum  
  c/o Epyx, Inc.  
  600 Allerton Dr.  
  Redwood City, CA 94063  
  (415) 368-3200 x14

## Contents of Handy release 1.3a

This release contains this documentation and an Amiga disk (Quarterback V2.2 format).

The Quarterback disk "Handy Release 1.3a" contains updates for the `6502:` directory, updates for the `Handy:` directory, an update for the files `lxfast.src` and `lxfast.bin` in the `Sound:` directory, and the file `Release_1.3` (documentation for this release) in the top level directory.

## 6502: changes

The cartridge code has been modified to support cartridges larger than 128K, and in the process has been streamlined for size and speed. Many small errors were fixed in the HMusic and HSFX drivers, and some amount of optimization was also done. Programs  assembled with the new cartridge code can only be run from systems with the new `cartgo.bin` and `howardcartgo.bin` files in the `Handy:` directory.

The following changes were made to files in the `6502:` directory:

- `6502:examples/TestAll`  
  Fixed a bad line in the script file
- `6502:examples/testcart.src`  
  Moved `GETDIR_USER` definition before include files
- `6502:include/cart.i`  
  Changed `ROM_HEADER_SIZE` to 410
- `6502:include/harddefs.i`  
  Added `SUZY_BASE` and `MIKEY_BASE` definitions  
  Removed `SUZY_ADDRESS`  
  Corrected the `LEFT_ATTENMASK` and `RIGHT_ATTENMASK` definitions
- `6502:include/hmusic.i`  
  Added `HMUSIC_DEFAULT_PRIORITY` which now sets a default priority for voices in music, and can be optionally defined in the game code
- `6502:macros/cart.mac`  
  Removed `SETCARTOFFSET` macro  
  Changed register assignments for arguments to `OPENFILE`, `SETCARTADDR` and `SETCARTPAGE` macros  
  `GETDIR` and `GETDIRFAST` have been replaced by single routine called by both macros
- `6502:macros/hmusic.mac`  
  Neatened up source for readability
- `6502:macros/hsfx.mac`  
  Neatened up source for readability
- `6502:src/cart.src`  
  Code optimized for size and speed
  Register arguments to `OpenFile0`, `SetCartAddress` and `SetCartPage` routines changed  
  `GetDirAddress` routine rewritten, registers returned for `OPENFILE` macro  
  `GetDirAddressFast` removed
- `6502:src/hmusic.src`  
  `InitHMusic` initializes all variables
  Rests are now silent  
  `HMUSIC_DEFAULT_PRIORITY` support added  
  `PlayMusic` and `AddMusic` now kill any notes still being played by previous song  
  Code tightened
- `6502:src/hmusic.var`  
  `ring0_flag` and `ring1_flag` arrays replaced by `ring_flag` array  
  Added `HMusicVarStart` and `HMusicVarEnd` address markers
- `6502:src/hsfx.src`  
  Added `USER_HSFX` support, if defined then HSFX calls user supplied routine `user_hsfx` every audio frame  
  `InitHSFX` initializes all variables  
  `FreeAudio` zeroes DC bias on stopped channel  
  `AllocAudio` and `GrabAudio` routines only included if new switch `GRABAUDIO_USER` defined  
  Moved `top_of_loop0` and `top_of_loop1` variables to `hsfx.var`  
  Moved `hsfx_temp` to zero page  
  Code tightened
- `6502:src/hsfx.var`  
  Added `HSFXVarStart` and `HSFXVarEnd` address markers  
  Moved `hsfx_temp` to zero page in `hsfx.src`  
  Moved `top_of_loop0` and `top_of_loop1` variables from `hsfx.src`
- `6502:src/tunpack.src`  
  Updated for compatibility with new `cart.src`

## Handy: changes

The tools have been updated to support ROMs larger than 128K. Code assembled with the `6502:` update needs the new version of `HandyRom`, and needs `cartgo.bin` and `howardcartgo.bin` to be updated on the system to be run from. The files `cartgo.bin` and `howardcartgo.bin` (used by the `BOOT CART` command in `Mandebug` and `Handebug`) now also provide a closer simulation of the actual boot conditions provided in a final cartridge.

The following changes were made to files in the `Handy:` directory:

- `Handy:Asm` version 1.09  
  Several error fixes:  
  `.TF` no longer writes garbage to first file  
  `.SY` error causing potential memory trashing fixed  
  `#REP` error fixed  
  `guru` after certain error messages fixed  
  multiplies in expressions now 32x32 bits
- `Handy:HandyRom` version A0.07
  Multiplies in expressions now 32x32 bits  
  Default `ROM_HEADER_SIZE` now 410  
  Supports larger than 128K cartridge files
- `Handy:HSPL`  
  More recent version with some errors fixed
- `Handy:cartgo.bin`  
  Updated to support larger than 128K ROMs closer simulation of actual cartridge code initial state
- `Handy:howardcartgo.bin`  
  Updated for ROMs larger than 128K closer simulation of actual cartridge code initial state
- `Handy:hsfxedit.bin`  
  Reassembled with new version of HSFX

## What to do to use this release

- Change directory to the root of your hard disk  
  `cd DH0B:` ; (or whatever you call your hard disk)
- Delete (if it exists) the directory `6502.old`  
  `Delete 6502.old all`
- Make a new directory `6502.old`  
  `Makedir 6502.old`
- Copy the directory `6502` to `6502.old`  
  `Copy 6502 6502.old all`

Run Quarterback, and restore all of the files to your hard disk. If you keep `6502`, `Handy` or `Sound` on separate hard disks or partitions, you will have to selectively restore them separately to appropriate disks.

Edit your source files to update them (there should be few if any changes needed). The macro `SETCARTOFFSET` no longer exists. Code that used to use it needs to be rewritten.  If the routines `AllocAudio` or `GrabAudio` are used, `GRABAUDIO_USER` needs to be defined. `USER_HSFX` support has been added. If `USER_HSFX` is defined, then the HSFX driver will call the user supplied routine `user_hsfx` every audio frame.

The files `lxfast.src` and `lxfast.bin` in the `Sound:` directory have been fixed to work with Howard board systems (previously it only work on Mandy/Pinky systems). Copy the file `Sound:lxfast.bin` to the `vdk:` directory. 

The `PLAY` script should now work properly.

## Tips and Gotchas

Symbols in the assembler (and in `HandyRom`) have 32 bit values. This is true for the intermediate expressions in all calculations, with one exception. Division in the assembler is a 32/16 bit signed divide. For example, the following line:

```
label	.EQ 200000/100000
```

should set label to the value `2`, but instead sets it to `-6` (`$fffa`).

A good programming practice is to make sure that ALL variables are initialized to a known state before entering the main loop in your code. In my programs, I do this by zeroing all of my variables in a loop, and then setting the ones that should be non-zero. Almost every program developed for the Lynx has had at least one bug at some time due to uninitialized variables. These bugs can be very nasty to have to track down.
