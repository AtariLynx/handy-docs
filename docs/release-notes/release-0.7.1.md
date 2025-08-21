# HANDY Release 0.7.1

3 February 1989  
Confidential and Proprietary

This document describes the V0.7.1 release of Handy development environment. It includes these sections:

- [Handycraft V1.47](#handycraft-v147)
- [6502: Notes](#6502-notes)
- [What you must do to start using this release](#what-you-must-do-to-start-using-this-release)

## Handycraft V1.47

Wow. No *Handycraft* 1.46 bug reports?

Here's a color-palette release of *Handycraft*. Now there are two distinct color palettes, the emulation palette and the edit palette. Changes to one don't affect the other, unless you explicitly copy one palette to the other. Also, you can write out the current edit palette in 6502 format. Finally, the emulator no longer uploads the palette with every new display, but rather uploads the new palette only when you use one of the color-change macros.

The `g` command copies the editor palette to the emulator palette. The `G` command copies the emulator palette to the editor palette. Why `G`? Because I'm out of mnemonics!

The `P` command will output the palette in 6502 assembler format.

Also, a reminder: if you hit the `HELP` key you get a summary of all the *Handycraft* commands.

## `6502:` Notes

There's a new `display.src` and `display.mac`, which now support uploading the color palette only when one of the color-modification macros is used.

## What you must do to start using this release

Nada.
