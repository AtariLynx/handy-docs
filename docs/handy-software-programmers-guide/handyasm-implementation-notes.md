# HandyASM B0.04 Implementation Notes 

18 March 1989  
Confidential and Proprietary

Here's some notes about the particulars and peculiarities of *HandyASM*. Hopefully, these notes will help you identify and avoid the most common problems as you come up to speed on our new assembler.

You are strongly encouraged to help expedite the *HandyASM* debugging by diligently reporting any problems you have. Every time you find new errors or get junk results, open (or reopen) an Amiga text file and make an entry describing the problem. Then give a copy of the text file to me. If appropriate, give me code that can reproduce the problem (doesn't matter how much code it takes, even if you have to give me your whole program). After I receive and log the bug reports I'll fire them off to Carl immediately, taking advantage of the fact that Carl is going to try to provide us with a bug-fix turn-around time measured in hours! By the way, don't be concerned about releasing confidential source code to Carl, he's eminently trustworthy and he's under a strong non-disclosure agreement. Besides, he was the tenth person or so in the entire universe to learn about Handy, so there's not much he doesn't already know.

Please report all problems you find, even if you think someone else already reported the problem.

## HandyASM Implementation Notes

*HandyASM* automatically defines a symbol named `ASM.VERSION`, which isn't defined by the old assembler and therefore (unless you yourself define this constant) can be used in your source code to test whether your files are currently being assembled by the old or the new. This allows you to change your code in a way that will use features of the new assembler but will still compile with the old, as long as you wrap those new sections of your code within a `#IFDEF ASM.VERSION` / `#ENDIF` pair. This simple technique is demonstrated below in the `.RUN` directive discussion.

The most complicated problem with using *HandyASM* is fortunately one of the problems you'll encounter least often: because the assembler is a one-pass engine, certain limitations must be put on your freedom to use symbolic forward referencing (using a symbol before it's defined).

In general, a reference to a not-yet-defined symbol is allowable only in those cases where the assembler, on encountering such a reference, can simply leave a "hole" in the binary being created, which hole is to be filled with valid data later during the resolution pass (when all symbols must be defined). For instance, in the following example the forward reference to the symbol `Buffer` is valid:

```
		LDA #<Buffer 
		STA ptr
		LDA #>Buffer 
		STA ptr+1
		LDA (ptr)
		... 
Buffer 	.DS 1
```

Note, however, that if `ptr` isn't defined by time *HandyASM*'s first pass reaches this code segment then the `STA ptr` instruction will generate a store to an unresolved 16-bit address, and the `LDA (ptr)` instruction will generate an `Improper addressing mode` error.

But if the assembler ever has an immediate need to know a symbol's value - for instance if the value is essential to the building of the binary image (such as the argument to a `.ORG`) or if it's part of an assembly decision point (such as a symbol used in an `#IF` equation) - then that symbol must be defined; if it isn't, the program is in error. Consider the following code, which is in error because the `#IF` can't be resolved; `Buffer` isn't defined when the `#IF` is encountered, so *HandyASM* has no way to decide how to proceed:

```
#IF Buffer > $80 ; Error: Buffer not defined yet
		LDA # 1 
#ELSE 
		LDX #1 
#ENDIF
	...
Buffer 	.DS 1
```

In short, *HandyASM*, by virtue of its single-pass nature, imposes the limitation that certain of its critical directives not be given symbolic forward referencing arguments.

This limitation is minor, can be worked around easily, and in the end can be dismissed as the price we pay for the speed of single-pass assembly. The immediate impact of this limitation is going to be minor anyway: during my preliminary test assembly of most everyone's source code I didn't find a single illegal forward reference except for the occasional `.RUN` forward reference, which is the easiest to fix, as described in the `.RUN` discussion below. So don't sweat this stuff.

The *HandyASM* directives and operators that specifically disallow symbolic forward referencing include: `.RUN`, `.SET` (aka `.=`), `.VE`, `.ORG`, `.DS`, `#IF`, `#MACRO`.

New directive name: `.SE` (or `.SET`), which is equivalent to `.=` (note that `.=` is still a valid directive).

New assembler directive: `.VERSION` (or `.VE`), which allows you to set version and revision numbers for your listings. The format is this:

```
.VERSION version-number, revision-number
```


The `.VERSION` arguments can be values or symbols (but not forward references).

The `.PRINTER` directive is not supported. Instead, use the `.PAGE` directive, with this format:

```
.PAGE page-length, page-width
```

Note that the `.PAGE` directive currently forces the assembler to start a new page of your listing, in the same way that the `.EJECT` directive command does.

Regarding `.PAGE` causing a page eject, which I don't like, I propose the following minor change to the assembler: `.PAGE` will be used only to set up the parameters of your listing page, and will not force a page ejection. The `.EJECT` directive will eject the page as normal. This is the way it should be. This change doesn't affect the ability to eject the page whenever you want as you can always add a `.EJECT` after your `.PAGE`, while at the same time it removes the undesirable effect of page ejection whenever you change the page parameters, whether you want it or not, even if it's the very first thing you do in your source file. If no one objects, I'm going to ask Carl to make this change.

The C-style comment starting with `/*` and ending with `*/` is not supported.

The `.RUN` argument can't be a symbolic forward reference. If you specify a symbol as the `.RUN` argument, the symbol must already be defined.

```
GOOD EXAMPLE: 							BAD EXAMPLE:
	Start										.RUN 	Start
		.RUN 	Start						Start
		SEI										SEI
		CLD										CLD
```

Unfortunately, since most of you used `testsprite.src` as your seed program and since `testsprite.src` used to contain a .RUN with a forward reference, most of you will find that your `.RUN` generates a *HandyASM* error.

Here's a nifty new feature of the `.RUN` directive: if no argument is given the current PC is used as the argument.

```
ANOTHER GOOD EXAMPLE:
	Start	.RUN ; Sets run address to Start
			SEI
			CLD
```

However, the above example will work only with the new assembler. To use this in a file that must assemble under both the old and the new, you might use the following technique (the extra lines of which can be edited out easily after you decide to commit completely to the new assembler).

```
AN EXAMPLE THAT WORKS UNDER BOTH OLD AND NEW:

	#IFDEF ASM.VERSION
Start	.RUN ; Sets run address to Start 
	#ENDIF 
	#IFNDEF ASM.VERSION 
Start	.RUN Start ; Sets run address to Start
	#ENDIF
		SEI 
		CLD
```

The `.CHARSET` directive's name is changed to `.CS` (or `.CSET`). This change was made to allow all assembler directives to have unique two-character representations. The `.CH` directive is equivalent to `.CHAIN`.

I've noticed that several of you use some pretty crazy characters in your labels, even though the old assembler's manual states that the only characters allowed in labels and macro names are `A-Z`, `a-z`, `0-9`, `_` (underscore), `.` (period) and `!` (exclamation point). *HandyASM* too allows you to use all of those characters, and further allows the characters `$` (dollar sign), `%` (percent sign) and `@` (ampersand). All other characters are explicitly not allowed by *HandyASM*; in particular, labels that use the characters `(` (open paran), `)` (close paran) and `#` (pound sign) will cause *HandyASM* to barf. We could change *HandyASM* to allow these characters, but, according to Carl, "The lexicon can become very complicated and slow to parse if you let the spaces [the character sets of the lexicon] overlap." That "slow to parse" part of his statement was the significant one for me: I opted for speed and instructed Carl to leave the allowable characters as currently defined.

Note, as Steve Landrum pointed out, that though *HandyASM* will allow you to use the `$` and `%` characters in your labels, to do so is asking for trouble as these are the same characters used to make numbers. One slip (of the space bar, for instance) can lead to really frustrating mystery bugs. So you might want to think again about using these characters in your names. In fact, I could ask Carl to protect us from ourselves by intentionally excluding `$` and `%` from the allowable characters. This would be fine by me. What do you think?

Currently, every label and macro must have a unique name. We could relax this constraint by having Carl manage label and macro names separately, thereby allowing both a label and a macro to have the same name. For me, having macros with the same names as labels in my code would make me nervous, though just a little bit nervous. What do you think? To split labels and macro names into two symbol spaces would consume a fair amount of Carl's time, which time he would otherwise spend implementing other features for us. On the other hand, having two symbol spaces would speed up assemblies a bit. Hmmm ... what do you think?

The `.END` directive is no longer required at the end of a file, though if `.END` is seen file processing ends as usual. Also, *HandyASM* successfully reads and processes the very last line of a file.

Well, we didn't get one of the things I really wanted (though I understand why, and we got a very useful substitute). We can't have local labels that exist only within the range of a macro expansion.

If locals within a macro were not visible outside of the macro and external locals could not be seen inside a macro, then any use of locals within a macro would be completely safe. Consider the following code segment, which would generate valid code if macro locals were visible only within the domain of the macro expansion :

```
	#MACRO fixcount 
		LDA		count
		BNE		.10
		INC		count
.10
	#ENDMACRO

	...

.10		fixcount 
		STA		count2
		BNE		.10
```

In the real world, however, the `.10` local label in the above macro would be added to the list of currently-defined locals every time the macro was invoked. The macro expansion in the above code segment would result in the definition of two `.10` local labels, which is an error.

To help satisfy the need for macro local labels *HandyASM* provides us with a powerful new tool: the `??` macro operator. Whenever the `??` operator is used within a macro it expands to the name of the macro appended with a macro activation number (which number is incremented by one with each use of the macro). This allows us to create a macro that generates unique labels every time the macro is invoked! But there's a catch (there's always a catch): if `??` is used to create a label, that label (like any label) will limit the domain of any currently-defined local labels. For instance, if we mindlessly modified the above macro to use the `??` operator:

```
	#MACRO fixcount 
		LDA		count
		BNE		??
		INC		count
??
	#ENDMACRO
.10
		fixcount
		STA		count2
		BNE		.10
```

We're still not happy, for when the macro is expanded we get a `local label not defined` error:

```
.10		LDA		count
		BNE		fixcount.0001
		INC		count
fixcount.0001:
		STA		count2
		BNE		.10		; Error: local no longer defined
```

So we despair. But wait, to the rescue comes the twisted mind of Scott Nelson, enjoining us to create a label that's not a label. Scott points out that when a label is defined using `.=` (aka `.SET`) local labels aren't reset. Armed with this fact, the `fixcount` macro can be modified to define a macro label that is both unique and local ("local" in that it can coexist amidst normal locals without disturbing them):

```
	#MACRO	fixcount 
		LDA		count
		BNE 	??
		INC		count
??		.=		*
	#ENDMACRO

		...

.10		fixcount
		STA		count2
		BNE		.10
```

which when expanded creates exactly the desired code segment:

```
.10		LDA		count
		BNE 	fixcount.0001 
		INC		count
fixcount.0001:	.= *	; Doesn't disturb local labels
		STA 	count2
		BNE		.10		; Local label still defined
```

Very clever, Scott, and most impressive considering you invented this solution no more than 75 seconds after learning about the new `??` operator.

One minor drawback to the `??` macro operator: macro labels defined using `??` become real symbols, with entries both in the assembler's symbol table and in the symbol output file. Currently there is no way to turn this off.

Command-line arguments can start with either `+` or `-` as is your pleasure. The argument characters can be either upper- or lower-case, and can appear in any order. The `L`, `S`, `R` and `O` arguments accept an optional file name. Also, note that you can specify more than one source file on the command-line.

The command-line argument `+I` was incorrectly implemented as "include path" rather than "include equates." This will be fixed soon, so don't start using it to mean "include path".

*HandyASM* allows you to `.INCLUDE` files to any depth. Likewise, you now may nest the `#IF` and its many variations to any depth. Actually, that's not true: in the end, you're limited by the amount of available RAM.

On examining our code, Carl has noticed that several programmers (including me) make copious use of `{` and `}` in expressions, even when these symbols aren't necessary (my guess is that the C programmers do this most often). Carl wants us to be aware of the fact that each instance of a `{` slows his expression evaluator dramatically, even if the bracket serves no purpose other than as a safety wall, as in:

```
VAL .EQU { 10 } 
	#IF VAL / 2 < 20
```

He advises us to have a look through our code and purge unneeded `{` / `}` pairs (such as the above) wherever possible. That is, *if* we want faster assemblies.

## Known bugs:

- `#ELSE` doesn't seem to be working yet (not that anyone will notice)
- `.CSET` isn't working correctly, which will cause `HPRINT` users to get junk text
- the `??` macro operator is flaky when being used to create a label

## Not implemented yet:

- the `.TF` (for `Target File`) directive
- using macro arguments by value rather than by text during macro expansion
- command-line arguments `+D`, `+I`, and `+X`
- some other stuff that I can't remember just now
