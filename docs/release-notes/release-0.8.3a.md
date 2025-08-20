# HANDY Release 0.8.3a

30 May 1989  
Confidential and Proprietary

This document describes the V0.8.3a bug-fix release of the Handy development environment.

`6502:examples/cartdefs.i` with the `ROMDIR_FILE1_Mumble` definitions I promised.

Oops, the system now sets `NO_COLLIDE` in `SPRSYS` before displaying the file `0` sprites.

In `6502:examples` the `testcart.src` and `testcart.hsf` files now demonstrate the file `0` / file `1` technique.

New `IODAT_RAM` shadow mask. The `IODAT` hardware register isn't really read/write, so you should use the `IODAT_RAM` shadow whenever writing to the register. `IODAT_RAM` is declared in `sysdata.src`. Also, in support of the new `IODAT_RAM`, a new `cart.mac` is released. Likewise, new Handy and Chip monitors are released. And, yes, `HANDY:cartgo.bin` and `.sym` coming at you.

New manual are released, which describe the newly discovered facts about `IODAT` and and about the `NEXTSCB` register.
