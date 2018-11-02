---
title: Xamarin.Essentials Cartes
description: La classe Cartes dans Xamarin.Essentials permet à une application d’ouvrir l’application de cartes installée à un emplacement spécifique.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
ms.openlocfilehash: fb4cbc2fd334d574abc57a3359fa346bc6795408
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674771"
---
# <a name="xamarinessentials-maps"></a>Xamarin.Essentials : Cartes

![Préversion NuGet](~/media/shared/pre-release.png)

La classe **Cartes** permet à une application d’ouvrir l’application de cartes installée à un emplacement spécifique.

## <a name="get-started"></a>Prise en main

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-maps"></a>Utilisation de Cartes

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité Cartes fonctionne en appelant la méthode `OpenAsync` avec `Location` ou `Placemark` pour l’ouvrir avec `MapsLaunchOptions` facultatif.

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

Lors de l’ouverture avec un `Placemark`, les informations suivantes sont requises :

- `CountryName`
- `AdminArea`
- `Thoroughfare`
- `Locality`

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

## <a name="extension-methods"></a>Méthodes d’extension

Si vous avez déjà une référence à un emplacement `Location` ou à un repère `Placemark`, vous pouvez utiliser la méthode d’extension intégrée `OpenMapsAsync` avec le paramètre facultatif `MapsLaunchOptions`:

```csharp
public class MapsTest
{
    public async Task OpenPlacemarkOnMaps(Placemark placemark)
    {
        await placemark.OpenMapsAsync();
    }
}
```

## <a name="directions-mode"></a>Mode Directions

Si vous appelez `OpenMapsAsync` sans `MapsLaunchOptions`, la carte se lancera à l’emplacement spécifié. Si vous le souhaitez, vous pouvez avoir un itinéraire de navigation calculé à partir de la position actuelle de l’appareil. Pour ce faire, il vous de définir `MapDirectionsMode` sur `MapsLaunchOptions` :

```csharp
public class MapsTest
{
    public async Task NavigateToBuilding25()
    {
        var location = new Location(47.645160, -122.1306032);
        var options =  new MapsLaunchOptions { MapDirectionsMode = MapDirectionsMode.Driving };

        await Maps.OpenAsync(location, options);
    }
}
```

## <a name="platform-differences"></a>Différences entre les plateformes

# <a name="androidtabandroid"></a>[Android](#tab/android)

- `MapDirectionsMode` prend en charge Bicycling (vélo), Driving (voiture) et Walking (marche).

# <a name="iostabios"></a>[iOS](#tab/ios)

- `MapDirectionsMode` prend en charge Bicycling (vélo), Transit et Walking (marche).

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- `MapDirectionsMode` prend en charge Bicycling (vélo), Transit et Walking (marche).

--------------

## <a name="platform-implementation-specifics"></a>Caractéristiques de mise en œuvre de la plateforme

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android utilise le schéma d’URI `geo:` pour lancer l’application Cartes sur l’appareil. L’utilisateur peut être invité à sélectionner à partir d’une application existante qui prend en charge ce schéma d’URI.  Xamarin.Essentials est testé avec Google Maps, qui prend en charge ce schéma.

# <a name="iostabios"></a>[iOS](#tab/ios)

Aucun détail d’implémentation spécifique à la plateforme.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucun détail d’implémentation spécifique à la plateforme.

--------------

## <a name="api"></a>API

- [Code source de Cartes](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Maps)
- [Documentation de l’API Cartes](xref:Xamarin.Essentials.Maps)
