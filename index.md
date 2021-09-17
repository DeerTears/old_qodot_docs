---
layout: default
title: Qodot Docs
nav_exclude: true
---

Welcome! This is the documentation for Qodot, a Quake-to-Godot plugin initially developed by Shifty.

Qodot is a powerful and complex tool to convert Trenchbroom and other Quake tools into Godot geometry, collisions, and scenes. When configured well, you can do all of your game's level design in Trenchbroom, and only move back to Godot to add entity definitions for doors, enemies, pickups, spawn locations, and more. You can turn a Trenchbroom map like this:

![](images/intro-trench.png)

Into a Godot scene like this:

![](images/intro-qodot.png)

If you are new to Qodot, you don't have to use all of its features to make good use of it. The [Beginner's Guide to Godot](/pages/beginner's-guide-to-godot.html) page has everything you need to get started with the plugin on a basic level.

If you have experience and want to transform Trenchbroom into a full level-design suite for Godot, the [Best Practices](/pages/best-practices.html) page gives insight and breakdowns on Qodot's many configurable systems.

# How to read this guide
_Anything In Italics_ is an editor property you can change in the Inspector.

Examples:
-   _Texture_
-   _Material_
-   _Scale_
-   _Fgd Files_

Anything → With → Arrows describes nested, foldable properties or buttons, in either Trenchbroom or Godot.

Examples:
-   File → Save
-   Transform → Position → X
-   Project → Project Settings → Plugins

If you see a “🚧” emoji means I’m still doing research to work out the details. You are free to contribute knowledge on the [docs Github page](https://github.com/DeerTears/DeerTears.github.io) to assist me with this section.

## Glossary

wip 🚧

I am a Source mapper at heart, so forgive me if I talk about "Brush Entities" because although it's functionally the same, technically Qodot calls them "Solid Classes".