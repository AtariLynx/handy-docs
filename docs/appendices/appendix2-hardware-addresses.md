# Handy Appendix 2 - Hardware Addresses

|||
|---|---|
|Original 21-Dec-87|Dave Needle|
|6-Mar-89|Rev K|
|27-Mar-89|Rev K-1|
|12-Apr-89|Rev K-2|
|17-Apr-89|Rev L|
|22-May-89|Rev L-1|
|30-May-89|Rev L-2|
|16-June-89|Rev L-3|
|7-July-89|Rev M (DevCon)|

## 1. Basic Address Ranges

The 6502 CPU has an address range of 64K bytes. The upper 1K byte of address space is shared by the RAM, the ROM, Suzy hardware and Mikey hardware. This upper 1K byte of space is divided as follows:

||||
|---|---|---|
|`FFFE`, `FFFF`|CPU Interrupt Vector||
|`FFFC`, `FFFD`|CPU Reset Vector|RAM or ROM|
|`FFFA`, `FFFB`|CPU NMI Vector||
|`FFF9`|Memory Map Control|Mikey and Suzy hardware|
|`FFF8`|Reserved for future hardware|Mikey and Suzy hardware|
|`FE00` thru `FFF7`|ROM Space||
|`FD00` thru `FDFF`|Mikey Space||
|`FC00` thru `FCFF`|Suzy Space||

The above listed adress ranges (except for `FFF8` and `FFF9`) can, under control of the CPU, have RAM overlayed on them. These overlays are controlled by the bits in the hardware register at `FFF9`. Both Mikey and Suzy accept a write at those addresses but only Mikey responds to a read.

The remaining 63k bytes are RAM.

These address ranges affect only CPU accesses from Mikey. The video and refresh generators in Mikey and the sprite engine in Suzy 'see' the entire 64K byte range as RAM.

> ### Big note !!!!!  
> We are done messing around with these addresses. Do not change them anymore !!!

## 1.1 Legend

(W), (R), (R/W) indicate write only, read only, and read/write capability of a particular address. Please follow the rules. If an address is read only, DON'T write to it. If an address is write only, DON'T read it.

The reset condition is shown with bit 7 on the left (`B7` ...... `B0`). X indicates un-changed by reset.

(U) indicates that it is unsafe to read or write this address without first checking (at a different address) to see if it is available. Please follow the rules. A description of `UNSAFE` is in the hardware specification.

(CPU), (ENG), (SCB), indicate the usual source of data for this particular address. `CPU` = Mikey, `ENG` = sprite engine, `SCB` = DMA from the RAM based sprite control block.

## 1.2 Memory Map Control

|||
|---|---|
|`FFF9` = `MAPCTL`, Memory Map Control|(`R/W`) Mikey reset = `0,0,0,0,0,0,0,0`<br> (`W`) Suzy reset = `x,x,x,x,x,x,x,0` (Only bit `0` is implemented)|

||||
|---|---|---|
|`B7`|sequential disable|If set, the CPU will always use full cycles (5 ticks min), never a sequential cycle (4 ticks).|
|`B6`, `B5`, `B4`|reserved for future use||
|`B3`|`FFFA -> FFFF`|Vector Space|
|`B2`|`FE00 -> FFF7`|ROM Space|
|`B1`|`FD00 -> FDFF`|Mikey Space|
|`B0`|`FC00 -> FCFF`|Suzy Space|

All 8 bits are set to `0` at reset. Any address space bit that is set to a `1` will cause its related address space to access RAM instead of the hardware or ROM normally accessed.

## 2. Suzy Addresses

### `FC00 -> FC7F`: Sprite Control Block

(R/W) (U) reset = `x,x,x,x,x,x,x,x` 

Even addresses are the LSB. Any CPU write to an LSB will set the MSB to `0`.

|??|Source|Address|Name|Description|
|---|---|---|---|---|
|`L0`|(ENG)|`00`,`01`|`TMPADRL`,`H`|Temporary address|
|`H0`|(ENG)|`02`,`03`|`TILTACUML`,`H`|Accumulator for tilt value|
|`L1`|(CPU)|`04`,`05`|`HOFFL`,`H`|Offset to H edge of screen|
|`H1`|(CPU)|`06`,`07`|`VOFFL`,`H`|Offset to V edge of screen|
|`L2`|(CPU)|`08`,`09`|`VIDBASL`,`H`|Base Address of Video Build Buffer|
|`H2`|(CPU)|`0A`,`0B`|`COLLBASL`,`H`|Base Address of Coll Build Buffer|
|`L3`|(ENG)|`0C`,`0D`|`VIDADRL`,`H`|Current Video Build Address|
|`H3`|(ENG)|`0E`,`0F`|`COLLADRL`,`H`|Current Collision Build Address|
|`L4`|(SCB)|`10`,`11`|`SCBNEXTL`,`H`|Address of Next SCB|
|`H4`|(SCB)|`12`,`13`|`SPRDLINEL`,`H`|Start of Sprite Data Line Address|
|`L5`|(SCB)|`14`,`15`|`HPOSSTRTL`,`H`|Starting Hpos|
|`H5`|(SCB)|`16`,`17`|`VPOSSTRTL`,`H`|Starting Vpos|
|`L6`|(SCB)|`18`,`19`|`SPRHSIZL`,`H`|H Size|
|`H6`|(SCB)|`1A`,`1B`|`SPRVSIZL`,`H`|V Size|
|`L7`|(ENG)|`1C`,`1D`|`STRETCHL`,`H`|H Size Adder|
|`H7`|(ENG)|`1E`,`1F`|`TILTL`,`H`|H Position Adder|
|`L8`|(ENG)|`20`,`21`|`SPRDOFFL`,`H`|Offset to Next Sprite Data Line|
|`H8`|(ENG)|`22`,`23`|`SPRVPOSL`,`H`|Current Vpos|
|`L9`|(CPU)|`24`,`25`|`COLLOFFL`,`H`|Offset to Collision Depository|
|`H9`|(ENG)|`26`,`27`|`VSIZACUML`,`H`|Vertical Size Accumulator|
|`L10`|(CPU)|`28`,`29`|`HSIZOFFL`,`H`|Horizontal Size Offset|
|`H10`|(CPU)|`2A`,`2B`|`VSIZOFFL`,`H`|Vertical Size Offset|
|`L11`|(ENG)|`2C`,`2D`|`SCBADRL`,`H`|Address of Current SCB|
|`H11`|(ENG)|`2E`,`2F`|`PROCADRL`,`H`|Current Spr Data Proc Address|
||(ENG)|`30` thru `4F`|Reserved|
||(ENG)|`50`,`51`|Do Not Use|
|`H4`|(ENG)|`52`|`MATHD` (`12`)|
|`H4`|(ENG)|`53`|`MATHC` (`13`)|
|`L5`|(ENG)|`54`|`MATHB` (`14`)|
|`L5`|(ENG)|`55`|`MATHA` (`15`)|
|`H5`|(ENG)|`56`|`MATHP` (`16`)|
|`H5`|(ENG)|`57`|`MATHN` (`17`)|
||(ENG)|`58` thru `5F`|Do Not Use|
|`L8`|(ENG)|`60`|`MATHH` (`20`)|
|`L8`|(ENG)|`61`|`MATHG` (`21`)|
|`H8`|(ENG)|`62`|`MATHF` (`22`)|
|`H8`|(ENG)|`63`|`MATHE` (`23`)|
||(ENG)|`64` thru `6B`|Do Not Use|
|`L11`|(ENG)|`6C`|`MATHM` (`2C`)|
|`L11`|(ENG)|`6D`|`MATHL` (`2D`)|
|`H11`|(ENG)|`6E`|`MATHK` (`2E`)|
|`H11`|(ENG)|`6F`|`MATHJ` (`2F`)|
||(ENG)|`70` thru `7F`|Do Not Use|

These are used for MATH operations. The meanings of the letters are explained in the spec.

### `FC80` = `SPRCTL0`: Sprite Control Bits 0
(W) (U) Reset = `x,x,x,x,x,x,x,x`  
||||
|---|---|---|
|`B7`,`B6`|Bits/pixel-1|(1,2,3,4)|
|`B5`|H flip|`0` = Not flipped|
|`B4`|V flip|`0` = Not flipped|
|`B3`|Reserved||
|`B2`,`B1`,`B0`|Sprite Type|`7` -> Shadow<br>`6` -> Exclusive-or-Shadow<br>`5` -> Non-collidable<br>`4` -> Normal<br>`3` -> Boundary<br>`2` -> Boundary-Shadow<br>`1` -> Background-No collision<br>`0` -> Background, Shadow<br>|

(old)
||||
|---|---|---|
|`B2`,`B1`,`B0`|Sprite Type|`7` -> Normal<br>`6` -> Exclusive-or|`5` -> Non-collidable|`4` -> Shadow|`3` -> Boundary-shadow|`2` -> Boundary|`1` -> Background-No collision|`0` -> Background|

### `FC81` = `SPRCTL1`: Sprite Control Bits 1

(W) (U) reset = `x,x,x,x,x,x,x,X` 

||||
|---|---|---|
|`B7`|Literal attribute|`0` = Normal<br>`1` = Totally literal|
|`B6`|Sizing algorithm choice|`0` = Adder (algo 4),<br>`1` = Shifter (algo 3)<br>(SET IT TO ZERO !!!! , algo 3 is broke)|
|`B5`,`B4`|Reloadable depth|`0` = None<br>`1` = Hsize, Vsize<br>`2` = Hsize, Vsize, Stretch<br>`3` = Hsize, Vsize, Stretch, Tilt|
|`B3`|Palette re-load|`0` = Reload the palette<br>`1` = Use existing palette|
|`B2`|Skipsprite|`1` = Skip this sprite<br>`0` = Use this sprite|
|`B1`|Start drawing up|`1` = Up, `0` = Down|
|`B0`|Start drawing left|`1` = Left, `0` = Right|

### `FC82` = `SPRCOLL`: Sprite Collision Number

(W) (U) reset = `x,x,x,x,x,x,x,x`

||||
|---|---|---|
|`B7`,`B6`|`0`||
|`B5`|Don't collide|`1` = Don't collide with this sprite|
|`B4`|`0`||
|`B3`,`B2`,`B1`,`B0`|Number||

### `FC83` = `SPRINIT`: Sprite Initialization Bits

(W) (U) reset = `x,x,x,x,x,x,x,x`

||||
|---|---|---|
|B7|fc1|
|B6|fc2|
|B5|fc3|
|B4|Reserved| 
|B3|ac1|
|B2|ac2|
|B1|ac3|
|B0|ac4|

Set to 'F3' after at least 100ms after power up and before any sprites are drawn.

FC84 -> FC87 = not yet allocated

### `FC88` = `SUZYHREV`: Suzy Hardware Revision 

(R) ='01'

### `FC89` = `SUZYSREV`: Suzy Software Revision

(W) No actual register is implemented

### `FC8A` -> `FC8F`: Not yet allocated

### `FC90` = `SUZYBUSEN`: Suzy Bus Enable FF

(W) Reset = `0` 

||||
|---|---|---|
|`B0`|Suzy Bus Enable|`0` = Disabled|

### `FC91` = `SPRGO`: Sprite Process Start Bit

(W) Reset = `0`

||||
|---|---|---|
|`B0`|Sprite process enabled|`0` = Disabled<br>Write a `1` to start the process.<br>At completion of process this bit will be reset to `0`.<br>Either setting or clearing this bit will clear the `Stop At End Of Current Sprite` bit.|
|`B1`|Reserved||
|`B2`|Enable everon detector|`1` = Enabled|

### `FC92` = `SPRSYS`: System Control Bits.

(R/W) (Write) Reset = `x,x,x,x,x,x,x,x`

||||
|---|---|---|
|`B7`|Signmath|`0` = Unsigned math<br>`1` = Signed math|
|`B6`|OK to accumulate|`0` = Do not accumulate<br>`1` = Yes, accumulate|
|`B5`|Don't collide|`1` = Don't collide with any sprites|
|`B4`|Vstretch|`1` = Stretch the v<br>`0` = Don't play with it, it will grow by itself.|
|`B3`|Lefthand|`0` = Normal handed|
|`B2`|Clear the `UnsafeAccess` bit|`1` = Clear it<br>`0` = No change|
|`B1`|Stop at end of current sprite|`1` = Request to stop. Continue sprite processing by setting the `Sprite Process Start` Bit.<br>Either setting or clearing the SPSB will clear this stop request|
|`B0`|Has no effect||

(read) reset = `0,0,0,x,x,0,x,0`

||||
|---|---|---|
|`B7`|Math in process||
|`B6`|Mathbit|If mult, `1` = accumulator overflow. If div, `1` = Div by zero attempted|
|`B5`|Last carry bit||
|`B4`|Vstretch||
|`B3`|Lefthand||
|`B2`|UnsafeAccess|`1` = Unsafe Access was performed|
|`B1`|Stop at end of current sprite|`1` = Request to stop|
|`B0`|Sprite process was started and has neither completed nor been stopped|

### `FC93` -> `FCAF`: Not yet allocated

### `FCB0` = `JOYSTICK`: Read Joystick and Switches (R)

||||
|---|---|---|
||If Lefthand = `1`|If Lefthand = `0`|
|`B7`|Joy Up|(Down)|
|`B6`|Joy Down|(Up)|
|`B5`|Joy Left|(Right)|
|`B4`|Joy Right|(Left)|
|`B3`|Option 1 (was Re-Start)|Option 1 (was Re-Start)|
|`B2`|Option 2 (was Pause)|Option 2 (was Pause)|
|`B1`|Inside (was Right Fire)|Inside (was Right Fire)|
|`B0`|Outside (was Left Fire)|Outside (was Left Fire)|

### `FCB1` = `SWITCHES`: Read Other Switches (R)

||||
|---|---|---|
|`B7`|`0`|
|`B6`|`0`|
|`B5`|`0`|
|`B4`|`0`|
|`B3`|`0`|
|`B2`|Cart1 I/O Inactive|
|`B1`|Cart0 I/O Inactive|
|`B0`|Pause (was Flablode)|

This picture shows the `Normal` orientation of the unit, which according to our previous definition is the `LeftHand` orientation. Please realize that this means that the normal setup is `Flip` = Off, and `LeftHand` = On.

The current agreement is that `Pause` and `Option 1` pressed together will result in the patter of little rubber feet around the house. Also, `Pause` and `Option 2` pressed together will be used to indicate a user request for the `Flip` function.

### `FCB2`,`FCB3`: `RCART` (R/W)

Read or write 8 bits of data. `FCB2` uses `CART0/` as the strobe, `FCB3` uses `CART1/` as the strobe.

### `FCB4` -> `FCBF`: Not yet allocated

### `FCC0`: LEDS 

(W) Reset = `0,0,0,0,0,0,0,0`

A `0` in a bit will turn its corresponding LED on.

### `FCC1`: Reserved

### `FCC2`: Parallel Port Status

(R/W) Reset = `0,0,x,x,x,x,x,x`

(Write)

||||
|---|---|---|
|`B7`|Direction|`0` = input<br>`1` = output|
|`B6`|Paper out, used to signal the device at the other end of the parallel cable.|

(Read)

|||
|---|---|
|`B7`|Data available.<br>When port is in input mode, `B7`=`1` indicates that a character has arrived and not yet been read by the CPU. This bit will be cleared when the CPU reads the parallel port data.<br>When in output mode, `B7`=`1` indicates that an output character is in the data register and awaiting acknowledgement from the device on the other end of the parallel cable. This bit is cleared when that other device accepts the character.<br>In addition, this bit is cleared whenever the direction of the port is changed.|
|`B6`|Hello Handy<br>This bit is connected to the `Busy` pin of the parallel port. It is intended as a signal line FROM the device on the other end of the parallel cable.|

### `FCC3`: Parallel Port Data

(R/W)  
Read or write as appropriate.

### `FCC4`: Howie

(R/W)  
Read or write as appropriate.

### `FCC5`: `Reserved`

### `FCC6` -> `FCCF`: Not yet allocated

### `FCDO` -> `FCFF`: Not yet allocated

## 3. Mikey Addresses (R/W)

Timers are reset to 0

|From|To|Description|Function|
|---|---|---|---|
|`FD00`|`FD03`|Timer channel 0|Hcount|
|`FD04`|`FD07`|Timer channel 1|Mag0a (read current state of TAPE0 in b7)|
|`FD08`|`FDOB`|Timer channel 2|Count|
|`FDOC`|`FDOF`|Timer channel 3|Mag0b|
|`FD10`|`FD13`|Timer channel 4|Serial rate|
|`FD14`|`FD17`|Timer channel 5|Magia (read current state of TAPE1 in b7)|
|`FD18`|`FD1B`|Timer channel 6||
|`FD1C`|`FD1F`|Timer channel 7|Mag1b|

|Address|Names|Description|
|---|---|---|
|`FD00`|`TIMOBKUP`, `HTIMBKUP`|Timer 0 backup value|
|`FD01`|`TIMOCTLA`, `HTIMCTLO`|Timer 0 static control|
|`FD02`|`TIMOCNT`|Timer 0 current count|
|`FD03`|`TIMOCTLB`|Timer 0 dynamic control|
|`FD04`|`TIM1BKUP`. `MAGA`|Timer 1 backup value|
|`FD05`|`TIM1CTLA`|Timer 1 static control|
|`FD06`|`TIM1CNT`|Timer 1 current count|
|`FD07`|`TIM1CTLB`|Timer 1 dynamic control|
|`FD08`|`TIM2BKUP`, `VTIMBKUP`|Timer 2 backup value|
|`FD09`|`TIM2CTLA`|Timer 2 static control|
|`FD0A`|`TIM2CNT`|Timer 2 current count|
|`FD0B`|`TIM2CTLB`|Timer 2 dynamic control|
|`FD0C`|`TIM3BKUP`, `MAGB`|Timer 3 backup value|
|`FD0D`|`TIM3CTLA`|Timer 3 static control|
|`FD0E`|`TIM3CNT`|Timer 3 current count|
|`FD0F`|`TIM3CTLB`|Timer 3 dynamic control|
|`FD10`|`TIM4BKUP`. `BAUDBKUP`|Timer 4 backup value|
|`FD11`|`TIM4CTLA`|Timer 4 static control|
|`FD12`|`TIM4CNT`|Timer 4 current count|
|`FD13`|`TIM4CTLB`|Timer 4 dynamic control|
|`FD14`|`TIM5BKUP`, `MAGC`|Timer 5 backup value|
|`FD15`|`TIM5CTLA`|Timer 5 static control|
|`FD16`|`TIM5CNT`|Timer 5 current count|
|`FD17`|`TIM5CTLB`|Timer 5 dynamic_control|
|`FD18`|`TIM6BKUP`|Timer 6 backup value|
|`FD19`|`TIM6CTLA`|Timer 6 static control|
|`FD1A`|`TIM6CNT`|Timer 6 current count|
|`FD1B`|`TIM6CTLB`|Timer 6 dynamic control|
|`FD1C`|`TIM7BKUP`, `MAGD`|Timer 7 backup value|
|`FD1D`|`TIM7CTLA`|Timer 7 static control|
|`FD1E`|`TIM7CNT`|Timer 7 current count|
|`FD1F`|`TIM7CTLB`|Timer 7 dynamic control|

| Offset  | Bit(s)      | Description                                                                         |
|---------|-------------|-------------------------------------------------------------------------------------|
| `xxx0`  |             | Backup value (count+1)                                                              |
| `xxx1`  | | Control bits |
|  | `B7`          | Enable interrupt                                                                    |
|  | `B6`          | Reset timer done (see `xxx3`, `B3`)                                                   |
|  | `B5`          | Unused in timers 0,2,4,6<br>Magmode in timers 1,3,5,7                                |
|  | `B4`          | Enable reload                                                                       |
|  | `B3`          | Enable count                                                                        |
|  | `B2`,`B1`,`B0`    | Clock select:<br>`0`=1µs<br>`1`=2µs<br>`2`=4µs<br>`3`=8µs<br>`4`=16µs<br>`5`=32µs<br>`6`=64µs<br>`7`=Linking |
| `xxx2`  |             | Timer current value                                                                 |
| `xxx3`  | | Other control bits |
|  | `B7`–`B4`       | Unused                                                                              |
|  | `B3`          | Timer done<br>Set on timeout<br>Reset with the reset timer done bit (`xxx1`, `B6`)      |
|  | `B2`          | Last clock                                                                          |
|  | `B1`          | Borrow in                                                                           |
|  | `B0`          | Borrow out                                                                          |
