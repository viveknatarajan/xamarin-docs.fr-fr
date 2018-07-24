---
title: Personnalisation d’un code confidentiel de carte
description: Cet article montre comment créer un convertisseur personnalisé pour le contrôle de carte qui affiche une carte native avec un code confidentiel personnalisé et une vue personnalisée des données de code pin sur chaque plateforme.
ms.prod: xamarin
ms.assetid: C5481D86-80E9-4E3D-9FB6-57B0F93711A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 351119a8b0089f78d4ce98729a1516c3cd7bae7b
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39203083"
---
# <a name="customizing-a-map-pin"></a>Personnalisation d’un code confidentiel de carte

_Cet article montre comment créer un convertisseur personnalisé pour le contrôle de carte qui affiche une carte native avec un code confidentiel personnalisé et une vue personnalisée des données de code pin sur chaque plateforme._

Chaque vue Xamarin.Forms a un convertisseur qui accompagne cet article pour chaque plateforme qui crée une instance d’un contrôle natif. Quand un [ `Map` ](xref:Xamarin.Forms.Maps.Map) est restitué par une application Xamarin.Forms dans iOS, le `MapRenderer` classe est instanciée, ce qui instancie à son tour native `MKMapView` contrôle. Sur la plateforme Android, le `MapRenderer` classe instancie native `MapView` contrôle. Sur la plateforme de Windows universelle (UWP), le `MapRenderer` classe instancie native `MapControl`. Pour plus d’informations sur les classes de contrôle natif correspondant aux contrôles Xamarin.Forms et le convertisseur, consultez [convertisseur des Classes de Base et des contrôles natifs](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Le diagramme suivant illustre la relation entre la [ `Map` ](xref:Xamarin.Forms.Maps.Map) et les contrôles natifs correspondants qui l’implémentent :

![](customized-pin-images/map-classes.png "Relation entre le contrôle de carte et l’implémentation des contrôles natifs")

Le processus de rendu peut être utilisé pour implémenter les personnalisations spécifiques à la plateforme en créant un convertisseur personnalisé pour un [ `Map` ](xref:Xamarin.Forms.Maps.Map) sur chaque plateforme. Le processus pour effectuer cette opération est la suivante :

1. [Créer](#Creating_the_Custom_Map) une carte personnalisée Xamarin.Forms.
1. [Consommer](#Consuming_the_Custom_Map) la carte personnalisée à partir de Xamarin.Forms.
1. [Créer](#Creating_the_Custom_Renderer_on_each_Platform) le convertisseur personnalisé pour la carte sur chaque plateforme.

Chaque élément maintenant nous reviendrons à son tour, pour implémenter un `CustomMap` convertisseur qui affiche une carte native avec un code confidentiel personnalisé et une vue personnalisée des données de code pin sur chaque plateforme.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) doit être initialisé et configuré avant utilisation. Pour plus d’informations, consultez [`Maps Control`](~/xamarin-forms/user-interface/map.md).

<a name="Creating_the_Custom_Map" />

## <a name="creating-the-custom-map"></a>Création de la carte personnalisée

Un contrôle de carte personnalisée peut être créé en sous-classant la [ `Map` ](xref:Xamarin.Forms.Maps.Map) classe, comme indiqué dans l’exemple de code suivant :

```csharp
public class CustomMap : Map
{
  public List<CustomPin> CustomPins { get; set; }
}
```

Le `CustomMap` contrôle est créé dans le projet de bibliothèque .NET Standard et définit l’API pour la carte personnalisée. La carte personnalisée expose le `CustomPins` propriété qui représente la collection de `CustomPin` les objets qui seront affichés par le contrôle de carte natif sur chaque plateforme. Le `CustomPin` classe est illustrée dans l’exemple de code suivant :

```csharp
public class CustomPin : Pin
{
  public string Url { get; set; }
}
```

Cette classe définit un `CustomPin` comme héritant des propriétés de la [ `Pin` ](xref:Xamarin.Forms.Maps.Pin) classe et en ajoutant un `Url` propriété.

<a name="Consuming_the_Custom_Map" />

## <a name="consuming-the-custom-map"></a>Utilisation de la carte personnalisée

Le `CustomMap` contrôle peut être référencé dans le XAML dans le projet de bibliothèque .NET Standard en déclarant un espace de noms pour son emplacement et en utilisant le préfixe d’espace de noms sur le contrôle de carte personnalisée. Le code suivant montre l’exemple comment la `CustomMap` contrôle peut être consommé par une page XAML :

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer">
    <ContentPage.Content>
        <local:CustomMap x:Name="myMap" MapType="Street"
          WidthRequest="{x:Static local:App.ScreenWidth}"
          HeightRequest="{x:Static local:App.ScreenHeight}" />
    </ContentPage.Content>
</ContentPage>
```

Le `local` préfixe d’espace de noms peut être n’importe quel nom. Toutefois, le `clr-namespace` et `assembly` valeurs doivent correspondre aux détails de la carte personnalisée. Une fois que l’espace de noms est déclaré, le préfixe est utilisé pour référencer la carte personnalisée.

Le code suivant montre l’exemple comment la `CustomMap` contrôle peut être consommé par une page c# :

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

Le `CustomMap` instance sera utilisée pour afficher la carte native sur chaque plateforme. Il a [ `MapType` ](xref:Xamarin.Forms.Maps.Map.MapType) propriété définit le style d’affichage de la [ `Map` ](xref:Xamarin.Forms.Maps.Map), avec les valeurs possibles, qui est définis dans le [ `MapType` ](xref:Xamarin.Forms.Maps.MapType) énumération. Pour iOS et Android, la largeur et la hauteur de la carte est définie via les propriétés de la `App` classe sont initialisés dans les projets spécifiques à la plateforme.

L’emplacement de la carte et les codes confidentiels, il contient, sont initialisées comme indiqué dans l’exemple de code suivant :

```csharp
public MapPage ()
{
  ...
  var pin = new CustomPin {
    Type = PinType.Place,
    Position = new Position (37.79752, -122.40183),
    Label = "Xamarin San Francisco Office",
    Address = "394 Pacific Ave, San Francisco CA",
    Id = "Xamarin",
    Url = "http://xamarin.com/about/"
  };

  customMap.CustomPins = new List<CustomPin> { pin };
  customMap.Pins.Add (pin);
  customMap.MoveToRegion (MapSpan.FromCenterAndRadius (
    new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
}
```

Cette initialisation ajoute un code confidentiel personnalisé et positionne la vue de la carte avec le [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) (méthode), ce qui modifie la position et le niveau de zoom de la carte en créant un [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan) à partir d’un [ `Position` ](xref:Xamarin.Forms.Maps.Position) et un [ `Distance` ](xref:Xamarin.Forms.Maps.Distance).

Un convertisseur personnalisé peut maintenant être ajouté à chaque projet d’application pour personnaliser les contrôles de carte natif.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Création du convertisseur personnalisé sur chaque plateforme

Le processus de création de la classe de convertisseur personnalisé est comme suit :

1. Créer une sous-classe de la `MapRenderer` classe qui rend la carte personnalisée.
1. Remplacer le `OnElementChanged` méthode qui restitue la carte personnalisée et écrire la logique pour le personnaliser. Cette méthode est appelée lorsque le mappage personnalisé Xamarin.Forms correspondant est créé.
1. Ajouter un `ExportRenderer` d’attribut à la classe de convertisseur personnalisé pour spécifier qu’il sera être utilisé pour restituer la carte personnalisée Xamarin.Forms. Cet attribut est utilisé pour inscrire le renderer personnalisé avec Xamarin.Forms.

> [!NOTE]
> Il est facultatif pour fournir un convertisseur personnalisé dans chaque projet de plateforme. Si un convertisseur personnalisé n’est pas inscrit, le convertisseur par défaut pour la classe de base du contrôle sera être utilisé.

Le diagramme suivant illustre les responsabilités de chaque projet dans l’exemple d’application, ainsi que les relations entre eux :

![](customized-pin-images/solution-structure.png "Responsabilités de projet de convertisseur CustomMap personnalisé")

Le `CustomMap` contrôle est restitué par des classes de renderer spécifique à la plateforme, qui dérivent de la `MapRenderer` classe pour chaque plateforme. Ainsi, chaque `CustomMap` contrôler le rendu avec des contrôles spécifiques à la plateforme, comme indiqué dans les captures d’écran suivante :

![](customized-pin-images/screenshots.png "CustomMap sur chaque plateforme")

Le `MapRenderer` classe expose le `OnElementChanged` (méthode), qui est appelé lorsque le mappage personnalisé Xamarin.Forms est créé pour restituer le contrôle natif correspondant. Cette méthode prend un `ElementChangedEventArgs` paramètre contienne `OldElement` et `NewElement` propriétés. Ces propriétés représentent l’élément Xamarin.Forms qui le convertisseur *a été* associée et l’élément Xamarin.Forms qui le convertisseur *est* attaché, respectivement. Dans l’exemple d’application le `OldElement` propriété sera `null` et `NewElement` propriété contiendra une référence à la `CustomMap` instance.

Une version substituée de la `OnElementChanged` (méthode), dans chaque classe de renderer spécifique à la plateforme, est l’endroit où effectuer la personnalisation du contrôle natif. Une référence typée au contrôle natif utilisé sur la plateforme sont accessibles via le `Control` propriété. En outre, une référence au contrôle Xamarin.Forms qui est rendu peut être obtenue via la `Element` propriété.

Être vigilant lors de l’abonnement aux gestionnaires d’événements dans le `OnElementChanged` (méthode), comme illustré dans l’exemple de code suivant :

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null) {
    // Unsubscribe from event handlers
  }

  if (e.NewElement != null) {
    // Configure the native control and subscribe to event handlers
  }
}
```

Le contrôle natif doit être configuré et abonner aux gestionnaires d’événements uniquement quand le renderer personnalisé est attaché à un nouvel élément Xamarin.Forms. De même, les gestionnaires d’événements qui ont été abonnés à doivent être annulés que si l’élément auquel le convertisseur est associé à des modifications. Adopter cette approche vous aidera à créer un convertisseur personnalisé qui ne connaît pas les fuites de mémoire.

Chaque classe de convertisseur personnalisé est décorée avec un `ExportRenderer` attribut qui inscrit le convertisseur avec Xamarin.Forms. L’attribut accepte deux paramètres : le nom de type du contrôle personnalisé Xamarin.Forms en cours d’affichage et le nom de type du convertisseur personnalisé. Le `assembly` préfixe à l’attribut spécifie que l’attribut s’applique à la totalité de l’assembly.

Les sections suivantes décrivent l’implémentation de chaque classe de convertisseur personnalisé spécifique à la plateforme.

### <a name="creating-the-custom-renderer-on-ios"></a>Création du convertisseur personnalisé sur iOS

Les captures d’écran suivantes montrent le mappage, avant et après la personnalisation :

![](customized-pin-images/map-layout-ios.png "Contrôle de carte avant et après la personnalisation")

Sur iOS le code pin est appelé un *annotation*, et peut être une image personnalisée ou un code confidentiel de définie par le système de couleurs différentes. Afficher les annotations peuvent éventuellement un *légende*, qui s’affiche en réponse à l’utilisateur en sélectionnant l’annotation. La légende affiche les `Label` et `Address` propriétés de la `Pin` instance, avec facultatif gauche et droite accessoires vues. Dans la capture d’écran ci-dessus, la vue accessoire gauche est l’image d’un monkey, la vue droite accessoire étant le *informations* bouton.

L’exemple de code suivant montre le convertisseur personnalisé pour la plateforme iOS :

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        UIView customPinView;
        List<CustomPin> customPins;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveAnnotations(nativeMap.Annotations);
                    nativeMap.GetViewForAnnotation = null;
                    nativeMap.CalloutAccessoryControlTapped -= OnCalloutAccessoryControlTapped;
                    nativeMap.DidSelectAnnotationView -= OnDidSelectAnnotationView;
                    nativeMap.DidDeselectAnnotationView -= OnDidDeselectAnnotationView;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                customPins = formsMap.CustomPins;

                nativeMap.GetViewForAnnotation = GetViewForAnnotation;
                nativeMap.CalloutAccessoryControlTapped += OnCalloutAccessoryControlTapped;
                nativeMap.DidSelectAnnotationView += OnDidSelectAnnotationView;
                nativeMap.DidDeselectAnnotationView += OnDidDeselectAnnotationView;
            }
        }
        ...
    }
}
```

Le `OnElementChanged` méthode effectue la configuration suivante de la [ `MKMapView` ](https://developer.xamarin.com/api/type/MapKit.MKMapView/) instance, la condition que le convertisseur personnalisé est attaché à un nouvel élément Xamarin.Forms :

- Le [ `GetViewForAnnotation` ](https://developer.xamarin.com/api/property/MapKit.MKMapView.GetViewForAnnotation/) propriété est définie sur le `GetViewForAnnotation` (méthode). Cette méthode est appelée lorsque le [emplacement de l’annotation est visible sur la carte](#Displaying_the_Annotation)et est utilisé pour personnaliser l’avant de l’annotation à afficher.
- Gestionnaires d’événements pour le `CalloutAccessoryControlTapped`, `DidSelectAnnotationView`, et `DidDeselectAnnotationView` événements sont inscrits. Ces événements déclenchent lorsque l’utilisateur [appuie sur l’accessoire droite de la légende](#Tapping_on_the_Right_Callout_Accessory_View)et lorsque l’utilisateur [sélectionne](#Selecting_the_Annotation) et [désélectionne](#Deselecting_the_Annotation) l’annotation, respectivement. Les événements sont désabonnés d’uniquement lorsque l’élément auquel le renderer est attaché change.

<a name="Displaying_the_Annotation" />

#### <a name="displaying-the-annotation"></a>Affichage de l’Annotation

Le `GetViewForAnnotation` méthode est appelée lorsque l’emplacement de l’annotation est visible sur la carte et est utilisé pour personnaliser l’avant de l’annotation à afficher. Une annotation comporte deux parties :

- `MkAnnotation` – inclut le titre, un sous-titre et un emplacement de l’annotation.
- `MkAnnotationView` : contient l’image pour représenter l’annotation et si vous le souhaitez, une légende qui s’affiche lorsque l’utilisateur appuie sur l’annotation.

Le `GetViewForAnnotation` méthode accepte un `IMKAnnotation` qui contient les données d’annotation et retourne un `MKAnnotationView` pour l’affichage sur la carte et est illustré dans l’exemple de code suivant :

```csharp
protected override MKAnnotationView GetViewForAnnotation(MKMapView mapView, IMKAnnotation annotation)
{
    MKAnnotationView annotationView = null;

    if (annotation is MKUserLocation)
        return null;

    var customPin = GetCustomPin(annotation as MKPointAnnotation);
    if (customPin == null) {
        throw new Exception("Custom pin not found");
    }

    annotationView = mapView.DequeueReusableAnnotation(customPin.Id.ToString());
    if (annotationView == null) {
        annotationView = new CustomMKAnnotationView(annotation, customPin.Id.ToString());
        annotationView.Image = UIImage.FromFile("pin.png");
        annotationView.CalloutOffset = new CGPoint(0, 0);
        annotationView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile("monkey.png"));
        annotationView.RightCalloutAccessoryView = UIButton.FromType(UIButtonType.DetailDisclosure);
        ((CustomMKAnnotationView)annotationView).Id = customPin.Id.ToString();
        ((CustomMKAnnotationView)annotationView).Url = customPin.Url;
    }
    annotationView.CanShowCallout = true;

    return annotationView;
}
```

Cette méthode garantit que l’annotation s’affichera sous la forme d’une image personnalisée, plutôt que comme définie par le système de code confidentiel et que lorsque l’annotation est appuyé sur une légende s’affiche qui inclut un contenu supplémentaire vers la gauche et à droite du titre de l’annotation et de l’adresse . Cela s’effectue comme suit :

1. Le `GetCustomPin` méthode est appelée pour retourner les données personnalisées de code confidentiel pour l’annotation.
1. Pour économiser la mémoire, la vue de l’annotation est regroupée à réutiliser avec l’appel à [ `DequeueReusableAnnotation` ](https://developer.xamarin.com/api/member/MapKit.MKMapView.DequeueReusableAnnotation/(System.String)/).
1. Le `CustomMKAnnotationView` classe étend la `MKAnnotationView` classe avec `Id` et `Url` propriétés qui correspondent aux propriétés identiques dans le `CustomPin` instance. Une nouvelle instance de la `CustomMKAnnotationView` est créé, à condition que l’annotation est `null`:
    - Le `CustomMKAnnotationView.Image` propriété est définie sur l’image qui représentera l’annotation sur la carte.
    - Le `CustomMKAnnotationView.CalloutOffset` propriété est définie sur une `CGPoint` qui spécifie que la légende est centrée au-dessus de l’annotation.
    - Le `CustomMKAnnotationView.LeftCalloutAccessoryView` propriété est définie sur une image d’un monkey qui apparaît à gauche du titre de l’annotation et de l’adresse.
    - Le `CustomMKAnnotationView.RightCalloutAccessoryView` propriété est définie sur une *informations* bouton qui apparaît à droite du titre de l’annotation et de l’adresse.
    - Le `CustomMKAnnotationView.Id` propriété est définie sur le `CustomPin.Id` propriété retournée par la `GetCustomPin` (méthode). Ainsi, l’annotation être identifié afin qu’il dispose [légende peut être personnalisé](#Selecting_the_Annotation), si vous le souhaitez.
    - Le `CustomMKAnnotationView.Url` propriété est définie sur le `CustomPin.Url` propriété retournée par la `GetCustomPin` (méthode). L’URL sera utilisée lorsque l’utilisateur [appuie sur le bouton affiché dans la vue accessoire légende droit](#Tapping_on_the_Right_Callout_Accessory_View).
1. Le [ `MKAnnotationView.CanShowCallout` ](https://developer.xamarin.com/api/property/MapKit.MKAnnotationView.CanShowCallout/) propriété est définie sur `true` afin que la légende s’affiche lorsque l’utilisateur appuie sur l’annotation.
1. L’annotation est retournée pour l’affichage sur la carte.

<a name="Selecting_the_Annotation" />

#### <a name="selecting-the-annotation"></a>Sélection de l’Annotation

Lorsque l’utilisateur appuie sur l’annotation, le `DidSelectAnnotationView` se déclenche des événements, qui à son tour s’exécute le `OnDidSelectAnnotationView` méthode :

```csharp
void OnDidSelectAnnotationView (object sender, MKAnnotationViewEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  customPinView = new UIView ();

  if (customView.Id == "Xamarin") {
    customPinView.Frame = new CGRect (0, 0, 200, 84);
    var image = new UIImageView (new CGRect (0, 0, 200, 84));
    image.Image = UIImage.FromFile ("xamarin.png");
    customPinView.AddSubview (image);
    customPinView.Center = new CGPoint (0, -(e.View.Frame.Height + 75));
    e.View.AddSubview (customPinView);
  }
}
```

Cette méthode étend la légende existante (qui contient les affichages gauche et droite accessoires) en ajoutant un `UIView` instance lui qui contient une image du logo Xamarin, à condition que l’annotation sélectionnée son `Id` lavaleurdepropriété`Xamarin`. Ainsi, pour les scénarios où des légendes différentes peuvent être affichées pour des annotations. Le `UIView` instance s’afficheront centré au-dessus de la légende existante.

<a name="Tapping_on_the_Right_Callout_Accessory_View" />

#### <a name="tapping-on-the-right-callout-accessory-view"></a>En appuyant sur la vue de droite accessoire de légende

Lorsque l’utilisateur appuie sur le *informations* bouton dans la vue accessoire légende droite, le `CalloutAccessoryControlTapped` se déclenche des événements, qui à son tour s’exécute le `OnCalloutAccessoryControlTapped` méthode :

```csharp
void OnCalloutAccessoryControlTapped (object sender, MKMapViewAccessoryTappedEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  if (!string.IsNullOrWhiteSpace (customView.Url)) {
    UIApplication.SharedApplication.OpenUrl (new Foundation.NSUrl (customView.Url));
  }
}
```

Cette méthode ouvre un navigateur web et accède à l’adresse stockée dans le `CustomMKAnnotationView.Url` propriété. Notez que l’adresse a été défini lors de la création du `CustomPin` collection dans le projet de bibliothèque .NET Standard.

<a name="Deselecting_the_Annotation" />

#### <a name="deselecting-the-annotation"></a>En désactivant l’Annotation

Lorsque l’annotation s’affiche et l’utilisateur appuie sur la carte, le `DidDeselectAnnotationView` se déclenche des événements, qui à son tour s’exécute le `OnDidDeselectAnnotationView` méthode :

```csharp
void OnDidDeselectAnnotationView (object sender, MKAnnotationViewEventArgs e)
{
  if (!e.View.Selected) {
    customPinView.RemoveFromSuperview ();
    customPinView.Dispose ();
    customPinView = null;
  }
}
```

Cette méthode garantit que lorsque la légende existante n’est pas sélectionnée, la partie étendue de la légende (l’image du logo Xamarin) s’arrête également affichées, et ses ressources seront libérées.

Pour plus d’informations sur la personnalisation d’un `MKMapView` une instance, consultez [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md).

### <a name="creating-the-custom-renderer-on-android"></a>Création du convertisseur personnalisé sur Android

Les captures d’écran suivantes montrent le mappage, avant et après la personnalisation :

![](customized-pin-images/map-layout-android.png "Contrôle de carte avant et après la personnalisation")

Sur Android le code pin est appelé un *marqueur*, et peut être une image personnalisée ou un marqueur définie par le système de couleurs différentes. Marqueurs peuvent afficher un *fenêtre info*, qui s’affiche en réponse à l’utilisateur en appuyant sur le marqueur. Affiche la fenêtre informations sur le `Label` et `Address` propriétés de la `Pin` de l’instance et peut être personnalisé pour inclure d’autres contenus. Toutefois, qu’une seule fenêtre informations peut être affichée à la fois.

L’exemple de code suivant montre le convertisseur personnalisé pour la plateforme Android :

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.Droid
{
    public class CustomMapRenderer : MapRenderer, GoogleMap.IInfoWindowAdapter
    {
        List<CustomPin> customPins;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                NativeMap.InfoWindowClick -= OnInfoWindowClick;
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                customPins = formsMap.CustomPins;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(GoogleMap map)
        {
            base.OnMapReady(map);

            NativeMap.InfoWindowClick += OnInfoWindowClick;
            NativeMap.SetInfoWindowAdapter(this);
        }
        ...
    }
}
```

Si le convertisseur personnalisé est attaché à un nouvel élément Xamarin.Forms, la `OnElementChanged` appels de méthode le `MapView.GetMapAsync` (méthode), qui obtient sous-jacent `GoogleMap` qui est lié à la vue. Une fois le `GoogleMap` instance n’est disponible, le `OnMapReady` remplacement sera appelé. Cette méthode inscrit un gestionnaire d’événements pour le `InfoWindowClick` événement qui se déclenche lorsque le [fenêtre informations sur le clic est effectué sur](#Clicking_on_the_Info_Window)et désabonné uniquement lorsque l’élément auquel le renderer est attaché change. Le `OnMapReady` remplacer également les appels le `SetInfoWindowAdapter` méthode pour spécifier que le `CustomMapRenderer` instance de la classe fournit les méthodes pour personnaliser la fenêtre info.

Le `CustomMapRenderer` la classe implémente le `GoogleMap.IInfoWindowAdapter` interface à [personnaliser la fenêtre info](#Customizing_the_Info_Window). Cette interface spécifie que les méthodes suivantes doivent être implémentées :

- `public Android.Views.View GetInfoWindow(Marker marker)` – Cette méthode est appelée pour retourner une fenêtre d’informations personnalisé pour un marqueur. Si elle retourne `null`, le rendu de fenêtre par défaut sera utilisée. Si elle retourne un `View`, puis qui `View` sont placés dans le cadre de la fenêtre d’informations.
- `public Android.Views.View GetInfoContents(Marker marker)` – Cette méthode est appelée pour retourner un `View` contenant le contenu de la fenêtre d’informations et est appelée uniquement si le `GetInfoWindow` retourne de la méthode `null`. Si elle retourne `null`, le rendu par défaut du contenu de la fenêtre info sera utilisée.

Dans l’exemple d’application, seul le contenu de la fenêtre d’informations personnalisé et donc la `GetInfoWindow` méthode retourne `null` pour activer cette option.

#### <a name="customizing-the-marker"></a>Personnalisation du marqueur

L’icône utilisée pour représenter un marqueur peut être personnalisée en appelant le `MarkerOptions.SetIcon` (méthode). Cela peut être effectué en substituant le `CreateMarker` (méthode), qui est appelée pour chaque `Pin` qui est ajoutée à la carte :

```csharp
protected override MarkerOptions CreateMarker(Pin pin)
{
    var marker = new MarkerOptions();
    marker.SetPosition(new LatLng(pin.Position.Latitude, pin.Position.Longitude));
    marker.SetTitle(pin.Label);
    marker.SetSnippet(pin.Address);
    marker.SetIcon(BitmapDescriptorFactory.FromResource(Resource.Drawable.pin));
    return marker;
}
```

Cette méthode crée un nouveau `MarkerOption` instance pour chaque `Pin` instance. Après avoir défini la position, une étiquette et une adresse du marqueur, son icône est défini avec la `SetIcon` (méthode). Cette méthode prend un `BitmapDescriptor` objet contenant les données nécessaires à l’affichage de l’icône, avec le `BitmapDescriptorFactory` classe fournissant des méthodes d’assistance pour simplifier la création de la `BitmapDescriptor`.

Pour plus d’informations sur l’utilisation de la `BitmapDescriptorFactory` classe pour personnaliser un marqueur, consultez [personnalisation d’un marqueur](~/android/platform/maps-and-location/maps/maps-api.md).

<a name="Customizing_the_Info_Window" />

#### <a name="customizing-the-info-window"></a>Personnalisation de la fenêtre Info

Lorsqu’un utilisateur appuie sur le marqueur, la `GetInfoContents` méthode est exécutée, à condition que le `GetInfoWindow` retourne de la méthode `null`. Le code suivant montre l’exemple le `GetInfoContents` méthode :

```csharp
public Android.Views.View GetInfoContents (Marker marker)
{
  var inflater = Android.App.Application.Context.GetSystemService (Context.LayoutInflaterService) as Android.Views.LayoutInflater;
  if (inflater != null) {
    Android.Views.View view;

    var customPin = GetCustomPin (marker);
    if (customPin == null) {
      throw new Exception ("Custom pin not found");
    }

    if (customPin.Id.ToString() == "Xamarin") {
      view = inflater.Inflate (Resource.Layout.XamarinMapInfoWindow, null);
    } else {
      view = inflater.Inflate (Resource.Layout.MapInfoWindow, null);
    }

    var infoTitle = view.FindViewById<TextView> (Resource.Id.InfoWindowTitle);
    var infoSubtitle = view.FindViewById<TextView> (Resource.Id.InfoWindowSubtitle);

    if (infoTitle != null) {
      infoTitle.Text = marker.Title;
    }
    if (infoSubtitle != null) {
      infoSubtitle.Text = marker.Snippet;
    }

    return view;
  }
  return null;
}
```

Cette méthode retourne un `View` contenant le contenu de la fenêtre info. Cela s’effectue comme suit :

- Un `LayoutInflater` instance est récupérée. Cela est utilisé pour instancier un fichier XML de disposition dans son `View`.
- Le `GetCustomPin` méthode est appelée pour retourner les données de code confidentiel personnalisé pour la fenêtre info.
- Le `XamarinMapInfoWindow` mise en page est agrandie si le `CustomPin.Id` propriété est égale à `Xamarin`. Sinon, le `MapInfoWindow` mise en page est agrandie. Ainsi, pour les scénarios où les dispositions de fenêtres des informations différentes peuvent être affichées pour différents marqueurs.
- Le `InfoWindowTitle` et `InfoWindowSubtitle` les ressources sont récupérées à partir de la mise en page gonflé, ainsi que leurs `Text` propriétés sont définies sur les données correspondantes à partir de la `Marker` instance, la condition que les ressources ne sont pas `null`.
- Le `View` instance est retournée pour l’affichage sur la carte.

> [!NOTE]
> Une fenêtre d’info n’est pas actif `View`. Au lieu de cela, Android convertira le `View` à statique bitmap et de les afficher en tant qu’image. Cela signifie que, lors d’une fenêtre d’info, peut répondre à un événement click, il ne peut pas répondre aux événements tactiles ni de mouvements et des contrôles individuels dans la fenêtre info ne peut pas répondre à leurs propres événements de clic.

<a name="Clicking_on_the_Info_Window" />

#### <a name="clicking-on-the-info-window"></a>En cliquant sur la fenêtre Info

Lorsque l’utilisateur clique sur la fenêtre d’informations, le `InfoWindowClick` se déclenche des événements, qui à son tour s’exécute le `OnInfoWindowClick` méthode :

```csharp
void OnInfoWindowClick (object sender, GoogleMap.InfoWindowClickEventArgs e)
{
  var customPin = GetCustomPin (e.Marker);
  if (customPin == null) {
    throw new Exception ("Custom pin not found");
  }

  if (!string.IsNullOrWhiteSpace (customPin.Url)) {
    var url = Android.Net.Uri.Parse (customPin.Url);
    var intent = new Intent (Intent.ActionView, url);
    intent.AddFlags (ActivityFlags.NewTask);
    Android.App.Application.Context.StartActivity (intent);
  }
}
```

Cette méthode ouvre un navigateur web et accède à l’adresse stockée dans le `Url` propriété récupérée `CustomPin` de l’instance pour le `Marker`. Notez que l’adresse a été défini lors de la création du `CustomPin` collection dans le projet de bibliothèque .NET Standard.

Pour plus d’informations sur la personnalisation d’un `MapView` une instance, consultez [API cartes](~/android/platform/maps-and-location/maps/maps-api.md).

### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Création du convertisseur personnalisé sur la plateforme Windows universelle

Les captures d’écran suivantes montrent le mappage, avant et après la personnalisation :

![](customized-pin-images/map-layout-uwp.png "Contrôle de carte avant et après la personnalisation")

Sur UWP le code pin est appelé un *icône Explorateur*, et peut être une image personnalisée ou l’image par défaut définie par le système. Une icône de carte peut représenter un `UserControl`, qui s’affiche en réponse à l’utilisateur en appuyant sur l’icône de carte. Le `UserControl` du contenu, y compris le `Label` et `Address` propriétés de la `Pin` instance.

L’exemple de code suivant montre le convertisseur personnalisé UWP :

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
        MapControl nativeMap;
        List<CustomPin> customPins;
        XamarinMapOverlay mapOverlay;
        bool xamarinOverlayShown = false;

        protected override void OnElementChanged(ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                nativeMap.MapElementClick -= OnMapElementClick;
                nativeMap.Children.Clear();
                mapOverlay = null;
                nativeMap = null;
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                nativeMap = Control as MapControl;
                customPins = formsMap.CustomPins;

                nativeMap.Children.Clear();
                nativeMap.MapElementClick += OnMapElementClick;

                foreach (var pin in customPins)
                {
                    var snPosition = new BasicGeoposition { Latitude = pin.Pin.Position.Latitude, Longitude = pin.Pin.Position.Longitude };
                    var snPoint = new Geopoint(snPosition);

                    var mapIcon = new MapIcon();
                    mapIcon.Image = RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///pin.png"));
                    mapIcon.CollisionBehaviorDesired = MapElementCollisionBehavior.RemainVisible;
                    mapIcon.Location = snPoint;
                    mapIcon.NormalizedAnchorPoint = new Windows.Foundation.Point(0.5, 1.0);

                    nativeMap.MapElements.Add(mapIcon);                    
                }
            }
        }
        ...
    }
}
```

Le `OnElementChanged` méthode effectue les opérations suivantes, si le convertisseur personnalisé est attaché à un nouvel élément Xamarin.Forms :

- Elle efface le `MapControl.Children` collection à supprimer les éléments d’interface utilisateur existants à partir de la carte, avant d’inscrire un gestionnaire d’événements pour le `MapElementClick` événement. Cet événement se déclenche lorsque l’utilisateur appuie ou clique sur un `MapElement` sur la `MapControl`et désabonné uniquement lorsque l’élément auquel le renderer est attaché change.
- Chaque code confidentiel dans la `customPins` ensemble est affiché à l’emplacement géographique approprié sur la carte, comme suit :
  - L’emplacement pour le code confidentiel est créé comme un `Geopoint` instance.
  - Un `MapIcon` instance est créée pour représenter le code confidentiel.
  - L’image utilisée pour représenter le `MapIcon` est spécifié en définissant le `MapIcon.Image` propriété. Toutefois, image de l’icône de la carte n'est pas toujours garanti à afficher, comme il peut être masqué par d’autres éléments sur la carte. Par conséquent, l’icône de carte `CollisionBehaviorDesired` propriété est définie sur `MapElementCollisionBehavior.RemainVisible`pour vous assurer qu’il reste visible.
  - L’emplacement de la `MapIcon` est spécifié en définissant le `MapIcon.Location` propriété.
  - Le `MapIcon.NormalizedAnchorPoint` propriété est définie sur l’emplacement approximatif du pointeur sur l’image. Si cette propriété conserve sa valeur par défaut (0,0), qui représente le coin supérieur gauche de l’image, les modifications dans le niveau de zoom de la carte peuvent entraîner l’image qui pointe vers un autre emplacement.
  - Le `MapIcon` instance est ajoutée à la `MapControl.MapElements` collection. Ainsi, l’icône de carte affiché sur le `MapControl`.

> [!NOTE]
> Lorsque vous utilisez la même image pour plusieurs icônes de mappage, le `RandomAccessStreamReference` instance doit être déclarée au niveau de la page ou d’application pour de meilleures performances.

#### <a name="displaying-the-usercontrol"></a>Afficher le UserControl

Lorsqu’un utilisateur appuie sur l’icône de la carte, le `OnMapElementClick` méthode est exécutée. L’exemple de code suivant illustre cette méthode :

```csharp
private void OnMapElementClick(MapControl sender, MapElementClickEventArgs args)
{
    var mapIcon = args.MapElements.FirstOrDefault(x => x is MapIcon) as MapIcon;
    if (mapIcon != null)
    {
        if (!xamarinOverlayShown)
        {
            var customPin = GetCustomPin(mapIcon.Location.Position);
            if (customPin == null)
            {
                throw new Exception("Custom pin not found");
            }

            if (customPin.Id.ToString() == "Xamarin")
            {
                if (mapOverlay == null)
                {
                    mapOverlay = new XamarinMapOverlay(customPin);
                }

                var snPosition = new BasicGeoposition { Latitude = customPin.Pin.Position.Latitude, Longitude = customPin.Pin.Position.Longitude };
                var snPoint = new Geopoint(snPosition);

                nativeMap.Children.Add(mapOverlay);
                MapControl.SetLocation(mapOverlay, snPoint);
                MapControl.SetNormalizedAnchorPoint(mapOverlay, new Windows.Foundation.Point(0.5, 1.0));
                xamarinOverlayShown = true;
            }
        }
        else
        {
            nativeMap.Children.Remove(mapOverlay);
            xamarinOverlayShown = false;
        }
    }
}
```

Cette méthode crée un `UserControl` instance qui affiche des informations sur le code confidentiel. Cela s’effectue comme suit :

- Le `MapIcon` instance est récupérée.
- Le `GetCustomPin` méthode est appelée pour retourner les données de code confidentiel personnalisé qui seront affiche.
- Un `XamarinMapOverlay` instance est créée pour afficher les données personnalisées de code confidentiel. Cette classe est un contrôle utilisateur.
- L’emplacement géographique où afficher la `XamarinMapOverlay` d’instance sur le `MapControl` est créé en tant qu’un `Geopoint` instance.
- Le `XamarinMapOverlay` instance est ajoutée à la `MapControl.Children` collection. Cette collection contient des éléments d’interface utilisateur XAML qui seront affichera sur la carte.
- L’emplacement géographique de la `XamarinMapOverlay` instance sur la carte est définie en appelant le `SetLocation` (méthode).
- L’emplacement relatif sur le `XamarinMapOverlay` instance, ce qui correspond à l’emplacement spécifié, est définie en appelant le `SetNormalizedAnchorPoint` (méthode). Cela garantit que les modifications dans le niveau de zoom du résultat de mappage dans le `XamarinMapOverlay` toujours affiché à l’emplacement correct de l’instance.

Vous pouvez également, si plus d’informations sur le code confidentiel sont déjà affichées sur la carte, appuyant sur la carte supprime le `XamarinMapOverlay` à partir de l’instance la `MapControl.Children` collection.

#### <a name="tapping-on-the-information-button"></a>En appuyant sur le bouton informations

Lorsque l’utilisateur appuie sur le *informations* situé dans le `XamarinMapOverlay` contrôle utilisateur, le `Tapped` se déclenche des événements, qui à son tour s’exécute le `OnInfoButtonTapped` méthode :

```csharp
private async void OnInfoButtonTapped(object sender, TappedRoutedEventArgs e)
{
    await Launcher.LaunchUriAsync(new Uri(customPin.Url));
}
```

Cette méthode ouvre un navigateur web et accède à l’adresse stockée dans le `Url` propriété de la `CustomPin` instance. Notez que l’adresse a été défini lors de la création du `CustomPin` collection dans le projet de bibliothèque .NET Standard.

Pour plus d’informations sur la personnalisation d’un `MapControl` une instance, consultez [cartes et vue d’ensemble de l’emplacement](https://msdn.microsoft.com/library/windows/apps/mt219699.aspx) sur MSDN.

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment créer un convertisseur personnalisé pour le `Map` (contrôle), permettant aux développeurs de substituer le rendu natif par défaut avec leur propres personnalisation spécifique à la plateforme. Xamarin.Forms.Maps fournit une abstraction d’inter-plateformes pour l’affichage des mappages qui utilisent le mappage natif expérience API sur chaque plateforme pour fournir une carte rapide et familier pour les utilisateurs.


## <a name="related-links"></a>Liens associés

- [Contrôle Maps](~/xamarin-forms/user-interface/map.md)
- [Mappages d’iOS](~/ios/user-interface/controls/ios-maps/index.md)
- [API Cartes](~/android/platform/maps-and-location/maps/maps-api.md)
- [Code confidentiel personnalisé (exemple)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/pin/)
