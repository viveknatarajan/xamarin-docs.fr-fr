---
title: Nouvelles fonctionnalités de MapKit sur iOS 11
description: 'Ce document décrit les nouvelles fonctionnalités de MapKit dans iOS 11 : regroupement de marqueurs, le bouton boussole, l’affichage de l’échelle et le bouton de suivi utilisateur.'
ms.prod: xamarin
ms.assetid: 304AE5A3-518F-422F-BE24-92D62CE30F34
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: f73078a2dcbaeefeb5608ce7ec1e2c12b261acad
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787404"
---
# <a name="new-features-in-mapkit-on-ios-11"></a>Nouvelles fonctionnalités de MapKit sur iOS 11

iOS 11 ajoute les nouvelles fonctionnalités suivantes à MapKit :

- [Annotation de Clustering](#clustering)
- [Bouton boussole](#compass)
- [Affichage de l’échelle](#scale)
- [Bouton de suivi utilisateur](#user-tracking)

![Carte montrant des marqueurs de cluster et compas bouton](mapkit-images/cyclemap-heading.png)

<a name="clustering" />

## <a name="automatically-grouping-markers-while-zooming"></a>Marqueurs de regroupement automatiquement lors du zoom

L’exemple [MapKit exemple « Tandm »](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) montre comment implémenter l’annotation iOS 11 nouvelles fonctionnalités de clustering.

### <a name="1-create-an-mkpointannotation-subclass"></a>1. Créer un `MKPointAnnotation` sous-classe

La classe d’annotation point représente chaque marqueur sur la carte. Ils peuvent être ajoutés à l’aide de `MapView.AddAnnotation()` ou à partir d’un tableau à l’aide de `MapView.AddAnnotations()`.

Classes d’annotations du point n’ont pas de représentation visuelle, ils ne sont obligatoires pour représenter les données associées à la marque (plus important encore, le `Coordinate` propriété qui est sa latitude et longitude sur la carte) et les propriétés personnalisées :

```csharp
public class Bike : MKPointAnnotation
{
  public BikeType Type { get; set; } = BikeType.Tricycle;
  public Bike(){}
  public Bike(NSNumber lat, NSNumber lgn, NSNumber type)
  {
    Coordinate = new CLLocationCoordinate2D(lat.NFloatValue, lgn.NFloatValue);
    switch(type.NUIntValue) {
      case 0:
        Type = BikeType.Unicycle;
        break;
      case 1:
        Type = BikeType.Tricycle;
        break;
    }
  }
}
```

### <a name="2-create-an-mkmarkerannotationview-subclass-for-single-markers"></a>2. Créer un `MKMarkerAnnotationView` sous-classe pour les marqueurs uniques

La vue d’annotation de marqueur est la représentation visuelle de chaque annotation et le style à l’aide des propriétés telles que :

- **MarkerTintColor** – la couleur du marqueur.
- **GlyphText** – texte affiché dans le marqueur.
- **GlyphImage** – définit l’image qui est affichée dans le marqueur.
- **DisplayPriority** – détermine l’ordre de plan (comportement empilement) lorsque le mappage est chargé de marqueurs. Utilisez une des `Required`, `DefaultHigh`, ou `DefaultLow`.

Pour prendre en charge le clustering automatique, vous devez également définir :

- **ClusteringIdentifier** – ce paramètre contrôle les marqueurs obtient organisés en clusters. Vous pouvez utiliser le même identificateur de toutes les marques de votre, ou utilisent des identificateurs différents pour contrôler la façon dont ils sont regroupés ensemble.

```csharp
[Register("BikeView")]
public class BikeView : MKMarkerAnnotationView
{
  public static UIColor UnicycleColor = UIColor.FromRGB(254, 122, 36);
  public static UIColor TricycleColor = UIColor.FromRGB(153, 180, 44);
  public override IMKAnnotation Annotation
  {
    get {
      return base.Annotation;
    }
    set {
      base.Annotation = value;

      var bike = value as Bike;
      if (bike != null){
        ClusteringIdentifier = "bike";
        switch(bike.Type){
          case BikeType.Unicycle:
            MarkerTintColor = UnicycleColor;
            GlyphImage = UIImage.FromBundle("Unicycle");
            DisplayPriority = MKFeatureDisplayPriority.DefaultLow;
            break;
          case BikeType.Tricycle:
            MarkerTintColor = TricycleColor;
            GlyphImage = UIImage.FromBundle("Tricycle");
            DisplayPriority = MKFeatureDisplayPriority.DefaultHigh;
            break;
        }
      }
    }
  }
```

### <a name="3-create-an-mkannotationview-to-represent-clusters-of-markers"></a>3. Créer une `MKAnnotationView` pour représenter les clusters de marqueurs

Lors de l’affichage de l’annotation qui représente un cluster de marqueurs _impossible_ être une image simple, les utilisateurs attendent l’application de fournir des signaux visuels sur les marqueurs combien ont été regroupées.

Le [exemple de code](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) utilise CoreGraphics pour afficher le nombre de marqueurs dans le cluster, ainsi que d’une représentation graphique de cercle de la proportion de chaque type de marqueur.

Vous devez également définir :

- **DisplayPriority** – détermine l’ordre de plan (comportement empilement) lorsque le mappage est chargé de marqueurs. Utilisez une des `Required`, `DefaultHigh`, ou `DefaultLow`.
- **CollisionMode** – `Circle` ou `Rectangle`.

```csharp
[Register("ClusterView")]
public class ClusterView : MKAnnotationView
{
  public static UIColor ClusterColor = UIColor.FromRGB(202, 150, 38);
  public override IMKAnnotation Annotation
  {
    get {
      return base.Annotation;
    }
    set {
      base.Annotation = value;
      var cluster = MKAnnotationWrapperExtensions.UnwrapClusterAnnotation(value);
      if (cluster != null)
      {
        var renderer = new UIGraphicsImageRenderer(new CGSize(40, 40));
        var count = cluster.MemberAnnotations.Length;
        var unicycleCount = CountBikeType(cluster.MemberAnnotations, BikeType.Unicycle);

        Image = renderer.CreateImage((context) => {
          // Fill full circle with tricycle color
          BikeView.TricycleColor.SetFill();
          UIBezierPath.FromOval(new CGRect(0, 0, 40, 40)).Fill();
          // Fill pie with unicycle color
          BikeView.UnicycleColor.SetFill();
          var piePath = new UIBezierPath();
          piePath.AddArc(new CGPoint(20,20), 20, 0, (nfloat)(Math.PI * 2.0 * unicycleCount / count), true);
          piePath.AddLineTo(new CGPoint(20, 20));
          piePath.ClosePath();
          piePath.Fill();
          // Fill inner circle with white color
          UIColor.White.SetFill();
          UIBezierPath.FromOval(new CGRect(8, 8, 24, 24)).Fill();
          // Finally draw count text vertically and horizontally centered
          var attributes = new UIStringAttributes() {
            ForegroundColor = UIColor.Black,
            Font = UIFont.BoldSystemFontOfSize(20)
          };
          var text = new NSString($"{count}");
          var size = text.GetSizeUsingAttributes(attributes);
          var rect = new CGRect(20 - size.Width / 2, 20 - size.Height / 2, size.Width, size.Height);
          text.DrawString(rect, attributes);
        });
      }
    }
  }
  public ClusterView(){}
  public ClusterView(MKAnnotation annotation, string reuseIdentifier) : base(annotation, reuseIdentifier)
  {
    DisplayPriority = MKFeatureDisplayPriority.DefaultHigh;
    CollisionMode = MKAnnotationViewCollisionMode.Circle;
    // Offset center point to animate better with marker annotations
    CenterOffset = new CoreGraphics.CGPoint(0, -10);
  }
  private nuint CountBikeType(IMKAnnotation[] members, BikeType type) {
    nuint count = 0;
    foreach(Bike member in members){
      if (member.Type == type) ++count;
    }
    return count;
  }
}
```

### <a name="4-register-the-view-classes"></a>4. Inscrire les classes d’affichage

Lorsque le contrôle d’affichage de carte est créé et ajouté à une vue, d’inscrire les types d’affichage d’annotation pour activer le comportement de clustering automatique comme la carte est un zoom avant et arrière :

```csharp
MapView.Register(typeof(BikeView), MKMapViewDefault.AnnotationViewReuseIdentifier);
MapView.Register(typeof(ClusterView), MKMapViewDefault.ClusterAnnotationViewReuseIdentifier);
```

### <a name="5-render-the-map"></a>5. Restituer la carte !

Lors du rendu, le mappage des marqueurs d’annotation seront mis en cluster ou rendus selon le niveau de zoom. Le niveau de zoom que les modifications apportées, marqueurs animer vers et depuis des clusters.

![Simulateur montrant des marqueurs de cluster sur la carte](mapkit-images/cyclemap-sml.png)

Reportez-vous à la [mappe section](~/ios/user-interface/controls/ios-maps/index.md) pour plus d’informations sur l’affichage des données avec MapKit.

<a name="compass" />

## <a name="compass-button"></a>Bouton boussole

iOS 11 ajoute la possibilité de dépiler la boussole hors de la carte et le rendre ailleurs dans la vue. Consultez le [Tandm, exemple d’application](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) pour obtenir un exemple.

Créer un bouton qui ressemble à une boussole (y compris l’animation dynamique lors de la modification de l’orientation de la carte), et le restitue sur un autre contrôle.

![Bouton boussole dans la barre de navigation](mapkit-images/compass-sml.png)

Le code ci-dessous crée un bouton boussole et effectue le rendu dans la barre de navigation :

```csharp
var compass = MKCompassButton.FromMapView(MapView);
compass.CompassVisibility = MKFeatureVisibility.Visible;
NavigationItem.RightBarButtonItem = new UIBarButtonItem(compass);
MapView.ShowsCompass = false; // so we don't have two compasses!
```

Le `ShowsCompass` propriété peut être utilisée pour contrôler la visibilité de la boussole par défaut à l’intérieur de la vue cartographique.

<a name="scale" />

## <a name="scale-view"></a>Affichage de l’échelle

Ajouter l’échelle ailleurs dans la vue à l’aide de la `MKScaleView.FromMapView()` méthode pour obtenir une instance de la vue de la mise à l’échelle pour ajouter un autre emplacement dans la hiérarchie.

![Affichage de l’échelle à superposer sur une carte](mapkit-images/scale-sml.png)

```csharp
var scale = MKScaleView.FromMapView(MapView);
scale.LegendAlignment = MKScaleViewAlignment.Trailing;
scale.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(scale); // constraints omitted for simplicity
MapView.ShowsScale = false; // so we don't have two scale displays!
```

Le `ShowsScale` propriété peut être utilisée pour contrôler la visibilité de la boussole par défaut à l’intérieur de la vue cartographique.

<a name="user-tracking" />

## <a name="user-tracking-button"></a>Bouton de suivi utilisateur

Le bouton de suivi utilisateur centre la carte sur l’emplacement actuel de l’utilisateur. Utilisez la `MKUserTrackingButton.FromMapView()` méthode pour obtenir une instance du bouton, mise en forme et ajouter un autre emplacement dans la hiérarchie.

![Bouton d’emplacement utilisateur à superposer sur une carte](mapkit-images/user-location-sml.png)

```csharp
var button = MKUserTrackingButton.FromMapView(MapView);
button.Layer.BackgroundColor = UIColor.FromRGBA(255,255,255,80).CGColor;
button.Layer.BorderColor = UIColor.White.CGColor;
button.Layer.BorderWidth = 1;
button.Layer.CornerRadius = 5;
button.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(button); // constraints omitted for simplicity
```


## <a name="related-links"></a>Liens associés

- [Exemple MapKit 'Tandm'](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/)
- [MKCompassButton](https://developer.apple.com/documentation/mapkit/mkcompassbutton)
- [Nouveautés dans MapKit (WWDC) (vidéo)](https://developer.apple.com/videos/play/wwdc2017/237/)
