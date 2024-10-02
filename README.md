# Mindustry Modding Guide
Note that a lot of information is from mindustrygame.github.io/wiki

## Modding Introduction
(from wiki, edited) Mindustry mods are simply directories of assets. There are many ways to use the modding API, depending on exactly what you want to do, and how far you're willing to go to do it.

You could just resprite existing game content, you can create new game content with the simpler Json API (which is the main focus of this documentation), you can add custom sounds (or reuse existing ones). It's possible to add maps to campaign mode, and add scripts to program special behavior into your mod, like custom effects.

Sharing your mod is as simple as giving someone your project directory; mods are also cross platform to any platform that supports them. You'll want to use GitHub (or a similar service) for hosting your source code. To make mods all you really need is any computer with a text editor.

## Directory Structure
Your project directory should look something like this:

mod_folder<br>
├── mod.hjson<br>
├── content<br>
│   ├── items<br>
│   ├── blocks<br>
│   ├── liquids<br>
│   └── units<br>
├── maps<br>
├── bundles<br>
├── sounds<br>
├── schematics<br>
├── scripts<br>
├── sprites-override<br>
└── sprites<br>

mod.hjson (required) metadata file for your mod which contains your mod name, identifier(prefix the game loads your content with), description, mod icon for the mod browser, whether or not your mod is in Java, and GitHub Repository location,

content/* directories for game Content,
maps/ directory for in-game maps, example: Ground Zero,
bundles/ directory for Bundles, these are used for adding a language,
sounds/ directory for Sound files, as .wav,
schematics/ directory for Schematic files, these are given to the player when they load the mod, can be used as examples or just in a schematic pack,
scripts/ directory for Scripts, this is only used if you are using JavaScript,
sprites-override/ Sprites directory for overriding ingame content, used only to override in game sprites for texture packs or such,
sprites/ Sprites directory for your content, sprites for your custom blocks, items, and liquids(icon)

## HJSON
(from wiki) Mindustry uses Hjson, which for anyone who knows Json, is simply a superset of the very popular serialization language known as Json. – This means that any valid Json will work, but you get extra useful stuff:

\# single line comment

// single line comment

/* multiline
comment */

key1: single line string

key2:
'''
multiline
string
'''

array: [ value 1
        value 2
        value 3 ]

dictionary: { key1: string
        key2: 0 }
If you don't know any of those words. – A serialization language, is simply a language which encodes information for a program, and encode means to translate informantion from one form to another, and in this case, to translate text into Java data structures.

## mod.hjson
(from wiki, edited) At the root of your project directory, you must have a mod.json which defines the basic metadata for your project. This file can also be (optionally) named mod.hjson to potentially help your text editor pick better syntax highlighting.

name: "mod-name"
displayName: "This isn't a mod."
author: Yourself
description: "Your description"
version: "1.0" // This is the version of your mod, doesn't actually matter except for when the game checks if any installed mods need updates
minGameVersion: "146" // The game version needed to launch the mod, v146 is the newest
dependencies: [ ] // The IDs of any mods your mod is dependent on
hidden: false // Whether or not this mod can be used in multiplayer
scripts: false // Whether or not your mod uses JavaScript

## Content
(from wiki, edited) At the root of your project directory you can have a content/ directory, this is where all the Json/Hjson data goes. Inside of content/ you have subdirectories for the various kinds of content, these are the current common ones:

content/items/ for items, like copper and surge-alloy;
content/blocks/ for blocks, like turrets and floors;
content/liquids/ for liquids, like water and slag;
content/units/ for flying or ground units, like eclipse and dagger;
Note that each one of these subdirectories needs a specific content type. The filenames of these files is important, because the stem name of your path (filename without the extension) is used to reference it.

Furthermore the files within these content/<content-type>/* directories may be arbitrarly nested into other sub-directories of any name, to help you organize them further, for example:

content/items/metals/iron.hjson, which would respectively create an item named iron.
The content of these files will tend to look something like this: <br>
```
type: TypeOfThing // This is the name of the type you are using, these are found on the wiki. Example: GenericCrafter, uses inputs to create outputs
name: Name Of Thing // This is what is displayed in game, can be omitted and defined in a bundle instead
description: Description of thing.
flammability: 1 // A decimal/float that determines how many flames appear when it is destroyed on a conveyor or in a block
radioactivity: 1 // A decimal/float that determines how much power is generated by an RTG generator
charge: 1 // A decimal/float that determines how much damage is caused by lightning when destroyed
cost: 2 // When used as building material, this number adds 2 frames of build time per unit of this item used
healthScaling: // (wiki)When this item is present in the build cost, a blocks default health is multiplied by 1 + scaling, where 'scaling' is summed together for all item requirement types.
lowPriority: false // If true then when a drill is placed on this items ore it will always prioritize another ore if the drill has another one, used for sand
frames: 0 // If this value  is larger than 0 then it is animated, see the animation section of the wiki for more info
transitionFrames: 0 // The amount of frames the game generates to change between the frames declared above
frameTime: 1 // The amount of in game frames that will pass before this item changes to the next frame
buildable: true // (wiki)If true, this material is used by buildings. If false, this material will be incinerated in certain cores.
hidden: false // Whether or not the player can see this item in menus
```






consumes: {
  /* This is the amount of power it consumes **per frame**, meaning you multiply this value by 60 to get the power per second usage. Or take the power per second consumption you want and divide it by 60 to get the amount you put there \*/
  power: 0.5, // Consumes 30 power per second
  liquids: {
    liquids: [
      /* This is the amount of liquid it consumes **per frame**, meaning you multiply this value by 60 to get the liquid per second usage. Or take the liquid per second consumption you want and            divide it by 60 to get the amount you put there */
      water/1 // consumes 60 water per second<br>
    \]
  },
  items:{
    items: [
      copper/2 // Consumes two copper per craft<br>
    \]
  }
