# HANDY Release 0.8.2

17 May 1989  
Confidential and Proprietary

This document describes the V0.8.2 release of the Handy development environment. It includes these sections:

- [Chip systems](#chip-systems)
- [Other `6502:` changes](#other-6502-changes)
- [Handycraft 1.59](#handycraft-158)
- [What you must do to start using this release](#what-you-must-do-to-start-using-this-release)

## Chip systems

Everyone should read this section. If you don't have a *CHIP* system yet, you will have one soon.

There's a new `setXXXX` script file in the `HANDY:` directory, `setCHIP` (which joins `setAIO2`, `setIO32` and `setHANDY`). If you are running a chip system, you will have to use `setCHIP` after a system installation if I forget to do it for you.

To reset a chip system:

- First, reset the Handy
- Count 4 seconds, then hit the *Handebug* NMI function key (`F-10`).
- You don't have to load bootload code, because the bootload code is in the ROM attached to the hardware. But you do have to load the monitor code, which you do as usual by hitting the `Handebug Bootstrap` gadget or `F-9`. When the `bootstrap` requester comes up, make sure the `bootload` string gadget is blank (don't forget that these text strings are saved when you save your *Handebug* settings). When you hit `OK`, you ought to be in normal monitor mode.

Other than these things, chip systems ought to work the same as the breadboard systems.

## Other `6502:` changes

There's a new `cart.src` which is one of the pieces required for game cartridge code to work correctly on the chip systems. Code that was assembled using the old `cart.src` may not work on chip systems.

The `SERCTL` register bit `TXOPEN` is now set in 3 places: at `ColdStart` time in the monitor, which happens once at time of reset; when you hit `GO`; and in the `INITSYS` macro. If you don't know what this means, forget about it.

The order that interrupts are processed by the handler is reversed. Low-number timers get processed first. This change actually was made for the 0.8 release, I just forgot to tell you about it.

It's now possible to sometimes have just *HSFX* code loaded into your program and sometimes both *HMUSIC* and *HSFX*. It wasn't possible before because, to conserve space, key features of *HSFX* weren't included in the assembly if *HMUSIC* was being included. Now you can override the efficiency and get complete versions of both *HSFX* and *HMUSIC* by defining the constant `SEPARATE_HMUSIC_HSFX`.

## Handycraft 1.58

When sprite data is written out, the files are no longer appended by `.END` and `;Eat me.`

## What you must do to start using this release

This space intentionally left blank.
