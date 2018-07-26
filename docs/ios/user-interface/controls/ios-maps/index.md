---
title: Cartes dans Xamarin.iOS
description: Ce document décrit l’infrastructure de MapKit iOS et comment il est utilisé avec Xamarin.iOS. Elle explique comment ajouter un mappage, style, panoramique et zoom, travailler avec l’emplacement de l’utilisateur, ajouter des annotations, des légendes et des superpositions et effectuer la recherche locale.
ms.prod: xamarin
ms.assetid: 5DD8E56D-51C1-4AFA-B387-79B5734698ED
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 5343f53b77319b08424263103834ffcf10e261a0
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242064"
---
# <a name="maps-in-xamarinios"></a>Cartes dans Xamarin.iOS

Les mappages sont une fonctionnalité commune dans tous les systèmes d’exploitation mobiles modernes. iOS prend en charge de mappage en mode natif par le biais du Kit de la carte. Avec le Kit de la carte, applications peuvent facilement ajouter des mappages riches et interactives. Ces cartes peuvent être personnalisés de plusieurs façons, telles que l’ajout d’annotations pour marquer des emplacements sur une carte et recouvrir des graphiques de formes arbitraires. Kit de carte a même la prise en charge intégrée de l’affichage de l’emplacement actuel d’un appareil.

## <a name="adding-a-map"></a>Ajout d’une carte

Ajout d’une carte à une application s’effectue en ajoutant un `MKMapView` de l’instance de la hiérarchie d’affichage, comme indiqué ci-dessous :

```csharp
// map is an MKMapView declared as a class variable
map = new MKMapView (UIScreen.MainScreen.Bounds);
View = map;
```

 `MKMapView` est un `UIView` sous-classe qui affiche une carte. Simplement ajouter le mappage en utilisant le code ci-dessus génère une carte interactive :

 ![](images/00-map.png "Un exemple de mappage")

## <a name="map-style"></a>Style de carte

 `MKMapView` prend en charge 3 différents styles de cartes. Pour appliquer un style de carte, il suffit de définir la `MapType` propriété une valeur à partir de la `MKMapType` énumération :
 ```
map.MapType = MKMapType.Standard; //road map
map.MapType = MKMapType.Satellite;
map.MapType = MKMapType.Hybrid;
 ```
  La capture d’écran suivante montrent les styles de carte différents sont disponibles :

 ![](images/01-mapstyles.png "Cette capture d’écran afficher les styles de carte différents sont disponibles")

## <a name="panning-and-zooming"></a>Panoramique et zoom

 `MKMapView` prend en charge les fonctionnalités d’interactivité carte telles que :

-  Le zoom via un mouvement de pincement
-  Panoramique via un mouvement panoramique


Ces fonctionnalités peuvent être activées ou désactivées en définissant simplement le `ZoomEnabled` et `ScrollEnabled` propriétés de la `MKMapView` instance, où la valeur par défaut est true pour les deux. Par exemple, pour afficher une carte statique, définissez simplement les propriétés appropriées sur false :

```csharp
map.ZoomEnabled = false;
map.ScrollEnabled = false;
```

## <a name="user-location"></a>Emplacement de l’utilisateur

En plus de l’interaction de l’utilisateur, `MKMapView` a également une prise en charge intégrée pour l’affichage de l’emplacement de l’appareil. Il utilise pour cela le *Core emplacement* framework. Avant de pouvoir accéder à l’emplacement utilisateur, vous devez inviter l’utilisateur. Pour ce faire, créez une instance de `CLLocationManager` et appelez `RequestWhenInUseAuthorization`.

```csharp
CLLocationManager locationManager = new CLLocationManager();
locationManager.RequestWhenInUseAuthorization();
//locationManager.RequestAlwaysAuthorization(); //requests permission for access to location data while running in the background
```

Notez que dans les versions d’iOS antérieure 8.0, essayez d’appeler `RequestWhenInUseAuthorization` entraîne une erreur. Veillez à vérifier la version d’e/s avant d’effectuer cet appel si vous avez l’intention de prendre en charge des versions antérieures à 8.

L’accès à l’emplacement utilisateur nécessite également des modifications à **Info.plist**. Vous devez définir les clés suivantes relatives aux données d’emplacement :

- **NSLocationWhenInUseUsageDescription** : cette clé est utilisée quand vous accédez à l’emplacement d’un utilisateur en train d’interagir avec votre application.
- **NSLocationAlwaysUsageDescription** : cette clé est utilisée quand votre application accède à l’emplacement d’un utilisateur en arrière-plan.

Vous pouvez ajouter ces clés en ouvrant **Info.plist** et en sélectionnant *Source* en bas de l’éditeur.

Une fois que vous avez mis à jour **Info.plist** et vous y êtes invité à l’utilisateur l’autorisation d’accéder à leur emplacement, vous pouvez afficher l’emplacement de l’utilisateur sur la carte en définissant le `ShowsUserLocation` true à la propriété :

```csharp
map.ShowsUserLocation = true;
```

 ![](images/02-location-alert.png "L’alerte de l’accès d’emplacement autoriser")
 
## <a name="annotations"></a>Annotations

 `MKMapView` prend également en charge d’affichage d’images, connu sous forme d’annotations, sur une carte. Il peut s’agir d’images personnalisées ou définie par le système de codes confidentiels de différentes couleurs. Par exemple, la capture d’écran suivante illustre un mappage avec un à la fois un code confidentiel et une image personnalisée :

 ![](images/03-annotations.png "Cette capture d’écran illustre un mappage avec un à la fois un code confidentiel et une image personnalisée")

### <a name="adding-an-annotation"></a>Ajout d’une annotation

Une annotation lui-même comporte deux parties :

-  Le `MKAnnotation` objet, qui inclut des données de modèle sur l’annotation, telles que le titre et l’emplacement de l’annotation.
-  Le `MKAnnotationView` , qui contient l’image à afficher et éventuellement une légende qui s’affiche lorsque l’utilisateur appuie sur l’annotation.


Mapkit utilise le modèle de délégation iOS pour ajouter des annotations à un mappage, où le `Delegate` propriété du `MKMapView` est définie sur une instance d’un `MKMapViewDelegate`. Il est l’implémentation de ce délégué qui est chargée de retourner la `MKAnnotationView` d’une annotation.

Pour ajouter une annotation, tout d’abord l’annotation est ajoutée en appelant `AddAnnotations` sur la `MKMapView` instance :

```csharp
// add an annotation
map.AddAnnotations (new MKPointAnnotation (){
    Title="MyAnnotation",
    Coordinate = new CLLocationCoordinate2D (42.364260, -71.120824)
});
```

Lorsque l’emplacement de l’annotation est visible sur la carte, le `MKMapView` appellera de son délégué `GetViewForAnnotation` méthode pour obtenir le `MKAnnotationView` à afficher.

Par exemple, le code suivant retourne fournie par le système `MKPinAnnotationView`:

```csharp
string pId = "PinAnnotation";

public override MKAnnotationView GetViewForAnnotation (MKMapView mapView, NSObject annotation)
{
    if (annotation is MKUserLocation)
        return null;

    // create pin annotation view
    MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);

    if (pinView == null)
        pinView = new MKPinAnnotationView (annotation, pId);

    ((MKPinAnnotationView)pinView).PinColor = MKPinAnnotationColor.Red;
    pinView.CanShowCallout = true;

    return pinView;
}
```

### <a name="reusing-annotations"></a>Réutilisation des Annotations

Pour économiser la mémoire, `MKMapView` permet aux vue de l’annotation de regroupés pour réutilisation, similaire à la façon dont les cellules du tableau sont réutilisées. Obtention d’une vue de l’annotation à partir du pool est effectuée avec un appel à `DequeueReusableAnnotation`:

```csharp
MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);
```

#### <a name="showing-callouts"></a>Affichage des légendes

Comme mentionné précédemment, une annotation peut éventuellement afficher une légende. Pour afficher une légende simplement définie `CanShowCallout` sur true sur le `MKAnnotationView`. Il en résulte dans le titre de l’annotation s’affiche lorsque l’annotation est activé par un clic, comme indiqué :

 ![](images/04-callout.png "Les annotations du titre affiché")

### <a name="customizing-the-callout"></a>Personnalisation de la légende

La légende peut également être personnalisée pour afficher les affichages gauche et droite Accessoires, comme indiqué ci-dessous :

```csharp
pinView.RightCalloutAccessoryView = UIButton.FromType (UIButtonType.DetailDisclosure);
pinView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile ("monkey.png"));
```

Ce code génère la légende suivante :

 ![](images/05-callout-accessories.png "Une légende de l’exemple")

Pour gérer l’utilisateur en appuyant sur la droite accessoire, implémentez simplement le `CalloutAccessoryControlTapped` méthode dans le `MKMapViewDelegate`:

```csharp
public override void CalloutAccessoryControlTapped (MKMapView mapView, MKAnnotationView view, UIControl control)
{
    ...
}
```

### <a name="overlays"></a>Superpositions

Une autre façon de graphiques de couche sur une carte à l’aide de superpositions. Superpositions prend en charge le dessin contenu graphique qui s’adapte à la carte, comme elle est redimensionnée. iOS prend en charge plusieurs types de superpositions, y compris :

-  Polygones - couramment utilisés pour mettre en surbrillance une région sur une carte.
-  Polylignes - souvent lors de l’affichage d’un itinéraire.
-  Cercles - utilisés pour mettre en surbrillance une zone circulaire d’un mappage.


En outre, des superpositions personnalisées peuvent être créées pour afficher les géométries arbitraires avec code de dessin granulaire et personnalisé. Par exemple, en radar weather serait un bon candidat pour un segment de recouvrement personnalisé.

#### <a name="adding-an-overlay"></a>Ajout d’une superposition

Similaire aux annotations, ajout d’un segment de recouvrement déroule 2 parties :

-  Création d’un objet de modèle pour la superposition et en l’ajoutant à la `MKMapView` .
-  Création d’une vue pour la superposition dans la `MKMapViewDelegate` .


Le modèle pour la superposition peut être une `MKShape` sous-classe. Inclut Xamarin.iOS `MKShape` sous-classes des polygones, des polylignes et des cercles, via le `MKPolygon`, `MKPolyline` et `MKCircle` classes respectivement.

Par exemple, le code suivant est utilisé pour ajouter un `MKCircle`:

```csharp
var circleOverlay = MKCircle.Circle (mapCenter, 1000);
map.AddOverlay (circleOverlay);
```

La vue de superposition est un `MKOverlayView` instance qui est retourné par la `GetViewForOverlay` dans le `MKMapViewDelegate`. Chaque `MKShape` correspond un `MKOverlayView` qui sait comment afficher la forme donnée. Pour `MKPolygon` est `MKPolygonView`. De même, `MKPolyline` correspond à `MKPolylineView`et pour `MKCircle` est `MKCircleView`.

Par exemple, le code suivant retourne un `MKCircleView` pour un `MKCircle`:

```csharp
public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
{
    var circleOverlay = overlay as MKCircle;
    var circleView = new MKCircleView (circleOverlay);
    circleView.FillColor = UIColor.Blue;
    return circleView;
}
```

Cela affiche un cercle sur la carte, comme indiqué :

 ![](images/06-circle-overlay.png "Un cercle affiché sur la carte")

## <a name="local-search"></a>Recherche locale

iOS inclut une API avec le Kit de mappage, ce qui permet des recherches asynchrones pour les points d’intérêt dans une région géographique spécifiée de recherche locale.

Pour effectuer une recherche en locale, une application doit procéder comme suit :

1.  Créer `MKLocalSearchRequest` objet.
1.  Créer un `MKLocalSearch` de l’objet à partir de la `MKLocalSearchRequest` .
1.  Appelez le `Start` méthode sur le `MKLocalSearch` objet.
1.  Récupérer le `MKLocalSearchResponse` objet dans un rappel.


L’API elle-même de recherche local ne fournit aucune interface utilisateur. Il ne nécessite pas même un mappage à utiliser. Toutefois, pour rendre l’utilisation pratique de recherche locale, une application doit fournir un moyen de spécifier une requête de recherche et d’affichage des résultats. En outre, étant donné que les résultats contiennent les données d’emplacement, il est souvent judicieux de les afficher sur une carte.

<a name="Adding_a_Local_Search_UI"/>

### <a name="adding-a-local-search-ui"></a>Ajout d’une interface utilisateur de recherche Local

La première consiste à accepter une entrée de recherche avec un `UISearchBar`, qui est fournie par un `UISearchController` et affiche les résultats dans une table.

Le code suivant ajoute le `UISearchController` (qui est une propriété de barre de recherche) dans le `ViewDidLoad` méthode de `MapViewController`:

```csharp
//Creates an instance of a custom View Controller that holds the results
var searchResultsController = new SearchResultsViewController (map);

//Creates a search controller updater
var searchUpdater = new SearchResultsUpdator ();
searchUpdater.UpdateSearchResults += searchResultsController.Search;
            
//add the search controller
searchController = new UISearchController (searchResultsController) {
                SearchResultsUpdater = searchUpdater
            };

//format the search bar
searchController.SearchBar.SizeToFit ();
searchController.SearchBar.SearchBarStyle = UISearchBarStyle.Minimal;
searchController.SearchBar.Placeholder = "Enter a search query";

//the search bar is contained in the navigation bar, so it should be visible
searchController.HidesNavigationBarDuringPresentation = false;

//Ensure the searchResultsController is presented in the current View Controller 
DefinesPresentationContext = true;

//Set the search bar in the navigation bar
NavigationItem.TitleView = searchController.SearchBar;

```csharp
Note that you are responsible for incorporating the search bar object into the user interface. In this example, we assigned it to the TitleView of the navigation bar, but if you do not use a navigation controller in your application you will have to find another place to display it.

In this code snippet, we created another custom view controller – `searchResultsController` –  that displays the search results and then we used this object to create our search controller object. We also created a new search updater, which becomes active when the user interacts with the search bar. It receives notifications about searches with each keystroke and is responsible for updating the UI.
We will take a look at how to implement both the `searchResultsController` and the `searchResultsUpdater` later in this guide.

This results in a search bar displayed over the map as shown below:

 ![](images/07-searchbar.png "A search bar displayed over the map")
 


### Displaying the Search Results

To display search results, we need to create a custom View Controller; normally a `UITableViewController`. As shown above, the `searchResultsController` is passed to the constructor of the `searchController` when it is being created.
The following code is an example of how to create this custom View Controller:

```csharp
public class SearchResultsViewController : UITableViewController
{
    static readonly string mapItemCellId = "mapItemCellId";
    MKMapView map;

    public List<MKMapItem> MapItems { get; set; }

    public SearchResultsViewController (MKMapView map)
    {
        this.map = map;

        MapItems = new List<MKMapItem> ();
    }

    public override nint RowsInSection (UITableView tableView, nint section)
    {
        return MapItems.Count;
    }

    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
    {
        var cell = tableView.DequeueReusableCell (mapItemCellId);

        if (cell == null)
            cell = new UITableViewCell ();

        cell.TextLabel.Text = MapItems [indexPath.Row].Name;
        return cell;
    }

    public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
    {
        // add item to map
        CLLocationCoordinate2D coord = MapItems [indexPath.Row].Placemark.Location.Coordinate;
        map.AddAnnotations (new MKPointAnnotation () {
            Title = MapItems [indexPath.Row].Name,
            Coordinate = coord
        });

        map.SetCenterCoordinate (coord, true);

        DismissViewController (false, null);
    }

    public void Search (string forSearchString)
    {
        // create search request
        var searchRequest = new MKLocalSearchRequest ();
        searchRequest.NaturalLanguageQuery = forSearchString;
        searchRequest.Region = new MKCoordinateRegion (map.UserLocation.Coordinate, new MKCoordinateSpan (0.25, 0.25));

        // perform search
        var localSearch = new MKLocalSearch (searchRequest);

        localSearch.Start (delegate (MKLocalSearchResponse response, NSError error) {
            if (response != null && error == null) {
                this.MapItems = response.MapItems.ToList ();
                this.TableView.ReloadData ();
            } else {
                Console.WriteLine ("local search error: {0}", error);
            }
        });


    }
}
```

### <a name="updating-the-search-results"></a>La mise à jour les résultats de recherche

Le `SearchResultsUpdater` agit comme un médiateur entre le `searchController`de barre de recherche et les résultats de la recherche. 

Dans cet exemple, nous avons d’abord créer la méthode de recherche dans le `SearchResultsViewController`. Pour cela nous devons créer un `MKLocalSearch` de l’objet et l’utiliser pour émettre une recherche pour un `MKLocalSearchRequest`, les résultats sont récupérés dans un rappel passé à la `Start` méthode de la `MKLocalSearch` objet. Les résultats sont ensuite renvoyés dans une `MKLocalSearchResponse` objet contenant un tableau de `MKMapItem` objets :

```csharp
public void Search (string forSearchString)
{
    // create search request
    var searchRequest = new MKLocalSearchRequest ();
    searchRequest.NaturalLanguageQuery = forSearchString;
    searchRequest.Region = new MKCoordinateRegion (map.UserLocation.Coordinate, new MKCoordinateSpan (0.25, 0.25));

    // perform search
    var localSearch = new MKLocalSearch (searchRequest);

    localSearch.Start (delegate (MKLocalSearchResponse response, NSError error) {
        if (response != null && error == null) {
            this.MapItems = response.MapItems.ToList ();
            this.TableView.ReloadData ();
        } else {
            Console.WriteLine ("local search error: {0}", error);
        }
    });


}
```

Ensuite, dans notre `MapViewController` nous allons créer une implémentation personnalisée de `UISearchResultsUpdating`, qui est assignée à la `SearchResultsUpdater` propriété de notre `searchController` dans le [Ajout d’une interface utilisateur de recherche Local](#Adding_a_Local_Search_UI) section :

```csharp
public class SearchResultsUpdator : UISearchResultsUpdating
{
    public event Action<string> UpdateSearchResults = delegate {};

    public override void UpdateSearchResultsForSearchController (UISearchController searchController)
    {
        this.UpdateSearchResults (searchController.SearchBar.Text);
    }
}
```

L’implémentation ci-dessus ajoute une annotation à la carte lorsqu’un élément est sélectionné dans les résultats, comme indiqué ci-dessous :

 ![](images/08-search-results.png "Une annotation ajoutée à la carte lorsqu’un élément est sélectionné à partir des résultats")
 
> [!IMPORTANT]
> `UISearchController` a été implémenté dans iOS 8. Si vous souhaitez prendre en charge les appareils antérieurs à cela, vous devrez utiliser `UISearchDisplayController`.



## <a name="summary"></a>Récapitulatif

Cet article examiné la *carte* *Kit* framework pour iOS. Tout d’abord, il étudié la façon la `MKMapView` classe permet de cartes interactives à inclure dans une application. Puis il a montré comment personnaliser davantage les mappages à l’aide des annotations et superpositions. Enfin, il a examiné les fonctionnalités de recherche local qui ont été ajoutées au Kit de la carte avec iOS 6.1, illustrant l’utilisation d’effectuer des requêtes d’emplacement en fonction des points d’intérêt et de les ajouter à une carte.

## <a name="related-links"></a>Liens associés

- [SearchController](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller)
- [MapDemo (exemple)](https://developer.xamarin.com/samples/monotouch/MapDemo)
