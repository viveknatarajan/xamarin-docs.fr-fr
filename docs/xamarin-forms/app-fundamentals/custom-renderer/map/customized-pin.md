---
title: "Personnalisation d’un code confidentiel de carte"
description: "Cet article explique comment créer un convertisseur personnalisé pour le contrôle de carte qui affiche une carte native avec un code confidentiel personnalisé et une vue personnalisée des données de code confidentiel sur chaque plateforme."
ms.topic: article
ms.prod: xamarin
ms.assetid: C5481D86-80E9-4E3D-9FB6-57B0F93711A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 91afe9b627d9d430fba38b34f915d7e79652a2d3
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="customizing-a-map-pin"></a>Personnalisation d’un code confidentiel de carte

_Cet article explique comment créer un convertisseur personnalisé pour le contrôle de carte qui affiche une carte native avec un code confidentiel personnalisé et une vue personnalisée des données de code confidentiel sur chaque plateforme._

Chaque vue Xamarin.Forms a un convertisseur qui l’accompagne pour chaque plateforme qui crée une instance d’un contrôle natif. Lorsqu’un [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) est restitué par une application Xamarin.Forms dans iOS, le `MapRenderer` classe est instanciée, qui instancie ensuite natif `MKMapView` contrôle. Sur la plateforme Android, le `MapRenderer` classe instancie natif `MapView` contrôle. Sur la plate-forme de Windows universelle (UWP), le `MapRenderer` classe instancie natif `MapControl`. Pour plus d’informations sur les classes de contrôle natif correspondant aux contrôles de Xamarin.Forms et le convertisseur, consultez [convertisseur des Classes de Base et des contrôles natifs](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Le diagramme suivant illustre la relation entre la [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) et les contrôles natifs correspondants qui l’implémentent :

![](customized-pin-images/map-classes.png "Relation entre le contrôle de carte et l’implémentation des contrôles natifs")

Le processus de rendu peut être utilisé pour implémenter les personnalisations spécifiques à une plateforme en créant un convertisseur personnalisé pour un [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) sur chaque plateforme. Le processus pour cette opération est la suivante :

1. [Créer](#Creating_the_Custom_Map) une carte personnalisée Xamarin.Forms.
1. [Consommer](#Consuming_the_Custom_Map) la carte personnalisée à partir de Xamarin.Forms.
1. [Créer](#Creating_the_Custom_Renderer_on_each_Platform) le convertisseur personnalisé pour la carte sur chaque plateforme.

Chaque élément présent nous reviendrons à son tour, pour implémenter un `CustomMap` convertisseur qui affiche une carte native avec un code confidentiel personnalisé et une vue personnalisée des données de code confidentiel sur chaque plateforme.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) doit être initialisé et configuré avant des utiliser. Pour plus d’informations, consultez [ `Maps Control` ](~/xamarin-forms/user-interface/map.md).

<a name="Creating_the_Custom_Map" />

## <a name="creating-the-custom-map"></a>Création de la carte personnalisée

Un contrôle de carte personnalisée peut être créé en sous-classant le [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) de classe, comme indiqué dans l’exemple de code suivant :

```csharp
public class CustomMap : Map
{
  public List<CustomPin> CustomPins { get; set; }
}
```

Le `CustomMap` contrôle est créé dans le projet de bibliothèque (PCL) de classes portable et définit l’API pour la carte personnalisée. La carte personnalisée expose le `CustomPins` propriété qui représente la collection de `CustomPin` les objets qui seront affichés par le contrôle de carte natif sur chaque plateforme. La `CustomPin` classe est illustrée dans l’exemple de code suivant :

```csharp
public class CustomPin : Pin
{
  public string Id { get; set; }
  public string Url { get; set; }
}
```

Cette classe définit un `CustomPin` comme héritant des propriétés de la [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/) classe et l’ajout de `Id`, et `Url` propriétés.

<a name="Consuming_the_Custom_Map" />

## <a name="consuming-the-custom-map"></a>Utilisation de la carte personnalisée

Le `CustomMap` contrôle qui peut être référencée dans XAML dans le projet de bibliothèque de classes portables par déclarer un espace de noms pour son emplacement et à l’aide du préfixe d’espace de noms sur le contrôle de carte personnalisée. Le code suivant exemple montre comment la `CustomMap` contrôle peut être consommée par une page XAML :

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

Le `local` préfixe d’espace de noms peut être n’importe quel nom. Toutefois, le `clr-namespace` et `assembly` valeurs doivent correspondre les détails de la carte personnalisée. Une fois que l’espace de noms est déclaré, le préfixe est utilisé pour faire référence à la carte personnalisée.

Le code suivant montre d’exemple comment la `CustomMap` contrôle peut être consommée par une page c# :

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

Le `CustomMap` instance sera utilisée pour afficher la carte native sur chaque plateforme. Il a [ `MapType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.MapType/) propriété définit le style d’affichage de la [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/), avec les valeurs possibles qui est définis dans le [ `MapType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapType/) énumération. Pour iOS et Android, la largeur et la hauteur de la carte est définie via les propriétés de la `App` classe sont initialisés dans les projets spécifiques à la plateforme.

L’emplacement de la carte, ainsi que les codes confidentiels qu’il contient, sont initialisées comme indiqué dans l’exemple de code suivant :

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

Cette initialisation ajoute un code confidentiel personnalisé et place la vue de la carte avec le [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) (méthode), ce qui modifie la position et le niveau de zoom de la carte en créant un [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) à partir d’un [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) et un [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/).

Un convertisseur personnalisé peut maintenant être ajouté à chaque projet d’application pour personnaliser les contrôles de mappage natif.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Création du convertisseur personnalisé sur chaque plateforme

Le processus de création de la classe de convertisseur personnalisé est comme suit :

1. Créer une sous-classe de la `MapRenderer` classe qui restitue la carte personnalisée.
1. Remplacer la `OnElementChanged` méthode qui restitue la carte personnalisée et l’écriture de logique pour la personnaliser. Cette méthode est appelée lorsque la carte personnalisée Xamarin.Forms correspondante est créée.
1. Ajouter un `ExportRenderer` d’attribut à la classe de convertisseur personnalisé pour spécifier qu’il sera utilisé pour restituer la carte personnalisée Xamarin.Forms. Cet attribut est utilisé pour inscrire le convertisseur personnalisé avec Xamarin.Forms.

> [!NOTE]
> **Remarque**: il est facultatif fournir un convertisseur personnalisé dans chaque projet de plateforme. Si un convertisseur personnalisé n’est pas inscrit, puis le convertisseur par défaut pour la classe de base du contrôle sera utilisé.

Le diagramme suivant illustre les responsabilités de chaque projet dans l’exemple d’application, ainsi que les relations entre eux :

![](customized-pin-images/solution-structure.png "Responsabilités de projet convertisseur CustomMap personnalisé")

Le `CustomMap` contrôle est restitué par les classes de rendu spécifique à la plateforme, qui dérivent de la `MapRenderer` classe pour chaque plateforme. Ainsi, chaque `CustomMap` contrôler le rendu avec des contrôles spécifiques à la plateforme, comme indiqué dans les captures d’écran suivantes :

![](customized-pin-images/screenshots.png "CustomMap sur chaque plateforme")

Le `MapRenderer` classe expose le `OnElementChanged` (méthode), qui est appelé lors de la création de la carte personnalisée Xamarin.Forms pour restituer le contrôle natif correspondant. Cette méthode prend un `ElementChangedEventArgs` paramètre contienne `OldElement` et `NewElement` propriétés. Ces propriétés représentent l’élément Xamarin.Forms que le convertisseur *a été* associée et l’élément Xamarin.Forms que le convertisseur *est* attaché, respectivement. Dans l’exemple d’application le `OldElement` propriété sera `null` et `NewElement` propriété contient une référence à la `CustomMap` instance.

Une version substituée de la `OnElementChanged` , dans chaque classe de convertisseur de plateforme spécifique, est l’endroit pour effectuer la personnalisation du contrôle natif. Une référence typée au contrôle natif utilisé sur la plateforme est accessible via la `Control` propriété. En outre, une référence au contrôle qui est rendue Xamarin.Forms peut être obtenue via le `Element` propriété.

Soyez prudent lors de l’abonnement aux gestionnaires d’événements dans le `OnElementChanged` méthode, comme illustré dans l’exemple de code suivant :

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

Le contrôle natif doit être configuré et gestionnaires d’événements abonnement uniquement lorsque le convertisseur personnalisé est attaché à un nouvel élément de Xamarin.Forms. De même, les gestionnaires d’événements auquel les clients ont été doivent être annulés uniquement lorsque le convertisseur est attaché à l’élément est modifié. Adoptant cette approche vous aidera à créer un convertisseur personnalisé qui ne connaît pas les fuites de mémoire.

Chaque classe de convertisseur personnalisé est décorée avec un `ExportRenderer` attribut qui inscrit le convertisseur avec Xamarin.Forms. L’attribut accepte deux paramètres : le nom de type du contrôle personnalisé Xamarin.Forms rendu et le nom de type du convertisseur personnalisé. Le `assembly` préfixe à l’attribut spécifie que l’attribut s’applique à la totalité de l’assembly.

Les sections suivantes traitent de l’implémentation de chaque classe de convertisseur de personnalisé spécifique à la plateforme.

### <a name="creating-the-custom-renderer-on-ios"></a>Création du convertisseur personnalisé sur iOS

Les captures d’écran suivantes montrent le mappage, avant et après personnalisation :

![](customized-pin-images/map-layout-ios.png "Contrôle de carte avant et après personnalisation")

Sur iOS, le code confidentiel est appelé un *annotation*, et peut être une image personnalisée ou un code confidentiel de définies par le système de couleurs différentes. Les annotations peuvent éventuellement apparaître un *légende*, qui s’affiche en réponse à l’utilisateur en sélectionnant l’annotation. La légende affiche les `Label` et `Address` propriétés de la `Pin` instance, avec facultatif gauche et droite accessoires vues. Dans la capture d’écran ci-dessus, la vue accessoire de gauche est l’image d’un singe, la vue de droite accessoire étant le *informations* bouton.

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

Le `OnElementChanged` méthode effectue la configuration suivante de la [ `MKMapView` ](https://developer.xamarin.com/api/type/MapKit.MKMapView/) de l’instance, sous réserve que le convertisseur personnalisé est attaché à un nouvel élément Xamarin.Forms :

- Le [ `GetViewForAnnotation` ](https://developer.xamarin.com/api/property/MapKit.MKMapView.GetViewForAnnotation/) est définie sur la `GetViewForAnnotation` (méthode). Cette méthode est appelée lorsque le [l’emplacement de l’annotation est visible sur la carte](#Displaying_the_Annotation)et est utilisé pour personnaliser l’avant de l’annotation à afficher.
- Gestionnaires d’événements pour le `CalloutAccessoryControlTapped`, `DidSelectAnnotationView`, et `DidDeselectAnnotationView` événements sont enregistrés. Ces événements déclenchent lorsque l’utilisateur [appuie sur l’accesseur à droite de la légende](#Tapping_on_the_Right_Callout_Accessory_View)et lorsque l’utilisateur [sélectionne](#Selecting_the_Annotation) et [désélectionne](#Deselecting_the_Annotation) l’annotation, respectivement. Les événements sont annulés uniquement lorsque l’élément le convertisseur est attaché aux modifications.

<a name="Displaying_the_Annotation" />

#### <a name="displaying-the-annotation"></a>Affichage de l’Annotation

Le `GetViewForAnnotation` méthode est appelée lorsque l’emplacement de l’annotation est visible sur la carte et est utilisé pour personnaliser l’avant de l’annotation à afficher. Une annotation comporte deux parties :

- `MkAnnotation` – inclut le titre, le sous-titre et l’emplacement de l’annotation.
- `MkAnnotationView` : contient l’image pour représenter l’annotation et éventuellement une légende qui est affichée lorsque l’utilisateur appuie sur l’annotation.

Le `GetViewForAnnotation` méthode accepte un `IMKAnnotation` qui contient les données de l’annotation et retourne un `MKAnnotationView` pour l’affichage sur la carte et est indiqué dans l’exemple de code suivant :

```csharp
MKAnnotationView GetViewForAnnotation(MKMapView mapView, IMKAnnotation annotation)
{
    MKAnnotationView annotationView = null;

    if (annotation is MKUserLocation)
        return null;

    var customPin = GetCustomPin(annotation as MKPointAnnotation);
    if (customPin == null) {
        throw new Exception("Custom pin not found");
    }

    annotationView = mapView.DequeueReusableAnnotation(customPin.Id);
    if (annotationView == null) {
        annotationView = new CustomMKAnnotationView(annotation, customPin.Id);
        annotationView.Image = UIImage.FromFile("pin.png");
        annotationView.CalloutOffset = new CGPoint(0, 0);
        annotationView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile("monkey.png"));
        annotationView.RightCalloutAccessoryView = UIButton.FromType(UIButtonType.DetailDisclosure);
        ((CustomMKAnnotationView)annotationView).Id = customPin.Id;
        ((CustomMKAnnotationView)annotationView).Url = customPin.Url;
    }
    annotationView.CanShowCallout = true;

    return annotationView;
}
```

Cette méthode garantit que l’annotation s’affichera sous la forme d’une image personnalisée, plutôt que comme défini par le système de code confidentiel et que lorsque l’annotation est cliquée une légende s’affiche qui inclut le contenu supplémentaire vers la gauche et droit du titre de l’annotation adresse . Pour cela, comme suit :

1. Le `GetCustomPin` méthode est appelée pour retourner les données personnalisées de code confidentiel pour l’annotation.
1. Pour économiser la mémoire, vue de l’annotation est regroupée à réutiliser avec l’appel à [ `DequeueReusableAnnotation` ](https://developer.xamarin.com/api/member/MapKit.MKMapView.DequeueReusableAnnotation/(System.String)/).
1. Le `CustomMKAnnotationView` classe étend la `MKAnnotationView` classe avec `Id` et `Url` propriétés qui correspondent à des propriétés identiques dans les `CustomPin` instance. Une nouvelle instance de la `CustomMKAnnotationView` est créé, à condition que l’annotation est `null`:
  - Le `CustomMKAnnotationView.Image` est définie sur l’image qui représentera l’annotation sur la carte.
  - Le `CustomMKAnnotationView.CalloutOffset` est définie sur une `CGPoint` qui spécifie que la légende est centrée au-dessus de l’annotation.
  - Le `CustomMKAnnotationView.LeftCalloutAccessoryView` est définie sur une image d’un singe qui apparaît à gauche du titre de l’annotation et de l’adresse.
  - Le `CustomMKAnnotationView.RightCalloutAccessoryView` est définie sur une *informations* bouton qui apparaît à droite du titre de l’annotation et de l’adresse.
  - Le `CustomMKAnnotationView.Id` est définie sur le `CustomPin.Id` propriété retournée par le `GetCustomPin` (méthode). Cela permet l’annotation être identifié afin qu’il puisse [légende peut être personnalisé](#Selecting_the_Annotation), si vous le souhaitez.
  - Le `CustomMKAnnotationView.Url` est définie sur le `CustomPin.Url` propriété retournée par le `GetCustomPin` (méthode). L’URL sera utilisée lorsque l’utilisateur [appuie sur le bouton affiché dans la vue accessoire légende droit](#Tapping_on_the_Right_Callout_Accessory_View).
1. Le [ `MKAnnotationView.CanShowCallout` ](https://developer.xamarin.com/api/property/MapKit.MKAnnotationView.CanShowCallout/) est définie sur `true` afin que la légende s’affiche lorsque l’annotation est cliquée.
1. L’annotation est retournée pour l’affichage sur la carte.

<a name="Selecting_the_Annotation" />

#### <a name="selecting-the-annotation"></a>Sélection de l’Annotation

Lorsque l’utilisateur appuie sur l’annotation, le `DidSelectAnnotationView` se déclenche des événements, qui à son tour exécute la `OnDidSelectAnnotationView` méthode :

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

Cette méthode étend la légende existante (qui contient les vues gauche et droite accessoires) en ajoutant un `UIView` instance qui contient une image du logo Xamarin, à condition que l’annotation sélectionnée son `Id` lavaleurdepropriété`Xamarin`. Ainsi, pour les scénarios où des légendes différentes peuvent être affichés pour les différentes annotations. Le `UIView` instance apparaît centré au-dessus de la légende existante.

<a name="Tapping_on_the_Right_Callout_Accessory_View" />

#### <a name="tapping-on-the-right-callout-accessory-view"></a>Pression sur la vue de droite accessoire de légende

Lorsque l’utilisateur appuie sur le *informations* bouton dans la vue accessoire légende à droite, la `CalloutAccessoryControlTapped` se déclenche des événements, qui à son tour exécute le `OnCalloutAccessoryControlTapped` (méthode) :

```csharp
void OnCalloutAccessoryControlTapped (object sender, MKMapViewAccessoryTappedEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  if (!string.IsNullOrWhiteSpace (customView.Url)) {
    UIApplication.SharedApplication.OpenUrl (new Foundation.NSUrl (customView.Url));
  }
}
```

Cette méthode ouvre un navigateur web et accède à l’adresse stockée dans le `CustomMKAnnotationView.Url` propriété. Notez que l’adresse a été défini lors de la création du `CustomPin` collection dans le projet de bibliothèque de classes portables.

<a name="Deselecting_the_Annotation" />

#### <a name="deselecting-the-annotation"></a>En désactivant l’Annotation

Lorsque l’annotation s’affiche et l’utilisateur appuie sur la carte, le `DidDeselectAnnotationView` se déclenche des événements, qui à son tour exécute la `OnDidDeselectAnnotationView` méthode :

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

Cette méthode garantit que lorsque la légende existante n’est pas sélectionnée, la partie étendue de la légende (l’image du logo Xamarin) s’arrête également affiché et ses ressources seront libérées.

Pour plus d’informations sur la personnalisation d’un `MKMapView` une instance, consultez [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md).

### <a name="creating-the-custom-renderer-on-android"></a>Création du convertisseur personnalisé sur Android

Les captures d’écran suivantes montrent le mappage, avant et après personnalisation :

![](customized-pin-images/map-layout-android.png "Contrôle de carte avant et après personnalisation")

Sur Android, le code confidentiel est appelé un *marqueur*, et peut être une image personnalisée ou un marqueur défini par le système de couleurs différentes. Marqueurs peuvent afficher un *fenêtre info*, qui s’affiche en réponse à l’utilisateur en appuyant sur le marqueur. La fenêtre d’informations affiche la `Label` et `Address` propriétés de la `Pin` d’instance et peut être personnalisé pour inclure d’autres contenus. Toutefois, une seule fenêtre d’informations peut être affichée à la fois.

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

Condition que le convertisseur personnalisé est attaché à un nouvel élément de Xamarin.Forms, le `OnElementChanged` les appels de méthode le `MapView.GetMapAsync` (méthode), qui obtient sous-jacent `GoogleMap` qui est lié à la vue. Une fois la `GoogleMap` instance n’est disponible, le `OnMapReady` remplacement sera appelé. Cette méthode inscrit un gestionnaire d’événements pour le `InfoWindowClick` événement qui se déclenche lorsque la [vous cliquez sur fenêtre info](#Clicking_on_the_Info_Window)et désinscrit uniquement lorsque l’élément le convertisseur est attaché aux modifications. Le `OnMapReady` remplacer également les appels le `SetInfoWindowAdapter` méthode pour spécifier que le `CustomMapRenderer` instance de la classe fournit les méthodes permettant de personnaliser la fenêtre d’informations.

Le `CustomMapRenderer` la classe implémente le `GoogleMap.IInfoWindowAdapter` interface [personnaliser la fenêtre d’info](#Customizing_the_Info_Window). Cette interface spécifie que les méthodes suivantes doivent être implémentées :

- `public Android.Views.View GetInfoWindow(Marker marker)` – Cette méthode est appelée pour retourner une fenêtre d’informations personnalisé pour un marqueur. Si elle retourne `null`, le rendu de fenêtre par défaut sera utilisée. Si elle retourne un `View`, puis qui `View` sont placés dans le frame de fenêtre d’informations.
- `public Android.Views.View GetInfoContents(Marker marker)` – Cette méthode est appelée pour retourner un `View` contenant le contenu de la fenêtre d’information et est appelée uniquement si la `GetInfoWindow` méthode renvoie `null`. Si elle retourne `null`, le rendu par défaut, l’informations du contenu de fenêtre sera utilisée.

Dans l’exemple d’application uniquement le contenu de la fenêtre info personnalisé et donc la `GetInfoWindow` retourne de la méthode `null` pour activer cette option.

#### <a name="customizing-the-marker"></a>Personnalisation de la marque

L’icône utilisée pour représenter un marqueur peut être personnalisé en appelant le `MarkerOptions.SetIcon` (méthode). Cela peut être accompli en substituant le `CreateMarker` (méthode), qui est appelé pour chaque `Pin` qui est ajoutée à la carte :

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

Cette méthode crée un nouveau `MarkerOption` instance pour chaque `Pin` instance. Après avoir défini la position, une étiquette et une adresse de la marque, son icône est définie avec la `SetIcon` (méthode). Cette méthode prend un `BitmapDescriptor` objet contenant les données nécessaires pour effectuer le rendu avec l’icône, le `BitmapDescriptorFactory` classe fournissant des méthodes d’assistance pour simplifier la création de la `BitmapDescriptor`.

Pour plus d’informations sur l’utilisation de la `BitmapDescriptorFactory` classe pour personnaliser un marqueur, consultez [personnalisation d’un marqueur](~/android/platform/maps-and-location/maps/maps-api.md).

<a name="Customizing_the_Info_Window" />

#### <a name="customizing-the-info-window"></a>Personnalisation de la fenêtre d’informations

Lorsqu’un utilisateur clique sur le marqueur, la `GetInfoContents` méthode est exécutée, à condition que la `GetInfoWindow` méthode renvoie `null`. Le code suivant exemple illustre la `GetInfoContents` méthode :

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

    if (customPin.Id == "Xamarin") {
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

Cette méthode retourne un `View` contenant le contenu de la fenêtre d’information. Pour cela, comme suit :

- A `LayoutInflater` instance est récupérée. Cela est utilisé pour instancier un fichier XML de mise en page dans son `View`.
- Le `GetCustomPin` méthode est appelée pour retourner les données de code confidentiel personnalisé pour la fenêtre d’information.
- Le `XamarinMapInfoWindow` mise en page est agrandie si le `CustomPin.Id` propriété est égale à `Xamarin`. Dans le cas contraire, le `MapInfoWindow` mise en page est agrandie. Ainsi, pour les scénarios où les dispositions de fenêtres des informations différentes peuvent être affichées pour les différents marqueurs.
- Le `InfoWindowTitle` et `InfoWindowSubtitle` ressources sont récupérés à partir de la mise en page augmenté et leur `Text` propriétés sont définies sur les données correspondantes à partir de la `Marker` de l’instance, sous réserve que les ressources ne sont pas `null`.
- Le `View` instance est retournée pour l’affichage sur la carte.

> [!NOTE]
> Une fenêtre d’informations n’est pas un live `View`. Au lieu de cela, Android convertira la `View` un statique bitmap et qui affiche en tant qu’image. Cela signifie que, lors d’une fenêtre d’informations peut répondre à un événement click, il ne peut pas répondre à des événements tactiles ou les mouvements et des contrôles individuels dans la fenêtre d’information ne peut pas répondre à leurs propres événements de clic.

<a name="Clicking_on_the_Info_Window" />

#### <a name="clicking-on-the-info-window"></a>En cliquant sur la fenêtre d’information

Lorsque l’utilisateur clique sur la fenêtre d’information, la `InfoWindowClick` se déclenche des événements, qui à son tour exécute la `OnInfoWindowClick` méthode :

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

Cette méthode ouvre un navigateur web et accède à l’adresse stockée dans le `Url` propriété extraites `CustomPin` d’instance pour le `Marker`. Notez que l’adresse a été défini lors de la création du `CustomPin` collection dans le projet de bibliothèque de classes portables.

Pour plus d’informations sur la personnalisation d’un `MapView` une instance, consultez [API Maps](~/android/platform/maps-and-location/maps/maps-api.md).

### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Création du convertisseur personnalisé sur la plateforme Windows universelle

Les captures d’écran suivantes montrent le mappage, avant et après personnalisation :

![](customized-pin-images/map-layout-uwp.png "Contrôle de carte avant et après personnalisation")

Sur la plateforme Windows universelle, le code confidentiel est appelé un *icône Explorateur*, et peut être une image personnalisée ou l’image par défaut définie par le système. Une icône de carte peut représenter un `UserControl`, qui s’affiche en réponse à l’utilisateur en appuyant sur l’icône de carte. Le `UserControl` du contenu, y compris le `Label` et `Address` propriétés de la `Pin` instance.

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

Le `OnElementChanged` méthode effectue les opérations suivantes, sous réserve que le convertisseur personnalisé est attaché à un nouvel élément Xamarin.Forms :

- Elle efface le `MapControl.Children` collection à supprimer les éléments d’interface utilisateur existants à partir de la carte, avant d’inscrire un gestionnaire d’événements pour le `MapElementClick` événement. Cet événement se déclenche lorsque l’utilisateur appuie ou clique sur un `MapElement` sur la `MapControl`et est désinscrit d’uniquement lorsque l’élément le convertisseur est attaché aux modifications.
- Chaque code confidentiel dans le `customPins` collection s’affiche à l’emplacement géographique correct sur la carte, comme suit :
  - L’emplacement pour le code confidentiel est créé comme un `Geopoint` instance.
  - A `MapIcon` instance est créée pour représenter le code confidentiel.
  - L’image utilisée pour représenter le `MapIcon` est spécifié en définissant le `MapIcon.Image` propriété. Toutefois, les images de l’icône de carte ne sont pas toujours certain qu’à afficher, comme il peut être masqué par d’autres éléments sur la carte. Par conséquent, l’icône de carte `CollisionBehaviorDesired` est définie sur `MapElementCollisionBehavior.RemainVisible`pour vous assurer qu’il reste visible.
  - L’emplacement de la `MapIcon` est spécifié en définissant le `MapIcon.Location` propriété.
  - Le `MapIcon.NormalizedAnchorPoint` est définie sur l’emplacement approximatif du pointeur sur l’image. Si cette propriété conserve sa valeur par défaut de (0,0), qui représente le coin supérieur gauche de l’image, les modifications dans le niveau de zoom de la carte peuvent entraîner l’image qui pointe vers un autre emplacement.
  - Le `MapIcon` instance est ajoutée à la `MapControl.MapElements` collection. Ainsi, l’icône de carte affichée sur le `MapControl`.

> [!NOTE]
> Lors de l’utilisation de la même image pour plusieurs icônes de mappage, le `RandomAccessStreamReference` instance doit être déclarée au niveau de la page ou d’une application pour de meilleures performances.

#### <a name="displaying-the-usercontrol"></a>Afficher le UserControl

Lorsqu’un utilisateur clique sur l’icône de la carte, le `OnMapElementClick` méthode est exécutée. L’exemple de code suivant illustre cette méthode :

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

            if (customPin.Id == "Xamarin")
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

Cette méthode crée un `UserControl` instance qui affiche des informations sur le code confidentiel. Pour cela, comme suit :

- Le `MapIcon` instance est récupérée.
- Le `GetCustomPin` méthode est appelée pour retourner les données de code confidentiel personnalisé qui seront affiche.
- A `XamarinMapOverlay` instance est créée pour afficher les données personnalisées de code confidentiel. Cette classe est un contrôle utilisateur.
- L’emplacement géographique d’affichage la `XamarinMapOverlay` d’instance sur le `MapControl` est créé comme un `Geopoint` instance.
- Le `XamarinMapOverlay` instance est ajoutée à la `MapControl.Children` collection. Cette collection contient des éléments d’interface utilisateur XAML qui seront affichés sur la carte.
- L’emplacement géographique de la `XamarinMapOverlay` instance sur la carte est définie en appelant le `SetLocation` (méthode).
- Emplacement relatif sur le `XamarinMapOverlay` instance, ce qui correspond à l’emplacement spécifié, est définie en appelant le `SetNormalizedAnchorPoint` (méthode). Cela garantit que les modifications dans le niveau de zoom du résultat de mappage dans le `XamarinMapOverlay` toujours affiché à l’emplacement correct de l’instance.

Ou bien, si plus d’informations sur le code confidentiel sont déjà affichées sur la carte, appuyez sur la carte supprime le `XamarinMapOverlay` instance à partir de la `MapControl.Children` collection.

#### <a name="tapping-on-the-information-button"></a>Pression sur le bouton informations

Lorsque l’utilisateur appuie sur le *informations* situé dans le `XamarinMapOverlay` contrôle utilisateur, le `Tapped` se déclenche des événements, qui à son tour exécute le `OnInfoButtonTapped` (méthode) :

```csharp
private async void OnInfoButtonTapped(object sender, TappedRoutedEventArgs e)
{
    await Launcher.LaunchUriAsync(new Uri(customPin.Url));
}
```

Cette méthode ouvre un navigateur web et accède à l’adresse stockée dans le `Url` propriété de la `CustomPin` instance. Notez que l’adresse a été défini lors de la création du `CustomPin` collection dans le projet de bibliothèque de classes portables.

Pour plus d’informations sur la personnalisation d’un `MapControl` instance, consultez [cartes et vue d’ensemble de l’emplacement](https://msdn.microsoft.com/library/windows/apps/mt219699.aspx) sur MSDN.

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment créer un convertisseur personnalisé pour le `Map` (contrôle), permettant aux développeurs de substituer le rendu natif par défaut avec leur propres personnalisation spécifiques à la plateforme. Xamarin.Forms.Maps fournit une abstraction de multiplateforme pour l’affichage des cartes qui utilisent le mappage natif rencontrer des API sur chaque plateforme pour fournir une carte rapide et familière pour les utilisateurs.


## <a name="related-links"></a>Liens associés

- [Contrôle de mappages](~/xamarin-forms/user-interface/map.md)
- [Cartes d’e/s](~/ios/user-interface/controls/ios-maps/index.md)
- [API Cartes](~/android/platform/maps-and-location/maps/maps-api.md)
- [Code confidentiel personnalisé (exemple)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/pin/)
