# lavesiime.github.io
***
# Adding Spring Recurl to Sonic 1 (2013)

This is a small guide on how to add Spring Recurl to Sonic 1, inspired by the SCHG. This is going to be based of the decompiled scripts by Rubberduckycooly, which also means that the RSDKv4 decompilation is needed and this cannot be run on the original release. Without further ado, let's get started!

## What you'll need

* Sonic the Hedgehog 1 (2013)
* [RSDKv4 decompilation](https://github.com/Rubberduckycooly/Sonic-1-2-2013-Decompilation)
* [Decompiled Sonic 1 scripts](https://github.com/Rubberduckycooly/Sonic-1-Sonic-2-2013-Script-Decompilation)
* A text editor
* A palette-respecting image editor

For the latter two software, what I'll be using are:
* Visual Studio Code
* GraphicsGale

However, keep in mind that these aren't the only programs that can be used; there are many other options such as Notepad++ and GIMP that can be used, instead.

## The Steps

Alright, now here's the fun stuff!

First, search `PlayerObject.txt` for the function `PlayerObject_HandleAir`. As the name implies, it manages the player while they're in the air, regardless of whether they're rolling or not. Inside of it, look for the part that checks if the player's current animation is `ANI_BOUNCING`. `ANI_BOUNCING` is the animation used after bouncing on a spring, so the player object checks against it to see when it should go back to the player's normal animations. Since we want this move to only be triggerable when the player is rising from a spring bounce, we can simply add a `else` to the `if player.yvel >= 0` check already present.

Now, if we want to make the move triggered by pressing the jump button, then how would we do that? All the player inputs are stored as variables that are set to `true` when pressed, so we can add a `if player.jumpPress == true` condition inside of this `else` section.


