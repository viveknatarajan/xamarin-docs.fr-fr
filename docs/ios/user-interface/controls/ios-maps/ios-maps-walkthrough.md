---
title: Les annotations et les superpositions
description: Cet article présente une procédure pas à pas montrant comment tirer parti des fonctionnalités d’annotation et superposition de Kit de la carte. Il montre comment ajouter un mappage à une application qui affiche un segment de recouvrement et d’annotation à l’emplacement de la conférence 2013 évoluer de Xamarin.
ms.prod: xamarin
ms.assetid: 1BC4F7FC-AE3C-46D7-A4D3-18E142F55B8E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 9defbade6fafefb26d87e88665c491b3a559c1ae
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="annotations-and-overlays--walkthrough"></a>Les annotations et les superpositions : procédure pas à pas

L’application que nous allons créer dans cette procédure pas à pas est indiquée ci-dessous :

 [![](ios-maps-walkthrough-images/00-map-overlay.png "Un exemple d’application MapKit")](ios-maps-walkthrough-images/00-map-overlay.png#lightbox)
 
Vous pouvez trouver le code terminé dans le **MapsWalkthroughComplete** dossier au sein de la [exemple de démonstration de la carte](https://developer.xamarin.com/samples/monotouch/MapDemo/).

Commençons par créer un nouveau **iOS projet vide**et en lui attribuant un nom approprié. Nous allons commencer en ajoutant du code à notre contrôleur d’affichage pour afficher le programme et crée ensuite les nouvelles classes pour notre MapDelegate et les annotations personnalisées. Suivez les étapes ci-dessous pour y parvenir :

## <a name="viewcontroller"></a>ViewController


1. Ajouter des espaces de noms suivants à la `ViewController`:

    ```csharp
    using MapKit;
    using CoreLocation;
    using UIKit
    using CoreGraphics
    ```

1. Ajouter un `MKMapView` variable à la classe d’instance avec un `MapDelegate` instance. Nous allons créer le `MapDelegate` peu de temps :

    ```csharp
    public partial class ViewController : UIViewController
    {
        MKMapView map;
        MapDelegate mapDelegate;
        ...
    ```

1. Dans le contrôleur de `LoadView` (méthode), ajouter un `MKMapView` et affectez-lui le `View` propriété du contrôleur :

    ```csharp
    public override void LoadView ()
    {
        map = new MKMapView (UIScreen.MainScreen.Bounds);
        View = map;
    }
    ```

    Ensuite, nous allons ajouter le code pour initialiser le mappage dans le ' ViewDidLoad'' (méthode).

1. Dans `ViewDidLoad` ajouter du code pour définir le type de carte indiquent l’emplacement de l’utilisateur et de zoom et défilement à l’aide :

    ```csharp
    // change map type, show user location and allow zooming and panning
    map.MapType = MKMapType.Standard;
    map.ShowsUserLocation = true;
    map.ZoomEnabled = true;
    map.ScrollEnabled = true;
    
    ```

1. Ensuite, ajoutez le code pour centrer la carte et de définir sa région :

    ```csharp
    double lat = 30.2652233534254;
    double lon = -97.73815460962083;
    CLLocationCoordinate2D mapCenter = new CLLocationCoordinate2D (lat, lon);
    MKCoordinateRegion mapRegion = MKCoordinateRegion.FromDistance (mapCenter, 100, 100);
    map.CenterCoordinate = mapCenter;
    map.Region = mapRegion;
    
    ```

1. Créer une nouvelle instance de `MapDelegate` et assignez-la à la `Delegate` de la `MKMapView`. Là encore, nous allons implcodeent le `MapDelegate` peu de temps :

    ```csharp
    mapDelegate = new MapDelegate ();
    map.Delegate = mapDelegate;     
    ```

1. À partir d’iOS 8, vous devez demande d’autorisation à partir de votre utilisateur d’utiliser leur emplacement, nous allons donc l’ajouter cela à notre exemple. Tout d’abord définir un `CLLocationManager` variable au niveau de la classe :

    ```csharp
    CLLocationManager locationManager = new CLLocationManager();
    ```

1. Dans le `ViewDidLoad` (méthode), nous souhaitons vérifier si l’appareil qui exécute l’application à l’aide d’iOS 8 et s’il s’agit nous demande d’autorisation lors de l’application est en cours d’utilisation :

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion(8,0)){
                    locationManager.RequestWhenInUseAuthorization ();
                }
    ```

1. Enfin, nous devons modifier le **Info.plist** fichier pour avertir les utilisateurs de la raison de la demande de leur emplacement. Dans le **Source** menu de la **Info.plist**, ajoutez la clé suivante :
    
    `NSLocationWhenInUseUsageDescription` 
    
    et la chaîne : 

    `Maps Demo`.


## <a name="conferenceannotationcs--a-class-for-custom-annotations"></a>ConferenceAnnotation.cs : une classe pour les Annotations personnalisées


1. Nous allons utiliser une classe personnalisée pour l’annotation appelée `ConferenceAnnotation`. Ajoutez la classe suivante au projet :

    ```csharp
    using System;
    using CoreLocation;
    using MapKit;
    
    namespace MapDemo
    {
        public class ConferenceAnnotation : MKAnnotation
        {
            string title;
            CLLocationCoordinate2D coord;
    
            public ConferenceAnnotation (string title,
            CLLocationCoordinate2D coord)
            {
                this.title = title;
                this.coord = coord;
            }
    
            public override string Title {
                get {
                    return title;
                }
            }
    
            public override CLLocationCoordinate2D Coordinate {
                get {
                    return coord;
                }
            }
        }
    }   
    ```

## <a name="viewcontroller---adding-the-annotation-and-overlay"></a>ViewController - Ajout de l’annotation et un segment de recouvrement

1. Avec la `ConferenceAnnotation` en place nous pouvons l’ajouter à la carte. Dans le `ViewDidLoad` méthode de la `ViewController`, ajouter une annotation à la coordonnée de centre de la carte :

    ```csharp
    map.AddAnnotations (new ConferenceAnnotation ("Evolve Conference", mapCenter)); 
    ```

1. Nous souhaitons également avoir un segment de recouvrement de l’hôtel. Ajoutez le code suivant pour créer le `MKPolygon` à l’aide de coordonnées pour l’hôtel fourni, puis l’ajouter à la carte par appel `AddOverlay`:

    ```csharp
    // add an overlay of the hotel
    MKPolygon hotelOverlay = MKPolygon.FromCoordinates(
        new CLLocationCoordinate2D[]{
        new CLLocationCoordinate2D(30.2649977168594, -97.73863627705),
        new CLLocationCoordinate2D(30.2648461170005, -97.7381627734755),
        new CLLocationCoordinate2D(30.2648355402574, -97.7381750192576),
        new CLLocationCoordinate2D(30.2647791309417, -97.7379872505988),
        new CLLocationCoordinate2D(30.2654525150319, -97.7377341711021),
        new CLLocationCoordinate2D(30.2654807195004, -97.7377994819399),
        new CLLocationCoordinate2D(30.2655089239607, -97.7377994819399),
        new CLLocationCoordinate2D(30.2656428950368, -97.738346460207),
        new CLLocationCoordinate2D(30.2650364981811, -97.7385709662122),
        new CLLocationCoordinate2D(30.2650470749025, -97.7386199493406)
    });
    
    map.AddOverlay (hotelOverlay);  
    ```
Cette étape termine le code dans `ViewDidLoad`. Maintenant nous devons implémenter notre `MapDelegate` classe de superposition respectivement les vues et de gérer la création de l’annotation.


## <a name="mapdelegate"></a>MapDelegate

1. Créez une classe appelée `MapDelegate` qui hérite de `MKMapViewDelegate` et inclure une `annotationId` variable à utiliser comme un identificateur de la réutilisation de l’annotation :

    ```csharp
    class MapDelegate : MKMapViewDelegate
    {
        static string annotationId = "ConferenceAnnotation";
        ...
    }
    ```
    Nous avons une annotation ici afin du réutiliser du code n’est pas strictement nécessaire, mais il est conseillé de l’inclure.

1. Implémentez la `GetViewForAnnotation` méthode pour retourner une vue pour la `ConferenceAnnotation` à l’aide de la **conference.png** image inclus dans cette procédure pas à pas :

    ```csharp
    public override MKAnnotationView GetViewForAnnotation (MKMapView mapView, NSObject annotation)
    {
        MKAnnotationView annotationView = null;
    
        if (annotation is MKUserLocation)
            return null; 
    
        if (annotation is ConferenceAnnotation) {
    
            // show conference annotation
            annotationView = mapView.DequeueReusableAnnotation (annotationId);
    
            if (annotationView == null)
                annotationView = new MKAnnotationView (annotation, annotationId);
        
            annotationView.Image = UIImage.FromFile ("images/conference.png");
            annotationView.CanShowCallout = true;
        } 
    
        return annotationView;
    }
    ```

1. Lorsque l’utilisateur appuie sur l’annotation, que vous souhaitez afficher une image de la ville de Austin. Ajoutez les variables suivantes à la `MapDelegate` pour l’image et la vue pour l’afficher :

    ```csharp
    UIImageView venueView;
    UIImage venueImage;
    ```

1. Ensuite, pour afficher l’image lors de l’annotation est tapée, implémenter la `DidSelectAnnotation` méthode comme suit :

    ```csharp
    public override void DidSelectAnnotationView (MKMapView mapView, MKAnnotationView view)
    {
        // show an image view when the conference annotation view is selected
        if (view.Annotation is ConferenceAnnotation) {
    
            venueView = new UIImageView ();
            venueView.ContentMode = UIViewContentMode.ScaleAspectFit;
            venueImage = UIImage.FromFile ("image/venue.png");
            venueView.Image = venueImage;
            view.AddSubview (venueView);
    
            UIView.Animate (0.4, () => {
            venueView.Frame = new CGRect (-75, -75, 200, 200); });
        }
    }
    ```

1. Pour masquer l’image lorsque l’utilisateur le désélectionne l’annotation en appuyant sur n’importe où sur la carte, vous devez implémenter la `DidSelectAnnotationView` méthode comme suit :

    ```csharp
    public override void DidDeselectAnnotationView (MKMapView mapView, MKAnnotationView view)
    {
        // remove the image view when the conference annotation is deselected
        if (view.Annotation is ConferenceAnnotation) {
    
            venueView.RemoveFromSuperview ();
            venueView.Dispose ();
            venueView = null;
        }
    }
    ```
    Nous avons maintenant le code pour l’annotation en place. Tout ce qui reste consiste à ajouter du code pour le `MapDelegate` pour créer la vue pour la superposition d’hôtel.

1. Ajoutez l’implémentation suivante de `GetViewForOverlay` à la `MapDelegate`:

    ```csharp
    public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
    {
        // return a view for the polygon
        MKPolygon polygon = overlay as MKPolygon;
        MKPolygonView polygonView = new MKPolygonView (polygon);
        polygonView.FillColor = UIColor.Blue;
        polygonView.StrokeColor = UIColor.Red;
        return polygonView;
    }
    ```

Exécutez l'application. Nous disposons désormais d’une carte interactive avec une annotation personnalisée et un segment de recouvrement ! Cliquez sur l’annotation et l’image de Austin s’affiche, comme indiqué ci-dessous :

 [![](ios-maps-walkthrough-images/01-map-image.png "Cliquez sur l’annotation et l’image de Austin est affichée.")](ios-maps-walkthrough-images/01-map-image.png#lightbox)

## <a name="summary"></a>Récapitulatif

Dans cet article, nous avons étudié comment ajouter une annotation à une carte, ainsi que l’ajout d’un segment de recouvrement pour un polygone spécifié. Il décrit également comment ajouter la prise en charge des fonctions tactiles à l’annotation pour animer une image sur une carte.


## <a name="related-links"></a>Liens associés

- [Démonstration de mappage (exemple)](https://developer.xamarin.com/samples/monotouch/MapDemo/)
- [Cartes d’e/s](~/ios/user-interface/controls/ios-maps/index.md)
