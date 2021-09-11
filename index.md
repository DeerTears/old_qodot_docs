If you’re in a hurry to start using Qodot, skip to **Getting Started -> Building a Map.**

# Readme
This guide is not yet a complete replacement for the [official wiki on Github](images/https://github.com/Shfty/qodot-plugin/wiki/). The official wiki has more details about all the systems found in Qodot. This guide is instead intended for all Qodot users (beginners or rusty veterans) who are looking to understand more about the plugin with step-by-step instructions.

**This guide is a work in progress** and needs testing. Ping Ember through the Qodot discord if you find anything here that’s incorrect, or if you’d like to help contribute.
Once this guide is mostly battle-tested, this guide will become a dedicated documentation repo on Github. Anyone interested in helping with GHpages and Hugo setup is welcome to contact me on discord as well. Again, ping Ember on the Qodot discord.

## Prerequisites
You should know how to use
- [Trenchbroom](images/https://trenchbroom.github.io/)
- [Godot Engine](images/https://godotengine.org/)

You can find resources to learn trenchbroom through the [Beginner's Guide to Trenchbroom](images/https://coda.io/d/Trenchbroom-Guide_d77T7fADkTg/Beginners-Guide-to-Trenchbroom_suqnS).

GDQuest’s [Getting Started with Godot in 2021](images/https://www.gdquest.com/tutorial/godot/learning-paths/getting-started-in-2021/chapter/1.getting-started/) series is great to help you get started with Godot Engine.

## How to read this guide
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

**Anything in bold** is referring to a section that desktop users can skip to in the outline on the top right.

Examples:
-   **Readme**
-   **Prerequisites**
-   **How to read this guide**

If you see a “🚧” emoji means I’m still doing research to work out the details. The information in a section with 🚧 might still be useful, just bear in mind it may be incomplete, untested, and misleading.

If you find anything that is incomplete, missing, confusing, or otherwise odd, please @ me (Ember) through the Qodot discord, I don’t mind random pings.

Once this guide is closer to completion, I’ll be moving it to a github repository where PRs can be made, and all the writing will be stored as .md files. The end goal is to eventually supersede the current Qodot Wiki, and I have Shifty’s approval to do so once this guide is ready.
## Quick demo
Trenchbroom can be used to place textures, entities, and brush entities, while Qodot can transform those into Godot scenes and materials. It’s super configurable, so you can turn something like this:
![](images/intro-trench.png)

Into this:
![](images/intro-qodot.png)

Qodot in its full capacity becomes really handy at speeding up your iteration process as a level designer. It also makes it easier to let team members work on levels with dedicated level design tools, rather than asking them to create or re-use 3D models all the time.

Anyways, let’s make some cool stuff using old technology in a modern game engine.
# Installing Qodot
Create a new Godot project.

Go to AssetLib in the top left and search for “Qodot”.
![](images/install-assetlib.png)

Once you click “Qodot” and click download, wait for it to finish downloading, then click install:
![](images/install-plugin.png)

A tree of items to be installed will show up. Click the install button at the bottom of the window.

**Note:** Godot can hang near-indefinitely during the import process, the UI thread is frozen during import so it seems like the engine has crashed. You can wait and the import will finish, or you can uncheck the /addons/qodot/textures folder if you plan on following this guide to import your own original textures and materials.

Once the installation/import process is done, go to Project → Project Settings at the top. Click on the “Plugins” tab. By default, Qodot is not enabled. Click the checkbox to enable it:
![](images/install-plugin-enable.png)

You’re ready to proceed with the rest of the guide!
# Getting Started
If you plan on using your project for longer than a few sittings, and you want to update the Qodot plugin, do not add new files to the `/addons/qodot` folder. Anything you add here will be erased if you update Qodot through AssetLib.

You are free to use the files in `/addons/qodot`, but anything you’re adding should be in your own project folders, not `/addons/qodot`.
Here is my personal way of structuring a project so there’s room to add new files outside of addons:

```
res://
 /addons
 /textures
 /meta
 /entities
```

## Building a Map
1.  Add a .map file to your project.
![](images/install-map.png)

2. Load it up from a QodotMap node.

![](images/install-qodotmap.png)

3. Hit Full Build.

![](images/install-fullbuild.png)

Your map is now in Godot!

![](images/install-final.png)

If you don’t see QodotMap in your nodes list, make sure you have enabled Qodot in the Project → Project Settings → Plugin window. If nothing happens, restart Godot and try again.

## Connecting your Godot project to Trenchbroom
The main benefit of connecting your Godot project to Trenchbroom is to synchronize textures and entities between the two editors.

There are three steps to connecting your project to Trenchbroom:
1.  Creating a Trenchbroom Game Definition (using Qodot’s .tres tools)
2.  Setting up Trenchbroom to read your Godot project directory
3.  Enabling Texture Collections in Trenchbroom

After all three are complete, you will be able to use the textures in your Godot project with Trenchbroom.

### Creating a Trenchbroom Game Definition
This will prepare Trenchbroom to read your project’s `/textures` folder.
A Trenchbroom Game Definition is stored as a .cfg file. This .cfg file holds a few things:
-   the name of your game
-   the location of your project’s textures folder
-   the icon of your game in Trenchbroom’s _Game Select_ screen
-   one or more list of entities you want to place in Trenchbroom (FGDs)

When you install Qodot, you get a Resource tool to create a .cfg file. This file is found here:
```
addons/qodot/game_definitions/trenchbroom/Qodot_Trenchbroom_Config_Folder.tres
```

There is also a Config_File.tres file but this isn’t as useful for first-time setup.
Open Config_Folder.tres by double clicking on it in the Filesystem dock. This tool produces a folder with a .cfg config file inside it. You should only need to use this tool once per Godot project.

![](images/definition-resource.png)

Inside of _Trenchbroom Games Folder_, add the path to the `/games` folder inside your computer’s Trenchbroom installation. You’ll have to copy and paste your computer’s filesystem path here.

You should change the _Game Name_ to your project’s name too. Finally, click the _Export File_ checkbox at the top. Your Game Definition should now appear in Trenchbroom!

Before switching over to Trenchbroom, make sure you have a `res://textures/group_name/` folder in your Godot project directory. This isn't a best practice for large projects (more on that later) but it's easiest when getting started.

![](images/definition-textures.png)

This folder structure is necessary for Trenchbroom to pick up textures from your project and display it in the Texture Collections. You can rename "group_name" in the above example to anything you want, it just has to be a sub-folder.
  
You can read more about [Game Definitions for Trenchbroom on the Qodot Wiki.](images/https://github.com/Shfty/qodot-plugin/wiki/2.-Usage#exporting-game-definitions-for-trenchbroom)
### Setting up Trenchbroom to read your Godot Project
This will get Trenchbroom to read your project’s textures.
Launch Trenchbroom, click "New Map..." and select your game's name and icon from the Select Game list.

https://raw.githubusercontent.com/wiki/Shfty/qodot-plugin/images/7-trenchbroom/trenchbroom-game-configs.png

Once it's selected, click "Open preferences..." and find your game from the preferences list. In this example, I called my game QodotTemplate. Yours will be different depending on the _Game Name_ you set earlier in Qodot_Trenchbroom_Config_Folder.tres.

![](images/definition-example.png)

Under "Game Path", click the ellipsis "..." and select your game project folder from your file system, then click Apply. You can now create a New Map with your Godot project as the Game.

**Note:** If you're having trouble clicking the apply button on the window, temporarily increase your screen's resolution. Not clicking apply here can cause issues later on when reloading Trenchbroom.

Once you’ve created something in Trenchbroom, you should save your .map file to your Godot project folder somewhere. This .map file is associated with your game type, you cannot (easily) change which game a .map is associated with, so it’s easiest to start saving your maps directly to your Godot project.

I created a subfolder for levels inside of my Godot project (called “Practice”) where I can keep different .map files, which will later be turned into .tres scenes for Godot.

Source: [https://github.com/Shfty/qodot-plugin/wiki/7.-TrenchBroom#setting-up-trenchbroom-for-godot-project-integration](images/https://github.com/Shfty/qodot-plugin/wiki/7.-TrenchBroom#setting-up-trenchbroom-for-godot-project-integration)
### Enabling Texture Collections in Trenchbroom
This will let you apply your project’s textures to brushes in Trenchbroom.
**Note:** In your Godot project, there always needs to be at least one additional folder under `/textures` for Trenchbroom to read textures from, or else no textures will be picked up.

To enable your textures in Trenchbroom:
1.  Create a map
2.  Click on the “Face” tab at the top right
3.  Click “Texture Collections” at the bottom to unfold it
4.  Click a folder
5.  Click + at the bottom-right to enable it.

Before enabling:

![](images/textures-none.png)

After enabling:

![](images/textures-enabled.png)

You can apply a texture by selecting a brush and clicking the texture.
Your textures and folders will look different depending on the .png / .jpg images inside the group folder, and the group folder name. I made a group folder called city and put brick1.png and brick2.png inside.

# Working with Materials
If you followed the entire **Connecting your Godot Project to Trenchbroom** section you should be able to apply textures to brushes in Trenchbroom, load the .map file in QodotMap, and bulid the map to see **Basic Texturing** take place; No normal maps, roughness, shaders, or anything fancy.

If you want to have several materials that utilize all the properties in Godot’s ShaderMaterial and SpatialMaterial resources, you can use either of Qodot’s two other processes to paint your level:
-   Material Override
-   Automatic PBR Texturing
## Comparison between Material Methods
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
Automatic PBR Texturing tells Qodot to create SpatialMaterials for you when building, provided you have named all texture files to follow the Qodot PBR naming format. You can read more about this in the **Automatic PBR Texturing** section coming up soon.

## Material Override
When you name a texture panel.png, Qodot interprets it as a new material called panel. In Basic Texturing, you’re creating a SpatialMaterial with the Albedo set to panel.png.
If you name a texture and a .material or .tres file the same name (not including the extension) you can **override** any instances of panel.png on a Trenchbroom brush with a panel.material in Godot.

In this example, I’m using an ice shader written in GLSL, originally from: [https://godotshaders.com/shader/spatial-ice-shader/](images/https://godotshaders.com/shader/spatial-ice-shader/)

![](images/materials-ice-details.png)

Both the image and the .material share the same name. If we go to Trenchbroom, we’ll put the texture on brushes:

![](images/materials-placeholder-pink.png)

Then we double-check the file extensions we’re using on the QodotMap node are png and material (or tres if that’s what you prefer to use)

![](images/materials-file-extensions.png)

Building our map should show the ice shader in place of the texture with the same name.

![](images/materials-ice.png)

If this didn’t work, and you followed all instructions, try changing the material to another valid texture in Trenchbroom, save the map, rebuild in Qodot, go back to Trenchbroom, reapply the shader material, then rebuild in Qodot again. That for some reason fixed it for me.

To learn even more about working with materials, read the [Qodot Wiki page on Textures and Materials.](images/https://github.com/Shfty/qodot-plugin/wiki/3.-Textures-and-Materials)

## Automatic PBR Texturing
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

## Default Material
For less control, but quicker setup, you can apply a default material to every single brush face in the map using the Default Material property in a QodotMap. This can be useful if you’re using Qodot to import models and you don’t want to setup textures or materials using any of the above methods.

![](images/materials-default.png)

# Adding your own Entities
Adding entities is a multi-step process, but it’s very rewarding since you get to place Godot scenes and even configure their properties all inside of Trenchbroom.
## Folder Structure
Create the following folders and files in your project directory:
```
/meta/fgd
 /base_classes
 /point_classes
 /solid_classes
 your_game_name_fgd.tres*
 ```
*Making your own fgd .tres is coming up soon in the **Making an FGD File** section.

Again, don’t edit what’s in the `/addons/qodot` folder. This will leave your entity definitions subject to deletion when AssetLib updates the plugin. Create your own folders like the ones above in your project’s directory.

You can keep using the Config_Folder tool inside of `/addons` since this it will be there reliably when the plugin updates between versions.

## Making a point entity
In this example, I’m going to make a point entity that will spawn a Godot scene, containing a tree with collision.

To make your own point entity, right click on a folder in Filesystem, and click _Create New Resource_. Search for “Qodot” and you should get a list of resources to extend.

![image.png](images/https://codahosted.io/docs/77T7fADkTg/blobs/bl-gxLeBPapgk/df7f3e64157cc5085709823ccce9ce813168255136a532fc78b5806905e228f0df0ab2bfdb7d12fa2ae9f791bc45fa8154f880fce88d6746f9442c49e1c2563cd0e3f7f4459740b023f077778689d3342975c87fffec366c36aa65493418849f0b1b9452)

In my case, I just want to set the origin of my .tscn, so I’m choosing QodotFGDPointClass. I don’t need it to be a solid brush or an abstract base class.
I named mine prop_tree.tres

![image.png](images/https://codahosted.io/docs/77T7fADkTg/blobs/bl-Wuhvn8GqQY/70c0e3f1041a02c76fb8e6c23b143ff883674fd9b7b2f81fbe036e378bbdc285f90474f56de267085912a787b294d36b7c0e82077ad08fefd23271244d33bc56b45eb972f1cec9eae609a06f8e9740170aca1c43d05ff8da98a13ddd40f3af2da90b6881)

I’m going to prepare a tree scene that I want the prop_tree.tres to represent.
Here’s my tree scene, it’s a StaticBody so the player collides with it, but you can have any type of node here instead to make up this entire scene.

![image.png](images/https://codahosted.io/docs/77T7fADkTg/blobs/bl-eNKN09n274/22870c857308cf0cf29b6433d54c5c6679f6477c41381e31d2ddc5b6656e47ca1daf079a9913bb40bbeefb0f6adb7ca3de7971dd5c864614b022ee2b349eaf569adfa6dc6f6484dcbd82b259008784881661c9ff7de3b9f45bfe6add0d236e5d2c416ee8)

I’ll save it next to the .tres so it’s easy to find and update both files representing the same entity.

![image.png](images/https://codahosted.io/docs/77T7fADkTg/blobs/bl-si9xMqe0Us/b59c5375c516b930d859f96b7cd09fdeda38f5e76ddd3b55f0308e4793910f96361faa6204337d4055a811b85cc13e53f02d58442361261749ff4a7b2fa4052e5fe173bd1373ef1f2b5aaf3799723fa2e73aec122932cabbba59385a8b642194f5e45a8f)

Now I can double click on the .tres. I’m going to set the Scene File to the tree scene:

![image.png](images/https://codahosted.io/docs/77T7fADkTg/blobs/bl-54U2U_gycH/34b38dd28822146a9f5d7388c23607f45d8a793ee21d68d976af608079ea25b62bd92cc561d7ce168d3b644aaa272f1c54cf8d204eba4484ce8a0e5538553b1f2c11e474f13cfc35e7513dc2913ea011930636805fd9347d5d6ef3eacfae068e32b85ac1)

Notice I only needed to change the Scene File and the Classname in this .tres. Everything else is optional.

Now we can include this entity in a new FGD file. That way, Qodot can parse all of our game’s objects, and we can load them into Trenchbroom as well.

To learn more about working with props and entities, read the [Qodot Wiki page on Entities.](images/https://github.com/Shfty/qodot-plugin/wiki/4.-Entities)
### Making an FGD File
Qodot comes with a default FGD file, but you shouldn’t edit it because any changes will be overwritten when you update the Qodot plugin.

Instead, you should create our own FGD for your  game. This way, Qodot will be able to port your objects over, and all changes will be safe if you decide to update Qodot later.

Right-click on the Filesystem dock and click Create New Resource. Make a QodotFGDFile:

![image.png](images/https://codahosted.io/docs/77T7fADkTg/blobs/bl-WrbQagxvxB/faca91bba64558581e77c577cb043b0965b26a778f42fca7bec3dfb4f94cae095fc9cde8007ff62a736d0b63810ccdaf0fedafab6786e931a065248d5d31f0f73649901ad49482265f0434f2e0b836f7f0bc75208900d67723557efc068c3ac0a50bffaf)

I recommend saving this file as your_game_name_fgd.tres so it’s easy to search for “fgd” or just your game’s name the Filesystem dock. Mine is practice_fgd.tres.
Change the _Fgd Name_ to your game’s name. You can then open the Entity Definitions array, delete everything inside it, set _Size_ to 1, and drop the tree_prop.tres into the list.

![image.png](images/https://codahosted.io/docs/77T7fADkTg/blobs/bl-y11gfMCDG5/7db319dcfe9df0846f090b47e5cb490f59ba0f2d2520f30e7b632c08fa30b30a7d5fbeb4a29b043d0268ee9a656e86a87cdd63b59403f3d4803efbbee7cc566c26027bab11d41b8b9906e8e4d3597741436fa3776f89ae374134171658a174d26d64cb60)

**Warning:** You have to set the Fgd Name so it doesn’t conflict with the “Qodot” namespace, otherwise Trenchbroom will discard your FGD.

It’s okay to delete the entity definitions that are included by default in an FGD because we can switch FGDs in Trenchbroom, and include multiple FGDs when building maps.

Finally, we’re almost done. Now we need to update the Trenchbroom Game Definition to include our new FGD and entity definitions.
### Updating the Trenchbroom Game Definition
Go back to the Config_Folder tool and add a new item to the _Fgd Files_ array. Add the FGD you made by dragging and dropping, or using the folder selection button.

![image.png](images/https://codahosted.io/docs/77T7fADkTg/blobs/bl-FzkbKV9nbb/04e6eb13407c60ff95322a0627a1dde76f89973d022315763f2abafcdf9630df2514565ef2d63dc21e5cfa9427e932f9eda3a067b1b132b17695f59704c268aeabe7fd8bb3d27fee23ab7cc49899ccc9ec875cf8a1a815d1623d4dbdd02795460e8ac106)

Click export file again. You can either restart Trenchbroom or click File → Refresh Entity Definitions.

**Note:** If you get an error about incomplete strings, make sure that the Fgd Name on your own FGD is not “Qodot”.

Now you should be able to see Entity Definitions in a list:

![image.png](images/https://codahosted.io/docs/77T7fADkTg/blobs/bl-yzq2y_LQzl/23eb7ddfe36250e9740bc5d9077f6641e0f54f06c4a9b81b386cbe5a29e874d2a647a967fa01e3b922e8bba1cfac00fdac0eb64540a1e5e0dc50bd233398cd9d8e10bd9bfed11b02e10071f72c5b1acc8990de95a1e2af4a5469f723893a646c3c2aeb63)

Switching to your FGD should show your list of entities.

![image.png](images/https://codahosted.io/docs/77T7fADkTg/blobs/bl-XB0MYn9qnD/a4a1cd0ffa6043ec43d13c3358b7c57fe3f07d3e4f154696a5e6cadabc9aad17a6fa1c1953455841bc66f15b2c1d779af4a3dacdb255c47ec983bd20b43776f73d5dffd867a45d87c5f3239d589109a3fc5df7fd775b204f9387bd16701e72a1d201c44c)

Place it somewhere in the level, save the map, then go back to Godot. There’s one final step required before we can see our entities in Godot.
### Adding FGDs to a QodotMap
In your QodotMap node, open the _Entity Fgd_ array_._ Although it’s already taken by Qodot’s default FGD, you can add your own FGD in the _Base Fgd Files_ array inside the Qodot FGD.

![image.png](images/https://codahosted.io/docs/77T7fADkTg/blobs/bl-dHL7W0LNh8/ea03ca3a428ae30fd81851b8c544c91da07001a2e8180344a6e1272ae5290cf8f16a2adc05873764a69580142d1fd5314e807b54bb646a2044f6a07d656c1aa86a608e95a3ec23ec85ce4dc911065e59a6243d200b3bb1d35a07b2c69ae72ef1022ad94b)

Here I’ve added a size of 1 to the _Base Fgd Files_ property and dragged my FGD inside.
Build the map, your point entity should show up!

![image.png](images/https://codahosted.io/docs/77T7fADkTg/blobs/bl-Quw6e7Htp7/053b2984d5984e7d894b3e13135746c977fc0378852f87ece9241a128aea48096806d7b9ec5d75e5def8838a5cc371d20342940e4531f8a261294e71906eb1a17d76500a8407366435fe56b5af47af0dfd0e238efc3baf3a35586034851c3af78e23a5e6)

## Working with Properties
There’s more we can do to extend entities.
### Example in Trenchbroom with the default FGD
Working off of the Qodot default FGD, you can see that the properties for a physics_ball look like this in Trenchbroom:

![image.png](images/https://codahosted.io/docs/77T7fADkTg/blobs/bl-qs9WY6QFQq/99e16b6be91dafe48a1fab6c9b38e69fc701ba960b9638e4830ac3bd7c7b4f825ad351c691a7e29592daebb3e4a88441bbc6b42696c361df344a9cccc6c3d313b3a7a70264a34713f65155f8023cc4bed6f8a593be9b25297650c5e0ff38973b4f6b277d)

You can double click on any of the values to edit them. I’m going to change velocity from 0 0 0 into 0 0 50.

Now when we compile the map, a few things happen.
1.  Qodot looks for all instances of the entity
2.  Qodot checks the properties for each entity and adds it to a properties dictionary.
(The key / value system of Trenchbroom properties works 1:1 with the key / value system of Godot’s Dictionaries.)
3. A script associated with the entity turns properties into actual changes on the node and script it represents.

Rebuilding my map, I get to watch the physics ball fly off into the distance because I set its velocity in Trenchbroom:

![image.png](images/https://codahosted.io/docs/77T7fADkTg/blobs/bl-E5r4Wput-J/7eb50f6ad45b2d3e587455dd134410599009b7f5376287b68e548f7a939c73929d1fe3d7292f53a594b7fa54493145e23e6227d6aa0a34015368ae9e43ff988ab905d34a48c881085702b420e6e5d78535a1f1bdd41ce1cbe2ee52b395c43bdc5e16feef)
### 🚧 Responding to properties with a script
Add a script that extends QodotEntity. This will give it a properties dictionary.
You can read contents from the properties dictionary by doing the following checks in code:

```gdscript
if "name" in properties:
 properties.name
```

Then, add this script to the entity definition. I think this won’t work if it’s just a script of the scene the entity represents.
## 🚧 Models in Trenchbroom
You can display a model in TrenchBroom that will be built as the equivalent model in Godot. In this case, the Trenchbroom model just represents a point entity.
The model has to be an .obj, and you have to create an entity definition in your game’s FGD for that one model. You cannot swap models in and out of Trenchbroom like you can with Source or other 2000s-era BSP workflows.

🚧 Step by step guide coming in future 🚧

Taken from the Qodot wiki:

You can add a **Meta Properties** in your point entity definition with model as the key and the relative path of your .obj file as the value.
Example:
`key: model value: "entities/models/my_model.obj"`

**Warning:** Add quotes surrounding your value, or TrenchBroom may crash when placing your entity class.

Now that you’ve done this, you also need to update your Game Definition every time your FGD changes. You can repeat the process for exporting a game definition from earlier to overwrite the entire folder, or see **Updating a Game Definition** in [Expert Zone](images/https://coda.io/d/Trenchbroom-Guide_d77T7fADkTg/Expert-Zone_suNzt) for more information on just updating the .cfg.

# Ideal Project Structure
Structuring the folders of your game project is a challenge in of itself. Doing so in a way that makes Qodot integration easy is another challenge. This section will show you ideal ways to structure a Qodot project, but also give tips where Qodot or Trenchbroom reads specific folder structures, such as when reading materials.
## Rule of Thumb for Large Projects 🚧
As an example, to get your project’s materials to be read from Trenchbroom, you need a folder called /textures and a subfolder to group your texture files:
res://textures/my_group_name/texture.png
Trenchbroom also needs to have the Game Path set for your game definition:

![unknown.png](images/https://codahosted.io/docs/77T7fADkTg/blobs/bl-xR7X0B1EAA/f5b91a8957aef0f553c817bf95a7283314d28f03e9dbcc43c92346e443e348deaeb56946eba4bb5cae803815a7b4c65942e88667d4a2a676dc0911f22f8b560e271c431f15c210d5165030307b48ad0a86abbba1bc1565182feaee2f626729e805494de4)

When you set your project directory in Trenchbroom, it looks for an immediate subfolder called `/textures`. If you just want textures in the game, this is fine:
- Game Path: `C:/Users/Ember/Documents/Godot Projects/MyQodotProject/`
- Automatically-detected textures path: `C:/Users/Ember/Documents/Godot Projects/MyQodotProject/textures/`

But for larger projects this isn’t ideal. You want to group assets by context, rather than filetype, you can always search for filetype in the search bar.

What you can do instead is create a subfolder to store Trenchbroom-specific textures and materials, to keep it contextually separate from textures you might use for models, shaders, and particles.
An example of this would be:
```
res://
 /trenchbroom
 /textures
```
The setting your Game Path not to your Godot project, but to `C:/Users/Ember/Documents/Godot Projects/MyQodotProject/trenchbroom`, so it picks up on just the textures you want to use for level design.
This way, anything that Trenchbroom needs, but you as a Godot user don’t need to regularly access for actors, logic, sounds, menus, and other assets, you can store in a separate folder to keep the structure explicit.
In full, you could separate everything Trenchbroom would be using into a folder like so:
```
res://
 /trenchbroom
 /textures
 /entity_definitions
 /fgds
 /maps
```
Although you don’t need to do this for anyhing outside of textures, it’s just one of many ways you can organize everything that trenchbroom relies on separate from your project folder.

The rest of these setups are agnostic if you choose the root folder with your .project, or if you create a separate `/trenchbroom` folder to contain anything fed to the level editor, directly or indirectly.
## Addons
Don’t add files to the `/addons` folder yourself. Your original work can be erased if Godot auto-updates Qodot, or if you ignore the /addons folder in a Git repository to save on space.
Instead, create a new folder on `res://` for whatever you’re adding.
## Materials
### Basic Texturing
Here’s an example of a folder structure for basic texturing.
```
- /textures
 - /foliage
 - vines.png
 - grass.png
```
`/foliage` can be replaced by any group name you’d like. The vines* and grass* textures can be replaced by any name appropriate for the texture you're adding.

Only .png files or ,jpg files are valid. You can’t use both file extensions at the same time per map. You can configure this on a QodotMap node's properties.
### Material Override
Either .material files, or .tres files containing a SpatialMaterial or ShaderMaterial, but not both file extensions at the same time.
```
- /textures
 - /jungle
 - vines.png
 - vines.material
 - zebra.png
 - zebra.material
```
### Automatic PBR Texturing
This format is required for any material using PBR texturing.
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
In the example above, the only names that can change is the group name, `/foliage`, and the material name referenced anywhere in the above structure: vines.
You can put `/foliage` into more sub-folders if you need, so long as its contents remain the same as far as structure is concerned.
## Storing .map files
Map files on their own are not playable scenes in Godot. They need to be built using a QodotMap node, and then saved as a .tscn scene file.
When you keep the .map file with your Godot project, it becomes worlds easier to transport your project, and use version control like Git. You just need to repeat the Trenchbroom Game Definition and Game Path process if you’re starting on a fresh machine.

Here is an example of a folder structure for creating a Quake-like game with Qodot, where one map equals one scene:
```
- /levels
 - /chapter1
 - /mapsource
 - jungle1.map
 - jungle2.map
 - /scenes
 - jungle1.tscn
 - jungle2.tscn
 - /chapter2
 - /mapsource
 - volcano1.map
 - /scenes
 - volcano1.tscn
```

You might also want to combine several QodotMap nodes with several .map files into a single .tscn. In this case, a structure like this might be a better choice:
```
/levels
 - /chapter1
 - /mapsource
 - jungle1a.map
 - jungle1b.map
 - jungle2a.map
 - jungle2b.map
 - jungle2c.map
 - /scenes
 - jungle1.tscn
 - jungle2.tscn
```
## Entities
```
- /meta/fgd/
 - /base_classes
 - /point_classes
 - /solid_classes
 - your_game_name_fgd.tres
```
# Good Graphics
One of the main benefits to using Qodot is that you can apply level design theory from the quake-era of games while using Godot’s many graphical features to make the game stand out visually.

This section will cover how to use Godot’s lighting and other graphical systems in your Qodot-made levels.
## Lighting
Godot 3.x comes with several lighting options:
-   Dynamic lighting
-   GIProbe
-   BakedLightmap
## Comparison of Lighting Methods
Benefit | Dynamic Lighting | BakedLightmap | GIProbe
------- | ---------------- |-------------- | --------
Optimized for low-end GPUs | true | true | false
Increases project filesize | false | true | false
Updates in realtime | true | false | true
Uses indirect light | false | true | true
Adds to project filesize | false | true | true
Photorealistic quality | false | true | true

### Dynamic Lighting
Using OmniLight, DirectionalLight, and SpotLight should be your first steps in lighting your level.

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
## Comparison of Reflection Methods
Benefit | ReflectionProbe | SS Reflections
------- | --------------- | --------------
Optimized for low-end GPUs | true | false
High quality | true | false
Increases project filesize | true | false
Updates in realtime | false | true
Uses indirect light | true | false
Adds to project filesize | true | false
### ReflectionProbe
Reflection probes provide pre-calculated reflections of an area to appear on shiny surfaces. For indoor scenes especially, this is significantly more accurate than letting the sky colour/texture determine the reflections shown on a shiny object.

The official docs for Godot 3 stable have a step-by-step tutorial on using reflection probes: [https://docs.godotengine.org/en/stable/tutorials/3d/reflection_probes.html](images/https://docs.godotengine.org/en/stable/tutorials/3d/reflection_probes.html)

Reflection probes are not volumetric, they work best in square rooms. You can fake a lot of reflection detail in non-boxy areas by enabling screenspace reflections in your WorldEnvironment.

Reflections are planned to improve greatly in Godot 4.0, where a much faster, approximate, and smoother-looking screenspace reflection method replaces the old screenspace reflections. You can read more about it in the official Godot Engine news post here:
https://godotengine.org/article/vulkan-progress-report-7
### 🚧 Screen Space Reflections
Todo.
# Unfinished Topics
This guide is meant to extensively cover steps to do what most users need to make the most of Qodot. I haven’t been able to cover it all, so I have a list of what’s not yet covered here:
## To Research
- Brush Entities
- Responding to properties with a script
- BakedLightmap in Qodot step-by-step
- Models in Trenchbroom step-by-step
- Screenspace reflections
- Emitting signals from brush triggers
- Setting up ReflectionProbes
- Worldspawn Layers
- Emitting signals from point entities (creating a point_hurt that calls take_damage())
- How to find entities that are emitting signals so you can connect them appropriately
