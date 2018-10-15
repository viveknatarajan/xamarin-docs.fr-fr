---
title: 'Xamarin.Essentials : Géocodage'
description: La classe de géocodage dans Xamarin.Essentials fournit des API pour les deux Géocode un placemark un coordonnées positionnels et inverser des coordonnées de Géocode vers un placemark.
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

La classe **géocodage** fournit des API aux Géocode un placemark un coordonnées positionnels et inverser des coordonnées de Géocode vers un placemark.

## <a name="getting-started"></a>Prise en main

Pour accéder aux fonctionnalités de l'API **géocodage** quelques étapes de configuration spécifiques à la plate-forme sont nécessaires.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Aucune configuration supplémentaire n’est requis.

# <a name="iostabios"></a>[iOS](#tab/ios)

Aucune configuration supplémentaire n’est requis.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Une clé API Bing Maps est requise pour utiliser la fonctionnalité de géocodage. Inscrivez-vous pour bénéficier d’un [Bing Maps](https://www.bingmapsportal.com/) compte. Sous **mon compte > Mes clés** créer une nouvelle clé et le remplissage des informations en fonction de votre type d’application (qui doit être **publique application Windows (UWP, 8.x et les versions antérieures)** pour les applications UWP).

Très tôt dans la vie de votre application avant d’appeler une **géocodage** méthodes définissent la clé d’API :

```csharp
Geocoding.MapKey = "YOUR-KEY-HERE";
```

-----

## <a name="using-geocoding"></a>Utilisation de **Geocoding**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Obtention de [emplacement](xref:Xamarin.Essentials.Location) coordonnées d’une adresse :

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

L’altitude n’est pas toujours disponible. S’il n’est pas disponible, le `Altitude` propriété peut être `null` ou la valeur peut être zéro. Si l’altitude est disponible, la valeur est en mètres au-dessus de plus haut niveau de la mer.

Obtention de [placemarks](xref:Xamarin.Essentials.Placemark) pour un ensemble existant de coordonnées :

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

Le [ `Location` ](xref:Xamarin.Essentials.Location) et [ `LocationExtensions` ](xref:Xamarin.Essentials.LocationExtensions) classes définissent les méthodes pour calculer la distance entre deux emplacements. Consultez l’article [ **Xamarin.Essentials : géolocalisation** ](geolocation.md#calculate-distance) pour obtenir un exemple.

## <a name="api"></a>API

- [Code source de géocodage](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geocoding)
- [Documentation de l’API de géocodage](xref:Xamarin.Essentials.Geocoding)
