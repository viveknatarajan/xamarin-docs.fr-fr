---
title: 'Xamarin.Essentials: Géocodage'
description: La classe Geocoding de Xamarin.Essentials fournit des API qui permettent à la fois de géocoder un repère en coordonnées de position, et d’inverser les coordonnées de géocodage en un repère.
ms.assetid: 3ADC440C-B000-4708-A2CC-296F5160AF90
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: bc08554f0060ca5de8d7ffc90ea5c65463b93f3b
ms.sourcegitcommit: 97dca3face7c4ad5555dfaca88f5b45a70ca556d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57972596"
---
# <a name="xamarinessentials-geocoding"></a>Xamarin.Essentials: Géocodage

La classe **Geocoding** fournit des API qui permettent de géocoder un repère en coordonnées de position, et d’inverser les coordonnées de géocodage en un repère.

## <a name="get-started"></a>Prise en main

[!include[](~/essentials/includes/get-started.md)]

Pour accéder à la fonctionnalité de **géocodage**, la configuration suivante spécifique à la plateforme est obligatoire.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Aucune configuration supplémentaire n’est requise.

# <a name="iostabios"></a>[iOS](#tab/ios)

Aucune configuration supplémentaire n’est requise.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Une clé API Bing Maps est nécessaire pour utiliser la fonctionnalité de géocodage. Inscrivez-vous pour obtenir un compte [Bing Maps](https://www.bingmapsportal.com/) gratuit. Sous **Mon compte > Mes clés**, créez une clé, puis indiquez les informations nécessaires en fonction de votre type d’application (qui doit être **Application Windows publique (UWP, 8.x et versions antérieures)** pour les applications UWP).

Très tôt dans la vie de votre application, avant d’appeler une méthode de **géocodage**, définissez la clé API (uniquement disponible sur UWP) :

```csharp
Platform.MapServiceToken = "YOUR-KEY-HERE";
```

-----

## <a name="using-geocoding"></a>Utilisation du géocodage

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Obtention des coordonnées de [localisation](xref:Xamarin.Essentials.Location) d’une adresse :

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

L’altitude n’est pas toujours disponible. Dans ce cas, la propriété `Altitude` peut avoir une valeur `null` ou égale à zéro. Si l’altitude est disponible, la valeur est exprimée en mètres au-dessus du niveau de la mer.

## <a name="using-reverse-geocoding"></a>Utilisation du géocodage inversé

Le géocodage inversé est le processus d’obtention de [repères](xref:Xamarin.Essentials.Placemark) pour un ensemble existant de coordonnées :

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

Les classes [`Location`](xref:Xamarin.Essentials.Location) et [`LocationExtensions`](xref:Xamarin.Essentials.LocationExtensions) définissent des méthodes qui permettent de calculer la distance entre deux emplacements. Consultez l’article [**Xamarin.Essentials : Géolocalisation**](geolocation.md#calculate-distance) pour obtenir un exemple.

## <a name="api"></a>API

- [Code source de la fonctionnalité de géocodage](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geocoding)
- [Documentation sur l’API de géocodage](xref:Xamarin.Essentials.Geocoding)
