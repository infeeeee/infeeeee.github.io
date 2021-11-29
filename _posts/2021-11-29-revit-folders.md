---
layout: default
categories: revit
tags: installation
title: Revit folders
---

# Revit folders

I'm trying to collect all (most) Revit related folders, so it would be easier to clean up things. I will try to add new folders as I have some problems with them.

❗ means folders which can grow huge, but can be deleted safely. Keep an eye on this folders

## Variables

`%userprofile%` = `C:\Users\Username`

`%appdata%` = `C:\Users\Username\AppData\Roaming\`

`%localappdata%` =  `C:\Users\Username\AppData\Local`

`[year]`= I use this as a placeholder for different folders for Revit years. Replace it with the Revit version.

## Userprofile dirs

`%userprofile%\Autodesk`: Empty...

`%userprofile%\ACCDocs`: Autodesk Docs local

`%userprofile%\ADrive`: Autodesk Drive local

## Appdata

`%appdata%\Autodesk\Revit\Autodesk Revit [year]`: Revit.ini and config files.

## LocalAppdata

`%localappdata%\Autodesk`: A lot of things, most important ones:

❗ `%localappdata%\Autodesk\Revit\PacCache`: Revit Accelerator cache. 

❗ `%localappdata%\Autodesk\Revit\Autodesk Revit [year]\CollaborationCache`: The collboration cache, BIM360 models will be saved here. 

`%localappdata%\Autodesk\Revit\Autodesk Revit [year]\Journals`: The logs