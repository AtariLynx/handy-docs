# LYNX release 1.60

25-Sep-91  
Confidential and Proprietary

## Update Notes

These revisions of *Handebug*/*Mandebug* ver 1.50 are the latest and greatest of this software. These revisions correct some bugs that were created in the previous revision (ver 1.45) and some cosmetic changes to problems that have been annoying us here at Atari Corp.

Please make sure you read through this entire document before you do anything. This is for your own good, trust me!

## Contents of LYNX release 1.60

LYNX release 1.60 contains this documentation and an update disk in Quarterback v4.3 format.  The update disk contains updates to the programs *Handebug*, *Mandebug* (found in your `6502:` directory) and a copy of this document `Release_1.60`.

*Handebug*/*Mandebug* bug fixes and changes:
1. The `DATA` window is refreshed whenever you switch to it. In the old version (1.45), the screen would not update thus you may have been looking at current memory values but more likely you were not.
2. There are independent display addresses for the `CODE` and `DATA` windows. This is nice for keeping the `DATA` window looking at a memory address while you step through code in the `CODE` window.
3. You can now search the symbol table on numeric value by "editing" the value field.  The `SYMBOLS` window would only let you search in the name field in previous versions.
4. `Handebug` should not assert additional NMI's once it is in debug mode. This used to disturb the stack when the user accidentally hit `F10` (NMI) while in debug, causing a bit of frustration.
5. The `MORE` button on the Bus Monitor now works as expected.
6. The Bus Monitor's messages for the channels are now alpha and not numeric for consistency.
7. The Bus Monitor remembers the last channel looked at when re-opening the Bus Monitor window.
8. "Breaking after n occurrences" should work as expected now in the Bus Monitor.
9. When using the hide `Handebug` feature with the main display window set to `SYMBOLS` mode, the hide `Handebug` window does not trash in certain situations now. Downloading a file or booting the cart does not trash the window.
10. Pasting a new value to a non-empty breakpoint slot now updates the gadget properly to the "enabled" state.
11. If location `0` contained a non-zero value (which occurred if you had an A2091 disk controller), `Handebug` would crash the machine.
12. Support for faster bus monitor setup is in place, and controlled by the config file entry:

```
FASTBUSMON FALSE ; default - no new howard board support
- or -
FASTBUSMON TRUE  ; enables fast mode for when ROMs are changed
```

## What to do to use thise release

If the two programs in your `Handy:` directory are called:

`Handebug` - for the Howard board
`Mandebug` - for the Pinky/Mandy setup

then simply restore the "Quarterback Update Release_1.60" disk to your `Handy:` directory. The two programs mentioned above will be replaced with the current version of the programs (ver 1.50). If you think you might want to preserve your old revisions, you can rename them `Handebug.old` and `Mandebug.old` before restoring the Quarterback Update
disk to your `Handy:` directory.

You can confirm that your updated programs were placed in the correct directory by running `Handebug` or `Mandebug` (you probably use an alias). While the program is loading, you can see the version number displayed at the top of the window (version 1.50); or you can place the pointer over the close box at the top of the window once the program is fully loaded and the version number will be displayed as well.

> ### Note
>
> Your old `handebug.config` file will work with the new `Handebug`/`Mandebug` programs.  **However**, once your current configuration file is loaded into `Handebug`/`Mandebug` upon running either of these programs for the first time, you should re-save your configuration (Shift F6) and a **new** `handebug.config` file will be created with some
"extra stuff" in the file. Once you have saved the **new** `handebug.config` file, you will be unable to use this config file with an old version of `Handebug`/`Mandebug`; but this way all of your preferences (directory, ROM size, color setup, etc.) will be
maintained for the new version of these programs.

This is my first release, but I have tried to maintain the same style in this document you are familiar with from previous update releases. If you have any questions concerning this update or if you find problems with either the new versions of *Handebug*/*Mandebug*,
please do not hesitate to call our BBS at (408)745-2157.

Thank You.

Jerome Strach
