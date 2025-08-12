# Lynx development standards

To insure consistency and to maintain the high quality of Lynx software, the following standards must be adhered to by all developers:

1. The static boot screen of the game must contain the game title and all necessary copyright information. No generic or blank boot screens allowed. However, if your project runs out of ROM space you can save memory by decreasing the complexity of your bootscreen graphically; but the copyright information is mandatory. The phrase "Licensed to Atari Corp." must follow the copyright information on games licensed to Atari Corp.

2. The animated title screen must contain all necessary copyright information. The phrase "Licensed to Atari Corp." must follow the copyright information on games licensed to Atari Corp. Programming credits may be included as desired, but they cannot supersede copyright information.

3. The `Option 2` button should be used on the title screen to toggle game music off and on; game sounds are unaffected by `Option 2`. The default condition of the music (upon bootup or `Restart`) should be on. If the `Option 2` button is not used in the game, it should be used to toggle game music off and on during gameplay as well.

4. The `Restart` function of simultaneously pressing the `Option 1` and `Pause` buttons should reset you back to the title screen. The order in which the buttons are pressed should not matter.

5. On multi-player games, the phrase `X PLAYERS` must be displayed on the title screen where `X` equals the number of machines Comlynxed. No icons will be accepted as substitutes. The lettering should be in the range of 7x7 pixels. Do not show `1 PLAYER` nor `TOO MANY PLAYERS`.

6. The timers for activating shutoff with no user activity in 5 minutes on battery and 1 hour on AC must be installed (the `AUTO_TIMEOUT_USER` equate invokes the macros to be included during assembly).

7. When the `Pause` button is pressed, all game actions and sounds must immediately stop and the word "PAUSED" must be displayed in the center of the screen. When the button is pressed again, all game actions and sounds should immediately resume and the word "PAUSED" should be erased from the screen. The `Pause` sprite should be of such color and size that it is easily seen against all backgrounds.

8. The `A` button must be used as the primary action button; the `B` button must be used as the secondary action button. If only one action button is used in the game, allow players to use either the `A` or `B` button.

9. The 180-degree screen flip function must be installed and activated by simultaneously pressing the `Option 2` and `Pause` buttons. The order in which the buttons are pressed should not matter. If `Restart` (`Option 1` and `Pause`) is pressed while the screen is flipped, the screen should remain flipped until `Option 2` and `Pause` are pressed again. If a player flips the screen during a game, the screen should flip and then "Pause" should be activated so as to give the player time to turn the machine over.

10. `Pause`, `Restart`, and `Flip` should be allowed anytime during a game with the exception that `Pause` is not necessary on the title screen.

11. If room allows on your ROM, we highly recommend a demo mode in all games showing some game action.

12. Please ensure that any text you may display during the game can be read easily over all backgrounds. Either a contrasting color scheme or an outline around the text are recommended solutions to this problem.

13. The "End of Game" logic should work as follows. When the game ends, there will probably be a Congrats screen, or a Game Over screen if the player didn't make it to the end, or a high score screen. No matter what screen is shown, we highly recommend you construct the end of the game so that the user cannot bypass any "Congrats Text" or High Score screen accidentally. Please make the program work such that a `Restart` is required to return to the title screen from the end screen, OR a timer which ignores all input for a period of time (except timer wouldn't restrict `Restart`) so that the user does not miss any valuable information.

## Comlynx information

Use the supplied Comlynx (Redeye) code. It works very well. It is bullet proof, quick, efficient and even flexible. If for some reason you feel you must write your own, it must work exactly the same way as ours in all circumstances or it will be rejected. Here are some rules your Comlynx code must follow:

1. Allow other players to connect or disconnect at anytime during the title screen. The title screen is displayed upon power up (after the bootscreen) and also after a `Restart` during a game.

2. If a dead machine (power off) is in the Comlynx "loop", it should simply be ignored and allow the other user(s) to begin a game.

3. If during a Comlynx game the Comlynx "loop" is broken (say someone turns off during a game), then `Restart` should be allowed for the remaining machines. If the "loop" is broken by an accidental disconnect of the Comlynx cable, the game should wait until the "loop" is re-established. NOTE: Adding players can only occur on the title screen during log-on.

4. If at anytime during a game one player pauses in a multi-player game, then all machines must pause.

Revised 9/91
