---
title: Navigateur de Xamarin.Essentials Open
description: La classe de navigateur dans Xamarin.Essentials permet à une application ouvrir un lien web dans le navigateur par défaut du système optimisé ou un navigateur externe.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 563d3899cffb80c0215d90e8e4392046c4635256
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815704"
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials : navigateur

![Version préliminaire NuGet](~/media/shared/pre-release.png)

Le **navigateur** classe permet à une application ouvrir un lien web dans le navigateur par défaut du système optimisé ou un navigateur externe.

## <a name="using-browser"></a>À l’aide du navigateur

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité de navigateur fonctionne en appelant le `OpenAsync` méthode avec le `Uri` et `BrowserLaunchType`.

```csharp

public class BrowserTest
{
    public async Task OpenBrowser(Uri uri)
    {
        await Browser.OpenAsync(uri, BrowserLaunchType.SystemPreferred);
    }
}
```

## <a name="platform-implementation-specifics"></a>Caractéristiques de mise en œuvre la plateforme

# <a name="androidtabandroid"></a>[Android](#tab/android)

Le Type de lancement détermine la façon dont le navigateur est lancé :

## <a name="system-preferred"></a>Système par défaut

[Chrome des onglets personnalisés](https://developer.chrome.com/multidevice/android/customtabs) sera tentée à utiliser l’Uri de charger et conserver la sensibilisation à la navigation.

## <a name="external"></a>Ressource externe

Un `Intent` permet de demander l’Uri d’ouvrir via le navigateur normal de systèmes.

# <a name="iostabios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>Système par défaut

[SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) permet de charger l’Uri et de conserver la sensibilisation à la navigation.

## <a name="external"></a>Ressource externe

La norme `OpenUrl` sur l’application principale est utilisé pour lancer le navigateur par défaut en dehors de l’application.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Navigateur par défaut de l’utilisateur sera toujours exécuté, quel que soit le `BrowserLaunchType`.

--------------

## <a name="api"></a>API

- [Code source du navigateur](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [Documentation du navigateur d’API](xref:Xamarin.Essentials.Browser)
