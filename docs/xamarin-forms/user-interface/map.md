---
title: Carte
description: Xamarin.Forms utilisant l’API de mappage natif sur chaque plateforme.
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 7dcf3cba72a07b06236e29ddf2603745fd348596
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2018
---
# <a name="map"></a>Carte

_Xamarin.Forms utilisant l’API de mappage natif sur chaque plateforme._

Xamarin.Forms.Maps utilise l’API de mappage natif sur chaque plateforme. Cela fournit une expérience de cartes rapide et familiers pour les utilisateurs, mais il signifie que certaines étapes de configuration sont nécessaires à respecter chaque plateformes API aux exigences.
Une fois configuré, le `Map` contrôle fonctionne exactement comme tout autre élément de Xamarin.Forms dans le code commun.

* [Mappe l’initialisation](#Maps_Initialization) : à l’aide `Map` nécessite que du code d’initialisation supplémentaire au démarrage.
* [Configuration de la plate-forme](#Platform_Configuration) -chaque plate-forme requiert une configuration pour les mappages fonctionner.
* [L’utilisation des mappages en C#](#Using_Maps) -affichage mappe et codes confidentiels à l’aide de c#.
* [L’utilisation des mappages dans XAML](#Using_Xaml) -affichage d’une carte avec XAML.

Le contrôle de carte a été utilisé dans le [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/) exemple illustré ci-dessous.

 [![Cartes dans l’exemple MobileCRM](map-images/maps-zoom-sml.png "exemple de mappage de contrôle")](map-images/maps-zoom.png#lightbox "exemple de mappage de contrôle")

Fonctionnalités de carte peuvent être améliorée en créant un [mapper convertisseur personnalisé](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).

<a name="Maps_Initialization" />

## <a name="maps-initialization"></a>Initialisation de cartes

Lors de l’ajout de mappages à une application de Xamarin.Forms, **Xamarin.Forms.Maps** est un un package NuGet distinct que vous devez ajouter à chaque projet de la solution.
Sur Android, cela a également une dépendance sur GooglePlayServices (un autre NuGet) qui est téléchargé automatiquement lorsque vous ajoutez Xamarin.Forms.Maps.

Après avoir installé le package NuGet, du code d’initialisation est requis dans chaque projet d’application, *après* le `Xamarin.Forms.Forms.Init` appel de méthode. Pour iOS, utilisez le code suivant :

```csharp
Xamarin.FormsMaps.Init();
```

Sur Android, vous devez passer les mêmes paramètres que `Forms.Init`:

```csharp
Xamarin.FormsMaps.Init(this, bundle);
```

Pour le Windows Runtime (WinRT) et la plateforme Windows universelle (UWP), utilisez le code suivant :

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

Ajoutez cet appel dans les fichiers suivants pour chaque plateforme :

-  **iOS** -AppDelegate.cs de fichiers, dans le `FinishedLaunching` (méthode).
-  **Android** -MainActivity.cs de fichiers, dans le `OnCreate` (méthode).
-  **WinRT et UWP** -fichier MainPage.xaml.cs, dans le `MainPage` constructeur.

Une fois que le package NuGet a été ajouté et la méthode d’initialisation appelée à l’intérieur de chaque d’applications, `Xamarin.Forms.Maps` API peuvent être utilisées dans le code de bibliothèque de classes portables ou de projet partagé commun.

<a name="Platform_Configuration" />

## <a name="platform-configuration"></a>Configuration de la plateforme

Étapes de configuration supplémentaires sont requises sur certaines plates-formes avant que la carte s’affiche.

### <a name="ios"></a>iOS

Sur iOS 7 le mappage de contrôle « fonctionne, tout simplement », aussi longtemps en tant que le `FormsMaps.Init()` appel a été effectué.

Pour iOS 8 deux clés doivent être ajoutés à la **Info.plist** fichier : [ `NSLocationAlwaysUsageDescription` ](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) et [ `NSLocationWhenInUseUsageDescription` ](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26). La représentation XML est illustrée ci-dessous, vous devez mettre à jour le `string` valeurs afin de refléter la manière dont votre application utilise les informations d’emplacement :

```xml
<key>NSLocationAlwaysUsageDescription</key>
    <string>Can we use your location</string>
<key>NSLocationWhenInUseUsageDescription</key>
    <string>We are using your location</string>
```

Le **Info.plist** entrées peuvent également être ajoutées dans **Source** affichage lors de la modification du **Info.plist** fichier :

![Info.plist pour iOS 8](map-images/ios8-map-permissions.png "iOS 8 des entrées Info.plist requises")


### <a name="android"></a>Android

Pour utiliser le [Google Maps API v2](https://developers.google.com/maps/documentation/android/) sur Android, vous devez générer une clé d’API et ajoutez-le à votre projet Android.
Suivez les instructions fournies dans la documentation de Xamarin [obtention d’une clé de v2 Google Maps API](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).
Après avoir suivi ces instructions, collez la clé d’API dans le **Properties/AndroidManifest.xml** fichier (afficher la source et rechercher/mettre à jour l’élément suivant) :

```xml
<meta-data
        android:name="com.google.android.geo.API_KEY"
        android:value="YOUR_API_KEY"/>
```

Sans une clé d’API valide le contrôle maps affiche sous forme de zone grise sur Android.

> [!NOTE]
> N’oubliez pas de générer une autre clé de l’aide du fichier de magasin de clés qui est utilisé pour signer la version de n’importe quelle application qui est téléchargée vers le Google Play store. La clé que vous générez pour le développement et le débogage ne fonctionnera pas et l’application téléchargée à partir de Google Play rompu affichage de la carte. Pensez également à régénérer la touche si l’application **nom du Package** modifications.

Vous devez également activer des autorisations appropriées en cliquant sur le projet Android et en sélectionnant **Options > Générer > Application Android** et son cycle les éléments suivants :

* `AccessCoarseLocation`
* `AccessFineLocation`
* `AccessLocationExtraCommands`
* `AccessMockLocation`
* `AccessNetworkState`
* `AccessWifiState`
* `Internet`

Certains d'entre eux sont affichés dans la capture d’écran ci-dessous :

![Autorisations requises pour Android](map-images/android-map-permissions.png "les autorisations requises pour Android")

Les deux derniers sont requis, car les applications nécessitent une connexion réseau pour télécharger les données de la carte. En savoir plus sur Android [autorisations](http://developer.android.com/reference/android/Manifest.permission.html) pour en savoir plus.

### <a name="windows-runtime-and-universal-windows-platform"></a>Windows Runtime et la plateforme Windows universelle

Pour utiliser des cartes sur le Windows Runtime et la plateforme Windows universelle, vous devez générer un jeton d’autorisation. Pour plus d’informations, consultez [demander une clé d’authentification maps](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx) sur MSDN.

Le jeton d’authentification doit ensuite être spécifié dans le `FormsMaps.Init("AUTHORIZATION_TOKEN")` appel de méthode, d’authentifier l’application avec Bing Maps.

<a name="Using_Maps" />

## <a name="using-maps"></a>L’utilisation des mappages

Consultez le [MapPage.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/MobileCRM/MobileCRM.Shared/Pages/MapPage.cs) dans l’exemple MobileCRM pour obtenir un exemple de comment le contrôle de carte peut être utilisé dans le code. Un simple `MapPage` classe peut ressembler à ce - Notez qu’un nouveau `MapSpan` est créée afin de positionner la vue de la carte :

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

Comme indiqué dans l’extrait de code ci-dessus, en fournissant un `MapSpan` instance à un constructeur de la carte définit la vue initiale (point central et le niveau de zoom) de la carte quand il est chargé. Le `MoveToRegion` méthode sur la classe map peut ensuite être utilisée pour modifié le niveau de zoom ou de la position de la carte. Il existe deux façons de créer un nouveau `MapSpan` instance :

-  **MapSpan.FromCenterAndRadius()** -méthode statique pour créer une étendue d’un `Position` et en spécifiant un `Distance` .
-  **nouvelle MapSpan ()** -constructeur qui utilise un `Position` et les degrés de latitude et longitude à afficher.


Pour modifier le niveau de zoom de la carte sans modifier l’emplacement, créez un `MapSpan` à l’aide de l’emplacement actuel à partir de la `VisibleRegion.Center` propriété du contrôle de carte. Un `Slider` peut être utilisé pour le contrôle de zoom de mappage comme suit (mais zoom directement dans le contrôle de carte actuellement ne peut pas mettre à jour la valeur du curseur) :

```csharp
var slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) => {
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

 [![Cartes avec zoom](map-images/maps-zoom-sml.png "contrôle de carte Zoom")](map-images/maps-zoom.png#lightbox "mappage de contrôle de Zoom")

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

 `PinType` peut être définie à une des valeurs suivantes, ce qui peuvent affecter le mode d’affichage du code confidentiel dans (selon la plateforme) :

-  Générique
-  Sur place
-  SavedPin
-  Résultats de la recherche


<a name="Using_Xaml" />

## <a name="using-xaml"></a>À l’aide de Xaml

Maps peuvent également être positionnées dans les dispositions de Xaml comme indiqué dans cet extrait de code.

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

Le `MapRegion` et `Pins` peut être définie dans le code à l’aide du `MyMap` référence (ou tout ce que la carte est nommée). Notez qu’un autre `xmlns` définition de l’espace de noms est obligatoire pour référencer les contrôles Xamarin.Forms.Maps.

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Le Xamarin.Forms.Maps est un NuGet distinct qui doit être ajouté à chaque projet dans une solution Xamarin.Forms. Code d’initialisation supplémentaire est requis, bien que certaines étapes de configuration pour iOS, Android, WinRT et UWP.

Une fois configurée, l’API mappe peut être utilisée pour restituer des mappages avec des marqueurs de code confidentiel en seulement quelques lignes de code. Mappages peuvent être améliorés avec un [convertisseur personnalisé](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).


## <a name="related-links"></a>Liens associés

- [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/)
- [Mapper le convertisseur personnalisé](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Exemples Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
