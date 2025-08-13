# HANDY release 0.5.4

21 December 1988  
Confidential and Proprietary

Out of our way, we've got code to write!

This document describes the V0.5.4 release of Handy development environment. It includes these sections:

- [*Handycraft* V1.35](#handycraft-v135)
- [*Handebug* V1.40](#handebug-v140)
- [*HSFX* 0.2](#hsfx-02)
- [ASM Notes](#asm-notes)
- [What you must do to start using this release](#what-you-must-do-to-start-using-this-release)

## *Handycraft* V1.35

Only as many bytes as necessary are output in the SCB palette. If you have 4-color imagery, only 2 bytes of palette data will be output.

Memory Manager default `CHIP RAM` allocation is now 100K (or less if you don't have the memory, of course).

Needle assures us that we no longer need it, so now gone is the extra pad-byte per line of sprite data output. Of course, there's still a pad-byte if the last significant bit of data falls in the last bit of the last byte of output data.

New Sprite Particulars Options:

- Draw Down/Up First, Draw Right/Left First  
  You use these to define the initial quadrant for your sprite data
- Reload Nothing/Size/Stretch/Tilt  
  These gadgets allow you to specify whether you want hardware reloading. The settings of these gadgets result both in flags set in your SCB and the amount of SCB data written out.
- Reload Palette  
  Use this to specify whether you want the hardware to reload the palette of this sprite.
- Trim Edges  
  You can now select whether or not you want the outer edges of your sprite to be trimmed. For instance, if you have a circle image, you can elect for the area outside the circle to be trimmed off or left on. The default selection is for edges to be trimmed. 
- Output Data Depth  
  When your sprite image data is being written out, you can force Handycraft to use more bits per pixel of output data than would normally be used. *Handycraft* figures out the minimum requirement. You can type in a number that's larger than the minimum. If you enter a number that's smaller than the minimum, *Handycraft* ignores your foolishness.

New Sprite Type Requester:

There's a brand new requester, the `Sprite Type requester`, which allows you select the type of your sprite. These reflect the 8 current Handy sprite types.

Bug fixes:

- Literal sprites now output correctly, rather than giving you `n` copies of the first pixel to be output
- The palette that's output in a SCB match the data pixels output in the imagery
- Used to be that editing a sprite and selecting `CANCEL` resulted in junk imagery until you hit the right mouse button. Not no more!
- You can now scale sprites larger than 127 times original size
- Bringing up the `Memory Manager Buffer Size` requester in low-memory conditions won't crash the system now

## *Handebug* V1.40

Ta dah! The *Handebug* select/copy mechanism now works. Now you can select data and then easily and conveniently copy it to another location. The implications of this are many, and the goodness of this will be great.

How does it work? Well, we'll tell you.

Just about any data that you can see can be selected, including the leftmost address of the data/code display window, the registers, the contents of breakpoint fields, and more. You select something by double-clicking on it (point at it and then click twice in rapid succession).

Once something is selected, you copy it to another location (called the depository) by positioning the pointer over the destination and clicking the right mouse button.

Note that this is really general purpose. For instance, to display breakpointed code you can double-click on one of the breakpoint addresses then clock in the address field of the code display window. Voila, the address changes to the breakpoint code.

Soon, you'll be able to select/copy the symbols in the code/symbol display. We mention this now because we know that you'll ask for it within seconds anyway so we beat you to it, nyah nyah.

The only problem is that you must make sure to select a window before trying to paste into it. This is a particular problem when you're trying to paste into the Structure Editor window. The great and manly and sophisticated Heliosmouse users will have no problem with this, but the others will have to remember to click before depositing.

## *HSFX* 0.2

Multiple sound effects are now supported, and sound effect names are now copied and supported.

## ASM Notes

Don't use conditional `ELSE`. If you do, your Amiga will blow up real good.

The `.PC` directive works. The `.PD` doesn't seem to work. Could someone verify the truth about this?

## What you must do to start using this release

Smile and be nice, and remember to give lots of treats to the tool creators.
