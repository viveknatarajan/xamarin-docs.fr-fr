---
title: Informations sur l’application Xamarin.Essentials
description: La classe AppInfo fournit des informations sur votre application.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 32e3eb8fab719540e4c9ffec4e57f5510c10e3f5
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
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
