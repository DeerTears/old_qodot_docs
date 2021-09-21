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

A Point Entity is a position in 3D space. They work well for spawn locations, pickups, enemies and more. Point Entities are useful for instanciating Godot scenes in 3D space. Having collisions, visuals, or scripts are completely optional for any given Point Entity.

Brush Entities are brushes with a script attached. By default, they are grouped separately from the world geometry, letting you transform and manipulate each Solid Class individually. One example is using a "physics" Brush Class to let the brush interact with physics. Multiple brushes can be a part of a single Solid Class.

**Note:** Keep in mind that "Entities" and "Classes" mean the same thing for this entire page. Qodot calls them classes, Trenchbroom calls them entities. This has nothing to do with the concept of classes in GDscript. This also applies for "Brush" and "Solid", Qodot calls them solid entities, Trenchbroom calls them brush entities. Godot on its own doesn't understand "brushes", rather it holds collision and geometry for groups of nodes once they're built through a QodotMap node.

You are given some entity definitions through `Qodot.fgd`, such as
- Light
- Breakable
- Ball

These example entities have limited functionality, but they exist as an example of what can be done.

# Creating new Entities

In Qodot, you define new types of entities by creating a new .tres resource file. Qodot provides 3 resource presets for this:

- QodotFGDPointClass
- QodotFGDSolidClass
- QodotFGDBaseClass

Click the "New Resource" icon in the Inspector:
![](../images/res-inspector.png)

Or right-click the FileSystem and click "Create New Resource":
![](../images/res-filesystem.png)

Either will take you to the resource search screen. Here you can type in the following quick terms to narrow down the type of resource you want to create:

- pointclass
- solidclass
- baseclass

Each class has a different set of properties to adjust:

- Point Classes instantiate a scene at a specific location, like health pickups and enemies.
- Solid Classes attach a script to a specific brush (or group of brushes), like an interactable door or a breakable window.
- Base Classes are empty; they only contain properties as a template for other classes. They're handy when needed, but never required.

## Valid Data Types

As shown later in this page, you can define properties for entities in the Class Properties dictionary. Although Godot provides several data-type options for the values, Trenchbroom can only read 8 of them. These data types are:

- Bool
- Int
- Float
- String
- Vector3
	- Stored as a string in the form X Y Z
- Color
	- Stored as a string in the form R G B
- Dictionary
	- Used for defining a set of choice keys and their associated values
	- Will display a dropdown in compatible editors
- Array
	- Used for bitmask flag properties
	- Each entry represents a flag as a nested array in the form [name, value, default]
	- Will display a grid of checkboxes in compatible editors

What follows is a breakdown of each property in the Inspector when editing one of the three main class types.

## Base Class Properties

These properties are shared by Point and Solid classes. Base Class only exists as a template for other classes, if you're looking for an ECS-like approach to sharing properties.

**Classname** - The name for this class in Trenchbroom.

**Description** - A short description that displays in Trenchbroom when this entity is selected.

**Qodot Internal** - Hides the entity in Trenchbroom's entity browser.

Qodot can still refer to this entity for building other entities (like base entities). Base Entities are already hidden in Trenchbroom by default, for most situations you can leave this off.

**Base Classes** - An array of template entities to inherit properties from.

This determines the parent entities for this class, where this entity gets all the class properties, meta properties, and property descriptions from its parents.

**Class Properties** - A dictionary of properties.

Once exported to the cfg, the dictionary values are written by editing entities in Trenchbroom, and read by accessing `properties` on a QodotEntity once the map is built.

To add new class properties to an entity:

1. Open the dictionary.
2. Click the first pencil by "New Key: [null]" and select "String" as the key data type.
3. Name your property in the key text field, no spaces.
4. Click the second pencil by "New Value: [null]" and select any valid data type.
5. Change the value to your desired default, or leave it blank.
6. Click "Add new key/value pair".
7. Repeat 1-6 for every new property.

You can also set default values for your properties, by repeating this process in Meta Properties, matching the key names and value datatypes from this dictionary.

**Class Property Descriptions** - A dictionary of descriptions for each property, visible in Trenchbroom.

Follow the same steps as adding class properties to add property descriptions. Ensure the description's key value matches the property's key value. The value can only be a String.

**Meta Properties** - Editor-specific properties, such as the default color and size used to represent this class.

Add an entry with a matching key to an existing property, set the value to the same data type, and whatever value is present will become the default. Only one meta property is read per class property.

**Node Class** - The type of Godot node to spawn at this location.

This doesn't fully control the type of node that Godot spawns, read on to learn more about using Node Class with Point Classes and Solid Classes.

## Point Class properties

Point Classes inherit all features from Base Classes, with a few additions. QodotMap reads their position in the map, and places a QodotMap node in their place. You have the option to replace the QodotEntity node with other node types, or even instance .tscn scenes in their place.

**Scene File** - The .tscn Godot scene that spawns in place of QodotEntity. The easiest way to spawn a specific node at a specific point.

Scene File takes priority over every other option here, especially if there's already a script attached to the .tscn file.

**Script Class** - The script to attach to your entity's root node.

This is ideal for spawning nodes without a .tscn file. If you want to access this entities' Class Properties, add a script here that `extends QodotMap` and accesses `properties["key_name"]`.

You can extend other Spatial-derived node types too, and add necessary children (CollisionShape, MeshInstance) through code, but you lose out on the abliity to access `properties`.

Make sure you update Node Class to match the node this class `extends` from.

**Node Class** - The type of node to spawn at this location. This property should match the `extends` of your Script Class. You can ignore this property if you're using a Scene File.

Be careful of which node you extend in Script Class. If you wanted to create Area nodes by point entity, extending `Area` in this script prevents you from accessing your Class Properties. However, extending `QodotEntity` prevents you from accessing the functions and signals of an Area.

You can get around this by extending `QodotEntity` and using `var node = Node.new()` and `add_child(node)` to add more complex nodes as children, while still having access to the `properties` dictionary.

## Solid Class Properties

🚧

# Creating an FGD file

FGD fles (Forge Game Data) hold definitions for entities. They are required to make Trenchbroom and QodotMap understand anything you want to place that will move, have scripts, or be instanciated.

Since it's impossible to display entities without an FGD file, this should be your first step before adding any entities for a brand new Godot project.

Qodot comes with a default FGD file, but you shouldn’t edit it because any changes will be overwritten when you update the Qodot plugin. Instead, you should create our own FGD file. This way, Qodot will be able to port your objects over, and all changes will be safe if you decide to update Qodot later.

Right-click on the Filesystem dock and click Create New Resource. Make a QodotFGDFile:

![](https://codahosted.io/docs/77T7fADkTg/blobs/bl-WrbQagxvxB/faca91bba64558581e77c577cb043b0965b26a778f42fca7bec3dfb4f94cae095fc9cde8007ff62a736d0b63810ccdaf0fedafab6786e931a065248d5d31f0f73649901ad49482265f0434f2e0b836f7f0bc75208900d67723557efc068c3ac0a50bffaf)

I recommend saving this file as your_game_name_fgd.tres so it’s easy to search for “fgd” or just your game’s name the Filesystem dock. Mine is practice_fgd.tres.

Change the _Fgd Name_ to your game’s name. You can then open the Entity Definitions array, delete everything inside it, set _Size_ to 1, and drop the tree_prop.tres into the list.

![](https://codahosted.io/docs/77T7fADkTg/blobs/bl-y11gfMCDG5/7db319dcfe9df0846f090b47e5cb490f59ba0f2d2520f30e7b632c08fa30b30a7d5fbeb4a29b043d0268ee9a656e86a87cdd63b59403f3d4803efbbee7cc566c26027bab11d41b8b9906e8e4d3597741436fa3776f89ae374134171658a174d26d64cb60)

**Warning:** You have to set the Fgd Name so it doesn’t conflict with the “Qodot” namespace, otherwise Trenchbroom will discard your FGD.

It’s okay to delete the entity definitions that are included by default in an FGD because we can switch FGDs in Trenchbroom, and include multiple FGDs when building maps.

The FGD on its own doesn't put entities into your map. We still need to update the Trenchbroom game config to include our new FGD and entity definitions, later we'll make sure the QodotMap node in our scene also catches wind of the new FGD.

### Updating your CFG

Check the *Fgd Files* property on your `config_folder.tres` and make sure your FGD is on the list.

Click Export File in the Inspector.

Go to Trenchbroom, press F6 to reload entity definitions.

Go to the Entity tab, unfold the Entity Definitions at the bottom, choose your game's FGD.

![](../images/entity-definitions-unfolded.png)
 
You can now drag your entity into the map geometry to place it in Trenchbroom.

## Add the FGD to your Trenchbroom game config
Open the `config_folder.tres` resource and add a new item to the _Fgd Files_ array. Add the FGD you made by dragging and dropping, or using the folder selection button.

![](https://codahosted.io/docs/77T7fADkTg/blobs/bl-FzkbKV9nbb/04e6eb13407c60ff95322a0627a1dde76f89973d022315763f2abafcdf9630df2514565ef2d63dc21e5cfa9427e932f9eda3a067b1b132b17695f59704c268aeabe7fd8bb3d27fee23ab7cc49899ccc9ec875cf8a1a815d1623d4dbdd02795460e8ac106)

**Note:** Make sure to save your FGD resource first by clicking the save icon in the top right, or by pressing Ctrl + S with any scene open.

Click _Export File_ again. You can either restart Trenchbroom, click File → Refresh Entity Definitions, or press F6.

**Note:** If you get a Trenchbroom error about incomplete strings, make sure the *Fgd Name* on your own FGD is not “Qodot”.

Now you should be able to see Entity Definitions in a list. This means your FGD made it into Trenchbroom successfully, and you can move onto [Creating new Entities](#creating-new-entities).

![](https://codahosted.io/docs/77T7fADkTg/blobs/bl-yzq2y_LQzl/23eb7ddfe36250e9740bc5d9077f6641e0f54f06c4a9b81b386cbe5a29e874d2a647a967fa01e3b922e8bba1cfac00fdac0eb64540a1e5e0dc50bd233398cd9d8e10bd9bfed11b02e10071f72c5b1acc8990de95a1e2af4a5469f723893a646c3c2aeb63)

If you already have entities in your FGD, switching to your FGD should show your list of entities in the entity browser.

![](https://codahosted.io/docs/77T7fADkTg/blobs/bl-XB0MYn9qnD/a4a1cd0ffa6043ec43d13c3358b7c57fe3f07d3e4f154696a5e6cadabc9aad17a6fa1c1953455841bc66f15b2c1d779af4a3dacdb255c47ec983bd20b43776f73d5dffd867a45d87c5f3239d589109a3fc5df7fd775b204f9387bd16701e72a1d201c44c)

To build our entities in a QodotMap, we'll still need to add our updated FGD to Qodot's _Entity Fgd_ property.

## Adding FGDs to a QodotMap
In your QodotMap node, open the _Entity Fgd_ array_._ Although it’s taken by Qodot.fgd by default, you can replace it with your own FGD file. Alternatively, you can open Qodot.fgd's _Base Fgd Files_ array and add your FGD there. Either works fine.

![](https://codahosted.io/docs/77T7fADkTg/blobs/bl-dHL7W0LNh8/ea03ca3a428ae30fd81851b8c544c91da07001a2e8180344a6e1272ae5290cf8f16a2adc05873764a69580142d1fd5314e807b54bb646a2044f6a07d656c1aa86a608e95a3ec23ec85ce4dc911065e59a6243d200b3bb1d35a07b2c69ae72ef1022ad94b)

Here I’ve added a size of 1 to the _Base Fgd Files_ property and dragged my FGD inside.
Build the map, your point entity should show up!

![](https://codahosted.io/docs/77T7fADkTg/blobs/bl-Quw6e7Htp7/053b2984d5984e7d894b3e13135746c977fc0378852f87ece9241a128aea48096806d7b9ec5d75e5def8838a5cc371d20342940e4531f8a261294e71906eb1a17d76500a8407366435fe56b5af47af0dfd0e238efc3baf3a35586034851c3af78e23a5e6)

# Accessing Class Properties in Code

`properties`

This dictionary's keys are the same as the keys you set earlier in Class Properties.

You can check for these properties using `if "key_name" in properties` and access them with `properties["key_name"]`. This is how you read Trenchbroom-set properties in Godot.

For example, if you have a point light entity with a "color" key and a Color value in the class properties, your code could look like this to apply the color to your light:

```gdscript
extends QodotEntity

# This variable is static-typed so I get code autocomplete.
# I want to code like it already exists, even though it's created at runtime.
var light: OmniLight

func _ready:
	light = OmniLight.new()
	add_child(light)
	# Optional step: prevent errors if the property doesn't exist.
	if "color" in properties:
		# Assign the value read from the map file to this node's property
		light.light_color = properties["color"]

```

In general, instance complex nodes in code, so you don't lose access to the properties dictionary. You can do this for .tscn files too if they have no script on the root. If there is a script on the .tscn root, you can also instance the entire .tscn file (script and all) as a child added through code, as shown above.

# Updating the entity pipeline
After every entity you create, there are 3 steps to ensure that Trenchbroom and QodotMap are aware of its existence. You can do these in a batch, or one-by-one.

1. Update your game's FGD to hold a new entity definition, featuring your new .tres file(s).
2. Re-export your Trenchbroom game config with your updated FGD included.
3. Add your FGD to a QodotMap's *Entity Fgd*, either directly, or as a *Base Fgd File* inside another FGD.

## Step-by-step examples

### Placing scenes with point entities

In this example, I’m going to make a point entity that will spawn a Godot scene, containing a tree with collision.

Start by making own point entity: right click on a folder in Filesystem, and click _Create New Resource_. Search for “Qodot” and you should get a list of resources to extend.

![](https://codahosted.io/docs/77T7fADkTg/blobs/bl-gxLeBPapgk/df7f3e64157cc5085709823ccce9ce813168255136a532fc78b5806905e228f0df0ab2bfdb7d12fa2ae9f791bc45fa8154f880fce88d6746f9442c49e1c2563cd0e3f7f4459740b023f077778689d3342975c87fffec366c36aa65493418849f0b1b9452)

In my case, I just want to set the origin of my .tscn, so I’m choosing QodotFGDPointClass. I don’t need it to be a solid brush or an abstract base class.
I named mine prop_tree.tres

![](https://codahosted.io/docs/77T7fADkTg/blobs/bl-Wuhvn8GqQY/70c0e3f1041a02c76fb8e6c23b143ff883674fd9b7b2f81fbe036e378bbdc285f90474f56de267085912a787b294d36b7c0e82077ad08fefd23271244d33bc56b45eb972f1cec9eae609a06f8e9740170aca1c43d05ff8da98a13ddd40f3af2da90b6881)

I’m going to prepare a tree scene that I want the prop_tree.tres to represent.
Here’s my tree scene, it’s a StaticBody so the player collides with it, but you can have any type of node here instead to make up this entire scene.

![](https://codahosted.io/docs/77T7fADkTg/blobs/bl-eNKN09n274/22870c857308cf0cf29b6433d54c5c6679f6477c41381e31d2ddc5b6656e47ca1daf079a9913bb40bbeefb0f6adb7ca3de7971dd5c864614b022ee2b349eaf569adfa6dc6f6484dcbd82b259008784881661c9ff7de3b9f45bfe6add0d236e5d2c416ee8)

I’ll save it next to the .tres so it’s easy to find and update both files representing the same entity.

![](https://codahosted.io/docs/77T7fADkTg/blobs/bl-si9xMqe0Us/b59c5375c516b930d859f96b7cd09fdeda38f5e76ddd3b55f0308e4793910f96361faa6204337d4055a811b85cc13e53f02d58442361261749ff4a7b2fa4052e5fe173bd1373ef1f2b5aaf3799723fa2e73aec122932cabbba59385a8b642194f5e45a8f)

Now I can double click on the .tres. I’m going to set the Scene File to the tree scene:

![](https://codahosted.io/docs/77T7fADkTg/blobs/bl-54U2U_gycH/34b38dd28822146a9f5d7388c23607f45d8a793ee21d68d976af608079ea25b62bd92cc561d7ce168d3b644aaa272f1c54cf8d204eba4484ce8a0e5538553b1f2c11e474f13cfc35e7513dc2913ea011930636805fd9347d5d6ef3eacfae068e32b85ac1)

Notice I only needed to change the Scene File and the Classname in this .tres. Everything else is optional.

Now we can include this entity in a new FGD file. That way, Qodot can parse all of our game’s objects, and we can load them into Trenchbroom as well. This is covered earlier in the [Creating an FGD file](#creating-an-fgd-file) section.

To learn more about working with props and entities, read the [Qodot Wiki page on Entities.](https://github.com/Shfty/qodot-plugin/wiki/4.-Entities)

### Creating a Brush Entity / Solid Class

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

# Foreword

If there's any information you expected to see here, but didn't (or something here had you lost) please [flag an issue on the github repository.](https://github.com/DeerTears/DeerTears.github.io/issues) This is the most complicated page of all and it needs the most attention.