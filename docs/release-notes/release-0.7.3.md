# HANDY Release 0.7.3

17 February 1989  
Confidential and Proprietary

This document describes the V0.7.3 release of Handy development environment. It includes these sections:

- [Handycraft V1.48](#handycraft-v148)
- [What you must do to start using this release](#what-you-must-do-to-start-using-this-release)

## Handycraft V1.48

*Handycraft* has three new features: all-new `Deep Compression` algorithm, extensive new controls over the `Data Pixel` values, and `Save/Restore Settings`.

The new `Deep-Compression` algorithm uses all the same logic as the old `Deep-Compression` routine of long ago, but this new implementation is much improved. It's much faster (hundreds of times faster) than the previous `Deep-Compression` and much more efficient than the current routine (I've seen as much as 10% improvement in resultant data size). However, because it's recursive and some of it still needs to be written in assembler, the routine can be pretty slow on really huge sprites. On the other hand, with regular sized sprites it runs lickety-split and always (poof) creates compressions the same size as or smaller than the current ones.

You turn `Deep-Compression` on and off using the `d` command. Every time you use this command, *Handycraft* tells you whether `Deep Compression` is now on or off with text on the display.

*Handycraft* now provides many ways to define `Data Pixel` values. When you bring up the `Edit Colors` edit requester, there are now 8 new gadgets. These give you control over what values are assigned to the sprite's `Data Pixel` array. There are 2 algorithmic ways to assign values to the current sprite, or you can select any of the 3 sets of presets. You can edit the presets, and if you save the *Handycraft* settings then your presets are automatically restored next time you run *Handycraft*.

The new Data Pixel gadgets are: 

|||
|---|---|
|`PACKED`|Gives you `Data Pixels` in the way they're currently defined, which is with the values packed down as low as they could possibly be.|
|1 TO 1|Gives you Data Pixel values that directly correspond to the Amiga pen numbers|
|PRE 1|Copy Data Pixel Preset 1 values into the current sprite|
|PRE 2|Copy Data Pixel Preset 2 values into the current sprite|
|PRE 3|Copy Data Pixel Preset 3 values into the current sprite|
|EDIT 1|Allows you to edit the Data Pixel Preset 1 values|
|EDIT 2|Allows you to edit the Data Pixel Preset 2 values|
|EDIT 3|Allows you to edit the Data Pixel Preset 3 values|

And, with great thanks to John for the code and the assistance, now *Handycraft* too supports saving and restoring settings. Most significant of the settings that are saved/restored are the above-mentioned `Data Pixel` presets.

Every time *Handycraft* runs, it tries to read in the file `HANDY:handycraft.config`. To save your current settings, use the `s` command.

Currently supported settings are:

```
HOME path specification 
COLORS rgb rgb rgb rgb 
DISKNAME VOLUME/DEVICE
PIXELPRESET1 nnnAnAnnAnAnnAn 
PIXELPRESET2 nnnnnnnnnAnAnn 
PIXELPRESET3 nnnnanAnn
```

If there are other config things you think *Handycraft* should save/restore, let us know.

When you write image files using `w` or `O` or use `I` to get the image size, the byte size of the image data is now written to the display for your reference (at least until you do something that causes it to be erased).

Coming next with Handycraft:

- Doing `TRIM EDGES` correctly, including not wasting a bit-per-pixel on zero when all zeroes will be trimmed, and trimming vertically when the tops and bottoms of quadrants have no significant data
- Gadgets showing possible and current `Pixel Depth` selections, rather than a string gadget as it is now
- Loading *Handycraft* palette from a sprite's brush

## What you must do to start using this release

Zip
