---
title: 'Xamarin.Essentials : Lanceur Inter-app'
description: La classe Lanceur dans Xamarin.Essentials permet à une application d’ouvrir un URI par le système.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 502ccaf8ef4fbeadb4b46f47668ac11f2747b89d
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898267"
---
# <a name="xamarinessentials-launcher"></a>Xamarin.Essentials : Lanceur

La classe **Lanceur** permet à une application d’ouvrir un URI par le système. Elle est souvent utilisée en cas de lien profond avec les schémas d’URI personnalisés d’une autre application. Si vous souhaitez ouvrir le navigateur sur un site web, reportez-vous à l’API **[Navigateur](open-browser.md)**.

## <a name="get-started"></a>Prise en main

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-launcher"></a>Utilisation de Lanceur

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Pour utiliser la fonctionnalité Lanceur, appelez la méthode `OpenAsync` et passez un `string` ou `Uri` pour l’ouvrir. Si vous le souhaitez, vous pouvez aussi utiliser la méthode `CanOpenAsync` pour vérifier si le schéma d’URI peut être géré par une application sur l’appareil.

```csharp
public class LauncherTest
{
    public async Task OpenRideShareAsync()
    {
        var supportsUri = await Launcher.CanOpenAsync("lyft://");
        if (supportsUri)
            await Launcher.OpenAsync("lyft://ridetype?id=lyft_line");
    }
}
```

## <a name="platform-differences"></a>Différences entre les plateformes

# <a name="androidtabandroid"></a>[Android](#tab/android)

La tâche retournée à partir de `CanOpenAsync` se termine immédiatement.

# <a name="iostabios"></a>[iOS](#tab/ios)

Si l’application de destination sur cet appareil n’a jamais été ouverte par `OpenAsync` à partir de votre application auparavant, iOS invite l’utilisateur une fois à autoriser votre application à l’ouvrir.

La tâche retournée à partir de `CanOpenAsync` se termine immédiatement.

Plus d’informations sur l’implémentation d’iOS sont disponibles [ici](https://developer.xamarin.com/api/member/UIKit.UIApplication.CanOpenUrl/p/Foundation.NSUrl/)

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucune différence entre les plateformes.

-----

## <a name="api"></a>API

- [Code source de Lanceur](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Launcher)
- [Documentation de l’API Lanceur](xref:Xamarin.Essentials.Launcher)
