# lavesiime.github.io
***
# RSDK V4 XML Guide

This is a small guide on how to use the XML modding API in the Retro Engine V4 decomp. Not much else to say, so let's get started!

## Basic Formatting

The basic formatting of an XML file starts with the XML version at the top. For this tutorial, all that's needed is `<?xml version="1.0"?>`. Then, the first "class" starts. For RSDK, the only class used is `game`. In this `game` class, all the other sub-classes are contained inside of it. These sub-classes are what actually do stuff, so let's talk about them, starting with...


## Global Variables

Global variables are variables that are, well, global and can be changed by any script. Unlike object values or static values, these are kept the same between scene reloads. They are inserted into the `<variables>` class. The format they follow is

```xml
<variables>
    <variable name="[variable name]" value="[default value]"> </variable>
    [...]
</variables>
```

Of course, `[variable name]` is replaced with your actual variable name, and `[default value]` is replaced with what you want the default value to be. More variable entries can be entered until a satisfactory amount is reached. An example would be...

```xml
    <variable name="options.remixedMusic" value="1"> </variable>
    <variable name="options.customPalettes" value="1"> </variable>
```

In this example, two global variables are created; `options.remixedMusic` and `options.customPalettes`, both with default values of `1`. Now, these variables can be accessed from scripts just like any other global variable.


## Palettes

Although it may be a bit tedious, the XML support also allows for changing the global palette. I say that because, instead of having you input an already packed color for each index, you have to provide the R, G, and B values separately. However, it isn't my place to complain about it, so let's get on to the useful info. The format allows all 256 colors across the 8 palettes to be replaced, the format it follows is

```xml
<palette>
    <color bank="[pal ID]" index="[index]" r="[red]" g="[green]" b="[blue]"> </color>
    [...]
</palette>
```

For example, the below would change their corresponding colors to the colors they should have
- index `64` would be `0x808080`, or `128`, `128`, `128`
- index `65` would be `0x8F8F8F`, or `143`, `143`, `143`
- index `66` would be `0x747474`, or `116`, `116`, `116`

```xml
<palette>
    <color bank="0" index="64" r="128" g="128" b="128"> </color>
    <color bank="0" index="65" r="143" g="143" b="143"> </color>
    <color bank="0" index="66" r="116" g="116" b="116"> </color>
</palette>
```


## Objects

The objects section of the XML file deals with adding global objects for the game to use. The format is as follows

```xml
<objects>
    <object name="[object name]" script="[script path]" forceLoad="[true/false]"> </object>
    [...]
</objects>
```

With this template, there are a few things you need to change
- `[object name]` should be replaced with the object's name (for use with `TypeName[]`)
- `[script path]` should be replaced with the object's script path, based on `Scripts/`
- `forceLoad` is set to either `true` or `false` depending on what you want
  - `false` appends it to the existing global object list, offseting all further object counts by 1
  - `true` will place it at the end of the stage object list, leaving all existing object types intact

For example...

```xml
    <object name="Coin" script="Global/Coin.txt" forceLoad="false"> </object>
    <object name="Shoe" script="Global/Shoe.txt" forceLoad="true"> </object>
```

This would add two objects to the object list.
1. `Coin` would get added to the _global_ object list, with it offsetting all stage objects by 1. Its script path would be `Scripts/Global/Coin.txt`
2. `Shoe` would get added to the end of the _stage_ object list, leaving all preceeding objects alone. Its script path would be `Scripts/Global/Shoe.txt`

## Sound effects

This section adds global SFX. Because they're global, they'll offset all SFX ID's by 1, but this can be avoided by using `TypeName[]`. By default, the decompiled scripts alreay use `TypeName[]` so this shouldn't be too much of an issue. The format used follows

```xml
<sounds>
    <soundfx name="[sfx name]" path="[sfx path]"> </soundfx>
    [...]
</sounds>
```

An example of this is...

```xml
    <soundfx name="Shoot" path="Global/Shoot.wav"> </soundfx>
    <soundfx name="Charged Shot" path="Global/ChargedShot.wav"> </soundfx>
```

This would add SFX `Shoot` and `Charged Shot` to the global SFX list, with paths of `Data/SoundFX/Global/Shoot.wav` and `Data/SoundFX/Global/ChargedShot.wav`, respectively.

## Players

Adding players is probably the easist part of the XML process, as it only involves

```xml
<players>
    <player name="[player name]"> </player>
    [...]
</players>
```

Every entry in this `players` class adds one entry to the selectable players on the dev menu. Of course, it is up to the scripts to proper implement this. Without script modifications, these extra character slots <!-- shudders --> will only appear as Knuckles & Tails in-game. An example of it can be seen below

<!-- I wanted to put Mario and Luigi or something like that but it was a bit too blunt so I went with this instead 
     I wonder who the Sulky equivalent is here -->

```xml
<players>
    <player name="NASH"> </player>
    <player name="RINGO"> </player>
</players>
```

This would add players `NASH` and `RINGO` to the character selection screen, normally taking up `stage.playerListPos` values `4` and `5`, by default.

## Stages

The format for stages is perhaps the most complex out of the bunch, but not because it's difficult. Rather, it just needs the most values assigned to it in order to be setup properly. The format follows

```xml
<[stagelist]>
    <stage name="[stage title]" folder="[stage folder]" id="[stage ID]" highlight="[true/false]"> </stage>
</[stagelist]>
```

- `[stagelist]` is the list for the stage to appear on
  - `presentationStages`
  - `regularStages`
  - `bonusStages`
  - `specialStages`
- `[stage title]` is the stage's title to use on the dev menu
- `[stage folder]` is the folder the stage is contained in
- `[stage ID]` is the ID/Act of the stage
- The entry gets highlighted if `highlight` is set to `true`

An example would be the following.

```xml
<bonusStages>
    <stage name="LEVEL EDITOR" folder="LevED" id="1" highlight="true"> </stage>
</bonusStages>
```

This would add stage `LEVEL EDITOR` to the `bonusStages` list, using act `1` of the stage and highlighting it because `highlight` is set to true.


## Big example

Now, let's take everything we've learned and make one big game.xml file.

<!-- Yeah I kinda just put gibberish in these examples, I wouldn't think of anything better so this'll have to do -->

```xml
<?xml version="1.0"?>
<game>
    <variables>
        <variable name="ANI_STAND_N" value="0"> </variable>
        <variable name="ANI_STAND_T" value="1"> </variable>
        <variable name="ANI_THINK_N" value="2"> </variable>
        <variable name="ANI_THINK_T" value="3"> </variable>
    </variables>

    <palette>
        <color bank="0" index="0" r="0" g="115" b="123"> </color>
        <color bank="0" index="1" r="41" g="33" b="0"> </color>
        <color bank="0" index="2" r="74" g="57" b="16"> </color>
        <color bank="0" index="3" r="99" g="74" b="16"> </color>
        <color bank="0" index="4" r="123" g="90" b="16"> </color>
        <color bank="0" index="5" r="156" g="123" b="16"> </color>
        <color bank="0" index="6" r="198" g="156" b="16"> </color>
        <color bank="0" index="7" r="231" g="189" b="24"> </color>
        <color bank="0" index="8" r="247" g="231" b="165"> </color>
        <color bank="0" index="9" r="123" g="107" b="82"> </color>
        <color bank="0" index="10" r="156" g="140" b="115"> </color>
        <color bank="0" index="11" r="181" g="173" b="156"> </color>
        <color bank="0" index="12" r="206" g="198" b="181"> </color>
        <color bank="0" index="13" r="231" g="222" b="206"> </color>
        <color bank="0" index="14" r="247" g="247" b="239"> </color>
        <color bank="0" index="15" r="132" g="66" b="24"> </color>
    </palette>

    <objects>
        <object name="Character 1" script="Global/Char1.txt" forceLoad="false"> </object>
        <object name="Character 2" script="Global/Char2.txt" forceLoad="false"> </object>
        <object name="Character 3" script="Global/Char3.txt" forceLoad="false"> </object>
        <object name="Camera Control" script="Global/CControl.txt" forceLoad="false"> </object>
    </objects>

    <sounds>
        <soundfx name="Turtle" path="Global/TurtleCol.wav"> </soundfx>
        <soundfx name="Matte" path="Global/Fold.wav"> </soundfx>
        <soundfx name="Cure" path="Global/Got.wav"> </soundfx>
        <soundfx name="Car" path="Global/Hang.wav"> </soundfx>
    </sounds>

    <players>
        <player name="VIS"> </player>
        <player name="KEI"> </player>
    </players>

    <presentationStages>
        <stage name="TITLE SCREEN" folder="title" id="1" highlight="true"> </stage>
    </presentationStages>

    <regularStages>
        <stage name="LAKE ZONE 1" folder="Lake" id="1" highlight="true"> </stage>
        <stage name="2" folder="Lake" id="2" highlight="false"> </stage>
        <stage name="LOBBY ZONE" folder="THLBYNo2" id="2" highlight="true"> </stage>
    </regularStages>
</game>
```

This (rather large) example will do a bunch of things. First, it will create the global variables `ANI_STAND_N`, `ANI_STAND_T`, `ANI_THINK_N`, and `ANI_THINK_T`. They will have default values going from `0` to `3`, in the order they appear in. Then, the global palette will be set to the colors set in the XML. Not much else that I can say here, so let's move on. Next, 4 global objects will be made; `Character 1`, `Character 2`, `Character 3`, and `Camera Control`. Their script paths will be `Global/Char1.txt`, `Global/Char2.txt`, `Global/Char3.txt`, and `Global/CControl.txt`. They will all be added to the global objects list, rather than the end of the stage objects list. After that, 4 global SFX will be made: `Turtle`, `Matte`, `Cure`, and `Car`. Their paths will be `Global/TurtleCol.wav`, `Global/Fold.wav`, `Global/Got.wav`, and `Global/Hang.wav`, respectively. Next, 2 players will be added to the list, those being `VIS` and `KEI`. Finally, stages get added to the list. The first one is `TITLE SCREEN` on the `presentationStages` list, with it using act `1` of folder `title`, and being able to get highlighted on the dev menu. Next, `regularStages` are added. In order, `LAKE ZONE 1`, (LAKE ZONE) `2`, and `LOBBY ZONE` are added. The former two pull from folder `Lake` while the last one pulls from THLBYNo2. The acts they use are `1`, `2`, and `1`, in that order. Only `LAKE ZONE 2` doesn't get highlighted on the menu, because it doesn't have anything to highlight. All other entries get highlighted.


## Conclusion

Well, that should just about be everything. This new system allows easy-to-read XML files to be used instead of byte-based `GameConfig.bin`'s, and that's pretty good, especially for modding. As I've stated throughout this tutorial, all this _adds_ onto the things existing in the `GameConfig`, so a complete replacement can only happen if you dummy out the Game Config file. Other than that, though, have fun!
