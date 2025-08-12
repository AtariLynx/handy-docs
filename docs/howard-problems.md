# Howard problems

People seem to be having troubles with their Howard boards. If you are also, here are some solutions to common problems:

`BRK_USER` should not be used with the Howard board. It was designed to be used with the Pinky/Mandy setup. Since that was all that we had at the time we developed the first games, it never was properly documented. Sorry.

The Howard board has its own special break command (`$13`) to break the system. That way you can use break as an instruction if you want. The `$13` maps to a nop on the 65C02.

*Handebug* uses a custom configuration for the parallel port. It is not compatible with printers. So if you want to print, I suggest going out of the serial port. It is the most convenient way, and since the Amiga is multi-tasking, you don't have to worry about print times getting any longer. Just go ahead and edit or whatever while your code is printing.

Until a future rev of the Howard board, at `$FFF9` the following should be typed from the *Handebug* monitor (in data display mode):

```
FFF9	08 00 30
```

* The Map Control Register

```
		.OR	$FFF9
MAPCTL	.DS 1	; FFF9
```

* These are the `MAPCTL` flag definitions

```
TURBO_DISABLE	.EQ %10000000
VECTOR_SPACE	.EQ %00001000
ROM_SPACE		.EQ %00000100
MIKEY_SPACE		.EQ %00000010
SUZY_SPACE		.EQ %00000001
```

`00 30` is the NMI vector.

This should not be done on Pinky/Mandy systems, since they have their own monitor which you bootload into program memory.

Don't use bootload on the Howard board. It has its own monitor in ROM which is transparent to the user.

Many people have asked about the mysterious lights and extra switch on the front of the machine. Well, here's what they do:

- The green light is just a power light.
- The amber light is Lynx power (`SYSCTL1`,`$fd87`); "on" means power is on.
- The red light turns on whenever the cart is being accessed.
- The extra switch is for power state. Left means external power. Right means battery power. This was set up so you can test Auto Timeout. (Just watch the amber light.)

If you have trouble running code after downloading, try hitting NMI. It seems to work. Also it's a good idea to clear all pending interrupts before you clear the interrupt flag.

By the way, there is no such thing as a "quebert root". It was just a joke. Dave Needle thought it was funny, but some programmers have taken it seriously.

We have tested every Howard board before we've sent it out, however, there is a chance that we missed something. Before you send one back to us, though, I suggest you give us a call. [Also, **before returning any equipment, you must obtain shipping info and approval from Juli Wade** - (408) 745-8850. Otherwise, we cannot guarantee that your Howard board will be received/repaired.]

Please take advantage of the Lynx BBS - instructions follow.

BBS:

1. Call (408) 745-2157 during West Coast business hours.
2. Follow prompts to register.
3. Wait one hour (have a sandwich ... ).
4. Call back and you're in the Club if you are a registered developer.
5. Don't forget to `.ARC` or `.LZH` your code before uploading.

Enjoy!

Revised 9/91
