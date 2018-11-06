---
title: À l’aide de l’API de cartes Google dans votre Application
description: Guide pratique pour implémenter des fonctionnalités de v2 de l’API Google Maps dans votre application Xamarin.Android.
ms.prod: xamarin
ms.assetid: C0589878-2D04-180E-A5B9-BB41D5AF6E02
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/07/2018
ms.openlocfilehash: fb7d18e6434e32941531d2c37cd8b938ec21ba90
ms.sourcegitcommit: 28dbb5fa2fbcc7e66cd09c1d0077496fcdefac56
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51027325"
---
# <a name="using-the-google-maps-api-in-your-application"></a>À l’aide de l’API de cartes Google dans votre application

À l’aide de l’application Maps est très utile, mais parfois vous souhaitez inclure des cartes directement dans votre application. Outre l’intégrée mappe application, Google propose également un [API de mappage native pour Android](https://developers.google.com/maps/documentation/android-sdk/intro).
L’API de cartes est approprié pour les cas où vous souhaitez conserver davantage de contrôle sur l’expérience de mappage. Les choses sont possibles avec l’API de cartes sont les suivantes :

-  Modification par programmation le point de vue de la carte.
-  Ajout et la personnalisation des marqueurs.
-  Annoter un mappage avec des superpositions.

Contrairement à la v1 de l’API Android de Google Maps désormais dépréciée, API Android de Google Maps v2 est la partie [Google Play Services](https://developers.google.com/android/guides/overview).
Une application Xamarin.Android doit respecter certaines conditions préalables requises obligatoires avant qu’il soit possible d’utiliser l’API Android de Google Maps.


## <a name="google-maps-api-prerequisites"></a>Conditions préalables de l’API Google Maps

Plusieurs étapes doivent être effectuées avant de pouvoir utiliser l’API de Maps, y compris :

-  [Obtenir une clé API cartes](#obtain-maps-key)
-  [Installer les Kit de développement logiciel de Services Google Play](#install-gps-sdk)
-  [Installer le package Xamarin.GooglePlayServices.Maps à partir de NuGet](#install-gpsmaps-nuget)
-  [Spécifier les autorisations requises](#declare-permissions)
-  [Si vous le souhaitez, créer un émulateur avec les APIs Google](#create-emulator-with-google-api)


### <a name="a-nameobtain-maps-key-obtain-a-google-maps-api-key"></a><a name="obtain-maps-key" />Obtenir une clé d’API Google Maps

La première étape consiste à obtenir une clé de l’API Google Maps (Notez que vous ne pouvez pas réutiliser une clé API à partir de l’API de v1 Google Maps héritée). Pour plus d’informations sur la façon d’obtenir et d’utiliser la clé d’API avec Xamarin.Android, consultez [obtention A Google Maps, clé d’API](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).
 

### <a name="a-nameinstall-gps-sdk--install-the-google-play-services-sdk"></a><a name="install-gps-sdk" /> Installer les Kit de développement logiciel de Services Google Play

Google Play Services est une technologie à partir de Google qui permet aux applications Android tirer parti des diverses fonctionnalités de Google, Google + dans l’application de facturation et cartes. Ces fonctionnalités sont accessibles sur les appareils Android en tant que services d’arrière-plan, f qui sont contenus dans le [Google Play Services APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en).

Applications Android interagissent avec les Services Google Play via la bibliothèque de client Google Play Services. Cette bibliothèque contient les interfaces et classes pour les services individuels tels que des cartes. Le diagramme suivant montre la relation entre une application Android et les Services Google Play :

![Diagramme illustrant le Store Play Google la mise à jour le fichier APK Google Play Services](maps-api-images/play-services-diagram.png)

L’API de cartes Android est fournie dans le cadre des Services Google Play.
Avant une application Xamarin.Android peut utiliser l’API de cartes, le kit SDK Google Play Services doit être installé à l’aide de la [Gestionnaire Android SDK](~/android/get-started/installation/android-sdk.md). La capture d’écran suivante indique où dans le gestionnaire Android SDK vous pouvez trouver le client des services Google Play :

![Google Play Services apparaît sous fonctionnalités supplémentaires dans le Gestionnaire de kit de développement logiciel Android.](maps-api-images/image01.png)

> [!NOTE]
> Les services Google Play APK est un produit sous licence ne peut pas être présent sur tous les appareils. S’il n’est pas installé, Google Maps ne fonctionne pas sur l’appareil.

### <a name="a-nameinstall-gpsmaps-nuget--install-the-xamaringoogleplayservicesmaps-package-from-nuget"></a><a name="install-gpsmaps-nuget" /> Installer le package Xamarin.GooglePlayServices.Maps à partir de NuGet

Le [Xamarin.GooglePlayServices.Maps package](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Maps) contient les liaisons de Xamarin.Android pour l’API de cartes Google Play Services.
Pour ajouter le package de la correspondance entre les Services Google Play, cliquez sur le **références** dossier de votre projet dans l’Explorateur de solutions et cliquez sur **gérer les Packages NuGet...** :

![Élément de menu de contexte de gérer les Packages NuGet Explorateur de solutions affichant sous références](maps-api-images/image02.png)

Cette opération ouvre le **Gestionnaire de Package NuGet**. Cliquez sur **Parcourir** et entrez **Xamarin Google Play Services Maps** dans le champ de recherche. Sélectionnez **Xamarin.GooglePlayServices.Maps** et cliquez sur **installer**. (Si ce package a été installé précédemment, cliquez sur **mise à jour**.) :

[![Gestionnaire de Package NuGet avec package Xamarin.GooglePlayServices.Maps sélectionné](maps-api-images/image03-sml.png)](maps-api-images/image03.png#lightbox)

Notez que les packages de dépendance suivants sont également installés :

-   **Xamarin.GooglePlayServices.Base**
-   **Xamarin.GooglePlayServices.Basement**
-   **Xamarin.GooglePlayServices.Tasks**

### <a name="a-namedeclare-permissions--specify-the-required-permissions"></a><a name="declare-permissions" /> Spécifier les autorisations requises

Les applications doivent identifier les exigences matérielles et d’autorisation pour pouvoir utiliser l’API de cartes Google.  Certaines autorisations sont accordées automatiquement par le SDK des Services Google Play, et il n’est pas nécessaire pour un développeur pour les ajouter explicitement **AndroidManfest.XML**:

-  **Accès à l’état du réseau** &ndash; l’API de cartes doit être en mesure de vérifier s’il peut télécharger les mosaïques.

-  **Accès à Internet** &ndash; accès à Internet est nécessaire pour télécharger les mosaïques et communiquer avec les serveurs de lire de Google pour l’accès à l’API.

Les autorisations et les fonctionnalités suivantes doivent être spécifiées dans le **AndroidManifest.XML** pour l’API Android de Google Maps :

-  **OpenGL ES v2** &ndash; l’application doit déclarer l’exigence OpenGL ES v2.

-  **Clé d’API Google Maps** &ndash; clé de l’API est utilisée pour vérifier que l’application est inscrit et autorisée à utiliser les Services Google Play. Consultez [obtention d’une clé d’API Google Maps](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md) pour plus d’informations sur cette clé.
   
- **Demander l’ancien client d’Apache HTTP** &ndash; les applications qui ciblent Android 9.0 (niveau d’API 28) ou version ultérieure doit spécifier que le client HTTP Apache hérité est une bibliothèque facultative à utiliser. 

-  **Accès aux Services Web de Google-based** &ndash; l’application doit être autorisé à accéder aux services web de Google qui soutiennent les API de cartes Android.

-  **Autorisations pour les Notifications de Services Google Play** &ndash; l’application doit être autorisée à recevoir des notifications à distance à partir de Google Play Services.

-  **Accès aux fournisseurs de localisation** &ndash; il s’agit des autorisations facultatives.
   Ils permettent la `GoogleMap` classe pour afficher l’emplacement de l’appareil sur la carte.


> [!NOTE]
> Très anciennes versions du SDK Google Play nécessitaient à une application de demander la `WRITE_EXTERNAL_STORAGE` autorisation. Cette exigence n’est plus nécessaire avec les liaisons Xamarin récentes pour les Services Google Play.

L’extrait de code suivant est un exemple des paramètres qui doivent être ajoutées à **AndroidManifest.XML**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionName="4.5" package="com.xamarin.docs.android.mapsandlocationdemo2" android:versionCode="6">
    <uses-sdk android:minSdkVersion="23" android:targetSdkVersion="28" />

    <!-- Google Maps for Android v2 requires OpenGL ES v2 -->
    <uses-feature android:glEsVersion="0x00020000" android:required="true" />
    
    <!-- Necessary for apps that target Android 9.0 or higher -->
    <uses-library android:name="org.apache.http.legacy" android:required="false" />


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

En plus de la demande les autorisations **AndroidManifest.XML**, une application doit également effectuer des vérifications d’autorisation de runtime pour la `ACCESS_COARSE_LOCATION` et `ACCESS_FINE_LOCATION` autorisations. Consultez le [Xamarin.Android autorisations](~/android/app-fundamentals/permissions.md) guide pour plus d’informations sur l’exécution des vérifications d’autorisations d’exécution.


### <a name="a-namecreate-emulator-with-google-api-create-an-emulator-with-google-apis"></a><a name="create-emulator-with-google-api" />Créer un émulateur avec les API Google

Dans le cas où un appareil Android physique avec Google Play services n’est pas installé, il est possible de créer une image d’émulateur pour le développement. Pour plus d’informations, consultez le [Device Manager](~/android/get-started/installation/android-emulator/device-manager.md).


## <a name="the-googlemap-class"></a>La classe GoogleMap

Une fois que les conditions préalables sont remplies, il est temps de commencer à développer l’application et utiliser l’API Android de Maps. Le [GoogleMap](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap) classe est l’API principale une application Xamarin.Android utilisera pour afficher et interagir avec un Google Maps pour Android. Cette classe responsabilités est les suivantes :

-  Interaction avec les services Google Play pour autoriser l’application avec le service web de Google.

-  Téléchargement, la mise en cache et afficher les vignettes de carte.

-  Affichage des contrôles d’interface utilisateur tels que panoramique et zoom à l’utilisateur.

-  Marqueurs de dessin et formes géométriques sur les cartes.

Le `GoogleMap` est ajouté à une activité de deux manières :

-  **MapFragment** - le [MapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment) est un Fragment spécialisé qui agit en tant qu’hôte pour le `GoogleMap` objet. Le `MapFragment` nécessite le niveau d’API Android 12 ou une version ultérieure.
   Les versions antérieures d’Android peuvent utiliser le [SupportMapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/SupportMapFragment).  Ce guide met l’accent sur l’utilisation de la `MapFragment` classe.

-  **MapView** - le [MapView](https://developers.google.com/android/reference/com/google/android/gms/maps/MapView) est une sous-classe spécialisée de la vue, qui peut agir comme un hôte pour un `GoogleMap` objet. Les utilisateurs de cette classe doivent transférer toutes les méthodes de cycle de vie d’activité à la `MapView` classe.

Chacun de ces conteneurs expose un `Map` propriété qui retourne une instance de `GoogleMap`. Préférence doit être accordée à la [MapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment) classe, comme c’est une API plus simple qui permet de réduire le code réutilisable quantité qu’un développeur doit implémenter manuellement.

### <a name="adding-a-mapfragment-to-an-activity"></a>Ajout d’un MapFragment à une activité

La capture d’écran suivante est un exemple d’une simple `MapFragment`:

[![Capture d’écran d’un appareil qui affiche un fragment de mappage de Google](maps-api-images/image05-sml.png)](maps-api-images/image05.png#lightbox)

Similaire à d’autres classes de Fragment, il existe deux façons d’ajouter un `MapFragment` à une activité :

-   **Manière déclarative** - le `MapFragment` peuvent être ajoutés via le fichier de disposition XML pour l’activité. L’extrait de code XML suivant montre un exemple montrant comment utiliser le `fragment` élément :

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <fragment xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/map"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              class="com.google.android.gms.maps.MapFragment" />
    ```

-   **Par programmation** - le `MapFragment` peuvent être instanciés par programmation à l’aide de la [ `MapFragment.NewInstance` ](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment.html#newInstance()) (méthode), puis ajouté à une activité. Cet extrait de code montre la façon la plus simple pour instancier un `MapFragment` de l’objet et l’ajouter à une activité :
    
    ```csharp
        var mapFrag = MapFragment.NewInstance();
        activity.FragmentManager.BeginTransaction()
                                .Add(Resource.Id.map_container, mapFrag, "map_fragment")
                                .Commit();

    ```

    Il est possible de configurer le `MapFragment` en passant un [ `GoogleMapOptions` ](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions) objet `NewInstance`. Ce sujet est abordé dans la section [GoogleMap propriétés](#googlemap_object) qui s’affiche par la suite dans ce guide.

Le `MapFragment.GetMapAsync` méthode est utilisée pour initialiser le [ `GoogleMap` ](#googlemap_object) qui est hébergé par le fragment et obtenir une référence à l’objet de mappage qui est hébergé par le `MapFragment`. Cette méthode prend un objet qui implémente le `IOnMapReadyCallback` interface. 

Cette interface a une méthode unique, `IMapReadyCallback.OnMapReady(MapFragment map)` qui seront invoqués lorsqu’il est possible que l’application d’interagir avec le `GoogleMap` objet. L’extrait de code suivant montre comment une activité Android peut initialiser un `MapFragment` et implémenter la `IOnMapReadyCallback` interface :
```csharp
public class MapWithMarkersActivity : AppCompatActivity, IOnMapReadyCallback
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.MapLayout);
    
        var mapFragment = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.map);
        mapFragment.GetMapAsync(this);
    
        // remainder of code omitted
    }
    
    public void OnMapReady(GoogleMap map)
    {
        // Do something with the map, i.e. add markers, move to a specific location, etc.
    }
}
```

### <a name="map-types"></a>Mapper des types

Il existe cinq types de cartes disponibles à partir de l’API mappe de Google :

-  **Normal** -il s’agit du type de mappage par défaut. Il montre les routes et importantes fonctionnalités naturelles, ainsi que certains points artificielle d’intérêt (par exemple, des bâtiments et des ponts).

-  **Satellite** -cette carte affiche des photographies satellite.

-  **Hybride** : cette carte affiche des photographies satellite et road mappe.

-  **Terrain** -Voici principalement topographiques fonctionnalités avec certaines routes.

-  **Aucun** -ce mappage ne charge pas les vignettes, il est restitué sous la forme d’une grille vide.


L’image ci-dessous illustre trois des différents types de cartes, de gauche à droite (normal, hybride, un terrain) :

[![Trois mappent des captures d’écran exemple : normale, hybrides et le Terrain](maps-api-images/map-types-sml.png)](maps-api-images/map-types.png#lightbox)

Le `GoogleMap.MapType` propriété est utilisée pour définir ou modifier le type de carte s’affiche. L’extrait de code suivant montre comment afficher une carte satellite.

```csharp
public void OnMapReady(GoogleMap map)
{
    map.MapType = GoogleMap.MapTypeHybrid;
}
```


### <a name="a-namegooglemapobject-googlemap-properties"></a><a name="googlemap_object" />Propriétés de GoogleMap

`GoogleMap` définit plusieurs propriétés qui peuvent contrôler les fonctionnalités et l’apparence de la carte. Une façon de configurer l’état initial d’un `GoogleMap` consiste à passer un [GoogleMapOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions) lors de la création de l’objet une `MapFragment`. L’extrait de code suivant est un exemple d’utilisation une `GoogleMapOptions` lors de la création de l’objet une `MapFragment`:

```csharp
GoogleMapOptions mapOptions = new GoogleMapOptions()
    .InvokeMapType(GoogleMap.MapTypeSatellite)
    .InvokeZoomControlsEnabled(false)
    .InvokeCompassEnabled(true);

FragmentTransaction fragTx = FragmentManager.BeginTransaction();
mapFragment = MapFragment.NewInstance(mapOptions);
fragTx.Add(Resource.Id.map, mapFragment, "map");
fragTx.Commit();
```

L’autre méthode permettant de configurer un `GoogleMap` est en manipulant des propriétés sur le [UiSettings](https://developers.google.com/android/reference/com/google/android/gms/maps/UiSettings) de l’objet map. L’exemple de code suivant montre comment configurer un `GoogleMap` pour afficher les contrôles de zoom et une boussole :

```csharp
public void OnMapReady(GoogleMap map)
{
    map.UiSettings.ZoomControlsEnabled = true;
    map.UiSettings.CompassEnabled = true;
}
```

## <a name="interacting-with-the-googlemap"></a>Interaction avec le GoogleMap

L’API de cartes Android fournit des API qui autorisent une activité à modifier le point de vue, ajouter des marqueurs, placer des superpositions personnalisées ou dessiner des formes géométriques. Cette section explique comment accomplir certaines de ces tâches dans Xamarin.Android.

### <a name="changing-the-viewpoint"></a>Modifier le point de vue

Mappages sont modélisées en tant qu’un plan plat sur l’écran, en fonction de la projection Mercator. La vue cartographique est celle d’un *caméra* qualité directement vers le bas sur ce plan. La position de l’appareil photo peut être contrôlée en modifiant l’emplacement, le zoom, l’inclinaison, incidence. Le [CameraUpdate](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdate) classe est utilisée pour déplacer l’emplacement de l’appareil photo. `CameraUpdate` les objets ne sont pas instanciés directement, à la place l’API Maps fournit le [CameraUpdateFactory](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdateFactory) classe.

Une fois un `CameraUpdate` objet a été créé, il est passé en tant que paramètre à un le [GoogleMap.MoveCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#moveCamera(com.google.android.gms.maps.CameraUpdate)) ou [GoogleMap.AnimateCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#animateCamera(com.google.android.gms.maps.CameraUpdate)) méthodes. Le `MoveCamera` méthode met à jour la carte instantanément lors de la `AnimateCamera` méthode fournit une transition sans heurts, des animation.

Cet extrait de code est un exemple simple d’utilisation le `CameraUpdateFactory` pour créer un `CameraUpdate` qui incrémente le niveau de zoom de la carte par niveau de zoom d’un :

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...

public void OnMapReady(GoogleMap map)
{   
    map.MoveCamera(CameraUpdateFactory.ZoomIn());
}
```

L’API Maps fournit un [CameraPosition](http://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.html) qui regroupera toutes les valeurs possibles pour la position de la caméra. Une instance de cette classe peut être fournie à la [CameraUpdateFactory.NewCameraPosition](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdateFactory#newCameraPosition%28com.google.android.gms.maps.model.CameraPosition%29) méthode qui retournera un `CameraUpdate` objet. L’API de cartes inclut également le [CameraPosition.Builder](http://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.Builder.html) classe qui fournit une API fluent création `CameraPosition` objets.
L’extrait de code suivant montre un exemple de création un `CameraUpdate` à partir d’un `CameraPosition` et son utilisation pour modifier la position de la caméra sur un `GoogleMap`:

```csharp
public void OnMapReady(GoogleMap map)
{
    LatLng location = new LatLng(50.897778, 3.013333);
    
    CameraPosition.Builder builder = CameraPosition.InvokeBuilder();
    builder.Target(location);
    builder.Zoom(18);
    builder.Bearing(155);
    builder.Tilt(65);
    
    CameraPosition cameraPosition = builder.Build();
    
    CameraUpdate cameraUpdate = CameraUpdateFactory.NewCameraPosition(cameraPosition);

    map.MoveCamera(cameraUpdate);
}
```

Dans l’extrait de code précédent, un emplacement spécifique sur la carte est représenté par le [LatLng](https://developers.google.com/android/reference/com/google/android/gms/maps/model/LatLng) classe. Le niveau de zoom est défini à 18, qui est une mesure arbitraire de zoom utilisé par Google Maps. L’incidence est la boussole mesure dans le sens horaire depuis le nord. La propriété inclinaison contrôle l’angle de vue et spécifie un angle de 25 degrés à la verticale. La capture d’écran suivante montre le `GoogleMap` après avoir exécuté le code précédent :

[![Exemple de mappage de Google montrant un emplacement spécifié avec un texte incliné angle d’affichage](maps-api-images/image06-sml.png)](maps-api-images/image06.png#lightbox)


### <a name="drawing-on-the-map"></a>Dessin sur la carte

L’API de cartes Android fournit de l’API de dessin des éléments suivants sur une carte :

-  **Marqueurs** -il s’agit des icônes spéciales qui sont utilisés pour identifier un emplacement unique sur une carte.

-  **Superpositions** -il s’agit d’une image qui peut être utilisée pour identifier une collection d’emplacements ou de la zone sur la carte.

-  **Lignes, des polygones et des cercles** -il s’agit des API qui permettent des activités à ajouter des formes à un mappage.


#### <a name="markers"></a>Markers

L’API Maps fournit un [marqueur](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker) classe qui encapsule toutes les données relatives à un emplacement unique sur une carte. Par défaut, le marqueur de la classe utilise une icône standard fournie par Google Maps. Il est possible de personnaliser l’apparence d’un marqueur et pour répondre aux clics de l’utilisateur.


##### <a name="adding-a-marker"></a>Ajout d’un marqueur

Pour ajouter un marqueur à un mappage, il convient de créer un nouveau [MarkerOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions) de l’objet, puis appelez le [AddMarker](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addMarker%28com.google.android.gms.maps.model.MarkerOptions%29) méthode sur un `GoogleMap` instance. Cette méthode retournera un [marqueur](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker) objet.

```csharp
public void OnMapReady(GoogleMap map)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");
    
    map.AddMarker(markerOpt1);
}
```

Le titre du marqueur s’affichera dans un *fenêtre info* lorsque l’utilisateur appuie sur le marqueur. La capture d’écran suivante montre à quoi ressemble ce marqueur :

[![Exemple de mappage de Google avec un marqueur et une fenêtre d’information pour Vimy Ridge](maps-api-images/image07-sml.png)](maps-api-images/image07.png#lightbox)


##### <a name="customizing-a-marker"></a>Personnalisation d’un marqueur

Il est possible de personnaliser l’icône utilisée par le marqueur en appelant le `MarkerOptions.InvokeIcon` méthode lorsque vous ajoutez le marqueur à la carte.
Cette méthode prend un [BitmapDescriptor](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptor) objet contenant les données nécessaires à l’affichage de l’icône. Le [BitmapDescriptorFactory](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptorFactory) classe fournit des méthodes d’assistance pour simplifier la création d’un `BitmapDescriptor`. La liste suivante présente certaines de ces méthodes :

-   `DefaultMarker(float colour)` &ndash; Utilisez le marqueur de Google Maps par défaut, mais changer la couleur.

-   `FromAsset(string assetName)` &ndash; Utiliser une icône personnalisée à partir du fichier spécifié dans le dossier de ressources.

-   `FromBitmap(Bitmap image)` &ndash; Utilisez la bitmap spécifiée en tant que l’icône.

-   `FromFile(string fileName)` &ndash; Créer l’icône personnalisée à partir du fichier dans le chemin spécifié.

-   `FromResource(int resourceId)` &ndash; Créer une icône personnalisée à partir de la ressource spécifiée.

L’extrait de code suivant montre un exemple de création d’un marqueur de valeur par défaut de couleur cyan :

```csharp
public void OnMapReady(GoogleMap map)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");
    
    var bmDescriptor = BitmapDescriptorFactory.DefaultMarker (BitmapDescriptorFactory.HueCyan);
    markerOpt1.InvokeIcon(bmDescriptor);
    
    map.AddMarker(markerOpt1);
}
```

#### <a name="info-windows"></a>Fenêtres d’informations

*Fenêtres d’informations* sont spéciales windows ce menu contextuel pour afficher des informations à l’utilisateur quand ils appuient sur un marqueur spécifique. Par défaut, la fenêtre d’info affichera le contenu du titre du marqueur. Si le titre n’a pas été attribué, aucun fenêtre info ne s’affiche. Qu’une seule fenêtre informations peut-être s’afficher à la fois.

Il est possible de personnaliser la fenêtre d’information en implémentant la [GoogleMap.IInfoWindowAdapter](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.InfoWindowAdapter) interface. Il existe deux méthodes importantes sur cette interface :

-  `public View GetInfoWindow(Marker marker)` &ndash; Cette méthode est appelée pour obtenir une fenêtre d’informations personnalisé pour un marqueur. Si elle retourne `null` , le rendu de fenêtre par défaut sera utilisée. Si cette méthode retourne une vue, cette vue sera placée dans le frame de fenêtre d’informations.

-  `public View GetInfoContents(Marker marker)` &ndash; Cette méthode est uniquement être appelée si GetInfoWindow retourne `null` . Cette méthode peut retourner un `null` valeur si le rendu par défaut, le contenu de la fenêtre informations doit être utilisé. Sinon, cette méthode doit retourner une vue avec le contenu de la fenêtre d’informations.

Une fenêtre d’info n’est pas un affichage en temps réel, à la place Android sera convertir la vue d’une image bitmap statique et de les afficher sur l’image. Cela signifie qu’une fenêtre d’info ne peut pas répondre aux événements tactiles ni de mouvements, ni il actualise automatiquement lui-même. Pour mettre à jour d’une fenêtre d’informations, il est nécessaire d’appeler le [GoogleMap.ShowInfoWindow](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker.html#showInfoWindow()) (méthode).

L’illustration suivante montre quelques exemples de certaines fenêtres d’informations personnalisé. L’image sur la gauche a son contenu personnalisé, alors que l’image de droite a sa fenêtre et son contenu personnalisé avec des angles arrondis est utilisée :

![Exemple les fenêtres de marqueur pour Melbourne, y compris l’icône et la population. La fenêtre de droite a des angles arrondis.](maps-api-images/marker-infowindows.png)

#### <a name="groundoverlays"></a>GroundOverlays

Contrairement aux marqueurs, ce qui permet d’identifier un emplacement spécifique sur une carte, une [GroundOverlay](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlay) est une image qui est utilisée pour identifier une collection d’emplacements ou d’une zone sur la carte.

##### <a name="adding-a-groundoverlay"></a>Ajout d’un GroundOverlay

Ajout d’un segment de recouvrement de bout en bout à un mappage est similaire à l’ajout d’un marqueur à un mappage. Tout d’abord, un [GroundOverlayOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlayOptions) objet est créé. Cet objet est ensuite transmis en tant que paramètre à la [ `GoogleMap.AddGroundOverlay` ](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addGroundOverlay(com.google.android.gms.maps.model.GroundOverlayOptions)) (méthode), qui retournera un `GroundOverlay` objet. Cet extrait de code est un exemple d’ajout d’un segment de recouvrement de bout en bout à une carte :

```csharp
BitmapDescriptor image = BitmapDescriptorFactory.FromResource(Resource.Drawable.polarbear);
GroundOverlayOptions groundOverlayOptions = new GroundOverlayOptions()
    .Position(position, 150, 200)
    .InvokeImage(image);
GroundOverlay myOverlay = googleMap.AddGroundOverlay(groundOverlayOptions);
```

La capture d’écran suivante montre cette superposition sur une carte :

[![Exemple de mappage avec une image superposée d’un ours polaires](maps-api-images/image09-sml.png)](maps-api-images/image09.png#lightbox)


#### <a name="lines-circles-and-polygons"></a>Lignes, des cercles et des polygones

Il existe trois types simples de figures géométriques qui peuvent être ajoutées à une carte :

-  **Polyline** -il s’agit d’une série de segments de ligne connectés. Il peut marquer un chemin d’accès sur une carte ou créer une forme géométrique.

-  **Cercle** -il dessine un cercle sur la carte.

-  **Polygone** -il s’agit d’une forme fermée pour le marquage des zones sur une carte.


##### <a name="polylines"></a>Polylignes

Un [polyligne](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Polyline) est une liste de consécutifs `LatLng` objets qui spécifient les sommets de chaque segment de ligne. Une polyligne est créée en créant d’abord un `PolylineOptions` objet et ajouter les points à celle-ci. Le `PolylineOption` objet est ensuite passé à un `GoogleMap` objet en appelant le `AddPolyline` (méthode).

```csharp
PolylineOption rectOptions = new PolylineOption();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
rectOptions.Add(new LatLng(37.35, -122.0)); // close the polyline - this makes a rectangle.

googleMap.AddPolyline(rectOptions);
```

##### <a name="circles"></a>Cercles

Cercles sont créés par instanciation premier un [CircleOption](https://developers.google.com/android/reference/com/google/android/gms/maps/model/CircleOptions) objet que vous spécifierez le centre et le rayon du cercle en mètres. Le cercle est dessiné sur la carte en appelant [GoogleMap.AddCircle](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addCircle(com.google.android.gms.maps.model.CircleOptions)).
L’extrait de code suivant montre comment dessiner un cercle :

```csharp
CircleOptions circleOptions = new CircleOptions ();
circleOptions.InvokeCenter (new LatLng(37.4, -122.1));
circleOptions.InvokeRadius (1000);

googleMap.AddCircle (circleOptions);
```


##### <a name="polygons"></a>Polygones

`Polygon`Control sont similaires aux `Polyline`s, mais ils ne sont pas ouverts terminée. `Polygon`sont une boucle fermée, ont leur intérieur renseigné.
`Polygon`s sont créés dans la même manière en tant qu’un `Polyline`, sauf le [GoogleMap.AddPolygon](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addPolygon(com.google.android.gms.maps.model.PolygonOptions)) méthode appelée.

Contrairement à un `Polyline`, un `Polygon` est à fermeture automatique. Le polygone est fermé par le `AddPolygon` méthode en dessinant une ligne qui connecte le premier et dernier point. L’extrait de code suivant crée un rectangle plein sur la même zone que l’extrait de code précédent dans le `Polyline` exemple.

```csharp
PolygonOptions rectOptions = new PolygonOptions();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
// notice we don't need to close off the polygon

googleMap.AddPolygon(rectOptions);
```


## <a name="responding-to-user-events"></a>Répondre aux événements utilisateur

Il existe trois types d’interactions de qu'un utilisateur peut avoir avec une carte :

-  **Cliquez sur le marqueur** -l’utilisateur clique sur un marqueur.

-  **Faites glisser le marqueur** -l’utilisateur a long-cliqué sur un mparger

-  **Cliquez sur la fenêtre Info** -l’utilisateur a cliqué sur une fenêtre d’informations.

Chacun de ces événements sera abordé plus en détail ci-dessous.


### <a name="marker-click-events"></a>Événements de clic de marqueur

Le `MarkerClicked` événement est déclenché lorsque l’utilisateur appuie sur un marqueur. Cet événement accepte un `GoogleMap.MarkerClickEventArgs` objet en tant que paramètre. Cette classe contient deux propriétés :

-  `GoogleMap.MarkerClickEventArgs.Handled` &ndash; Cette propriété doit être définie sur `true` pour indiquer que le Gestionnaire d’événements a utilisé l’événement. Si cela est défini sur `false` le comportement par défaut se produit en plus du comportement personnalisé du Gestionnaire d’événements.

-  `Marker` &ndash; Cette propriété est une référence à la marque qui a déclenché le `MarkerClick` événement.


Cet extrait de code montre un exemple d’un `MarkerClick` qui modifie la position de la caméra vers un nouvel emplacement sur la carte :

```csharp
void MapOnMarkerClick(object sender, GoogleMap.MarkerClickEventArgs markerClickEventArgs)
{
    markerClickEventArgs.Handled = true;

    var marker = markerClickEventArgs.Marker;
    if (marker.Id.Equals(gotMauiMarkerId))
    {
        LatLng InMaui = new LatLng(20.72110, -156.44776);
    
        // Move the camera to look at Maui.
        PositionPolarBearGroundOverlay(InMaui);
        googleMap.AnimateCamera(CameraUpdateFactory.NewLatLngZoom(InMaui, 13));
        gotMauiMarkerId = null;
        polarBearMarker.Remove();
        polarBearMarker = null;
    }
    else
    {
        Toast.MakeText(this, $"You clicked on Marker ID {marker.Id}", ToastLength.Short).Show();
    }
}
```


### <a name="marker-drag-events"></a>Événements de glisser de marqueur

Cet événement est déclenché lorsque l’utilisateur souhaite faire glisser le marqueur. Par défaut, les marqueurs ne sont pas déplaçables. Un marqueur peut être défini comme déplaçable en définissant le `Marker.Draggable` propriété `true` ou en appelant le `MarkerOptions.Draggable` méthode avec `true` en tant que paramètre.

Pour faire glisser le marqueur, l’utilisateur doit tout d’abord long, cliquez sur le marqueur et ensuite leurs doigts doit rester sur la carte. Lorsque le doigt de l’utilisateur est déplacé autour de l’écran, le marqueur déplace. Lorsque les doigts de l’utilisateur soulève le visage, le marqueur reste en place.

La liste suivante décrit les différents événements qui seront déclenchés pour un marqueur peut être déplacé :

-   `GoogleMap.MarkerDragStart(object sender, GoogleMap.MarkerDragStartEventArgs e)` &ndash; Cet événement est déclenché lorsque l’utilisateur fait tout d’abord glisser le marqueur.

-   `GoogleMap.MarkerDrag(object sender, GoogleMap.MarkerDragEventArgs e)` &ndash; Cet événement est déclenché comme le marqueur est glissé.

-   `GoogleMap.MarkerDragEnd(object sender, GoogleMap.MarkerDragEndEventArgs e)` &ndash; Cet événement est déclenché lorsque l’utilisateur est terminé en faisant glisser le marqueur.

Chacun de la `EventArgs` contient une propriété unique appelée `P0` qui est une référence à la `Marker` de l’objet glissé.


### <a name="info-window-click-events"></a>Événements de fenêtre, cliquez sur informations

Qu’une seule fenêtre d’info peut être affichée à la fois. Lorsque l’utilisateur clique sur une fenêtre d’informations dans un mappage, l’objet map déclenchera une `InfoWindowClick` événement. L’extrait de code suivant montre comment associer un gestionnaire pour l’événement :

```csharp
public void OnMapReady(GoogleMap map)
{
    map.InfoWindowClick += MapOnInfoWindowClick;
}

private void MapOnInfoWindowClick (object sender, GoogleMap.InfoWindowClickEventArgs e)
{
    Marker myMarker = e.Marker;
    // Do something with marker.
}
```

Rappelez-vous qu’une fenêtre d’info est statique `View` est restituée en tant qu’image sur la carte. Les widgets tels que des boutons, des cases à cocher ou des vues de texte qui sont placés à l’intérieur de la fenêtre d’info seront inertes et ne peut pas répondre à certaines de leurs événements utilisateur intégraux.


## <a name="related-links"></a>Liens associés

- [SimpleMapDemo](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3/SimpleMapDemo)
- [Services Google Play](https://developers.google.com/android/guides/overview)
- [Mappe les Google Android API v2](https://developers.google.com/maps/documentation/android-sdk/intro)
- [Services de Google Play APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)
- [Obtention d’une clé de l’API Google Maps](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [utilise-library](https://developer.android.com/guide/topics/manifest/uses-library-element)
- [utilise la fonctionnalité](https://developer.android.com/guide/topics/manifest/uses-feature-element)
