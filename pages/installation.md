---
layout: default
title: Installation
nav_order: 1
---

Before installing Qodot, you should know how to use:
- [Trenchbroom](https://trenchbroom.github.io/)
- [Godot Engine](https://godotengine.org/)

You can find resources to learn trenchbroom through the [Beginner's Guide to Trenchbroom](https://coda.io/d/Trenchbroom-Guide_d77T7fADkTg/Beginners-Guide-to-Trenchbroom_suqnS).

GDQuest’s [Getting Started with Godot in 2021](https://www.gdquest.com/tutorial/godot/learning-paths/getting-started-in-2021/chapter/1.getting-started/) series is great to help you get started with Godot Engine.

# Installing Qodot
Create a new Godot project, or load an existing one.

Go to the AssetLib tab and search for “Qodot”.

![](../images/install-assetlib.png)

Once you click “Qodot” and click download, wait for it to finish downloading, then click "Install":

![](../images/install-plugin.png)

A tree of items to be installed will show up. Click "Install" at the bottom of the window.

**Note:** Godot 3.x can hang during the import process, but this is not a crash. It will finish importing if you wait long enough. You can speed this process up by moving `/addons/qodot` out of your project, set Project -> Project Settings -> Import -> Import Etc2 to false. You can also delete `/addons/qodot/textures` if you don't plan on copying the example textures to your own `/textures` folder.

When the importer is done, go to Project → Project Settings -> Plugins. By default, Qodot is not enabled. Click the checkbox next to its name to enable it:

![](../images/install-plugin-enable.png)

You’re ready to proceed with the rest of the guide! Check out [Beginner's Guide to Qodot](beginner's-guide-to-qodot) to get started with the plugin.