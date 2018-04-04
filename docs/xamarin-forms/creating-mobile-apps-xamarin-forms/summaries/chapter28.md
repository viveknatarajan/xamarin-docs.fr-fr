---
title: Résumé du chapitre 28. Emplacement et des mappages
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F6E20077-687C-45C4-A375-31D4F49BBFA4
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 40d2b67f1a1655ec1d731493446f320b8aef17ca
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-28-location-and-maps"></a>Résumé du chapitre 28. Emplacement et des mappages

Xamarin.Forms prend en charge un [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) élément dérivé `View`. En raison des exigences spéciales plateforme impliqués dans l’utilisation des mappages, elles sont implémentées dans un assembly séparé, **Xamarin.Forms.Maps**, impliquent l’intervention d’un autre espace de noms : `Xamarin.Forms.Maps`.

## <a name="the-geographic-coordinate-system"></a>Le système de coordonnées géographique

Un système de coordonnées géographiques identifie les positions sur un objet sphérique (ou presque sphérique), comme la terre. Une coordonnée comprend à la fois un *latitude* et *longitude* exprimée dans les angles.

Un plus grand cercle appelé le `equator` se trouve à mi-chemin entre deux pôles par le biais duquel axe de la terre s’étend sur le plan conceptuel.

### <a name="parallels-and-latitude"></a>Parallels et latitude

Un angle, mesuré à l’Équateur à partir du centre de marques terre Nord ou lignes de latitude égale appelé *parallels*. Ces vont de 0 degrés au niveau de l’Équateur à 90 degrés au nord et Sud pôles. Par convention, Latitude au nord de l’Équateur sont des valeurs positives, et celles au sud de l’Équateur sont des valeurs négatives.

### <a name="longitude-and-meridians"></a>Longitude et les méridiens

Deux moitiés de cercles great depuis le pôle nord au Pôle Sud sont également appelés des lignes de longitude égale, *les méridiens*. Il s’agit par rapport à du premier méridien de Greenwich en Angleterre. Par convention, longitudes à l’est sont des valeurs positives de 0 degrés, 180 degrés et longitudes à l’ouest du premier méridien sont des valeurs négatives de 0 degrés à &ndash;180 degrés.

### <a name="the-equirectangular-projection"></a>La projection équirectangulaire

Tout mappage plat de la terre introduit distorsion. Si toutes les lignes de latitude et de longitude sont droits, et si vous égales différences dans les angles de latitude et longitude correspondant à égale distance sur la carte, le résultat est un *projection équirectangulaire*. Ce mappage déforme la plus proche des zones jusqu’aux pôles, car ils sont étirées horizontalement.

### <a name="the-mercator-projection"></a>La projection Mercator

Le courant *projection Mercator* tente de compenser l’étirement horizontal par étirement également ces zones de verticalement. Cela entraîne une carte de proximité pôles emplacement beaucoup plus importante que qu’ils sont réellement, mais n’importe quel réseau local conforme très près à la zone réelle.

### <a name="map-services-and-tiles"></a>Services de mappage et de vignettes

Services de carte utilisent une variation de la projection Mercator appelée `Web Mercator`. Les services de mappage remettre des vignettes d’image bitmap à un client basé sur l’emplacement et niveau de zoom.

## <a name="getting-the-users-location"></a>Mise en route de l’utilisateur

Le Xamarin.Forms `Map` classes n’incluent pas une fonctionnalité pour obtenir l’emplacement géographique de l’utilisateur, mais il est souvent souhaitable lors de l’utilisation des mappages, par conséquent, un service de dépendance doit la gérer.

### <a name="the-location-tracker-api"></a>Le dispositif de suivi d’emplacement API

Le [ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) solution contient du code pour une composant API de mise hors tension de l’emplacement. Le [ `GeographicLocation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs) structure encapsule la latitude et longitude. Le [ `ILocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs) interface définit deux méthodes pour démarrer, suspendre la mise hors tension de l’emplacement et un événement lorsqu’un nouvel emplacement est disponible.

#### <a name="the-ios-location-manager"></a>Le Gestionnaire d’emplacement iOS

L’implémentation d’e/s de `ILocationTracker` est un [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs) classe qu’utiliser d’iOS [ `CLLocationManager` ](https://developer.xamarin.com/api/type/CoreLocation.CLLocationManager/).

#### <a name="the-android-location-manager"></a>Le Gestionnaire d’emplacement Android

L’implémentation Android de `ILocationTracker` est un [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs) classe qui utilise le Android [ `LocationManager` ](https://developer.xamarin.com/api/type/Android.Locations.LocationManager/) classe.

#### <a name="the-windows-runtime-geo-locator"></a>Le localisateur de géo-réplication Windows Runtime

L’implémentation de Windows Runtime de `ILocationTracker` est un [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs) classe qui utilise la plateforme Windows universelle [ `Geolocator` ](https://msdn.microsoft.com/library/windows/apps/br225534).

### <a name="display-the-phones-location"></a>Afficher l’emplacement du téléphone

Le [ **WhereAmI** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI) utilise le suivi de l’emplacement pour afficher l’emplacement du téléphone, à la fois dans le texte et sur une carte équirectangulaire.

### <a name="the-required-overhead"></a>Le requis de surcharge

Une surcharge est requise pour **WhereAmI** à utiliser le suivi de l’emplacement. Premier, tous les projets dans le **WhereAmI** solution doit avoir des références aux projets correspondants dans **Xamarin.FormsBook.Platform**et chaque **WhereAmI** projet doit appeler le `Toolkit.Init` (méthode).

Une surcharge supplémentaire spécifique à la plateforme, sous la forme d’autorisations de l’emplacement est requise.

#### <a name="location-permission-for-ios"></a>Autorisation de l’emplacement pour iOS

Pour iOS, le **info.plist** fichier doit inclure les éléments contenant le texte d’une question à demander à l’utilisateur pour permettre la mise en route d’emplacement de l’utilisateur.

#### <a name="location-permissions-for-android"></a>Autorisations de localisation pour Android

Les applications Android qui obtiennent l’emplacement de l’utilisateur doivent avoir une autorisation ACCESS_FILE_LOCATION dans le fichier AndroidManifest.xml.

#### <a name="location-permissions-for-the-windows-runtime"></a>Autorisations de l’emplacement pour le Windows Runtime

Une application Windows ou Windows Phone doit avoir un `location` marquées de fonctionnalité de l’appareil dans le fichier Package.appxmanifest.

## <a name="working-with-xamarinformsmaps"></a>Utilisation de Xamarin.Forms.Maps

Plusieurs conditions requises ne participent pas à l’aide de la `Map` classe.

### <a name="the-nuget-package"></a>Le package NuGet

Le **Xamarin.Forms.Maps** NuGet bibliothèque doit être ajouté à la solution d’application. Le numéro de version doit être le même que le **Xamarin.Forms** package actuellement installé.

### <a name="initializing-the-maps-package"></a>L’initialisation de l’ensemble de cartes

Les projets d’application doivent appeler la `Xamarin.FormsMaps.Init` méthode après avoir effectué un appel à `Xamarin.Forms.Forms.Init`.

### <a name="enabling-map-services"></a>L’activation des services de mappage

Étant donné que la `Map` peut obtenir l’emplacement de l’utilisateur, l’application doit obtenir l’autorisation de l’utilisateur de la manière décrite plus haut dans ce chapitre :

#### <a name="enabling-ios-maps"></a>L’activation iOS mappe

Une application iOS en utilisant `Map` a besoin de deux lignes dans le fichier info.plist.

#### <a name="enabling-android-maps"></a>L’activation de Android mappe

Une clé d’autorisation est requise pour l’utilisation des services de mappage de Google. Cette clé est insérée dans le **AndroidManifest.xml** fichier. En outre, le **AndroidManifest.xml** fichier nécessite `manifest` balises nécessaires pour obtenir de l’utilisateur.

#### <a name="enabling-windows-runtime-maps"></a>L’activation de Windows Runtime mappe

Une application Windows Runtime nécessite une clé d’autorisation pour l’utilisation de Bing Maps. Cette clé est passée comme argument à la `Xamarin.FormsMaps.Init` (méthode). L’application doit également être activée pour les services d’emplacement.

### <a name="the-unadorned-map"></a>La carte sans ornement

Le [ **MapDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos) exemple se compose d’un [MapsDemoHomePage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml) fichier et [MapsDemoHomePage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs) fichier code-behind Permet d’accéder à différents programmes de démonstration.

Le [BasicMapPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml) fichier montre comment afficher le [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) vue. Par défaut, il affiche la ville de Rome, mais le mappage peut être manipulé par l’utilisateur.

Pour désactiver le défilement horizontal et vertical, définissez la [ `HasScrollEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.HasScrollEnabled/) propriété `false`. Pour désactiver le zoom, définissez [ `HasZoomEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.HasZoomEnabled/) à `false`. Ces propriétés peut ne pas fonctionnent sur toutes les plateformes.

### <a name="streets-and-terrain"></a>Des rues et Terrain

Vous pouvez afficher différents types de cartes en définissant le `Map` propriété [ `MapType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.MapType/) de type [ `MapType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapType/), une énumération avec trois membres :

- [`Street`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Street/), la valeur par défaut
- [`Satellite`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Satellite/)
- [`Hybrid`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Hybrid/)

Le [MapTypesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml) fichier montre comment utiliser une case d’option pour sélectionner le type de carte. Il utilise le [ `RadioButtonManager` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs) classe dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque et une classe basée sur le [MapTypeRadioButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml) fichier.

### <a name="map-coordinates"></a>Coordonnées de la carte

Un programme peut obtenir la zone actuelle qui le `Map` affiche via le [ `VisibleRegion` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.VisibleRegion/) propriété. Cette propriété est *pas* soutenu par une propriété pouvant être liée, et il n’existe aucun mécanisme de notification pour indiquer quand il a changé, par conséquent, un programme qui souhaite surveiller la propriété doit utiliser probablement un minuteur à cet effet.

`VisibleRegion` est de type [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/), une classe avec quatre propriétés en lecture seule :

- [`Center`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.Center/) de type [`Position`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/)
- [`LatitudeDegrees`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.LatitudeDegrees/) de type `double`, qui indique la hauteur de la zone affichée de la carte
- [`LongitudeDegrees`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.LongitudeDegrees/) de type `double`, indiquant la largeur de la zone affichée de la carte
- [`Radius`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.Radius/) de type [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/), indiquant la taille de la plus grande zone circulaire visible sur la carte

`Position` et `Distance` sont les deux structures. `Position` définit deux propriétés en lecture seule définies via la [ `Position` constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Position.Position/p/System.Double/System.Double/):

- [`Latitude`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Position.Latitude/)
- [`Longitude`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Position.Longitude/)

`Distance` vise à fournir une distance indépendants de l’unité par la conversion entre métrique et unités. A `Distance` valeur permettre être créée de plusieurs façons :

- [`Distance` constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Distance.Distance/p/System.Double/) à une distance en mètres
- [`Distance.FromMeters`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromMeters/p/System.Double/) méthode statique
- [`Distance.FromKilometers`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromKilometers/p/System.Double/) méthode statique
- [`Distance.FromMiles`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromMiles/p/System.Double/) méthode statique

La valeur est disponible à partir de trois propriétés :

- [`Meters`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Meters/) de type `double`
- [`Kilometers`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Kilometers/) de type `double`
- [`Miles`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Miles/) de type `double`

Le [MapCoordinatesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml) fichier contient plusieurs `Label` éléments pour afficher les `MapSpan` plus d’informations. Le [MapCoordinatesPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs) fichier code-behind utilise un minuteur pour conserver les informations de mise à jour lorsque l’utilisateur manipule la carte.

### <a name="position-extensions"></a>Extensions de position

Une nouvelle bibliothèque pour cet ouvrage nommé [ **Xamarin.FormsBook.Toolkit.Maps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps) contient des types spécifiques de mappage mais indépendant de la plateforme. Le [ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) classe a un `ToString` méthode pour `Position`et une méthode pour calculer la distance entre deux `Position` valeurs.

### <a name="setting-an-initial-location"></a>Définissez un emplacement initial

Vous pouvez appeler la [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion/p/Xamarin.Forms.Maps.MapSpan/) méthode `Map` pour définir par programme un emplacement et niveau de zoom sur la carte. L’argument est de type `MapSpan`. Vous pouvez créer un `MapSpan` de l’objet à l’aide d’une des opérations suivantes :

- [`MapSpan` constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.MapSpan.MapSpan/p/Xamarin.Forms.Maps.Position/System.Double/System.Double/) avec un `Position`et l’intervalle de latitude et longitude
- [`MapSpan.FromCenterAndRadius`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius/p/Xamarin.Forms.Maps.Position/Xamarin.Forms.Maps.Distance/) avec un `Position` et radius

Il est également possible de créer un nouveau `MapSpan` à partir d’un à l’aide des méthodes [ `ClampLatitude` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.ClampLatitude/p/System.Double/System.Double/) ou [ `WithZoom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.WithZoom/p/System.Double/).

Le [WyomingPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml) fichier et [WyomingPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs) fichier code-behind montre comment utiliser la `MoveToRegion` méthode pour afficher l’état du Wyoming.

Vous pouvez également utiliser le [ `Map` constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Map.Map/p/Xamarin.Forms.Maps.MapSpan/) avec un `MapSpan` objet pour initialiser l’emplacement de la carte. Le [XamarinHQPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml) fichier montre comment effectuer cette opération entièrement en XAML pour afficher le siège social de Xamarin à San Francisco.

### <a name="dynamic-zooming"></a>Zoom dynamique

Vous pouvez utiliser un `Slider` dynamiquement un zoom avant d’une carte. Le [RadiusZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml) fichier et [RadiusZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs) fichier code-behind montrent comment modifier le rayon d’une table basée sur la `Slider` valeur.

Le [LongitudeZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml) fichier et [LongitudeZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs) fichier code-behind afficher une autre approche qui fonctionne mieux sur Android, mais aucune de ces approches fonctionne correctement sur Windows plateformes.

### <a name="the-phones-location"></a>L’emplacement du téléphone

Le [ `IsShowingUser` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.IsShowingUser/) propriété du `Map` fonctionne un peu différemment sur les trois plateformes que le [ShowLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml) fichier montre :

- Sur iOS, un point bleu indique l’emplacement du téléphone, mais vous devez accéder manuellement
- Sur Android, une icône s’affiche que quand vous appuyez déplace le mappage à l’emplacement du téléphone
- La plateforme Windows universelle est similaire à iOS mais parfois automatiquement accède à l’emplacement

Le **MapDemos** projet tente de reproduire l’approche Android en premier définissant un bouton basé sur l’icône basé sur le [MyLocationButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml) fichier et [MyLocationButton.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs) fichier code-behind.

Le [GoToLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml) fichier et [GoToLocationPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs) fichier code-behind Utilisez ce bouton pour accéder à l’emplacement du téléphone.

### <a name="pins-and-science-museums"></a>Musées de science et codes confidentiels

Enfin, le `Map` classe définit un [ `Pins` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.Pins/) propriété de type `IList<Pin>`. Le [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/) classe définit quatre propriétés :

- [`Label`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Label/) de type `string`, une propriété requise
- [`Address`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Address/) de type `string`, une adresse contrôlable de visu facultatif
- [`Position`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Position/) de type `Position`, qui indique où le code confidentiel est affiché sur la carte
- [`Type`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Type/) de type [ `PinType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.PinType/), une énumération, qui n’est pas utilisée

Le **MapDemos** projet contient le fichier [ScienceMuseums.xml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml), qui répertorie les musées science aux États-Unis, et [ `Locations` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs) et [ `Site` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs) des classes pour la désérialisation de ces données.

Le [ScienceMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml) fichier et [ScienceMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs) codes confidentiels complet de fichier code-behind pour ces musées science dans le mappage. Quand l’utilisateur appuie sur un code confidentiel, elle affiche l’adresse et un site Web pour le musée.

### <a name="the-distance-between-two-points"></a>La distance entre deux points

Le [ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) classe contient un [ `DistanceTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88) méthode avec un calcul simplifié de la distance entre deux emplacements géographiques.

Cela est utilisé dans le [LocalMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml) fichier et [LocalMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs) fichier code-behind pour afficher également la distance à la musée à partir de l’emplacement de l’utilisateur :

[![Capture d’écran de triple de la Page musées locale](images/ch28fg28-small.png "Distance vers un emplacement")](images/ch28fg28-large.png#lightbox "Distance vers un emplacement")

Le programme montre également comment dynamiquement restreindre le nombre de codes confidentiels en fonction de l’emplacement de la carte.

## <a name="geocoding-and-back-again"></a>Le géocodage et revenir

Le [ **Xamarin.Forms.Maps** ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) assembly contient également un [ `Geocoder` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Geocoder/) classe avec un [ `GetPositionsForAddressAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync/p/System.String/) méthode convertit une adresse de texte en zéro ou plus possibles positions géographiques et une autre méthode [ `GetAddressesForPositionAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync/p/Xamarin.Forms.Maps.Position/) qui convertit dans l’autre direction.

Le [GeocoderRoundTrip.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml) fichier et [GeocoderRoundTrip.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs) fichier code-behind illustrent cette fonctionnalité.



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 28 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)
- [Exemples de chapitre 28](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [Contrôle de carte](~/xamarin-forms/user-interface/map.md)
