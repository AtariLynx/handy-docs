# HPRINT - HANDY print routines

This chapter describes the *HPRINT* Handy print routines and supporting macros. This chapter contains the following sections:

- [Introduction and overview](#introduction-and-overview)
- [*HPRINT* fonts](#hprint-fonts)
- [Sprite data buffer](#sprite-data-buffer)
- [ASCII `.CHARSET` remapping](#ascii-charset-remapping)
- [`HPRINT`](#hprint)
- [Summary of *HPRINT* macros](#summary-of-hprint-macros)
- [Example `HPRINT` usage](#example-hprint-usage)
- [Plea for feedback](#plea-for-feedback)

## Introduction and overview

The *HPRINT* text routines translate text strings into imagery data. You can then display the text imagery as a normal Handy sprite.

The *HPRINT* fonts are one-color, 8 pixels wide, any height. Fonts are subdivided into character groups: upper-case, lower-case, digits, simple punctuation and extended punctuation. You can elect to have any combination of these character groups as your actual font. The current system provides one font, but others may soon be added and you are free to create your own. Rules for including and creating fonts can be found in the [*HPRINT* Fonts](#hprint-fonts) section below.

The data buffer rendered into by the `HPRINT` routine is a rectangular completely-literal sprite image data block. Rules for the declaration, initialization and usage of the data buffer can be found in the [Sprite data buffer](#sprite-data-buffer) section of this chapter. This data buffer is pointed to by a normal Handy sprite control block. After you use `HPRINT` to render text into the data buffer, you can display the text as any type of Handy sprite, including using stretch, tilt (for italics), scaling, exclusive-or or anything else. The example *HPRINT* test program (`6502:examples/testhprint.src`) shows how two sprite control blocks can point to the same text imagery, with one remapping the colors differently from the other, creating a pleasant drop-shadow effect.

In order to pack the font characters down to the minimum required space, the *HPRINT* routines use the assembler's `.CHARSET` capability to redefine ASCII characters from normal ASCII to the lowest possible values, which allows a direct correspondence between the ordinal value of a character and the position of its data in the font table. This results in smaller code, smaller data and faster execution, but can cause problems when dealing with individual ASCII characters. See the section [ASCII .CHARSET remapping](#ascii-charset-remapping) below for details.

The *HPRINT* code is kept simple intentionally to prevent it from becoming too large. For example, for a cost of 541 bytes, here's what you can get:

- the *HPRINT* source code
- a font with upper-case alphabet characters, digits and simple punctuation
- a sprite control block to display the text
- a buffer large enough to hold 20 characters, each 7 lines tall

## *HPRINT* Fonts

- upper-case characters (defined as `HPR_UPPER`)
- lower-case characters (defined as `HPR_LOWER`)
- digits (defined as `HPR_DIGIT`)
- simple punctuation (defined as `HPR_PUNC`)
- extended punctuation and other (defined as `HPR_OTHER`)

You must specify the font subgroups you desire by setting the `FONT_TYPE` constant equal to a combination of the font subgroup names. For example, to get the standard short font you 

```
FONT_TYPE 	.EQU HPR_UPPER+HPR_DIGIT+HPR_PUNC
```

Remember that the ASCII characters in your code are actually translated to alternate values via the assembler's `.CHARSET` capability. This allows the fonts to be packed together as tightly as possible. See the section [ASCII `.CHARSET` remapping](#ascii-charset-remapping) below for details about working with ASCII this way.

Whatever subgroups you request, you always get an additional 2 characters: the `SPACE` character and the `NIX` character. `SPACE` is space as in the gap between two words. `NIX` is the character that is printed when you've accidentally specified the ASCII for a character that isn't among the subgroups of your font.

Each character of the font is 8 bits wide. You must specify your desired font height by declaring the constant `FONT_HEIGHT`. For example, to specify that you want a font height of 7 (the standard font), you use:

```
FONT_HEIGHT 	.EQU 7
```

Because each line of a character is a byte wide, there are `FONT_HEIGHT` number of bytes per each font character. If you request upper-case characters, digits and simple punctuation you'll get `(26 + 10 + 11 + 1)` `48` characters, which, if you've selected the standard `FONT_HEIGHT` of 7, will generate 336 bytes of character data.

You include an *HPRINT* font in your code by using the `HPRFONT` macro. The macro accepts a single argument: the name of the font you want to include. At this writing, there is only one font you can include: `plain`. The macro defines the memory reference `FontBase` and includes the correct components of your desired font.

Currently, only one *HPRINT* font can be included in any given program. This is done for memory savings. However, the *HPRINT* routines could easily be expanded to support multiple fonts.

If you want to define your own font, here's the rules:

- You need one of each of these font files (where "name" stands for the name of your font and the `#` stands for the font height):
  - `name_space#.src`
  - `name_digit#.src`
  - `name_upper#.src`
  - `name_lower#.src`
  - `name_punc#.src`
  - `name_other#.src`
  - `name_nix#.src`
- In each of the font files, you must declare the characters upside down, one byte per line, and the same number of lines for each character

## Sprite data buffer

You can create as many *HPRINT* data buffers as you like.

The *HPRINT* data buffer must be rectangular completely-literal sprite image data block. This data block is rendered into by `HPRINT`, and then displayed by having the hardware process a normal sprite control block that points to this data.

Each line of a completely-literal data block starts with an offset (`n`) to the start of the next line, followed by `n-1` bytes for sprite imagery data. After the last line of the data block you need one more byte, with value zero, to specify "end of imagery data." The data buffer can be initialized to look like this sort of data block using the *HPRINT* macro `INITLIT`, which stuffs the appropriate offset values into the buffer.

Note that the buffer needs one more byte per line than the number of characters than can be printed per line.

To correctly allocate an *HPRINT* buffer, you should reserve space using something like this:

```
MAX_CHARS 	.EQU 20
BUF_WIDTH 	.EQU MAX_CHARS+1
TextBuffer 	.DS { BUF_WIDTH*FONT_HEIGHT } + 1
...
			INITLIT TextBuffer, BUF_WIDTH
```

## ASCII .CHARSET remapping

In order to pack all the characters of a font as tightly together as possible, the *HPRINT* routines take advantage of the assembler's ability to remap ASCII characters by invoking the assembler's `.CHARSET` directive. The `SPACE` character (normally ASCII `$20`) is always mapped to value `$01`, and the font data for `SPACE` is always the first in the array. If you've asked for digits (normally ASCII `$30` through `$39`), these get translated to values `$02` through `$0B` and become the next 10 characters in the font array. Then, when you declare ASCII data the assembler uses the `.CHARSET` translation table and generates remapped data. The *HPRINT* routine then uses these translated data values as absolute indices, thereby resulting in RAM and execution time savings.

You get *HPRINT* `.CHARSET` remapping by using the `HPR_CHARSET` macro. Once you've used this macro, all the `.CHARSET` remapping is done automatically for you. Note that before you invoke the `HPR_CHARSET` macro you don't get these translations, so it's important to use the macro early in your source code.

If you don't want this ASCII translation stuff, don't invoke `HPR_CHARSET`. However, you'll need to provide some sort of ASCII to *HPRINT* `.CHARSET` translation routine for your buffers before invoking `HPRINT`. The desire to avoid incurring the size of this code (or a big translation table) was the motivation for using the `.CHARSET` technique in the first place.

Refer to the assembler manual for more information about the `.CHARSET` stuff.

## `HPRINT`

And then, finally, there's the `HPRINT` macro.

`HPRINT` renders the imagery of your text into your buffer. You supply to the macro the address of a null-terminated text string and the address of your sprite data buffer.

If you specify fewer characters than fit in the buffer, `HPRINT` will fill to the end of the buffer with blanks.

No error checking is done, so if you overwrite the end of your buffer then you get some kind of mean garbage.

## Summary of *HPRINT* macros

`INITLIT BufferAddress, LineByteCount`  
This macro initializes a buffer to look like a completely-literal sprite data block. Note that the buffer is expected to be `LineByteCount"FONT_HEIGHT+1` bytes long.

`HPR_CHARSET`  
This macro includes a `.CHARSET` file according to your `FONT_TYPE` specifications.

`HPRFONT FontName`  
This macro includes the elements of the named font according to your `FONT_TYPE` specifications. For instance, if you have set `FONT_TYPE` equal to `HPR_UPPER` and `HPR_DIGIT` then `HPRFONT` plain will cause the `plain_upper` and `plain_digit` characters to be included.

`HPRINT TextAddress, TextAddressIndirection, BufferAddress`  
This is the macro that actually renders the text into the buffer. `TextAddressIndirection` should be zero if `TextAddress` points to a text string, one if `TextAddress` points to a variable that points to a text string. The text string should be null-terminated. `BufferAddress` should point to the completely-literal data buffer that will receive the imagery.

## Example `HPRINT` usage

Coming soon. For now, refer to `6502:examples/testhprint.src`

Also, here's a checklist:

- include `6502:include/hprint.i`
- include `6502:macros/hprint.mac`
- include `6502:src/hprint.src`
- define `FONT_TYPE` with some combination of `HPR_UPPER`/`LOWER`/`DIGIT`/`PUNC`/`OTHER`
- define `FONT_HEIGHT` (standard is `FONT_HEIGHT .EQU 7`)
- include a font using the `HPRFONT` macro (standard is `HPRFONT plain`)
- create a completely-literal data buffer, or just allocate enough memory for one and initialize the buffer using `INITLIT`
- invoke the `HPR_CHARSET` macro somewhere near the beginning of your source code
- use `HPRINT` to create text imagery in your buffer

## Plea for feedback

I made some arbitrary decisions. If you would rather have things work differently, let me know. For instance:

- `HPRINT` doesn't stop you from overwriting the end of the buffer. To do so would cost 3 bytes. Should I put this in?
- `HPRINT` always fills with blanks to the end of the buffer. Should this be a switch (like carry set or clear?)
- `HPRINT` always starts printing from the beginning of the buffer. Would you rather be able to specify the starting position?
