# HANDY release 1.1

27-Feb-90  
Confidential and Proprietary

## Update notes

It's been a while since there has been an official release, so a number of things are being covered.

This document contains the following sections:

- Update notes
- Contents of Handy release 1.1
- `6502:` changes
- `Handy:` changes
- What to do to use this release
- Amiga use tips

The `6502:` development directory has undergone some major overhaul. I've probably made some minor mistake somewhere, so please let me know if you have any problems.

Have fun,  
    Stephen H. Landrum  
    c/o Epyx, Inc.  
    600 Allerton Dr.  
    Redwood City, CA 94063  
    (415) 368-3200 x14

## Contents of Handy release 1.1

Handy release 1.1 contains this documentation, an update disk in Quarterback v2.2 format, and a new keyboard template. The update disk contains a complete replacement for the `6502` directory, updates to `Howdebug`, `Mandebug`, `handebug.config` and `asmstrip` in the `Handy` directory, new programs `FIND` and `UNDELETE` in the `C2` directory, and a copy of this document `Release_1.1` in the root directory.

## 6502: changes

The `examples` directory has been completely redone. See the file `6502:examples/readme` for a description of the current files.

The following files have been removed, and should no longer be referenced by your code:

- `6502:include/aio2defs.i`
- `6502:include/io32defs.i`
- `6502:include/sys.i`
- `6502:include/tapedeck.i`

- `6502:macros/tapedeck.mac`

- `6502:src/controls.src`
- `6502:src/handymath.src`
- `6502:src/sprite.src`
- `6502:src/tapedeck.src`

All files have had the old Apple IIe emulation system support removed. The emulation code was out of date and no longer worked. All references to `HANDYIO` and `HANDYMATH` have been removed since these were used to determine whether or not the code was going to run on the emulation system. In addition, the following changes were made to specific files:

- `6502:include/harddefs.i`  
  Added missing defines, corrected some to reflect current spec.
- `6502:macros/cart.mac`  
  Removed broken write-cart code.  
  Tightened code.
- `6502:macros/controls.mac`  
  Removed `GETINPUT` macro.
- `6502:macros/display.mac`  
  Tightened code.  
  Moved `TIMEOUT` macro from `display.src`
- `6502:macros/hprint.mac`  
  Tightened code.
- `6502:macros/sprite.mac`  
  Tightened code.
- `6502:macros/sys.mac`  
  Added `HBRK` macro.  
  `BRK_USER` support changed, now forces `BRK` on Howard board, unless redirected by `SET_BRK` macro.  
  `INITINT` now required before use of `INITEOF`, `INITEOL`, or `SET_DISPLAY...` macros.  
  Moved initialization of `MAPCTL` to `INITSYS`.
- `6502:src/cart.src`  
  Fixed bug in `GetDirAddressFast` routine.  
  Fixed error that caused carts to power off during reads.  
  Removed broken write-cart code.  
  Tightened code.
- `6502:src/display.src`  
  `TIMEOUT` macro moved to display.mac .
- `6502:src/sys.src`  
  INITINT now required before use of `INITEOF`, `INITEOL`, or `SET_DISPLAY...` macros.  
  Fixed references so this file can be included before   `INITINT` macro invoked.  
  Tightened code.

A new file `6502:src/tunpack.src` has been added. This code is used to unpack ROM files packed with the `TOPACK` utility. Full documentation for this utility will come with a later release. Several programs have used this unpacker even though it hasn't been an official part of the system. The version with this release fixes a bug in the cartridge read routines.

## Handy: changes

New versions of `Howdebug` and `Mandebug` (Howard/Howdie and Mandy/Pinky versions of `Handebug`) are being included. `Howdebug` has a fix in it that in conjunction with a hardware fix prevents the Howard board from getting lost when an IRQ and NMI happen simultaneously. `Howdebug` and `Handebug` have some additional settings in `handy:handebug.config` file.  `PORTLOOP` and `TIMEOUT` are two constants used for timing during communication. On a standard Amiga 2000, they should be set to 50 and 1 respectively. On a 2500 with a 68020 accelerator board, they should be set to 200 and 1. Faster systems in the
future with 68030s or 68040s or other hardware modifications may require some other settings. A new copy of `handebug.config` is included.

The last release may have had a problem with `ASMSTRIP`, so a more recent version is being included.

## What to do to use this release

- Change directory to the root of your hard disk   
  `cd DH0B:`	; (or whatever you call your hard disk)
- Delete (if it exists) the directory `6502.old`  
  `Delete 6502.old all`
- Rename the directory `6502` to `6502.old`  
  `Rename 6502 6502.old`
- Create a new directory `6502`  
  `Makedir 6502`
- Assign `6502:` to `6502`  
  `Assign 6502: 6502`

If you like your current `handebug.config` settings, copy the file

```
Copy Handy:handebug.config Handy:handebug.old.config
```

and after updating your system, you can reference it to restore your settings to the way you like.

Run Quarterback, and restore all of the files to your hard disk. If you keep `6502`, `C2` and `Handy` on separate hard disks or partitions, you will have to selectively restore them separately to appropriate disks.

Copy `Howdebug` or `Mandebug` (depending on whether you use a Howard board or a Mandy/Pinky setup) over `Handebug`  

```
Copy Handy:Howdebug Handy:Handebug
- or -
Copy Handy:Mandebug Handy:Handebug
```

Edit your source files to update them.  Remove (delete or comment out) any lines that include files that have been removed from `6502:`. The constants `HANDYIO` and `HANDYMATH` no longer need to be defined, and should be removed from your code. The `GETINPUT` macro no longer exists, and
references to it should be removed from your code. The macro `HBRK` has been added. `HBRK` drops a `$13` byte into your code, which causes the Howard board to break immediately upon execution, but is otherwise a one-byte one-cycle
NOP for the 65C02.

## Amiga tips

Wildcards on the Amiga are non-standard, and unfortunately also vary a little bit from program to program. Most Amiga programs accept `?` to stand for any single character, and `#?` to stand for any number of any characters.

Some programs (`Copy` and `Delete` for example) have a more general regular expression parser. The construct `(exp1|exp2|exp3...)` will match an instance of `exp1` or `exp2` or `exp3`. For example: `Delete #?.(bin|rom|sym)` means delete any file whose name ends in `.bin`, `.rom`, or `.sym`.

The `rename` command can be used to move a file from one directory to another, but does not accept wild cards. The alias `REN` has been provided on your system to allow you to move a group of files from one directory to another.   

For example:

```
Ren #?.bin DH0B:temp
```

will move all files in the current directory ending in `.bin` to the directory `DH0B:temp`. Files moved this way must remain on the same disk drive that they started on.

Unfortunately, command line options for many programs on the Amiga take the form of words. You should avoid giving files names like `TO`, `FROM`, `ALL`, `QUIET`, `DRIVE`, `NAME`, `QUICK`, `OPT`, etc.

`ALL` is a useful keyword when copying or removing files.  For example:

```
Copy dir1 dir2 all
```

will copy all files and all subdirectories recursively from `dir1` to `dir2`.

```
Delete temp all
```

will delete all files in directory `temp` as well as all subdirectories and their contents.

Most Amiga supplied programs will prompt with a list of acceptable arguments if you supply a single argument of a single question mark. For
example: `Rename ?` will prompt: `FROM/A,TO=AS/A:` which means that `rename` requires the arguments `FROM` and `TO`, which can
optionally be preceded by the words `FROM` and `TO`, and the word `AS` can be substituted for `TO`. `/A` means that the argument is required, `/S` means that the argument is a switch and can only be given by name, `/K` means that the
argument if given must be preceded by its name, `=` between two words means that either may be used.

The useful programs `FIND` and `UNDELETE` have been included for this release in the directory `C2`. `FIND` will search for a specified file or group of files. For instructions, just type `FIND` with no arguments. `UNDELETE` can be used to recover a file after it is deleted. To use `UNDELETE` you have to know the name of the file you are looking for, but `UNDELETE` does not care which directory the file might have been in. `UNDELETE` will search the entire disk for the named file, and may find more than one copy. `UNDELETE` requires that you have another disk to restore the file(s) to.
