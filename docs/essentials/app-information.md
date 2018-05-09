---
title: Informations sur l’application Xamarin.Essentials
description: La classe AppInfo fournit des informations sur votre application.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 5b235fdeb666c3f8f2c1b52a9691c931724ce2b8
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-app-information"></a>Informations sur l’application Xamarin.Essentials

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

- [Code source de AppInfo](https://github.com/xamarin/Essentials/tree/master/Essentials/AppInfo)
- [Documentation de l’API de AppInfo](xref:Xamarin.Essentials.AppInfo)
