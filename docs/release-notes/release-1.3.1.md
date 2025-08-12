# HANDY release 1.3.1

04-May-90  
Confidential and Proprietary

## Update notes

This document contains the following sections:

- Update notes
- Contents of Handy release 1.3.1
- `6502:` changes
- `Handy:` changes
- What to do to use this release

This is a small incremental release.  Some minor adjustments were made to HSFX (to minimize speaker "clicks" between different notes or sound effects). `Cartload.bin` has been updated to be able to handle ROM emulation of larger than 128K on the Mandy/Pinky systems.

As always, let me know if you have any problems or questions.

Have fun,  
  Stephen H. Landrum  
  c/o Epyx, Inc.  
  600 Allerton Dr.  
  Redwood City, CA 94063  
  (415) 368-3200 x14

## Contents of Handy release 1.3.1

This release contains this documentation and an Amiga disk (Quarterback V2.2 format).

The Quarterback disk "Handy Release 1.3.1" contains the file `hsfx.src` in the `6502:src` directory, the files `cartload.bin` and `hsfxedit.bin` in the `Handy:` directory, the file `lxfast.bin` in the `Sound:` directory, and the file `Release_1.3.1` in the top level directory.

## 6502: changes

The HSFX driver has been modified to improve speaker "clicks" between notes or sound effects. If a sound being terminated was using the integrate mode of the hardware, the volume (DC bias) on the channel is returned to zero one volume increment each audio frame.

The following changes were made to files in the `6502:` directory:

- `6502:src/hsfx.src`  
  See above note

## Handy: changes

The file `cartload.bin` (used by the `LOAD CART` command in the Mandy/Pinky version of `Handebug`) has been updated to support ROM emulation of larger than 128K carts.

The following changes were made to files in the `Handy:` directory:

- `Handy:cartload.bin`  
  Supports >128K cart code
- `Handy:hsfxedit.bin`  
  Reassembled with new version of HSFX

## What to do to use this release

Run Quarterback, and restore all of the files to your hard disk. If you keep `6502`, `Handy` or `Sound` on separate hard disks or partitions, you will have to selectively restore them separately to appropriate disks.

There should be no source code changes required.

The file `lxfast.bin` in the `Sound:` directory has been reassembled with the latest fix to HSFX. Copy the file `Sound:lxfast.bin` to the `vdk:` directory.
