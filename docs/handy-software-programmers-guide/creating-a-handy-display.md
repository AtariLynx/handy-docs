# Creating a Handy display

This chapter describes the steps required to create a Handy display, with the primary emphasis on the system macros and support code that you should use. All aspects of creating a display are covered, from initializing the display hardware and color palette to rendering sprites, double-buffering, waiting for end-of-frame, and more. The chapter ends with a summary of all of the system's display macros and a few examples of their usage.

## Introduction and overview

To create a normal Handy display using sprites, you must:

- Set up the display timers to create the desired display update rate (also known as the frame rate)
- Write the address of your render buffer into the appropriate Suzy sprite registers
- If you want hardware collision, write the address of your collision buffer into the appropriate Suzy sprite registers
- Render your sprites
- Display the buffer

Another Handy display that you might be interested in making is the still-picture display. This is the sort of display where you aren't interested in fancy double-buffering and all, you simply have a picture to display or you want to render a picture and then display it. To display a still-picture you must:

- Set up the display timers to create any display update rate (the suggested rate for still pictures is 60 frames per second)
- Fill in your display buffer with display data of some sort, including using the sprite engine if you're careful to take the necessary preliminary steps
- Display your buffer

The Handy display macros take care of all the details of the above steps for you, making the creation and management of a Handy display an easy affair. This chapter describes these macros - with respect to the hardware they affect - and explains how to use the macros.

The sprite rendering and display creation macros can be found in the files `6502:macros/sprite.mac` and `6502:macros/display.mac`. Supporting source code can be found in the files `6502:src/sprite.src` and `6502:src/display.src`. If you are going to use sprites you must include both `sprite.mac` and `sprite.src`. If you are going to create displays you must include both `display.mac` and `display.src`.

## Initializing the display hardware and system software

To initialize the Handy display, you use the `SETDISP` macro or one of its big brothers `SETDISP_60`, `SETDISP_50`, `SETDISP_30`, `SETDISP_25`, `SETDISP_20`, `SETDISP_15` and `SETDISP_10`. These macros, known collectively as the set-display macros, do everything to make a display except actually turn the display on (you do that later after everything is all set up; see [Turning The Display On And Off](#turning-the-display-on-and-off) for details).

An essential step taken by the set-display macros is to establish the display's frame rate, which is the number of times per second that a new buffer will be displayed by the hardware.

If you want your display to have one of the 'standard' frame rates, you can use one of the macros `SETDISP_60`, `SETDISP_50`, `SETDISP_30`, `SETDISP_25`, `SETDISP_20`, `SETDISP_15` or `SETDISP_10`. These macros set up the display for 60, 50, 30, 25, 20, 15 or 10 frames per second respectively by invoking the `SETDISP` macro with appropriate arguments.

If you want a frame rate that's not among the 'standard' frame rates, you can use the `SETDISP` macro directly. The `SETDISP` macro requires 5 arguments: the reload value and control flags for both the vertical and horizontal display timers which are named `HCOUNT` and `VCOUNT`, and the magic 'P' value for the `PBKUP` display register. This gets the display hardware running in such a way that the correct amount of time is spent displaying each line of your buffer so that its beautiful glow correctly reflects in the eye of the beholder. To understand what values should be used for an invocation of `SETDISP`, refer to the """Display""" section of the """Handy Hardware Specification""" manual for a discussion and formulae.

The `SETDISP` macro also initializes `DISPCTL_RAM`, which is the RAM-based shadow mask of the hardware's `DISPCTL` register.

If you are going to be using sprites then your initialization process should include a call to `INITSUZY`, which must be done only once and must be done before the first use of the sprite engine.

If you are going to be waiting for end-of-frame or end-of-line then your initialization process should include a call to `INITEOF` and/or `INITEOL`, which must be done only once and must be done before the first use of the wait macros.

You need to set up a color palette. See the section [Setting The Display Colors](#setting-the-display-colors) for details.

If you're going to use hardware collision detection, you need to perform some initialization. See the section [Setting Up For Hardware Collisions](#setting-up-for-hardware-collisions) for details.

If you're going to be double-buffering, you need to perform some initialization. See the section [Setting Up For Double-Buffering](#setting-up-for-double-buffering) for details.

## Setting the display colors

The `RGB16`, `RGB16_1` and `RGB_AXY` macros set up the Handy color palette for you. `RGB16` and `RGB16_I` allow you to set up all 16 colors at once, while `RGB_AXY` lets you change the values of a single color.

The set-color macros take effect immediately, so if you call them at any time other than during horizontal or vertical retrace you may get display glitches. So, you may ask, how do you wait until horizontal or vertical retrace? See the section [Waiting For End Of Frame And End Of Line](#waiting-for-end-of-frame-and-end-of-line) for details.

## Setting the display's horizontal and vertical offsets

The `HOFF8`, `VOFF8`, `HOFF16` and `VOFF16` macros, known collectively as the set-offset macros, set up the horizontal and vertical offsets of the Handy display window within the imaginary display world. The values that you might choose to provide to these macros is discussed in detail in the """Positioning of the Handy display and sprites""" chapter of this manual.

The set-offset macros take effect immediately. The very next time you render sprites the new offsets will be used.

## Setting up for hardware collisions

If you're going to use hardware collision detection, you should use the macro `SETCOLL`, which expects two arguments: the address of a collision buffer that's `DISPLAY_BUFSIZE` big and aligned on a 4-byte boundary, and the offset into your Sprite Control Blocks of the byte that will act as your collision depository. Refer to the """Handy Hardware Specification""" manual for complete details about the meaning of these two values.

Note that the offset to the sprite's collision depository is a 16-bit signed value, which allows you to specify a negative offset to your depository if you want your collision values to be written to memory locations that precede your SCB's. This is a very reasonable thing to do if you have SCB's that vary in size, which they do when you take advantage of the sprite engine's reuse-data ability.

## Declaring a display buffer

Handy hardware requires that your display buffers start on an address whose binary representation ends with two bits of zero (any address which is an even multiple of 4) and must be `DISPLAY_BUFSIZE` (currently `8160`, or `$1FE0`) bytes long. You can force your buffer to be aligned on an address that's a multiple of 4 by using the assembler's `.ALIGN` directive, like this:

```
	SETDBUF Bufferl, Buffer2
	...

	.ALIGN 4 
Bufferl	.DS DISPLAY_BUFSIZE
	.ALIGN 4
Buffer2 .DS DISPLAY_BUFSIZE
```

## Rendering sprites

Before the first time you render sprites, you must invoke the `INITSUZY` macro. You should do this only once, during the initialization sequence of your program.

The `SPRITES` macro renders your list of sprites into the off-display buffer pointed to by the variable `RenderBuffer`. The double-buffer logic initializes `RenderBuffer` for you. If you are not double-buffering, you need to set `RenderBuffer` to the address of your buffer.

The `SPRITES` macro doesn't automatically cause the render buffer to be displayed, but instead allows you to call `SPRITES` multiple times with different sprite lists before making the final display visible to the user. After you're done rendering all of your sprites, to make the `RenderBuffer` the on-display buffer use `DBUF_DISPLAY` if you are double-buffering or `DISPLAY` if you're not.

`SPRITES` calls `WAITSUZY` for you while the sprite engine is running, which serves dual purposes: it puts the CPU to sleep which allows the sprite engine to run; it returns control to you only after the sprite engine is finished, after which it's safe for you to do any Suzy stuff like using the math hardware or starting up the sprite engine again. Note that because `SPRITES` calls `WAITSUZY` you should not call `WAITSUZY` after calling `SPRITES`, else the system will hang forever.

You don't need to be double-buffering to use the `SPRITES` macro. However, if you aren't double-buffering you'll need to set the `RenderBuffer` variable to the address of your display buffer before using the `SPRITES` macro. Note that the value you store in `RenderBuffer` should be the top-left byte of your display, without regard to the state of `FLIP`.

The `INITSUZY` and `SPRITES` macros set up the sprite engine hardware in all the required ways, thereby simplifying your usage of the sprite engine. All of the steps performed by these macros are detailed in the """Sprite Engine Initialization""" section of the """Handy Specification""" document.

## About double-buffering

Usually, game designers will want to use two buffers for creating their displays. The buffers are alternately used as the display buffer and the render buffer. At any given instant, the display buffer is being displayed to the user while the render buffer is available for sprite rendering. The address of the render buffer can usually be found in a system-maintained variable named `RenderBuffer`, which will be described presently. When the programmer has finished rendering a new display into the render buffer, the render and display buffer roles are swapped. This technique of maintaining two buffers and swapping them as appropriate is known as double-buffering.

The Handy system software provides you with a collection of macros that make it easy to create a double-buffered display. All you need to do is:

- Define the addresses of your buffers using the `SETDBUF` macro
- Render your sprites using `SPRITES`
- Display your new buffer using `DBUF_DISPLAY`

## Setting up for double-buffering

You use the `SETDBUF` macro to define your display buffers and to set the system up for double-buffering. `SETDBUF` expects two arguments: the addresses of your two video buffers. One of the buffers will be set up as the render buffer, which you can begin rendering into. This buffer then will be displayed when you first call `DBUF_DISPLAY`.

The rules for declaring display buffers are discussed in the section [Declaring A Display Buffer](#declaring-a-display-buffer) above.

You provide the `SETDBUF` macro with the addresses of your buffers as they are defined when `FLIP` mode is not selected. If you want to flip the display, you do so after calling `SETDBUF` by using the `FLIP` macro (defined below).

Any time after invoking `SETDBUF` you can detect which buffer the system believes to be the render buffer by examining the system variable `RenderBuffer`, which is initialized within `SETDBUF` and thereafter managed by the system. At all times this variable will point to the first byte of the buffer that's currently the off-display buffer. However, note that it's not always safe to render into the `RenderBuffer` without first using the `WAITEOF` function. See the sections [Rendering Sprites](#rendering-sprites) and [Waiting For End Of Frame And End Of Line](#waiting-for-end-of-frame-and-end-of-line) for a description and discussion of these macros.

If you won't be using double-buffering to create your display - brave, aren't you - you don't need to use the `SETDBUF` macro.

## The double-buffer display dance

When double-buffering, first you define your buffers using `SETDBUF`, then you render your sprites into the render buffer using `SPRITES`, and finally you invoke the `DBUF_DISPLAY` macro to swap the display and render macros. Easy, eh?

Unfortunately, there's a catch: after control returns from `DBUF_DISPLAY` the `RenderBuffer` will contain the address of the buffer that's still be being displayed by the hardware. If you begin to render in the buffer before waiting for the start of the next frame, then you will probably overwrite the current display before the hardware is finished displaying it. The result: unsightly display dandruff. Instead, you'll need to wait until the hardware is finished displaying the current on-display buffer before you can start rendering into it as the new off-display buffer. This can be accomplished by using the `WAITEOF` (wait for end-of-frame) macro with a sequence of events something like this:

- Start the sprite engine using `SPRITES`, which returns control to you after the sprites have been rendered
· Use `DBUF_DISPLAY` to set up the hardware to show your newly rendered display buffer at the start of the next frame
- React to player input, perform enemy intelligence and do other game logic
- Set up your sprites for the next display frame
- Use `WAITEOF` to wait until the last line of the current display has been processed
- Restart at the top of this loop

The above technique requires that you are sure that your game logic will be completed before `EOF` is reached. If your code runs too long one frame and you miss `EOF`, you end up waiting all the way to the end of the next frame. An alternate way to wait until the hardware is finished displaying the current on-display buffer can be achieved by using the `WAITNEOF` (wait no-clear for end-of-frame) macro with a sequence of events something like this:

- Start the sprite engine using `SPRITES`, which returns control to you after the sprites have been rendered
- Use `DBUF_DISPLAY` to set up the hardware to show your newly rendered display buffer at the start of the next frame
- Stash the processor status byte, set interrupt disable, clear the `DISPLAY_EOFFLAG` in the `DisplayFlags` variable, restore the processor status byte
- React to player input, perform enemy intelligence and do other game logic
- Set up your sprites for the next display frame
- Use `WAITNEOF` (which doesn't clear the DISPLAY_EOFFLAG) to wait until the last line of the current display has been processed, (it may already have been processed)
- Restart at the top of this loop

However, everything costs. The problem with these alternate techniques is that it allows you to get 'way behind EOF.

The `WAITEOF` and `WAITNEOF` macros and the `DISPLAY_EOFFLAG` are described in the section [Waiting For End of Frame and End of Line](#waiting-for-end-of-frame-and-end-of-line) below.

After the `DBUF_DISPLAY` macro has done its specific double-buffering work, the address of the new display buffer is written to the hardware display registers by the system's end-of-frame handler.

## Presenting a still picture

Your program may want to present a still picture to the user. For instance, you may want to present a series of full-frame pictures to the user. Also, in a normal game environment, before your first sprite display you may want to display a startup screen. This section describes the technique for displaying a still picture.

If you are double-buffering using the system's macros and you want to render a picture into the off-display buffer, the `RenderBuffer` variable will contain the address of the buffer you should use. After you render into the buffer, display it as usual using the `DBUF_DISPLAY` macro.

If you have a single buffer that you want to display, use the `DISPBUF` macro. `DISPBUF` expects one argument: the address of your buffer.

## Displaying a buffer

To tell the hardware that you want the render buffer to be the next display buffer, use the `DISPLAY` macro.

The `DISPLAY` macro writes the `RenderBuffer` value `DISPADRL`,`H` registers after translating the value if the `FLIP` bits are set. After the hardware has finished displaying the current frame, it will use your new buffer pointer when setting up to display the next frame.

Under normal system operation the `DISPLAY` macro is invoked by the system's end-of-frame handler. You shouldn't need to use `DISPLAY` yourself.

## Turning the display on and off

After you've initialized the hardware, set your color palette, created a display buffer, filled it up with gorgeous imagery and told the hardware where to find it, finally you can turn on the display hardware. Aah.

The display is turned on using the cleverly-named `DISPLAY_ON` macro. Let's see, what might we call the macro to turn the display off? How about `DISPLAY_NOT_ON`? Nah, the macro to turn off the display is called `DISPLAY_OFF`.

These macros work by reading the value from `DISPCTL_RAM` which is a RAM shadow copy of the `DISPCTL` hardware register, then setting or clearing the display-on bit and saving the resultant value to both `DISPCTL` and `DISPCTL_RAM`.

You can call `DISPLAY_ON` and `DISPLAY_OFF` any time you want, as often as you want.

## Flipping the display (and the joystick)

To flip the Handy display 180° all you have to do is use the `FLIP` macro, which sets everything up to flip the display and sets the joystick control bit to flip the meaning of the joystick positions. This means that to flip the display, all you do is call `FLIP` and everything else works exactly the same, which makes flipping the display a complete no-brainer.

Note that regardless of the state of `FLIP` the `RenderBuffer` variable will contain the address of the top-left byte of the off-display buffer.

`FLIP` sets the `DISPCTL` register and `DISPCTL_RAM` variable when you call the macro. In the current implementation of the hardware, this could cause the display to flip immediately, even if the display is halfway rendered, which could cause the display to be mixed for part of a frame. If you want to avoid this effect, you should make sure that you call `FLIP` only immediately after `WAITEOF`. On the other hand, the effect is brief and therefore probably not worth mangling your logic too much.

## Waiting for End Of Frame and End Of Line

Many times you will want to do something that you shouldn't do until after the video is finished processing the last line of the display (or the last pixel of the current line). For instance, you might want to change the color palette at a time that is guaranteed to not glitch the display, or you may have finished everything you need to do for the current display and you want to wait until you're sure that the display buffers have been swapped before you start creating the next display.

The instant of time when the Handy hardware has just finished displaying the lines of the current buffer is known as end of frame (or `EOF`). When the hardware starts displaying the next buffer, this is known as top of frame. The interval between the end and top of frame is known as vertical retrace, which on video displays is the time required to reposition the electron beam to the top of the video tube. Handy has no need for a vertical retrace, but a pseudo vertical retrace interval is created anyway to allow programmers to make vertical retrace sorts of changes to the system.

The same is true horizontally. There is an instant of time known as the end of line (or `EOL`), and the hardware is set up to create a pseudo horizontal retrace time interval.

If you are going to use the `EOF` macros, you must define the constant `EOF_USER`. If you are going to use the `EOL` macros, you must define the constant `EOL_USER`. If you don't define these constants, the associated macros won't be defined and and supporting source code won't be included.

Before you use the `WAITEOF` and/or `WAITEOL` macros you must use `INITEOF` and/or `INITEOL`. You should invoke these macros only once, presumably during the initialization code of your program.

To wait for the end of frame, you use the `WAITEOF` macro. To wait for the end of a display line, you use the `WAITEOL` macro. These macros clear the system's `DisplayFlags` variable's `DISPLAY_EOFFLAG` and `DISPLAY_EOLFLAG` respectively, and then hold the CPU in a loop until the end-of-frame or end-of-line event takes place (which is detected by the fact that the system's `EOF` handler sets the `DISPLAY_EOFFLAG` and the `EOL` handler sets the `DISPLAY_EOLFLAG`).

`WAITEOF` (and `WAITEOL`) presume that the event of interest has not yet occurred at the time the macro is entered. This can cause problems: if your code gets behind in a given frame and reaches `WAITEOF` just after `EOF`, then `WAITEOF` would have you wait an entire frame, which is probably at odds with your designs. Fortunately, there is a way to either wait for `EOF` if it hasn't happened yet or proceed immediately if it has already occurred. This is accomplished by clearing the `DISPLAY_EOFFLAG` early in your logic and then using the `WAITNEOF` macro at the end of your logic. `WAITNEOF` is identical to `WAITEOF` except that the `DISPLAY_EOFFLAG` isn't cleared before the wait loop is entered. Therefore, if the `EOF` handler has already gotten in there and set the `DISPLAY_EOFFLAG` by time your code reaches `WAITNEOF`, your code will detect this and exit the wait loop without delay. The problem with this technique is that if your code gets way behind the display you can end up with severe wrap-around if you're not careful.

The `WAITNEOL` macro is analogous horizontally to `WAITNEOF`.

## Summary of display creation macros

Following is an alphabetic listing and summary of all the display creation macros. For the most up-to-date list of the arguments required by these macros, refer to the `6502:macros/display.mac` and `6502:macros/sprite.mac` files.

`DBUF_DISPLAY`  
After you've set everything up for double-buffering and you've rendered into the off- display buffer, this macro causes the render buffer to be the one displayed during the next display frame

`DISPBUF`  
This macro causes the buffer specified by the argument to be the one displayed during the next display frame. Doesn't perform any double-buffering functions

`DISPLAY`  
This macro is used by the display system, during the `EOF` interrupt code. The programmer shouldn't have any need for this macro. To display a single buffer, refer to the `DISPBUF` macro

`DISPLAY_ON` and `DISPLAY_OFF`  
These macros turn the display hardware on and off respectively

`FLIP`  
When the user wants the display flipped 180°, this macro does all the work for you

`HOFF8` and `VOFF8`, `HOFF16` and `VOFF16`  
These macros are used to position the display window within the display world. The values take effect the next time you render sprites

`INITEOF` and `INITEOL`  
If you are going to use the `WAITEOF` and/or `WAITEOL` functions, you must first invoke the associated `INITEOF` and/or `INITEOL` macro. These macros should be called only once

`INITSUZY`  
This macro must be called only once, before you first use Suzy's sprite engine or math hardware

`RGB16`, `RGB16_I` and `RGB_AXY`  
These macros provide several different ways to write values to the hardware color palette registers

`SETCOLL`  
If you are going to be using hardware collision detection, you must use this macro to set up the collision buffer and to define the offset to your SCB collision depository

`SETDBUF`  
You use this macro to set up for double-buffered display creation, including specifying the addresses of your buffers

`SETDISP`  
This macro sets up the display hardware timers with the values specified, and initializes certain system variables for the fun to come

`SETDISP_60`, `SETDISP_50`, `SETDISP_30`, `SETDISP_25`, `SETDISP_20`, `SETDISP_15`, `SETDISP_10`  
These macros set up for "normal" frame rates by calling the `SETDISP` macro with preset values

`SPRITES`  
This macro works within the normal double-buffering scheme to render your sprite list into the off-display buffer

`WAITEOF` and `WAITEOL`, `WAITNEOF` and `WAITNEOL`  
These macros loop until end-of-frame or end-of-line is reached

`WAITSUZY`  
This macro performs the normal `CPUSLEEP` logic to keep the CPU out of Suzy's hair while she's working on sprite rendering. The `SPRITES` and `RESPRITE` macros invoke this macro for you automatically, so `WAITSUZY` is a macro that you'll probably never invoke directly

## Example double-buffered sprite display

Here's some example code that shows the steps you must take to set up the display and system to get a double-buffering display with sprite rendering.

```
* Initialize display with frame rate of 60, EOF-capable,
* sprite-capable
EOF USER 	.EQU 1	; define to make us EOF-capable

Start
			SETDISP 30					; frame rate of 30
			INITEOF  					; set for WAITEOF
			INITSUZY					; spritely bits
			RGB16	TestPalette			; init colors
			SETCOLL CollBuffer,-1		; init collision
			SETDBUF Buffer1,Buffer2		; will double-buff
			LDA 	#0					; horiz offset to 0
			HOFF8
			LDA 	#0					¡ vert offset to 0
			VOFF8
Loop
			SPRITES	FirstSprite, 1 		; draw sprites
			DBUF_DISPLAY				; double-buff disp
			WAITEOF						; wait until switch
			DISPLAY_ON					; turn on display
			JSR 	PlayWithSprites		; do some stuff 
			BRA 	Loop				; do it again!
  
CollBuffer	.DS 	DISPLAY_BUFSIZE
			.ALIGN 4
Buffer1		.DS		DISPLAY BUFSIZE
			.ALIGN 4
Buffer2 	.DS		DISPLAY BUFSIZE
```

## Example still-picture display

Here's some example code that shows the steps you must take to get a still-picture display.

```
* Initialize display with frame rate of 60, show picture 
EOF_USER 	.EQU 1 			; define this to make us EOF-capable

Start
			SETDISP 60		; frame rate will be 60 RGB16
			TestPalette 	; init colors
			LDA # 0 		; set horiz offset to 0
			HOFF8
			LDA #0 			; set vertical offset to 0
			VOFF 8
			LDX #<Picture 	; get address of picture
			LDA #>Picture
			DISP_AX			; set address of picture
			DISPLAY_ON		; turn on the display
			BRK
			.ALIGN 4
Picture		.IN PictureData
```