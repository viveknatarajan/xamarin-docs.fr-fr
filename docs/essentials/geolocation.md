---
title: Xamarin.Essentials géolocalisation
description: La classe de géolocalisation fournit des API pour récupérer les coordonnées de géolocalisation du périphérique en cours.
ms.assetid: 8F66092C-13F0-4FEE-8AA5-901D5F79B357
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: bf0fa7d2caf7c8857bc1272f4471def04100383f
ms.sourcegitcommit: 9f8e7393019791bbd6af4fefaa24a1602adabb4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="xamarinessentials-geolocation"></a>Xamarin.Essentials géolocalisation

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
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}");
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

Pour interroger le périphérique actuel [emplacement](xref:Xamarin.Essentials.Location) coordonnées, la `GetLocationAsync` peut être utilisé. Il est préférable de passer un complet `GeolocationRequest` et `CancellationToken` , car il peut prendre un certain temps pour obtenir l’emplacement de l’appareil.

```csharp
try
{
    var request = new GeolocationRequest(GeolocationAccuracy.Medium);
    var location = await Geolocation.GetLocationAsync(request);

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}");
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

## <a name="api"></a>API

- [Code source de géolocalisation](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geolocation)
- [Documentation de l’API de géolocalisation](xref:Xamarin.Essentials.Geolocation)
