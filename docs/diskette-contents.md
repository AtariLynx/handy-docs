
Atari Corporation 
1196 Borregas Avenue 
Sunnyvale, California 
94088-3427 (408) 745-2000

# Diskette contents - Lynx development system

## Handy A2000 Boot Disk: 

1 Normal AmigaDos disk. A copy of the standard development system boot disk used at Epyx. This disk is only necessary if you have a system that doesn't boot off of the hard disk.

## DH0:

1 *Quarterback* V4.3 disk. A copy of the "boot partition" of the standard hard disk setup used in the development system. Does not contain the file devs/MountList, allowing the contents to be restored directly to a hard disk that has already been partitioned correctly ..

## "Standard" system:

6 *Quarterback* V4.3.disks. A complete copy of the "standard" development system at Epyx, with the most recently used copies of the development tools,

These disks assume your Amiga is an Amiga 2000 with 3MB of RAM and a 40 MEG hard drive with *Workbench* V1.3. They also assume you have a copy of *Quarterback* V4.3 to unarchive your files. Your hard disk drive should be partitioned as a standard `DH0` boot partition and a `DH0B` fast file partition. `DH0` should be named `BootPart` and `DH0B` should be named `AmigaHD`.

By the way, some of the test examples in `6502:examples` may not work. They need to have includes for `.var` files put into them. For example: `hmusic.var` and `hsfx.var` are not included in `testhmusic.src`. The `.var` files can be found in `6502:src`.

Have Fun !!

Revised 9/91
