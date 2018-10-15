---
title: 'Xamarin.Essentials : Géolocalisation'
description: Ce document décrit la classe de géolocalisation dans Xamarin.Essentials, qui fournit des API pour récupérer les coordonnées de géolocalisation actuelle de l’appareil.
ms.assetid: 8F66092C-13F0-4FEE-8AA5-901D5F79B357
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 0aeb2ed96e6c21def69eb2e6f305b26e2e478825
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353852"
---
# <a name="xamarinessentials-geolocation"></a>Xamarin.Essentials : Géolocalisation

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **géolocalisation** fournit des API pour récupérer les coordonnées de géolocalisation actuelle de l’appareil.

## <a name="getting-started"></a>Prise en main

Pour accéder aux fonctionnalités de l'API **géolocalisation** quelques étapes de configuration spécifiques à la plate-forme sont nécessaires.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Épais et l’emplacement précis des autorisations sont nécessaires et doivent être configurées dans le projet Android. En outre, si votre application cible Android 5.0 (niveau 21 d’API) ou une version ultérieure, vous devez déclarer que votre application utilise les fonctionnalités matérielles dans le fichier manifeste. Il peut être ajouté comme suit :

Ouvrez le fichier **AssemblyInfo.cs** sous le dossier **propriétés** et ajoutez :

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessCoarseLocation)]
[assembly: UsesPermission(Android.Manifest.Permission.AccessFineLocation)]
[assembly: UsesFeature("android.hardware.location", Required = false)]
[assembly: UsesFeature("android.hardware.location.gps", Required = false)]
[assembly: UsesFeature("android.hardware.location.network", Required = false)]
```

Ou mettre à jour le manifeste Android :

Ouvrez le fichier **AndroidManifest.xml** sous le dossier **propriétés** et ajoutez le code suivant à l’intérieur de la **manifeste** nœud :

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-feature android:name="android.hardware.location" android:required="false" />
<uses-feature android:name="android.hardware.location.gps" android:required="false" />
<uses-feature android:name="android.hardware.location.network" android:required="false" />
```

Ou avec le bouton droit sur le projet Android et ouvrez les propriétés du projet. Sous **manifeste Android** trouver la **autorisations requises :** zone et vérifiez la **ACCESS_COARSE_LOCATION** et **ACCESS_FINE_LOCATION**autorisations. Cela met automatiquement à jour le fichier **AndroidManifest.xml**.

# <a name="iostabios"></a>[iOS](#tab/ios)

De votre application **Info.plist** doit contenir le `NSLocationWhenInUseUsageDescription` clé afin d’accéder à l’emplacement de l’appareil.

Ouvrez l’éditeur plist et ajoutez le **confidentialité - emplacement lors de la Description de l’utilisation utilisation** propriété et renseignez une valeur pour afficher l’utilisateur.

Ou modifier manuellement le fichier et ajoutez ce qui suit :

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app needs access location when open.</string>
```

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Vous devez définir le `Location` autorisation pour l’application. Cela est possible en ouvrant le **Package.appxmanifest** et en sélectionnant le **fonctionnalités** onglet et en vérifiant **emplacement**.

-----

## <a name="using-geolocation"></a>Utilisation de **Geolocation**

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

L’altitude n’est pas toujours disponible. S’il n’est pas disponible, le `Altitude` propriété peut être `null` ou la valeur peut être zéro. Si l’altitude est disponible, la valeur est en mètres au-dessus de plus haut niveau de la mer.

Pour interroger l’appareil actuel [emplacement](xref:Xamarin.Essentials.Location) coordonnées, la `GetLocationAsync` peut être utilisé. Il est préférable de transmettre un intégral `GeolocationRequest` et `CancellationToken` dans la mesure où il peut prendre un certain temps pour obtenir l’emplacement de l’appareil.

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
| ---------- | -------------------- |
| Android    | 500                  |
| iOS        | 3 000                |
| UWP        | 1000 - 5000          |

### <a name="low"></a>Faible

| Plateforme | Distance (en mètres) |
| ---------- | -------------------- |
| Android    | 500                  |
| iOS        | 1000                 |
| UWP        | 300 - 3000           |

### <a name="medium-default"></a>Moyenne (valeur par défaut)

| Plateforme | Distance (en mètres) |
| ---------- | -------------------- |
| Android    | 100 - 500            |
| iOS        | 100                  |
| UWP        | 30-500               |

### <a name="high"></a>Haute

| Plateforme | Distance (en mètres) |
| ---------- | -------------------- |
| Android    | 0 - 100              |
| iOS        | 10                   |
| UWP        | < = 10               |

### <a name="best"></a>Meilleur

| Plateforme | Distance (en mètres) |
| ---------- | -------------------- |
| Android    | 0 - 100              |
| iOS        | ~0                   |
| UWP        | < = 10               |

<a name="calculate-distance" />

## <a name="distance-between-two-locations"></a>Distance entre deux emplacements

Le [ `Location` ](xref:Xamarin.Essentials.Location) et [ `LocationExtensions` ](xref:Xamarin.Essentials.LocationExtensions) classes définissent `CalculateDistance` méthodes qui vous permettent de calculer la distance entre deux emplacements géographiques. Cette valeur calculée distance ne tient pas routes ou autres voies de compte et est simplement la distance la plus courte entre les deux points le long de la surface de la terre, également connu sous le _distance orthodromique_ ou quand, le distance « vol d’oiseau. »

Voici un exemple :

```csharp
Location boston = new Location(42.358056, -71.063611);
Location sanFrancisco = new Location(37.783333, -122.416667);
double miles = Location.CalculateDistance(boston, sanFrancisco, DistanceUnits.Miles);
```

Le `Location` constructeur a des arguments de latitude et longitude dans cet ordre. Positif sont des valeurs de latitude au nord de l’Équateur et sont des valeurs de longitude positive à l’est. Utilisez l’argument final de `CalculateDistance` pour spécifier les miles ou kilomètres. Le `Location` classe définit également `KilometersToMiles` et `MilesToKilometers` méthodes pour la conversion entre les deux unités.

## <a name="api"></a>API

- [Code source de géolocalisation](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geolocation)
- [Documentation de l’API de géolocalisation](xref:Xamarin.Essentials.Geolocation)
