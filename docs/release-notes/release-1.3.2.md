# HANDY release 1.3.2

17-May-90  
Confidential and Proprietary

## Update notes

This document contains the following sections:

- [Update notes](#update-notes)
- [Contents of Handy release 1.3.2](#contents-of-handy-release-132)
- [Assembler changes](#assembler-changes)
- [What to do to use this release](#what-to-do-to-use-this-release)

This is a small incremental release. Some minor features were added to the assembler, and a new set of documentation for the assembler has been typed up.

As always, let me know if you have any problems or questions.

Have fun,  
  Stephen H. Landrum  
  c/o Epyx, Inc.  
  600 Allerton Dr.  
  Redwood City, CA 94063  
  (415) 368-3200 x14

## Contents of Handy release 1.3.2

This release contains this documentation, a copy of the assembler documentation, and an Amiga disk (Quarterback v2.2 format).

The Quarterback disk "Handy Release 1.3.2" contains an update for `Asm` (version 1.10) in the `Handy:` directory, and the files `Release_1.3.2` (a copy of this document) and `HandyAsm.doc` (the assembler documentation) in the top level directory.

## Assembler changes

The following changes have been made to version 1.10 of the `HandyAsm` assembler:

- The construct `?#` has been added to the macro expansions. `?#` is replaced by two decimal digits representing the number of arguments passed to the macro.
- Label definitions can now end with a colon (`:`). The colon is not part of the label name.
- The `.LIST ON` and `.LIST OFF` lines now show up in the listing file.
- The `.PAGE` command no longer ejects a page if arguments are passed to set the page height and (optionally) width.
- The value of the label `ASM.VERSION` (pre-defined by the assembler) is now computed correctly. `ASM.VERSION` is defined to be the assembler version number * `256` + revision number. The current version (1.10) computes as `$10a`. Previously, `ASM.VERSION` was computed as the sum of the version and revision numbers multiplied by `256`.

## What to do to use this release

Run Quarterback, and restore all of the files to your hard disk.

No changes to source files should be necessary.
