# HANDY Release 0.7.8

16 March 1989  
Confidential and Proprietary

This document describes the V0.7.8 release of the Handy development environment. It includes these sections:

- [Handycraft V1.53](#handycraft-v153)
- [Handebug 1.49](#handebug-v149)
- [6502:](#6502)
- [Handy Music soon](#handy-music-soon)
- [Coming this Friday](#coming-this-friday)
- [Good news bad news](#good-news-bad-news)
- [What you must do to start using this release](#what-you-must-do-to-start-using-this-release)

## Handycraft V1.53

In the top-level edit-sprite menu there's a new gadget, labelled `Use Palette`, which allows you to set the *Handycraft* palette from a sprite's palette. Finally. Looking back at my *Handycraft* notes, I find the first mention of this feature almost 6 months ago. Well, now you have it.

The `G` command, standing for "Group All Sprites," allows you to write all of the current sprites into one file. Tomorrow this is going to be changed to a `W` command - standing for "Write All" . which will present a requester to allow many levels of control over the writing out of all of the current sprites.

Coming soon:

- Doing `TRIM EDGES` correctly, including not wasting a bit-per-pixel on zero when all zeroes will be trimmed
- Highlight currently selected `Data Pixel` array selection
- Even faster data jamesing (five to ten percent)
- Other stuff I've found on my lists

## Handebug V1.49

Bug fixes:

- Branches now disassembly correctly again
- Bootstrap requester always gives you back your pointer, regardless of what you were doing previously

## `6502:`

A compressed version of the monitor is released. This monitor lives starting at `$F900` and uses all addresses up through `$FBFF`.

By the way, in case you didn't know, the constant `MONITOR_RAM_RESERVED` has always been defined in the `monitor.i` file. This constant describes the first non-zpage RAM location reserved for use by the monitor. With this release, this constant is now set to `$F900`.

## Handy Music soon

OK, beta versions of the *Midi-to-SPL* and the *SPL-to-Handy* compilers are now in the hands of the music guys, so after a few days of smoothing they'll have full compositions running on Handy hardware.

## Coming this Friday

There's a small but real chance that 3 things will be showing up on Friday: Mikey, *HandyASM* and *HandyROM* (the ROM builder).

Yes, that's right, the first Mikey chips might show up here as early as Friday. To everyone's delight the Mikeys seem to be passing the VTI tests, which makes the chances extremely high that they'll work first time here. Hot damn!

The assembler is now inches away. We're using Steve's Jet Game source as a test suite, and *HandyASM* currently creates an identical binary image in about 1/5 the time. Meanwhile, Carl (the guy who's writing the assembler) is going to be setting up a BBS so we can post bug reports and get new HandyASM's just by calling. I can't wait to get my hands on this puppy.

## Good news bad news

Hey! The good news is that the manufacturing costs of regular ROM's has gone down by about a buck, so that every normal production ROM we sell makes us an extra buck. Yeah!

The bad news is that the cost of the OTP ROM's (which you'll remember is what we'll have to make if you miss your release date) has gone up about a buck, so if you miss your release date the cost is now one-dollar-per-unit-produced worse than it was before, which was already pretty bad.

By the way, don't forget that information about costs and pricing is strictly confidential. I tell you because I feel you deserve to know.

## what you must do to start using this release

Zip.
