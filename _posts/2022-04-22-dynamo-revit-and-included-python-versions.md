---
layout: default
categories: revit
tags: dynamo
title: Dynamo, Revit and included Python versions
---

# Dynamo, Revit and included Python versions

Dynamo versions shipped with Revit, and the included Python versions:

| Revit version | Dynamo | IronPython2 | CPython3 |
| ------------- | ------ | ----------- | -------- |
| **2020**      | 2.1    | 2.7.8 💀     | ❌        |
| 2020.1        | 2.2    | 2.7.8 💀     | ❌        |
| 2020.2        | 2.3    | 2.7.8 💀     | ❌        |
| **2021**      | 2.5    | 2.7.9 💀     | ❌        |
| 2021.1        | 2.6    | 2.7.9 💀     | ❌        |
| **2022**      | 2.10   | 2.7.9 💀     | 3.8.3    |
| 2022.1        | 2.12   | 2.7.9 💀     | 3.8.3    |
| **2023**      | 2.13   | ❌           | 3.8.10   |
| 2023.1        | 2.16.1 | ❌           | 3.9.12 ⬆️ |
| **2024**      | 2.17   | ❌           | 3.9.12   |

Legend:

* ❌: Not included
* ⬆️: Minor version upgrade
* 💀: EOL as of the last update of this document


## DynamoIronPython2.7 package

From Dynamo 2.13 Ironpython is not included, but it can be downloaded from the package manager.

IronPython2 versions in th `DynamoIronPython2.7` package:

| Package version | IronPython2 |
| --------------- | ----------- |
| 1.0 - 2.5.0     | 2.7.9       |

## Old versions, before 2020

Before 2020, Dynamo had to be installed manually. Compatible versions are listed in the primer: [The Revit Connection - The Dynamo Primer](https://primer.dynamobim.org/08_Dynamo-for-Revit/8-1_The-Revit-Connection.html)

I copied the table from that site, links point to the same downloads:

| Revit Version | First Stable Dynamo Version                                                       | Last Supported Dynamo for Revit Version                                                                                                                                |
| ------------- | --------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2013          | [0.6.1](http://dyn-builds-data.s3-us-west-2.amazonaws.com/DynamoInstall0.6.1.exe) | [0.6.3](http://dyn-builds-data.s3-us-west-2.amazonaws.com/DynamoInstall0.6.3.exe)                                                                                      |
| 2014          | [0.6.1](http://dyn-builds-data.s3-us-west-2.amazonaws.com/DynamoInstall0.6.1.exe) | [0.8.2](http://dyn-builds-data.s3-us-west-2.amazonaws.com/DynamoInstall0.8.2.exe)                                                                                      |
| 2015          | [0.7.1](http://dyn-builds-data.s3-us-west-2.amazonaws.com/DynamoInstall0.7.1.exe) | [1.2.1](http://dyn-builds-data.s3-us-west-2.amazonaws.com/DynamoInstall1.2.1.exe)                                                                                      |
| 2016          | [0.7.2](http://dyn-builds-data.s3-us-west-2.amazonaws.com/DynamoInstall0.7.2.exe) | [1.3.2](http://dyn-builds-data.s3-us-west-2.amazonaws.com/DynamoInstall1.3.2.exe)                                                                                      |
| 2017          | [0.9.0](http://dyn-builds-data.s3-us-west-2.amazonaws.com/DynamoInstall0.9.0.exe) | [1.3.4](http://dyn-builds-data.s3-us-west-2.amazonaws.com/DynamoInstall1.3.4.exe) / [2.0.3](https://dyn-builds-data.s3-us-west-2.amazonaws.com/DynamoInstall2.0.3.exe) |
| 2018          | [1.3.0](http://dyn-builds-data.s3-us-west-2.amazonaws.com/DynamoInstall1.3.0.exe) | [1.3.4](http://dyn-builds-data.s3-us-west-2.amazonaws.com/DynamoInstall1.3.4.exe) / [2.0.3](https://dyn-builds-data.s3-us-west-2.amazonaws.com/DynamoInstall2.0.3.exe) |
| 2019          | [1.3.3](http://dyn-builds-data.s3-us-west-2.amazonaws.com/DynamoInstall1.3.3.exe) | [1.3.4](http://dyn-builds-data.s3-us-west-2.amazonaws.com/DynamoInstall1.3.4.exe) / [2.0.4](https://dyn-builds-data.s3-us-west-2.amazonaws.com/DynamoInstall2.0.4.exe) |

# Advance Steel

Dynamo is separately downloadable as an extension.

| Advance Steel | Dynamo |
| ------------- | ------ |
| 2019          | 2.0.2  |
| 2020          | 2.1.0  |
| 2021          | 2.5.2  |
| 2022          | 2.10.1 |
| 2023          | 2.13.1 |
