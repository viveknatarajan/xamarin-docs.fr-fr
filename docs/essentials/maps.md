---
title: 'Xamarin.Essentials : Carte'
description: La classe Maps permet à une application d'ouvrir une application cartes installée sur le téléphone à un emplacement spécifique ou sur un placemark.
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
# <a name="xamarinessentials-maps"></a>Xamarin.Essentials : Carte

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **Maps** permet à une application d'ouvrir une application cartes installée sur le téléphone à un emplacement spécifique ou sur un placemark.

## <a name="using-maps"></a>Utilisation de **Maps**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Pour ouvrir la carte, passez un objet `Location` ou `Placemark` à la méthode `OpenAsync`. Le paramètre facultatif `MapsLaunchOptions` permet d'ajouter des précisions à la requête.

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

## <a name="extension-methods"></a>Méthode d’extension

Si vous avez déjà une référence à un `Location` ou `Placemark` vous pouvez utiliser la méthode d’extension intégrée `OpenMapsAsync` avec le paramètre facultatif `MapsLaunchOptions`:

```csharp
public class MapsTest
{
    public async Task OpenPlacemarkOnMaps(Placemark placemark)
    {
        await placemark.OpenMapsAsync();
    }
}
```

## <a name="platform-differences"></a>Différences entre les plateformes

# <a name="androidtabandroid"></a>[Android](#tab/android)

* `MapDirectionsMode` n’est pas pris en charge et n’a aucun effet.

# <a name="iostabios"></a>[iOS](#tab/ios)

* `MapDirectionsMode` est pris en charge pour définir le mode de la direction par défaut lorsque l’application maps est ouvert.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* `MapDirectionsMode` n’est pas pris en charge et n’a aucun effet.

--------------

## <a name="platform-implementation-specifics"></a>Implémentations spécifiques par plateforme

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android utilise le schéma d’Uri `geo:` pour lancer l’application maps sur l’appareil. Cela peut inviter l’utilisateur à sélectionner une application parmis les applications de carte qui prend en charge ce schéma d’Uri. Xamarin.Essentials est testé avec Google Maps, qui prend en charge de ce schéma.

# <a name="iostabios"></a>[iOS](#tab/ios)

Aucun détail d’implémentation spécifique de plate-forme.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucun détail d’implémentation spécifique de plate-forme.

--------------

## <a name="api"></a>API

- [Cartes de code source](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Maps)
- [Documentation sur Maps API](xref:Xamarin.Essentials.Maps)
