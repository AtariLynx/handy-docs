# HSFX Editor Notes

Copyright (C) 1988, Epyx, Inc. 
CONFIDENTIAL and PROPRIETARY

This document describes the *HSFX Editor*.

If you haven't noticed yet, you will soon notice that this document is in-the-works.

This is not intended as a stand-alone document, but rather is destined to become a chapter of the Handy Software Programmer's Guide and Notes. It describes only the components and usage of the HSFX Editor. It doesn't attempt to instruct on Handy audio control or sound creation techniques; before reading this you should read the HSFX and the Handy Audio System Description chapters of the Programmer's Guide.

Also, not everything described in this document is implemented yet.

## Introduction and Definitions

The program has two main displays, the Main Menu and the Key Frame Editor.

The `Main Menu` allows you to load and save sound effects and to make global changes to the current sound effect.

The `Key Frame Editor` allows you to create, modify and delete the key frames of the sound effect.

A sound effect is comprised of a series of key frames, which key frames contain values that the audio hardware should have at given instants of time and instructions on moving from one key frame's value to another's.

The *HSFX Editor and Driver* support a technique called interpolation, which is the opposite of step. Interpolation refers to the numeric progression from one value to another. If you have specified that you want one of the audio components, volume for example, to be at one value at a given instant and a different value later, you are allowed specify that you want the value to either change abruptly to the new setting or to ramp incrementally once per audio frame from the initial to final setting.

Currently, the HSFX Editor and Driver support only linear interpolation, which is a simple algebraic slope fromone value to another. We are considering adding musical interpolation, which would interpolate from one value to another using the curve that describes moving from one musical note to the next.

Using the FllelO Requester.

Rounding errors. Today's technique for avoiding them. Tomorrow's technique for avoiding them.

## Main Menu

New
Copy 
Set Audio Frame Rate 
Quit

Load an HSFX Editor File 
Save Sound Effect as an HSFX Editor File 
Write Sound Effect as a 6502 Text File 
Write Sound Effect as an Assembler .bin File

Set this Sound Effect's Equate Name 
Set this Sound Effect's Assembler Label 
Set this Sound Effect's Priority

Edit

Eval
Play

Cut
Copy
Paste

Selecting a keyframe and drag-selecting multiple adjacent keyframes

## Key Frame Editor

The Key Frame Editor display has three edit areas:

- There's a L-shaped area that starts top left and flows down then over to bottom-right. This `L` presents a programmable analogue of the Handy sound hardware. The programmer can set values in this edit area that will be converted by the *HSFX* editor into a table of coded instructions, which the *HSFX* driver on Handy can understand and execute.

- In the top right is the `Key Frame Entry` edit area. Here's where the user cruises though the current list of keyframe entries and creates and deletes entries, and where the user sets the sound driver parameters of this entry.

- Halfway down the right-hand side is the control section. Here the user can decide to play the current sound effect, return to the main menu, or undo the last operation.

### Hardware analogue

The hardware analogue has three areas where the user can control the settings of the audio hardware. These three areas correspond to the three components of Handy audio. The first area allows the user to specify the base frequency for the sound. The second area provides control gadgets for the audio polynomial sequencer. The third area allows control over the sound effect's volume and integration.

The first box (top-left) of the hardware analogue has gadgets which allow the user to select the base frequency/period for this frame of the sound effect. The base frequency is the number of times per second that this sound effect's clock will tick. The base period is the amount of time that elapses between two ticks of this sound effect's clock.

The base frequency/period can be specified three different ways: the user selects one of the clocks and enters a multipler for that clock; the user specifies the desired frequency; the user specifies the desired period. When the user selects a clock and enters a multiplier, the value in the frequency number gadget should be updated simply by multiplying the clock by the multiplier. On the other hand when the user enters a frequency value the specified value should be converted into clock and multiplier selections, using the smallest clock possible. Note that the specified frequency may not evenly divide into a clock and multiplier. If this is the case, get as close as you can. Save the clock, the multiplier and the frequency in the `KeyFrame` data structure. The 7 clock selection gadgets are mutually-excluding, of course, and have special imagery that create a beautiful complement mode effect. You'll need to create alternate imagery somehow. Using `PowerWindows 2` may do the trick for you. Finally, there's two gadgets, `Interpolate to Here` and `Step Now`, which are mutually-excluding gadgets. If `Interpolate To Here` is selected then set the KeyFrame's `INTERPOLATE_FREQUENCY` flag. If `Step Now` is selected then clear the KeyFrame's `INTERPOLATE_FREQUENCY` flag. You'll need two pieces of imagery for these gadgets, one with the circle filled in and one with the circle blank. These two images can be used in several places on the `Editor` display.

The next box of the hardware analogue (bottom-right) allows the user to set up the polynomial sequencer. There's two pieces to the polynomial sequencer, the shifter value and the freedback-enable lines. The user may or may not want to set the shifter to a specific value during any given keyframe. The user makes this choice by

First time a Key Frame Entry is edited 
Rules for Copying and Deleting Key Entries

## When User Selects PLAY

## Converting the Key Frame List into an HSFX Table
