---
layout: default
title: Textures
nav_order: 3
---

1. TOC
{:toc}

# Introduction
There are two ways Qodot reads textures from a map file:
- Loose image files
- .wad files

Information on [using .wad files](https://github.com/Shfty/qodot-plugin/wiki/3.-Textures-and-Materials#wad-file-support) is available on the old Qodot Wiki. I still need to research the application of .wad files, especially when trying to apply more complicated materials. The rest of this guide covers loose image files.

When using loose textures, you can use any of Qodot’s three material-building processes to paint your level in ShaderMaterials and SpatialMaterials. They are:
- Basic Texturing
- Material Override
- Automatic PBR Texturing

**Warning:** QodotMap won't read textures with spaces in the filename, including the folder it comes from. Please check your textures fit this limitation before continuing.

# Comparison of Texturing Methods
Here is a table showing a quick overview of the benefits some methods have over others.

Benefit | Basic Texturing | Material Override | Auto PBR Texturing
------- | --------------- | ----------------- | ------------------
Use textures between Trenchbroom and Qodot | y | y | y
Name materials freely | y | y | n
Flexible folder structure | y | y | n
Keep material tweaks on map rebuild | n | y | n
Uses GLSL shaders | n | y | n
Uses PBR Maps | n | y | y
Builds Godot PBR materials for you | n | n | y

Material Override applies a .material or .tres of the same name as your texture file, letting you use ShaderMaterials and customized SpatialMaterials instead of flat textures.

Automatic PBR Texturing tells Qodot to create SpatialMaterials for you when building, provided you have named all texture files to follow the Qodot PBR naming format.

# Basic Texturing

Basic texturing applies an image to the brushes in your map. You can still control if filtering and other import effects are used by double-clicking the texture and changing the settings in the Import dock.

Read [Connecting your project to Trenchbroom](../Beginner's-Guide-to-Qodot#connecting-your-project-to-trenchbroom) to learn how to get textures applied to your map.

# Material Override
When you name a texture panel.png, Qodot interprets it as a new material called panel. In Basic Texturing, you’re creating a SpatialMaterial with the Albedo set to panel.png.
If you name a texture and a .material or .tres file the same name (not including the extension) you can **override** any instances of panel.png on a Trenchbroom brush with a panel.material in Godot.

In this example, I’m using an ice shader written in GLSL, originally from: [https://godotshaders.com/shader/spatial-ice-shader/](https://godotshaders.com/shader/spatial-ice-shader/)

![](../images/materials-ice-details.png)

Both the image and the .material share the same name. If we go to Trenchbroom, we’ll put the texture on brushes:

![](../images/materials-placeholder-pink.png)

Then we double-check the file extensions we’re using on the QodotMap node are png and material (or tres if that’s what you prefer to use)

![](../images/materials-file-extensions.png)

Building our map should show the ice shader in place of the texture with the same name.

![](../images/materials-ice.png)

If this didn’t work, and you followed all instructions, try changing the material to another valid texture in Trenchbroom, save the map, rebuild in Qodot, go back to Trenchbroom, reapply the shader material, then rebuild in Qodot again. That for some reason fixed it for me.

To learn even more about working with materials, read the [Qodot Wiki page on Textures and Materials.](https://github.com/Shfty/qodot-plugin/wiki/3.-Textures-and-Materials)

# Automatic PBR Texturing
You can use Automatic PBR Texturing to let Qodot do the hard work of assembling a SpatialMaterial for you, so long as you give it PBR maps and name them appropriately. 

**This method doesn’t let you tweak the materials after without undoing your work every build**. It's intended more-or-less to mass-import PBR materials onto a map. If you want more control, use **Material Override** instead.

All PBR maps work off of a base texture. The base texture serves as the Albedo/Diffuse, as well as the texture shown in Trenchbroom. To get Automatic PBR Texturing to work:
1.  Create a texture inside a subfolder: textures/foliage/vines.png
2.  Create a folder in the same directory with the texture’s name: textures/foliage/vines/
3.  Using the texture’s name, append suffixes to make up the PBR maps.

**Example:**
```
- /textures
 - /foliage
 - /vines
 - vines_normal.png
 - vines_displacement.png
 - vines_roughness.png
 - vines.png
 - vines.material
```

In the example above, the only names that can change is the group name, `/foliage`, and the material name, "vines".

You can put `/foliage` into more subfolders if you need, so long as its contents remain the same as far as structure is concerned.

To apply a PBR material in Trenchbroom, include only the folder that contains the Albedo texture, vines.png.

# Default Material
For less control, but quicker setup, you can apply a default material to every single brush face in the map using the Default Material property in a QodotMap. This can be useful if you’re using Qodot to import models and you don’t want to setup textures or materials using any of the above methods.

![](../images/materials-default.png)
