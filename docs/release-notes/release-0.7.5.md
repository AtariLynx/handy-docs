# HANDY Release 0.7.5

28 February 1989  
Confidential and Proprietary

This document describes the V0.7.5 release of Handy development environment. It includes these sections:

- [Project summary reminder](#project-summary-reminder)
- [Handycraft V1.50](#handycraft-v150)
- [What you must do to start using this release](#what-you-must-do-to-start-using-this-release)


## Project summary reminder

Just to remind you, everyone is supposed to submit a summary of and schedule for what of your game needs completing. Be sure to make it sound as if you can get everything done by April Fools Day. Just kidding, be realistic.

## Handycraft V1.50

New paste sprite string gadget, `Data Name Suffix`, which allows you to specify a suffix that will be appended automatically to the sprite's name when constructing the sprite's data name. You can specify any suffix you like (up to 12 characters). This suffix is saved when you save *Handycraft* settings.

Here's one you might like: now *Handycraft* makes a guess at the output filename for you. Remember, to clear the text out of an `Intuition` string gadget fast, enter `RightAmiga-X`.

Here's another one you might like. There are now three different directory pathnames saved as settings: `HOMEPASTE`, `HOMEWRITE` and `HOMEWORLD`. These are used for saving your desired default paths to the directories for pasting sprites, write out sprite text, and edit world input/output.

Currently supported settings are:

```
HOMEPASTE path specification 
HOMEWRITE path specification 
HOMEWORLD path specification

COLORS rgb rgb rgb rgb 
DISKNAME VOLUME/DEVICE

PIXELPRESET1 n n n n n n n n n n n n n n n n 
PIXELPRESET2 n n n n n n n n n n n n n n n n 
PIXELPRESET3 n n n n n n n n n n n n n n n n 

CHIPMEM n 
DATASUFFIX string
```

Well, several have asked for the ability to write out all of the current edit sprites with one command. Meanwhile, no one claimed to be a user of the `W` function (write out the SCB only, not the data). So, the `W` command now will write out all of the current edit sprites. The title of the requester tells you what sprite is currently being written out, and a guess is made at the name of the output file. Tell me how you might like this to be different.

When a new sprite is pasted, it is always positioned in the center of the window regardless of the positioning of the curretly selected sprite.

The `x` command does what it's supposed to. The `X` command now does nothing.

Oops. Whose fault is it? Hmm. Under `Intuition`, it's possible for a new requester to open and for a program to get an event about a gadget hit in that new requester before getting the event that says that the requester has opened. In *Handycraft*, the event stating that the requester has opened causes certain variables to be initialized, while the gadget event causes code to execute which code expects that those variables are correctly initialized. Wouldn't you expect that you must necessarily hear about the opening of the requester before you hear about any gadget hits in that requester. Anyway, that's what I foolishly presumed oh dopey me, and so hitting one of the `EDIT COLORS` gadgets as soon as you could see it would cause the system to `GURU`. Not no more, I hope.

Oops. When you edit a sprite's `Data Pixels`, then edit one of the presets, the `Data Pixel` edits were being lost. Hopefully not no more.

Oops. Now no more is the last pixel of data thrown away by the compressor. Ha ha.

Coming soon:

- Doing `TRIM EDGES` correctly, including not wasting a bit-per-pixel on zero when all zeroes will be trimmed
- Highlight currently selected `Data Pixel` array selection
- Loading *Handycraft* palette from a sprite's brush
- Better Data Compression

## What you must do to start using this release

Zip.
