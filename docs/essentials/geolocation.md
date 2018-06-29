---
title: 'Xamarin.Essentials : géolocalisation'
description: Ce document décrit la classe de géolocalisation dans Xamarin.Essentials, qui fournit des API pour récupérer les coordonnées de géolocalisation du périphérique en cours.
ms.assetid: 8F66092C-13F0-4FEE-8AA5-901D5F79B357
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 11749107403fc99e1d49b63ee3b50ff105abaa57
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080285"
---
# <a name="xamarinessentials-geolocation"></a>Xamarin.Essentials : géolocalisation

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

Le **géolocalisation** classe fournit des API pour récupérer les coordonnées de géolocalisation du périphérique en cours.

## <a name="getting-started"></a>Prise en main

Pour accéder à la **géolocalisation** fonctionnalités, le programme d’installation spécifique à la plateforme suivante est requise :

# <a name="androidtabandroid"></a>[Android](#tab/android)

Autorisations épais et l’emplacement de fin sont requises et doivent être configurées dans le projet Android. En outre, si votre application cible Android 5.0 (niveau 21 d’API) ou une version ultérieure, vous devez déclarer que votre application utilise les fonctionnalités du matériel dans le fichier manifeste. Il peut être ajouté comme suit :

Ouvrez le **AssemblyInfo.cs** de fichiers sous le **propriétés** dossier et ajouter :

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessCoarseLocation)]
[assembly: UsesPermission(Android.Manifest.Permission.AccessFineLocation)]
[assembly: UsesFeature("android.hardware.location", Required = false)]
[assembly: UsesFeature("android.hardware.location.gps", Required = false)]
[assembly: UsesFeature("android.hardware.location.network", Required = false)]
```

Ou mettre à jour le manifeste Android :

Ouvrez le **AndroidManifest.xml** fichier sous le **propriétés** dossier et ajoutez le code suivant à l’intérieur de la **manifeste** nœud :

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-feature android:name="android.hardware.location" android:required="false" />
<uses-feature android:name="android.hardware.location.gps" android:required="false" />
<uses-feature android:name="android.hardware.location.network" android:required="false" />
```

Ou avec le bouton droit sur le projet Android et ouvrez les propriétés du projet. Sous **manifeste Android** de trouver la **les autorisations requises :** zone et vérifiez la **ACCESS_COARSE_LOCATION** et **ACCESS_FINE_LOCATION**autorisations. Met automatiquement à jour la **AndroidManifest.xml** fichier.

# <a name="iostabios"></a>[iOS](#tab/ios)

De votre application **Info.plist** doit contenir le `NSLocationWhenInUseUsageDescription` clé afin d’accéder à l’emplacement de l’appareil.

Ouvrez l’éditeur plist et ajoutez le **confidentialité - emplacement lors de la Description de l’utilisation d’utilisation** propriété et fournissez une valeur pour afficher l’utilisateur.

Ou modifier manuellement le fichier et ajoutez le code suivant :

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app needs access location when open.</string>
```

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Vous devez définir le `Location` l’autorisation de l’application. Cela est possible en ouvrant le **Package.appxmanifest** et sélectionnant le **fonctionnalités** onglet et la vérification de **emplacement**.

-----

## <a name="using-geolocation"></a>À l’aide de géolocalisation

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

L’API Geoloation également invitera l’utilisateur pour les autorisations lorsque cela est nécessaire.

Vous pouvez obtenir la dernière valeur connue [emplacement](xref:Xamarin.Essentials.Location) de l’appareil en appelant le `GetLastKnownLocationAsync` (méthode). Cela est souvent plus rapide puis en effectuant une requête complète, mais peut être moins précis.

```csharp
try
{
    var location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to get location
}
```

L’altitude n’est pas toujours disponible. S’il n’est pas disponible, le `Altitude` propriété peut être `null` ou la valeur peut être zéro. Si l’altitude est disponible, la valeur est en mètres au-dessus altitude. 

Pour interroger le périphérique actuel [emplacement](xref:Xamarin.Essentials.Location) coordonnées, la `GetLocationAsync` peut être utilisé. Il est préférable de passer un complet `GeolocationRequest` et `CancellationToken` , car il peut prendre un certain temps pour obtenir l’emplacement de l’appareil.

```csharp
try
{
    var request = new GeolocationRequest(GeolocationAccuracy.Medium);
    var location = await Geolocation.GetLocationAsync(request);

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to get location
}
```

## <a name="geolocation-accuracy"></a>Précision de géolocalisation

Le tableau suivant présente la précision par plateforme :

### <a name="lowest"></a>Minimale

| Plateforme | Distance (en mètres) |
| --- | --- |
| Android | 500 |
| iOS | 3 000 |
| UWP | 1000 - 5000 |

### <a name="low"></a>Faible

| Plateforme | Distance (en mètres) |
| --- | --- |
| Android | 500 |
| iOS | 1000 |
| UWP | 300 - 3000 |

### <a name="medium-default"></a>Moyenne (par défaut)

| Plateforme | Distance (en mètres) |
| --- | --- |
| Android | 100 - 500 |
| iOS | 100 |
| UWP | 30-500 |

### <a name="high"></a>Haute

| Plateforme | Distance (en mètres) |
| --- | --- |
| Android | 0 - 100 |
| iOS | 10 |
| UWP | < = 10 |

### <a name="best"></a>Mieux

| Plateforme | Distance (en mètres) |
| --- | --- |
| Android | 0 - 100 |
| iOS | ~0 |
| UWP | < = 10 |

<a name="calculate-distance" />

## <a name="distance-between-two-locations"></a>Distance entre deux emplacements

Le [ `Location` ](xref:Xamarin.Essentials.Location) et [ `LocationExtensions` ](xref:Xamarin.Essentials.LocationExtensions) définissent des classes `CalculateDistance` méthodes qui vous permettent de calculer la distance entre deux emplacements géographiques. Cette valeur calculée distance ne tient pas des routes ou autres voies de compte et est simplement la distance la plus courte entre les deux points le long de la surface de la terre, également connue sous le _great-cercle distance_ ou de manière simplifiée, la distance « vol d’oiseau. »

Voici un exemple :

```csharp
Location boston = new Location(42.358056, -71.063611);
Location sanFrancisco = new Location(37.783333, -122.416667);
double miles = Location.CalculateDistance(boston, sanFrancisco, DistanceUnits.Miles);
```

Le `Location` constructeur a des arguments de latitude et longitude dans cet ordre. Les valeurs de latitude positif sont au nord de l’Équateur, et les valeurs de longitude positif à l’est. Utilisez l’argument final `CalculateDistance` pour spécifier les miles ou kilomètres. Le `Location` classe définit également `KilometersToMiles` et `MilesToKilometers` méthodes pour la conversion entre les deux unités.

## <a name="api"></a>API

- [Code source de géolocalisation](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geolocation)
- [Documentation de l’API de géolocalisation](xref:Xamarin.Essentials.Geolocation)
