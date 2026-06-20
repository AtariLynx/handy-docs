# HSPL - The Handy Sound Programming Language

This document describes *HSPL*, the Handy Sound Programming Language. It contains the following sections:

- [Introduction](#introduction)
- [Conditional Compilation](#conditional-compilation)
- [Macros](#macros)
- [Including Files](#including-files)
- [Defining Constants and Numbers](#defining-constants-and-numbers)
- [Complex Expression Evaluation](#complex-expression-evaluation)
- [Music Notes and Rests](#music-notes-and-rests)
- [HSPL Directives](#hspl-directives)

## Introduction

*HSPL*, the Handy Sound Programming Language, is a slightly-misnamed computer language that allows you to create music for the Handy game system (misnamed in that *HSPL* lets you create one type of sound, music; other sound effects are created by using the *HSFX* editor).

*HSPL* compiles source code files which consist of music notation and compiler directives, and creates a data file that's ready to be used by the Handy system's *HMUSIC* driver. You include this data file during your program assembly, and then use simple *HMUSIC* macros to play whole songs or single tracks.

*HSPL* music notation consists of notes and rests. *HSPL* directives are used to define music voicing and control structures, and allow access to other *HMUSIC* driver features.

*HSPL* source code looks much like assembly language source code, and has many of the same features. *HSPL* supports conditional compilation, macros, comments, the ability to include files and to evaluate complex expressions. The compiler is case insensitive, which means that your can enter the directives and music notation in upper- or lower-case or any combination thereof that pleases you.

The basic unit of *HSPL*/*HMUSIC* time is defined by the *HSFX* (Handy Sound Effects) driver's audio frame. The duration of a single audio frame is definable by the Handy game designer; typically (by default) the duration is `1/240` second. Note that, unlike the audio systems of other computers, Handy audio timing is completely independent of the rest of system timing; there's neither a need nor a requirement to synchronize audio to the video frame or anything else.

## Music Notes and Rests

Note events are expressed with a pitch name, octave, and duration:

```
<pitch name>.<octave> <duration in ticks>
```

Note names consist of three parts: the letter of the note (`a` through `g`), a natural (`.`) or sharp (`s`), and the octave of the note; for example `a.1` is the note `A-natural` in octave `1`. Duration specifies the number of music ticks (defined by the `Tempo` instruction; see below) the note will play. For example:

```
9.2 4 ; Play a G, octave 2, for 4 ticks 
bs3 16 ; Play a B-sharp, octave 3, for 16 ticks
```

Octaves change numbers at `C` and start with octave `-1`. Notes above octave `4` will have tuning problems.

Rest events are expressed by the word `rest` and a duration in ticks.

```
rest <duration in ticks>
```

For example:

```
rest 4 ; Rest for 4 ticks 
rest 16 ; Rest for 16 ticks
```

The duration value for a rest is the same as for a note. Note that the *HSPL* articulation directives `AGD` and `ASD` allow for space between consecutive notes without requiring a separate rest event.

## Complex Expression Evaluation

The compiler can evaluate expressions of reasonable complexity. Expressions can be comprised of operators, numbers, and constants. The operators are C-style and have the same precedence. The allowable operators are listed below, ordered in groups of precedence.

|||
|---|---|
|`( )`|Parentheses, used for clarity and overriding normal operator precedence|
|`·` `!` `~`|Unary minus, logical negation, bitwise negation|
|`*` `/` `%|Multiplication, division, modulo|
|`+` `-`|Addition, subtraction|
|`<<` `>>`|Shift left, shift right|
|`‹` `<=` `>` `>=`|Less than, less than or equal, greater than, greater than or equal|
|`==` `!=`|Equal, not equal|
|`&`|Bitwise and|
|`\|`|Bitwise or|
|`&&`|Logical and|
|`\|\|`|Logical or|
|`=`|Assignment|

Here's some examples of expressions and operator precedence

|||
|---|---|
|`2+3*2`|Evaluates to 8|
|`3 & 6`|Evaluates to 2|
|`3 && 6`|Evaluates to non-zero (logical result, not bitwise)|
|`~foo + 1`|Evaluates to `-foo`|
|`(100 << 2) / 2`|Evaluates to two times foo|

## Defining Constants and Numbers

Decimals numbers are need no special notation. Hex numbers are prefixed by either `$` or `0x`. 
For example:

```
; Both of these assignments are equal to decimal 256
Dec256	= 256
Hex256	= $0100
```

Constants can be defined using either of the two `HSPL` assignment operators, `=` or `.EQ`. 

```
LeadOctave		=		2
BassOctave		.EQ		0
#if BassOctave = 0
	c.0		; Give 'em a c-note
#elseifBassOctave = 1
	c.1		; Give 'em a c-note
#else		; presume BassOctave of 2
	c.2		; Give 'em a c-note
#endif
```

Also, constants can be declared using other constants and complex expressions:

```
OCTAVE			.EQ		12 
OCTAVE_RANGE	.EQ		OCTAVE*5
```

## Conditional Compilation

The *HSPL* compiler supports several conditional compilation directives, known in short as conditionals. If a conditional evaluates to be true then the code following the conditional will be compiled, otherwise the code will be skipped.

These are the *HSPL* conditionals:

`#ifdef <name>`

Causes subsequent code to be compiled if `<name>` has been defined.

`#ifndef <name>`

Causes subsequent code to be compiled if <name> has not been defined.

`#if <expression>`

Causes subsequent code to be compiled if <expression> evaluates to a non- zero value.

`#elseif <expression>`

Causes subsequent code to be compiled if <expression> evaluates to a non- zero value and the immediately preceeding `#if`, `#ifdef`, `#ifndef` or `#elseif` didn't evaluate to be true.

`#else`

Causes subsequent code to be compiled if the immediately preceeding `#if`, `#ifdef`, `#ifndef` or `#elseif` didn't evaluate to be true.

`#endif`

Ends a set of conditional clauses. It is an error to encounter the end of a file before all conditionals have been terminated with an #endif statement.

Conditionals may be nested to any depth, as illustrated in the following examples:

```
; This first conditional is false, so the following 
; commands won't be compiled

#if 0 == 1 					; False
  #if rj == knucklehead 	; not evaluated
    c.2 					; not evaluated
    #if needle == fat 		; not evaluated
      d.2 					; not evaluated
    #endif					; not evaluated
    #if morse == vroom		; not evaluated
      e.2					; not evaluated
    #endif					; not evaluated
  #endif					; not evaluated
#endif						; End of false conditional

; In this example, the first conditional is true so some
; of the following commands will be compiled

#if 1 == 1					; True
  #if rj == knucklehead 	; True 
    c.2						; This note is entered
    #if needle == fat		; False 
      d.2					; not evaluated ..
    #endif					; End of false conditional
    #if morse == vroom		; True
      e.2					; This note is entered
    #endif					; End of vroom
  #endif					; End of knucklehead
#endif						; End of true conditional
```

## Macros

*HSPL* provides macro support, to make it easier for you to systematically create blocks of *HSPL* source code. You could create a macro to, for instance, define a particular arpeggio with the arpeggio's key as an argument, and then use the macro anywhere you want the arpeggio to sound. For an example, see the [Arpeggio macro definition](#arpeggio) at the bottom of this section.

Macro arguments have the form `?0`, `?1`, `?2` ... `?n`. The arguments that will be used within the macro must be declared when the macro is defined, which is different from the macro argument technique used by Handy program assembler where the arguments don't have to be declared.

Macro definitions may not be nested, which is to say that you can't define a macro within the definition of another macro. The macro body may have any HSPL code or directives. Macros may invoke other macros, but more than ten levels of such nesting will cause a compiler error.

Here's the HSPL macro directives:

`#macro <name> ?0, ?1, ?2 ...`

Define `<name>` as a macro with arguments `?0`, `?1`, `?2` ... `?n`.

`#endm`

End a macro definition.

Here's an example of macro usage:

<a id="arpeggio"></a>

```
; Define a macro to play an arpeggio 
#macro	Arpeggio	?0
	#if ?0 == 0 
	c.0
	#elseif ?0 == 1
	c.1
	#else	; presume octave 2 
	c.2 
	#endif
#endm

Arpeggio	0		; Play arpeggio in octave 0
Arpeggio	1		; Play arpeggio in octave 1
Arpeggio	2		; Play arpeggio in octave 2
```

## Including Files

During compilation, any *HSPL* source file can cause another source file to be included by using the `.in` directive. Any object code generated by the included file will appear at the point of inclusion. Files may include other files ad infinitum.

`.in <filename>`

Causes the named file to be opened and compiled.

## HSPL Directives

The *HSPL* directives are used to direct the compiler's creation of voice parameter tables, flow cobstructs, and control of the special features provided by *HSPL*. The directives affect only the current voice, except for the `Tempo` and `GlobalTranspose` commands which affect both the current voice and all subsequently compiled voices.

Most *HSPL* directives have both a long, descriptive name and a short, fast-to-type name. Some of the directives also have a collection of bizarre alternate names which exist in order to support the porting of music files created for ancient versions of SPL, which versions required musicians to think like engineers rather than artists. You are encouraged to use the uppermost names listed.

```
ArticGateDur <gate-on duration in frames>
AGD <gate-on duration in frames>
```

Determines voice articulation. In every subsequent note played on this voice, the envelope generator gate bit will be turned on at the beginning of the note, will stay on for the indicated number of frames, and then will stay off for the remainder of the note's duration. This gives staccato effects. Stays in effect until another `AGD` or an `ASD` is executed.

```
ArticSepDur <gate-off duration in frames>
ASD <gate-off duration in frames>
```

Determines voice articulation. In every subsequent note played on this voice, the envelope generator gate bit will be turned on at the beginning of the note, will stay on until the indicated number of frames before the end of the note's duration, and then will turn off and stay off for the remainder of the note's duration. This gives legato effects. Stays in effect until another `AGD` or an `ASD` is executed.

```
DefaultVolce
DFV
```

Makes the compiler create a set of simple-sounding default voicing parameters. `DFV` has no parameters.

If you simply want to hear something (say fresh from a MIDI file converter) you can use this instruction to audition the notes in the track before you start voicing.

```
EndOfVoice
EndOfMusic
EMS
END_SFX
EndSfx
```

Indicates the end of a voice of music. Required at the end of each voice and each ring. No parameters.

The `END_SFX` and `EndSfx` tokens are used in other machines' SPL versions, and are left in Handy SPL only to allow music in some of these other SPL's to be ported without the assistance of a brain. Use `EndOfVoice` (or `EMS` or `EndOfMusic`) in all *HSPL* code.

```
Feedback <Handy feedback register image>
FB <Handy feedback register Image>
```

Sets the voice's hardware feedback register to the indicated pattern. What it will sound like? Well, that's another question ... only patient experimentation will create the most satisfying results.

```
GlobalTranspose <pitch offset in semitones>
GTR <pitch offset in semitones>
```

Causes all subsequent scored pitches in all voices to be offset by the indicated number of semitones before the note pitch is emitted. Stays in effect until another `GTR` supercedes it. The pitch offset number may be positive (transpose up) or negative (transpose down).

At the start of compilation of a piece, `GTR` is set to `0`.

The readers who are musicians will know that a transposition in multiples of +/- 12 semitones will transpose voices by whole octaves. Non-musicians know this too, now.

```
LocalTranspose <pitch offset in semitones>
LTR <pitch offset in semitones>
```

Much like the `GlobalTranspose` function, but `LTR` only applies to the current voice.

At the start of compilation of a voice, the voice's local transposition is set to zero.

The readers who are musicians will know that a transposition in multiples of +/- 12 semitones will transpose voices by whole octaves. Non-musicians know this too, now.

```
StartLoop
SetLoop
LoopTop
SLP
```

Mark the start of a range of SPL code to be looped; the next `LOP` command marks the end of the looped range. No parameters.

There are two levels of looping in the driver. `SetLoop`'s may be nested and the compiler will keep track of which loop is which. For example, the following structure is a nested loop:

```
SLP
  (0 or more SPL statements) 
  SLP
    (0 or more SPL statements)
  LOP <number of times to play inner loop>
  (0 or more SPL statements)
LOP <number of times to play outer loop>
```

In this example, the inner `SLP` and `LOP` could have been a `SLP2`/`LOP2` pair.

```
Loop <number of times to play loop>
LOP <number of times to play loop>
```


Marks the end of a range of SPL code to be looped and indicates the number of times to play the loop. Note that `Loop 2` plays the looped region twice, `Loop 1` plays the looped region once, and `Loop 0` plays the looped region endlessly until terminated by a program call to the *HMUSIC* driver.

```
StartLoop2
SetLoop2
LoopTop2
SLP2
```

```
Loop2 <number of times to play inner loop>
LOP2 <number of times to play inner loop>
```

These directives are for the inner level of looping, but otherwise are analogous to the outer-loop directives. However, if these inner loop directives are used when an outer level is not currently defined, the compiler translates these into outer loop versions of the directives.

These alternate names are preserved from other machines' SPL's and, although not required for nested loops in Handy SPL, both simplify some music ports and may be used for the sake of structural clarity in original Handy music.

```
Tempo <number of frames per tick>
TPO <number of frames per tick>
```

Set the number of audio frames per music tick. Note and rest durations always describe multiples of the current `Tempo`. After the `Tempo` is processed, the following notes and rests in the current voice and all subsequent voices will use the new `Tempo` until another `Tempo` command is encountered.

```
Priority <priority number>
PRI <priority-number>
```

Set the current voice's priority to `<priority number>`.

Every voice should have a unique priority. The compiler will warn you if you compile a piece of music where a priority is assigned to more than one voice.

Refer to the *HMUSIC* chapter for full details about music and sound effects priorities.

```
ADSR <attack>,<peak>,<decay>,<sustain>,<release>
```

Sets the volume envelope shape for subsequent notes played on this voice. When a note is started, the voice's volume ramps up from zero to the `<peak>` value over a span of `<attack>` audio frames, and then ramps from the `<peak>` to the `<sustain>` value over a span of <decay> frames. The volume holds at `<sustain>` for as long as the envelope generator's gate bit is held on (see `AGD` and `ASD` for a discussion of the gate bit). When the gate bit is turned off, the volume ramps from `<sustain>` down to zero over `<release>` frames. All ramps are linear. Stays in effect until another `ADSR` is executed by this voice. The `<attack>`, `<decay>` and `<release>` times are in audio frames (not `Tempo` frames) and have a range of sixteen bits. The `<peak>` and `<sustain>` values are volume levels and have a range of seven bits.

```
VoiceFreqMod <start>,<attack>,<decay>,<sustain>,<release> 
VFM <start>,<attack>,<decay>,<sustain>,<release>
```

Sets the frequency modulator (pitch envelope) shape for subsequent notes played on this voice. Stays in effect until another `VFM` is executed for the current voice. Useful for drums and other sound-effects-like sounds.

Frequency modulation allows a note's pitch to change over the life of the note, in a way tied to the current voice's envelope generator (ADSR) timing. The pitch envelope linearly ramps at the `<attack>`, `<decay>`, `<sustain>`, and `<release>` rates respectively during the note's normal attack, decay, sustain, and release phases, with continuity at the transitions between phases. The VFM rates can be positive or negative.

When a VFM is in effect, a note's frequency starts at the usual frequency for the scored note (after taking into account the `GTR` and `LTR`) plus `<start>`, then ramps at the rate of `<attack>` per audio frame for the ADSR's `<attack>` duration. Any non-zero `<start>` will offset the notes from musically correct pitch (but that's not always bad). During the ADSR's `<release>` phase the `VFM` ramps from the value it had at the end of the `<attack>` phase at the rate of `<decay>` until the ADSR hits its `<sustain>` level. Then the VFM ramps from its value at the end of the `<attack>` phase at the rate of `<sustain>` until the ADSR gate bit goes off. When the gate bit goes off, the VFM ramps from its value at the end of the `<sustain>` phase at the rate of `<release>` until the note ends.

To turn off the effect of a `VFM`, use the `VFM` directive with all-zero parameters:

```
VFM 0,0,0,0,0
```

Changes in `ADSR` will affect any `VFM` in effect at the time. All `VFM` parameters are sixteen bit signed values.

```
Shifter <shifter register image>
SHF <shifter register image>
```

Loads the hardware shifter register for the current voice with the indicated pattern.

Note that the exact shifter load time is non-deterministic, subject to other activity in the machine at the time. Therefore, it's not clear exactly what effect this directive wil have. But it's always fun to experiment!

```
VoiceFBMod <start>,<attack>,<decay>,<sustain>,<release>
VFBM <start>,<attack>,<decay>,<sustain>,<release>
```

This sets up a feedback modulator in much the same way that `VFM` sets up a frequency modulator. In this case, a `<start>` value is loaded into the Handy hardware feedback-enable register at the start of a new note, and is ramped and rewritten to hardware every audio frame during the `ADSR`'s attack, decay, sustain, and release phases at the `<attack>`, `<decay>`, `<sustain>` and `<release>` rates. All parameters are sixteen bit signed values.

```
User <token> 
USR <token> 
Hook <token>
```

The `User` directive provides a mechanism for communicating with the program that the song has reached a certain point. This allows the programmer to synchronize game events with music events.

In order for the programmer to take advantage of the `User` directive, the *HMUSIC* command 1 must be called. Refer the the [*HMUSIC* chapter](hmusic-handy-music-driver.md) for details about `SETUSER`.

The `User` directive's `<token>` parameter is an 8-bit value that is passed to the `User` routine. This value is not interpreted by the *HMUSIC* driver in any way. It can be used for any purpose that the programmer desires.

```
CallSFX <name>,<duration in ticks> 
SFX <name>,<duration in ticks>
```

Start the *HSFX* sound effect `<name>` on the current channel and let it play for `<duration>` number of `Tempo` ticks, just as if the sound effect were a normal note or rest. The `<name>` parameter is the full pathname to the file containing the sound effect.

The note, rest, or command immediately following the `CallSFX` is executed at the musically correct time. If the sound effect would normally last longer than `<duration>` it is stopped at the time that the next music event is to start. If the sound effect is doesn't last as long as its allotted time, the voice will be silent for the remainder of the time.

```
Ring <name>
GosubSoundJob <name>
GSJ <name>
```

This represents the *HSPL* subroutine call, where `<name>` is the full pathname to the file of the *HSPL* track you want to call as a subroutine. Causes the track in the indicated file to be played once. When the ring track ends (via an `EndOfVoice` or any of the many `EndOfVoice` aliases) this voice continues on to the following directive. Used to create larger pieces out of smaller phrases.
