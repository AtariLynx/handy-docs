# HANDY Release 0.5b

28 November 1988  
Confidential and Proprietary

This document describes the V0.5b release of Handy development environment. It includes these sections:

- [New `FileIO` Requester](#new-fileio-requester)
- [Handebug V1.36](#handebug-v136)
- [Handycraft V1.33](#handycraft-v133)
- [What you must do to start using this release](#what-you-must-do-to-start-using-this-release)

## New FileIO Requester

There's a new `FileIO` Requester, which is used by both *Handebug* and *Handycraft*. This requester does a few major new things and a lot of little things, most of which are hidden from you except that you might notice the improved performance.

For instance, your current directory when *Handebug* or *Handycraft* is run is the now the default directory of the requester. The disk and drawer string gadgets show you the name of your current directory the first time that the requester appears.

Also, the requester now supports pattern matching. There are two new string gadgets, `Include:` and `Exclude:`, which allow you to give patterns for the filenames you want included or excluded. If the `Include` field is non-empty then the filename must match to be included. If the `Exclude` field is non-empty then any filename that matches will be excluded.

You have some special pattem match characters that you can use:

- AmigaDOS-style wild-card characters `?` and `#?` match one character or any number of characters respectively
- Unix-style wild-card character `*` matches any number of characters
- The character `|` (a vertical bar) stands for `OR` as in "match this pattern OR this other pattern"

For example, `*.src|*.i|*. mac` will match any filename that ends in `.src`, `.i` or `.mac` (which are all the normal 6502 source files)

One day, when we get around to supporting the ability to save your settings as the defaults, the strings you enter into the requester will be among the info that's saved. For now, you'll have to reenter it every time you run the program.

Still broken: when a disk is inserted, the requester loses track of where you want to be.

## Handebug V1.36

*Handebug* now has line labels in the disassembly listing.

The disassembly highlights the current line if the line is on the display. Coming soon: rules for bringing the current line into the display window if not there already.

Control-key mnemonics are now supported for the display functions (and for quit):

- `CONTROL-C` changes the display to `Code`
- `CONTROL-D` changes the display to `Data`
- `CONTROL-S` changes the display to `Symbols` (not supported yet)
- `CONTROL-T` changes the display to `Trace` (not supported yet)
- `CONTROL-Q` quits *Handebug*

When downloading in the presence of an old symbol file, old symbols are discarded only as long as there are old symbols to discard. In other words, if there's an old symbol file around it's used to throw away any current symbols only if you have symbols. If you have no symbols loaded, then the presence of an old symbol file no has no ill effect.

When you close the *Handebug* display and reopen it, any color modifications you made are now restored.

There are new and improved rules for the Structure Editor. Please refer to the new document [[[Structure Editor Rules of Engagement]]] for details.

## Handycraft V1.33

The date of creation is now present on the *Handycraft* display, as well as the current revision number.

When you ask to have data output, there is a text display line informing you of the progress. The compression currently takes laughably long, so while this text won't keep you from depair at least this text will keep you from wondering whether or not your Amiga has crashed.

*Handycraft*'s memory manager is now working. This guy allocates a `CHIP RAM` buffer and then does chip memory allocations and deallocations within that buffer, doing garbage collection as needed, et cetera. The guinea pigs who have been using this release report much better performance and far fewer `Out-Of-Memory` crashes. This newest release, which no one has used yet, should have even fewer crashes and does garbage collection 5 times faster than before. If you get an `Out-Of-Memory` error it's because your current frame required more `CHIP RAM` than the buffer size, or you ran out of FAST RAM (don't laugh, Steve is already close to the edge).

The default `CHIP RAM` buffer allocated when *Handycraft* opens is the total available *CHIP RAM* minus 40K. You can change the buffer size by using the `C` command. Garbage collection might run slow when the buffer is huge, so you should experiment with the buffer size until you find one that suits your needs.

For a good time (and a lot of text) use the `D` command to debug the memory manager.

*Handycraft* supports the new mnemonic control keys that *Handebug* now accepts. In other words, you can type `Control-C` into *Handycraft* has the same effect as typing `Control-C` to *Handebug*.

## What you must do to start using this release

You must be prepared to enjoy yourself.

Note that this release installs a new *Handebug* and *Handycraft* in your `HANDY:` directory, but doesn't destroy the old ones; instead, it renames the old ones to `Handebug.old` and `Handycraft.old`.
