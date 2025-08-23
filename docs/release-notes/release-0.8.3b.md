# HANDY Release 0.8.3b

31 May 1989  
Confidential and Proprietary

This document describes the V0.8.3b enhancement release of the Handy development environment.

There's a new `display.mac` with better `WAITEOF` / `WAITEOL` macros.

Everyone must use the `INITINT` macro now. Usage of the `INITINT` macro is described in the [Other `6502:` Changes](./release-0.8.md#other-6502-changes) section, page 2 of [Handy Release 0.8](./release-0.8.md). Also, to correctly debug in conjunction with `INITINT`, you must define the constant `BRK_USER`. When `BRK_USER` is defined, you are declaring that you want the system to process `BRK` opcodes.

Until we move to the Howard board, you must declare `BRK_USER` or else you won't be able to set breakpoints. After we move to Howard, the only reason to use `BRK` instruction will be to implement a fast "go subroutine" technique or something like that.

You might want to copy the `BRK_USER` declaration and comment that's being distributed in the `6502:examples/testsprite.src` file. The reason for copying the comment is to remind yourself later that `BRK_USER` isn't needed once we're Howarded.

If `BRK_USER` isn't defined, the system ignores `BRK` opcodes which results in smaller code and faster interrupt handling. So if you're not doing something weird with `BRK`, you should not define `BRK_USER` when generating "release" code (such as the CES code).

For completeness, `BRK_USER`'s can use the macro `SET_BRK` to set the `BRK` vector to point to specific code.
