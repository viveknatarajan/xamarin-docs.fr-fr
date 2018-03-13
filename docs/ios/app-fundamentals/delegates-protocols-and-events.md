---
title: "Les délégués, les protocoles et les événements"
description: "Cet article présente les technologies de clé iOS utilisés pour recevoir des rappels et remplir les contrôles d’interface utilisateur avec des données. Ces technologies sont les événements, les protocoles et les délégués. Cet article explique ce que chacune d’elles est et comment chacune est utilisée à partir de c#. Il montre comment Xamarin.iOS utilise les contrôles d’iOS pour exposer des événements de .NET familiers, ainsi que la manière dont Xamarin.iOS prend en charge les concepts de Objective-C, tels que les protocoles et des délégués (délégués d’Objective-C ne doivent pas être confondues avec les délégués en c#). Cet article fournit également des exemples qui montrent comment les protocoles sont utilisés : à la fois comme base pour les délégués de Objective-C et dans les scénarios non délégué."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7C07F0B7-9000-C540-0FC3-631C29610447
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 69296992c503d536a4160f172022c7ce5578812f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="events-protocols-and-delegates"></a>Les délégués, les protocoles et les événements

_Cet article présente les technologies de clé iOS utilisés pour recevoir des rappels et remplir les contrôles d’interface utilisateur avec des données. Ces technologies sont les événements, les protocoles et les délégués. Cet article explique ce que chacune d’elles est et comment chacune est utilisée à partir de c#. Il montre comment Xamarin.iOS utilise les contrôles d’iOS pour exposer des événements de .NET familiers, ainsi que la manière dont Xamarin.iOS prend en charge les concepts de Objective-C, tels que les protocoles et des délégués (délégués d’Objective-C ne doivent pas être confondues avec les délégués en c#). Cet article fournit également des exemples qui montrent comment les protocoles sont utilisés : à la fois comme base pour les délégués de Objective-C et dans les scénarios non délégué._

Xamarin.iOS utilise des contrôles pour exposer des événements pour la plupart des interactions de l’utilisateur.
Xamarin.iOS applications consomment ces événements de la même façon, comme des applications .NET traditionnelles. Par exemple, la classe Xamarin.iOS UIButton dispose d’un événement appelé TouchUpInside et utilise cet événement, comme si cette classe et les événements ont été dans une application .NET.

Outre cette approche .NET, Xamarin.iOS expose un autre modèle qui peut être utilisé pour la liaison de données et une interaction plus complexe. Cette méthode utilise qu’Apple appelle les délégués et les protocoles. Les délégués sont un concept similaire aux délégués en c#, mais au lieu de la définition et l’appel d’une méthode unique, un délégué dans Objective-C est une classe entière est conforme à un protocole. Un protocole est similaire à une interface en c#, sauf que ses méthodes peuvent être facultatifs. Par exemple, pour remplir un UITableView avec des données, vous créeriez une classe déléguée qui implémente les méthodes définies dans le protocole de UITableViewDataSource le UITableView appellerait remplit elle-même.

Dans cet article, vous allez en savoir plus sur tous ces aspects, ce qui vous donne une base solide pour gérer les scénarios de rappel dans Xamarin.iOS, y compris :

-  **Événements** : événements à l’aide de .NET avec des contrôles de UIKit.
-  **Protocoles** : apprentissage que les protocoles sont et comment elles sont utilisées, et créer un exemple qui fournit des données pour une annotation de la carte.
-  **Délégués** – apprentissage sur les délégués Objective-C en étendant l’exemple de mappage pour gérer l’interaction utilisateur qui inclut une annotation, puis la différence entre les délégués de nom forts et faibles et quand utiliser chacun de ces d’apprentissage.


Pour illustrer les protocoles et les délégués, nous allons construire une application de mappage simple qui ajoute une annotation à un mappage, comme illustré ici :

 [![](delegates-protocols-and-events-images/01-map.png "Un exemple d’une application de mappage simple qui ajoute une annotation à un mappage") ](delegates-protocols-and-events-images/01-map.png#lightbox) [ ![ ] (delegates-protocols-and-events-images/04-annotation-with-callout.png "une annotation exemple ajoutée à une carte")](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

Avant d’aborder cette application, nous allons commencer en examinant les événements .NET sous la UIKit.

 <a name=".NET_Events_with_UIKit" />


## <a name="net-events-with-uikit"></a>Événements .NET avec UIKit

Xamarin.iOS expose des événements .NET sur les contrôles UIKit. Par exemple, UIButton a un événement TouchUpInside, que vous gérer comme vous le feriez normalement dans .NET, comme indiqué dans le code suivant qui utilise une expression lambda en c# :

```csharp
aButton.TouchUpInside += (o,s) => {
    Console.WriteLine("button touched");
};
```

Vous pouvez également implémenter cela avec une style 2.0 méthode anonyme c# comme celle-ci :

```csharp
aButton.TouchUpInside += delegate {
    Console.WriteLine ("button touched");
};
```

Le code précédent est intégré dans la méthode ViewDidLoad de la UIViewContoller. La variable aButton fait référence à un bouton, vous pouvez ajouter dans le concepteur iOS ou avec le code. La figure suivante illustre ce bouton lorsqu’il est ajouté dans iOS concepteur, tiré de l’exemple qui accompagne cet article :

 [![](delegates-protocols-and-events-images/02-interface-builder-outlet.png "Un bouton ajouté dans le Concepteur d’iOS")](delegates-protocols-and-events-images/02-interface-builder-outlet.png#lightbox)

Xamarin.iOS prend également en charge le style d’action de la cible de la connexion à une interaction qui se produit avec un contrôle de votre code. Pour créer une action de cible pour le bouton Hello, double-cliquez dessus dans le concepteur iOS. Fichier de code-behind de la UIViewController s’affiche et vous demandera le développeur pour sélectionner un emplacement pour insérer la méthode de connexion :

 [![](delegates-protocols-and-events-images/03-interface-builder-action.png "Le fichier code-behind UIViewControllers")](delegates-protocols-and-events-images/03-interface-builder-action.png#lightbox)

Une fois un emplacement est sélectionné, une nouvelle méthode est créée et accès câblé au contrôle. Dans l’exemple suivant, un message est écrit dans la console lorsque le bouton est activé :

 [![](delegates-protocols-and-events-images/05-interface-builder-action.png "Un message est écrit dans la console lorsque le bouton est activé")](delegates-protocols-and-events-images/05-interface-builder-action.png#lightbox)

Pour plus d’informations sur le modèle d’action de la cible iOS, consultez la section Action de la cible de « [compétences d’Application pour iOS](http://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html)» dans la bibliothèque du développeur d’iOS d’Apple.

Pour plus d’informations sur la façon d’utiliser le concepteur iOS avec Xamarin.iOS, consultez le « [iOS vue d’ensemble du concepteur](~/ios/user-interface/designer/index.md)« documentation.

 <a name="Events" />


## <a name="events"></a>Événements

Si vous souhaitez intercepter des événements de contrôle, vous disposez d’une gamme d’options : à partir d’en utilisant les expressions lambda c# et les fonctions de délégué à l’aide des API Objective-C bas niveau.

La section suivante montre comment vous devez capturer l’événement de TouchDown sur un bouton, selon la quantité contrôle que vous avez besoin.

 <a name="C#_Style" />


## <a name="c-style"></a>Style c#

À l’aide de la syntaxe de délégué :

```csharp
UIButton button = MakeTheButton ();
button.TouchDown += delegate {    
    Console.WriteLine ("Touched");
};
```

Si vous le souhaitez à la place les expressions lambda :

```csharp
button.TouchDown += () => {
   Console.WriteLine ("Touched");
};
```

Si vous souhaitez que plusieurs boutons utilisent le même gestionnaire à partager le même code :

```csharp
void handler (object sender, EventArgs args)
{
   if (sender == button1)
      Console.WriteLine ("button1");
   else
      Console.WriteLine ("some other button");
}

button1.TouchDown += handler;
button2.TouchDown += handler;
```

<a name="Monitoring_more_than_one_kind_of_Event" />


## <a name="monitoring-more-than-one-kind-of-event"></a>Plusieurs types d’événement d’analyse

Les événements c# pour les indicateurs de UIControlEvent ont un mappage aux indicateurs individuels. Lorsque vous souhaitez avoir la même partie du code gérer deux ou plusieurs événements, utilisez la `UIControl.AddTarget` méthode :

```csharp
button.AddTarget (handler, UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

À l’aide de la syntaxe d’expression lambda :

```csharp
button.AddTarget ((sender, event)=> Console.WriteLine ("An event happened"), UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

Si vous devez utiliser les fonctionnalités de bas niveau de Objective-C, comme un raccordement d’une instance d’objet particulier et l’appel d’un sélecteur :

```csharp
[Export ("MySelector")]
void MyObjectiveCHandler ()
{
    Console.WriteLine ("Hello!");
}

// In some other place:

button.AddTarget (this, new Selector ("MySelector"), UIControlEvent.TouchDown);
```

Notez que si vous implémentez la méthode d’instance dans une classe de base héritée, il doit être une méthode publique.

 <a name="Protocols" />


## <a name="protocols"></a>Protocoles

Un protocole est une fonctionnalité de langage Objective-C qui fournit une liste de déclarations de méthode. Elle sert similaire à une interface en c#, la principale différence étant qu’un protocole peut avoir des méthodes facultatives. Méthodes facultatives ne sont pas appelés si la classe qui prend un protocole de ne pas les implémente. En outre, une seule classe dans Objective-C peut implémenter plusieurs protocoles, tout comme une classe c# peut implémenter plusieurs interfaces.

Apple utilise les protocoles dans l’ensemble d’iOS pour définir les contrats pour les classes d’adopter, tandis que l’abstraction de la classe d’implémentation de l’appelant, ce qui fonctionne comme une interface c#. Les protocoles sont utilisés à la fois dans les scénarios non délégué (comme avec la `MKAnnotation` exemple montre l’illustration suivante) et avec les délégués (comme décrit plus loin dans ce document, dans la section délégués).

 <a name="Protocols_with_Monotouch" />


### <a name="protocols-with-xamarinios"></a>Protocoles de Xamarin.ios

Examinons un exemple d’utilisation d’un protocole Objective-C à partir de Xamarin.iOS. Pour cet exemple, nous allons utiliser la `MKAnnotation` protocole, qui fait partie de la `MapKit` framework. `MKAnnotation` est un protocole qui permet à n’importe quel objet qui prend pour fournir des informations sur une annotation peut être ajoutée à une carte. Par exemple, un objet qui implémente `MKAnnotation` fournit l’emplacement de l’annotation et le titre associé.

De cette façon, le `MKAnnotation` protocole est utilisé pour fournir des données pertinentes qui accompagne une annotation. L’affichage réel pour l’annotation elle-même est construit à partir des données dans l’objet adopte la `MKAnnotation` protocole. Par exemple, le texte de la légende qui s’affiche lorsque l’utilisateur appuie sur l’annotation (comme indiqué dans la capture d’écran ci-dessous) est fourni à partir de la `Title` propriété dans la classe qui implémente le protocole :

 [![](delegates-protocols-and-events-images/04-annotation-with-callout.png "Exemple de texte de la légende lorsque l’utilisateur appuie sur l’annotation")](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

Comme décrit dans la section suivante, les protocoles de présentation approfondie, Xamarin.iOS lie les protocoles à des classes abstraites. Pour le `MKAnnotation` protocole, la classe c# liée nommée `MKAnnotation` pour reproduire le nom du protocole et il est une sous-classe de `NSObject`, la classe de base racine pour CocoaTouch. Le protocole nécessite un accesseur Get et un accesseur Set pour être implémentée pour la coordonnée ; Toutefois, un titre et le sous-titre sont facultatifs. Par conséquent, dans le `MKAnnotation` (classe), le `Coordinate` propriété est *abstraite*, qu’elle doit être implémentée et `Title` et `Subtitle` propriétés sont marquées *virtuel* , ce qui les rend facultatifs, comme indiqué ci-dessous :

```csharp
[Register ("MKAnnotation"), Model ]
public abstract class MKAnnotation : NSObject
{
    public abstract CLLocationCoordinate2D Coordinate
    {
        [Export ("coordinate")]
        get;
        [Export ("setCoordinate:")]
        set;
    }

    public virtual string Title
    {
        [Export ("title")]
        get
        {
            throw new ModelNotImplementedException ();
        }
    }

    public virtual string Subtitle
    {
        [Export ("subtitle")]
        get
        {
            throw new ModelNotImplementedException ();
        }
    }
...
}
```

Toute classe peut fournir des données de l’annotation en simplement dérivant de `MKAnnotation`, tant qu’au moins les `Coordinate` propriété est implémentée. Par exemple, Voici un exemple de classe qui prend la coordonnée dans le constructeur et retourne une chaîne pour le titre :

```csharp
/// <summary>
/// Annotation class that subclasses MKAnnotation abstract class
/// MKAnnotation is bound by Xamarin.iOS to the MKAnnotation protocol
/// </summary>
public class SampleMapAnnotation : MKAnnotation
{
    string _title;

    public SampleMapAnnotation (CLLocationCoordinate2D coordinate)
    {
        Coordinate = coordinate;
        _title = "Sample";
    }

    public override CLLocationCoordinate2D Coordinate { get; set; }

    public override string Title {
        get {
            return _title;
        }
    }
}
```

Via le protocole, il est lié, toute classe qui sous-classe `MKAnnotation` peut fournir des données pertinentes qui seront utilisées par la carte lorsqu’il crée la vue de l’annotation. Pour ajouter une annotation à un mappage, appelez simplement le `AddAnnotation` méthode d’un `MKMapView` de l’instance, comme indiqué dans le code suivant :

```csharp
//an arbitrary coordinate used for demonstration here
var sampleCoordinate =
new CLLocationCoordinate2D (42.3467512, -71.0969456);

//create an annotation and add it to the map
map.AddAnnotation (new SampleMapAnnotation (sampleCoordinate));
```

La carte, la variable est une instance d’un `MKMapView`, qui est la classe qui représente le mappage lui-même. Le `MKMapView` utilisera le `Coordinate` données dérivées de la `SampleMapAnnotation` instance à la position de la vue de l’annotation sur la carte.

Le `MKAnnotation` protocole fournit un jeu connu de fonctionnalités sur tous les objets qui l’implémentent, sans le consommateur (la carte dans ce cas) avoir besoin de connaître les détails d’implémentation. Cela simplifie l’ajout d’une variété d’annotations possible à une carte.

 <a name="Protocols_Deep_Dive" />


### <a name="protocols-deep-dive"></a>Présentation approfondie des protocoles

Étant donné que les interfaces c# ne prennent pas en charge les méthodes facultatives, Xamarin.iOS mappe les protocoles pour les classes abstraites. Par conséquent, adoption d’un protocole dans Objective-C est effectuée Xamarin.iOS en dérivant de la classe abstraite qui est liée au protocole et en implémentant les méthodes requises. Ces méthodes seront exposées en tant que méthodes abstraites dans la classe. Les méthodes facultatives du protocole seront liés à des méthodes virtuelles de la classe c#.

Par exemple, voici une partie de la `UITableViewDataSource` protocole en tant que Xamarin.iOS liée dans :

```csharp
public abstract class UITableViewDataSource : NSObject
{
    [Export ("tableView:cellForRowAtIndexPath:")]
    public abstract UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath);
    [Export ("numberOfSectionsInTableView:")]
    public virtual int NumberOfSections (UITableView tableView){...}
...
}
```

Notez que la classe est abstraite. Xamarin.iOS rend la classe abstraite pour prendre en charge requis/facultatifs de méthodes dans les protocoles.
Toutefois, contrairement aux protocoles de Objective-C (ou interfaces c#), classes c# ne prend en charge l’héritage multiple. Cela affecte la conception de code c# qui utilise les protocoles et entraîne généralement des classes imbriquées. Plus sur ce problème est couvert plus loin dans ce document, dans la section de délégués.

 `GetCell(…)` est une méthode abstraite, liée à la Objective-C *sélecteur*, `tableView:cellForRowAtIndexPath:`, qui est une méthode requise de la `UITableViewDataSource` protocole. Sélecteur est le terme Objective-C pour le nom de la méthode. Pour appliquer la méthode comme étant obligatoire, Xamarin.iOS déclare comme abstract. L’autre méthode, `NumberOfSections(…)`, est lié à `numberOfSectionsInTableview:`. Cette méthode est facultative dans le protocole, Xamarin.iOS déclare comme virtual, rendant facultatif pour substituer en c#.

Xamarin.iOS prend en charge de tous les liaison iOS pour vous. Toutefois, si vous devez lier manuellement un protocole d’Objective-C, vous pouvez le faire par le fait de décorer une classe avec le `ExportAttribute`. Il s’agit de la même méthode utilisée par Xamarin.iOS lui-même.

Pour plus d’informations sur la façon de lier des types de Objective-C dans Xamarin.iOS, consultez l’article [Objective-C, les Types de liaison](~/ios/platform/binding-objective-c/index.md).

Nous ne sommes pas via avec des protocoles, cependant. Elles sont également utilisées dans iOS comme base pour les délégués Objective-C, qui est le sujet de la section suivante.

 <a name="Delegates" />


## <a name="delegates"></a>Délégués

iOS utilise Objective-C délégués pour implémenter le modèle de délégation, dans lequel un objet transmet travail vers un autre. L’objet qui effectue le travail est le délégué du premier objet. Un objet indique son délégué pour effectuer le travail en envoyant des que messages d’après que certains événements se produisent. Envoyer un message comme celui-ci dans Objective-C est fonctionnellement équivalente à l’appel d’une méthode en c#. Un délégué implémente des méthodes en réponse à ces appels et fournit ainsi une fonctionnalité à l’application.

Les délégués permettent de vous permettent d’étendre le comportement des classes sans avoir à créer des sous-classes. Les applications iOS utilisent souvent des délégués quand une classe rappelle à l’autre après une action importante. Par exemple, la `MKMapView` classe les appels vers son délégué lorsque l’utilisateur appuie sur une carte, d’une annotation offrant l’opportunité de répondre au sein de l’application à l’auteur de la classe déléguée. Vous pouvez utiliser un délégué avec Xamarin.iOS via un exemple de ce type d’utilisation de délégué plus loin dans cet article, à l’aide de l’exemple.

À ce stade, vous vous demandez peut-être comment une classe détermine les méthodes à appeler sur son délégué. Il s’agit d’un autre emplacement où vous utilisez des protocoles. En règle générale, les méthodes disponibles pour un délégué proviennent des protocoles qu’ils adoptent.

 <a name="How_Protocols_are_used_with_Delegates" />


### <a name="how-protocols-are-used-with-delegates"></a>Utilisation des protocoles avec des délégués

Nous avons vu plus haut de la façon dont les protocoles sont utilisés pour prendre en charge l’ajout d’annotations à un mappage.
Protocoles sont également utilisés pour fournir un jeu connu de méthodes pour les classes à appeler après certains événements se produisent, tels qu’après les pressions utilisateur une annotation sur une carte ou sélectionne une cellule dans une table. Les classes qui implémentent ces méthodes sont appelées sur les délégués des classes de les appellent.

Les classes qui prennent en charge la délégation faire en exposant une propriété de délégué, à laquelle est affectée une classe qui implémente le délégué. Les méthodes que vous implémentez pour le délégué varient en fonction du protocole que le délégué particulier adopte. Pour le `UITableView` (méthode), vous implémentez le `UITableViewDelegate` protocole, pour le `UIAccelerometer` (méthode), vous pouvez implémenter `UIAccelerometerDelegate`, et ainsi de suite pour toutes les autres classes dans l’ensemble d’iOS pour lequel vous souhaitez exposent un délégué.

Le `MKMapView` nous l’avons vu dans notre précédent exemple de classe a également une propriété appelée délégué, lequel il appelle une fois que certains événements se produisent. Le délégué de `MKMapView` est de type `MKMapViewDelegate`.
Vous allez l’utiliser peu de temps dans un exemple pour répondre à l’annotation qu’elle est sélectionnée, mais premier nous allons discuter de la différence entre les délégués de nom forts et faibles.

 <a name="Strong_Delegates_vs._Weak_Delegates" />


### <a name="strong-delegates-vs-weak-delegates"></a>Les délégués fort vs. Délégués faibles

Les délégués, que nous avons parlé jusqu'à présent sont délégués fort, c'est-à-dire qu’ils sont fortement typées. Les liaisons de Xamarin.iOS sont fournies avec une classe fortement typée pour chaque protocole délégué dans iOS. Toutefois, iOS a également le concept d’un délégué faible. Au lieu de sous-classement d’une classe liée au protocole Objective-C pour un délégué particulier, iOS vous permet également de choisir de lier les méthodes de protocole dans n’importe quelle classe que qui dérive NSObject, la décoration de vos méthodes avec ExportAttribute, puis en fournissant les sélecteurs appropriés.
Lorsque vous adoptez cette approche, vous affectez une instance de la classe à la propriété WeakDelegate plutôt qu’à la propriété de délégué. Un délégué faible vous offre la flexibilité nécessaire pour prendre votre classe délégué vers le bas d’une hiérarchie d’héritage différent. Examinons un exemple de Xamarin.iOS qui utilise des délégués fort et faibles.

 <a name="Example_using_a_Delegate_with_Xamarin.iOS" />


### <a name="example-using-a-delegate-with-xamarinios"></a>Exemple à l’aide d’un délégué avec Xamarin.iOS

Pour exécuter le code en réponse à l’utilisateur en appuyant sur l’annotation dans notre exemple, nous pouvons sous-classe `MKMapViewDelegate` et assignez une instance de la `MKMapView`de `Delegate` propriété. Le `MKMapViewDelegate` protocole contient uniquement les méthodes facultatives.
Par conséquent, toutes les méthodes sont virtuelles qui sont liés à ce protocole dans la Xamarin.iOS `MKMapViewDelegate` classe. Lorsque l’utilisateur sélectionne une annotation, le `MKMapView` instance enverra le `mapView:didSelectAnnotationView:` message à son délégué. Pour gérer cette situation dans Xamarin.iOS, nous devons remplacer le `DidSelectAnnotationView (MKMapView mapView, MKAnnotationView annotationView)` méthode dans la sous-classe MKMapViewDelegate comme suit :

```csharp
public class SampleMapDelegate : MKMapViewDelegate
{
    public override void DidSelectAnnotationView (
        MKMapView mapView, MKAnnotationView annotationView)
    {
        var sampleAnnotation =
            annotationView.Annotation as SampleMapAnnotation;

        if (sampleAnnotation != null) {

            //demo accessing the coordinate of the selected annotation to
            //zoom in on it
            mapView.Region = MKCoordinateRegion.FromDistance(
                sampleAnnotation.Coordinate, 500, 500);

            //demo accessing the title of the selected annotation
            Console.WriteLine ("{0} was tapped", sampleAnnotation.Title);
        }
    }
}
```

La classe SampleMapDelegate indiquée ci-dessus est implémentée comme une classe imbriquée dans le contrôleur qui contient la `MKMapView` instance. Dans Objective-C, affichent souvent le contrôleur adopter plusieurs protocoles directement au sein de la classe. Toutefois, étant donné que les protocoles liés aux classes de Xamarin.iOS, les classes qui implémentent des délégués fortement typés sont généralement inclus en tant que classes imbriquées.

Avec l’implémentation de classe de délégué en place, vous devez uniquement instancier une instance du délégué dans le contrôleur et l’affecter à la `MKMapView`de `Delegate` la propriété comme indiqué ici :

```csharp
public partial class Protocols_Delegates_EventsViewController : UIViewController
{
    SampleMapDelegate _mapDelegate;
    ...
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        //set the map's delegate
        _mapDelegate = new SampleMapDelegate ();
        map.Delegate = _mapDelegate;
        ...
    }
    class SampleMapDelegate : MKMapViewDelegate
    {
        ...
    }
}
```

Pour utiliser un délégué faible pour accomplir la même chose, vous devez lier la méthode dans une classe qui dérive de `NSObject` et assignez-la à la `WeakDelegate` propriété de la `MKMapView`. Étant donné que la `UIViewController` dérivent `NSObject` (par exemple, chaque classe Objective-C dans CocoaTouch), nous pouvons simplement implémenter une méthode liée à `mapView:didSelectAnnotationView:` directement dans le contrôleur et affecter le contrôleur de `MKMapView`de `WeakDelegate`, ce qui évite la nécessité de la classe imbriquée supplémentaire. Le code ci-dessous illustre cette approche :

```csharp
public partial class Protocols_Delegates_EventsViewController : UIViewController
{
    ...
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
        //assign the controller directly to the weak delegate
        map.WeakDelegate = this;
    }
    //bind to the Objective-C selector mapView:didSelectAnnotationView:
    [Export("mapView:didSelectAnnotationView:")]
    public void DidSelectAnnotationView (MKMapView mapView,
        MKAnnotationView annotationView)
    {
        ...
    }
}
```

Lorsque vous exécutez ce code, l’application se comporte exactement comme lors de l’exécution de la version fortement typée de délégué. L’avantage de ce code est que le délégué faible ne nécessite pas la création de la classe supplémentaire qui a été créée lorsque nous avons utilisé le délégué fortement typé. Toutefois, cela se fait au détriment de la sécurité de type. Si vous ne faites une erreur dans le sélecteur qui a été passé à la `ExportAttribute`, vous ne Découvrez jusqu'à l’exécution.

 <a name="Events_and_Delegates" />


### <a name="events-and-delegates"></a>Événements et délégués

Les délégués sont utilisés pour les rappels dans iOS de la même façon à la manière dont .NET utilise des événements. Pour rendre iOS API et la façon dont ils utilisent Objective-C délégués sembler plus .NET, Xamarin.iOS expose des événements .NET dans de nombreux endroits où les délégués sont utilisés dans iOS.

Par exemple, l’implémentation précédente où la `MKMapViewDelegate` a répondu à une annotation sélectionnée peut également être implémentée dans Xamarin.iOS à l’aide d’un événement .NET. Dans ce cas, l’événement serait définie dans `MKMapView` et appelé `DidSelectAnnotationView`. Cela aurait un `EventArgs` sous-classe du type `MKMapViewAnnotationEventsArgs`. Le `View` propriété du `MKMapViewAnnotationEventsArgs` permet d’obtenir une référence à la vue de l’annotation, à partir de ce qui peut procéder à la même implémentation vous aviez précédemment, comme illustré ici :

```csharp
map.DidSelectAnnotationView += (s,e) => {
    var sampleAnnotation = e.View.Annotation as SampleMapAnnotation;
    if (sampleAnnotation != null) {
        //demo accessing the coordinate of the selected annotation to
        //zoom in on it
        mapView.Region = MKCoordinateRegion.FromDistance (
            sampleAnnotation.Coordinate, 500, 500);

        //demo accessing the title of the selected annotation
        Console.WriteLine ("{0} was tapped", sampleAnnotation.Title);
    }
};
```

 <a name="Summary" />


## <a name="summary"></a>Récapitulatif

Cet article expliqué comment utiliser des délégués, des protocoles et des événements dans Xamarin.iOS. Nous avons vu comment Xamarin.iOS expose des événements de style .NET normales pour les contrôles.
Ensuite, nous avons appris sur les protocoles Objective-C, y compris comment ils sont différents d’interfaces c# et Xamarin.iOS leur utilisation. Enfin, nous avons examiné les délégués à partir d’un point de vue Xamarin.iOS Objective-C. Nous avons vu comment Xamarin.iOS prend en charge les deux fortement et faiblement typés délégués et comment lier des événements .NET pour déléguer des méthodes.


## <a name="related-links"></a>Liens associés

- [Protocoles, les délégués et événements (exemple)](https://developer.xamarin.com/samples/Protocols_Delegates_Events/)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Types de liaison Objective-C](~/ios/platform/binding-objective-c/index.md)
- [Le langage de programmation Objective-C](http://developer.apple.com/library/ios/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
- [Conception des Interfaces utilisateur dans Xcode 4](http://developer.apple.com/library/ios/#documentation/IDEs/Conceptual/Xcode4TransitionGuide/InterfaceBuilder/InterfaceBuilder.html)
- [Compétences en Application pour iOS](http://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html)
