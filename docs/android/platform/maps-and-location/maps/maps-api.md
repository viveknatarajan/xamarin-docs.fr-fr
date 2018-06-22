---
title: API de mappages
ms.prod: xamarin
ms.assetid: C0589878-2D04-180E-A5B9-BB41D5AF6E02
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: fc16178a4068b2dcf22fc19047e0ef403e83633f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30773522"
---
# <a name="maps-api"></a>API de mappages

À l’aide de l’application de cartes est très utile, mais parfois vous souhaitez inclure les mappages directement dans votre application. En plus de la fonction intégrée mappe application, Google offre également une [API de mappage native pour Android](https://developers.google.com/maps/documentation/android/).
L’API de cartes est adapté aux cas où vous souhaitez gérer davantage de contrôle sur l’expérience de mappage. Les éléments qui sont possibles avec l’API de cartes sont :

-  Par programme, la modification du point de vue de la carte.
-  Ajout et la personnalisation des marqueurs.
-  Annoter un mappage des superpositions.

Contrairement aux maintenant déconseillé API Android de Google Maps v1, v2 d’API Android de Google Maps fait partie de [Services Google Play](http://developer.android.com/google/play-services/index.html).
Par conséquent, il est nécessaire répondre à certaines conditions préalables requises obligatoires avant qu’il soit possible d’utiliser l’API Android de Google Maps dans une application Xamarin.Android.


## <a name="google-maps-api-prerequisites"></a>Mappe les conditions préalables des API Google

Plusieurs éléments doivent être configurés avant de pouvoir utiliser l’API de Maps, y compris :

-  Installer les Kit de développement de Services Google Play
-  Créer un émulateur avec les APIs Google
-  Obtenir une clé API Maps
-  Spécifier les autorisations requises



### <a name="install-the-google-play-services-sdk"></a>Installer les Kit de développement de Services Google Play

Services Google Play est une technologie à partir de Google qui permet aux applications Android tirer parti des diverses fonctionnalités de Google tels que Google +, dans l’application de facturation et des cartes. Ces fonctionnalités sont accessibles sur les appareils Android en tant que services en arrière-plan qui sont contenus dans le [APK de Services Google Play](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en).

Les applications Android d’interagir avec les Services Google Play via la bibliothèque cliente Services Google Play. Cette bibliothèque contient les interfaces et les classes pour les services individuels, tels que des cartes. Le diagramme suivant montre la relation entre une application Android et les Services Google Play :

![Diagramme illustrant la mise à jour de Google Play Services APK Google Play Store](maps-api-images/play-services-diagram.png)

L’API Android de cartes est fournie dans le cadre de Services Google Play.
Avant d’une application de Xamarin.Android peut utiliser l’API Maps, le Kit de développement Google Play Services doit être installé et lié. Le Kit de développement Google Play Services est installé via le Gestionnaire de kit de développement logiciel Android. La capture d’écran suivante indique où dans le Gestionnaire de SDK Android se trouve le client des services Google Play :

![Services Google Play apparaît sous fonctionnalités supplémentaires dans le Gestionnaire de kit de développement logiciel Android.](maps-api-images/image01.png)

> [!NOTE]
> Les services Google Play APK est un produit sous licence peut ne pas exister sur tous les appareils. Si elle n’est pas installé, cartes Google Maps ne fonctionnera pas sur l’appareil.


#### <a name="binding-google-play-services"></a>Services de liaison Google Play

Une fois que la bibliothèque cliente Services Google Play est installée, il doit être lié à une bibliothèque de liaison de Xamarin.Android Java. Il existe deux façons de procéder :

-  **Utilisez le package NuGet de cartes de Services de lecture de Google** -c’est l’approche la plus simple (uniquement disponible dans Xamarin.Android 4.8 ou ultérieure).
   Installer le [Xamarin Google Play Services Maps NuGet](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Maps); il installe également tous les packages de dépendance de Services Google Play.
   Le reste de ce guide se concentre sur cette approche.

-  **Lier manuellement la bibliothèque cliente Services Google Play** -il s’agit d’une approche plus complexe qui est la seule façon pour Xamarin.Android 4.4 ou Xamarin.Android 4.6 lier le SDK des Services Google Play.
   Manuellement la liaison de la bibliothèque cliente Services Google Play est abordée dans ce document, mais vous trouverez un exemple de procédure à suivre dans le [cartes et emplacement démonstration v3 exemple](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3) sur Github.


#### <a name="adding-the-google-play-services-map-package"></a>Ajout du Package de carte de Services Google Play

Pour ajouter le package de la carte de Services Google Play, bouton droit sur le **références** dossier de votre projet dans l’Explorateur de solutions et cliquez sur **gérer les Packages NuGet...** :

![Élément de menu contextuel affichant l’Explorateur de solutions gérer les Packages NuGet sous références](maps-api-images/image02.png)

Cette opération ouvre le **Gestionnaire de Package NuGet**. Cliquez sur **Parcourir** et entrez **Xamarin Google Play Services Maps** dans le champ de recherche. Sélectionnez **Xamarin.GooglePlayServices.Maps** et cliquez sur **installer**. (Si ce package a été installé précédemment, cliquez sur **mise à jour**.) :

[![Gestionnaire de Package NuGet avec package Xamarin.GooglePlayServices.Maps sélectionné](maps-api-images/image03-sml.png)](maps-api-images/image03.png#lightbox)

Notez que les packages de dépendance suivants sont également installés :

-   **Xamarin.GooglePlayServices.Base**
-   **Xamarin.GooglePlayServices.Basement**
-   **Xamarin.GooglePlayServices.Tasks**



### <a name="create-an-emulator-with-google-apis"></a>Créer un émulateur avec les API de Google

Bien qu’il n’est pas recommandé, il est possible de configurer l’émulateur pour prendre en charge l’API Android de cartes. L’émulateur doit être configuré pour cibler le 17 de niveau API Google (Android 4.2.2) ou une version ultérieure. La capture d’écran ci-dessous, une image de l’émulateur est configurée pour API niveau 19 : 

![Gestionnaire de l’émulateur Android avec un AVD configuré pour l’API niveau 19](maps-api-images/image04.png)



### <a name="obtain-a-google-maps-api-key"></a>Obtenir une clé d’API Google Maps

L’étape finale consiste pour obtenir une clé API de Google Maps (Notez que vous ne pouvez pas réutiliser une clé d’API V1 de Google Maps hérité). Pour plus d’informations sur la façon d’obtenir et d’utiliser la clé d’API avec Xamarin.Android, consultez [clé d’API obtention A Google Maps](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).
 


### <a name="specify-the-required-permissions"></a>Spécifier les autorisations requises

Les autorisations suivantes doivent être spécifiées dans le **AndroidManifest.XML** pour l’API Android de Google Maps :

-  **Accès à l’état du réseau** &ndash; l’API Maps doit être en mesure de vérifier s’il peut télécharger les mosaïques.

-  **Accès à Internet** &ndash; accès à Internet est nécessaire pour télécharger les vignettes de carte et de communiquer avec les serveurs de lecture de Google pour l’accès aux API.

-  **OpenGL ES v2** &ndash; l’application doit déclarer la configuration requise pour OpenGL ES v2.

-  **Clé d’API Google Maps** &ndash; clé de l’API est utilisée pour vérifier que l’application est inscrit et autorisée à utiliser les Services Google Play. Consultez [obtenir une clé d’API Google Maps](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md) pour plus d’informations sur cette clé.

-  **Écrire dans le stockage externe** &ndash; l’API Android Maps de Google mettra en cache les vignettes téléchargés vers le stockage externe.

-  **Accès aux Services Web de Google basée sur** &ndash; l’application a besoin d’autorisations pour accéder aux services web de Google qui assurent l’API Android de cartes.

-  **Autorisations pour les Notifications de Services Google Play** &ndash; l’application doit être autorisée à recevoir des notifications à distance à partir des Services Google Play.

-  **Accès aux fournisseurs de localisation** &ndash; il s’agit d’autorisations facultatives.
   Ils permettent la `GoogleMap` classe pour afficher l’emplacement de l’appareil sur la carte.


L’extrait de code suivant est un exemple des paramètres qui doit être ajouté à **AndroidManifest.XML**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionName="4.5" package="com.xamarin.docs.android.mapsandlocationdemo2" android:versionCode="6">
    <uses-sdk android:minSdkVersion="14" android:targetSdkVersion="17" />

    <!-- Google Maps for Android v2 requires OpenGL ES v2 -->
    <uses-feature android:glEsVersion="0x00020000" android:required="true" />

    <!-- We need to be able to download map tiles and access Google Play Services-->
    <uses-permission android:name="android.permission.INTERNET" />

    <!-- Allow the application to access Google web-based services. -->
    <uses-permission android:name="com.google.android.providers.gsf.permission.READ_GSERVICES" />

    <!-- Google Maps for Android v2 will cache map tiles on external storage -->
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

    <!-- Google Maps for Android v2 needs this permission so that it may check the connection state as it must download data -->
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <!-- Permission to receive remote notifications from Google Play Services -->
    <!-- Notice here that we have the package name of our application as a prefix on the permissions. -->
    <uses-permission android:name="<PACKAGE NAME>.permission.MAPS_RECEIVE" />
    <permission android:name="<PACKAGE NAME>.permission.MAPS_RECEIVE" android:protectionLevel="signature" />

    <!-- These are optional, but recommended. They will allow Maps to use the My Location provider. -->
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />


    <application android:label="@string/app_name">
        <!-- Put your Google Maps V2 API Key here. -->
        <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
        <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
    </application>
</manifest>
```


## <a name="the-googlemap-class"></a>La classe GoogleMap

Une fois que les conditions préalables sont pris en charge, il est temps de commencer à développer l’application et utiliser l’API Android de cartes. Le [GoogleMap](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap) classe est l’API principale une application Xamarin.Android utilisera pour afficher et interagir avec un Google Maps pour Android. Cette classe possède les responsabilités suivantes :

-  Interaction avec les services de Google Play pour autoriser l’application avec le service web de Google.

-  Téléchargement, la mise en cache et afficher les vignettes de carte.

-  Affichage des contrôles d’interface utilisateur tels que de panoramique et de zoom à l’utilisateur.

-  Marqueurs de dessin et des formes géométriques sur des cartes.

Le `GoogleMap` est ajouté à une activité de deux manières :

-  **MapFragment** - le [MapFragment](http://developer.android.com/reference/com/google/android/gms/maps/MapFragment.html) est un Fragment spécialisé qui agit en tant qu’hôte pour le `GoogleMap` objet. Le `MapFragment` requiert un niveau d’API Android 12 ou supérieur.
   Les versions antérieures d’Android peuvent utiliser le [SupportMapFragment](http://developer.android.com/reference/com/google/android/gms/maps/SupportMapFragment.html).

-  **MapView** - le [MapView](https://developer.xamarin.com/api/type/Android.GoogleMaps.MapView/) est une sous-classe de vue spécialisée qui peut agir comme un hôte pour un `GoogleMap` objet. Les utilisateurs de cette classe doivent transférer toutes les méthodes de cycle de vie des activités à la `MapView` classe.

Chacun de ces conteneurs exposent une `Map` propriété qui retourne une instance de `GoogleMap`. Préférence doit être accordée à la [MapFragment](http://developer.android.com/reference/com/google/android/gms/maps/MapFragment.html) classe, comme c’est une API simple qui permet de réduire le code réutilisable quantité qu’un développeur doit implémenter manuellement.


### <a name="adding-a-mapfragment-to-an-activity"></a>Ajout d’un MapFragment à une activité

La capture d’écran suivante est un exemple très simple `MapFragment`:

[![Capture d’écran d’un périphérique d’affichage d’un fragment de mappage](maps-api-images/image05-sml.png)](maps-api-images/image05.png#lightbox)

Similaire à d’autres classes de Fragment, il existe deux façons d’ajouter ce `MapFragment` à une activité :

-   **Déclarative** - le `MapFragment` peuvent être ajoutés via le fichier XML de la disposition de l’activité. L’extrait de code XML suivant montre un exemple montrant comment utiliser le `fragment` élément :

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <fragment xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/map"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              class="com.google.android.gms.maps.MapFragment" />
    ```

-   **Par programmation** - le `MapFragment` peuvent être ajoutés par programmation, comme décrit ci-après.

Pour ajouter par programmation un `MapFragment`, votre activité doit implémenter la `IOnMapReadyCallback` interface. Étant donné que l’initialisation d’un `GoogleMap` objet peut prendre du temps comme l’API communique avec Google Play, vous devez fournir un rappel qui notifie votre application lorsque le `GoogleMap` est prêt.

Tout d’abord, ajoutez `IOnMapReadyCallback` à la `Activity` déclaration de classe.
Par exemple :

```csharp
public class MapWithMarkersActivity : Activity, IOnMapReadyCallback
```

Ensuite, dans le `OnCreate` (méthode), ajouter le `MapFragment` comme indiqué dans l’exemple de code suivant (le `GoogleMapOptions` classe est expliquée plus loin dans ce guide) :

```csharp
_mapFragment = FragmentManager.FindFragmentByTag("map") as MapFragment;
if (_mapFragment == null)
{
    GoogleMapOptions mapOptions = new GoogleMapOptions()
        .InvokeMapType(GoogleMap.MapTypeSatellite)
        .InvokeZoomControlsEnabled(false)
        .InvokeCompassEnabled(true);

    FragmentTransaction fragTx = FragmentManager.BeginTransaction();
    _mapFragment = MapFragment.NewInstance(mapOptions);
    fragTx.Add(Resource.Id.map, _mapFragment, "map");
    fragTx.Commit();
}
_mapFragment.GetMapAsync(this);
```

A `GoogleMap` doit être acquis à l’aide de `GetMapAsync`, comme illustré à la fin de l’exemple de code précédent &ndash; ce code initialise automatiquement le système de mappages et de la vue. (Notez que cette méthode n’utilise pas `await` / `async` sémantique &ndash; le `Async` comportement est implémenté par Android.) Lorsque le `GoogleMap` objet est prêt, Android appelle de votre application `OnMapReady` (méthode) (que vous devez implémenter dans le cadre de la `IOnMapReadyCallback` interface). Par exemple :

```csharp
public void OnMapReady (GoogleMap map)
{
    _map = map;
}
```

Dans l’exemple de code ci-dessus, le `OnMapReady` rappel initialise le `_map` variable créé `GoogleMap` objet.

Par exemple, pour l’utilisation de ce résultat, lorsque `OnResume` est appelé, il peut vérifier si `_map` n’est pas null. Si `_map` est définie sur une `GoogleMap` objet `OnResume` peut appeler des méthodes pour ajouter des marques, déplacez son caméra vers un spécifié de longitude et de latitude. Pour obtenir un exemple de code complet, consultez [SimpleMapDemo](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3/SimpleMapDemo).



### <a name="map-types"></a>Types de cartes

Il existe cinq types de cartes disponibles à partir de l’API mappe de Google :

-  **Normal** -il s’agit du type de mappage par défaut. Il montre des routes et ses principales fonctionnalités naturelles, ainsi que certains points synthétiques d’intérêt (par exemple, les bâtiments et les ponts).

-  **Satellite** -cette carte montre la photographie du satellite.

-  **Hybride** : cette carte montre la photographie de satellites et road mappe.

-  **Terrain** -cela indique principalement les fonctionnalités topographiques avec certaines des routes.

-  **Aucun** -ce mappage ne charge pas toutes les mosaïques, il est restitué sous la forme d’une grille vide.


L’image ci-dessous illustre trois des différents types de mappages, de gauche à droite (normal, hybride, terrain) :

[![Trois mappent des captures d’écran : Normal, hybrides et le Terrain](maps-api-images/map-types-sml.png)](maps-api-images/map-types.png#lightbox)

Le `GoogleMap.MapType` propriété est utilisée pour définir ou modifier le type de carte s’affiche. L’extrait de code suivant montre comment afficher une carte satellite.

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.MapType = GoogleMap.MapTypeSatellite;
}
```


### <a name="googlemap-properties"></a>Propriétés de GoogleMap

`GoogleMap` définit plusieurs propriétés qui peuvent contrôler les fonctionnalités et l’apparence de la carte. Une façon de configurer l’état initial d’un `GoogleMap` consiste à passer un [GoogleMapOptions](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMapOptions.html) lors de la création de l’objet un `MapFragment`. L’extrait de code suivant est un exemple d’utilisation une `GoogleMapOptions` lors de la création de l’objet un `MapFragment`:

```csharp
GoogleMapOptions mapOptions = new GoogleMapOptions()
    .InvokeMapType(GoogleMap.MapTypeSatellite)
    .InvokeZoomControlsEnabled(false)
    .InvokeCompassEnabled(true);

FragmentTransaction fragTx = FragmentManager.BeginTransaction();
_mapFragment = MapFragment.NewInstance(mapOptions);
fragTx.Add(Resource.Id.map, _mapFragment, "map");
fragTx.Commit();
```

L’autre méthode permettant de configurer un `GoogleMap` objet est en définissant les valeurs du [UiSettings](http://developer.android.com/reference/com/google/android/gms/maps/UiSettings.html) propriété de l’objet de mappage. L’exemple de code suivant montre comment configurer un `GoogleMap` pour afficher les contrôles de zoom et d’une boussole :

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.UiSettings.ZoomControlsEnabled = true;
    _map.UiSettings.CompassEnabled = true;
}
```


## <a name="interacting-with-the-map"></a>Interaction avec la carte

L’API de cartes Android fournit des API qui autorise une activité modifier le point de vue, ajouter des marques, placez des superpositions personnalisées ou dessiner des formes géométriques. Cette section explique comment accomplir certaines de ces tâches dans Xamarin.Android.

### <a name="changing-the-viewpoint"></a>La modification du point de vue

Cartes sont un plan plat conforme sur l’écran, en fonction de la projection Mercator. La vue cartographique est celle d’un *caméra* recherchent verticalement sur ce plan. La position de l’appareil photo peut être contrôlée en modifiant l’emplacement, le zoom, l’inclinaison et incidence. Le [CameraUpdate](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdate) classe est utilisée pour déplacer l’emplacement de l’appareil photo. `CameraUpdate` les objets ne sont pas directement instanciées, à la place l’API Maps fournit le [CameraUpdateFactory](http://developer.android.com/reference/com/google/android/gms/maps/CameraUpdateFactory.html) classe.

Une fois un `CameraUpdate` objet a été créé, il est passé comme paramètre soit le [GoogleMap.MoveCamera](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap.html#moveCamera(com.google.maps.CameraUpdate)) ou [GoogleMap.AnimateCamera](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap.html#animateCamera(com.google.maps.CameraUpdate)) méthodes. Le `MoveCamera` méthode met à jour la carte instantanément lors de la `AnimateCamera` méthode fournit une transition fluide et animée.

Cet extrait de code est un exemple simple de l’utilisation de la `CameraUpdateFactory` pour créer un `CameraUpdate` qui incrémente le niveau de zoom de la carte d’une unité :

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.MoveCamera(CameraUpdateFactory.ZoomIn());
}
```

L’API de mappages fournit un [CameraPosition](http://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.html) qui agrège toutes les valeurs possibles pour la position de la caméra. Une instance de cette classe peut être fournie pour le [CameraUpdateFactory.NewCameraPosition](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdateFactory#newCameraPosition(com.google.android.gms.maps.model.CameraPosition)) méthode qui retournera un `CameraUpdate` objet. L’API de cartes inclut également le [CameraPosition.Builder](http://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.Builder.html) classe qui fournit une API fluent création `CameraPosition` objets.
L’extrait de code suivant montre un exemple de création d’un `CameraUpdate` d’un `CameraPosition` et l’utiliser pour modifier la position de la caméra sur un `GoogleMap`:

```csharp
LatLng location = new LatLng(50.897778, 3.013333);
CameraPosition.Builder builder = CameraPosition.InvokeBuilder();
builder.Target(location);
builder.Zoom(18);
builder.Bearing(155);
builder.Tilt(65);
CameraPosition cameraPosition = builder.Build();
CameraUpdate cameraUpdate = CameraUpdateFactory.NewCameraPosition(cameraPosition);

MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.MoveCamera(cameraUpdate);
}
```

Dans l’extrait de code précédent, un emplacement spécifique sur la carte est représenté par l’un [LatLng](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/LatLng) classe. Le niveau de zoom est défini à 18. L’incidence est la mesure boussole dans le sens horaire à partir du Nord. L’inclinaison propriété détermine l’angle de visualisation et qu’elle spécifie un angle de 25 degrés à la verticale. La capture d’écran suivante affiche la `GoogleMap` après l’exécution du code précédent :

[![Exemple de mappage de Google montrant un emplacement spécifié avec un texte incliné angle d’affichage](maps-api-images/image06-sml.png)](maps-api-images/image06.png#lightbox)


### <a name="drawing-on-the-map"></a>Dessin sur la carte

L’API Android de mappages fournit l’API de dessin des éléments suivants sur une carte :

-  **Marqueurs** -il s’agit des icônes spéciales qui sont utilisés pour identifier un emplacement unique sur une carte.

-  **Superpositions** -il s’agit d’une image qui peut être utilisée pour identifier une collection d’emplacements ou de la zone sur la carte.

-  **Polygones, lignes et des cercles** -il s’agit d’API qui permettent d’ajouter des formes à une carte des activités.


#### <a name="markers"></a>Markers

L’API de mappages fournit un [marqueur](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/Marker) classe qui encapsule toutes les données sur un emplacement unique sur une carte. Par défaut, ils utilisent une icône standard fournie par Google Maps. Il est possible de personnaliser l’apparence d’un marqueur et pour répondre aux clics d’utilisateur.


##### <a name="adding-a-marker"></a>Ajout d’un marqueur

Pour ajouter un marqueur à un mappage, il est nécessaire créer un nouveau [MarkerOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions) de l’objet, puis appelez le [AddMarker](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addMarker(com.google.android.gms.maps.model.MarkerOptions)) méthode sur un `GoogleMap` instance. Cette méthode retourne un [marqueur](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/Marker) objet.

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");
    _map.AddMarker(marker1);
}
```

Le titre de la marque s’affichera dans un *fenêtre info* lorsque l’utilisateur appuie sur le marqueur. La capture d’écran suivante montre à quoi ressemble ce marqueur :

[![Exemple de mappage avec un marqueur et d’une fenêtre d’information pour la ligne en relief Vimy Google](maps-api-images/image07-sml.png)](maps-api-images/image07.png#lightbox)


##### <a name="customizing-a-marker"></a>Personnalisation d’un marqueur

Il est possible de personnaliser l’icône utilisée par le marqueur en appelant le `MarkerOptions.InvokeIcon` méthode lors de l’ajout de la marque pour la carte.
Cette méthode prend un [BitmapDescriptor](http://developer.android.com/reference/com/google/android/gms/maps/model/BitmapDescriptor.html) objet contenant les données nécessaires pour restituer l’icône. Le [BitmapDescriptorFactory](https://developer.android.com/reference/com/google/android/gms/maps/model/BitmapDescriptorFactory.html) classe fournit des méthodes d’assistance pour simplifier la création d’un `BitmapDescriptor`. La liste suivante présente certaines de ces méthodes :

-   `DefaultMarker(float colour)` &ndash; Utilisez le marqueur de Google Maps par défaut, mais changer la couleur.

-   `FromAsset(string assetName)` &ndash; Utilisez une icône personnalisée à partir du fichier spécifié dans le dossier de ressources.

-   `FromBitmap(Bitmap image)` &ndash; Utilisez la bitmap spécifiée en tant que l’icône.

-   `FromFile(string fileName` &ndash; Créer l’icône personnalisée à partir du fichier dans le chemin d’accès spécifié.

-   `FromResource(int resourceId)` &ndash; Créer une icône personnalisée à partir de la ressource spécifiée.

L’extrait de code suivant montre un exemple de création d’un marqueur par défaut de couleur cyan :

```csharp
mapFrag.GetMapAsync(this);
...
if (_map != null)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");
    markerOpt1.InvokeIcon(BitmapDescriptorFactory.DefaultMarker (BitmapDescriptorFactory.HueCyan));
    _map.AddMarker(marker1);
}
```


#### <a name="info-windows"></a>Informations sur Windows

*Fenêtres d’informations* sont spéciales windows ce menu contextuel pour afficher des informations à l’utilisateur lors de l’action de cliquer sur un marqueur spécifique. Par défaut, la fenêtre d’informations s’affichent le contenu du titre de la marque. Si le titre n’a pas été assigné, aucun fenêtre info ne s’affiche. Une seule fenêtre d’informations peut s’afficher à la fois.

Il est possible de personnaliser la fenêtre d’information en implémentant la [GoogleMap.IInfoWindowAdapter](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap.InfoWindowAdapter) interface. Il existe deux méthodes importantes sur cette interface :

-  `public View GetInfoWindow(Marker marker)` &ndash; Cette méthode est appelée pour obtenir une fenêtre d’informations personnalisé pour un marqueur. Si elle retourne `null` , le rendu de fenêtre par défaut sera utilisée. Si cette méthode retourne une vue, cette vue sera placée à l’intérieur du frame de fenêtre d’informations.

-  `public View GetInfoContents(Marker marker)` &ndash; Cette méthode est uniquement être appelée si GetInfoWindow retourne `null` . Cette méthode peut retourner un `null` valeur si le rendu par défaut, le contenu de la fenêtre info doit être utilisé. Sinon, cette méthode doit retourner une vue avec le contenu de la fenêtre d’informations.

Une fenêtre d’informations n’est pas un affichage en temps réel : à la place Android convertit l’affichage d’une bitmap statique et qui affiche sur l’image. Cela signifie qu’une fenêtre d’informations ne peut pas répondre à des événements tactiles ou les mouvements, ni il actualise automatiquement lui-même. Pour mettre à jour d’une fenêtre d’informations, il est nécessaire d’appeler le [GoogleMap.ShowInfoWindow](http://developer.android.com/reference/com/google/android/gms/maps/model/Marker.html#showInfoWindow()) (méthode).

L’illustration suivante montre quelques exemples de certaines fenêtres d’informations personnalisé. L’image de gauche a un contenu personnalisé, alors que l’image de droite a sa fenêtre et un contenu personnalisé :

![Exemple des fenêtres de marqueur de Melbourne, y compris l’icône et le remplissage. La fenêtre de droite a des angles arrondis.](maps-api-images/marker-infowindows.png)


#### <a name="ground-overlays"></a>Sol superpositions

Contrairement aux marqueurs qui identifient un emplacement spécifique sur une carte, une [GroundOverlay](http://developer.android.com/reference/com/google/android/gms/maps/model/GroundOverlay.html) est une image qui permet d’identifier une collection d’emplacements ou d’une zone sur la carte.


##### <a name="adding-a-groundoverlay"></a>Ajout d’un GroundOverlay

Ajout d’un segment de recouvrement sol à une carte est très similaire à l’ajout d’un marqueur à un mappage. Tout d’abord, un [GroundOverlayOptions](http://developer.android.com/reference/com/google/android/gms/maps/model/GroundOverlayOptions.html) objet est créé. Cet objet est ensuite passé comme paramètre à la `GoogleMap.AddGroundOverlay` (méthode), qui permet de renvoyer un `GroundOverlay` objet. Cet extrait de code est un exemple d’ajout d’un segment de recouvrement sol à une carte :

```csharp
BitmapDescriptor image = BitmapDescriptorFactory.FromResource(Resource.Drawable.polarbear);
GroundOverlayOptions groundOverlayOptions = new GroundOverlayOptions()
    .Position(position, 150, 200)
    .InvokeImage(image);
GroundOverlay myOverlay = _map.AddGroundOverlay(groundOverlayOptions);
```

La capture d’écran suivante montre ce segment de recouvrement sur une carte :

[![Exemple de mappage avec une image superposée d’un ours polaires](maps-api-images/image09-sml.png)](maps-api-images/image09.png#lightbox)


#### <a name="lines-circles-and-polygons"></a>Lignes, des cercles et des polygones

Il existe trois types simples des illustrations géométriques qui peuvent être ajoutées à une carte :

-  **Polyligne** -il s’agit d’une série de segments de ligne connectés. Il peut marquer un chemin d’accès sur une carte ou toute forme requis.

-  **Polygone** -il s’agit d’une forme fermée pour le marquage des zones sur une carte.

-  **Cercle** -cela sera tracer un cercle sur la carte.



##### <a name="polylines"></a>Polylignes

A [polyligne](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/Polyline) est une liste de consécutifs `LatLng` objets qui spécifient les sommets de chaque segment de ligne. Une polyligne est créée en créant d’abord un `PolylineOptions` objet et en lui ajoutant les points à. Le `PolylineOption` objet est ensuite transmis à un `GoogleMap` objet en appelant le `AddPolyline` (méthode).

```csharp
PolylineOption rectOptions = new PolylineOption();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
rectOptions.Add(new LatLng(37.35, -122.0)); // close the polyline - this makes a rectangle.
myMap.AddPolyline(rectOptions);
```


##### <a name="polygons"></a>Polygones

`Polygon`s sont très semblables aux `Polyline`s, mais ils ne sont pas ouverts terminée. `Polygon`sont une boucle fermée, ont leur intérieur renseigné.
`Polygon`sont créés dans la même manière en tant qu’un `Polyline`, sauf le [GoogleMap.AddPolygon](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addPolygon(com.google.android.gms.maps.model.PolygonOptions)) méthode appelée.

Contrairement à un `Polyline`, un `Polygon` est fermeture automatique. Lorsque `AddPolygon` est appelée, la méthode est à l’automatiquement fermer hors du polygone en dessinant une ligne qui connecte le premier et le dernier point. L’extrait de code suivant crée un rectangle plein sur la même zone que l’extrait de code précédent dans le `Polyline` exemple.

```csharp
PolygonOptions rectOptions = new PolygonOptions();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
// notice we don't need to close off the polygon
myMap.AddPolygon(rectOptions);
```


##### <a name="circles"></a>Circles

Cercles sont créés par la première instanciation d’un [CircleOption](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/CircleOptions) objet que vous spécifierez le centre et le rayon du cercle en mètres. Le cercle est dessiné sur la carte en appelant [GoogleMap.AddCircle](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap#addCircle(com.google.android.gms.maps.model.CircleOptions)).
L’extrait de code suivant montre comment tracer un cercle :

```csharp
CircleOptions circleOptions = new CircleOptions ();
circleOptions.InvokeCenter (new LatLng(37.4, -122.1));
circleOptions.InvokeRadius (1000);
_map.AddCircle (CircleOptions);
```


## <a name="responding-to-events"></a>Réponse aux événements

Il existe trois types d’interactions de qu'un utilisateur peut avoir avec une carte :

-  **Cliquez sur le marqueur** -l’utilisateur clique sur un marqueur.

-  **Faites glisser le marqueur** -l’utilisateur a long-cliqué sur un mparger

-  **Cliquez sur la fenêtre Info** -l’utilisateur a cliqué dans une fenêtre d’informations.

Chacun de ces événements sera abordée plus en détail ci-dessous.


### <a name="marker-click-events"></a>Événements Click de marqueur

Lorsque l’utilisateur clique sur un marqueur de la `MarkerClick` événement est déclenché et un `GoogleMap.MarkerClickEventArgs` passé. Cette classe contient deux propriétés :

-  `GoogleMap.MarkerClickEventArgs.Handled` &ndash; Cette propriété doit être définie sur `true` pour indiquer que le Gestionnaire d’événements consomme l’événement. Si la valeur est `false` le comportement par défaut se produit en plus du comportement personnalisé du Gestionnaire d’événements.

-  `P0` &ndash; Ce paramètre de nom mal est une référence à la marque qui a déclenché la `MarkerClick` événement.


Cet extrait de code montre un exemple d’un `MarkerClick` cela risque de changer la position de la caméra vers un nouvel emplacement sur la carte :

```csharp
private void MapOnMarkerClick(object sender, GoogleMap.MarkerClickEventArgs markerClickEventArgs)
{
    markerClickEventArgs.Handled = true;
    Marker marker = markerClickEventArgs.P0;
    if (marker.Id.Equals(MyMarkerId)) // The ID of a specific marker the user clicked on.
    {
        _map.AnimateCamera(CameraUpdateFactory.NewLatLngZoom(new LatLng(20.72110, -156.44776), 13));
    }
    else
    {
        Toast.MakeText(this, String.Format("You clicked on Marker ID {0}", marker.Id), ToastLength.Short).Show();
    }
}
```


### <a name="marker-drag-events"></a>Événements de marqueur de glissement

Cet événement est déclenché lorsque l’utilisateur souhaite faire glisser le marqueur. Par défaut, les marqueurs ne sont pas déplaçables. Un marqueur peut être défini comme déplaçable en définissant le `Marker.Draggable` propriété `true` ou en appelant le `MarkerOptions.Draggable` méthode avec `true` en tant que paramètre.

Pour tout d’abord, faites glisser le marqueur, l’utilisateur doit long-cliquez sur ce dernier et conserver leur doigt sur la carte. Lorsqu’il est glissé leur doigt sur l’écran, le marqueur se déplace. Lorsque le doigt de l’utilisateur lève hors de l’écran, le marqueur restent en place.

La liste suivante décrit les différents événements qui seront déclenchés pour un marqueur déplaçable :

-   `GoogleMap.MarkerDragStart(object sender, GoogleMap.MarkerDragStartEventArgs e)` &ndash; Cet événement est déclenché lorsque l’utilisateur fait tout d’abord glisser le marqueur.

-   `GoogleMap.MarkerDrag(object sender, GoogleMap.MarkerDragEventArgs e)` &ndash; Cet événement est déclenché comme le marqueur est glissé.

-   `GoogleMap.MarkerDragEnd(object sender, GoogleMap.MarkerDragEndEventArgs e)` &ndash; Cet événement est déclenché lorsque le moment où l’utilisateur a terminé en faisant glisser le marqueur.

Chacun de la `EventArgs` contient une propriété unique appelée `P0` qui est une référence à la `Marker` de l’objet glissé.


### <a name="info-window-click-events"></a>Fenêtre d’informations sur les événements Click

Qu’une fenêtre d’informations permettre être affichée à la fois. Lorsque l’utilisateur clique sur une fenêtre d’informations dans une carte, l’objet map génère une `InfoWindowClick` événement. L’extrait de code suivant montre comment associer un gestionnaire pour l’événement :

```csharp
private bool SetupMapIfNeeded()
{
    if (_map == null)
    {
        _map = _mapFragment.Map;
        if (_map != null)
        {
            _map.InfoWindowClick += MapOnInfoWindowClick;
            return true;
        }
        return false;
    }
    return true;
}

private void MapOnInfoWindowClick (object sender, GoogleMap.InfoWindowClickEventArgs e)
{
    Marker myMarker = e.P0;
    // Do something with marker.
}
```

Rappelez-vous qu’une fenêtre d’info est statique `View` qui est restitué sous la forme d’une image sur la carte. Les widgets tels que des boutons, des cases à cocher ou des vues de texte qui sont placées dans la fenêtre d’information seront inertes et ne peut pas répondre à certaines de leurs événements utilisateur intégraux.



## <a name="related-links"></a>Liens associés

- [Services Google Play](http://developer.android.com/google/play-services/index.html)
- [Google mappe l’API Android v2](https://developers.google.com/maps/documentation/android/)
- [Google Play Services APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)
- [Obtention d’une clé d’API de Google Maps](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [Problème 57880 : Les Services de Google Play mise à jour mais AVD ne pas](https://code.google.com/p/android/issues/detail?id=57880)
