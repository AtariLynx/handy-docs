# Using Midi2Spl

Chris Grigg

## Introduction

*Midi2Spl* is an Amiga application, written by Stephen Jungels and Dave Staugas of Atari, that converts the score in a 1-voice Format 0 Standard MIDI File into an SPL source file (text format) that can be compiled by the *HSPL* compiler. This allows Lynx composers and arrangers to use any popular computer and sequencer- like whatever their own personal favorite happens to be- to edit their tunes before solidifying into harder-to-edit SPL for voicing and final tweeking. Mainly, *Midi2Spl* severs our former unfortunate dependence upon Dr. T's *KCS* and the *HKCS* score translator. There is also an Atari ST program, also called *Midi2Spl*, that provides the same functionality on that platform.

## Preparing files for conversion

Before saving your MIDIfile, strip out all non-note events: program changes, controller events, system exclusive messages, and so forth aren't parsed by *Midi2Spl* and usually cause incomplete translation.

Any text found in the MIDIfile is output to the SPL file in a comment. Performer's track names are passed through to the SPL file in this way.

Only monophonic Format 0 files are accepted, so you'll have to save and *Midi2Spl* a seperate MIDIfile for every voice for every section that you want to see as a seperate SPL source file. For example, if your piece uses four voices and has three sections you'll need to save and translate a total of twelve files. To minimize procedural overhead you can catenate the sections sequentially in your sequencer, then save and *Midi2Spl* just one MIDIfile per voice, and finally cut the resulting SPL file back out into the original sections.

MIDIfiles can be moved from your sequencer's computer onto the Amiga with any usable file- transfer utility- we use Red Ryder on the Mac side and Online! on the Amiga side. All MIDIfiles' names should end in .mid because that's how *Midi2Spl* expects them.

## Musical conversions

It seems that a sixteenth note in a Performer file shows up as a duration of 10 in the SPL source file. Barlines fall every 160 time units. It also seems that with normal MIDI instruments and *Midi2Spl*'s default voicing, notes need to be transposed down about 18 semitones to produce the same pitch on the Lynx as from the sequencer over MIDI.

## Program operation

```
midi2spl filename
```

Looks for the file `filename.mid`. If it exists, opens the file `filename.spl`, writes a default voice header, and starts trying to translate. Also writes an EMS at the end of the track.

## Macintosh users' addendum

In Performer on the Macintosh, use `Save As ...`, click `Standard MIDI File`, and in the `MIDI File Options` dialog check only the Format 0 - one multi-channel track option. Don't expand loops, put them in by hand in the generated SPL file. If you end your filename in `.mid` and use the Kermit protocol to transfer the file to the Amiga you'll never have to type out the full filename.
