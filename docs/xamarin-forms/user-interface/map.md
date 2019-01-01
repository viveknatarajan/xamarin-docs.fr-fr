---
title: Mappage de Xamarin.Forms
description: Cet article explique comment utiliser la classe Map de Xamarin.Forms à utiliser l’API de mappage natif sur chaque plateforme pour fournir qu'un familier mappe l’expérience des utilisateurs.
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 1d164c8593e358a97b21f42bf7116f64d0ac460d
ms.sourcegitcommit: 6e84adf7358dc05f4d888ab2674de70d88214090
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/31/2018
ms.locfileid: "53815215"
---
# <a name="xamarinforms-map"></a>Mappage de Xamarin.Forms

[![Télécharger l’exemple](~/media/shared/download.png) télécharger l’exemple](https://developer.xamarin.com/samples/WorkingWithMaps/)

_Xamarin.Forms utilise l’API de mappage natif sur chaque plateforme._

Xamarin.Forms.Maps utilise l’API de mappage natif sur chaque plateforme. Cela offre une expérience de cartes rapide, familière pour les utilisateurs, mais signifie que certaines étapes de configuration sont nécessaires à respecter pour chaque configuration requise des plateformes spécifiques API.
Une fois configuré, le `Map` contrôler fonctionne exactement comme tout autre élément de Xamarin.Forms dans le code commun.

* [Mappe l’initialisation](#Maps_Initialization) : à l’aide `Map` nécessite du code d’initialisation supplémentaires au démarrage.
* [Configuration de la plateforme](#Platform_Configuration) -chaque plateforme requiert une configuration pour les mappages fonctionner.
* [À l’aide de cartes en C#](#Using_Maps) -affichage mappe et épingle à l’aide de c#.
* [À l’aide des mappages dans XAML](#Using_Xaml) -affichage d’une carte avec XAML.

Le contrôle de carte a été utilisé dans le [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/) exemple, qui est indiqué ci-dessous.

 [![Cartes dans l’exemple MobileCRM](map-images/maps-zoom-sml.png "exemple de contrôle de carte")](map-images/maps-zoom.png#lightbox "exemple de contrôle de carte")

Fonctionnalité de carte peut encore être améliorée en créant un [mapper convertisseur personnalisé](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).

<a name="Maps_Initialization" />

## <a name="maps-initialization"></a>Initialisation de cartes

Lorsque vous ajoutez des mappages à une application Xamarin.Forms, **Xamarin.Forms.Maps** est un package NuGet distinct, vous devez ajouter à chaque projet dans la solution.
Sur Android, cela a également une dépendance sur GooglePlayServices (un autre NuGet) qui est téléchargé automatiquement lorsque vous ajoutez Xamarin.Forms.Maps.

Après avoir installé le package NuGet, du code d’initialisation est requis dans chaque projet d’application, *après* le `Xamarin.Forms.Forms.Init` appel de méthode. Pour iOS, utilisez le code suivant :

```csharp
Xamarin.FormsMaps.Init();
```

Sur Android, vous devez passer les mêmes paramètres que `Forms.Init`:

```csharp
Xamarin.FormsMaps.Init(this, bundle);
```

Pour la plateforme Windows universelle (UWP) utilisez le code suivant :

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

Ajoutez cet appel dans les fichiers suivants pour chaque plateforme :

-  **iOS** -fichier AppDelegate.cs, dans le `FinishedLaunching` (méthode).
-  **Android** -MainActivity.cs de fichiers, dans le `OnCreate` (méthode).
-  **UWP** -fichier MainPage.xaml.cs, dans le `MainPage` constructeur.

Une fois que le package NuGet a été ajouté et la méthode d’initialisation appelée à l’intérieur de chaque d’applications, `Xamarin.Forms.Maps` API peuvent être utilisées dans le projet de bibliothèque .NET Standard commun ou le code de projet partagé.

<a name="Platform_Configuration" />

## <a name="platform-configuration"></a>Configuration de la plateforme

Étapes de configuration supplémentaires sont requises sur certaines plateformes avant que la carte s’affiche.

### <a name="ios"></a>iOS

Pour accéder aux services de localisation sur iOS, vous devez définir les clés suivantes dans **Info.plist**:

- iOS 11
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) – pour l’utilisation des services de localisation lors de l’application est en cours d’utilisation
    - [`NSLocationAlwaysAndWhenInUseUsageDescription`](https://developer.apple.com/documentation/corelocation/choosing_the_authorization_level_for_location_services/requesting_always_authorization?language=objc) – pour l’utilisation des services de localisation à tout moment
- iOS 10 et versions antérieures
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) – pour l’utilisation des services de localisation lors de l’application est en cours d’utilisation
    - [`NSLocationAlwaysUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) – pour l’utilisation des services de localisation à tout moment    

Pour prendre en charge iOS 11 et versions antérieures, vous pouvez inclure toutes les trois clés : `NSLocationWhenInUseUsageDescription`, `NSLocationAlwaysAndWhenInUseUsageDescription`, et `NSLocationAlwaysUsageDescription`.

La représentation XML de ces clés dans **Info.plist** est indiqué ci-dessous. Vous devez mettre à jour le `string` valeurs afin de refléter la façon dont votre application utilise les informations d’emplacement :

```xml
<key>NSLocationAlwaysUsageDescription</key>
<string>Can we use your location at all times?</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>Can we use your location when your app is being used?</string>
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>Can we use your location at all times?</string>
```

Le **Info.plist** entrées peuvent également être ajoutées dans **Source** affichage lors de la modification la **Info.plist** fichier :

![Info.plist pour iOS 8](map-images/ios8-map-permissions.png "iOS 8 des entrées Info.plist requises")


### <a name="android"></a>Android

Pour utiliser le [v2 de l’API Google Maps](https://developers.google.com/maps/documentation/android/) sur Android, vous devez générer une clé d’API et ajoutez-le à votre projet Android.
Suivez les instructions de la documentation de Xamarin [obtention d’une clé de v2 de l’API Google Maps](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).
Après avoir suivi ces instructions, collez la clé d’API dans le **Properties/Androidmanifest.XML** fichier (afficher la source et find/mettre à jour l’élément suivant) :

```xml
<application ...>
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
</application>
```

Sans une clé API valide le contrôle maps s’affichera sous forme de zone grise sur Android.

> [!NOTE]
> Notez que, afin que votre APK accéder à Google Maps, vous devez inclure les empreintes digitales de SHA-1 et empaqueter des noms pour chaque magasin de clés (debug et release) que vous utilisez pour signer votre APK. Par exemple, si vous utilisez un ordinateur pour le débogage et un autre ordinateur pour générer le fichier APK de mise en production, vous devez inclure l’empreinte de certificat SHA-1 à partir du magasin de clés de débogage du premier ordinateur et l’empreinte de certificat SHA-1 à partir du magasin de clés de version de le deuxième ordinateur. Pensez également à modifier les informations d’identification clées si l’application **nom_package** modifications. Consultez [obtention d’une clé de v2 de l’API Google Maps](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).

Vous devez également activer les autorisations appropriées en effectuant un clic droit sur le projet Android et en sélectionnant **Options > Générer > Application Android** et déjà les éléments suivants :

* `AccessCoarseLocation`
* `AccessFineLocation`
* `AccessLocationExtraCommands`
* `AccessMockLocation`
* `AccessNetworkState`
* `AccessWifiState`
* `Internet`

Certaines d'entre elles sont présentées dans la capture d’écran ci-dessous :

![Autorisations requises pour Android](map-images/android-map-permissions.png "les autorisations requises pour Android")

Les deux derniers sont requis, car les applications nécessitent une connexion réseau pour télécharger les données cartographiques. En savoir plus sur Android [autorisations](http://developer.android.com/reference/android/Manifest.permission.html) pour en savoir plus.

### <a name="universal-windows-platform"></a>Plateforme Windows universelle

Pour utiliser des cartes sur la plateforme Windows universelle, vous devez générer un jeton d’autorisation. Pour plus d’informations, consultez [demander une clé d’authentification maps](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx) sur MSDN.

Le jeton d’authentification doit ensuite être spécifié dans le `FormsMaps.Init("AUTHORIZATION_TOKEN")` appel de méthode, pour authentifier l’application avec Bing Maps.

<a name="Using_Maps" />

## <a name="using-maps"></a>Utilisation de Cartes

Consultez le [MapPage.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/MobileCRM/MobileCRM.Shared/Pages/MapPage.cs) dans l’exemple MobileCRM pour obtenir un exemple de comment le contrôle de carte peut être utilisé dans le code. Une simple `MapPage` classe peut ressembler à cet - avis qui un nouveau `MapSpan` est créé pour positionner les vue de la carte :

```csharp
public class MapPage : ContentPage {
    public MapPage() {
        var map = new Map(
            MapSpan.FromCenterAndRadius(
                    new Position(37,-122), Distance.FromMiles(0.3))) {
                IsShowingUser = true,
                HeightRequest = 100,
                WidthRequest = 960,
                VerticalOptions = LayoutOptions.FillAndExpand
            };
        var stack = new StackLayout { Spacing = 0 };
        stack.Children.Add(map);
        Content = stack;
    }
}
```

### <a name="map-type"></a>Type de mappage

Le contenu de la carte peut également être modifié en définissant le `MapType` propriété, pour afficher une carte postale régulière (la valeur par défaut), les images satellite ou une combinaison des deux.

```csharp
map.MapType == MapType.Street;
```

Valide `MapType` les valeurs sont :

-  Hybride
-  Satellite
-  Rue (la valeur par défaut)


### <a name="map-region-and-mapspan"></a>MapSpan et région

Comme indiqué dans l’extrait de code ci-dessus, en fournissant un `MapSpan` instance à un constructeur de la carte définit la vue initiale (point central et un niveau de zoom) de la carte lorsqu’il est chargé. Le `MoveToRegion` méthode sur la classe map peut ensuite être utilisée pour modifier le niveau de zoom ou de la position de la carte. Il existe deux façons de créer un nouveau `MapSpan` instance :

-  **MapSpan.FromCenterAndRadius()** -méthode statique pour créer une étendue d’un `Position` et en spécifiant un `Distance` .
-  **nouvelle MapSpan ()** -constructeur qui utilise un `Position` et les degrés de latitude et longitude à afficher.


Pour modifier le niveau de zoom de la carte sans modifier l’emplacement, créez un nouveau `MapSpan` à l’aide de l’emplacement actuel à partir de la `VisibleRegion.Center` propriété du contrôle de carte. Un `Slider` peut servir à contrôler le zoom de mappage comme suit (mais le zoom directement dans le contrôle de carte actuellement ne peut pas mettre à jour la valeur du curseur) :

```csharp
var slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) => {
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

 [![Mappages avec zoom](map-images/maps-zoom-sml.png "effectuer un zoom avant du contrôle de carte")](map-images/maps-zoom.png#lightbox "effectuer un zoom avant du contrôle de carte")

### <a name="map-pins"></a>Codes confidentiels de carte

Emplacements peuvent être marquées sur la carte avec `Pin` objets.

```csharp
var position = new Position(37,-122); // Latitude, Longitude
var pin = new Pin {
            Type = PinType.Place,
            Position = position,
            Label = "custom pin",
            Address = "custom detail info"
        };
map.Pins.Add(pin);
```

 `PinType` peut être définie à une des valeurs suivantes, qui peuvent affecter la façon dont le code confidentiel est rendu (selon la plateforme) :

-  Générique
-  Sur place
-  SavedPin
-  Résultats de la recherche


<a name="Using_Xaml" />

## <a name="using-xaml"></a>À l’aide de Xaml

Maps peuvent également être positionnés dans les dispositions de Xaml, comme indiqué dans cet extrait de code.

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
    x:Class="MapDemo.MapPage">
    <StackLayout VerticalOptions="StartAndExpand" Padding="30">
        <maps:Map WidthRequest="320" HeightRequest="200"
            x:Name="MyMap"
            IsShowingUser="true"
            MapType="Hybrid"
        />
    </StackLayout>
</ContentPage>
```

Le `MapRegion` et `Pins` peuvent être définies dans le code à l’aide du `MyMap` référence (ou quel que soit la carte est nommée). Notez que supplémentaires `xmlns` définition de l’espace de noms est obligatoire pour référencer les contrôles Xamarin.Forms.Maps.

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Le Xamarin.Forms.Maps est un package NuGet distinct qui doit être ajouté à chaque projet dans une solution Xamarin.Forms. Code d’initialisation supplémentaire est nécessaire, bien que certaines étapes de configuration pour iOS, Android et UWP.

Une fois configurée, l’API mappe peut être utilisée pour rendre des cartes avec des marqueurs de code confidentiel en seulement quelques lignes de code. Maps peuvent encore être améliorées avec une [convertisseur personnalisé](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).


## <a name="related-links"></a>Liens associés

- [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/)
- [Mapper le convertisseur personnalisé](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Exemples Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
