# Handy software development overview

This section presents an overview of the Handy software development environment. The subsections found below are:

- [Handy is Lynx, Lynx is Handy](#handy-is-lynx-lynx-is-handy)
- [System Philosophy - Our Gift to You](#system-philosophy---our-gift-to-you)

## Handy is Lynx, Lynx is Handy

We begin this Handy Developer's Guide and Notes with a clarification and a mildly interesting observation of human nature.

Listen: Epyx has sold its new game system to Atari. Atari has chosen to name the new game system "Lynx." This is good for Atari and probably for the buying public too.

However, no matter what it's called in the real world, the system's creators and its first batch of game developers will always know it as "Handy." From the very first day the project's code-name was "Handy." Most of the software tools were named along the Handy theme: *Handycraft*, *Handebug*, *HandyAsm*. Several of the hardware pieces were named likewise: Howdy, Pinky, Mandy. The task of changing everything over to the new name is as daunting as it is needless, and it would probably be a bit demoralizing too. So it seems that "Handy" is here to stay, at least within our development community.

The Epyx folks tried to switch over to calling it "Lynx," really we did. But we failed. At the same time we tried to help the Atari marketing folks recognize the wisdom (ahem) of leaving it named "Handy," but we failed that too.

So instead, the universe and all in it are required to endure yet another new rule: those who work with both Epyx and Atari will have to deal with the fact that the machine is both "the Handy" and "the Lynx." It's all in the Zen of the thing.

Long live Lynx. Long live Handy.

## System philosophy - our gift to you

In the beginning, from the beginning ...

At all times during the design of the system's hardware we kept the programmer in mind: many features were added to help shorten program development time, others were added to help the programmer attain that ever-sought-after Holy Grail: improved execution rate.

For example, the hardware provides certain graphics functionality (e.g. the multi-faceted sprite engine) that all programs need. If the hardware didn't provide this functionality the programmer would have to spend great quantities of precious time attempting to implement it in software. Furthermore, the hardware performs complex tasks (e.g. hardware math) that would consume vast amounts of CPU time if performed using software.

And, as with the hardware, we kept the programmer in mind at all times during the design of the system software. Our body of software support code provides a complete set of macros that provide a unified quality to all components of the program. Furthermore, use of the system software eliminates unnecessary programming jobs two ways: by providing low-level routines to take care of grunt-work details that are unimportant to the programmer, and by providing a complete set of the high-level system drivers that all programs need.

By using the low-level system macros the programmer is freed from having to tend to the more bothersome, more exacting system details (e.g. being required to write to the hardware display registers at just the right time). But the time-savings gained by using the low-level system code is small compared with the time saved by using the high-level system drivers (e.g. sound effects driver and the ROM access routines) rather than creating these drivers from scratch. All of the essential system drivers, already designed and debugged, are available to the Handy developer. The developer trades the small amount of effort required to learn and incorporate the system code with the enormous effort of reinventing the wheels. This is a very good deal for the programmer, because several of the drivers are massively complex and required many man-months to develop. Instead of requiring the programmer to create and debug these drivers, the programmer's job is made easy for each of the system's drivers can be plugged into a program with very little effort.

All of the software support and powerful features of the hardware are presented to the programmer with our compliments. Enjoy.

By taking full advantage of all that our system provides, the programmer should find that most of the hardest programming tasks are rendered trivial. This will leave the programmer with much spare time, which time can be devoted to improving game quality. This is our grandest intent. This is our gift to you. We sure hope you use it.

## Development environment overview

This section presents a broad-brush overview of the development environment by describing the individual components of the environment. These components are presented below, in these subsections:

- [Hardware](#hardware)
- [System software](#system-software)
- [Software tools](#software-tools)
- [Documentation](#documentation)

### Hardware

The hardware environment consists of Amiga computers, Howard boards with their accompanying Howdys, and Pinky boards with their accompanying Mandys.

The Amiga computer can be any of the Amiga line, from the A2500 to the A500. We suggest that each primary developer use an A2000 or better, configured with at least 3M of RAM and 40M of mass storage.

Note that a reasonably well-equipped A1000 can be used in place of the A2000.

Test departments and work stations that will be used only occasionally can get away with the low-cost A500 (and external floppy) with little inconvenience. Downloading through the A500 takes a bit longer because the file is read from a floppy disk, but the extra delay is hardly noticeable. However, full-scale software development on an A500 is unreasonably clumsy and slow and will remain so until A500 RAM and hard disks come available.

The "Howard board" (with its Howdy) is the primary component of the system's debugging hardware. The Howard supports all normal debugging operations such as viewing a hex dump or a symbolic disassembly, and monitoring program flow using controls such as breakpoints. But the Howard also provides features normally found only in sophisticated, expensive hardware state analyzers: the Howard monitors all activity on the system bus, recording as many as 128,000 of these events, and allowing a break to the debugger to occur when a series of events from the simple to the fantastically complex has been witnessed.

"Howdy" is the name of the Howard board's Handy. The Howdy is a completely self-contained Handy. When Howdy is executing a program it does so with no interference from the Howard board (though Howard continually monitors Howdy's bus and NMI line).

The Howard is a sort of shell around the Howdy. From the Howard you can peer into the Howdy, while from the Howdy you are completely unaware of and completely uninfluenced by Howard (that is, until the programmer takes such explicit action that causes Howard to modify Howdy).

The Howard is controlled by an Amiga program called *Handebug*, which is described below in this section's [Software Tools](#software-tools) sub-section and also in this chapter's [`HANDY:`](#handy) section.

For developers who need several development Handys - for instance, when developing multi-unit games or when play-testing - there is the Pinky/Mandy pair. Pinky/Mandy is a low-cost version of Howard/Howdy.

"Pinky" is a stripped-down version of the Howard board. Pinky supports only 2 development functions: 

- downloading programs to and/or executing them in Handy RAM; 
- downloading programs to and/or executing them in a simulated Handy ROM cartridge. 

Other than these functions, normal debugging operations cannot be performed through a Pinky.

"Mandy" is a Modified-Handy, modified just enough to be able to communicate with a Pinky.

The minimum hardware development configuration is a Howard/Howdy connected to an A2000, an A2500, or an A1000. Additional Howard/Howdy and Pinky/Mandy systems can be added to the environment either by linking them by switchbox to the one Amiga or by acquiring additional Amigas.

### System software

To support and simplify the development effort we provide you with files that define a vast assortment of system macros, definitions, drivers and utility routines. Included in this collection are items that range from the obvious such as the hardware register definitions to code as complicated as the music driver and the text-rendering routines.

These system files are kept in the sub-directories of the Amiga's `6502:` directory. The `6502:` directory is described below in this chapter's [`6502:`](#6502) section.

Developers are strongly encouraged to use the system's definitions, macros and drivers whenever possible. There are many advantages for doing so, and several disadvantages for not doing so.

First of all, using our stuff will speed your software development dramatically. When you use our macros and utility routines many of the system grunt-work details are tended to automatically. Furthermore, very complex system routines (such as the multi-unit communications driver) are already designed, debugged, and ready for you to use. And yet, whether a given sub-system is simple or complex, the programmer interface is always uniform, simple, and well-documented.

Another reason for using the system-supplied code is to help assure that your game will be upwardly-compatible with future revs of the hardware. We will design new hardware with a mind to any requirements or limitations imposed by the system software. If you write your own code rather than using ours, you have no such assurance.

Also, if you use our stuff then it will be much, much easier for us to support your ongoing development, especially when such support involves bug-hunting or the search for ways to improve your game performance. The more our system software is used, the more quality and quantity of support we can provide, and the more our system software improves. It's obvious that we will do better by providing more support to those who choose to live within our system.

Finally, perhaps most importantly, with many programmers learning and exploring the system code we are sure to discover better ways to accomplish the system goals. In fact, this has already been the case: programmer feedback often has lead to extensive increases in code quality and decreases in code size. And these improvements, when reported to the Handy development support group, ultimately are made available to all developers in the Handy community. Meanwhile, with each enhancement the support code becomes increasingly compact and efficient, evolving continually into ever more powerful, highly-tuned system software.

### Software tools

The development environment includes a healthy number of software tools. A few of the tools are generic - "text editor" and "paint program," for instance - but the majority of the software tools were created specifically to provide support during the many aspects of Handy software development.

The Handy-specific-tools can be found in a directory named `HANDY:` on the Amiga. This directory is described below in this chapter's [`HANDY:`](#handy) section. The generic development tools may be found in any number of places on the Amiga. For more information, refer to this chapter's [Where Things Are on the Amiga](#where-things-are-on-the-amiga) section.

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

- """Handy Developer's Guide and Notes
- """Handy Hardware Specification
- """Handy Appendix 2: Hardware Addresses

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

The primary `HANDY:` development tools are detailed in the chapters """HandyAsm User's Guide""", [Handycraft User's Guide](./handycraft-users-guide.md), [Handebug User's Guide](./handebug-users-guide.md), and [HSFX User's Guide](./hsfx-users-guide.md). The software tools kept in `HANDY:` include:

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
