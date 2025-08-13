This file describes the contents of the `sys:c2` directory.

|||
|---|---|
|`amigamonitor`|Amiga monitor program|
|`arc`|Standard archiver/dearchiver|
|`areacode`|Accepts a 3-digit area code, prints out areas covered|
|`avail`|Reports the available memory|
|`blitz`|Very fast text typer|
|`blitzfonts`|Very fast text replacer|
|`calendar`|Calender displayer, args are: [month,] 4-digit year|
|* `ced`|CygnusEd Professional, editor from ASDG<br/>Requires `req.library` in `libs:`|
|`clickupfront`|Allows a double-click to bring a window to the front.<br/>Needs `click-handler` in `sys:l`|
|`clipPrint`|Prints the contents of the ClipBoard|
|`cls`|Clears the CLI window|
|`conman`|A utility that allows command-line editing an more.<br/>Needs `ConHandler` in `sys:l` and `ConHandler.library` in `sys:libs`|
|`csh`|Matt Dillon's CLI shell|
|`diff`|From Manx, a utility to report on the difference between two text files|
|`diskx`|Disk editor|
|`display`|Carolyn S.'s new display program (use `?` for args)|
|* `dp3`|EA's Deluxe Paint 3, great painting program|
|`dropcloth`|DropCloth, adds a texture to the background of the *Workbench*|
|`e`|Andy Finkel's emacs|
|`ed2`|Will launch `ced` if none running, or send message telling `ced` to open a new file(s)|
|`expose`|Resizes and moves (only as needed) all windows on the *Workbench* so that the top pixel of the *Workbench* screen is revealed|
|`filezap`|Binary file editor|
|`filldisk`|This routine munches the unused bits of a disk, thereby concealing any old data that may still be lingering out there|
|`funkeys`|Charlie Heath's function key editor|
|`goodshow`|RJ Mical's slide show program, where any argument is the name of an `IFF` file that you want displayed, unless the arg is `-f` in which case what follows is a file that contains file names, one per line with three zeroes following each name (e.g. `PictureB 0 0 0`)|
|`grep`|Manx's grep tries to match patterns in text files|
|`heliosmouse`|This program causes the window under the pointer to become the active one.<br/>Needs helios-handler in `sys:l`|
|`iconassembler`|A crude but useful tool, allows a certain amount of icon editing, better than *zapicon* in some ways|
|`im`|InfoMinder which works only on 1.2 autodocs|
|`ld`|List directory, with args: `-a` means all subdirectories, `-d` means list directories only, `-f` means list files only, `-u` means unsorted|
|`lpem`|Long-Persistence EMulator (fun display hack)|
|`ls`|Nice directory program|
|`macview`|Utility to view Mac `PICT` format files and write out as `ILBM` format|
|`make`|The ever-wonderful *make* utility for programmers, this one from Manx|
|`memwatch`|Software Distillery's low-memory watch program|
|`metascope`|Program debugging tool|
|`more`|Ah yes, a unix-like more utility shows files a page at a time. Not as good as *blitz* in terms of speed, but has search!|
|`mousezoom`|Meissen's mouse-movement accelerator|
|`newzap`|John Hodgson's file editor.  Very good interface|
|`noz`|My own utility, strips `Ctrl-Z` out of the specified file(s).|
|`pchard`|Bill Koester's Bridge-card hard reset utility|
|`play`|Plays `IFF` sound effects files|
|`popcli`|A great utility, sits in the background waiting for you to type `LeftAmiga`-`Escape`, at which time it creates a new CLI. Also does screenblanking by accepting a second argument as the number of seconds to delay before blanking the screen. This is Version IV, from the Software Distillery|
|`pr`|print utility|
|`prg`|Programmer Calculator from Gizmoz 2.0|
|`print`|Prints a file to the printer. There must be a better way...|
|* `quarterback`|Hard disk backup utility by Central Coast Software|
|`readme`|This text file|
|`runback`|Runs programs in the background, not tied to the CLI. Most useful for running programs from a CLI that you intend on closing (like from the startup-sequence)|
|`sclock`|Clock program that brings up little rectangle of clock on right side of screen, which clock lingers for a few seconds and then vanishes|
|`screenblanker`|Charlie Heath's screen blanker|
|`setfont`|SetFont sets any font as the system one|
|`spacetotab`|Converts spaces into tabs|
|`tabtospace`|Converts tabs into spaces|
|`touch`|Touch changes the date of a file to the current date and time, useful when used in conjunction with the make utility|
|`tsize`|Tells you the bytes contained in the specified directory|
|`vc`|a virus checking program|
|`vcheck1.2`|Virus Checker 1.2|
|`wack`|Carl's wack|
|`which`|Searches command path for the given file name, returns path to it|
|`xicon`|Goodeve's xicon, is used to execute files associated with the icon from the *Workbench*|
|`zapicon`|A program for creating icons out of dpaint brush files|

> #### Note: 
> `*`'ed files are not publicly distributable, and are not included
	with releases
