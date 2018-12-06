---
title: Carte Xamarin.Essentials
description: La classe Map dans Xamarin.Essentials permet à une application d’ouvrir l’application de cartes installée à un emplacement spécifique.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 9797244a9f89d0658b65b132eaf541ed763be97b
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898964"
---
# <a name="xamarinessentials-map"></a>Xamarin.Essentials : Carte

La classe **Map** permet à une application d’ouvrir l’application de cartes installée à un emplacement spécifique.

## <a name="get-started"></a>Prise en main

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-map"></a>Utilisation de Map

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité Carte appelle la méthode `OpenAsync` avec le `Location` ou `Placemark` à ouvrir avec des `MapLaunchOptions` facultatif.

```csharp
public class MapTest
{
    public async Task NavigateToBuilding25()
    {
        var location = new Location(47.645160, -122.1306032);
        var options =  new MapLaunchOptions { Name = "Microsoft Building 25" };

        await Map.OpenAsync(location, options);
    }
}
```

Lors de l’ouverture avec un `Placemark`, les informations suivantes sont requises :

- `CountryName`
- `AdminArea`
- `Thoroughfare`
- `Locality`

```csharp
public class MapTest
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
        var options =  new MapLaunchOptions { Name = "Microsoft Building 25" };

        await Map.OpenAsync(placemark, options);
    }
}
```

## <a name="extension-methods"></a>Méthodes d’extension

Si vous avez déjà une référence à un emplacement `Location` ou à un repère `Placemark`, vous pouvez utiliser la méthode d’extension intégrée `OpenMapAsync` avec le paramètre facultatif `MapLaunchOptions`:

```csharp
public class MapTest
{
    public async Task OpenPlacemarkOnMap(Placemark placemark)
    {
        await placemark.OpenMapAsync();
    }
}
```

## <a name="directions-mode"></a>Mode Directions

Si vous appelez `OpenMapAsync` sans `MapLaunchOptions`, la carte se lancera à l’emplacement spécifié. Si vous le souhaitez, vous pouvez avoir un itinéraire de navigation calculé à partir de la position actuelle de l’appareil. Pour ce faire, il vous de définir `NavigationMode` sur `MapLaunchOptions` :

```csharp
public class MapTest
{
    public async Task NavigateToBuilding25()
    {
        var location = new Location(47.645160, -122.1306032);
        var options =  new MapLaunchOptions { NavigationMode = NavigationMode.Driving };

        await Map.OpenAsync(location, options);
    }
}
```

## <a name="platform-differences"></a>Différences entre les plateformes

# <a name="androidtabandroid"></a>[Android](#tab/android)

- `NavigationMode` prend en charge Bicycling (vélo), Driving (voiture) et Walking (marche).

# <a name="iostabios"></a>[iOS](#tab/ios)

- `NavigationMode` prend en charge Bicycling (vélo), Transit et Walking (marche).

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- `NavigationMode` prend en charge Bicycling (vélo), Transit et Walking (marche).

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

- [Code source de Map](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Map)
- [Documentation de l’API Map](xref:Xamarin.Essentials.Map)
