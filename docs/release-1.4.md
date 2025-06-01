# HANDY release 1.4 

15-Nov-90  
Confidential and Proprietary

## Update notes

This is my first update note. It is mostly tying up loose ends and should not cause any grief. Feel free to call me at 415-368-3200 Craig Nelson @ Epyx

Many of you have called saying your trace memory hasn't worked ever since you upgraded cartridge space to 256K. The reason is that the upgrade note Atari sent directs you to flip dipswitch `1` off. This is incorrect, no switch changes are needed. To eliminate any future confusion I am attaching the complete dipswitch document.

For some reason most systems seem to be missing the `sound_examples` directory. This directory included music examples and the Epyx sound effects library. This release contains the music examples and the current Epyx sound effects library (682 sounds). Feel free to use or modify any sounds. The only request is that you not make your game sound so much like another that people will notice the similarity.

The program *Midi2Spl* is included. Documentation is attached.

People anticipating using 4 Mbit carts (512 Kbyte) should give me a call. We have a different cart addressing scheme in the works (using `AUDIN` as the high order address line) that will make cart loading much faster. This will be the topic of a future release note. 2 Mbit (256 Kbyte) users who are having trouble loading from the cart fast enough may also benefit from this, though rewriting parts of `CART.SRC` so that it loads page aligned blocks will help a lot. Again this may be part of a future release. Call me if you're desperate for it now.

## What to do to use this release

Make a directory on your hard disk for *quarterback* to restore to, then run *quarterback* with full subdirectories. 

> ### Note
>
> The sound effects supersede the files in `sound_examples/SFX` which some of you have. They also supersede the library some of you downloaded from the BBS.
