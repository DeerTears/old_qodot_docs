---
layout: default
title: Building Maps
nav_order: 4
parent: Beginner's Guide to Qodot 
---

# Building a Map
Assuming your map is original, and has no textures or entities, this is the fastest way to get maps into Godot.

- Add a .map file to your project.

![](../images/install-map.png)

- Load it up from a QodotMap node.

![](../images/install-qodotmap.png)

- Hit Full Build.

![](../images/install-fullbuild.png)

Your map is now in Godot!

![](../images/install-final.png)

**Note:** If you don’t see QodotMap in your nodes list, make sure you have enabled Qodot in the Project → Project Settings → Plugin window.

If you want to display textures on your map geometry, you'll need to connect your Godot project to Trenchbroom with a .cfg, as shown in the [Connecting your project to Trenchbroom](#connecting-your-project-to-trenchbroom) section.

If you don't have an original map, and you're trying to port a map instead, read the page on [Porting](../porting.html).

# Connecting your project to Trenchbroom
When working with an original project, there are three main steps to connecting your project to Trenchbroom:
1.  Create a Trenchbroom game config (using Qodot’s `config_folder.tres` tool)
2.  Add your Godot project directory to Trenchbroom's game path
3.  Enable texture collections to place your project's textures on brushes

**Warning:** Never add new files to the `/addons/qodot` folder. Anything you add here will be erased since Qodot can auto-update through AssetLib. You are free to use or remove files in `/addons/qodot` as we'll be seeing soon. Keep any new files to your own project structure.
