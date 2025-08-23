# HANDY Release 0.6.2b

3 January 1989  
Confidential and Proprietary

This document describes the V0.6.2b bug-fix release of the Handy development environment. It includes these sections:

- [`6502:` changes](#6502-changes)
- [Not kidding about recording](#not-kidding-about-recording)

WARNING TO ALL: when setting up your video display frame rate, use only the `SETDISP_60` macro. There's some idiosyncratic behavior in the display hardware if you ask for slower frame rates.

## `6502:` changes

Two bug fixes: all code in `control.src` now is wrapped within `#IFNDEF HANDYIO`; `tapedeck.mac` has a better description of the animation table entries.

## Not Kidding About Recording

I'm not kidding about the importance of your video recording. Tonight we lost Suzy hardware for a long time ... it's still not up, and it's been hours now. Please don't leave Bavl and Uncle Daddy strung out there with their yarbles flapping in the breeze. Get some good footage of your game.
