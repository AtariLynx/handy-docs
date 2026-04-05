# CART - Game cartridge I/O system

This chapter describes the tools and techniques known collectively as *CART* that are used to interact with the Handy game cartridge. The chapter is comprised of the following sections:

- Introduction and Overview
- *CART* File System
- If You Don't Want to Use the *CART* File System
- File 0 Boot Code
- *CART* Commands
- *CART* Memory Variables
- Example Usage of *CART* Commands
- *HandyROM* Overview
- Summary of *HandyROM* Constants and Directives
- Debugging Cartridge-Based Programs
- Roadmap to a Complete Example
- Physical Properties of the Game Cartridge

A special note to today's Handy programmers: your humble author invented whole bunches of this stuff without input from you and whilst under considerable pressure. Because of this, it's highly probable that the *CART* logic and algorithms will evolve a good deal during the next several weeks, both as we discover better ways to accomplish the tasks tackled by this system and when the Howard board comes together. So please don't hate me if I require you to endure several revisions of these ideas. And remember, this stuff will get better only if you make your opinions known. So read it, use it, and then shout about anything you don't like and anything that could be better!

## Introduction and overview

In order to provide Handy programmers with the capability to access game cartridge data quickly and easily, *CART* - the Handy game cartridge system software - was created. The goal of *CART* is to allow convenient access to the Handy cartridge through the use of powerful support tools and a complete body of cartridge I/O commands. The *CART* system gives programmers both high-level "file system" access to the data and low-level direct access to the cartridge hardware.

The Handy old-timers may have noticed that the description of the Handy storage medium, formerly referred to as the "Handy ROM," is now being called the "Handy game cartridge." This change reflects a pleasant reality: the storage devices that plug into Handy's cartridge slot can contain RAM as well as ROM, can be written to as well as read from. But even though Handy cartridges can contain RAM, in the near future most game cartridges won't have RAM because it would make for an extravagantly expensive cartridge. Nevertheless, in order to correctly support all of the Handy capabilities, the *CART* commands make no distinction between addressing ROM and RAM. Both are supported equally; *CART* provides commands both for reading from and writing to the cartridge. In fact, throughout this chapter you will find generic references to cartridge programming when features that pertain to both ROM and RAM are being described; specific mention of ROM or RAM will be made only when necessary to refer to that particular type of storage. The commands that read cartridge files and data pertain to both ROM and RAM. The commands that write to the cartridge pertain only to cartridge RAM, of course, though there's nothing to stop you from trying to write to ROM.

The *CART* software system provides programmers with all the tools and system commands they need to create and debug cartridge-based versions of their games. By using *CART* and the support tools, programmers are able to build pseudo-game cartridges and then access them using precisely the same code in their development environment that will later be released to the real world. Because of this, *CART* acts as a highly dependable stepping-stone from software under development to release software. Care has been taken to make sure that the game cartridge will behave during debugging exactly as it will in the real world (as soon as we get up on Howard boards, that is).

The most valuable function performed by *CART* is its management of the interface between the program and the game cartridge hardware (which is among the most troublesome pieces of hardware in Handy). Game cartridge memory is segmented into pages, with each page consisting of a fixed number of bytes. There are 256 pages in a normal cartridge (it would be very abnormal for a cartridge to not have 256 pages), and there are as many bytes per page as is called for by the total storage capacity. For instance, a 64K cartridge has 256 pages and each page has 256 bytes. A 128K cartridge has 256 pages and each page has 512 bytes.

Cartridge data lives at a cartridge address, which is a combination of a page and an offset within that page. You can think of the hardware address of a cartridge byte as the concatenation of the page with the offset.

The page segmentation of cartridge memory forces the use of less than ideal techniques when accessing data. The hardware design allows access to only one cartridge page at a time; changing to a different page involves the execution of a complex, long-winded piece of code. Also, reaching the end of a page resets the offset to `0` but doesn't automatically bump the cartridge address to the start of the next page; rather, the software must detect that the end of the page has been reached and manually advance the cartridge page when so. This makes for hairy cartridge I/O algorithms, you bet. Fortunately, the *CART* software commands hide the complexity of all this from the programmer.

When your program is ready to go and you want to get it into the game cartridge, first you use the *HandyROM* tool to create a ROM image of your program. *HandyROM*, the Handy ROM file builder, is a powerful tool that supports many of the same constructs and features supported by *HandyAsm*. *HandyROM* constructs a raw binary file that's ready to be loaded into a game cartridge, and the tool goes on to generate useful diagnostics about the ROM image you've built. *HandyROM* not only creates the ROM image according to your specifications, but it also can construct an entire file system for you. The *CART* commands use this file system to give you the power to get at your data quickly and effortlessly.

After you've created your ROM image, you use *Handebug* to download it to your development system's pseudo-game cartridge. You can download either an entire cartridge or just parts of it. *Handebug* also provides the means by which you do a cold-start boot of the game in the cartridge, thereby completing the development cycle.

Taking a brief look at the hardware, we find that the Handy game cartridge is quite a knobby beast, requiring strange handling techniques which have evolved over time to keep up with the many versions of the Handy storage medium. Fortunately, the cumbersome details of getting the hardware to obey are all hidden from the programmer beneath the tools and the *CART* system software. If you want to know more about how the software interacts with the hardware, refer to the [Physical properties of the game cartridge](#physical-properties-of-the-game-cartridge) section of this chapter. For complete details about game cartridge I/O, refer to the manuals """Handy Specification""" and """Handy Appendix 2: Hardware Addresses."""

## CART File system

*CART* provides you with several levels of control when reading and writing your cartridge data, starting with low-level I/O control at the bottom and working up through increasing levels of sophistication to the top, the simple yet powerful *CART* file system.

The *CART* file system consists of game cartridge files and commands that access those files. A *CART* file contains your program data. The commands access your data via file directory entries. A directory entry describes the number of bytes in the file, where the file data can be found in the cartridge, and more.

A file directory entry is comprised of a minimum set of required components and some or none or all of the optional components. All directory entries have an identical set of fields. *CART* provides a set of directory options - a sort of directory construction kit - which allows the programmer to create a customized directory and make file interaction even more convenient. The added convenience has a cost, however, for each directory entry must be larger and extra *CART* code must be included to support the fancier features.

Every directory entry is required to have these fields:

`ROMDIR_SIZE` - 16 bits  
This field has the file's data byte count.
`ROMDIR_PAGE` - 8 bits  
This field contains the cartridge page number of the file's first byte of data.
`ROMDIR_OFFSET` - 8 or 16 bits  
This field contains the offset into the cartridge page of the file's first byte of data. This field can be either 8 or 16 bits wide, depending on the number of bits required to describe the offset to the data, as described in the following paragraphs.

The simple case: if the page size is 256 bytes (a page can't have fewer than 256 bytes) then a maximum of 8 bits are needed.

But if the page size is greater than 256 bytes then another factor comes into play, the `ROMFILE_ALIGN` factor. `ROMFILE_ALIGN` is a constant that you can define to be used both by *HandyAsm* when assembling your code and by *HandyROM* when compiling your ROM image. `ROMFILE_ALIGN` allows you to specify that you want every file's first byte of data to be aligned on a given boundary. The major benefit of using `ROMFILE_ALIGN` is that it reduces the number of bits required to describe the offset everywhere throughout the file system. For instance, when `ROMFILE_ALIGN` is defined to be `1` then the entire offset is required in order to be able to address any byte in the page. But if `ROMFILE_ALIGN` is defined to be `2` then all files will start on even boundaries, which means that the least significant bit of every file offset will always be zero. When this is the case, the system can drop the least significant bit and shift all offsets right by one bit. This in fact is what the system does. Likewise, if `ROMFILE_ALIGN` is `4` the two least significant bits will be zero so both are dropped. And then the payoff: after dropping the excess bits, if the required number of bits per offset is less than or equal to 8 then the `ROMDIR_OFFSET` directory entry will be only 1 byte wide, not 2.

The benefit of this is small but reasonable. In a 128K cart, you normally need 9 bits to describe the 512 possible offsets. But by specifying a `ROMFILE_ALIGN` of `2`, all of your directory entries will be one byte smaller while on average only half of your files will have to waste the odd byte in order to align on an even boundary. This results in a savings of one byte for every two files (it also improves directory access performance a little tiny bit).

The optional directory entry fields include:

`ROMDIR_DEST` - 16 bits  
The `ROMDIR_DEST` field is used to define a default RAM destination for the file when it is loaded using one of the *CART* commands. The default destination is either the `.ORG` you defined when assembling the file's code, or a `RAMDEST` that you declared when building the ROM image with *HandyROM*.

Note that any of the commands that might use this default RAM destination field also provide you with the ability to override the default by specifying an alternate destination.

If you don't specify to *HandyROM* that you want your directory entries to contain a `ROMDIR_DEST` field then you are required to supply a RAM destination to all of the *CART* commands that copy data into RAM.

`ROMDIR_FLAGS` - 8 bits  
This field is used to record flags of your own definition. This provides you with a mechanism by which you can flag files that have features of note. It's a general-purpose feature that you can use any way you want.

When we get around to implementing packing and unpacking of files, and if the programmer has decided to have some files packed and some unpacked, then whether or not a given file is packed will be designated in the `ROMDIR_FLAGS` field using a flag named `ROMFILE_PACKED`.

You declare which of the above fields your directory will have and simultaneously declare the offset of each field in the directory entry by defining the above constants in a file that contains declarations used both during the *CART* code assembly and during the ROM image building. This file is typically named `cartdefs.i`; the file `6502:examples/cartdefs.i` is a template of the file you should create. Here's an example of the directory entry field declaration that your file might define:

```
ROMDIR_SIZE 		.EQU 0 ; Two bytes for SIZE 
ROMDIR_PAGE 		.EQU 2 ; One byte for PAGE 
ROMDIR_OFFSET 		.EQU 3 ; One byte for OFFSET
ROMDIR_FLAGS 		.EQU 4 ; One byte for FLAGS 
ROMDIR_DEST			.EQU 5 ; Two bytes for DEST
ROMDIR_ENTRY_SIZE 	.EQU 7 ; Entry byte count
```

Another file system constant that must be defined is the `ROMFILE_ALIGN` constant. Every file's first byte of data will be forced to start on an even multiple of the `ROMFILE_ALIGN` value. The major benefit of using `ROMFILE_ALIGN` is that it reduces the number of bits required to describe the offset to a file, as described under `ROMDIR_OFFSET` above. If you don't care how your files are aligned, define `ROMFILE_ALIGN` to be `1`. This constant too is defined in the template `6502:examples/cartdefs.i` file.

The very first file of your ROM, file `0`, must refer to special boot code. Refer to the section ```FILE 0 BOOT CODE``` for details.

Finally, a note on using the file system with game cartridge RAM. As noted above, the storage medium of the Handy cartridge can be either ROM or part ROM part RAM. Typically a cartridge will contain only ROM, but it doesn't matter one way or the other. The *CART* file system is designed to handle both.

## If you don't want to use the *CART* file system

Some of you will prefer to work with the game cartridge in your own way rather than using the *CART* file system. You are free to use the game cartridge any way you like, of course, but there is one limitation: your ROM image must start with data that looks like a file entry that points to the cold-start boot code described in the [File `0` boot code](#file-0-boot-code) section below. In the absence of this file entry, your program will not boot.

When compiling your ROM image, the way to specify that you don't want a directory and that you won't be using the file system is by defining the constant `ROM_NODIR`. An example of this declaration can be found in the sample template file `6502:examples/cartdefs.i`.

Note that even if you declare `ROM_NODIR` *HandyROM* "enforces" the requirement of having at least one file entry in your ROM file by automatically creating the entry and using as boot code the first file included by your HandyROM Specifications File (described below in the ```HANDYROM OVERVIEW``` section). Also, *Handebug* depends on the presence of that first file entry when doing a cold-start boot. But other than that, *HandyROM* and *Handebug* and the lower-level *CART* routines work just fine in the absence of the *CART* file system.

## File `0` boot code

Here's a very important fact about the Handy cold-start procedure and its use of the *CART* file system: for your cartridge to be loadable by the Handy boot software you must provide a special boot file containing some sort of power-up (cold-start) code. At time of cold-start, the Handy system will load and execute this special boot code file. The boot code must meet certain requirements:

- The file `0` directory entry (the very first directory entry) must refer to this boot code
- The very first byte of the boot code must be an executable instruction
- If your directory entry includes the `ROMDIR_DEST` field then this field must contain the destination address of the file's first byte; if your directory entry doesn't include `ROMDIR_DEST` then your code will be downloaded to RAM address `$0200` and then a `JMP $0200` will be performed
- A temporary requirement, which will exist only until we get up on the Howard board, is that your boot code must end below `$F700`

Notice that though the phrase "boot code" is being used, the code doesn't actually have to be boot code but rather can be any code at all, including your main program code if you like. [Well, you can't exactly do just anything you like. Remember that the Handy Software Quality Assurance Board imposes certain restrictions on and reserves the right of final say over what your code can do at time of cold-start].

## *CART* commands

The *CART* system software provides commands (mostly in the form of macros) that can be used to perform I/O with the Handy game cartridge.

The *CART* system supplies many features, some of which may not be useful to you. You get to include only the features that you want. It's configurable, allowing you at assembly time to define which of the features and commands you'll be using.

Here's the high-level file system commands:

`GETDIR`  
The `GETDIR` command sets up the `A`, `X` and `Y` registers with the cartridge page and offset of the directory entry for the file specified in `A`. The register values set by this call are the same as the ones required by the `OPENFILE` command.

`GETDIR` is a command that isn't necessary if you have the page and offset for your files available to you in some way, perhaps defined as equates for instance. `GETDIR` is provided as a convenience command for those who have the 75 bytes to spare and who desire the simplification that this command offers. But because `GETDIR` isn't required, the macros and support code aren't included in your assembly unless you specifically ask for them. To use `GETDIR`, you must define the constant `GETDIR_USER`. See the [Example usage of *CART* commands](#example-usage-of-cart-commands) section and the file `6502:examples/testcart.src` for an example of this.

Note that `GETDIR` and `GETDIRFAST` perform the same function. `GETDIRFAST` is larger and faster than `GETDIR`.

`GETDIRFAST`  
The `GETDIRFAST` command sets up the `A`, `X` and `Y` registers with the page and offset cartridge address of the directory entry for the file specified in `A`. The register values set by this call are the same as the ones required by the `OPENFILE` command.

`GETDIRFAST` is a command that isn't necessary if you have the page and offset for your files available to you in some way, perhaps defined as equates for instance. `GETDIRFAST` is provided as a convenience command for those who have the 125 bytes to spare and who desire the simplification that this command offers. But because `GETDIRFAST` isn't required, the macros and support code aren't included in your assembly unless you specifically ask for them. To use `GETDIRFAST`, you must define the constant `GETDIRFAST_USER`. See the [Example usage of *CART* commands](#example-usage-of-cart-commands) section and the file `6502:examples/testcart.src` for an example of this.

Note that `GETDIR` and `GETDIRFAST` perform the same function. `GETDIRFAST` is larger and faster than `GETDIR`.

`OPENFILE`  
The `OPENFILE` command "opens" a file by:

- Loading its directory entry into the *CART* system's `CartDirectory` buffer
- Setting the `CartPage` and `CartOffsetLow`,`High` variables to address the file's first data byte
- Setting the cartridge hardware ready to read the file's first data byte

After this call, a call to `READFILE` will load the file's data into RAM.

When called, `X` should have the page number and `A`,`Y` (low,high) the offset of the file's directory entry.

Note that if you've defined `ROM_NODIR` then the `OPENFILE` command and support code won't be included in your assembly.

`READFILE`  
The READFILE command reads the currently-opened file's data into RAM.

If you have declared `ROMDIR_DEST` to specify that your file directory should have a RAM destination field, then you have the option when calling `READFILE` to have the data written to the destination specified in the directory entry or to an alternate destination of your choosing. You specify which you want with the value you pass in `A`. If `A` is zero, the `ROMDIR_DEST` value will be used as the destination. If `A` is non-zero, then `X`,`Y` (low,high) should specify the desired destination.

If `ROMDIR_DEST` is not part of your directory entry, then you must specify in `X`, `Y` (low,high) the RAM destination for the file's data.<br>After this call, the `CartPage` and `CartOffsetLow`,`High` variables are advanced to the byte after the last file byte read. Also, `X`,`Y` (low,high) has the address of the RAM byte just after the last byte written by `READCART`.

Note that if you've defined `ROM_NODIR` then the `OPENFILE` command and support code won't be included in your assembly.

Here's the lower-level cartridge I/O commands, which you might not need to use if you're relying on the CART file system to do the work for you:

`SETCARTADDR`  
This command is used to set the cartridge I/O hardware to the specified cartridge address. The page of the address should be in `X`, and `A`,`Y` (low,high) should contain the page offset of the address. After this command is executed, the hardware is set ready to read the specified byte.

After this call, the `CartPage` and `CartOffsetLow`,`High` variables are set to reflect the specified cartridge address.

`SETCARTPAGE`  
`SETCARTPAGE` is used to set the cartridge I/O hardware to the specified cartridge page. The page number should be in `X`. After this command is executed, the hardware is set to read the first byte (offset of zero) of the specified page.

If it's your desire to set both the page and offset of the cartridge, it's more efficient to use `SETCARTADDR` rather than `SETCARTPAGE` followed by `SETCARTOFFSET`.

`SETCARTOFFSET`  
`SETCARTOFFSET` is used to set the cartridge I/O hardware to the specified cartridge page offset. The offset should be contained in the `CartOffsetLow`,`High` variables. After this command is executed, the hardware is advanced to read the the specified byte.

This command presumes that on entry the cartridge hardware page offset is zero.

If it's your desire to set both the page and offset of the cartridge, it's more efficient to use `SETCARTADDR` rather than `SETCARTPAGE` followed by `SETCARTOFFSET`.

`READCART`  
`READCART` lets you read data from the cartridge address. The arguments of this function will be defined real soon now, honest.

After this call, the `CartPage` and `CartOffsetLow`,`High` variables are advanced to the byte after the last file byte read. Also, `X`,`Y` (low,high) has the address of the RAM byte just after the last byte written by `READCART`.

Here's the very-low-level cartridge access commands, which you shouldn't need to use unless you're controlling the cartridge hardware directly:

`SET_TO_READ_CART`  
This command turns the cartridge power on and sets the cartridge I/O direction to read.

`SET_TO_WRITE_CART`  
This command turns the cartridge power on and sets the cartridge I/O direction to write.

`RESTORE_CART`  
This command restores the cartridge status to its state before either of the above commands was invoked.

There's no reason to not be both a `GETDIR_USER` and an `GETDIRFAST_USER`. It depends on your application. In RAM-tight configurations of your code you might want the slower, smaller `GETDIR` code in memory, but if in other levels of the same program you have RAM to spare you can choose to have the faster, fatter `GETDIRFAST` code around.

## Cart memory variables

This is the list of memory variables set and maintained by *CART*. You may read the values of these variables, but it's not safe to write to them:

|||
|---|---|
|`CartPage`|Contains the 8-bit page number of the current cartridge address.|
|`CartOffsetLow`,`High`|Contains the 16-bit offset into the page of the current cartridge address.|
|`CartDirectory`|Contains a copy of the currently opened file (files are opened using the `OPENFILE` command).<br>Note that `CartDirectory` is defined only if `ROM_NODIR` isn't defined.|

## Example usage of *CART* commands

Here's a demonstration of the code required to load a file's data into RAM using the easiest of all techniques:

```
; Tell the CART system that we'll be using GETDIR 
GETDIR USER . EQU 1
	LDA FileToBeOpened	; Number of file to be opened
	GETDIR				; Get file directory address
	OPENFILE			; Open file, readying for read 
	LDA #0				; Use default RAM destination 
	READFILE			; Load the file's data into RAM
```

If you know the cart address of your file directories, you can omit the `GETDIR` functionality and code:

```
	LDX #MYFILE PAGE	; Load registers with cart 
	LDA #<MYFILE OFFSET	; address of the file's
	LDY #>MYFILE OFFSET ; directory entry
	OPENFILE			; Open file, readying for read
	LDA #0				; Use default RAM destination
	READFILE			; Load the file's data into RAM
```

## HandyROM overview

The *HandyROM* program merges your assembly output files into one large binary file, a Handy game cartridge ROM image. *HandyROM* also can create for you a *CART* file system directory structure at the beginning of the ROM.

*HandyROM* accepts as input arguments found in a *HandyROM* specifications file. This file contains directives to *HandyROM* regarding the type of ROM that should be built and the files that should be included in the ROM. The specifications file typically ends with the file extension `.hsf`.

*HandyROM* supports the following command-line arguments:

|||
|---|---|
|`-v` or `+V`|Verbose diagnostics will be printed to the CLI while your ROM image is being compiled|
|`-l` or `+l`|A listing file (with extension `.lst`) will be created|
|`-r` or `+r`|An error file (with extension .err) will be created|
|`-d` or `+d`|This argument, identical to the assembler argument, allows you to define a constant|
|`-o` or `+o`|You can use this argument to specify an output file name different from the default|

Any token in the command-line that's not one of the above arguments must be the name of *HandyROM* specifications file. If more than one specifications file appears in the command line, the files will be processed sequentially in the order that they appear.

The output from the *HandyROM* program, unless redirected using the `-o` switch or the `OUT` directive, will go by default to a file with a `.rom` name extension using the name of the first specifications file in the command line according to the following rules:

- If the name of the first specifications file has a dot extension (if it ends with, say, `.src`, `.txt`, `.anything`) then the dot extension will be stripped off
- The extension `.rom` then will be appended to the name, and this will be used as the output file name

Here's the basic format of the ROM image file created by *HandyROM*:

- At the beginning of the ROM are a number of bytes set to zero, which ROM locations are used later by the system for the Handy Authorization Verification data. The exact number of bytes in this block is defined by the constant `ROM_HEADER_SIZE`, found in the `6502:include/cart.i` file.
- Next comes the *CART* file system directory, which is `ROMDIR_ENTRY_COUNT*ROMDIR_ENTRY_SIZE` bytes long. The `ROMDIR_ENTRY_COUNT` will be at least `1`, even if you don't want directories (refer to the sections [Cart file system](#cart-file-system) and [If you don't want to use the cart file system](#if-you-dont-want-to-use-the-cart-file-system) and [File 0 boot code](#file-0-boot-code) in this chapter for details).
- Finally, your program data can occupy the ROM from the end of the directory to the end of cartridge memory

There can be up to 256 files in a ROM, and therefore up to 256 file entries. *HandyROM* creates only as many directory entries as required.

*HandyROM* also generates a text facsimile of the directory structure, writing it to a file that ends with the extension `.dir`. This file is in *HandyAsm* format, for you to include in an assembly if you so desire. For each directory entry, *HandyROM* generates a comment describing the file number and then the directory entry data declared as `.BYTE`'s and `.WORD`'s as appropriate.

With each assembler output (`.bin`) file *HandyROM* processes, these steps are taken:

- A 64K buffer is cleared
- Each data packet is copied into the buffer. During this process, *HandyROM* keeps track of the lowest and highest buffer locations to which data is written
- All data between the lowest and highest locations inclusive is written into the ROM image

The *HandyROM* specifications files look much like the files accepted by *HandyAsm*. Any numeric value processed by *HandyROM* can be defined in symbolic format or in any of the numeral formats (decimal, hexidecimal, binary) accepted by the assembler. A line that starts with `;` (semi-colon) or `*` (asterisk) is a comment. Many of the assembler constructs such as macros and conditional assembly are supported by *HandyROM* too.

You are strongly encouraged to create a common file, typically named `cartdefs.i`, which contains cartridge and file system definitions needed both during the assembly of your code and during the compilation of your ROM image. `HandyROM` was fashioned to accept the same definitions file accepted by the assembler in order to simplify the process of (and minimize the errors with) defining and referring to your cartridge data.

Following the definitions of the directory structure and all of the required constants come the *HandyROM* directives. The *HandyROM* directives control the inclusion of data into the ROM and the construction of the file system directory. Data is included using either the `BIN` or `RAW` directive. *CART* files are declared using the `FILE` directive. Other *HandyROM* directives allow you to control the layout of your data and the values of your file's directory entry. All of these are specified in the [Summary of HandyROM constants and directives](#summary-of-handyrom-constants-and-directives) section of this chapter.

## Summary of HandyROM constants and directives

These constants must be defined before the first `FILE` directive is encountered:

```
ROMDIR_PAGE .EQU n
ROMDIR_OFFSET .EQU n 
ROMDIR_SIZE .EQU n 
ROMDIR_ENTRY_SIZE .EQU n
```
These constants must be defined, whether or not you are going to use the *CART* file system:

```
ROMDIR_DEST .EQU n 
ROMDIR_FLAGS .EQU n
```
These constants should be defined if you want your directory entries to have these optional fields:

```
ROMFILE_ALIGN .EQU n
```
This constant defines the byte boundary to which every file's first data byte will be aligned:

```
ROMSIZE .EQU n
```
You use this to define the total size of the ROM.

```
ROMPAGECOUNT .EQU n
```
This almost always will be set to 256.

```
ROMPAGESIZE .EQU n
ROM_FILEO_PAGE .EQU n
ROM_FILEO_OFFSET .EQU n
```
These are defined by formulae in the `cartdefs.i` file, based on the values of other constants.

|||
|---|---|
|`OUT pathname`|If this directive is given, the output will be sent to the named file rather than to the default file. If you're going to use this directive it should be used before most other directives, should probably be one of the first directives in the specifications file.|

After all of the above, you can start using the following *HandyROM* directives. If you are using the *CART* file system, then the `FILE` directive must come first followed by a combination of other directives.

|||
|---|---|
|`ALIGN n`|This directive causes the ROM address to be aligned to the next multiple of the specified argument. This works identically to the assembler's `.ALIGN` directive, and can be used to temporarily override the `ROMFILE_ALIGN` constant if it defines an align value greater than the `ROMFILE_ALIGN` value; it's an error for ALIGN to define a value less than the `ROMFILE_ALIGN` value.<br><br>If you're using the file system, note that only one `ALIGN` directive can be used per `FILE`. The normal `ROMFILE_ALIGN` is reestablished at the start of the next `FILE`.|
|`BIN pathname`|This directive causes a normal assembler output file (`.bin`) to be included in the ROM. This data will the next sequential data of the current ROM image.|
|`FILE file number`|This *CART* file system directive initiates the building of a new file directory entry. If a numeric argument is provided, then the number defines a file number. If no argument is given, the file gets the next sequential file number (starting from zero).|
|`FLAG bit mask`|This *CART* file system directive follows a `FILE` directive, refers to the current file. The bit or bits specified by the FLAG argument will be set in the `ROMDIR_FLAG` field of this file's entry.|
|`ORG page,offset`|This directive causes the next data to go to the specified ROM page and offset.|
|`RAMDEST n`|This *CART* file system directive follows a `FILE` directive, refers to the current file. You can specify where a particular file should be loaded in RAM. You can use `RAMDEST` only if you've defined `ROMDIR_DEST` to specify that your directory will contain RAM destination information. You must use `RAMDEST` to declare the RAM destination for a raw binary file. You may use `RAMDEST` to change the RAM destination of an assembler output file.|
|`RAW pathname`|This directive causes a raw binary file to be included in the ROM. This data will the next sequential data of the current ROM image|

## Debugging cartridge-based programs

In the beginning of your game development, you will probably start by creating simple versions of your program, downloading these into RAM and executing them immediately. You probably won't write to the cartridge or otherwise using the file system until near the end of the development cycle, and even then you will probably go through a lengthy transition period during which you'll use a software switch to select either a RAM or a cartridge destination for your binary.

When you get to the point where you're interested in trying to create and execute a real ROM image, you'll find that *Handebug* has some nice features to assist you. All normal features work as always. Download the ROM image into the game cartridge using *Handebug*. Two ways to do this. Also, you can stop your code from executing, download new data to the game cartridge, and continue.

*Handebug* controls for cart writing: `LOAD CART` to load the cartridge with your data, and `BOOT CART` to perform a cold-start boot of file `0` in the cartridge.

Downloading to the game cartridge is completely non-destructive to Handy RAM, which means that you're able to stop your code from executing, download new data to the game cartridge, and then continue. Also, with a single keystroke you can have `Handebug` load and execute a special "boot code" file. The entire procedure for loading your current ROM image into the cartridge and then initiating a cold-start boot requires only 3 keystrokes.


## Roadmap to a complete example

This section provides a roadmap to the complete example of using the *CART* system that can be found in the `6502:examples` directory. The files of interest in that directory are:

- `testcart.src`
- `testcart.hsf`
- `cartdefs.i`

When you assemble the `testcart.src` file using the command `asm testcart` several binary files are created: the initial file plus an additional file for each `.TF` directive contained in `testcart.src`. These binary files comprise the data of the CART file system files that are created when you build the ROM image file `testcart.rom` using the command `handyrom testcart.hsf`. The ROM image file `testcart.rom` is downloaded to your development system's pseudo-game cartridge using the *Handebug* `LOAD CART` command. After your program is loaded into the cartridge, you can cold-start boot the program using the Handebug `BOOT CART` command.

## Physical properties of the game cartridge

The game cartridge has 2 ports, with independent storage attached to the ports. The storage on port 0 must be ROM, but the storage on port 1 can be either ROM or battery-powered RAM. Each port's maximum storage capacity is 1 megabyte, which means that 2-megabyte cartridges are possible if each port has a 1M part. The minimum capacity of a port is 256 bytes. Because the ports are independent, different sizes of storage medium can be attached to the two ports, allowing odd-sized cartridges (96K, 160K, 192K for instance) to be created.

To access a byte of cartridge data, first you must set the hardware to access the byte's page using a rather tortured technique that involves strobing in the page number one bit at a time. Setting the page number also sets the hardware offset to address the page's first byte. Each read from or write to the cartridge data register causes the address offset to be advanced by one. The offset wraps back to zero after the maximum offset is reached.
