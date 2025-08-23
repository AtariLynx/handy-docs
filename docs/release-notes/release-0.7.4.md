# HANDY Release 0.7.4

23 February 1989  
Confidential and Proprietary

This document describes the V0.7.4 release of Handy development environment. It includes these sections:

- [Handycraft V1.49](#handycraft-v149)
- [What you must do to start using this release](#what-you-must-do-to-start-using-this-release)

## Handycraft V1.49

Now, rather than having a string gadget with which you enter your `Data Pixel Depth` in the `EDIT PARTICULARS` requester, there are 4 `Data Pixel Depth` gadgets which show you current choice and possible choices.

There's a new setting that's saved when you write out the settings: `CHIPMEM`. This contains whatever you have the ChipMem Manager's buffer set to, and the buffer gets set to this whenever you subsequently run *Handycraft*.

Currently supported settings are:

```
HOME path specification 
COLORS rgb rgb rgb rgb 
DISKNAME VOLUME/DEVICE

PIXELPRESET1 n n n n n n n n n n n n n n n n 
PIXELPRESET2 n n n n n n n n n n n n n n n n 
PIXELPRESET3 n n n n n n n n n n n n n n n n

CHIPMEM n
```

Oops. Saving the color palette was accidentity removed from the last release. Now it's put back in.

Oops. The `i` command used to set all sprite's `Data Pixel` entries to `PACKED`. Now they retain whatever values they had when written out.

Oops. The `Data Pixel` value of `0` was being used to when trimming sprite data edges. Now the `Handy Palette` value of `0` is used, as it should be.

The `Data Pixel` selection that you make becomes the default when you paste in new sprites. This means that if you, for instance, select `PRE 1` for your `Data Pixels`, then every sprite you paste in after that will get the `PRE 1` assignment automatically. Your current selection isn't highlighted yet, but it will be next release.

Excess (completely blank) data lines are now trimmed from the end of each imagery data quadrant. This will save 2 bytes for each blank line.

The `HSize` and `VSize` that are written out in a sprite control block now have the correct values.

By popular demand, the "Are you sure?" stuff has been taken out.

Coming soon:

- Doing `TRIM EDGES` correctly, including not wasting a bit-per-pixel on zero when all zeroes will be trimmed
- Highlight currently selected `Data Pixel` array selection
- Loading *Handycraft* palette from a sprite's brush
- Better `Deep Compression` (I think. Created by James, and I haven't understood it yet)

## What you must do to start using this release

Zip.
