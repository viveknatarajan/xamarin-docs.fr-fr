---
title: Géocodage de Xamarin.Essentials
description: La classe géocodage fournit des API pour geocode une placemark à une position coordonnées et inverser geocode coordincates à un placemark.
ms.assetid: 3ADC440C-B000-4708-A2CC-296F5160AF90
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 95301dad847887e867b220997ea9c34dba827982
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="xamarinessentials-geocoding"></a>Géocodage de Xamarin.Essentials

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

Le **géocodage** classe fournit des API pour geocode une placemark à une position coordonnées et inverser geocode coordincates à un placemark.

## <a name="getting-started"></a>Prise en main

Pour accéder à la **géocodage** fonctionnalité de la configuration spécifique plate-forme suivante est requise.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Aucune configuration supplémentaire n’est requise.

# <a name="iostabios"></a>[iOS](#tab/ios)

Aucune configuration supplémentaire n’est requise.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Une clé d’API Bing maps est requise pour utiliser le géocodage funcationality. Inscrivez-vous gratuitement [Bing Maps](https://www.bingmapsportal.com/) compte. Sous **mon compte > Mes clés** créer une nouvelle clé et le remplissage des informations en fonction de votre type d’application.

Dès le début de la durée de vie de votre application avant d’appeler une **géocodage** méthodes définissent la clé d’API :

```csharp
Geocoding.MapKey = "YOUR-KEY-HERE";
```

-----

## <a name="using-geocoding"></a>À l’aide de géocodage

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Mise en route [emplacement](xref:Xamarin.Essentials.Location) coordonnées d’une adresse :

```csharp
try
{
    var address =  "Microsoft Building 25 Redmond WA USA";
    var locations = await Geocoding.GetLocationsAsync(address);

    var location = locations?.FirstOrDefault();
    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Handle exception that may have occured in geocoding
}
```

Mise en route [placemarks](xref:Xamarin.Essentials.Placemark) pour un ensemble existant de coordonnées :

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

## <a name="api"></a>API

- [Code source de géocodage](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geocoding)
- [Documentation de géocodage API](xref:Xamarin.Essentials.Geocoding)
