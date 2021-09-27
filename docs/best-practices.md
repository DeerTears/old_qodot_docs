---
layout: default
title: Best Practices
nav_order: 9
---

1. TOC
{:toc}

# Best Practices

This document covers several topics including how to:
- Organize your project folders
- Get the most performance out of Qodot
- Get the best quality out of Godot using Qodot
- Avoid development disasters and pitfalls when using Qodot

These are all subject to change over time as we all get more experience using Qodot.

# Tips

## Don't add to Addons
Don’t add files to the `/addons` folder yourself. Your original work can be erased if Godot auto-updates Qodot, or if you ignore the `/addons` folder in a Git repository to save on space.

Instead, create a new folder on `res://` for whatever you’re adding.

## Separate your world textures
When you set your project directory in Trenchbroom, it looks for an immediate subfolder called `/textures`. If you just want textures in the game, this is fine:
- Game Path: `C:/Users/Ember/Gamedev/MyQodotProject/`
- Automatically-detected textures path: `C:/Users/Ember/Gamedev/MyQodotProject/textures/`

But this goes against a certain philosophy of organizing files. You can always just search for your textures by looking for "png" in the FileSystem, it's more useful to organize assets by **context** rather than existing metadata that's already inherent to the file.

So there should actually be no one-size-fits-all `/textures` folder. Instead, Trenchbroom just cares about **world** textures. Adding a top-level `/trenchbroom` or `/world` folder to keep level design assets can leverage you to create a structure like this:

```
res://
	/trenchbroom
		/textures
```

This way, your world textures can stay wholly separate from your UI and model textures.

If you go this route, you'll have to adjust your Trenchbroom game path to point to the `/trenchbroom` subfolder inside your Godot project, rather than just the root of your Godot project. It's a bit more complicated, but then it ensures that Trenchbroom will only look inside its designated folder.

Although Trenchbroom can't automatically pickup entity definitions made by Qodot, you can also keep all Qodot-related resources inside this `/trenchbroom` folder for easier organizing. An example of this would be:
```
res://
	/trenchbroom
		/textures
		/enemy_entities
			enemy_base.tres
			enemy_fly_point.tres
			enemy_walk_point.tres
			enemy_heal_solid.tres
		my_game_fgd.tres
```

Again, this is only one example on the principle of organizing by context, not by file extension. This is all completely optional compared to what's described in the beginner's guide.

# Project structure examples

## Basic Texturing
Here’s an example of a folder structure for basic texturing.
```
/textures
	/foliage
		vines.png
		grass.png
```
`/foliage` can be replaced by any group name you’d like. The vines* and grass* textures can be replaced by any name appropriate for the texture you're adding.

Only .png files or ,jpg files are valid. You can’t use both file extensions at the same time per map. You can configure this on a QodotMap node's properties.

## Material Override
Either .material files, or .tres files containing a SpatialMaterial or ShaderMaterial, but not both file extensions at the same time.
```
/textures
	/jungle
		vines.png
		vines.material
		zebra.png
		zebra.material
```

## Automatic PBR Texturing
This format is required for any material using PBR texturing.
```
/textures
	/foliage
		/vines
			vines_normal.png
			vines_displacement.png
			vines_roughness.png
		vines.png
```

In the example above, the only names that can change is the group name, `/foliage`, and the material name referenced anywhere in the above structure: vines.
You can put `/foliage` into more sub-folders if you need, so long as its contents remain the same as far as structure is concerned.

## Storing .map files
Map files on their own are not playable scenes in Godot. They need to be built using a QodotMap node, and then saved as a .tscn scene file.
When you keep the .map file with your Godot project, it becomes worlds easier to transport your project, and use version control like Git. You just need to repeat the Trenchbroom Game Definition and Game Path process if you’re starting on a fresh machine.

Here is an example of a folder structure for creating a Quake-like game with Qodot, where one map equals one scene:
```
/levels
	/chapter1
		/mapsource
			jungle1.map
			jungle2.map
		/scenes
			jungle1.tscn
			jungle2.tscn
	/chapter2
		/mapsource
			volcano1.map
		/scenes
			volcano1.tscn
```
You might also want to combine several QodotMap nodes with several .map files into a single .tscn. In this case, a structure like this might be a better choice:
```
/levels
	/chapter1
		/mapsource
			jungle1a.map
			jungle1b.map
			jungle2a.map
			jungle2b.map
			jungle2c.map
		/scenes
			jungle1.tscn
			jungle2.tscn
```

# Inverse Scale Factor

The `Inverse Scale Factor` setting on the `QodotMap` node determines the mapping between Quake units - used in TrenchBroom and other editors - to Godot's metric coordinate system. All coordinates are divided by this value during build.

Having a well-defined mapping from Quake units to the ones used by your game is important - it can make it easier to reason about the layout of your maps, and having your object scale translate properly to the equivalent real-world measurement will result in more accurate physics simulation.

As the metric used by map files varies game-by-game, this setting is dependent on your assets, game logic and physics simulation, and will have to be decided on a case-by-case basis. The table below lists some common examples, as well as reasoning for their usage.

|              Name | Inverse Scale Factor |     1qu |    2qu |    4qu |    8qu |   16qu | Notes |
| ----------------: | :------------------- | :------ | :----- | :----- | :----- | :----- | :---- |
|  .map Passthrough |                    1 |       1 |      2 |       4|      8 |     16 | 1:1 with map file, Godot grid corresponds to TrenchBroom grid. Will result in very large geometry by Godot standards. |
|     Qodot Default |                   16 |  0.0625 |  0.125 |   0.25 |    0.5 |    1.0 | 'Best effort' mapping from Quake 1 environments to metric. |
|          Uradamus |                   40 |   0.025 |   0.05 |    0.1 |    0.2 |    0.4 | Artist-friendly setting with tidy fractional numbers. |
| Valve Environment |       52.49343832021 | 0.01905 | 0.0381 | 0.0762 | 0.1524 | 0.3048 | Half Life 1/2 environment metric. |

# Good Graphics
One of the main benefits to using Qodot is that you can apply level design theory from the quake-era of games while using Godot’s many graphical features to make the game stand out visually.

This section will cover how to use Godot’s lighting and other graphical systems in your Qodot-made levels.

## Lighting
Godot 3.x comes with several lighting options:
-   Dynamic lighting
-   GIProbe
-   BakedLightmap

### Comparison of Lighting Methods

| Benefit | Dynamic Lighting | BakedLightmap | GIProbe |
| ------- | ---------------- |-------------- | ------- |
| Optimized for low-end GPUs | True | True | False |
| Increases project filesize | False | True | False |
| Updates in realtime | True | False | True |
| Uses indirect light | False | True | True |
| Adds to project filesize | False | True | True |
| Photorealistic quality | False | True | True |

### Baked Lightmaps

Qodot supports Godot's static lighting pipeline via a UV2 unwrap option available in the action menu of a QodotMap:

#### Lightmap Volumes

In order to bake static lighting for your map in Godot, it must be covered with a set of BakedLightmap volumes. How many are necessary will depend on the side of your map and number of lights- if you try to bake too much at once the editor will crash, so larger maps will need to be split into multiple volumes.

There are some limitations to be mindful of when doing this:
- Each mesh can only be affected by a single lightmap volume
	- Meshes overlapping multiple BakedLightmap volumes will draw from whichever one is lower in the scene tree
- If using BakedLightmap volumes, each will need to have its `Image Path` property set to a different folder
	- Lightmaps are saved alongside the scene file by default, which causes overwrites
- The editor will only update multiple lightmap volumes on scene reload
	- Be sure to save and reload your scene after baking multiple volumes in order to see correct results

#### Lighting Behaviours

- Energy vs Indirect Energy
  - Energy is used for dynamic lighting
  - Energy and Indirect energy both contribute to static lighting if `Bake Mode` is set to `All`

- Light Settings
  - Bake Indirect is used to supplement dynamic direct lighting with baked indirect lighting
    - Results in a hybrid baked + dynamic light
  - Bake All is used to bake both direct and indirect lighting
    - Dynamic lights set to Bake All must have all their render layers disabled for correct results

#### ConeTrace vs RayTrace

- ConeTrace
	- More accurate to dynamic lighting system
	- Similar metrics, so light settings are easily transferable
	- Shorter bake times on account of a simpler algorithm
	- Global illumination / sky lighting
		- Activated by setting `Propagation` to 1
		- Controlled by baked directional lights
- RayTrace
    - Better quality overall
	- More accurate to real-world light behavior
	- Metrics are incompatible with dynamic lighting system
	- Requires more art time to refine

Some unexpected behavior with certain light types is that spotLight uses both Energy and Indirect energy for baked lighting in RayTrace mode.

Using a Node as the parent of a BakedLightmap will break it out of the scene hierarchy, this allows the user to group meshes and lights together for baking in isolation.

#### Tips on Baked Lightmaps

Godot's lightmap process will crash if too many lights or meshes are used. Grouping rooms into Trenchbroom groups rather than a single worldspawn is recommended for baking lights on larger maps/

Be sure to save often or use version control software like Git to avoid loss of work.

Larger maps should be saved as .scn instead of .tscn in order to minimize saving and loading overhead

### Dynamic Lighting

While there’s a limit to 8 dynamic lights per surface, you can switch their bake mode to “all” so they’re only contributing to static lighting in a BakedLightmap.

Because of the 8 dynamic light limit, you’ll have to split these lights apart in some way, or add a BakedLightmap node to render these lights as static.

### GIProbe
GIProbe provides scenes with realtime, indirect light bounces. Using the surrounding area of a GIProbe node, it can determine how nearby objects should bleed light onto eachother. If you have a big red ball in the middle of a white room, the red of the ball will indirectly appear on the white walls.

The method used in Godot 3.x’s GIProbe is _Monte Carlo Based Global Illumination_.
GDQuest’s video tutorial on using GIProbe is really handy if you’re looking for a guide to get started with using it:
https://www.youtube.com/watch?v=lPngD4uzWVc

In Godot 4, _Signed Distance Field Global Illumination_ is coming as another global illumination option on top of the current Monte Carlo method.

### BakedLightmap
If you’re used to mapping for Quake, GoldSrc, Source, or Source 2, BakedLightmap is your light.exe / vrad.exe equivalent.

Make sure you have UV2 unwrapped in the QodotMap settings, Qodot can provide this UV2 unwrap for you.

Use multiple BakedLightmap nodes to split your map up into sections to allow for modular edits to your lighting.

## Reflections
There are two main methods you can achieve reflections in Godot 3.x:
-   Screen space reflections
-   ReflectionProbes

### Comparison of Reflection Methods

| Benefit | ReflectionProbe | SS Reflections |
| ------- | --------------- | -------------- |
| Optimized for low-end GPUs | True | False |
| High quality | True | False |
| Increases project filesize | True | False |
| Updates in realtime | False | True |
| Uses indirect light | True | False |
| Adds to project filesize | True | False |

### ReflectionProbe
Reflection probes provide pre-calculated reflections of an area to appear on shiny surfaces. For indoor scenes especially, this is significantly more accurate than letting the sky colour/texture determine the reflections shown on a shiny object.

The official docs for Godot 3 stable have a step-by-step tutorial on using reflection probes: [https://docs.godotengine.org/en/stable/tutorials/3d/reflection_probes.html](https://docs.godotengine.org/en/stable/tutorials/3d/reflection_probes.html)

Reflection probes are not volumetric, they work best in square rooms. You can fake a lot of reflection detail in non-boxy areas by enabling screenspace reflections in your WorldEnvironment.

Reflections are planned to improve greatly in Godot 4.0, where a much faster, approximate, and smoother-looking screenspace reflection method replaces the old screenspace reflections. You can read more about it in the official Godot Engine news post here:
https://godotengine.org/article/vulkan-progress-report-7

### 🚧 Screen Space Reflections
Todo.
