---
layout: default
title: Porting
nav_order: 8
---

1. TOC
{:toc}

# Porting

Porting a map means you don't have to make the geometry yourself. On a less-comical note, porting allows you to re-experience maps from other games in the Godot engine. Qodot was made for porting Quake 1 maps, but it also is capable of porting:

- Standard
- Valve
- Quake
- Quake 2
- Quake 3
- Quake 3 (Legacy)
- Hexen 2
- Daikatana

Qodot supports the full range of Quake-derived texture formats.

## Prerequisites

Before starting to import a map into Qodot, it's important to check if you have the following:  
- A Trenchbroom .cfg that supports your specific game.
- A .map that doesn't crash Trenchbroom when saving or loading, using your game's .cfg.  
- Textures as .wad or image files.  
- All .fgd files used to define the map's entities.

### How to get around missing prerequisites

If you don't have a game config for your ported game, even after searching community forums, try loading it into other game definitions. You can read the [Trenchbroom Manual](https://trenchbroom.github.io/manual/latest/#game_configuration_file_syntax) for more info on creating a .cfg file from scratch.

If your map cannot be loaded or saved properly, you may need to try other game definitions in Trenchbroom. It can help to compare the .map file structure to other .map files which can load correctly in Trenchbroom.

If your map is missing .fgd files, you will be missing out on the map's entities. You can build the map with Qodot anyways, and any issues with missing/wireframe geometry can be solved by deleting any `undefined` entities in the .map.

If you don't have the map's textures, you can re-texture the map geometry with .png and .jpg/.jpeg files as shown in [Texturing your map](#texturing-your-map).

# Scaling a Map

The Inverse Scale Factor setting on a QodotMap node determines the ratio between Quake units (used in TrenchBroom and other editors) to Godot's metric coordinate system. All Quake coordinates are divided by this value to create Godot coordinates during build.

Having a well-defined ratio from Quake units to the ones used by your game is important - it can make it easier to reason about the layout of your maps, and having your object scale translate properly to the equivalent real-world measurement will result in more accurate physics simulation.

As the metric used by map files varies game-by-game, this setting is dependent on your assets, game logic and physics simulation, and will have to be decided on a case-by-case basis. The table below lists some common examples, as well as reasoning for their usage.

|              Name | Inverse Scale Factor |     1qu |    2qu |    4qu |    8qu |   16qu | Notes |
| ----------------: | :------------------- | :------ | :----- | :----- | :----- | :----- | :---- |
|  .map Passthrough |                    1 |       1 |      2 |       4|      8 |     16 | 1:1 with map file, Godot grid corresponds to TrenchBroom grid. Will result in very large geometry by Godot standards. |
|     Qodot Default |                   16 |  0.0625 |  0.125 |   0.25 |    0.5 |    1.0 | 'Best effort' mapping from Quake 1 environments to metric. |
|          Uradamus |                   40 |   0.025 |   0.05 |    0.1 |    0.2 |    0.4 | Artist-friendly setting with tidy fractional numbers. |
| Valve Environment |       52.49343832021 | 0.01905 | 0.0381 | 0.0762 | 0.1524 | 0.3048 | Half Life 1/2 environment metric. |
