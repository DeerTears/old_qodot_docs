---
layout: default
title: Installation
nav_order: 1
---

# Installation

1. TOC
{:toc}

# Prerequisites
You should know how to use
- [Trenchbroom](https://trenchbroom.github.io/)
- [Godot Engine](https://godotengine.org/)

You can find resources to learn trenchbroom through the [Beginner's Guide to Trenchbroom](https://coda.io/d/Trenchbroom-Guide_d77T7fADkTg/Beginners-Guide-to-Trenchbroom_suqnS).

GDQuest’s [Getting Started with Godot in 2021](https://www.gdquest.com/tutorial/godot/learning-paths/getting-started-in-2021/chapter/1.getting-started/) series is great to help you get started with Godot Engine.

# Installing Qodot
Create a new Godot project.

Once you're loaded into the project, go to the AssetLib tab and search for “Qodot”.

![](../images/install-assetlib.png)

Once you click “Qodot” and click download, wait for it to finish downloading, then click "Install":

![](../images/install-plugin.png)

A tree of items to be installed will show up. Click "Install" at the bottom of the window.

**Note:** Godot 3.x can hang during the import process, but this is not a crash. It will finish importing if you wait long enough. You can speed this up by:
- Deleting `/addons/qodot/textures` if you don't plan on copying the example textures to your own `/textures` folder
- Force-closing and force-reopening Godot until each image import is complete
- Temporarily remove `/addons/qodot` and disable the Import Etc2 property in your Project Settings, then move `/addons/qodot` back in and re-enable in Project Settings -> Plugins.

![](../images/install-no-examples.png)

When it's done, go to Project → Project Settings -> Plugins. By default, Qodot is not enabled. Click the checkbox next to its name to enable it:

![](../images/install-plugin-enable.png)

You’re ready to proceed with the rest of the guide!