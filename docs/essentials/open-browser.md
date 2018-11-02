---
title: 'Xamarin.Essentials : Ouvrir le navigateur'
description: La classe Navigateur dans Xamarin.Essentials permet à une application d’ouvrir un lien web dans le navigateur préféré du système optimisé ou le navigateur externe.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a68837ac4447dabcf52a1d1b27913adf80b4cbd7
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675391"
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials : Navigateur

![Préversion NuGet](~/media/shared/pre-release.png)

La classe **Navigateur** permet à une application d’ouvrir un lien web dans le navigateur préféré du système optimisé ou le navigateur externe.

## <a name="get-started"></a>Prise en main

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-browser"></a>Utilisation de Navigateur

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité Navigateur fonctionne en appelant la méthode `OpenAsync` avec `Uri` et `BrowserLaunchMode`.

```csharp

public class BrowserTest
{
    public async Task OpenBrowser(Uri uri)
    {
        await Browser.OpenAsync(uri, BrowserLaunchMode.SystemPreferred);
    }
}
```

## <a name="platform-implementation-specifics"></a>Caractéristiques de mise en œuvre de la plateforme

# <a name="androidtabandroid"></a>[Android](#tab/android)

Le Mode de lancement détermine la façon dont le navigateur est lancé :

## <a name="system-preferred"></a>Système par défaut

L’utilisation de [Onglets Chrome personnalisés](https://developer.chrome.com/multidevice/android/customtabs) sera tentée pour charger l’URI et conserver la sensibilisation à la navigation.

## <a name="external"></a>Ressource externe

Un `Intent` sera utilisé pour demander l’ouverture de l’URI via le navigateur normal du système.

# <a name="iostabios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>Système par défaut

[SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) est utilisé pour charger l’URI et conserver la sensibilisation à la navigation.

## <a name="external"></a>Ressource externe

`OpenUrl` standard sur l’application principale est utilisé pour lancer le navigateur par défaut en dehors de l’application.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Le navigateur par défaut de l’utilisateur sera toujours exécuté, quel que soit `BrowserLaunchMode`.

--------------

## <a name="api"></a>API

- [Code source de Navigateur](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [Documentation de l’API Navigateur](xref:Xamarin.Essentials.Browser)
