# 6502 Cross assembler notes

This section discusses the particulars of writing Handy 6502 assembly code.

## Things to Include

## Things You Must Define

If you're going to use Handy sprites or the Handycraft sprite emulator, you must define the type of IO you have. If you define `HANDYIO` then the files are configured for you to talk through Handy hardware. If you define `AIO2` then the files reflect communications through an Apple AIO2 parallel card. Likewise, an IO32 definitions will set up the files to talk through an Apple IO32 parallel card.
