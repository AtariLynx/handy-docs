# Editing Sound Effects With HSFX

Chris Grigg  

## Introduction

This quickie document is intended for Lynx sound artists and programmers who need to create or edit six with the *HSFX* editor. It includes practical information not present in the other *HSFX* documents, which should also be read by such people: [[[HSFX - HANDY SOUND EFFECTS SYSTEM]]], aka. [[[HSFX Editor and Drivers]]], and [[[HSFX Editor Notes]]]. Note that even though the [[[HSFX Editor and Drivers]]] document alludes to an HSFX Editor User's Guide, I don't think it exists.

## Getting Started

1. Turn development system power on
2. Boot the Amiga
3. At CLI prompt: `run handebug`
4. In Handebug:
   A. Reset development system
   B. NMI (Amiga `F10` key)
   C. Click on `Bootstrap` and download monitor (click `OK!`). For chip systems, this is `Handy:monitors/monitor_chip.bin`
   D. Click on `Download` and download `Handy:hsfxedit.bin` (Load & Go/OK!) Development system will make a funny noise if all is well (is your volume up?). 
   E. Send screen to back
5. At CLI prompt: `run hsfx`

You're now ready to edit sfx.

## Editing Sfx

This is an acquired skill. The Lynx audio hardware is limited, and it takes persistance to craft a really good sound effect. This is because the link between changes in some parameters and the resulting changes in sound frequently confounds intuition. A good place to start is with the `Music & Six Examples Disk`. Load some sfx, change them, take them apart, etc.

Some combinations of shifter and feedback settings can cause the hardware to emit no net sound. Don't be discouraged if this happens, just manually undo or press on until it starts making sound again.

## General guidelines:

- For noise, interpolate the shifter feedback register. The more different the starting and ending settings, the whiter the noise spectrum.
- For more high-frequency overtones, use a higher bandwidth in the shifter. For example, `101010101010` is buzzier than `111111111110`.

## Other Notes

- *HSFX* Always runs at 240 Hz, which means that if a programmer wants to use a slower audio interrupt rate, like 120 Hz or 200 Hz, you can't property edit sfx for that programmer. *HSFX* and *hsfxedit* should be modified to add an onscreen interrupt rate control to make support for such programmers possible.
- Not all of the onscreen gadgets that should work actually do work, nor do all of the program features. On the `Menu` page graphic display, text table display, cut, copy, paste, write bin file, and multiple loaded six names don't work. On the `Edit` page dynamic interpolation and undo don't work.

End of document
