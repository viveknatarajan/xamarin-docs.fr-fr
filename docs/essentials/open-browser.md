---
title: 'Xamarin.Essentials : Ouvrir le navigateur'
description: La classe Navigateur dans Xamarin.Essentials permet à une application d’ouvrir un lien web dans le navigateur préféré du système optimisé ou le navigateur externe.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: ea2a10c11a77fcb2b3ce142d176522ebf0310725
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898870"
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials : Navigateur

La classe **Browser** permet à une application d’ouvrir un lien web dans le navigateur préféré du système optimisé ou le navigateur externe.

## <a name="get-started"></a>Prise en main

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-browser"></a>Utilisation de Browser

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité Browser fonctionne en appelant la méthode `OpenAsync` avec `Uri` et `BrowserLaunchMode`.

```csharp

public class BrowserTest
{
    public async Task<bool> OpenBrowser(Uri uri)
    {
        return await Browser.OpenAsync(uri, BrowserLaunchMode.SystemPreferred);
    }
}
```

Cette méthode retourne des résultats une fois que le navigateur a été _lancé_ et pas nécessairement quand il est _fermé_ par l’utilisateur.  Le résultat `bool` indique si le lancement a réussi ou non.

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

La méthode standard `OpenUrl` de l'application principale est utilisée pour lancer le navigateur par défaut en dehors de l'application.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Le navigateur par défaut de l’utilisateur sera toujours exécuté, quel que soit `BrowserLaunchMode`.

--------------

## <a name="api"></a>API

- [Code source de Browser](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [Documentation de l’API Browser](xref:Xamarin.Essentials.Browser)
