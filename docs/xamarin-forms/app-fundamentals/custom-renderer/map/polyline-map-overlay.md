---
title: "Un itinéraire sur une carte de mise en surbrillance"
description: "Cet article explique comment ajouter un segment de recouvrement polyligne à une carte. Un segment de recouvrement polyligne est une série de segments de ligne connectés qui sont généralement utilisées pour afficher un itinéraire sur une carte ou de forme qui est requis."
ms.topic: article
ms.prod: xamarin
ms.assetid: FBFDC715-1654-4188-82A0-FC522548BCFF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: c7becef16009596148b4de28e4e8f6892cb44fe1
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="highlighting-a-route-on-a-map"></a>Un itinéraire sur une carte de mise en surbrillance

_Cet article explique comment ajouter un segment de recouvrement polyligne à une carte. Un segment de recouvrement polyligne est une série de segments de ligne connectés qui sont généralement utilisées pour afficher un itinéraire sur une carte ou de forme qui est requis._

## <a name="overview"></a>Vue d'ensemble

Un segment de recouvrement est un graphique en couche sur une carte. Superpositions prend en charge le dessin contenu graphique qui met à l’échelle avec la carte comme il est agrandi ou réduit. Les captures d’écran suivantes affichent le résultat de l’ajout d’un segment de recouvrement polyligne à une carte :

![](polyline-map-overlay-images/screenshots.png)

Lorsqu’un [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) contrôle est restitué par une application de Xamarin.Forms, dans iOS le `MapRenderer` classe est instanciée, qui instancie ensuite natif `MKMapView` contrôle. Sur la plateforme Android, le `MapRenderer` classe instancie natif `MapView` contrôle. Sur la plate-forme de Windows universelle (UWP), le `MapRenderer` classe instancie natif `MapControl`. Le processus de rendu peut être exploitée pour implémenter des personnalisations de mappage de plateforme spécifique en créant un convertisseur personnalisé pour un `Map` sur chaque plateforme. Le processus pour cette opération est la suivante :

1. [Créer](#Creating_the_Custom_Map) une carte personnalisée Xamarin.Forms.
1. [Consommer](#Consuming_the_Custom_Map) la carte personnalisée à partir de Xamarin.Forms.
1. [Personnaliser](#Customizing_the_Map) le mappage en créant un convertisseur personnalisé pour le mappage sur chaque plateforme.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) doit être initialisé et configuré avant des utiliser. Pour plus d’informations, consultez [ `Maps Control` ](~/xamarin-forms/user-interface/map.md).

Pour plus d’informations sur la personnalisation d’un mappage à l’aide d’un convertisseur personnalisé, consultez [personnalisation d’un code confidentiel de carte](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Création de la carte personnalisée

Créer une sous-classe de la [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) (classe), qui ajoute un `RouteCoordinates` propriété :

```csharp
public class CustomMap : Map
{
  public List<Position> RouteCoordinates { get; set; }

  public CustomMap ()
  {
    RouteCoordinates = new List<Position> ();
  }
}
```

Le `RouteCoordinates` propriété stocke une collection de coordonnées qui définissent l’itinéraire à être mis en surbrillance.

<a name="Consuming_the_Custom_Map" />

### <a name="consuming-the-custom-map"></a>Utilisation de la carte personnalisée

Consommer le `CustomMap` contrôle en déclarant une instance de celui-ci dans l’instance de la page XAML :

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

Vous pouvez également utiliser le `CustomMap` contrôle en déclarant une instance de celui-ci dans l’instance de page c# :

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
    customMap.RouteCoordinates.Add (new Position (37.785559, -122.396728));
    customMap.RouteCoordinates.Add (new Position (37.780624, -122.390541));
    customMap.RouteCoordinates.Add (new Position (37.777113, -122.394983));
    customMap.RouteCoordinates.Add (new Position (37.776831, -122.394627));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
  }
}
```

Cette initialisation spécifie une série de coordonnées de latitude et longitude, pour définir l’itinéraire sur la carte pour être mis en surbrillance. Il positionne ensuite la vue de la carte avec le [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) (méthode), ce qui modifie la position et le niveau de zoom de la carte en créant un [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) d’un [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) et un [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Personnalisation de la carte

Un convertisseur personnalisé doit maintenant être ajouté à chaque projet d’application pour ajouter le segment de recouvrement polyligne au mappage.

#### <a name="creating-the-custom-renderer-on-ios"></a>Création du convertisseur personnalisé sur iOS

Créer une sous-classe de la `MapRenderer` classe et substituer sa `OnElementChanged` méthode pour ajouter le segment de recouvrement polyligne :

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKPolylineRenderer polylineRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    polylineRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                nativeMap.OverlayRenderer = GetOverlayRenderer;

                CLLocationCoordinate2D[] coords = new CLLocationCoordinate2D[formsMap.RouteCoordinates.Count];
                int index = 0;
                foreach (var position in formsMap.RouteCoordinates)
                {
                    coords[index] = new CLLocationCoordinate2D(position.Latitude, position.Longitude);
                    index++;
                }

                var routeOverlay = MKPolyline.FromCoordinates(coords);
                nativeMap.AddOverlay(routeOverlay);
            }
        }
        ...
    }
}

```

Cette méthode effectue la configuration suivante, sous réserve que le convertisseur personnalisé est attaché à un nouvel élément Xamarin.Forms :

- Le `MKMapView.OverlayRenderer` est définie sur un délégué correspondant.
- La collection de coordonnées de latitude et longitude sont récupérés à partir de la `CustomMap.RouteCoordinates` propriété et stockées sous la forme d’un tableau de `CLLocationCoordinate2D` instances.
- La polyligne est créée en appelant la méthode statique `MKPolyline.FromCoordinates` (méthode), qui spécifie la latitude et la longitude de chaque point.
- La polyligne est ajoutée à la carte en appelant le `MKMapView.AddOverlay` (méthode).

Ensuite, implémentez la `GetOverlayRenderer` méthode pour personnaliser le rendu de la superposition :

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKPolylineRenderer polylineRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (polylineRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          polylineRenderer = new MKPolylineRenderer(overlay as MKPolyline) {
              FillColor = UIColor.Blue,
              StrokeColor = UIColor.Red,
              LineWidth = 3,
              Alpha = 0.4f
          };
      }
      return polylineRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Création du convertisseur personnalisé sur Android

Créer une sous-classe de la `MapRenderer` classe et substituer sa `OnElementChanged` et `OnMapReady` méthodes pour ajouter le segment de recouvrement polyligne :

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        List<Position> routeCoordinates;

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
                routeCoordinates = formsMap.RouteCoordinates;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var polylineOptions = new PolylineOptions();
            polylineOptions.InvokeColor(0x66FF0000);

            foreach (var position in routeCoordinates)
            {
                polylineOptions.Add(new LatLng(position.Latitude, position.Longitude));
            }

            NativeMap.AddPolyline(polylineOptions);
        }
    }
}
```

Le `OnElementChanged` méthode récupère la collection de coordonnées de latitude et la longitude de le `CustomMap.RouteCoordinates` propriété et les stocke dans une variable membre. Il appelle ensuite la `MapView.GetMapAsync` (méthode), qui obtient sous-jacent `GoogleMap` qui est lié à la vue, sous réserve que le convertisseur personnalisé est attaché à un nouvel élément de Xamarin.Forms. Une fois la `GoogleMap` instance n’est disponible, le `OnMapReady` méthode sera appelée, où la polyligne est créée en instanciant une `PolylineOptions` objet qui spécifie la latitude et la longitude de chaque point. La polyligne est ensuite ajoutée à la carte en appelant le `NativeMap.AddPolyline` (méthode).

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Création du convertisseur personnalisé sur la plateforme Windows universelle

Créer une sous-classe de la `MapRenderer` classe et substituer sa `OnElementChanged` méthode pour ajouter le segment de recouvrement polyligne :

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
                foreach (var position in formsMap.RouteCoordinates)
                {
                    coordinates.Add(new BasicGeoposition() { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polyline = new MapPolyline();
                polyline.StrokeColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polyline.StrokeThickness = 5;
                polyline.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polyline);
            }
        }
    }
}
```

Cette méthode effectue les opérations suivantes, sous réserve que le convertisseur personnalisé est attaché à un nouvel élément Xamarin.Forms :

- La collection de coordonnées de latitude et longitude sont récupérés à partir de la `CustomMap.RouteCoordinates` propriété et converti en un `List` de `BasicGeoposition` coordonnées.
- La polyligne est créée en instanciant une `MapPolyline` objet. Le `MapPolygon` classe est utilisée pour afficher une ligne sur la carte en définissant son `Path` propriété un `Geopath` objet qui contient les coordonnées de la ligne.
- La polyligne est rendue sur la carte en l’ajoutant à la `MapControl.MapElements` collection.

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment ajouter un segment de recouvrement polyligne à une carte, d’afficher un itinéraire sur une carte, ou de forme qui est requis.


## <a name="related-links"></a>Liens associés

- [Mappage de type polyligne Ovlerlay (exemple)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polyline/)
- [Personnalisation d’un code confidentiel de carte](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/)
