---
title: 'Xamarin.Essentials : Géolocalisation'
description: La classe Geolocation fournit des API permettant de récupérer les coordonnées géographiques actuelles de l’appareil.
ms.assetid: 8F66092C-13F0-4FEE-8AA5-901D5F79B357
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 0aeb2ed96e6c21def69eb2e6f305b26e2e478825
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353852"
---
# <a name="xamarinessentials-geolocation"></a>Xamarin.Essentials : Géolocalisation

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **Geolocation** fournit des API permettant de récupérer les coordonnées géographiques actuelles de l’appareil.

## <a name="getting-started"></a>Prise en main

Pour accéder aux fonctionnalités de l'API **Geolocation**, quelques étapes de configurations propres aux plateformes sont nécessaires.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Épais et l’emplacement précis des autorisations sont nécessaires et doivent être configurées dans le projet Android. En outre, si votre application cible Android 5.0 (niveau 21 d’API) ou une version ultérieure, vous devez déclarer que votre application utilise les fonctionnalités matérielles dans le fichier manifeste. Il peut être ajouté comme suit :

Ouvrez le fichier **AssemblyInfo.cs** dans le dossier **Proprerties** et ajoutez :

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessCoarseLocation)]
[assembly: UsesPermission(Android.Manifest.Permission.AccessFineLocation)]
[assembly: UsesFeature("android.hardware.location", Required = false)]
[assembly: UsesFeature("android.hardware.location.gps", Required = false)]
[assembly: UsesFeature("android.hardware.location.network", Required = false)]
```

Ou mettez à jour le manifeste Android :

Ouvrez le fichier **AndroidManifest.xml** dans le dossier **Properties** et ajoutez le code suivant dans le nœud **manifest** :

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-feature android:name="android.hardware.location" android:required="false" />
<uses-feature android:name="android.hardware.location.gps" android:required="false" />
<uses-feature android:name="android.hardware.location.network" android:required="false" />
```

Ou cliquez avec le bouton droit sur le projet Android et ouvrez les propriétés du projet. Sous **manifeste Android**, trouvez la zone "**autorisations requises :**" et cochez les autorisations **ACCESS_COARSE_LOCATION** et **ACCESS_FINE_LOCATION**. Cela met automatiquement à jour le fichier **AndroidManifest.xml**.

# <a name="iostabios"></a>[iOS](#tab/ios)

Le fichier **Info.plist** doit contenir la clé `NSLocationWhenInUseUsageDescription` afin d’accéder à l’emplacement de l’appareil.

Ouvrez l’éditeur de plist, ajoutez la propriété **Privacy - Location When In Use Usage Description** et renseignez un texte à afficher à l’utilisateur.

Ou modifier manuellement le fichier et ajoutez ce qui suit :

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app needs access location when open.</string>
```

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Vous devez définir l'autorisation `Location` pour l’application. Ouvrez **Package.appxmanifest**, sélectionnez l'onglet **fonctionnalités** et cochez **Location**.

-----

## <a name="using-geolocation"></a>Utilisation de **Geolocation**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

L’API Geoloation également invitera l’utilisateur pour les autorisations lorsque cela est nécessaire.

Vous pouvez obtenir la dernière [position](xref:Xamarin.Essentials.Location) connue de l’appareil en appelant la méthode `GetLastKnownLocationAsync`. Cela est souvent plus rapide que de demander une nouvelle lecture, mais cela peut être moins précis.

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

L’altitude n’est pas toujours disponible. Si elle n’est pas disponible, la propriété `Altitude` pourra être soit `null`, soit zéro. Si l’altitude est disponible, la valeur sera exprimée en mètres par rapport au plus haut niveau de la mer.

La fonction `GetLocationAsync` permet de récupérer les [coordonnées](xref:Xamarin.Essentials.Location) actuelles de l'appareil. Il est préférable de passer en paramètres un objet `GeolocationRequest` et un `CancellationToken` dans la mesure où la lecture des coordonnées actuelles de l'appareil peut prendre un certain temps.

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

### <a name="medium-default"></a>Moyenne (valeur par défaut)

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

### <a name="best"></a>Meilleur

| Plateforme | Distance (en mètres) |
| --- | --- |
| Android | 0 - 100 |
| iOS | ~0 |
| UWP | < = 10 |

<a name="calculate-distance" />

## <a name="distance-between-two-locations"></a>Distance entre deux emplacements

Le [ `Location` ](xref:Xamarin.Essentials.Location) et [ `LocationExtensions` ](xref:Xamarin.Essentials.LocationExtensions) classes définissent `CalculateDistance` méthodes qui vous permettent de calculer la distance entre deux emplacements géographiques. Cette valeur calculée distance ne tient pas routes ou autres voies de compte et est simplement la distance la plus courte entre les deux points le long de la surface de la terre, également connu sous le _distance orthodromique_ ou quand, le distance « vol d’oiseau. »

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
