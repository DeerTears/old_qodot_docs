---
layout: default
title: Beginner's Guide to Qodot
nav_order: 2
---

1. TOC
{:toc}

# Prerequisites
Before continuing, you should know how to use:
- [Trenchbroom](https://trenchbroom.github.io/)
- [Godot Engine](https://godotengine.org/)

You can find resources to learn trenchbroom through the [Beginner's Guide to Trenchbroom](https://coda.io/d/Trenchbroom-Guide_d77T7fADkTg/Beginners-Guide-to-Trenchbroom_suqnS).

GDQuest’s [Getting Started with Godot in 2021](https://www.gdquest.com/tutorial/godot/learning-paths/getting-started-in-2021/chapter/1.getting-started/) video series is great to help you get started with Godot Engine, as well as many other YouTube channels like [Garbaj](https://www.youtube.com/c/Garbaj), and text resources like [KidsCanCode](http://kidscancode.org/godot_recipes/).

# Building a Map
Assuming your map is original, and has no textures or entities, this is the fastest way to get maps into Godot.

1.  Add a .map file to your project.

![](../images/install-map.png)

2. Load it up from a QodotMap node.

![](../images/install-qodotmap.png)

3. Hit Full Build.

![](../images/install-fullbuild.png)

Your map is now in Godot!

![](../images/install-final.png)

**Note:** If you don’t see QodotMap in your nodes list, make sure you have enabled Qodot in the Project → Project Settings → Plugin window.

If you want to display textures on your map geometry, you'll need to connect your Godot project to Trenchbroom with a .cfg, as shown in the [Connecting your project to Trenchbroom](#connecting-your-project-to-trenchbroom) section.

## Porting a map

While this guide can't cover all steps for porting, it's important to check if you have the following:  
- A Trenchbroom .cfg that supports your specific game. (Quake, Daikatana)  
- A .map that doesn't crash Trenchbroom when saving or loading, using your game's .cfg.  
- Textures as .wad or image files.  
- All .fgd files used to define the map's entities.  

If you don't have a game config for your ported game, even after searching community forums, you can compare your map's file structure to maps for similar games to find out how to parse it in a .cfg, or generate your own Godot project .cfg file and make new geometry instead.

If your map cannot be loaded or saved properly, you may need to try other game settings in Trenchbroom. It helps to compare the .map file structure to other .map files which load correctly in Trenchbroom.

If your map is missing .fgd files, you will be missing out on the map's entities. You can build the map with Qodot anyways, and any issues with missing/wireframe geometry can be solved by deleting any `undefined` entities in the .map.

If you don't have the map's textures, you can re-texture the map geometry with .png and .jpg/.jpeg files as shown in [Texturing your map](#texturing-your-map).

# Connecting your project to Trenchbroom
When working with an original project, there are three main steps to connecting your project to Trenchbroom:
1.  Create a Trenchbroom game config (using Qodot’s `config_folder.tres` tool)
2.  Add your Godot project directory to Trenchbroom's game path
3.  Enable texture collections to place your project's textures on brushes

**Warning:** Never add new files to the `/addons/qodot` folder. Anything you add here will be erased since Qodot can auto-update through AssetLib. You are free to use or remove files in `/addons/qodot` as we'll be seeing soon. Keep any new files to your own project structure.

With this in mind, you should create a unique Trenchbroom game config for each Godot project you create, rather than extending the Qodot.fgd provided by the plugin.

If you're porting a map with an existing game config, and you're rolling your own textures into the map, you can skip ahead to [Setting your project directory in Trenchbroom](#Setting-your-project-directory-in-Trenchbroom). Otherwise, you can read about [using .wad files](https://github.com/Shfty/qodot-plugin/wiki/3.-Textures-and-Materials#wad-file-support) on the old Qodot Wiki.

## Making a Trenchbroom game config
A Trenchbroom game configuration (stored as a .cfg file) tells Trenchbroom the name and icon for your game, how your maps are saved, where your `/textures` folder is, and where the entity definitions are kept.

When you install Qodot, you get a resource tool to create your own .cfg file.

Look for the `Qodot_Trenchbroom_Config_Folder.tres` file in your addons folder. It is installed at  `res://addons/qodot/game_definitions/trenchbroom/`. You can ignore `Qodot_Trenchbroom_Config_File.tres`, it's the Folder variant we want.

**Note:** Your project needs an `icon.png` file, or else Qodot won't be able to generate a Trenchbroom game config. Godot will complain on this step until you re-add `res://icon.png` and restart Godot.

Open the resource by double clicking on it in the Filesystem dock. This will give you a list of tools to edit the .cfg inside of your Inspector.

![](../images/definition-resource.png)

Inside of _Trenchbroom Games Folder_, add the path to the `/games` folder inside your computer’s Trenchbroom installation. You’ll have to copy and paste your computer’s filesystem path here. An example would be `C:/Users/Ember/Documents/My Games/Trenchbroom/games/`.

You should change the _Game Name_ to your project’s name too, to avoid clashing with other game names.

Before clicking _Export File_ make sure you have a `res://textures/tutorial/` folder in your Godot project directory. This will help us later when we add textures to our project. You can call the folder something else, there just needs to be a subfolder under the `/textures` folder for Trenchbroom to read textures later. This isn't a best practice for large projects (more on that later) but it's easiest when getting started.

![](../images/definition-textures.png)

Finally, click the _Export File_ checkbox at the top. Your game config should now be in Trenchbroom! You can go and see if Trenchbroom has your game listed in the games list when creating a new map.

The next two steps will ensure Trenchbroom can read your project's textures, and so Qodot can correctly read those textures and apply it to geometry in Godot.

## Setting Game Path in Trenchbroom Preferences
Once you've got a config file, Trenchbroom still needs you to manually set the *Game Path* property in its Preferences menu. Once set, Trenchbroom will be able to read your Godot project’s `/textures` folder.

Launch Trenchbroom, click "New Map..." and select your game's name and icon from the Select Game list.

https://raw.githubusercontent.com/wiki/Shfty/qodot-plugin/images/7-trenchbroom/trenchbroom-game-configs.png

Once it's selected, click "Open preferences..." and find your game in the preferences list. In this example, I called my game QodotTemplate. Yours will be different depending on the _Game Name_ you set earlier in `Qodot_Trenchbroom_Config_Folder.tres`.

![](../images/definition-example.png)

Under "Game Path", click the ellipsis "..." and select your Godot project folder from your file system. Click Apply.

You can now create a New Map with your Godot project name as the game type.

**Note:** If you're having trouble clicking the apply button on the window, temporarily increase your screen's resolution. Not clicking apply here can cause issues later on when reloading Trenchbroom.

## Enabling Texture Collections in Trenchbroom
You can only enable a texture collection if your project has the following conditions:
- Your game path is set
- There's a top-level folder inside your game path called `/textures`
- There's a subfolder inside of `/textures`, such as `/tutorial` or `/forest`
- There's a .png or .jpg image file inside of the subfolder

Once all of these conditions are satisfied, press F5 to refresh the texture definitions in Trenchbroom.

Navigate to the "Face" tab in the top right, and unfold the "Texture Collections" menu at the bottom.

Click the name of a folder you want to enable.

![](../images/textures-none.png)

Click "+" at the bottom-right to enable it, moving it to the "enabled" collection.

![](../images/textures-enabled.png)

## The Next Step

Congratulations! You've completed the Beginner's Guide to Qodot. From having completed this guide, you should be able to build maps in Qodot and unify textures between Trenchbroom and your Godot project.

If you're interested in learning how to apply PBR materials and shaders to brushes, read [Textures](textures.html) for more info.

If you want to place Godot scenes and configure their properties using Trenchbroom, read [Entities](entities.html) for more info.

If you want a breakdown of Qodot's many systems, and prepare your project for scaling up, read [Best Practices](best-practices.html).