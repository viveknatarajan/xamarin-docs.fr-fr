---
title: Mappages de Xamarin.Essentials
description: La classe de mappages dans Xamarin.Essentials permet à une application ouvrir l’application de cartes installées à un emplacement spécifique ou à placemark.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
ms.openlocfilehash: 445e2da84e9a9aaf1ce4d836af11cfba963b8cbb
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353933"
---
# <a name="xamarinessentials-maps"></a>Xamarin.Essentials : mappages

![Version préliminaire NuGet](~/media/shared/pre-release.png)

Le **mappe** classe permet à une application ouvrir l’application de cartes installées à un emplacement spécifique ou à placemark.

## <a name="using-maps"></a>À l’aide de cartes

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité de mappages fonctionne en appelant le `OpenAsync` méthode avec le `Location` ou `Placemark` ouvrir avec facultatif `MapsLaunchOptions`.

```csharp
public class MapsTest
{
    public async Task NavigateToBuilding25()
    {
        var location = new Location(47.645160, -122.1306032);
        var options =  new MapsLaunchOptions { Name = "Microsoft Building 25" };

        await Maps.OpenAsync(location, options);
    }
}
```

Lors de l’ouverture avec un `Placemark` les informations suivantes sont requises :

* `CountryName`
* `AdminArea`
* `Thoroughfare`
* `Locality`

```csharp
public class MapsTest
{
    public async Task NavigateToBuilding25()
    {
        var placemark = new Placemark
            {
                CountryName = "United States",
                AdminArea = "WA",
                Thoroughfare = "Microsoft Building 25",
                Locality = "Redmond"
            };
        var options =  new MapsLaunchOptions { Name = "Microsoft Building 25" };

        await Maps.OpenAsync(placemark, options);
    }
}
```

## <a name="extension-methods"></a>méthodes d’extension.

Si vous avez déjà une référence à un `Location` ou `Placemark` vous pouvez utiliser la méthode d’extension intégrés `OpenMapsAsync` avec facultatif `MapsLaunchOptions`:

```csharp
public class MapsTest
{
    public async Task OpenPlacemarkOnMaps(Placemark placemark)
    {
        await placemark.OpenMapsAsync();
    }
}
```

## <a name="platform-differences"></a>Différences de plateforme

# <a name="androidtabandroid"></a>[Android](#tab/android)

* `MapDirectionsMode` n’est pas pris en charge et n’a aucun effet.

# <a name="iostabios"></a>[iOS](#tab/ios)

* `MapDirectionsMode` est pris en charge pour définir le mode de la direction par défaut lorsque l’application maps est ouvert.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* `MapDirectionsMode` n’est pas pris en charge et n’a aucun effet.

--------------

## <a name="platform-implementation-specifics"></a>Caractéristiques de mise en œuvre la plateforme

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android utilise le `geo:` schéma d’Uri pour lancer l’application maps sur l’appareil. Cela peut inviter l’utilisateur à sélectionner à partir d’une application existante qui prend en charge de ce schéma d’Uri.  Xamarin.Essentials est testé avec Google Maps, qui prend en charge de ce schéma.

# <a name="iostabios"></a>[iOS](#tab/ios)

Aucun détail d’implémentation spécifique de plate-forme.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucun détail d’implémentation spécifique de plate-forme.

--------------

## <a name="api"></a>API

- [Cartes de code source](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Maps)
- [Documentation sur Maps API](xref:Xamarin.Essentials.Maps)
