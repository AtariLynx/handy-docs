HANDY Release 0.7.12

28 March 1989  
Confidential and Proprietary

This document describes the V0.7.12 release of the Handy development environment. It includes these sections:

- HandyASM V0.17
- What you must do to start using this release

## HandyAsm V0.17

*HandyAsm* now supports cross-referencing. You get it using the `-X` option. Hoo boy, does this work great. Acid-test it for us please, and report any problems.

We need some feedback. Carl and I have figured out ways to create 2 *HandyAsm* "enhancements": an option to have all references in a listing resolved (currently you get `***` for unresolved symbols); text-named local labels, allowing you to create locals with names like .loop and .loopend, which locals don't go into the global symbol table and therefore give you more readable code without slowing assemblies with a symbol table that's fatter than needed. Carl can create both of these effects, but there's a cost: each one will slow all assemblies down a teeny bit. So I need you to vote whether or not these features are desirable enough to take a teeny performance hit.

Also, if we implement the listing symbol resolution stuff, we can do it as an option. Should the default be no resolution, requiring you to turn resolution on, or should the default be resolution, requiring you to turn it off.

You might be tickled to learn that the resolution of undefined symbols in the listing will work in much the same way that symbols are resolved in the binary: during the resolution stage, the assembler will seek back into the listing file and write text where the `***` used to be.

Bug fixes:

- `.SY` and `.PD` work (poof)
- Branch range checking is fixed
- Operand errors are now detected (no longer can you specify `BRA` without a destination)

## What you must do to start using this release

If you want to assemble using the new assembler, first enter the CLI command `NewAsm`. When you want to go back to the old assembler, enter `OldAsm`.
