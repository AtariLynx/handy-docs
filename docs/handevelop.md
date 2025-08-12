# Handevelop 

Hardware & programming description 

Designed by Howard Delman

||||
|---|---|---|
|FEB 13, 1989|REV 1.0|Original draft|
|MAR 17, 1989|REV 1.1|Change name/Correct mistakes/Change port, SEGWR/cartridge access|
|APR 04, 1989|REV 1.2|Add cartridge information|
|APR 27, 1989|REV 2.0|Add TRAP & forced NOP info/correct breakpoint processing|
|MAY 15, 1989|REV 2.1|Minor changes|
|MAY 24, 1989|REV 3.0|New memory map/new CBUS/new analyzer functions|
|JUN 08, 1989|REV 3.1|Minor changes|

## 1.0 Overview:

*Handevelop* is a software development system for the Handy consumer game. Working with an Amiga computer, *Handevelop* allows a game programmer to load programs; control their execution; set breakpoints on address, data, control, and external matches; observe a trace of bus activity, including optional Suzy activity and video/refresh; and time arbitrary events. Numerous options allow great flexibility.

*Handevelop* contains the two custom chips used in Handy, as well as additional hardware for the debugging functions. Additionally, the DRAM used in Handy has been replaced by Static RAM.

In this document, all address are in hexadecimal.

*Handevelop* operates in one of two modes - execute and debug.

A dip switch selects which mode is entered from reset. If entry into debug mode is selected, an NMI is forced by the hardware immediately after reset negates.

A memory map of *Handevelop* is given in Table 1. All named addresses are typed in capital letters. For multi-byte reads or writes, the most significant byte of data is at the least significant address, maintaining the standard of the 6502.

### 1.1 Execute mode:

In execute mode, an application program is being run. The debugger is completely transparent, and operation proceeds exactly as it would in a real Handy.

### 1.2 Debug mode:

In debug mode, the CPU is either executing special debug code or is waiting for further instructions. When in debug mode, part of Handy's address space is mapped away for use by *Handevelop*. Specifically, the space from `0000` to `3FFF` will not decode to Handy RAM, but to Handevelop RAM, ROM, and other registers. A control bit in *Handevelop* allows a remapping of Handy RAM to make the masked area available. By setting the bit `SEG`, the RAM from `0000` to `3FFF` will be remapped to `4000` to `7FFF`. The RAM normally at those locations will be masked. With `SEG` cleared, the RAM is normally mapped, with `4000` to `7FFF` appearing normally and `0000` to `3FFF` masked. Of course, in execute mode, all Handy RAM appears normally, and `SEG` has no effect. This is summarized in Table 3. 

Table 1: Handevelop memory map

||||||
|---|---|---|---|---|
|0000-07FF|RAM|R/W|DO-D7||
|1000-1001|RCARTEN|R/W|DO-D7|Read and write cartridge|
|1100-11FF|ABREN|R/W|DO-D3|Set Abus breaks; Dn -> Channel n|
|1200-12FF|DBREN|R/W|DO-D3|Set Dbus breaks; Dn -> Channel n|
|1300-13FF|CBREN|R/W|DO-D3|Set Cbus breaks; Dn -> Channel n|
|1400-1403|ANALRD|R|DO-D7|32 bits of trace data|
|1510-1512|ANADD|R/W|DO-D7|24 bits of current trace address|
|1518-151A|FRSTBRK|R|DO-D7|24 bits of first break address|
|1520-1523|TIMERDEN|R|DO-D7|32 bits of timer value|
|1540-1542|LDDELO|W|DO-D7|24 bits of channel 0 break delay|
|1543|BRKCONO|W|DO-D3|Any write clears BRKLO|
||AENO|W|DO|0 -> Disregard Abus|
|||||1 -> Enable Abus compares|
||DENO|W|D1|0 -> Disregard Dbus|
||CENO|W|D2|0 -> Disregard Cbus|
|||||1 -> Enable Cbus compares|
||DELSELO|W|D3|0 -> Clock delay with PH2|
|||||1 -> Clock delay with Sync|
|1550-1552|LDDEL1|W|DO-D7|24 bits of channel 1 break delay|
|1553|BRKCON1|W|DO-D3|Any write clears BRKL1|
||AEN1|W|DO|0 -> Disregard Abus|
|||||1 -> Enable Abus compares|
||DEN1|W|D1|0 -> Disregard Dbus|
|||||1 -> Enable Dbus compares|
||CENI|W|D2|0 -> Disregard Cbus|
|||||1 -> Enable Cbus compares|
||DELSEL1|W|D3|0 -> Clock delay with PH2|
|||||1 -> Clock delay with Sync|
|1560-1562|LDDEL2|W|DO-D7|24 bits of channel 2 break delay|
|1563|BRKCON2|W|DO-D3|Any write clears BRKL2|
||AEN2|W|DO|0 -> Disregard Abus|
|||||1 -> Enable Abus compares|
||DEN2|W|D1|0 -> Disregard Dbus|
|||||1 -> Enable Dbus compares|
||CEN2|W|D2|0 -> Disregard Cbus|
|||||1 -> Enable Cbus compares|


## Table 2: Port operation

||||||
|---|---|---|---|---|
|1000|STATUS|R|D6-D7|BUSY -> D6|
|||||DATA AVAILABLE -> D7|
|1001|DATA|R/W|DO-D7|PORT DATA|
|1002|DIRECTION|W|D7|0 -> OUTPUT|
|||||1 -> INPUT|
|1003|POUT|W|D7|D7 -> POUT

> ### Note:
> When receiving data, `DATA AVAILABLE` goes `HI` when data has been received. When sending data, `DATA AVAILABLE` goes `LOW` until the Amiga acknowledges the transfer.

## Table 3 Handy RAM mapping

|Mode|SEG|CPU address|Handy RAM address|
|---|---|---|---|
|Execute|X|0000 - FFFF|0000 - FFFF|
|Debug|0|0000 - 3FFF|No select|
|||4000 - 7FFF|4000 - 7FFF|
|||8000 - BFFF|8000 - BFFF|
|||C000 - FFFF|C000 - FFFF|
|Debug|1|0000 - 3FFF|No select|
|||4000 - 7FFF|0000 - 3FFF|
|||8000 - BFFF|8000 - BFFF|
|||C000 - FFFF|C000 - FFFF|
