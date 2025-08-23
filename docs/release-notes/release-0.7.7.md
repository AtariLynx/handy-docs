# HANDY Release 0.7.7

6 March 1989  
Confidential and proprietary

This document describes the V0.7.7 release of the Handy development environment. It includes these sections:

- [Project summary reminder](#project-summary-reminder)
- [Handycraft V1.52](#handycraft-v152)
- [Handebug 1.48](#handebug-v148)
- [What you must do to start using this release](#what-you-must-do-to-start-using-this-release)

## Project summary reminder

Just to remind you, everyone is supposed to submit a summary of and time projection for the components and details of your game that still need completing.

THIS MEANS YOU. YOU BUM. Unless of course you've already submitted your summary, in which case: don't read this section.

## Handycraft V1.52

Bug fixes:

- Zero-pixel and one-pixel data lines are now output correctly, hopefully
- There's a chance that `I` and `O` now use the programmer's requested pixel output depth rather than the minimum required output depth

Coming soon:

- Doing `TRIM EDGES` correctly, including not wasting a bit-per-pixel on zero when all zeroes will be trimmed
- Highlight currently selected `Data Pixel` array selection
- Loading *Handycraft* palette from a sprite's brush
- Option for `W` command (write all sprites) to write all sprites to one file

## Handebug V1.48

This release of *Handebug* introduces structure definitions that can be reloaded. `Shift-F7` brings up a file requester that allows you to specify a new `.defs` file from which structure definitions will be read. If a file is selected then the previous definitions are discarded and new ones are loaded. The structure window is closed if it is opened when the new definitions are loaded.

Also, if a window has a size specification and it doesn't specify `x` and `y` coordinates, the window will open in the upper-left corner of the Handebug display. If you don't like that then specify `x,y` coordinates in your structure definition.

Bug fix:

- Windows positioned near the right edge of the display no longer open wrapped around to the other side of the display

## What You Must Do To Start Using This Release

Zip
