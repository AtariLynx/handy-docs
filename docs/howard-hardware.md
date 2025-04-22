# Howard Board Hardware Notes

There is a bank of eight DIP switches located next to the cartridge connector. The functions are as follows:

## SW1 & SW2

Analyzer RAM Configuration. Up to 128K words of trace RAM may be installed, either as 16 32K x 8 parts or 4 128K x 8 parts. There is no need to notify the system of the size of RAM chip being used, only the total number of physical parts.

|SW2|SW1|Amount of RAM|
|---|---|---|
|on|on|4 32K x 8 RAMS - 32K lines of trace <br/> or 4 128K x 8 RAMS - 128k lines of trace|
|on|off|8 32K X 8 RAMS - 64K lines of trace|
|off|on|16 32K x 8 RAMS - 128K lines of trace |
|off|off|Trace disabled|

## SW3 through SW6

Cartridge emulation RAM. Up to 512K bytes of emulation RAM may be installed in each section of cartridge space, using 4 128K x 8 RAMS. With 32K x 8 RAMS, up to eight may be installed, allowing for 256K bytes of emulation memory. Optionally, EPROMS may be installed in the sockets as desired. The option switches specify the type of chips used. Each section is independant, but only one type of memory chip may be used in each section.

|SW4|SW3|Section Zero Memory Type<br/> (Read from address `FCB2`)|
|---|---|---|
|on|on|32K × 8 RAM|
|on|off|32K × 8 EPROM|
|off|on|128K x 8 RAM|
|off|off|128K × 8 EPROM|

|SW6|SW5|Section One Memory Type<br/>(Read from address `FCB3`)|
|---|---|---|
|on|on|32K × 8 RAM|
|on|off|32K × 8 EPROM|
|off|on|128K x 8 RAM|
|off|off|128K x 8 EPROM|

## SW7

Reset operation. This switch controls whether the system comes out of reset into `Debug` mode, or `Handy` mode. A reset into `Debug` mode means that communication with the Amiga is established as soon as reset negates. No code is executed. A reset into `Handy` mode means that the system begins executing MIKEY ROM, looking for a cartridge, etc. Normally, the system should reset into `Debug` mode.

|SW7|Reset Operation|
|---|---|
|off|Reset into Debug mode|
|on|Reset into Handy mode|

## SW8

Cartridge memory write protect. This switch controls whether the cartridge emulation memory may be written while in Handy mode. Normally, writing while in Handy mode should be prohibited. However, if a developer has decided to put RAM into a cartridge, this switch will enable the signal `AUDIN` to function as a read/write line for the cartridge RAM.

|SW8|Cartridge Memory Write Protect|
|---|---|
|on|Cartridge is write protected in `Handy` mode|
|off|AUDIN functions as a cartridge read/write line in Handy mode.|

> ### Note
>
> RAMs used for trace and emulation memory must have an access time at least as fast as 100 ns.
