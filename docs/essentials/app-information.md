---
title: 'Xamarin.Essentials : Informations sur l’application'
description: Ce document décrit la classe AppInfo dans Xamarin.Essentials, qui fournit des informations relatives à votre application. Par exemple, il expose le nom de l’application et la version.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 7e79b3003f41b8de22950624e44e8c9e0e7e7e31
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080272"
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials : Informations sur l’application

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

Le **AppInfo** classe fournit des informations sur votre application.

## <a name="using-appinfo"></a>À l’aide de AppInfo

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

## <a name="obtaining-application-information"></a>Obtention d’informations sur l’Application :

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

## <a name="displaying-application-settings"></a>Affichage des paramètres d’Application

Le **AppInfo** classe peut également afficher une page de paramètres gérés par le système d’exploitation de l’application :

```csharp
// Display settings page
AppInfo.OpenSettings();
```

Cette page de paramètres permet à l’utilisateur de modifier les autorisations d’application et effectuer d’autres tâches spécifiques à la plateforme.

## <a name="api"></a>API

- [Code source de AppInfo](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [Documentation de l’API de AppInfo](xref:Xamarin.Essentials.AppInfo)
