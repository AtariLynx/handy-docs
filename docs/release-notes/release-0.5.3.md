# HANDY release 0.5.3

19 December 1988  
Confidential and Proprietary

In anticipation of a celebration of a successful presentation of our machinations, we have prepared and present to you now the Handy Celebration Release. All of this software is guaranteed to work with a smile!

This document describes the V 0.5.3 release of Handy development environment. It includes these sections:

- [`S:HC` and Notes on Stack Size](#shc-and-notes-on-stack-size)
- [Handycraft V1.34](#handycraft-v134)
- [Handebug V1.39](#handebug-v139)
- [Download](#download)
- [HSFX 0.1](#hsfx-v01)
- [What you must do to start using this release](#what-you-must-do-to-start-using-this-release)

## `S:HC` and Notes on Stack Size

Are you ready for this? Seems that many of *Handycraft*'s problems over the past few months come down to stack overflow. I didn't know, did you? If you enter `stack 8000` just before running *Handycraft*, boy do things work better. What a pisser.

We have to devise a convenient way for your stack size to be increased automatically. Just putting `stack 8000` in your `startup-sequence` isn't enough, as new CLI's don't inherit the stack parameter from the parent CLI's.

For now, you must remember to manually set stack size. If you are using a script file to start up *Handycraft*, then put it there. If you aren't using a script file, you might consider using the one that's now released into your `S:` directory, named `HC`. If you start up *Handycraft* using *X HC* then the stack is set up for you and Handycraft is run.

## Handycraft V1.34

New *Handycraft* has a few bug fixes and does only one thing different: compression now runs about 1000 times faster than before. Really. The `Fast Compression` mode is now implemented. You don't get any choice yet, you just get `Fast Compression`. `Fast Compression` is much faster, but also does a slightly lesser job of finding the ultimate compaction. Most of the time the difference is very small (I saw one example where 576 bytes became 577 bytes), but it can be as bad as 5% larger. Ouch. On the other hand, a 20-hour job now takes 45 seconds. Hmm. Anyway, soon you'll have a choice.

You should have a stack size of at least 6000 (better would be 8000) before running *Handycraft*.

## Handebug V1.39

See the attached document, named `v1.39.changes.doc`, for details.

## Download

See the attached document, named `v1.39.changes.doc`, for details.

## HSFX V0.1

Well, this is a pre-release of the audio editor. You can't do much with it yet unless you have ready access to Handy hardware, but it's fun to play with so here it comes.

## What you must do to start using this release

To use the new *Handycraft*, you should first set your stack to 6000 (or, better yet, 8000).
