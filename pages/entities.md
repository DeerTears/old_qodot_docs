---
layout: default
title: Entities
nav_order: 4
---

1. TOC
{:toc}

# Introduction
Entities tie Godot functionality to 3D positions and specially-marked brushes in your map file.

There are two main types of entities:
- Point Classes (Point Entities)
- Solid Classes (Brush Entities)

**Note:** As a former Hammer user, I gravitate to "Entities" as a name rather than "Classes", since Godot also has script classes declared with `class_name`. This is how I differentiate Qodot functionality from Godot functionality.

A Point is a position in the map. Point Entities make great spawn locations for players, pickups, enemies and more. Point  are useful for instanciating Godot scenes in 3D space, but having collisions or visuals is optional.

Solid Classes are brushes with a script attached. By default, they are grouped separately from the world geometry, letting you transform and manipulate each Solid Class individually. One example is using a "physics" Brush Class to let the brush interact with physics. Multiple brushes can be a part of a single Solid Class.

You are given some entity definitions through `Qodot.fgd`, such as
- Light
- Breakable
- Ball

These example entities have limited functionality, but they exist as an example of what can be done.

# Creating new entities

In Qodot, you define new types of entities by creating a .tres resource file, using one of Qodot's 3 resource presets:

- QodotFGDPointClass: Instantiates a scene at a specific location, like health pickups and enemies.
- QodotFGDSolidClass Attaches a script to a specific brush, like an interactable door or a breakable window.
- QodotFGDBaseClass: An empty class that just contains properties, acting a template for other classes. Handy when needed, but never required.

You can access these presets by creating a new resource. You can either click the "New Resource" icon in the Inspector, or right-click the FileSystem and click "Create New Resource".

![](../images/Pasted image 20210911194206.png)

![](../images/Pasted image 20210911194226.png)

Both will take you to the resource search screen. Here you can type in the following quick terms to narrow down the type of resource you want to create:

- pointclass
- solidclass
- baseclass

All of these files serve different purposes, so using the suffix `_point`, `_solid`, and `_base` in filenames makes it easier to search for them in the FileSystem dock.

## Updating your game config

Every time you create a new entity definition, you need to ensure that Trenchbrom is told a new entity definition exists. You do this by re-exporting your game config.

After you've created new entity definitions, make sure to:
- Add the new entity definitions to your game's FGD
- Check the FGD is included in your Trenchbroom game config
- Export the new config by clicking Export File in the Inspector
- Go to Trenchbroom and press F6 to reload entity definitions

## 🚧 Creating new properties

todo

# Placing Scenes with Point Entities

In this example, I’m going to make a point entity that will spawn a Godot scene, containing a tree with collision.

Start by making own point entity: right click on a folder in Filesystem, and click _Create New Resource_. Search for “Qodot” and you should get a list of resources to extend.

![image.png](https://codahosted.io/docs/77T7fADkTg/blobs/bl-gxLeBPapgk/df7f3e64157cc5085709823ccce9ce813168255136a532fc78b5806905e228f0df0ab2bfdb7d12fa2ae9f791bc45fa8154f880fce88d6746f9442c49e1c2563cd0e3f7f4459740b023f077778689d3342975c87fffec366c36aa65493418849f0b1b9452)

In my case, I just want to set the origin of my .tscn, so I’m choosing QodotFGDPointClass. I don’t need it to be a solid brush or an abstract base class.
I named mine prop_tree.tres

![image.png](https://codahosted.io/docs/77T7fADkTg/blobs/bl-Wuhvn8GqQY/70c0e3f1041a02c76fb8e6c23b143ff883674fd9b7b2f81fbe036e378bbdc285f90474f56de267085912a787b294d36b7c0e82077ad08fefd23271244d33bc56b45eb972f1cec9eae609a06f8e9740170aca1c43d05ff8da98a13ddd40f3af2da90b6881)

I’m going to prepare a tree scene that I want the prop_tree.tres to represent.
Here’s my tree scene, it’s a StaticBody so the player collides with it, but you can have any type of node here instead to make up this entire scene.

![image.png](https://codahosted.io/docs/77T7fADkTg/blobs/bl-eNKN09n274/22870c857308cf0cf29b6433d54c5c6679f6477c41381e31d2ddc5b6656e47ca1daf079a9913bb40bbeefb0f6adb7ca3de7971dd5c864614b022ee2b349eaf569adfa6dc6f6484dcbd82b259008784881661c9ff7de3b9f45bfe6add0d236e5d2c416ee8)

I’ll save it next to the .tres so it’s easy to find and update both files representing the same entity.

![image.png](https://codahosted.io/docs/77T7fADkTg/blobs/bl-si9xMqe0Us/b59c5375c516b930d859f96b7cd09fdeda38f5e76ddd3b55f0308e4793910f96361faa6204337d4055a811b85cc13e53f02d58442361261749ff4a7b2fa4052e5fe173bd1373ef1f2b5aaf3799723fa2e73aec122932cabbba59385a8b642194f5e45a8f)

Now I can double click on the .tres. I’m going to set the Scene File to the tree scene:

![image.png](https://codahosted.io/docs/77T7fADkTg/blobs/bl-54U2U_gycH/34b38dd28822146a9f5d7388c23607f45d8a793ee21d68d976af608079ea25b62bd92cc561d7ce168d3b644aaa272f1c54cf8d204eba4484ce8a0e5538553b1f2c11e474f13cfc35e7513dc2913ea011930636805fd9347d5d6ef3eacfae068e32b85ac1)

Notice I only needed to change the Scene File and the Classname in this .tres. Everything else is optional.

Now we can include this entity in a new FGD file. That way, Qodot can parse all of our game’s objects, and we can load them into Trenchbroom as well.

To learn more about working with props and entities, read the [Qodot Wiki page on Entities.](https://github.com/Shfty/qodot-plugin/wiki/4.-Entities)

## Making an FGD File
Qodot comes with a default FGD file, but you shouldn’t edit it because any changes will be overwritten when you update the Qodot plugin.

Instead, you should create our own FGD for your  game. This way, Qodot will be able to port your objects over, and all changes will be safe if you decide to update Qodot later.

Right-click on the Filesystem dock and click Create New Resource. Make a QodotFGDFile:

![image.png](https://codahosted.io/docs/77T7fADkTg/blobs/bl-WrbQagxvxB/faca91bba64558581e77c577cb043b0965b26a778f42fca7bec3dfb4f94cae095fc9cde8007ff62a736d0b63810ccdaf0fedafab6786e931a065248d5d31f0f73649901ad49482265f0434f2e0b836f7f0bc75208900d67723557efc068c3ac0a50bffaf)

I recommend saving this file as your_game_name_fgd.tres so it’s easy to search for “fgd” or just your game’s name the Filesystem dock. Mine is practice_fgd.tres.
Change the _Fgd Name_ to your game’s name. You can then open the Entity Definitions array, delete everything inside it, set _Size_ to 1, and drop the tree_prop.tres into the list.

![image.png](https://codahosted.io/docs/77T7fADkTg/blobs/bl-y11gfMCDG5/7db319dcfe9df0846f090b47e5cb490f59ba0f2d2520f30e7b632c08fa30b30a7d5fbeb4a29b043d0268ee9a656e86a87cdd63b59403f3d4803efbbee7cc566c26027bab11d41b8b9906e8e4d3597741436fa3776f89ae374134171658a174d26d64cb60)

**Warning:** You have to set the Fgd Name so it doesn’t conflict with the “Qodot” namespace, otherwise Trenchbroom will discard your FGD.

It’s okay to delete the entity definitions that are included by default in an FGD because we can switch FGDs in Trenchbroom, and include multiple FGDs when building maps.

Finally, we’re almost done. Now we need to update the Trenchbroom game config to include our new FGD and entity definitions.

## Updating the Trenchbroom game config
Go back to the Config_Folder tool and add a new item to the _Fgd Files_ array. Add the FGD you made by dragging and dropping, or using the folder selection button.

![image.png](https://codahosted.io/docs/77T7fADkTg/blobs/bl-FzkbKV9nbb/04e6eb13407c60ff95322a0627a1dde76f89973d022315763f2abafcdf9630df2514565ef2d63dc21e5cfa9427e932f9eda3a067b1b132b17695f59704c268aeabe7fd8bb3d27fee23ab7cc49899ccc9ec875cf8a1a815d1623d4dbdd02795460e8ac106)

Click export file again. You can either restart Trenchbroom or click File → Refresh Entity Definitions.

**Note:** If you get an error about incomplete strings, make sure that the Fgd Name on your own FGD is not “Qodot”.

Now you should be able to see Entity Definitions in a list:

![image.png](https://codahosted.io/docs/77T7fADkTg/blobs/bl-yzq2y_LQzl/23eb7ddfe36250e9740bc5d9077f6641e0f54f06c4a9b81b386cbe5a29e874d2a647a967fa01e3b922e8bba1cfac00fdac0eb64540a1e5e0dc50bd233398cd9d8e10bd9bfed11b02e10071f72c5b1acc8990de95a1e2af4a5469f723893a646c3c2aeb63)

Switching to your FGD should show your list of entities.

![image.png](https://codahosted.io/docs/77T7fADkTg/blobs/bl-XB0MYn9qnD/a4a1cd0ffa6043ec43d13c3358b7c57fe3f07d3e4f154696a5e6cadabc9aad17a6fa1c1953455841bc66f15b2c1d779af4a3dacdb255c47ec983bd20b43776f73d5dffd867a45d87c5f3239d589109a3fc5df7fd775b204f9387bd16701e72a1d201c44c)

Place it somewhere in the level, save the map, then go back to Godot. There’s one final step required before we can see our entities in Godot.

## Adding FGDs to a QodotMap
In your QodotMap node, open the _Entity Fgd_ array_._ Although it’s already taken by Qodot’s default FGD, you can add your own FGD in the _Base Fgd Files_ array inside the Qodot FGD.

![image.png](https://codahosted.io/docs/77T7fADkTg/blobs/bl-dHL7W0LNh8/ea03ca3a428ae30fd81851b8c544c91da07001a2e8180344a6e1272ae5290cf8f16a2adc05873764a69580142d1fd5314e807b54bb646a2044f6a07d656c1aa86a608e95a3ec23ec85ce4dc911065e59a6243d200b3bb1d35a07b2c69ae72ef1022ad94b)

Here I’ve added a size of 1 to the _Base Fgd Files_ property and dragged my FGD inside.
Build the map, your point entity should show up!

![image.png](https://codahosted.io/docs/77T7fADkTg/blobs/bl-Quw6e7Htp7/053b2984d5984e7d894b3e13135746c977fc0378852f87ece9241a128aea48096806d7b9ec5d75e5def8838a5cc371d20342940e4531f8a261294e71906eb1a17d76500a8407366435fe56b5af47af0dfd0e238efc3baf3a35586034851c3af78e23a5e6)

## Creating a Brush Entity / Solid Class

Create a new Solid Entity resource by searching for "solid".

![](../images/Pasted image 20210911192501.png)

Although not necessary, it helps to organize entities by which category they fall into, since they otherwise share the file extension of .tres.

![](../images/Pasted image 20210911192614.png)

Here you get several options to customize a solid class.

![](../images/Pasted image 20210911192631.png)

Our goal will be to give the brush entity a name and then to print that name.

Notice that unlike the point entity, we don't have a place to add a .tscn file like before. We are only given a place to hold a script.

Now that we've created a new entity class, we can add it to our game's FGD.

![](../images/Pasted image 20210911192948.png)

Then update the configuration for Trenchbroom using the FGD resource by clicking Export File.

Now we can go back into Trenchbroom and try to create a brush entity.

When loading up a map for your game config, first check that you're using your game's FGD and not Qodot's by clicking your game's name:

![](../images/Pasted image 20210911193415.png)

<!-- Problem! I cannot get a brush entity to transfer over? -->

# Working with Properties
There’s more we can do to extend entities.
## Example in Trenchbroom with the default FGD
Working off of the Qodot default FGD, you can see that the properties for a physics_ball look like this in Trenchbroom:

![image.png](https://codahosted.io/docs/77T7fADkTg/blobs/bl-qs9WY6QFQq/99e16b6be91dafe48a1fab6c9b38e69fc701ba960b9638e4830ac3bd7c7b4f825ad351c691a7e29592daebb3e4a88441bbc6b42696c361df344a9cccc6c3d313b3a7a70264a34713f65155f8023cc4bed6f8a593be9b25297650c5e0ff38973b4f6b277d)

You can double click on any of the values to edit them. I’m going to change velocity from 0 0 0 into 0 0 50.

Now when we compile the map, a few things happen.
1.  Qodot looks for all instances of the entity
2.  Qodot checks the properties for each entity and adds it to a properties dictionary.
(The key / value system of Trenchbroom properties works 1:1 with the key / value system of Godot’s Dictionaries.)
3. A script associated with the entity turns properties into actual changes on the node and script it represents.

Rebuilding my map, I get to watch the physics ball fly off into the distance because I set its velocity in Trenchbroom:

![image.png](https://codahosted.io/docs/77T7fADkTg/blobs/bl-E5r4Wput-J/7eb50f6ad45b2d3e587455dd134410599009b7f5376287b68e548f7a939c73929d1fe3d7292f53a594b7fa54493145e23e6227d6aa0a34015368ae9e43ff988ab905d34a48c881085702b420e6e5d78535a1f1bdd41ce1cbe2ee52b395c43bdc5e16feef)
## 🚧 Responding to properties with a script
Add a script that extends QodotEntity. This will give it a properties dictionary.
You can read contents from the properties dictionary by doing the following checks in code:

```gdscript
if "name" in properties:
 properties.name
```

Then, add this script to the entity definition. I think this won’t work if it’s just a script of the scene the entity represents.
# 🚧 Models in Trenchbroom
You can display a model in TrenchBroom that will be built as the equivalent model in Godot. In this case, the Trenchbroom model just represents a point entity.
The model has to be an .obj, and you have to create an entity definition in your game’s FGD for that one model. You cannot swap models in and out of Trenchbroom like you can with Source or other 2000s-era BSP workflows.

🚧 Step by step guide coming in future 🚧

Taken from the Qodot wiki:

You can add a **Meta Properties** in your point entity definition with model as the key and the relative path of your .obj file as the value.
Example:
`key: model value: "entities/models/my_model.obj"`

**Warning:** Add quotes surrounding your value, or TrenchBroom may crash when placing your entity class.

Now that you’ve done this, you also need to update your game config every time your FGD changes. You can repeat the process for exporting a game config from earlier to overwrite the entire folder, or see **Updating a game config** in [Expert Zone](https://coda.io/d/Trenchbroom-Guide_d77T7fADkTg/Expert-Zone_suNzt) for more information on just updating the .cfg.