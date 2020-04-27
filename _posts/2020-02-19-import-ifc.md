---
layout: default
categories: revit
tags: ifc
title: Opening ifc files in Revit
---

# Opening ifc files in Revit

## Links

### Eye opening blog post

This was the blog post where I learned about this two opening methods: [https://justshutupandbim.wordpress.com/2018/07/30/to-ifc-or-not-to-ifc-that-is-the-question/](https://justshutupandbim.wordpress.com/2018/07/30/to-ifc-or-not-to-ifc-that-is-the-question/)

This post uses some outdated information which is not true if you use a newer version of Revit and the ifc plugin.

### Official ifc links

[Official IFC specifications](https://technical.buildingsmart.org/standards/ifc/ifc-schema-specifications/)

[2x3 TC1 specifications](https://standards.buildingsmart.org/IFC/RELEASE/IFC2x3/TC1/HTML/) This is the the latest version of the most widely used version at the time of writing. However we should change to ifc4.

## Prerequisites

### The Revit IFC plugin

The open source ifc plugin developed by Autodesk is required for successful import of ifc files. At first glance it seems that this plugin only replaces the ifc export dialog, but it also modifies the import behaviour. If no parameters show up in revit after an otherwise successful import, it means the plugin is not installed correctly. 

There are two versions of this plugin, one is available from the Revit Store, the other is downloadable from Sourceforge. You have to install a separate plugin for every Revit version.

The sourceforge and store version signed differently, so they are not compatible to each other. My experience is that the sourceforge version is somewhat more "stable" than the store version. I mean, the store version can stop working after Revit and other updates. This claim needs more testing.

You can download them from here:

- [Revit Store](https://apps.autodesk.com/All/en/List/Search?isAppSearch=True&searchboxstore=All&facet=&collection=&sort=&query=ifc)

- [Sourceforge](https://sourceforge.net/projects/ifcexporter/files/)

### An ifc viewer

If you work with ifc files you need a native program which can show everything inside the ifc file, so you can make sure if something lost during conversion it was lost between the exporting application and ifc or between ifc and revit.

Ifc file is just a text file, sou you can open it with any text editor. This is not true for ifczips, which are - as the name suggest - zips.

#### BIM Vision

Nowadays I use this viewer, because it's free, no registration needed, and I found this opens ifc files quicker than any other viewer.

[Download from here](https://bimvision.eu/en/download/)

#### Navisworks

Navisworks can open ifc files, however I don't recommend using it for this type of verification, as this is also an Autodesk product, so the same bugs can show up as in Revit (e.g. see this post on [Revit Forum](https://revitforum.org/showthread.php/42408-IFC-color-for-material-not-working-for-floors-that-are-cut))

#### IFC Syntax VScode plugin

If you want to check the real data in a text editor I recommend this plugin, as it highlights the numbers in ifc files, it's easier to see what's going on.

To install simply search for 'ifc' on the extensions tab in VScode.

[Marketplace](https://marketplace.visualstudio.com/items?itemName=alanrynne.ifc-syntax)

### Proper IFC 2x3 import settings

V0S0 Github and RevitForum user created a proper import setting file, so every IFC types should be translated to the correct Revit category. You can find this list here:

https://github.com/V0S0/Revit-IFC-Import-Settings

The default category mapping is documented in the Revit Category Guide:

https://docs.google.com/spreadsheets/d/1uNa77XYLjeN-1c63gsX6C5D5Pvn_3ZB4B0QMgPeloTw/edit#gid=3

## The opening and linking methods

An ifc file can be opened two ways in Revit

### Opening ifc

To use this method go to File -> Open -> IFC

This is the older method, and in most cases I recommend using the linking method instead.

Characteristics of this method:

- Every element is a different type, so no real types, huge file size

- All parameters imported as Type parameter, you should be able to schedule them

- There is one instance parameter, IfcGUID, the "ifc id" of the element

- Ifc elements show up in the project browser, and they are in-place models. If you copy them they will duplicate in the project browser and in schedules as well. But you cannot create a new instance from the project browser (just like with in place models)

### Linking ifc

To use this method go to Insert -> Link IFC

In reality it doesn't link the original ifc file. It will create an rvt file named the same way as the ifc file. If you want to work on the ifc file, not just link it, you can open this automatically generated rvt file.

- Parameters are created as Instance parameters, you can schedule and even modify them

- Schedules created automatically listing all parameters

- Imported types don't show up in the project browser, but you can copy them in a view. After copying they still won't show up in the project browser, but they will show up correctly in schedules.

This method uses the newer directshape api. You can read more about this api on bildingcoder: [Directshape api topics](https://thebuildingcoder.typepad.com/blog/about-the-author.html#5.50)
