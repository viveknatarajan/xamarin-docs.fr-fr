---
title: Mise en surbrillance d’une région sur une carte
description: Cet article explique comment ajouter un segment de recouvrement de polygone à une carte, pour mettre en surbrillance une région sur la carte. Les polygones sont une forme fermée et que leurs intérieurs renseigné.
ms.prod: xamarin
ms.assetid: E79EB2CF-8DD6-44A8-B47D-5F0A94FB0A63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 0a11e9c25922531727ad2fee3bbed9c8d4e2b80c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998132"
---
# <a name="highlighting-a-region-on-a-map"></a>Mise en surbrillance d’une région sur une carte

_Cet article a expliqué comment ajouter un segment de recouvrement de polygone à une carte, pour mettre en surbrillance une région sur la carte. Les polygones sont une forme fermée et que leurs intérieurs renseigné._

## <a name="overview"></a>Vue d'ensemble

Une superposition est un graphique en couche sur une carte. Superpositions prend en charge le dessin contenu graphique qui s’adapte à la carte, comme elle est redimensionnée. Les captures d’écran suivantes montrent le résultat de l’ajout d’un segment de recouvrement de polygone à une carte :

![](polygon-map-overlay-images/screenshots.png)

Quand un [ `Map` ](xref:Xamarin.Forms.Maps.Map) contrôle est restitué par une application Xamarin.Forms, dans iOS le `MapRenderer` classe est instanciée, ce qui instancie à son tour native `MKMapView` contrôle. Sur la plateforme Android, le `MapRenderer` classe instancie native `MapView` contrôle. Sur la plateforme de Windows universelle (UWP), le `MapRenderer` classe instancie native `MapControl`. Le processus de rendu peut être exploitée pour implémenter les personnalisations spécifiques à la plateforme mappage en créant un convertisseur personnalisé pour un `Map` sur chaque plateforme. Le processus pour effectuer cette opération est la suivante :

1. [Créer](#Creating_the_Custom_Map) une carte personnalisée Xamarin.Forms.
1. [Consommer](#Consuming_the_Custom_Map) la carte personnalisée à partir de Xamarin.Forms.
1. [Personnaliser](#Customizing_the_Map) la carte en créant un convertisseur personnalisé pour la carte sur chaque plateforme.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) doit être initialisé et configuré avant utilisation. Pour plus d’informations, consultez [`Maps Control`](~/xamarin-forms/user-interface/map.md).

Pour plus d’informations sur la personnalisation d’une carte à l’aide d’un convertisseur personnalisé, consultez [personnalisation d’un code confidentiel de carte](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Création de la carte personnalisée

Créer une sous-classe de la [ `Map` ](xref:Xamarin.Forms.Maps.Map) (classe), qui ajoute un `ShapeCoordinates` propriété :

```csharp
public class CustomMap : Map
{
  public List<Position> ShapeCoordinates { get; set; }

  public CustomMap ()
  {
    ShapeCoordinates = new List<Position> ();
  }
}
```

Le `ShapeCoordinates` propriété stocke une collection de coordonnées qui définissent la région à mettre en surbrillance.

<a name="Consuming_the_Custom_Map" />

### <a name="consuming-the-custom-map"></a>Utilisation de la carte personnalisée

Consommer le `CustomMap` contrôle en déclarant une instance de celle-ci dans l’instance de la page XAML :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MapOverlay;assembly=MapOverlay"
             x:Class="MapOverlay.MapPage">
    <ContentPage.Content>
        <local:CustomMap x:Name="customMap" MapType="Street" WidthRequest="{x:Static local:App.ScreenWidth}" HeightRequest="{x:Static local:App.ScreenHeight}" />
    </ContentPage.Content>
</ContentPage>
```

Vous pouvez également utiliser le `CustomMap` contrôle en déclarant une instance de celle-ci dans l’instance de page c# :

```csharp
public class MapPageCS : ContentPage
{
    public MapPageCS ()
    {
        var customMap = new CustomMap {
            MapType = MapType.Street,
            WidthRequest = App.ScreenWidth,
            HeightRequest = App.ScreenHeight
        };
        ...
        Content = customMap;
    }
}
```

Initialiser le `CustomMap` contrôler en fonction des besoins :

```csharp
public partial class MapPage : ContentPage
{
  public MapPage ()
  {
    ...
    customMap.ShapeCoordinates.Add (new Position (37.797513, -122.402058));
    customMap.ShapeCoordinates.Add (new Position (37.798433, -122.402256));
    customMap.ShapeCoordinates.Add (new Position (37.798582, -122.401071));
    customMap.ShapeCoordinates.Add (new Position (37.797658, -122.400888));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (0.1)));
  }
}
```

Cette initialisation spécifie une série de coordonnées de latitude et longitude, pour définir la région de la carte à mettre en surbrillance. Il positionne ensuite la vue de la carte avec le [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) (méthode), ce qui modifie la position et le niveau de zoom de la carte en créant un [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan) à partir d’un [ `Position` ](xref:Xamarin.Forms.Maps.Position) et un [ `Distance` ](xref:Xamarin.Forms.Maps.Distance).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Personnalisation de la carte

Un convertisseur personnalisé doit maintenant être ajouté à chaque projet d’application pour ajouter la superposition de polygone à la carte.

#### <a name="creating-the-custom-renderer-on-ios"></a>Création du convertisseur personnalisé sur iOS

Créer une sous-classe de la `MapRenderer` classe et substituer sa `OnElementChanged` méthode pour ajouter la superposition de polygone :

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKPolygonRenderer polygonRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    polygonRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;

                nativeMap.OverlayRenderer = GetOverlayRenderer;

                CLLocationCoordinate2D[] coords = new CLLocationCoordinate2D[formsMap.ShapeCoordinates.Count];

                int index = 0;
                foreach (var position in formsMap.ShapeCoordinates)
                {
                    coords[index] = new CLLocationCoordinate2D(position.Latitude, position.Longitude);
                    index++;
                }

                var blockOverlay = MKPolygon.FromCoordinates(coords);
                nativeMap.AddOverlay(blockOverlay);
            }
        }
        ...
    }
}

```

Cette méthode effectue la configuration suivante, sous réserve que le convertisseur personnalisé est attaché à un nouvel élément Xamarin.Forms :

- Le `MKMapView.OverlayRenderer` propriété est définie sur un délégué correspondant.
- La collection de coordonnées de latitude et longitude sont récupérés à partir de la `CustomMap.ShapeCoordinates` propriété et stockées en tant que tableau de `CLLocationCoordinate2D` instances.
- Le polygone est créé en appelant la méthode statique `MKPolygon.FromCoordinates` (méthode), qui spécifie la latitude et la longitude de chaque point.
- Le polygone est ajouté à la carte en appelant le `MKMapView.AddOverlay` (méthode). Cette méthode ferme automatiquement le polygone en dessinant une ligne qui relie le premier et dernier point.

Ensuite, implémentez la `GetOverlayRenderer` méthode pour personnaliser le rendu de la superposition :

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKPolygonRenderer polygonRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (polygonRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          polygonRenderer = new MKPolygonRenderer(overlay as MKPolygon) {
              FillColor = UIColor.Red,
              StrokeColor = UIColor.Blue,
              Alpha = 0.4f,
              LineWidth = 9
          };
      }
      return polygonRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Création du convertisseur personnalisé sur Android

Créer une sous-classe de la `MapRenderer` classe et substituer sa `OnElementChanged` et `OnMapReady` méthodes pour ajouter la superposition de polygone :

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        List<Position> shapeCoordinates;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                shapeCoordinates = formsMap.ShapeCoordinates;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var polygonOptions = new PolygonOptions();
            polygonOptions.InvokeFillColor(0x66FF0000);
            polygonOptions.InvokeStrokeColor(0x660000FF);
            polygonOptions.InvokeStrokeWidth(30.0f);

            foreach (var position in shapeCoordinates)
            {
                polygonOptions.Add(new LatLng(position.Latitude, position.Longitude));
            }
            NativeMap.AddPolygon(polygonOptions);
        }
    }
}
```

Le `OnElementChanged` méthode récupère la collection de coordonnées de latitude et longitude à partir de la `CustomMap.ShapeCoordinates` propriété et les stocke dans une variable membre. Il appelle ensuite la `MapView.GetMapAsync` (méthode), qui obtient sous-jacent `GoogleMap` qui est lié à la vue, sous réserve que le convertisseur personnalisé est attaché à un nouvel élément Xamarin.Forms. Une fois le `GoogleMap` instance n’est disponible, le `OnMapReady` méthode sera appelée, où le polygone est créé en instanciant un `PolygonOptions` objet qui spécifie la latitude et la longitude de chaque point. Le polygone est ensuite ajouté à la carte en appelant le `NativeMap.AddPolygon` (méthode). Cette méthode ferme automatiquement le polygone en dessinant une ligne qui relie le premier et dernier point.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Création du convertisseur personnalisé sur la plateforme Windows universelle

Créer une sous-classe de la `MapRenderer` classe et substituer sa `OnElementChanged` méthode pour ajouter la superposition de polygone :

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
               // Unsubscribe
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MapControl;

                var coordinates = new List<BasicGeoposition>();
                foreach (var position in formsMap.ShapeCoordinates)
                {
                    coordinates.Add(new BasicGeoposition() { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polygon = new MapPolygon();
                polygon.FillColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeColor = Windows.UI.Color.FromArgb(128, 0, 0, 255);
                polygon.StrokeThickness = 5;
                polygon.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polygon);
            }
        }
    }
}
```

Cette méthode effectue les opérations suivantes, si le convertisseur personnalisé est attaché à un nouvel élément Xamarin.Forms :

- La collection de coordonnées de latitude et longitude sont récupérés à partir de la `CustomMap.ShapeCoordinates` propriété et convertie en un `List` de `BasicGeoposition` coordonnées.
- Le polygone est créé en instanciant un `MapPolygon` objet. Le `MapPolygon` classe est utilisée pour afficher une forme multipoint sur la carte en définissant son `Path` propriété un `Geopath` objet qui contient les coordonnées de la forme.
- Le polygone est rendu sur la carte en l’ajoutant à la `MapControl.MapElements` collection. Notez que le polygone est fermé automatiquement en dessinant une ligne qui relie le premier et dernier point.

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment ajouter un segment de recouvrement de polygone à une carte, pour mettre en surbrillance une région de la carte. Les polygones sont une forme fermée et que leurs intérieurs renseigné.


## <a name="related-links"></a>Liens associés

- [Polygone de carte de superposition (exemple)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polygon/)
- [Personnalisation d’une épingle de carte](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
