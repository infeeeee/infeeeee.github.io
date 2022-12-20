---
layout: default
categories: archicad
tags: troubleshooting
title: Bimcloud Basic notes
---

# Bimcloud Basic notes

Bimcloud Basic, formerly Bimserver is the on prem installable teamwork solution by Graphisoft.

The software is available for Windows and Mac, here I write only Windows specific things.

## Installation folder

Bimcloud Basic Server is installed to the following folder by default:

```
C:\Program Files\GRAPHISOFT\BIMcloud\Server-YYYY-MM-DD
```

Where `YYYY-MM-DD` is the [ISO8601](https://en.wikipedia.org/wiki/ISO_8601) formatted date of installation

## Safely removable files

Sometimes the installation folders become huge. The following folders can be safely deleted from there:

- `BlobCache`: Some cache. This folder can grow awfully huge. [Source](https://community.graphisoft.com/t5/Teamwork-articles/BIMcloud-Troubleshooting-Guide/ta-p/303381)
- `Mailboxes\Mailboxes.db`: Contains messages between users. [Source](https://community.graphisoft.com/t5/Teamwork-articles/BIMcloud-Troubleshooting-Guide/ta-p/303381)
- `Temp`: BIMprojecct exports from the webui use this panel. Even though it's called temp, it isn't cleared at restart (But it is cleared somtime) [Source](https://community.graphisoft.com/t5/Collaborate-forum/BIM-Server-Temp-ObjectContents-directory-became-too-large/m-p/210609)
