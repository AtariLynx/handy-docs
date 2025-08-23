# HANDY Release 0.7.9

18 March 1989
Confidential and proprietary

This document describes the V0.7.9 release of the Handy development environment. It includes these sections:

- [HandyASM V0.04](#handyasm-v004)
- [Handebug 1.50](#handebug-v150)
- [Changes to 6502:](#changes-to-6502)
- [Changes to HANDY:](#changes-to-handy)
- [Good news bad news (Revised)](#good-news-bad-news-revised)
- [What you must do to start using this release](#what-you-must-do-to-start-using-this-release)

## HandyASM V0.04

Announcing, with no small amount of fanfare, the first distribution of *HandyASM*. I extend hardy congratulations to Carl Sassenrath, *HandyASM*'s creator. The assembler is already great, and there are further enhancements and performance improvements still to come.

You might notice that *HandyASM* is a bit faster than the old assembler. You might also notice that *HandyASM* still has its problems.

For now, the new assembler has been installed on your system as *HANDY:HandyASM*. When *HandyASM* is debugged to the point where we'll commit to using it as our regular assembler, then we'll rename it to `asm` (and rename the old assembler to `asm.old`).

To facilitate the testing of *HandyASM* you now have two new script files, `OldAsm` and `NewAsm`, in your `HANDY:` directory. Invoking one or the other of these scripts will cause either the old or the new assembler to become the program named `HANDY:asm`. The intent is to provide you with a convenient way to switch between assembling with the old and the new (this will be especially handy for those of you who have automated the assembly process using script files). Here's how it works: as part of the V0.7.9 release `HandyASM` is copied to `HANDY:` and `HANDY:asm` is copied to `HANDY:asm.old`; when `NewAsm` is invoked `HANDY:HandyASM` is copied to `HANDY:asm`; when `OldAsm` is invoked `HANDY:asm.old` is copied to `HANDY:asm`.

Sooner or later we're going to have to make the leap and start using *HandyASM* full-time. My guess is that committing to the new assembler will be a one-way step because I'm sure that *HandyASM*'s new features will immediately pervade our code, making it impossible for us to go back. Because we won't be able to go back, we shouldn't switch until we're sure that *HandyASM* is sufficiently robust. On the other hand we have to make the switch before too long as we're counting on *HandyASM* to provide the functionality needed to create ROM images larger than 64K (not to mention satisfying our desire to tap into *HandyASM*'s great assembly speed). So we must do everything we can to help Carl work the kinks out of *HandyASM*; most importantly, as we discover bugs we must promptly provide him with thorough, detailed bug reports. As a first step in this direction, I ask each of you to do the following at your earliest possible convenience:

- Start by carefully reading the [[[HandyASM Implementation Notes]]] document included with this release
- If you agree with the changes that the new assembler will require you to make to your code, then make them. If you disagree, type your arguments against the changes into a file and submit the file to me right away
- Try to assemble your code, making further changes to your code as needed, and if you have problems with the assembler then create a text file with detailed bug reports. Get this bug report file into my hands right away

On the lighter side, all about *HandyASM* is not sweat and tension. Let's also make sure we expend some good energy considering ways we might craft *HandyASM* to better serve us. Nothing about *HandyASM* is cast in stone yet, and we have a pretty flexible arrangement with Carl regarding feature enhancements. So when musing upon possible assembler improvements allow yourself to think expansively, as you do when working on your Handycraft and Handebug wish lists. Please be sure to record any and all of your reasonable and semi-reasonable ideas.

Submit bug reports and enhancement requests to me as text files (separate files for bugs and enhancements, please) on an Amiga disk. As Cari promised (the guy never fails me), he has set up a bulletin board system for us to which we can post files and from which we can download new releases at our convenience.

Finally, *HandyASM* is complemented by a program named `CmpBin`, also found in `HANDY:,` which program compares and contrasts the binary images created by the old and new assemblers.

## Handebug V1.50

*Handebug* and the Handy monitor code both support a new data packet type, `LARGE_DATA`, which is the data packet used by *HandyASM* whenever possible (perhaps you old Missing Linkers recognize this extension; we borrowed the packet type verbatim from you).

You might be interested to note that the old assembler was capable of creating only one type of data packet, which packet could contain a maximum of 255 bytes of data. Because of this small packet size, the time required to download, say, a 48K file would include the overhead of processing 192 data packet headers, with a *Handebug* / monitor handshake between each packet. To download a 48K file using the `LARGE_DATA` packet type requires 1 handshake and the processing of 1 data packet header. We have noticed that downloading *HandyASM* binary files takes 20 - 30% less time.

## Changes to `6502:`

This release includes several `6502:` files, which files have been modified to work with the new assembler. These changes are supposed to be downwardly-compatible with the old assembler.

*HPRINT* users note: the newly-modified `HPR_CHARSET` macro will generate an assembly warning about `unknown directive .CSET` which you can ignore. This warning doesn't occur when using *HandyASM*.

## Changes to `HANDY:`

As described above in the [HandyASM V0.04](#handyasm-v004) section, `HANDY:` now contains the new `HandyASM` and `CmpBin` programs and the old assembler is copied to the file `asm.old`. If you enter the command `NewAsm` and then invoke the `asm` program you will get `HandyASM`. If you enter `OldAsm`, subsequent use of asm will get you the old assembler. The `NewAsm` and `OldAsm` script files are in `HANDY:` too.

Also, `HANDY:` contains a new monitor which supports the new `LARGE_DATA` packet type generated by the new assembler as described above in the [Handebug V1.50](#handebug-v150) section. Note that this monitor is supposed to be completely downward-compatible with the old assembler, so you don't have to worry about binary files not downloading if compiled with the old assembler (poof).

## Good news bad news (Revised)

I got the good news/bad news stuff a bit wrong. I did manage to get the numbers right: the normal production ROM's are going to cost a buck less than we thought; the OTP ROM's, which we'll have to use if you miss your ship date, will cost a buck more than we thought. I said that missing your date would be a buck worse per unit than it was before, but in fact it's now two bucks worse than before.

## What you must do to start using this release

If you want to assemble using the new assembler, first enter the CLI command `NewAsm`. When you want to go back to the old assembler, enter `OldAsm`.

*HPRINT* users reminder: the `HPR_CHARSET` macro will generate an assembly warning about `unknown directive .CSET` which you can ignore.
