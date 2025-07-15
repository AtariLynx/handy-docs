# Handy software programmer's guide and notes

Robert J. Mical
Created: 26 January 1988 Last modification: 1 June 1989

## Table of contents
2 Handy Software Development Overview
10 Handycraft User's Guide
11 Handebug User's Guide
12 Handebug Structure Editor
14 HSFX User's Guide
27 6502 Cross Assembler Notes
28 HandyAsm Implementation Notes
34 The Two Scaling Algorithms
35 Handy Audio Hardware Overview
43 Handy Interrupts and CPU SLEEP
47 Creating a Handy Display
59 Positioning of the Handy Display and Sprites
68 HPRINT Text Routines
73 CART - Game Cartridge I/O System
87 HMUSIC - The Handy Music Driver
99 HSPL - The Handy Sound Programming Language

## Handy software development overview

This section presents an overview of the Handy software development environment. The subsections found below are:

- Handy is Lynx, Lynx is Handy
- System Philosophy - Our Gift to You

### Handy is Lynx, Lynx is Handy

We begin this Handy Developer's Guide and Notes with a clarification and a mildly interesting observation of human nature.

Listen: Epyx has sold its new game system to Atari. Atari has chosen to name the new game system "Lynx." This is good for Atari and probably for the buying public too.

However, no matter what it's called in the real world, the system's creators and its first batch of game developers will always know it as "Handy." From the very first day the project's code-name was "Handy." Most of the software tools were named along the Handy theme: *Handycraft*, *Handebug*, *HandyAsm*. Several of the hardware pieces were named likewise: Howdy, Pinky, Mandy. The task of changing everything over to the new name is as daunting as it is needless, and it would probably be a bit demoralizing too. So it seems that "Handy" is here to stay, at least within our development community.

The Epyx folks tried to switch over to calling it "Lynx," really we did. But we failed. At the same time we tried to help the Atari marketing folks recognize the wisdom (ahem) of leaving it named "Handy," but we failed that too.

So instead, the universe and all in it are required to endure yet another new rule: those who work with both Epyx and Atari will have to deal with the fact that the machine is both "the Handy" and "the Lynx." It's all in the Zen of the thing.

Long live Lynx. Long live Handy.

### System philosophy - our gift to you

In the beginning, from the beginning ...

At all times during the design of the system's hardware we kept the programmer in mind: many features were added to help shorten program development time, others were added to help the programmer attain that ever-sought-after Holy Grail: improved execution rate.

For example, the hardware provides certain graphics functionality (e.g. the multi-faceted sprite engine) that all programs need. If the hardware didn't provide this functionality the programmer would have to spend great quantities of precious time attempting to implement it in software. Furthermore, the hardware performs complex tasks (e.g. hardware math) that would consume vast amounts of CPU time if performed using software.

And, as with the hardware, we kept the programmer in mind at all times during the design of the system software. Our body of software support code provides a complete set of macros that provide a unified quality to all components of the program. Furthermore, use of the system software eliminates unnecessary programming jobs two ways: by providing low-level routines to take care of grunt-work details that are unimportant to the programmer, and by providing a complete set of the high-level system drivers that all programs need.

By using the low-level system macros the programmer is freed from having to tend to the more bothersome, more exacting system details (e.g. being required to write to the hardware display registers at just the right time). But the time-savings gained by using the low-level system code is small compared with the time saved by using the high-level system drivers (e.g. sound effects driver and the ROM access routines) rather than creating these drivers from scratch. All of the essential system drivers, already designed and debugged, are available to the Handy developer. The developer trades the small amount of effort required to learn and incorporate the system code with the enormous effort of reinventing the wheels. This is a very good deal for the programmer, because several of the drivers are massively complex and required many man-months to develop. Instead of requiring the programmer to create and debug these drivers, the programmer's job is made easy for each of the system's drivers can be plugged into a program with very little effort.

All of the software support and powerful features of the hardware are presented to the programmer with our compliments. Enjoy.

By taking full advantage of all that our system provides, the programmer should find that most of the hardest programming tasks are rendered trivial. This will leave the programmer with much spare time, which time can be devoted to improving game quality. This is our grandest intent. This is our gift to you. We sure hope you use it.

## Development environment overview

This section presents a broad-brush overview of the development environment by describing the individual components of the environment. These components are presented below, in these subsections:

- Hardware
- System software
- Software tools
- Documentation

### Hardware

The hardware environment consists of Amiga computers, Howard boards with their accompanying Howdys, and Pinky boards with their accompanying Mandys.

The Amiga computer can be any of the Amiga line, from the A2500 to the A500. We suggest that each primary developer use an A2000 or better, configured with at least 3M of RAM and 40M of mass storage.

Note that a reasonably well-equipped A1000 can be used in place of the A2000.

Test departments and work stations that will be used only occasionally can get away with the low-cost A500 (and external floppy) with little inconvenience. Downloading through the A500 takes a bit longer because the file is read from a floppy disk, but the extra delay is hardly noticeable. However, full-scale software development on an A500 is unreasonably clumsy and slow and will remain so until A500 RAM and hard disks come available.

The "Howard board" (with its Howdy) is the primary component of the system's debugging hardware. The Howard supports all normal debugging operations such as viewing a hex dump or a symbolic disassembly, and monitoring program flow using controls such as breakpoints. But the Howard also provides features normally found only in sophisticated, expensive hardware state analyzers: the Howard monitors all activity on the system bus, recording as many as 128,000 of these events, and allowing a break to the debugger to occur when a series of events from the simple to the fantastically complex has been witnessed.

"Howdy" is the name of the Howard board's Handy. The Howdy is a completely self-contained Handy. When Howdy is executing a program it does so with no interference from the Howard board (though Howard continually monitors Howdy's bus and NMI line).

The Howard is a sort of shell around the Howdy. From the Howard you can peer into the Howdy, while from the Howdy you are completely unaware of and completely uninfluenced by Howard (that is, until the programmer takes such explicit action that causes Howard to modify Howdy).

The Howard is controlled by an Amiga program called *Handebug*, which is described below in this section's "Software Tools" sub-section and also in this chapter's "HANDY:" section.

For developers who need several development Handys - for instance, when developing multi-unit games or when play-testing - there is the Pinky/Mandy pair. Pinky/Mandy is a low-cost version of Howard/Howdy.

"Pinky" is a stripped-down version of the Howard board. Pinky supports only 2 development functions: 

- downloading programs to and/or executing them in Handy RAM; 
- downloading programs to and/or executing them in a simulated Handy ROM cartridge. 

Other than these functions, normal debugging operations cannot be performed through a Pinky.

"Mandy" is a Modified-Handy, modified just enough to be able to communicate with a Pinky.

The minimum hardware development configuration is a Howard/Howdy connected to an A2000, an A2500, or an A1000. Additional Howard/Howdy and Pinky/Mandy systems can be added to the environment either by linking them by switchbox to the one Amiga or by acquiring additional Amigas.

### System software

To support and simplify the development effort we provide you with files that define a vast assortment of system macros, definitions, drivers and utility routines. Included in this collection are items that range from the obvious such as the hardware register definitions to code as complicated as the music driver and the text-rendering routines.

These system files are kept in the sub-directories of the Amiga's `6502:` directory. The `6502:` directory is described below in this chapter's "6502:" section.

Developers are strongly encouraged to use the system's definitions, macros and drivers whenever possible. There are many advantages for doing so, and several disadvantages for not doing so.

First of all, using our stuff will speed your software development dramatically. When you use our macros and utility routines many of the system grunt-work details are tended to automatically. Furthermore, very complex system routines (such as the multi-unit communications driver) are already designed, debugged, and ready for you to use. And yet, whether a given sub-system is simple or complex, the programmer interface is always uniform, simple, and well-documented.

Another reason for using the system-supplied code is to help assure that your game will be upwardly-compatible with future revs of the hardware. We will design new hardware with a mind to any requirements or limitations imposed by the system software. If you write your own code rather than using ours, you have no such assurance.

Also, if you use our stuff then it will be much, much easier for us to support your ongoing development, especially when such support involves bug-hunting or the search for ways to improve your game performance. The more our system software is used, the more quality and quantity of support we can provide, and the more our system software improves. It's obvious that we will do better by providing more support to those who choose to live within our system.

Finally, perhaps most importantly, with many programmers learning and exploring the system code we are sure to discover better ways to accomplish the system goals. In fact, this has already been the case: programmer feedback often has lead to extensive increases in code quality and decreases in code size. And these improvements, when reported to the Handy development support group, ultimately are made available to all developers in the Handy community. Meanwhile, with each enhancement the support code becomes increasingly compact and efficient, evolving continually into ever more powerful, highly-tuned system software.

### Software tools

The development environment includes a healthy number of software tools. A few of the tools are generic - "text editor" and "paint program," for instance - but the majority of the software tools were created specifically to provide support during the many aspects of Handy software development.

The Handy-specific-tools can be found in a directory named `HANDY:` on the Amiga. This directory is described below in this chapter's "HANDY:" section. The generic development tools may be found in any number of places on the Amiga. For more information, refer to this chapter's "Where Things Are on the Amiga" section.

Here's a short list of the tools you'll use:

- A text editor
- A paint program for creating and modifying sprite imagery
- *Handycraft*, for writing out Handy Sprite Control Blocks (also known as SCB's, which is pronounced "scabs" in remembrance of the ankle sandpaper) and writing out compressed image data
- *HSFX*, the real-time sound effects editor
- A MIDI music editor
- *HSPL*, for compiling music text files into Handy music data files
- *HandyAsm*, a multi-featured 65C02 cross-assembler
- *HandyROM*, for translating your program's binary file(s) into a "ROM image" (a file configured for downloading into a Handy game cartridge)
- And finally, *Handebug*, for downloading, debugging, and playing!

### Documentation

The value of good documentation is immeasurable. We have striven to provide you with good documentation.

Every effort has been made to make sure that all Handy references materials are accurate and complete. For starters, the documentation was written by the same people who created the software and hardware. Then over the years the text has been reviewed and re-edited on an ongoing basis. The accuracy of the material was increased by the usage of it by the developers who have come before you.

The result is a robust collection of Handy documents, complemented by many illustrations when the going gets rough and the descriptions get hairy.

There are many Handy development documents. The primary ones include: 

- Handy Developer's Guide and Notes
- Handy Hardware Specification
- Handy Appendix 2: Hardware Addresses

## Where things are on the Amiga

|||
|---|---|
|`C:`|`C:` contains the Amiga system programs distributed by Commodore. The directory is preserved exactly as it came from Commodore, except that the command `Execute` is copied to `X` and `Delete` is copied to `Del`.|
|`C2:`|`C2:` contains a load of other tools. Everything in `C2:` is either ours to distribute or from public domain. Oh yes, lots of goodies are kept in this directory. See the `README` file (enter `cd SYS:c2` then `more readme`) for a complete list of this directory's contents.|
|`RAM:`|Temporary files are kept here. If your system resets, these files go away.|
|`HANDY:`|These are your Handy-specific tools. Remember, don't make any changes to the HANDY: directory, except for editing the HD.User.defs file, unless you first copy the HANDY: directory to your own directory and change the HANDY: assignment to refer to the new directory.|
|`6502:`|This directory is the head of several subdirectories which have the Handy system software's 6502 definitions, macros and source code. Remember, don't make any changes to the `6502:` directory unless you first copy the `6502:` directory to your own directory and change the `6502:` assignment to refer to the new directory.|

## `6502:` 

The `6502:` directory contains subdirectories which have all the definitions, macros and source code needed to do Handy software development. There are several files in these directories that you are required to use, and many others that you may choose to use.

`6502:` is a logical assignment that refers to some actual directory. The default `6502:` directory is `SYS:6502`. You can change `6502:` to refer to any directory of your choosing.

The details of the files and the routines contained in the `6502:` subdirectories are detailed all throughout the remainder of this manual. Here's a listing of the `6502:` subdirectories:

|||
|---|---|
|`src`|This directory has 6502 source code that you might find useful. Included are math routines, ASCII routines, et cetera.|
|`include`|Here are the system-wide include files, which all developers should use.|
|`macros`|Here we've got macros that do most of the Handy-specific stuff.|
|`fonts`|The fonts directory contains the fonts used by the system's HPRINT text routines.|
|`examples`|This directory contains several examples of techniques for creating displays, using sprites and the math hardware and the other components of Handy. The example programs in this directory exercise all of the source and macros provided for in the other `6502:` directories.|

All pathnames in the system files (and hopefully all of the pathnames in your program) start with the logical `6502:` at the head of the pathname (e.g. `6502:macros/display.mac`). This means that if you assign `6502:` to some directory other than `SYS:6502`, subsequent assemblies will access files from the new directory.

This allows you to do something like copy all of the `6502:` files to `RAM:` and then assign `6502:` there. Because file access from `RAM:` is faster than hard-disk file access, subsequent assemblies will run faster:

```
makedir RAM:6502 
copy 6502: RAM:6502 all 
assign 6502: RAM:6502
```

If you're considering making changes to the `6502:` directory, take care. If a system release is installed on your Amiga, you may lose any changes you've made to `6502:` files.

The danger comes from the fact that system releases often result in `6502:` files being deleted, moved or overwritten. Because of this, you should avoid adding to or otherwise modifying the original `6502:` directory.

However, it's safe for you to create your own copy of `6502:` and then make changes in your copy. This is because system release files are written explicitly to the literal `SYS:6502` directory, not the logical `6502:` directory, specifically to allow programmers to maintain their own copies of the `6502:` directory. By copying the `6502:` directory to a new directory and then changing the `6502:` assignment to refer to the new directory, you can then make changes galore to the `6502:` files with no risk of the changes being overwritten by a subsequent system release (which, again, would go to the `SYS:6502` directory, not to your new `6502:` directory). For example:

```
makedir SYS:6502.mine 
copy 6502: SYS:6502.mine all 
assign 6502: SYS:6502.mine
```

If you do something like this, you probably should edit the `6502:` assignment in your `s:startup-sequence.epyx` script file to make sure that the next time you turn on your Amiga `6502:` will still refer to your new directory.

## `HANDY:`

All of the Handy-specific development tools and data files are kept in a logical directory named `HANDY:`. The normal Amiga boot process assigns `HANDY:` to an actual directory and adds it to the Amiga search path. After a normal boot, any of the development tools can be invoked from any directory just by entering the name of the program.

The primary `HANDY:` development tools are detailed in the chapters HandyAsm User's Guide, Handycraft User's Guide, Handebug User's Guide, and HSFX User's Guide. The software tools kept in `HANDY:` include:

|||
|---|---|
|*HandyAsm*|*HandyAsm* is the Handy 65C02 cross-assembler.|
|*Handycraft*|*Handycraft* is the Handy sprite tool, used for two main purposes: translating Amiga image graphics files into Handy image data files, and creating Sprite Control Block text files.
|*Handebug*|*Handebug* is the primary Handy development tool. It is a symbolic debugger and a state analyzer rolled into one.|
|*HSFX*|*HSFX* is the real-time Handy sound effects editor.|
|*Handypic*|The *Handypic* program accepts as input the name|
|*Download*|The *download* program|

The system data files kept in HANDY: include:

|||
|---|---|
|`monitor.bin`|`HANDY:monitor.bin` is the file that contains the Handy side of the debugging code. After you reset a Chip system you BOOTSTRAP the system using Handebug to download the monitor.bin file (as specified in the `xxx` field; note that the xxx field of the BOOTSTRAP requester should be left blank). Note that the Howard board system requires you to download no files at all; after you reset the Howard board, all you have to do is hit Handebug's NMI (F10) to establish communications.|

`HANDY:` must be assigned in the `s:startup-sequence.epyx` script file to refer to your Handy directory. The `s:startup-sequence.epyx` script file is executed during the normal Amiga boot. By default (when the Handy software is first installed on your Amiga) `HANDY:` is set to refer to the `SYS:Handy` directory. But, as with any Amiga assignment (such as `6502:`), you are free to change `HANDY:` to refer to any directory of your choosing. However, unlike the `6502:` assignment, you have little reason to change the `HANDY:` assignment so you'll probably just leave it alone.

After assigning `HANDY:` the `s:startup-sequence.epyx` script file should add `HANDY:` to the path that's searched when you specify a program that you want to run.

The commands in `s:startup-sequence.epyx` should look something like this:

```
assign HANDY: SYS:handy 
path HANDY: add
```

The `HANDY:` directory may be updated with any system release. Any changes you make to this directory may be destroyed accidently by a later update. Because of this, except for the Handebug file `HANDY:HD.User.defs` (which you are allowed to modify) you should avoid adding to or otherwise modifying this directory.

## CLI environment

You probably know that script files are in a logical directory assigned with the name `S:` and if you didn't know when this sentence started you know now.

I have the `S:startup-sequence` file edited to start up a bunch of background utilities. You can experiment with these, and edit the `S:startup-sequence` file to delete them and/or add others. When you boot using my startup-sequence, here's what you get:

|||
|---|---|
|*heliosmouse*|This program causes the window under the mouse to be automatically activated. Without this program, you have to click in a window to activate it. On the other hand, without this program a selected window remains selected until you intentionally select another, which helps with menu operations. Some people like it, others don't. To turn *heliosmouse* on or off, enter `hellosmouse`. The program politely tells you whether it's coming or going.|
|*mousezoom*|Created by Mr. Meissen himself, this program detects when you move the mouse rapidly and accelerates the mouse beyond its normal 1:1 ratio. Doesn't seem to get in the way of anything, and is a great boon. To turn *mousezoom* on or off, enter `mousezoom`. The program politely tells you whether it's coming or going.|
|*memwatch*|The program watches the bottom bunch of Amiga bytes for you (where the interrupt vectors live) and lets you know when someone has trashed low memory. This routine will ask whether you want to correct the memory location before proceeding. We've been proceeding without suffering any noticeable ill-effect, but be advised that some naughty program is writing wildly. Funnily, the Amiga printer device steps on low memory. Ha ha. To turn off *memwatch*, enter `memwatch quit`. To turn *memwatch* back on, use `memwatch`.|
|*blitzfonts*|This program gives you faster text systemwide. Never saw it screw anything up! This is a good one. To get rid of *blitzfonts*, use `blitzfonts -r`. Do this to see how slow normal text is without *blitzfonts*, and then use `blitzfonts` to get it back.|
|*sclock*|This routine brings up a little clock sprite on the right of the screen once per minute. Some people like this, others find it obnoxious. What do you think? If you want to get rid of *sclock*, the only way to do so is to comment out the line (put ; at the start of it) that invokes `sclock` in your startup-sequence and then reboot. If you try to get rid of sclock by entering `sclock` or `sclock -q` or `sclock quit` you just get more and more *sclocks* running. Ha ha.

The C: directory contains the system programs distributed by Commodore. Here's a list of some of the most useful ones:

Note that a copy of the `Execute` program is saved under the name X, so you can execute script files using either `execute script` or `x script`. Likewise, `Delete` is copied to `Del` so you can delete a file using either `delete filename` or `del filename`.

The `C2:` directory contains many useful utilities. Here's a list of some of the most useful ones:

|||
|---|---|
|*more*|This tool allows you to look at text files conveniently, a page at a time with pauses between pages. Also, it allows you to back up a page, to go back to the top of the file, and to search for a particular text string. Use the `h` command for help while using the program.|
|*pr*|This tool is for printing text files. Puts file name, page number, date and time on the head of each page.|
|*blitz*|When you need to look at a large text file and you want to scan around through it quickly, use *blitz*. This program screams through text files. Put it in smooth scroll mode and wheeee!|
|*ced* (aka *CygnusEd*)|*Ced* is the best Amiga editor I've ever seen. I whole-heartedly suggest that you get this editor, take the time to learn it, and configure the function keys to your liking. I believe that once you get used to this editor you'll never go back to whatever you used to use (unless you're one of those unix *vi* hack-heads in which case there's no hope for you anyway).|
|*quarterback*|This is a great hard disk backup utility. PLEASE back up regularly. 

In the `S:` directory can be found several other scripts of interest. 

The #$&@% *Preferences* program allows you to set your display and printer preferences, but it gives you no way to save these preferences to your boot disk. In your `S:` directory is a script named `SavePrefs` which does this for you. Use `S:SavePrefs` after running *Preferences* to copy your new system configuration file to your boot disk, presuming that your boot disk is in `df0:` and is not write-protected.

## Handycraft user's guide

This section, what there is of it, presents a user's guide to the *Handycraft* program.

- What It Is and What It Will Be
- Editor
- Data File Creation
- Emulator
- Handycraft Command Summary

### What It Is and What It Will Be

*Handycraft* is a Handy image manipulation program. *Handycraft*'s 3 main areas of capability are:

- Handy image scaling preview and manipulation, especially in being able to create a table that drives Scale-3 scaling
- Handy sprite image data creation, including finding the best (most compressed) way to write out the data
- Handy display emulator

*Handycraft* may also become a mini art editor and animation tool. Also, *Handycraft* will one day allow a file of sprite data to be created using only command-line arguments, without requiring the programmer/artist to create a display, paste the art, and then create the data file.

After a while, each programmer will have real Handy hardware at his or her own desk, and so the display emulation capability of Handy will become unnecessary and will finally be removed from the program.

### Editor
### Data File Creation
### Emulator
### Handycraft Command Summary

Hit the `HELP` key for a display of the commands you can give to *Handycraft*.

If you run out of memory, try exiting all tools and then rebooting your system. Often the Amiga looks like it has enough memory left but various programs don't behave well and leave memory fragmented, which impedes Handycraft as it sometimes needs large blocks of contiguous memory.

## Handebug user's guide

This section presents a user's guide to the Handebug program.

Hey! What are you doing looking here? This page intentionally left mostly blank, like my brain right now.

## Handebug Structure Editor 

Rules of Engagement 
15 November 1988

To add your own structures to Handebug's structure editor, you must edit the `HANDY:handebug.defs` file using the following format:

```
STRUCTURE
	NUMBER n
	NAME string
	FIELD
		POSITION x,y
		SIZE 8/16
		TYPE HEX/DEC/BIN/TEXT/POINTER/STRUCT n
		TITLE
			POSITION x,y
			TEXT string
```

Every structure definition starts out with the word `STRUCTURE`.

Within a structure you declare the structure's type number and an optional name. Following that you declare one or more `FIELD` entries. The fields must be declared in the order that they are present in the actual structure. Any field can be edited by the user.

Each field has an x,y position, which is the position of the first displayed character of the field, whether that character is a digit or part of a text string. A field has a size, which is currently limited to 8-bit or 16-bit. A field also has a type, which can be any of the types listed below. Note that the `POINTER` type is a 16-bit pointer to data, whereas the `STRUCT` type specifies that the field is a pointer to another structure, a structure of number `n`. If you double-click on a field that is of type `STRUCT` and the field is non-null, the structure editor will load and display the structure pointed to by the `STRUCT` field. Finally, the field can have one or more `TITLE` definitions, which allows you to have one or more text strings rendered with the field. The fact that you can have multiple `TITLES` per field allows you to get as descriptive as you want.

Comments are created by having a semi-colon as the first character of the line.

Here's an example. To get this display: 

```
	Field 1: 134E
             ----
	Field 2: 01101010
```

You could make this structure definition: 

```
STRUCTURE 
	NUMBER 16 
	NAME Test Structure 
	FIELD 
		SIZE 16 
		TYPE HEX 
		POSITION 10,0 
		TITLE
			POSITION 0,0 
			TEXT Field 1:
		TITLE
			POSITION 10,1 
			TEXT ----
	FIELD 
		SIZE 8 
		TYPE BIN 
		POSITION 10,2 
		TITLE 
			POSITION 0,2 
			TEXT Field 2:
```

Forthcoming features:

- You'll be able to define an initial position and size for a structure's window
- You'll be able to specify that you want normal 16-bit hexidecimal values displayed in $high/low order without having to use the kludge of `POINTER` declaration (after all, not all 16-bit hex values are pointers)
- Comments can start anywhere on a line, not just in the first position

## HSFX - Handy sound effects system

This chapter describes most everything there is to know about *HSFX*, the Handy Sound Effects system. The topics covered in this chapter include the HSFX concept, tools, tables, and function calls.

But not all HSFX topics are discussed in this chapter; several of the topics are significant enough to warrant separate chapters. The HSFX editor, used to create sound effects, is described in the chapterHSFX Editor User's Guide. Also, the details of the Handy audio hardware are covered in the chapter Overview of the Handy Audio Hardware. Lastly, the *HMUSIC* (Handy Music) driver, which depends heavily on *HSFX*, is covered in the chapter "HMUSIC - The Handy Music System".

This chapter's information is presented in these sections:

- HSFX Concept
- Editor
- Priority/ID Number
- Table Format
- Loop Construct
- An Example HSFX Table
- Accumulation and Interpolation
- The Driver Mechanism
- Summary of HSFX 6502 Functions
- Miscellaneous

### Introduction to HSFX

*HSFX* - the initials stand for Handy Sound Effects - is a software system designed to provide convenient, flexible control of the Handy audio hardware. The *HSFX* system embodies many things, including the audio hardware, algorithms that make the hardware create specific sounds, a system driver that implements the audio algorithms, data tables used by the driver to create the sound effects, and an editor used to create the sound effects data tables.

A Handy sound effect is created by setting and then varying over time the 5 components of a hardware audio channel:

- the frequency (created by the Base Frequency Generator)
- the 12-bit shift value of the Wave Shape Generator
- the feedback-enable lines of the Wave Shape Generator
- the volume value of the Volume Control
- the integration flag of Wave Shape Selector

The audio channel's hardware components are controlled by the *HSFX* driver. The driver is interrupt code that is activated at uniform time intervals due to the interrupts generated by the audio timer, which is a hardware timer dedicated to driving *HSFX*. The *HSFX* driver is initialized and told what to do through the use of *HSFX* commands. While the *HSFX* driver runs as interrupt code, the *HSFX* commands are designed to be called by main-line program code. The *HSFX* command used most often is `PLAYHSFX`, which command gives the driver the address of a sound effect data table and asks the driver to "play" that sound effect. The driver plays a sound effect by executing the commands specified in the sound effect's data table.

Sound effects (and their data tables) are created using the *HSFX* editor, which is a program designed to provide the user with a real-time, easy-to-use sound playground. The *HSFX* editor lets the user manipulate a graphical representation of the audio hardware to specify what values the audio hardware should be have at a given instants of time, known as a keyframes, and how to change those settings from one instant to another. After the user has created the desired sound, the *HSFX* editor writes a file containing the data table required by the *HSFX* driver.

*HMUSIC*, the Handy music system, exists as a shell around the *HSFX* system. Like *HSFX*, *HMUSIC* consists of commands called by the main-line program and a driver that runs during interrupt . The *HMUSIC* driver plays a song by processing the commands in a song data table. The song table commands define music notes. *HMUSIC* plays each music note by converting it into an *HSFX* data table and then submitting the sound effect to the *HSFX* driver (in exactly the same way all other sound effects are submitted).

*HMUSIC* will not be discussed further in this chapter. To learn more about it, refer to the chapter "HMUSIC - The Handy Music System".

Throughout this manual's audio system chapters the phrase audio frame is used. This refers to the system activity that takes place with each tick of the audio timer clock. With every audio frame each active component of the audio system is advanced to its next setting (e.g. you might set up an audio register's value to start at 10 and change to 30 over a span of 10 audio frames, which the driver would do for you by adding 2 to the register every audio frame). The default audio frame duration is 1/240 of a second, but the duration can be modified both at assembly time and at run time. The programmer gets final say on the audio frame duration; of course, the wise programmer always consults with the sound guys before making a decision about the audio frame rate.

Now that you know about the audio frame we can describe the life and times of a sound effect with respect to the audio frames.

The *HSFX* driver records the number of audio frames that a sound effect has been active, initializing the sound's frames-active count to zero when the sound is launched and incrementing the count every audio frame thereafter. This `frames-active` count is used to trigger the fetching of the next bunch of data from the sound effect's table.

As already mentioned, a sound effect is defined by the contents of its data table. In particular, the data table is comprised of a series of keyframe entries, with each keyframe entry being comprised of an audio frame number and audio control data.

The *HSFX* driver uses the keyframe's audio frame number to detect whether it's time to process the keyframe. This is accomplished by comparing the keyframe's frame number with the sound effect's `frames-active` count every audio frame. When the `frames-active` count reaches the keyframe's frame number, it's time to process the keyframe.

Processing a keyframe consists of copying the keyframe's data to the control registers of the audio channel assigned to the sound effect, and then advancing the keyframe pointer to the address of the next keyframe.

The *HSFX* driver uses the audio control data to create the desired sound effect. To describe this briefly: each audio channel has a number of accumulator and interpolator registers, which registers might be initialized in any given audio frame by keyframe data. The registers are manipulated every audio frame by the *HSFX* driver, which converts the accumulators into values that are written to the audio hardware registers, and then adds the interpolators into the accumulators for the next frame. This whole process is described in more detail below in the section "Accumulation and interpolation".

In addition to a frame number and audio register values, keyframes can contain some special items that are used to define a `keyframe` loop: the `top-of-loop` and `bottom-of-loop` indicators and a `loop-count` value. A keyframe loop is series of keyframes that will be executed repeatedly. The keyframe containing the `top-of-loop` indicator defines the first in the series, and the keyframe with the `bottom-of-loop` indicator defines the last. The `loop-count` value defines how many times the loop should be executed. When the `loop-count` is exhausted, the loop is exited and the keyframe following the loop is the next one executed. Also, you can specify a `loop-count` value that will cause the sound effect to begin an `infinite` loop, that is, a loop that will never exit.

Keyframe loops are detailed below in the section "Loop construct".

And then, after all the keyframes are processed, after all the loops are executed and all the data is consumed, the normal sound effect reaches its end and is removed from the system. A special keyframe entry, one that defines no data and no control constructs, is used to designated `end-of-sound-effect`. When the *HSFX* driver encounters an `end-of-sound-effect` keyframe, the driver silences the audio hardware, frees the audio channel, and erases all references to the sound effect's data table.

However, not all sound effects have an end. If you use the keyframe loop construct to define an infinite loop, the sound effect will play on until you explicitly stop it or until some audio event causes the sound to stop.

A final note: all sound effects, including the `music-note` sound effects created by *HMUSIC*, must be assigned a priority. The priority is used to define the importance of a given sound effect with respect to the others. Higher-priority sound effects will always be heard over lower-priority ones, even when it means killing a lesser one to free up a channel for the greater.

Refer to this chapter's "Priority/ID Number" section for details.

### Priority/ID Number

Each sound effect has a priority/ID number, which is used both as a unique identifier for the sound and to specify the importance of the sound with respect to the others. The programmer provides a priority/ID number for a sound when submitting the sound to be played.

Sounds that have a greater priority will always play over sounds with lesser priority. A high-priority sound will bump a lower one off of an audio channel as needed. Low-priority sounds will not disturb high-priority ones.

If there's a free audio channel when a sound effect is submitted, the channel is given to the sound effect and priority isn't considered.

But if all channels are currently in use when a request to play a sound is made, the *HSFX* driver will look through the current sounds to try to find one with a priority that's less than or equal to the the priority of the new sound. If one or more is found, the one with the lowest priority is zapped and the channel is given to the new sound. If no sound with a lower or equal priority is found, the new sound isn't played.

### Data table format

When an audio channel is newly selected to play a sound effect, the channel's *HSFX* accumulators and interpolators are preset to zero. Those registers are then modified according to the specifications in the sound effect's data table. Every audio frame the channel's accumulators are converted into data that's written to the audio hardware, thereby creating the desired sound effect.

The *HSFX* data table is made up of one or more keyframe entries. The keyframe entry There must be at least one keyframe entry in an *HSFX* table.

A keyframe entry is comprised of three parts: a frame number, followed by specification flags, and then a block of data that's variable in size coincident with the order, number and type of specification flags.

#### Frame number

The keyframe entry starts with a 16-bit keyframe number. This number designates the audio frame number during which the specifications of this keyframe entry should take effect. The keyframe number of a sound effect is relative to the frame during which the sound effect started to play. If an entry has a frame number of zero, then the keyframe data will be loaded during the first frame that the sound exists (there almost always is a 
keyframe zero). Note, as described in this chapter's "Loop construct" section, that the frame number is reset every time the driver takes a branch to the top of a loop.

Normally keyframe entries have ascending keyframe numbers. The only time that adjacent keyframes can have the same frame number is when the first keyframe is a loop specifier; this is described below under "HSFX Loop Construct". Keyframes in ascending memory locations should never have descending frame numbers unless you want your sound to play for a really long time.

Note that restricting the number to 16 bits means that the longest a sound can live is 65536 audio frames. This shouldn't be a problem. If the audio frame time interval turns out to be 1/240 of a second, sound effects will be able to live for 273 seconds, which should be plenty.

#### Specification flags

Following a keyframe's frame number comes a byte that contains the keyframe specification flags. Most of these flags, if set, cause the registers of the sound effect's audio channel to be loaded with keyframe data. Other keyframe flags are used to define the beginning and end of a loop.

If any of the specification flags is set, a corresponding value can be found among the data of this entry. The driver look sat the flag bits in order from high to low, and for each flag that's set a value will be fetched from the next sequential location of this keyframe's data block. Thus the amount of data in a keyframe entry is dependent on the number of specification flags that are set.

A keyframe that has no flags set is a special keyframe that denotes end-of-sound-effect. When the *HSFX* driver processes such a keyframe, the audio is silenced and all references to the sound effect are removed.

Here's the names and meanings of keyframe flags, listed in the correct high-to-low order:

|||
|---|---|
|KEY LOOP|If this bit is set and we aren't already looping, then this keyframe entry includes a loop counter. If this bit is set and we are already looping, then this keyframe entry marks the end of the loop and there is no loop counter.|
|FREQ ACCUM|If this bit is set, the entry includes a new frequency accumulator value.|
|SHIFT ACCUM|If this bit is set, the entry includes a new shifter accumulator value.|
|FBACK ACCUM|If this bit is set, the entry includes a new feedback-enable accumulator value.|
|VOL ACCUM|If this bit is set, the entry includes a new volume accumulator value.|
|FREQ INTERP|If this bit is set, the entry includes a new frequency interpolation value.|
|FBACK INTERP|If this bit is set, the entry includes a new feedback-enable interpolation value.|
|VOL INTERP|If this bit is set, the entry includes a new volume interpolation value.|

#### Keyframe data

Following the keyframe specification flags are zero or more data values. The number and byte count of the keyframe data is defined by which specification flags are set and what data is supposed to accompany the flag. For example, if the frequency accumulator flag is set then immediately following the keyframe header will be a 16-bit frequency accumulator value. If the volume interpolation flag is set then the next thing in the table will be a 16-bit volume interpolation value, and so on.

Note that the only keyframe with zero data values is the special `end-of-sound-effect` keyframe, where all the specification flags are zero.

A 6502 note about keyframe data that's almost not worth mentioning: whenever keyframe data fields are declared as 16-bit, the data is always stored in normal 6502 low byte/high byte fashion, of course.

### Loop construct

There are 4 keyframes associated with a loop: the keyframe where the loop is initiated, a keyframe that acts as the `top-of-loop`, a keyframe that defines the `bottom-of-loop`, and the keyframe that's executed when you fall out of the bottom of a loop.


The loop counter is initialized with the HSFX table value in the keyframe where the loop flag is set. The following keyframe is the top-of-loop keyframe. When the end of a loop is reached and the loop condition succeeds, the top-of-frame keyframe is the next keyframe executed. The top-of- frame keyframe is executed immediately, not during the following frame. When the loop condition finally fails the keyframe following the end-of-loop keyframe is executed immediately. It would be valid for this keyframe to be another top-of-loop specifier.

If the loop counter that comes from the *HSFX* table should be zero this is a request for an infinite loop, which *HSFX* merrily provides for you.

When you take a loop branch, the frame number of the top-of-loop keyframe becomes the current frame number for this sound effect, and then the frame number will ascend from there as usual.

When the loop is taken, the frame number of the sound effect is set to the frame number of the top-of-loop keyframe.

### An example sound effect data table

```
SampleSFX

	;---- Next keyframe for this sound effect 
	.WORD $0000 ; At frame $0000
	;---- new freq/shift/fback accum and vol interp
	.BYTE FREQ_ACCUM+SHIFT_ACCUM+FBACK_ACCUM+VOL_INTERP
	.WORD $2222 ; Initial frequency
	.WORD $FFFF ; Initial shifter
	.WORD $0001 ; Initial feedback-enable
	.WORD $00CC ; Initial volume interpolation

	;---- Next keyframe for this sound effect
	.WORD $0064 ; At frame $0064
	;---- new freq and vol interp
	.BYTE FREQ_INTERP+VOL_INTERP
	.WORD $0010 ; New frequency interpolation
	.WORD $0000 ; New volume interpolation

	;---- Next keyframe for this sound effect
	.WORD $00ff ; At frame $00ff
	;---- Stop the sound effect
	.BYTE $00
```

## Accumulation and interpolation

Each sound effects component (except the shifter) has two 16-bit registers, an accumulator and an interpolator, which contain 16-bit signed fixed-point quantities. By fixed point we mean that each register has an integer and a fraction part, with a fixed, unchanging number of bits in each part; note that each component has its own number of bits per part. All registers are signed, allowing interpolations to be either positive or negative.

With each audio frame, the accumulators are translated into values that are stored in the audio hardware registers. Then, if we're at a keyframe, all new accumulator and interpolator values are fetched from the table and stored in the appropriate registers. Finally the interpolation values are added into the accumulators.

The shifter component is a special case. There is no shifter interpolation register, nor is there a shifter accumulator. Whenever a shifter value is encountered in an HSFX table the value is written directly to the hardware.

The translation of the accumulators into hardware values is different for each component. Here's the rules for the various translations:

`FREQUENCY`

This register has 14 bits of significant information, where the 8 most significant bits of the 14 are translated into a base frequency selection and the integer multiplier of the selected base frequency. The frequency accumulator's other 2 bits are used for sign and 1 bit of fraction. Before translations are performed, the value should be shifted right one place to truncate the fraction. After that, the translation of the top 6 bits can work like this:

- If bit 5 ($20) is set select the 64 usec frequency
- Else if bit 4 ($10) is set select the 32 usec frequency
- Else if bit 3 ($08) is set select the 16 usec frequency
- Else if bit 2 ($04) is set select the 8 usec frequency
- Else if bit 1 ($02) is set select the 4 usec frequency
- Else if bit 0 ($01) is set select the 2 usec frequency
- Else if all of the top 6 bits are zero, select the 1 usec frequency

The 8 most significant bits are used as the multiplier. Note that in the case where none of the high 6 bits is set, the 1 usec clock is selected and the lower byte, whatever its value, is used as the multiplier.

`FEEDBACK-ENABLE`

This accumulator has 1 bit of sign, 8 bits of significant information and 6 bits of fraction. Some fancy dancing is done to get the correct bits of this accumulator out to the hardware.

`VOLUME`

This accumulator has 8 bits of significant information (including a sign bit) and 8 bits of fraction. Simply write the high byte to the hardware register and ignore the low byte.

`SHIFTER`

The 12-bit shifter value is written directly to the Handy audio hardware. There is no accumulator or interpolation register.

Summary of the HSFX commands

This section is seriously preliminary. We need to decide what we really want the arguments to and results from these functions to be.

`StartHSFX`

You call this routine with the low-byte of an *HSFX* table in the accumulator, the high-byte of the table in the X register, and the sound's priority in the Y register.

Presumably this routine will typically be called by a routine named something like `PlaySoundEffect` which will want the index number of the sound effect in the accumulator and which loads up the registers accordingly before calling `StartHSFX`.

If the sound is started, the channel number is returned in the accumulator in case you have some need for it. If the sound wasn't started, a `-1` is returned.

`StopHSFX`

This routine allows the programmer to stop a sound effect if it's still running. The logic of this routine is unfortunately a bit slippery. The argument that you supply to this routine is a priority number for the sound effect you want to stop (thereby forcing the priority number to act as an ID number). The *HSFX* driver will look for a sound effect that has that priority and if one is found then it is stopped.

If the programmer has created more than one sound effect with the same priority/ID number then the found one found will be the one stopped.

`AllocAudio`

This routine tries to allocate an audio channel for the caller's exclusive use. You provide a priority number and if any channel is available at that priority then you'll get it (see the section **Priority/ID Number** above for more details). When calling this routine set the accumulator to the priority of your desire for a channel. If you successfully get a channel, the system will not disturb it until you call `FreeAudio`.

There's a definition, `HFSX_BIGPRIORITY`, which will get you a channel if there's one to get. However, note that even if you use a priority of `HFSX_BIGPRIORITY` this routine can fail nevertheless if all of the channels have already been allocated away from *HSFX*. If you don't want to mess around, you just want the gol-darned channel, then use the `GrabAudio` routine.

If a channel is allocated, the channel number is returned in the accumulator. If no channels are currently available, a `-1` is returned.

`GrabAudio`

This routine forces the *HSFX* driver to stop using the audio channel specified in the accumulator.

Once you have grabbed a channel, the system will not disturb it until you call `FreeAudio`.

`FreeAudio`

If you have allocated or grabbed an audio channel away from *HSFX* and you want to allow *HSFX* to use the channel again, call `FreeAudio` with the channel number in the accumulator.

If you call this routine with an invalid channel number (like `-1` for instance) nothing bad happens, promise.

## Miscellaneous

You can turn off all sound effects like this:

```
		PHP ; Don't have to do this if you're in interrupt code
		SEI : Don't have to do this if you're in interrupt code
		LDA 	#3
		STA 	temp
.10 	JSR
		GrabAudio
		JSR FreeAudio
		DEC temp
		LDA temp
		BPL .10
		PLP ; Don't have to do this if you're in interrupt code
```

## HSFX Editor Notes

Copyright (C) 1988, Epyx, Inc. 
CONFIDENTIAL and PROPRIETARY

This document describes the *HSFX Editor*.

If you haven't noticed yet, you will soon notice that this document is in-the-works.

This is not intended as a stand-alone document, but rather is destined to become a chapter of the Handy Software Programmer's Guide and Notes. It describes only the components and usage of the HSFX Editor. It doesn't attempt to instruct on Handy audio control or sound creation techniques; before reading this you should read the HSFX and the Handy Audio System Description chapters of the Programmer's Guide.

Also, not everything described in this document is implemented yet.

### Introduction and Definitions

The program has two main displays, the Main Menu and the Key Frame Editor.

The `Main Menu` allows you to load and save sound effects and to make global changes to the current sound effect.

The `Key Frame Editor` allows you to create, modify and delete the key frames of the sound effect.

A sound effect is comprised of a series of key frames, which key frames contain values that the audio hardware should have at given instants of time and instructions on moving from one key frame's value to another's.

The *HSFX Editor and Driver* support a technique called interpolation, which is the opposite of step. Interpolation refers to the numeric progression from one value to another. If you have specified that you want one of the audio components, volume for example, to be at one value at a given instant and a different value later, you are allowed specify that you want the value to either change abruptly to the new setting or to ramp incrementally once per audio frame from the initial to final setting.

Currently, the HSFX Editor and Driver support only linear interpolation, which is a simple algebraic slope fromone value to another. We are considering adding musical interpolation, which would interpolate from one value to another using the curve that describes moving from one musical note to the next.

### Using the FllelO Requester.

Rounding errors. Today's technique for avoiding them. Tomorrow's technique for avoiding them.

#### Main Menu

New
Copy 
Set Audio Frame Rate 
Quit

Load an HSFX Editor File 
Save Sound Effect as an HSFX Editor File 
Write Sound Effect as a 6502 Text File 
Write Sound Effect as an Assembler .bin File

Set this Sound Effect's Equate Name 
Set this Sound Effect's Assembler Label 
Set this Sound Effect's Priority

Edit

Eval
Play

Cut
Copy
Paste

Selecting a keyframe and drag-selecting multiple adjacent keyframes

### Key Frame Editor

The Key Frame Editor display has three edit areas:

- There's a L-shaped area that starts top left and flows down then over to bottom-right. This `L` presents a programmable analogue of the Handy sound hardware. The programmer can set values in this edit area that will be converted by the *HSFX* editor into a table of coded instructions, which the *HSFX* driver on Handy can understand and execute.

- In the top right is the `Key Frame Entry` edit area. Here's where the user cruises though the current list of keyframe entries and creates and deletes entries, and where the user sets the sound driver parameters of this entry.

- Halfway down the right-hand side is the control section. Here the user can decide to play the current sound effect, return to the main menu, or undo the last operation.

#### Hardware analogue

The hardware analogue has three areas where the user can control the settings of the audio hardware. These three areas correspond to the three components of Handy audio. The first area allows the user to specify the base frequency for the sound. The second area provides control gadgets for the audio polynomial sequencer. The third area allows control over the sound effect's volume and integration.

The first box (top-left) of the hardware analogue has gadgets which allow the user to select the base frequency/period for this frame of the sound effect. The base frequency is the number of times per second that this sound effect's clock will tick. The base period is the amount of time that elapses between two ticks of this sound effect's clock.

The base frequency/period can be specified three different ways: the user selects one of the clocks and enters a multipler for that clock; the user specifies the desired frequency; the user specifies the desired period. When the user selects a clock and enters a multiplier, the value in the frequency number gadget should be updated simply by multiplying the clock by the multiplier. On the other hand when the user enters a frequency value the specified value should be converted into clock and multiplier selections, using the smallest clock possible. Note that the specified frequency may not evenly divide into a clock and multiplier. If this is the case, get as close as you can. Save the clock, the multiplier and the frequency in the `KeyFrame` data structure. The 7 clock selection gadgets are mutually-excluding, of course, and have special imagery that create a beautiful complement mode effect. You'll need to create alternate imagery somehow. Using `PowerWindows 2` may do the trick for you. Finally, there's two gadgets, `Interpolate to Here` and `Step Now`, which are mutually-excluding gadgets. If `Interpolate To Here` is selected then set the KeyFrame's `INTERPOLATE_FREQUENCY` flag. If `Step Now` is selected then clear the KeyFrame's `INTERPOLATE_FREQUENCY` flag. You'll need two pieces of imagery for these gadgets, one with the circle filled in and one with the circle blank. These two images can be used in several places on the `Editor` display.

The next box of the hardware analogue (bottom-right) allows the user to set up the polynomial sequencer. There's two pieces to the polynomial sequencer, the shifter value and the freedback-enable lines. The user may or may not want to set the shifter to a specific value during any given keyframe. The user makes this choice by

First time a Key Frame Entry is edited 
Rules for Copying and Deleting Key Entries

### When User Selects PLAY

### Converting the Key Frame List into an HSFX Table

## 6502 Cross assembler notes

This section discusses the particulars of writing Handy 6502 assembly code.

### Things to Include

### Things You Must Define

If you're going to use Handy sprites or the Handycraft sprite emulator, you must define the type of IO you have. If you define `HANDYIO` then the files are configured for you to talk through Handy hardware. If you define `AIO2` then the files reflect communications through an Apple AIO2 parallel card. Likewise, an IO32 definitions will set up the files to talk through an Apple IO32 parallel card.

## HandyASM B0.04 Implementation Notes 

18 March 1989  
Confidential and Proprietary

Here's some notes about the particulars and peculiarities of *HandyASM*. Hopefully, these notes will help you identify and avoid the most common problems as you come up to speed on our new assembler.

You are strongly encouraged to help expedite the *HandyASM* debugging by diligently reporting any problems you have. Every time you find new errors or get junk results, open (or reopen) an Amiga text file and make an entry describing the problem. Then give a copy of the text file to me. If appropriate, give me code that can reproduce the problem (doesn't matter how much code it takes, even if you have to give me your whole program). After I receive and log the bug reports I'll fire them off to Carl immediately, taking advantage of the fact that Carl is going to try to provide us with a bug-fix turn-around time measured in hours! By the way, don't be concerned about releasing confidential source code to Carl, he's eminently trustworthy and he's under a strong non-disclosure agreement. Besides, he was the tenth person or so in the entire universe to learn about Handy, so there's not much he doesn't already know.

Please report all problems you find, even if you think someone else already reported the problem.

### HandyASM Implementation Notes

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
````

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

### Known bugs:

- `#ELSE` doesn't seem to be working yet (not that anyone will notice)
- `.CSET` isn't working correctly, which will cause `HPRINT` users to get junk text
- the `??` macro operator is flaky when being used to create a label

### Not implemented yet:

- the `.TF` (for `Target File`) directive
- using macro arguments by value rather than by text during macro expansion
- command-line arguments `+D`, `+I`, and `+X`
- some other stuff that I can't remember just now

## The two scaling algorithms

This section describes the two Handy scaling algorithms.

With both algorithms, the horizontal and vertical scale values are independent.

### Algorithm 3 - Shifter

This algorithm loads the 16-bit `HSize` and `VSize` values into a shift register and then tests the bits of the shift register sequentially to decide whether or not to use a particular pixel of source data when creating the sprite imagery. Wherever a bit is set, a corresponding pixel will be selected.

For example, if the SCB `HSize` field of a sprite is initialized to `%1010101010101010` then the first pixel would be drawn, the second pixel skipped, third drawn, fourth skipped, et cetera. As only half the pixels would be drawn, the image would be half-sized. Note that if your source imagery is greater than 16 pixel wide, the shift register is reloaded for the 17th pixel (likewise the 33rd and so on).

Likewise, whether or not a line of source data will be processed or skipped entirely is decided using the sprite's `VSize` value.

Note that this algorithm allows imagery to scale between 0 and full-sized only, as contrasted with Algorithm 4 which allows imagery to scale from 0 to 256 times full-size.

*Handycraft* allows you to edit a Scale 3 table and see the scaling imagery that results from your table. When you're satisfied with the results, you can write out the table and assemble it into your 6502 program, the entries of which your program must feed into your sprite's `HSize` and `VSize` fields.

### Algorithm 4 - Adder

When you select Algorithm 4, your imagery is scaled according to the `HSize` and `VSize` fields of the sprite control block where the fields are 16 bit fixed-point values where the lower 8 bits are fraction and the upper 8 are the integer part.

For example, an `HSize` value of `$0100` means that you want your sprite scaled full-size. A value of `$0080` means you want you sprite half-sized, and `$0200` means double-sized.

## Handy audio hardware overview

INCLUDE EXAMPLES someday real soon now.

This chapter describes (to a certain extent) the features and functionality of the Handy audio hardware. It presents an overview of the steps required to create Handy audio, starting from the generation of the audio's base frequency clock and following the flow of information all the way out to the Handy speaker.

This information of this chapter is geared toward the sound creator and the audio system programmer; emphasis is placed on the areas that are affected by design and programming choices, while features that are strictly hardware and are not modifiable, such as the digital to analog conversion circuitry, are described only briefly.

Some of the descriptions in this chapter are intended for novice sound programmers; the patience of advanced programmers is requested during these passages.

This document is comprised of the following sections:

- HSFX Concept
- Editor
- Priority/ID Number
- Table Format
- Loop Construct
- An Example HSFX Table
- Accumulation and Interpolation
- The Driver Mechanism
- Summary of HSFX 6502 Functions
- Miscellaneous

## Introduction to audio and the Handy audio system

Sound, very briefly, is a compression wave that travels through air. To create this compression wave in Handy, a speaker cone is pushed and pulled from a zero position. The movement of the cone is driven by an electrical signal that varies from a negative to a positive value. The strength of the electrical signal (the amount the cone is displaced from the zero position) is called the amplitude. The period of time that it takes for the signal to go from zero to full positive to full negative and back to zero again is called the frequency.

The Handy programmer can adjust both the amplitude and frequency of the sounds being created. Also, the programmer can cause the wave shape to be relatively steady or wildly erratic. Steady amplitude and frequency creates a steady tone, like a musical note. Crazy amplitude and frequency changes create crazy sounds like explosions.

The following paragraphs give an overview of the Handy audio components and the flow of information through them. More detailed descriptions of the operation of these components can be found in the sections below.

Base Frequency Generator
Wave Shape Generator
Volume Control
Mixer
Analog Stuff
External Volume Control
Speaker
Wave Shape Selector

The Handy audio programmer has 4 audio channels at his or her disposal. Throughout most of the description below we'll be looking at sound creation using just one of those channels, but bear in mind that these procedures can be performed on any of the audio channels.

Now, to create a sound.

The first step is to generate the base frequency of the sound by programming the Base Frequency Generator. The base frequency can range from 60 to 1 million pulses per second. The Base Frequency Generator sends clock pulses at the selected rate to the Wave Shape Generator. The Wave Shape Generator uses a strange algorithm under program control to decide whether to send out a zero or a one. This strange and mystical algorithm, when properly tickled, produces sounds that vary from musical notes to "white noise" sounds like explosions or engine roars. The zeros and ones come out of the Wave Shape Generator at the base frequency rate and go to the Volume Control module, which converts them into signed 8-bit values taking integration into account, and sends the values to the Mixer. The Mixer takes this information, blends in the information from the other audio channels, and creates a single digital audio signal. The rest is all hardware. The digital signal goes to the Analog Stuff component, where the digital signal is converted into an analog signal. The analog signal then passes through the External Volume Control for human adjustment, and from there finally makes it to the speaker.

And that's it. Pretty simple, except for the weird Wave Shape Generator algorithm which is described below (well, at least an attempt is made to do so).

### Base Frequency Generator

The audio starts with the generation of the sound's base frequency. The base frequency is used to create the actual frequency of the sound (high frequency makes high-pitched sounds, low frequency makes deep sounds). Handy audio's base frequency can be used to generate tones with frequencies as high as 1/2 million beats per second (above hearing) and as low as 30 beats per second (almost below hearing). The Handy audio base frequency is created by the Base Frequency Generator, which consists of two parts: the clock selector and the down counter.

With the clock selector the programmer is able to specify the frequency of clock ticks that will be sent to the down counter. The clock selector is made up of two parts: a hardware timer and the bits used to select the timer's frequency. The normal frequencies are: 1 µsec, 2 µsec, 4 µsec, 8 µsec, 16 µsec, 32 µsec, and 64 µsec (where "µsec" stands for microsecond). There is an 8th frequency selection: previous. When previous is selected, the output from another timer (this timer's "previous" timer) is used as the input to this timer. This process is described in more detail a few paragraphs below.

The clock selection causes the timer to periodically wake up and send a pulse to the down counter. This means that if the clock selector is set to, say, 2 microseconds, then every 2 microseconds a pulse will be sent to the down counter (500,000 pulses per second). If the clock selector is set to 1 microsecond, then every microsecond a pulse will be sent to the down counter (1,000,000 pulses per second). This clock with its selectable frequency is known in the paragraphs below as the source clock.

The down counter, which gets its ticks from the source clock, is programmable too. You set the down counter with a value that can range from `0` to `255`. The down counter acts as an integer divider of the clock frequency generated by the source clock, because the down counter must count down to zero before the pulse generated by the source clock will be passed along. Every time the source clock "clocks" (sends a pulse to) the down counter, the down counter's value is tested for zero and when zero the down counter finally causes the Base Frequency Generator to send a clock tick to the next component of the audio system, the Wave Shape Generator. If not zero, the counter is decremented and tested again next time.

If the down counter is set to a value of 0 then every tick from the source clock is passed directly out of the Base Frequency Generator. However, if the down counter is set to `1` then every second tick from the source clock gets sent to the Wave Shape Generator. If the counter is `255`, every 256th tick gets sent. When the down counter is combined with a source clock at a frequency of 1 µsec, any Base Frequency from 1 to 256 µsecs can be created. The 2-µsec clock can be used to create frequencies from 2 to 512 µsecs with an increment of 2 µsecs, et cetera, down to the 64-microsecond source clock being used to create frequencies as long as 16,384 µsecs (16.384 milliseconds), which would result in a very deep sound indeed.

Now that you know how the source clocks work, you can understand selecting "previous" as one of the clock frequency selections. First, if you don't already know, every one of the Handy timers has a so-called previous timer, which is the timer that's processed by the hardware immediately prior to the processing of the current timer. When a timer counts the down counter down to zero and it sends out a pulse, this pulse can be used as the input tick to the timer that's processed next (literally) by the hardware. The "next" timer's frequency is set by the output rate of the "previous" timer. Here's a table of the audio timers and their "previous" timers:

|Audio Timer|Previous Timer|
|---|---|
|AUDO|TIMER7|
|AUD1|AUDO|
|AUD2|AUD1|
|AUD3|AUD2|

Unlike the normal system timers, it's not possible to get interrupted when an audio timer times out. However, at least a mechanism is provided which allows you to detect that the timer reached the end and restarted. You detect this by checking the `TIMER DONE` flag. `TIMER DONE` gets set when the down counter counts down to zero and gets reloaded. This bit gets cleared when we write the `RESET TIMER DONE` bit. We should never change the `TIMER DONE` bit directly because the bit is in a register that's shared with hardware, so setting it directly could screw up the hardware's state a bit if the timer is running. On the other hand, if the timer is stopped then it's safer (and faster) to clear the `TIMER DONE` bit directly.

So anyway, if we've got everything right the Base Frequency Generator starts sending periodic pulses down the tube to the rest of this channel's audio hardware. We follow the pulses to their first destination, the mysterious Wave Shape Generator.

### Wave Shape Generator

The Wave Shape Generator is a weird little beastie. It runs at the frequency that's created by the Base Frequency Generator. Every time the Wave Shape Generator receives a pulse, it runs through another iteration of its strange logic to decide whether or not to send a `0` or a `1` to the next component of the audio chain, the Volume Control.

The Wave Shape Generator is comprised of a shift register, a large exclusive-or gate and an inverter. The shift register has 12 bits, each of which can be initialized by the programmer. 9 of the 12 bits (specifically bits `0` to `5`, `7`, `10` and `11`) are used as inputs to the `XOR` gate. Each of these 9 outputs can be individually enabled or disabled. The enabled outputs are exclusive-or'ed together, the result of the `XOR` is inverted, and the resultant value is both the output of this component and the input to the shift register (thereby creating a "feedback" loop).

With each pulse that arrives the Wave Shape Generator creates an output value. Then the contents of the shift register are shifted left, with bit 11 discarded and bit 0 receiving a copy of the output value.

`TIMER7` refers to the normal system timer channel 7. By the way, the audio timers and the system timers behave identically. System timers have clock selectors and down counters too.


The output that comes from various combinations of inputs and line-enables is strange, and is predictable only by people with brains that are a certain shade of grey. The following hints are provided for the less than fortunate:

- If you enable only bit `0`, then no matter how you've initialized the shift register the output will be a stream of `0 1 0 1 0 1 0 1`. This is the simplest of all waves.
- To expand on the above, if you enable any one of the bits and disable all of the other, you will generate a flip-flopping stream of bits that are correspondingly long. For example, setting only bit `1` will generate `0 0 1 1 0 0 1 1`. Setting only bit `7` will generate `0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1`. These periodic patterns can be used to make nice symmetric square and triangle waves (as described below).
- more notes ...

### Volume Control and Wave Shape Selector

With each base frequency tick the Wave Shape Generator sends a `0` or a `1` to the Volume Control. The Volume Control converts that binary value into a digital value that can range from `+127` to `-128`, depending both on the state of the Wave Shape Selector's integration bit and on the Volume Control's 8-bit signed volume value.

When in the non-integration mode, the Volume Control output is a simple square wave. The output is the volume value made positive or negative depending on whether the input is `1` or `0` respectively.

On the other hand, when the Wave Shape Selector's integration bit is set the Volume Control's output register is used as an accumulator to create the output value, with the volume value added to or subtracted from the accumulator depending on whether the input to the Volume Control is `1` or `0` respectively. The value is added or subtracted at the clock rate created by the Base Frequency Generator. Because the value is iteratively added and subtracted, the output value can be made to ramp from minimum to maximum and back again. In this way a triangular waveform can be created.

The integration accumulator, you might notice, can float all over the place, and often you won't know where it is at a given instant. The hardware guys assure us that this is OK, that the current value of the accumulator can be ignored because the significance of the Volume Control output is not in the level of the output but in the change of the level. When the output is constant, regardless of the level, no sound will be made. Also, the accumulator clips when it "hits up against the stops" and therefore is constantly justifying itself. As shown in the above illustration, adding a value to an accumulator that's already at the maximum positive value leaves it at the max positive value, rather than causing it to wrap around to a negative value. Subtracting from that accumulator then decreases the output value appropriately. This means that the output wave tends to have the correct shape, although not necessarily centered around zero. See the illustration above for a picture of this logic in action. Note that after clipping, the desired waveform is created indeed although the waveform is offset from the zero position.

### Mixer, Analog stuff, External Volume Control and Speaker

The programmer's ability to affect the audio system ends at the mouth of the mixer. The Mixer accepts the output from each of the four channel's Volume Control components, and, as you might guess, mixes the signals together into a single digital signal. This signal is passed along to the Analog Stuff, which converts the digital signal into analog and then shoots that along to the External Volume Control. There, the user is given final say in the volume level. Finally, the signal makes it to the speaker where the aforementioned compression wave is created, which ripples across the fabric of time-space to beat against the eardrums of all good children. The end.

### Turning a channel on and off

There are two ways to stop an audio timer from running. The first stops the timer immediately, the second lets the timer count down and generate a tick one last time before stopping.

You can stop an audio timer immediately by clearing the timer's `ENABLE COUNT` flag. When the `ENABLE COUNT` bit is set, the down counter counts down as usual. But when the bit is clear, the down counter freezes and no pulses are sent out of the Base Frequency Generator, even if the down counter freezes when its value is zero.

The more elegant way to shut off the audio timer is to have it count to the end of its current down counter setting, send one last pulse, then stop and shut itself off. To shut off a timer this way, clear its `ENABLE RELOAD` flag. After the down counter counts down and the timer shuts down, it's as off as if you'd cleared the `ENABLE COUNT` flag.

Here's a slightly weird truth about the audio hardware: if the timer has timed out and the `ENABLE RELOAD` flag is cleared, then the timer will stop as you would expect, but if you then clear the `TIMER DONE` flag a single timer pulse will be sent after which `TIMER DONE` will be set again. You can use this to drive the source clock directly from software, which can be used to create weird LX audio effects.

If the channel is already on, you may want to turn it off before setting it up to make a new sound. To turn a channel on, you need to initialize:

- The Base Frequency Generator's frequency selection and down counter
- The Wave Shape Generator shift register's 12 bits and the 9 feedback-enable flags
- The Volume Control's volume value and accumulator (also known in hardware land as the "audio output value")
- The Wave Shape Selector's integration bit
- You may want to set and then clear the `RESET TIMER DONE` flag
- You should set both the `ENABLE COUNT` and `ENABLE RELOAD` flags

To turn off a channel:

- You should clear either the `ENABLE COUNT` or `ENABLE RELOAD` flag, depending on whether you want the sound to stop immediately (clear `ENABLE COUNT`) or as soon as it's finished with the current tick (clear `ENABLE RELOAD`)
- Set the Volume Control's volume value and accumulator to zero. Actually, no sound comes out if these aren't set to zero, but setting them to zero helps save battery life a teeny little bit.
