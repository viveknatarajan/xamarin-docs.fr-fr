---
title: 'Xamarin.Essentials : informations sur l’application'
description: Ce document décrit la classe AppInfo de Xamarin.Essentials, qui fournit des informations sur votre application. Par exemple, il expose le nom et la version de l’application.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 00419fb746609464b49be343938905614c59ab29
ms.sourcegitcommit: 704d4cfd418c17b0e85a20c33a16d2419db0be71
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51691761"
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials : informations sur l’application

![Préversion NuGet](~/media/shared/pre-release.png)

La classe **AppInfo** fournit des informations sur votre application.

## <a name="get-started"></a>Prise en main

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-appinfo"></a>Utilisation d’AppInfo

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

## <a name="obtaining-application-information"></a>Obtention d’informations sur l’application :

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

## <a name="displaying-application-settings"></a>Affichage des paramètres de l’application

La classe **AppInfo** peut également afficher une page de paramètres gérés par le système d’exploitation pour l’application :

```csharp
// Display settings page
AppInfo.OpenSettings();
```

Cette page de paramètres permet à l’utilisateur de changer les autorisations de l’application et d’effectuer d’autres tâches spécifiques à la plateforme.

## <a name="api"></a>API

- [Code source d’AppInfo](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [Documentation sur l’API d’AppInfo](xref:Xamarin.Essentials.AppInfo)
