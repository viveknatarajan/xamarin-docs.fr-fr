---
title: 'Xamarin.Essentials : Informations sur l’application'
description: Ce document décrit la classe AppInfo dans Xamarin.Essentials, qui fournit des informations relatives à votre application. Par exemple, il expose le nom de l’application et la version.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 25765fbbcbd2475bfcbc72ca5c4feefa8ef19d08
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782103"
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials : Informations sur l’application

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

Le **AppInfo** classe fournit des informations sur votre application.

## <a name="using-appinfo"></a>À l’aide de AppInfo

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Les informations suivantes sont exposées via l’API :

```csharp
// Application Name
var appName = AppInfo.Name;

// Package Name/Application Identifier (com.microsoft.testapp)
var packageName = AppInfo.PackageName;

// Application Version (1.0.0)
var version = AppInfo.VersionString;

// Application Build Number (1)
var build = AppInfo.BuildString;
```

## <a name="api"></a>API

- [Code source de AppInfo](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [Documentation de l’API de AppInfo](xref:Xamarin.Essentials.AppInfo)
