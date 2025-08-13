# HANDY Release 0.9 

4 July 1989
Confidential and Proprietary

This document describes the V 0.9 release of the Handy development environment. It includes these sections:

- [`MUTE` the Music](#mute-the-music)
- [Other `6502:` changes](#other-6502-changes)
- [*HandyAsm* 1.08](#handyasm-108-macros)
- [What you must do to start using this release](#what-you-must-do-to-start-using-this-release)

## `MUTE` the Music

If you would like to mute the playing of music without pausing the song, define `MUTE_USER` and then use the new `MUTE` music macro.

The `MUTE` macro acts as a toggle. You can detect whether music is currently muted by examining the `Mute_flag` field. If the field is non-zero, the music is currently muted.

The `INITHMUSIC` call causes music to become non-muted.

## Other `6502:` Changes

The `GETSWITCH` and `GETJOY` macros no longer perform the `AUTO_TIMEOUT` reset functions. This logic has been moved to the `TIMEOUT` macro defined in `display.src`. Also, the `SWITCHES`-reading logic has been corrected to test explicitly for the `PAUSE_SWITCH` bit.

The old `cart.src` didn't get distributed just before CES. Now it's distributed. An invalid `LDA IODAT` now reads `LDA IODAT_RAM`.

The auto-timeout constant is adjusted to cause timeout to happen in 1 hour rather than 1.5 hours (when external power is present).

`END_ZPAGE` macro now warns you if your allocations go out of zero-page.

## *HandyAsm* 1.08 Macros

Macros now can refer to their arguments by value as well as by name. The format for reference to argument by value is `?%FN` where `%F` stands for a format argument (`%d` equals decimal, `%x` for hexidecimal, `%c` for character); the `N` stands for the usual macro argument number.

For example, consider the new macro `ECHO_VALUE` (defined in `sys.mac`):

```
#MACRO ECHO_VALUE 
	.ECHO "Value of ?0 is ?%d0 ($?%x0) 
#ENDMACRO
```

The following invocation:

```
		.ORG $1234 
Start
		ECHO_VALUE Start
```

would result in the following echo: 

```
Value of Start is 4660 ($1234)
```

This has a known bug: you can't use `***` as an argument to pass the current address. For now, to pass the current address you need to assign a temporary value:

```
current_addr 	.= *
	ECHO_VALUE 	current_addr
```

## Other HandyAsm 1.08 Features

Assembler is a bit faster.

When the `+E` argument has been specified to have equates included in the symbol file, the assembler now automatically defines the constant `ASM.EQUSYMS`.

Error reporting is improved.

When defining ASCII characters as immediate arguments (eg `LDA #'a'`), you can now use either single or double quotes to delimit the character. Single quotes work as always. Double quotes allow you to define the character using its current .CSET value. This will be useful to `HPRINT` users.

Multiple symbols can be checked with `#IFDEF` and `#IFNDEF` by using the `AND` and/or `OR` operators. What used to take:

```
temp_value .= 0 
	#IFDEF SYMBOL_1
temp_value .= 1 
	#ENDIF 
	#IFDEF SYMBOL 2 
temp_value .= 1 
	#ENDIF 
	#IF temp_value = 1 
	; Do stuff
	#ENDIF
```

now takes:

```
	#IFDEF SYMBOL 1 | SYMBOL 2 ; 
		Do stuff 
	#ENDIF
```

The old way of doing listings is restored. To get the new kind of listing, which requires the presence of an accurate `.bin` file, you must use the `+C` (for Complete listing) command-line argument.

The final listing solution, which will do the job everyone wants it to do, is still pending.

Flat `.TF` symbol references are now allowed. Any `.TF` segment can refer to another `.TF` segment's symbols.

Note that the current listing generated with `+c` will generate bogus addresses when referencing external `.TF` symbols. It'll be fixed real soon now, honest, would we lie?

## What you must do to start using this release

You must ask me to install it on your system. After the installation, the old `6502:` directory will be renamed to `SYS:6502.0.8` and the old `1.04` release of the assembler will be renamed to `asm.old`.
