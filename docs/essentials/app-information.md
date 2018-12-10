---
title: 'Xamarin.Essentials : informations sur l’application'
description: Ce document décrit la classe AppInfo de Xamarin.Essentials, qui fournit des informations sur votre application. Par exemple, il expose le nom et la version de l’application.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 3e67b605e485b724ec11f2ac94dcf3d1aa77d5cf
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53057295"
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials : informations sur l’application

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
AppInfo.ShowSettingsUI();
```

Cette page de paramètres permet à l’utilisateur de changer les autorisations de l’application et d’effectuer d’autres tâches spécifiques à la plateforme.

## <a name="platform-implementation-specifics"></a>Caractéristiques de mise en œuvre de la plateforme

# <a name="androidtabandroid"></a>[Android](#tab/android)

Les informations d’application sont tirées de `AndroidManifest.xml` pour les champs suivants :

- **Build** – `android:versionCode` dans le nœud `manifest`
- **Name** - `android:label` dans le nœud `application`
- **PackageName** : `package` dans le nœud `manifest`
- **VersionString** – `android:versionName` dans le nœud `application`

# <a name="iostabios"></a>[iOS](#tab/ios)

Les informations d’application sont tirées de `Info.plist` pour les champs suivants :

- **Build** – `CFBundleVersion`
- **Name** - `CFBundleDisplayName` si défini, sinon `CFBundleName`
- **PackageName** : `CFBundleIdentifier`
- **VersionString** – `CFBundleShortVersionString`

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Les informations d’application sont tirées de `Package.appxmanifest` pour les champs suivants :

- **Build** – Utilise la valeur `Build` de la `Version` sur le nœud `Identity`
- **Name** - `DisplayName` sur le nœud `Properties`
- **PackageName** : `Name` sur le nœud `Identity`
- **VersionString** – `Version` sur le nœud `Identity`


--------------

## <a name="api"></a>API

- [Code source d’AppInfo](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [Documentation sur l’API d’AppInfo](xref:Xamarin.Essentials.AppInfo)
