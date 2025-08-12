# The two scaling algorithms

This section describes the two Handy scaling algorithms.

With both algorithms, the horizontal and vertical scale values are independent.

## Algorithm 3 - Shifter

This algorithm loads the 16-bit `HSize` and `VSize` values into a shift register and then tests the bits of the shift register sequentially to decide whether or not to use a particular pixel of source data when creating the sprite imagery. Wherever a bit is set, a corresponding pixel will be selected.

For example, if the SCB `HSize` field of a sprite is initialized to `%1010101010101010` then the first pixel would be drawn, the second pixel skipped, third drawn, fourth skipped, et cetera. As only half the pixels would be drawn, the image would be half-sized. Note that if your source imagery is greater than 16 pixel wide, the shift register is reloaded for the 17th pixel (likewise the 33rd and so on).

Likewise, whether or not a line of source data will be processed or skipped entirely is decided using the sprite's `VSize` value.

Note that this algorithm allows imagery to scale between 0 and full-sized only, as contrasted with Algorithm 4 which allows imagery to scale from 0 to 256 times full-size.

*Handycraft* allows you to edit a Scale 3 table and see the scaling imagery that results from your table. When you're satisfied with the results, you can write out the table and assemble it into your 6502 program, the entries of which your program must feed into your sprite's `HSize` and `VSize` fields.

## Algorithm 4 - Adder

When you select Algorithm 4, your imagery is scaled according to the `HSize` and `VSize` fields of the sprite control block where the fields are 16 bit fixed-point values where the lower 8 bits are fraction and the upper 8 are the integer part.

For example, an `HSize` value of `$0100` means that you want your sprite scaled full-size. A value of `$0080` means you want you sprite half-sized, and `$0200` means double-sized.
