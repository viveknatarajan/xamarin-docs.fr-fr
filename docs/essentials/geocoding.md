---
title: 'Xamarin.Essentials : Géocodage'
description: La classe Geocoding fournit les API permettant de transformer des adresses physiques de lieux _(placemark)_ en coordonnées géographiques et vice versa.
ms.assetid: 3ADC440C-B000-4708-A2CC-296F5160AF90
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a4d6e4d9b32e665893d82693a3c858630b63d372
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353673"
---
# <a name="xamarinessentials-geocoding"></a>Xamarin.Essentials : Géocodage

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **Geocoding** fournit les API permettant de transformer des adresses physiques de lieux _(placemark)_ en coordonnées géographiques et vice versa.

## <a name="getting-started"></a>Prise en main

Pour accéder aux fonctionnalités de l'API **Geocoding** quelques étapes de configurations spécifiques aux plateformes sont nécessaires.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Aucune configuration supplémentaire n’est requise.

# <a name="iostabios"></a>[iOS](#tab/ios)

Aucune configuration supplémentaire n’est requise.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Une clé d'API Bing Maps est requise pour utiliser la fonctionnalité de géocodage. Inscrivez-vous sur [Bing Maps](https://www.bingmapsportal.com/) et créez un compte. Sous **mon compte > Mes clés** vous pouvez créer une nouvelle clé en renseignant des informations sur votre application (qui doit être une **application publique Windows (UWP, 8.x et les versions antérieures)**).

Aussi tôt que possible dans l'execution de votre application, avant d'appler une autre fonction de `Geocoding` renseignez votre clé d'API comme ceci :

```csharp
Geocoding.MapKey = "YOUR-KEY-HERE";
```

-----

## <a name="using-geocoding"></a>Utilisation de **Geocoding**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Obtenir les [coordonnées géographiques](xref:Xamarin.Essentials.Location) d’une adresse :

```csharp
try
{
    var address =  "Microsoft Building 25 Redmond WA USA";
    var locations = await Geocoding.GetLocationsAsync(address);

    var location = locations?.FirstOrDefault();
    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Handle exception that may have occurred in geocoding
}
```

L’altitude n’est pas toujours disponible. Si elle n’est pas disponible, la propriété `Altitude` pourra être soit `null`, soit zéro. Si l’altitude est disponible, la valeur sera exprimée en mètres par rapport au plus haut niveau de la mer.

Obtenir l'adresse ([placemark](xref:Xamarin.Essentials.Placemark)) à partir de coordonnées :

```csharp
try
{
    var lat = 47.673988;
    var lon = -122.121513;

    var placemarks = await Geocoding.GetPlacemarksAsync(lat, lon);

    var placemark = placemarks?.FirstOrDefault();
    if (placemark != null)
    {
        var geocodeAddress =
            $"AdminArea:       {placemark.AdminArea}\n" +
            $"CountryCode:     {placemark.CountryCode}\n" +
            $"CountryName:     {placemark.CountryName}\n" +
            $"FeatureName:     {placemark.FeatureName}\n" +
            $"Locality:        {placemark.Locality}\n" +
            $"PostalCode:      {placemark.PostalCode}\n" +
            $"SubAdminArea:    {placemark.SubAdminArea}\n" +
            $"SubLocality:     {placemark.SubLocality}\n" +
            $"SubThoroughfare: {placemark.SubThoroughfare}\n" +
            $"Thoroughfare:    {placemark.Thoroughfare}\n";

        Console.WriteLine(geocodeAddress);
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Handle exception that may have occurred in geocoding
}
```

## <a name="distance-between-two-locations"></a>Distance entre deux emplacements

Les classes [`Location`](xref:Xamarin.Essentials.Location) et [`LocationExtensions`](xref:Xamarin.Essentials.LocationExtensions) définissent les méthodes permettant de calculer la distance entre deux emplacements géographiques. Consultez l’article [**Xamarin.Essentials : Géolocalisation**](geolocation.md#distance-between-two-locations) pour obtenir un exemple.

## <a name="api"></a>API

- [Code source de géocodage](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geocoding)
- [Documentation de l’API de géocodage](xref:Xamarin.Essentials.Geocoding)
