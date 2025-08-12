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

Taking a brief look at the hardware, we find that the Handy game cartridge is quite a knobby beast, requiring strange handling techniques which have evolved over time to keep up with the many versions of the Handy storage medium. Fortunately, the cumbersome details of getting the hardware to obey are all hidden from the programmer beneath the tools and the *CART* system software. If you want to know more about how the software interacts with the hardware, refer to the """[Physical properties of the game cartridge] section of this chapter. For complete details about game cartridge VO, refer to the manuals """Handy Specification""" and """Handy Appendix 2: Hardware Addresses."""

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

Another file system constant that must be defined is the `ROMFILE_ALIGN` constant. Every file's first byte of data will be forced to start on an even multiple of the `ROMFILE_ALIGN` value. The 