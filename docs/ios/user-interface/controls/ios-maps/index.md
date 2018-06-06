---
title: Mappages de Xamarin.iOS
description: Ce document décrit le framework MapKit iOS et comment il est utilisé avec Xamarin.iOS. Elle explique comment ajouter un mappage, le style, panoramique et zoom, travailler avec l’emplacement de l’utilisateur, ajouter des annotations, travailler avec des légendes et les superpositions et effectuer la recherche locale.
ms.prod: xamarin
ms.assetid: 5DD8E56D-51C1-4AFA-B387-79B5734698ED
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 3649c8eb9c8c1a82940b8e2eece7d2bfd005d024
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790228"
---
# <a name="maps-in-xamarinios"></a>Mappages de Xamarin.iOS

Les cartes sont une fonctionnalité courante dans tous les systèmes d’exploitation mobiles modernes. iOS offre la prise en charge du mappage en mode natif par le biais du Kit de la carte. Kit de la carte, les applications peuvent facilement ajouter des mappages riches et interactives. Ces mappages peuvent être personnalisés dans une variété de différentes manières, comme l’ajout d’annotations pour marquer des emplacements sur une carte, recouvrir des graphiques des formes arbitraires. Kit de carte possède même prise en charge intégrée pour afficher l’emplacement actuel d’un périphérique.

## <a name="adding-a-map"></a>Ajout d’une carte

Ajout d’un mappage à une application s’effectue en ajoutant un `MKMapView` de l’instance à la hiérarchie de la vue, comme indiqué ci-dessous :

```csharp
// map is an MKMapView declared as a class variable
map = new MKMapView (UIScreen.MainScreen.Bounds);
View = map;
```

 `MKMapView` est un `UIView` sous-classe qui affiche une carte. Simple ajout de la carte en utilisant le code ci-dessus génère une carte interactive :

 ![](images/00-map.png "Un exemple de mappage")

## <a name="map-style"></a>Style de carte

 `MKMapView` prend en charge 3 différents styles de cartes. Pour appliquer un style de carte, il suffit de définir la `MapType` propriété à une valeur à partir de la `MKMapType` énumération :
 ```
map.MapType = MKMapType.Standard; //road map
map.MapType = MKMapType.Satellite;
map.MapType = MKMapType.Hybrid;
 ```
  La capture d’écran suivante affiche les styles de carte différents sont disponibles :

 ![](images/01-mapstyles.png "Cette capture d’écran afficher les styles de carte différents sont disponibles")

## <a name="panning-and-zooming"></a>Le panoramique et zoom

 `MKMapView` inclut la prise en charge des fonctionnalités d’interactivité de mappage tels que :

-  Zoom via un mouvement de pincement
-  Panoramique via un mouvement panoramique


Ces fonctionnalités peuvent être activées ou désactivées en définissant simplement le `ZoomEnabled` et `ScrollEnabled` propriétés de la `MKMapView` instance, où la valeur par défaut est true pour les deux. Par exemple, pour afficher un mappage statique, définissez simplement les propriétés appropriées sur false :

```csharp
map.ZoomEnabled = false;
map.ScrollEnabled = false;
```

## <a name="user-location"></a>Emplacement de l’utilisateur

En plus des interventions de l’utilisateur `MKMapView` a également une prise en charge intégrée pour l’affichage de l’emplacement de l’appareil. Pour cela, à l’aide de la *site central* framework. Avant de pouvoir accéder à l’emplacement utilisateur, vous devez inviter l’utilisateur. Pour ce faire, créez une instance de `CLLocationManager` et appelez `RequestWhenInUseAuthorization`.

```csharp
CLLocationManager locationManager = new CLLocationManager();
locationManager.RequestWhenInUseAuthorization();
//locationManager.RequestAlwaysAuthorization(); //requests permission for access to location data while running in the background
```

Notez que dans les versions d’iOS avant 8.0, toute tentative d’appel `RequestWhenInUseAuthorization` entraîne une erreur. Veillez à vérifier la version d’iOS avant d’effectuer cet appel si vous envisagez de prendre en charge les versions antérieures à 8.

L’accès à l’emplacement utilisateur requiert également les modifications apportées à **Info.plist**. Vous devez définir les clés suivantes relatives aux données d’emplacement :

- **NSLocationWhenInUseUsageDescription** : cette clé est utilisée quand vous accédez à l’emplacement d’un utilisateur en train d’interagir avec votre application.
- **NSLocationAlwaysUsageDescription** : cette clé est utilisée quand votre application accède à l’emplacement d’un utilisateur en arrière-plan.

Vous pouvez ajouter ces clés en ouvrant **Info.plist** et en sélectionnant *Source* au bas de l’éditeur.

Une fois que vous avez mis à jour **Info.plist** et vous y êtes invité à l’utilisateur l’autorisation d’accéder à leur emplacement, vous pouvez afficher l’emplacement de l’utilisateur sur la carte en définissant le `ShowsUserLocation` true à la propriété :

```csharp
map.ShowsUserLocation = true;
```

 ![](images/02-location-alert.png "L’alerte de l’accès d’emplacement autoriser")
 
## <a name="annotations"></a>Annotations

 `MKMapView` prend également en charge d’affichage d’images, connue sous forme d’annotations, sur une carte. Il peut s’agir d’images personnalisées ou codes confidentiels de définies par le système de couleurs différentes. Par exemple, la capture d’écran suivante illustre un mappage avec un à la fois un code confidentiel et une image personnalisée :

 ![](images/03-annotations.png "Cette capture d’écran illustre un mappage avec un à la fois un code confidentiel et une image personnalisée")

### <a name="adding-an-annotation"></a>Ajout d’une annotation

Une annotation compose de deux parties :

-  Le `MKAnnotation` objet, qui inclut des données de modèle sur l’annotation, telles que le titre et l’emplacement de l’annotation.
-  Le `MKAnnotationView` , qui contient l’image à afficher et éventuellement une légende qui est affichée lorsque l’utilisateur appuie sur l’annotation.


Kit de la carte utilise le modèle de délégation d’iOS pour ajouter des annotations à une carte, où le `Delegate` propriété de la `MKMapView` est définie à une instance d’un `MKMapViewDelegate`. Il s’agit d’une mise en œuvre de ce délégué qui est chargée de retourner la `MKAnnotationView` d’une annotation.

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

Pour économiser la mémoire, `MKMapView` permet l’affichage de l’annotation permettre d’être regroupées pour une réutilisation, similaire à la façon dont les cellules du tableau sont réutilisées. Obtention d’une vue de l’annotation à partir du pool est effectuée par un appel à `DequeueReusableAnnotation`:

```csharp
MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);
```

#### <a name="showing-callouts"></a>Affichage des légendes

Comme mentionné précédemment, une annotation peut éventuellement afficher une légende. Pour afficher une légende simplement définie `CanShowCallout` sur true la `MKAnnotationView`. Cela entraîne le titre de l’annotation affichée lorsque l’utilisateur clique sur l’annotation, comme indiqué :

 ![](images/04-callout.png "Les annotations du titre affiché")

### <a name="customizing-the-callout"></a>Personnalisation de la légende

La légende peut également être personnalisée pour afficher des vues gauche et droite Accessoires, comme indiqué ci-dessous :

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

Une autre façon de graphiques de couche sur une carte à l’aide de superpositions. Superpositions prend en charge le dessin contenu graphique qui met à l’échelle avec la carte comme il est agrandi ou réduit. e/s prend en charge plusieurs types de superpositions, y compris :

-  Polygones - couramment utilisés pour mettre en surbrillance une région sur une carte.
-  Polylignes - souvent lors de l’affichage d’un itinéraire.
-  Cercles - utilisés pour mettre en surbrillance une zone circulaire d’un mappage.


En outre, des superpositions personnalisées peuvent être créées pour afficher les géométries arbitraires avec code de dessin granulaire et personnalisé. Par exemple, en radar de météo serait un bon candidat pour un segment de recouvrement personnalisé.

#### <a name="adding-an-overlay"></a>Ajout d’un segment de recouvrement

Similaires aux annotations, l’ajout d’un segment de recouvrement nécessite 2 parties :

-  Création d’un objet de modèle pour le segment de recouvrement, puis ajoutez-la à la `MKMapView` .
-  Création d’une vue pour la superposition dans le `MKMapViewDelegate` .


Le modèle pour le segment de recouvrement peut être une `MKShape` sous-classe. Inclut des Xamarin.iOS `MKShape` sous-classes des polygones, des polylignes et des cercles, via le `MKPolygon`, `MKPolyline` et `MKCircle` respectivement des classes.

Par exemple, le code suivant permet d’ajouter un `MKCircle`:

```csharp
var circleOverlay = MKCircle.Circle (mapCenter, 1000);
map.AddOverlay (circleOverlay);
```

La vue pour un segment de recouvrement est un `MKOverlayView` instance qui est retournée par le `GetViewForOverlay` dans le `MKMapViewDelegate`. Chaque `MKShape` est associée à une `MKOverlayView` qui sait comment afficher la forme donnée. Pour `MKPolygon` est `MKPolygonView`. De même, `MKPolyline` correspond à `MKPolylineView`et pour `MKCircle` est `MKCircleView`.

Par exemple, le code suivant retourne un `MKCircleView` pour une `MKCircle`:

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

iOS inclut une recherche locale API avec le Kit de carte, ce qui permet de rechercher asynchrone pour les points d’intérêt dans une région géographique spécifié.

Pour effectuer une recherche locale, une application procédez comme suit :

1.  Créer `MKLocalSearchRequest` objet.
1.  Créer un `MKLocalSearch` de l’objet à partir de la `MKLocalSearchRequest` .
1.  Appelez le `Start` méthode sur le `MKLocalSearch` objet.
1.  Récupérer le `MKLocalSearchResponse` objet dans un rappel.


La recherche locale API elle-même ne fournit aucune interface utilisateur. Il ne nécessite pas même un mappage à utiliser. Toutefois, pour rendre l’utilisation pratique de recherche locale, une application doit fournir un moyen de spécifier une requête de recherche et afficher les résultats. En outre, étant donné que les résultats contiendra les données d’emplacement, il est souvent judicieux de les afficher sur une carte.

<a name="Adding_a_Local_Search_UI"/>

### <a name="adding-a-local-search-ui"></a>Ajout d’une interface utilisateur de recherche Local

La première consiste à accepter l’entrée de recherche avec un `UISearchBar`, qui est fournie par un `UISearchController` et affiche les résultats dans une table.

Le code suivant ajoute le `UISearchController` (qui a une propriété de barre de recherche) dans le `ViewDidLoad` méthode `MapViewController`:

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

### <a name="updating-the-search-results"></a>Mise à jour les résultats de recherche

Le `SearchResultsUpdater` agit comme un médiateur entre le `searchController`de barre de recherche et les résultats de la recherche. 

Dans cet exemple, nous avons tout d’abord créer la méthode de recherche dans le `SearchResultsViewController`. Pour cela, nous devons créer un `MKLocalSearch` de l’objet et l’utiliser pour émettre une recherche pour un `MKLocalSearchRequest`, les résultats sont récupérés dans un rappel passé à la `Start` méthode de la `MKLocalSearch` objet. Les résultats sont ensuite renvoyés dans un `MKLocalSearchResponse` objet contenant un tableau de `MKMapItem` objets :

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

 ![](images/08-search-results.png "Une annotation ajoutée à la table lorsqu’un élément est sélectionné à partir des résultats")
 
> [!IMPORTANT]
> `UISearchController` a été implémentée dans iOS 8. Si vous souhaitez prendre en charge des appareils antérieure à cela, vous devez utiliser `UISearchDisplayController`.



## <a name="summary"></a>Récapitulatif

Cet article examiné le *carte* *Kit* framework pour iOS. Tout d’abord, il examiné comment la `MKMapView` classe permet à des cartes interactives à inclure dans une application. Puis il a montré comment personnaliser davantage les mappages à l’aide des annotations et les superpositions. Enfin, elle examinait les fonctionnalités de recherche local qui ont été ajoutées à la carte Kit iOS 6.1, illustrant l’utilisation d’exécuter des requêtes d’emplacement en fonction de points d’intérêt et de les ajouter à une carte.

## <a name="related-links"></a>Liens associés

- [SearchController](https://developer.xamarin.com/recipes/ios/content_controls/search-controller/)
- [MapDemo (exemple)](https://developer.xamarin.com/samples/monotouch/MapDemo)
