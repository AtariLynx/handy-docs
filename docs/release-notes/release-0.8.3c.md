# HANDY Release 0.8.3c

1 June 1989, 11:45  
Confidential and Proprietary

This document describes the V0.8.3c enhancement release of the Handy development environment.

`hsfx.src` and `hmusic.src` with channel count bug fix and rearranged *HSFX* driver to allow. `SEPARATE_HMUSIC_HSFX` to work correctly. Also added bullet-proofing to `HSFX` calls in case `HMUSIC` loses its mind, which it seems to be doing when a music note comes in as a sound effect (such as a drum beat).

`sprite.mac` has some pre-CES bullet-proofing, with `STZ SDONEACK` before each write to `SPRGO` in the `SPRITES` and `RESPRITE` macros.

*Handebug* Vx.xx (22:16:36) with less crashing around `LOAD CART` operations.

All files changed are first saved as `<filename>.old`.
