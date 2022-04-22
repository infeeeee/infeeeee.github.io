---
layout: default
categories: revit
tags: dynamo
title: List all updaters
---

# List all updaters

## About updaters

Updaters can 'hook' to Revit events and can run things automatically when the user does something.

The UpdaterRegistry stores all currently registered updaters. `UpdaterRegistry Class` from [Revit Api Docs](https://www.revitapidocs.com/2022/4f24f516-5274-1420-f255-458c0af5d318.htm):

> The registry is an application-wide singleton. It maintains all dynamic updaters currently registered, and also invokes them per their respective trigger condition during subsequent transactions.
> Please note that only the application (an add-in, typically) which registered an updater is allowed to modify it later, including unregistering it. Also, an application is not allowed to register an updater with an Id, that is based on another application's Id.

More info about updaters on BuildingCoder: [The Building Coder: About the Author 5.31](https://thebuildingcoder.typepad.com/blog/about-the-author.html#5.31)

## List updaters with Dynamo

The original question was, if addins can slow down you Revit session, and my first idea was that with updaters they can. So you can check which addins have an updater registered, and you can disable or remove them to test if they help.

I just made this small script to list all of them:

![DynamoUpdaters](/assets/images/dynamo-updaters.png)

The code:

```python
import clr

# Import RevitAPI
clr.AddReference("RevitAPI")
from Autodesk.Revit.DB import UpdaterRegistry 

# Import DocumentManager
clr.AddReference("RevitServices")
from RevitServices.Persistence import DocumentManager

doc = DocumentManager.Instance.CurrentDBDocument

app_updaters = UpdaterRegistry.GetRegisteredUpdaterInfos()
doc_updaters = UpdaterRegistry.GetRegisteredUpdaterInfos(doc)

def list_updaterinfo(updaters):
	updaterdata = []
	for updater in updaters:
		updaterdata.append([updater.UpdaterName, updater.ApplicationName, updater.AdditionalInformation])
	return updaterdata

output = []
for updaters in [app_updaters, doc_updaters]:
	output.append(list_updaterinfo(updaters))

OUT = output
```

As visible on the image, the first sublist in OUT is the application wide updaters, and second is the updaters for the current document.

It should work with python3, though I haven't tried it.