# HANDY release 0.5.2 

7 December 1988  
Confidential and Proprietary

This document describes the V 0.5.2 release of Handy development environment. It includes these sections:

- [*Handebug* V1.38](#handebug-v138)
- [What you must do to start using this release](#what-you-must-do-to-start-using-this-release)

# *Handebug* V1.38

New *Handebug* template, now numbered 1.38. Clever little boys, aren't we?

The symbol display is now working. You can select `SYMBOLS` from the bottom of the *Handebug* display (or use `Control-S`) to display the current symbols. Symbols can be sorted either alphabetically or numerically. When switching between alpha and numeric sort, if it's possible to keep the same symbol on the same display line, *Handebug* will do so for you.

New support commands for symbols:

- `SYMBOLS` gadget and `Control-S` now bring up the symbols display
-  `Control-A` selects Alphabetic sort (which is the default)
- `Control-N` brings up the Numeric sort

`Step` and `Set Flat` are now much faster, thanks to a stroke of John genius.

Formerly, if you had a `.RUN` assembler directive in a file that you download through *Handebug*, the `PC` would be set to the `.RUN` address as well as the `.RUN` address becoming the `GO` address. Now no more. The `PC` isn't disturbed by downloads, and instead the `.RUN` address is stored only as the `GO` address. Those of you who "download and GO" won't notice the difference. Those of you who "download and CONTINUE" will have to start using "download and GO."

There's a new code mode, `FOLLOW PC`, which will cause the current `PC` instruction to always be displayed when in `Code` display mode. The effects of this are many: if your program is running and it hits a `BRK` instruction or a breakpoint, the `BRK` or break-pointed code will be brought onto the code display; if you are using `STEP FLAT` and you step off the current display, the new code will be brought onto the display; if you are using `STEP` and you step into a subroutine, the first line of that subroutine will be brought onto the display.

The mode is optional, the default is that the mode is disabled.

Currently, the only way to select and deselect the `FOLLOW PC` mode is by using the Shift-F8 key, which toggles the `FOLLOW PC` mode. Someday this option will be available on the `Preferences` display too.

And then, from the *You Need Only Ask* department: whenever you download, if there are breakpoints defined in the range of the download, those breakpoints are now automatically disabled for you.

Bug fixes:
- When you download, chances are better that you won't soon crash
- Strings are updated correctly by the structure editor, really

## What you must do to start using this release

You don't have to do anything, just enjoy!

Well, that's not exactly TRUE. If you're used to using the `CONTINUE` command after downloading, you'll now have to get used to using the `GO` command after downloading.

Here's a *Handebug* hint for you: when using symbols, if you have a lot of symbols you can speed up downloads dramatically by using the `CLEAR SYMBOLS` command (`Shift-F9`) just before downloading. This allows *Handebug* to skip the step of trying to look up old symbols that need to be discarded when reading in the new symbol table.

Finally, Uncle RJ pleads: don't accept bootlegged versions of Handy software from shady characters. If someone wants to foist unreleased software on you, JUST SAY NO!
