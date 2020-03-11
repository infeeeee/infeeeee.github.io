---
layout: default
categories: revit
tags: ifc
title: Exporting ifc files from Revit
---

# Exporting ifc files from Revit

## The Revit IFC plugin

The open source ifc plugin developed by Autodesk adds a lot of extra options to the default ifc exporter dialog. If you want to export an ifc files from Revit it's highly recommended to install this plugin.

There are two versions of this plugin, one is available from the Revit Store, the other is downloadable from Sourceforge. You have to install a separate plugin for every Revit version.

The sourceforge and store version signed differently, so they are not compatible to each other. My experience is that the sourceforge version is somewhat more "stable" than the store version. I mean, the store version can stop working after Revit and other updates. This claim needs more testing.

You can download them from here:

- [Revit Store](https://apps.autodesk.com/All/en/List/Search?isAppSearch=True&searchboxstore=All&facet=&collection=&sort=&query=ifc)

- [Sourceforge](https://sourceforge.net/projects/ifcexporter/files/)

## Exporting parameters

Fortunately this is very well documented in the help:

Go to Export Ifc>Modify Setup>Property sets  

- Export Revit property sets: it will export all Revit properties of all elements to ifc  
- Export user defined property sets: it will export only selected Revit 
  properties to a custom property set, more info and example config here: [Official help](https://knowledge.autodesk.com/support/revit-products/learn-explore/caas/simplecontent/content/export-custom-bim-standards-and-property-sets-to-ifc.html)  
- Export parameter mapping table: You can select revit properties to 
  export as standard ifc properties, more info and example config here: [Official help](https://knowledge.autodesk.com/support/revit-products/learn-explore/caas/simplecontent/content/using-parameter-mapping-tables-for-ifc.html)

*This note was based on my comment on Revitforum: [Link](https://revitforum.org/showthread.php/42521-Revit-project-parameters-in-IFC-export-possible?p=227095&viewfull=1#post227095)*
