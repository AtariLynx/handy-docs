# HKCS Update Notes 27 March 1989

## HKCS Changes

Rather than needing to encounter one channel only in an entire `.all` file, *HKCS* now needs to see only one channel per track or sequence.

When writing the `.spl` file, sequences and tracks are numbered starting from 1.

If Sequence 1 is the dumb old sequence that `KCS` plugs into Sequence 1 by default, *HKCS* ignores the sequence. However, if Sequence 1 contains data that's not the dumb old sequence normally plugged in by *KCS*, the sequence is processed normally.

Sequence `Z` (aka Sequence 35) is now ignored completely.

Songs in the KCS file are ignored completely.

Each track / sequence ends with the SPL command `ems`.

The lowest octave put out by KCS will now appear as octave -1 in the `.spl` file.

The rest now appears correctly with tracks and sequences that start with a rest.

*HKCS*'s default bar-duration is 96 if you don't provide one on the command line when invoking the program.
