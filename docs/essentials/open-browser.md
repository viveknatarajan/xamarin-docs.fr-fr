---
title: 'Xamarin.Essentials : Navigateur internet'
description: La classe Browser permet à une application d'ouvrir un lien web dans un navigateur installé sur le téléphone.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 7e58d439f5a6eaafe9b1b5e7ca874a986e468cb9
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353280"
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials : Navigateur internet

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **Browser** permet à une application d'ouvrir un lien web dans un navigateur installé sur le téléphone.

## <a name="using-browser"></a>Utilisation de **Browser**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Pour ouvrir le navigateur, passez un objet de type `Uri` et un `BrowserLaunchMode` à la méthode `OpenAsync`.

```csharp
public class BrowserTest
{
    public async Task OpenBrowser(Uri uri)
    {
        await Browser.OpenAsync(uri, BrowserLaunchMode.SystemPreferred);
    }
}
```

## <a name="platform-implementation-specifics"></a>Implémentations spécifiques par plateforme

# <a name="androidtabandroid"></a>[Android](#tab/android)

Le Mode de lancement détermine la façon dont le navigateur est lancé :

## <a name="system-preferred"></a>SystemPreferred

[Un onglet personnalisé Chrome](https://developer.chrome.com/multidevice/android/customtabs) sera utilisé pour charger l'Url.

## <a name="external"></a>External

Un `Intent` demandant quel navigateur, parmi les navigateurs présents sur le système, utiliser.

# <a name="iostabios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>SystemPreferred

[SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) sera utilisé pour charger l'Url.

## <a name="external"></a>External

La norme `OpenUrl` sera utilisé pour lancer le navigateur par défaut en dehors de l’application.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Le navigateur par défaut de l’utilisateur sera toujours exécuté, quel que soit le `BrowserLaunchMode`.

--------------

## <a name="api"></a>API

- [Code source du navigateur](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [Documentation du navigateur d’API](xref:Xamarin.Essentials.Browser)
