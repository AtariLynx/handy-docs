## Handebug Downloading a Game Cartridge

Either `Control-G` or `Shift-F4` will start the cart download process.

*Handebug* presents the `FileIO` requester for the user to specify the game cartridge data file. This requester is accompanied by the extra gadgets box that has these gadgets:

- "Page" `LONGINT` gadget
- "Offset" `LONGINT` gadget
- "Reset" `BOOL` gadget, which sets `Page` and `Offset` back to zero
- "Port 0" and "Port 1" `BOOL` gadget that toggles between these two
- Mutually-excluding ROM-size gadgets (pertaining to the particular port): `16K`, `32K`, `64K`, `128K`, `256K`, `512K`, `1M`

*Handebug* uploads `$1800` bytes starting from `$2000` and stashes them for later restoration.

*Handebug* downloads the program `HANDY:cartload.bin` and then performs a `GO`.

The `cartload.bin` program does a `NotifyHandyReset`. *Handebug* waits for this signal, times out after awhile.

*Handebug* sends down the page (8 bits) and offset (16 bits) cart destination address.

*Handebug* sends down the size (24 bits) of the data.

*Handebug* sends down a status byte where: 

- bit 7 This bit is clear if the user has specified Port 0, set if Port 1 
- bits 6 - 3 These bits are currently unused, should be zero
- bits 2 - 0 These bits are used to select the ROM size of the port. These are the possible values:
  - 7 = 1M 
  - 6 = 512K
  - 5 = 256K
  - 4 = 128K
  - 3 = 64K
  - 2 = 32K
  - 1 = 16K
  - 0 = unused

Finally, *Handebug* sends down the data.

After the `cartload.bin` program has received all the data and copied it into the game cartridge, the program executes a `BRK` instruction. The monitor is reentered, and on entry a `NotifyHandyReset` is performed. After downloading the data, *Handebug* waits for this signal, times out after awhile. If the signal is received, *Handebug* replaces the stashed `$1800` bytes to `$2000`.

The ROM size of each of the two ports should be made part of the settings saved in the `HANDY:handebug.config` file.

## Handebug Causing a Game Cartridge to Execute

Either `Control-X` or `Shift-F5` will start the game execution process.

*Handebug* downloads the program `HANDY:cartgo.bin` and then performs a `GO`. After this, *Handebug* behaves as it normally would after the user used the `GO` command.
