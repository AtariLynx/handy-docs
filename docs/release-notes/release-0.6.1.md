# HANDY Release 0.6.1

31 December 1988  
Confidential and Proprietary

This document describes the V0.6.1 release of Handy development environment. It includes these sections:

- [HandyFakers take note](#handyfakers-take-note)
- [Video recording](#video-recording)
- [`6502:` changes](#6502-changes)
- [Handycraft V1.37](#handycraft-v137)
- [Handebug V1.41](#handebug-v141)
- [What you must do to start using this release](#what-you-must-do-to-start-using-this-release)

## HandyFakers take note

HandyFakers notice: all changes in this document refer to your PseudoSuziness too, so make all changes in both of your codes.

## Video recording

Regarding video recording, the operative phrase is: record lots. Every chance you get, if the recorder is sitting idle then grab it and throw some imagery on it. Don't tape over anything that you've put down, even if you think early stuff is too junky to keep. 18 hours before we go to CES I'm going to take what tape we've got and edit up a master, and I want to start with as much material as possible, so record record record. If we start running short of videotape, give me some warning and I'll get us tapes until they come out our ears.

Later today (Saturday) I'll start on the *Handycraft* recorder. Larry is writing an input force-feeder that will allow you to enter a table of joystick/fire button commands which will end up looking as if they came from the user. This will allow you to write a script to drive your program, which, working in conjunction with auto-Handycraft video recording, will allow you to generate lengthy recordings of your stuff.

You should do your recordings while in *Handycraft*'s expanded display mode. How do you do that without your display looking like junk? Well, I'll tell you:

- NMI to make sure your code isn't running
- `f` and `F` to free everything up
- `C` to set your CHIP RAM to a much larger number than before
- `x` to expand your display

## `6502:` changes

New final honest works this time rule regarding zpage and file inclusion: after you include your J include files, and before you include any other files or declare any data or code, you should include `zpage.mac` and then `syszpage.src`, like so:

```
.IN 6502:include/harddefs.i
.IN 6502:include/monitor.i
.IN 6502:macros/zpage.mac
.IN 6502:src/syszpage.src
.IN 6502:macros/display.mac
.IN 6502:macros/sprite.mac
```

The include file `harddefs.i` now has sprite type definitions. For a good time, try the `XOR_SPRITE`. Also, the constants `REUSE_HV`, `REUSE_HVS`, `REUSE_HVST` and `REUSE_NONE` of the SCB's `Control1` field are now renamed to `RELOAD_HV`, `RELOAD_HVS`, `RELOAD_HVST` and `RELOAD_NONE` in order to more correctly reflect the function of the field; ie when the field has a given value, those elements of the sprite will be reloaded (and therefore values for those fields must be part of the SCB). Note that the *Handycraft* SCB output routines now output the values of the SCB `Control0` and `Control1` fields as text, rather than as hexidecimal,

To use real Handy math hardware, now you will have to define `HANDYMATH`. If `HANDYMATH` isn't defined, the macros in `6502:macros/handymath.mac` will use the Handy fake-math software instead. This is different from days of olde (yesterday) when simply defining `HANDYIO` would get you Handy math hardware. But everydobby knows that Handy math hardware is still far away, thus the new define. For a good time, define `HANDYMATH` now and blow up the hardware.

Bug fixes:

- `6502:macros/display.mac`  
  Fixed `DBUF_DISPLAY` macro under `HANDYIO` to call `DISPLAY` correctly  
  Fixed `SETCOLL` under `HANDYIO` to write the collision value to the hardware register ...
- `6502:macros/sprite.mac`  
  Changed an errant `#<?0` to `#>?0` in SPRITES  
  `SPRITES` (Non-HANDYIO) no longer sets `DISPLAY`-request bit in `X` register before the `JSR UploadSprites`
- `6502:src/sprite.src`  
  `UploadSprites` (Non-HANDYIO) no longer recognizes `DISPLAY`-request bit in `X` register
- `6502:examples/video.src` and `testsprite.src`  
  These files now reflect the new zpage include rule

## Handycraft V1.37

*Handycraft* now trims edges of literal sprites. A beneficial side-effect: compression runs a little faster too, because fixing one fixed a side-effect.

Bug fixes:

- Sense of output SCB reload-palette bit is inverted
- Your `RELOAD_` size, stretch and tilt are selections are now reflected in the SCB output

By the way, here's a short-term decision I made a while ago and forgot to tell anyone about: when you ask *Handycraft* to `TRIM EDGES` when outputting your data, some value has to be chosen as the "whitespace" value to be trimmed. The current choices for whitespace were:

- Amiga color 0
- Handy image data color 0
- Handy output palette color 0

Currently, I trim image data `0`, which is to say that any pixels of your data that are translated into image data `0` that can be cut away from the outside of your data will be cut away if you select `TRIM EDGES`. If enough people ask me to change it immediately, then I will, else you'll have to wait for the long-term solution.

The long-term solution is to provide complete flexibility: three gadgets which let you select which data values you want trim to occur in, be it `Amiga`, `image` or `output` palette, and which value you want used for the trim value (`0` is the default).

## Handebug V1.41

*Handebug* 1.41 has had a baby girl named Emily! Yay! Oops, sorry, wrong creation.

One quick but pleasant change to `Handebug:` when *Handebug* goes out of communication with the target machine, pages of the data display that are subsequently viewed will be refreshed with dashes rather than junk numbers. This little change will give you undeniable notice that the target is down, rather than leading you to believe that you're looking at valid data.

## What you must do to start using this release

You must translate you order for including files, especially as concerns `zpage.mac` and `syszpage.src`, as described under [6502: changes](#6502-changes) above.

If you were using any of the sprite's `REUSE_` definitions from `harddefs.i`, you'll need to translate them to `RELOAD_` definitions.

Also on Saturday the new joystick/fire button macros are coming, which macros will break the way you're currently talking to the controls. I'd call it the Joystick-Buster release, but that sounds too ominous.

Sorry, no more shorting the circuit boards in the lab to make joysticks work. One slip, and you short lots, and things blow up real bad sez Needle so no more of that, OK?
