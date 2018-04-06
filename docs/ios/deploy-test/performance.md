---
title: Niveau de performance de Xamarin.iOS
description: Il existe plusieurs techniques permettant d’accroître le niveau de performance des applications générées avec Xamarin.iOS. Utilisées conjointement, ces techniques peuvent considérablement réduire la charge de travail d’une UC, ainsi que la quantité de mémoire consommée par une application. Cet article aborde ces techniques.
ms.prod: xamarin
ms.assetid: 02b1f628-52d9-49de-8479-f2696546ca3f
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/29/2016
ms.openlocfilehash: 3fc6263aa99edb94ae69f1ce8f87835043477392
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinios-performance"></a>Niveau de performance de Xamarin.iOS

_Il existe plusieurs techniques permettant d’accroître le niveau de performance des applications générées avec Xamarin.iOS. Ensemble, ces techniques peuvent considérablement réduire la charge de travail d’un processeur, de même que la quantité de mémoire consommée par une application. Cet article décrit et explique ces techniques._

Le mauvais niveau de performance d’une application se présente de plusieurs façons. L’application semble ne pas répondre, le défilement de l’affichage est ralenti et la durée de vie de la batterie de l’appareil réduite. Toutefois, l’optimisation des performances implique davantage de choses que l’implémentation d’un code efficace. L’expérience utilisateur liée au niveau de performance de l’application doit également être prise en compte. Par exemple, pour contribuer à améliorer l’expérience utilisateur, vous devez vérifier que les opérations s’exécutent sans empêcher l’utilisateur d’effectuer d’autres activités.

Il existe un certain nombre de techniques permettant d’accroître le niveau de performance, ainsi que le niveau de performance perçu, des applications générées avec Xamarin.iOS. Elles comprennent :

- [Éviter les cycles de références fortes](#avoidcircularreferences)
- [Optimiser les affichages de tableaux](#optimizetableviews)
- [Utiliser les affichages opaques](#opaqueviews)
- [Éviter les XIB volumineux](#avoidfatxibs)
- [Optimiser les ressources d’images](#optimizeimages)
- [Tester sur les appareils](#testondevices)
- [Synchroniser les animations avec l’actualisation de l’affichage](#synchronizeanimations)
- [Évitez la transparence Core Animation](#avoidtransparency)
- [Éviter la génération de code](#avoidcodegeneration)

> [!NOTE]
> Avant de lire cet article, lisez d’abord [Niveau de performance multiplateforme](~/cross-platform/deploy-test/memory-perf-best-practices.md), qui décrit les techniques spécifiques indépendantes des plateformes qui permettent d’améliorer l’utilisation de la mémoire et le niveau de performance des applications générées à l’aide de la plateforme Xamarin.

<a name="avoidcircularreferences" />

## <a name="avoid-strong-circular-references"></a>Éviter les références circulaires fortes

Dans certaines situations, vous pouvez créer des cycles de références fortes qui peuvent empêcher le récupérateur de mémoire de récupérer la mémoire des objets. Par exemple, une sous-classe dérivée de [`NSObject`](https://developer.xamarin.com/api/type/Foundation.NSObject/), c’est-à-dire une classe qui hérite de [`UIView`](https://developer.xamarin.com/api/type/UIKit.UIView/), est ajoutée à un conteneur dérivé de `NSObject` et est fortement référencée à partir d’Objective-C, comme le montre l’exemple de code suivant :

```csharp
class Container : UIView
{
    public void Poke ()
    {
    // Call this method to poke this object
    }
}

class MyView : UIView
{
    Container parent;
    public MyView (Container parent)
    {
        this.parent = parent;
    }

    void PokeParent ()
    {
        parent.Poke ();
    }
}

var container = new Container ();
container.AddSubview (new MyView (container));
```

Quand ce code crée l’instance de `Container`, l’objet C# a une référence forte à un objet Objective-C. De même, l’instance de `MyView` a également une référence forte à un objet Objective-C.

De plus, l’appel à `container.AddSubview` augmente le nombre de références sur l’instance non managée de `MyView`. Quand cela se produit, le runtime Xamarin.iOS crée une instance de `GCHandle` pour garder l’objet `MyView` actif dans le code managé, car il n’existe aucune garantie que les objets managés vont continuer à le référencer. Du point de vue du code managé, l’objet `MyView` serait récupéré après l’appel de [`AddSubview`](https://developer.xamarin.com/api/member/UIKit.UIView.AddSubview/p/UIKit.UIView/), s’il n’y avait pas `GCHandle`.

L’objet `MyView` non managé a un `GCHandle` qui pointe vers l’objet managé. Cela s’appelle un *lien fort*. L’objet managé contient une référence à l’instance de `Container`. À son tour, l’instance de `Container` a une référence managée à l’objet `MyView`.

Dans les cas où un objet contenu conserve un lien vers son conteneur, plusieurs options sont disponibles pour traiter la référence circulaire :

-  Rompez manuellement le cycle en affectant au lien vers le conteneur la valeur `null`.
-  Supprimez manuellement l’objet contenu du conteneur.
-  Appelez `Dispose` sur les objets.
-  Évitez la référence circulaire en gardant une référence faible au conteneur. Pour plus d’informations sur les références faibles.

### <a name="using-weakreferences"></a>Utilisation de WeakReferences

Pour empêcher un cycle, vous pouvez utiliser une référence faible de l’enfant au parent. Par exemple, vous pouvez écrire le code ci-dessus comme ceci :

```csharp
class Container : UIView
{
    public void Poke ()
    {
    // Call this method to poke this object
    }
}

class MyView : UIView
{
    WeakReference<Container> weakParent;
    public MyView (Container parent)
    {
        this.weakParent = new WeakReference<Container> (parent);
    }

    void PokeParent ()
    {
        if (weakParent.TryGetTarget (out var parent))
            parent.Poke ();
    }
}

var container = new Container ();
container.AddSubview (new MyView (container));
```

Cela signifie que l’objet contenu ne garde pas le parent actif. Seul le parent garde l’enfant actif grâce à l’appel de `container.AddSubView`.

Cela se produit également dans les API iOS qui utilisent le modèle de délégué ou de source de données, où une classe homologue contient l’implémentation, par exemple quand vous définissez la propriété [`Delegate`](https://developer.xamarin.com/api/property/MonoTouch.UIKit.UITableView.Delegate/) ou [`DataSource`](https://developer.xamarin.com/api/property/MonoTouch.UIKit.UITableView.DataSource/) dans la classe [`UITableView`](https://developer.xamarin.com/api/type/UIKit.UITableView/).

Dans le cas de classes créées uniquement pour l’implémentation d’un protocole, par exemple [`IUITableViewDataSource`](https://developer.xamarin.com/api/type/MonoTouch.UIKit.IUITableViewDataSource/), au lieu de créer une sous-classe, vous pouvez implémenter simplement l’interface dans la classe et remplacer la méthode, puis affecter à la propriété `DataSource` la valeur `this`.

### <a name="disposing-of-objects-with-strong-references"></a>Suppression d’objets avec des références fortes

Si une référence forte existe et qu’il est difficile de supprimer la dépendance, utilisez une méthode `Dispose` pour effacer le pointeur parent.

Pour les conteneurs, remplacez la méthode `Dispose` afin de supprimer les objets contenus, comme indiqué dans l’exemple de code suivant :

```csharp
class MyContainer : UIView
{
    public override void Dispose ()
    {
        // Brute force, remove everything
        foreach (var view in Subviews)
        {
              view.RemoveFromSuperview ();
        }
        base.Dispose ();
    }
}
```

Pour un objet enfant qui conserve une référence forte à son parent, effacez la référence au parent dans l’implémentation de `Dispose` :

```csharp
    class MyChild : UIView {
    MyContainer container;
    public MyChild (MyContainer container)
    {
        this.container = container;
    }
    public override void Dispose ()
    {
        container = null;
    }
}
```

Pour plus d’informations sur la libération de références fortes, consultez [Libérer des ressources IDisposable](~/cross-platform/deploy-test/memory-perf-best-practices.md#idisposable).
Il existe également des informations intéressantes dans ce billet de blog : [Xamarin.iOS, the garbage collector and me](http://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/).

### <a name="more-information"></a>Informations complémentaires

Pour plus d’informations, consultez [Rules to Avoid Retain Cycles](http://www.cocoawithlove.com/2009/07/rules-to-avoid-retain-cycles.html) sur Cocoa With Love, [Is this a bug in MonoTouch GC](http://stackoverflow.com/questions/13058521/is-this-a-bug-in-monotouch-gc) sur StackOverflow, et [Why can’t MonoTouch GC kill managed objects with refcount > 1?](http://stackoverflow.com/questions/13064669/why-cant-monotouch-gc-kill-managed-objects-with-refcount-1) sur StackOverflow.


<a name="optimizetableviews" />

## <a name="optimize-table-views"></a>Optimiser les affichages de tableaux

Les utilisateurs attendent un défilement fluide et des temps de chargement rapides pour les instances de [`UITableView`](https://developer.xamarin.com/api/type/UIKit.UITableView/). Toutefois, le niveau de performance du défilement peut se dégrader quand les cellules contiennent des hiérarchies d’affichages profondément imbriquées, ou quand elles contiennent des dispositions complexes. Toutefois, vous pouvez utiliser certaines techniques pour éviter une dégradation du niveau de performance de `UITableView` :

- Réutilisez les cellules. Pour plus d’informations, consultez [Réutiliser les cellules](#reusecells).
- Réduisez le nombre de sous-affichages.
- Mettez en cache le contenu de cellule récupéré à partir d’un service web.
- Mettez en cache la hauteur des lignes, si elles ne sont pas identiques.
- Rendez la cellule et les autres affichages opaques.
- Évitez la mise à l’échelle et les dégradés d’images.

Utilisées conjointement, ces techniques peuvent vous aider à conserver la fluidité du défilement des instances de [`UITableView`](https://developer.xamarin.com/api/type/UIKit.UITableView/).

<a name="reusecells" />

### <a name="reuse-cells"></a>Réutiliser les cellules

Quand des centaines de lignes s’affichent dans [`UITableView`](https://developer.xamarin.com/api/type/UIKit.UITableView/), ne gaspillez pas de la mémoire en créant des centaines d’objets [`UITableViewCell`](https://developer.xamarin.com/api/type/UIKit.UITableViewCell/), alors qu’un petit nombre d’entre eux s’affichent en même temps à l’écran. À la place, seules les cellules visibles à l’écran peuvent être chargées en mémoire, le **contenu** étant chargé dans les cellules réutilisées. Cela empêche l’instanciation de centaines d’objets supplémentaires, ce qui se traduit par un gain de temps et une économie de mémoire.

Ainsi, quand une cellule disparaît de l’écran, son affichage peut être placé en file d’attente de réutilisation, comme indiqué dans l’exemple de code suivant :

```csharp
class MyTableSource : UITableViewSource
{
    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
    {
        // iOS will create a cell automatically if one isn't available in the reuse pool
        var cell = (MyCell) tableView.DequeueReusableCell (MyCellId, indexPath);

        // Perform required cell actions
        return cell;
    }
}
```

Quand l’utilisateur fait défiler ce qu’il voit, [`UITableView`](https://developer.xamarin.com/api/type/UIKit.UITableView/) appelle la substitution `GetCell` pour demander la présentation de nouveaux affichages. Cette substitution appelle ensuite la méthode [`DequeueReusableCell`](https://developer.xamarin.com/api/member/UIKit.UITableView.DequeueReusableCell/p/Foundation.NSString/). Si une cellule est disponible pour être réutilisée, elle est retournée.

Pour plus d’informations, consultez [Réutilisation des cellules](~/ios/user-interface/controls/tables/populating-a-table-with-data.md) dans [Remplissage d’un tableau avec des données](~/ios/user-interface/controls/tables/populating-a-table-with-data.md).

<a name="opaqueviews" />

## <a name="use-opaque-views"></a>Utiliser les affichages opaques

Vérifiez que la propriété [`Opaque`](https://developer.xamarin.com/api/property/UIKit.UIView.Opaque/) est définie pour les affichages qui n’ont pas de transparence. Cela permet de garantir le rendu optimal des affichages par le système de dessin. Cet aspect est particulièrement important quand un affichage est incorporé dans [`UIScrollView`](https://developer.xamarin.com/api/type/UIKit.UIScrollView/) ou fait partie d’une animation complexe. Sinon, le système de dessin effectue une composition des affichages avec d’autres contenus, ce qui peut impacter le niveau de performance de manière importante.

<a name="avoidfatxibs" />

## <a name="avoid-fat-xibs"></a>Éviter les XIB volumineux

Bien que les XIB aient été largement remplacés par les storyboards, il existe certaines situations où vous pouvez encore utiliser des XIB. Quand un XIB est chargé en mémoire, tout son contenu est chargé en mémoire, notamment les images. Si le XIB contient un affichage qui n’est pas immédiatement utilisé, la mémoire est gaspillée. Ainsi, quand vous utilisez un XIB, vérifiez qu’il n’existe qu’un seul XIB par contrôleur d’affichage et, si possible, séparez la hiérarchie d’affichages du contrôleur d’affichage dans des XIB distincts.

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>Optimiser les ressources d’images

Les images font partie des ressources les plus lourdes utilisées par les applications, et sont souvent capturées à des résolutions élevées. Ainsi, quand vous affichez une image provenant du bundle de l’application dans [`UIImageView`](https://developer.xamarin.com/api/type/UIKit.UIImageView/), vérifiez que l’image et `UIImageView` sont de taille identique. La mise à l’échelle des images au moment de l’exécution peut être une opération lourde, en particulier si `UIImageView` est incorporé dans [`UIScrollView`](https://developer.xamarin.com/api/type/UIKit.UIScrollView/).

Pour plus d’informations, consultez [Optimiser les ressources d’images](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages) dans le guide [Niveau de performance multiplateforme](~/cross-platform/deploy-test/memory-perf-best-practices.md).

<a name="testondevices" />

## <a name="test-on-devices"></a>Tester sur les appareils

Commencez à déployer et à tester votre application sur un appareil physique le plus tôt possible. Les simulateurs ne reproduisent pas parfaitement les comportements et les limitations des appareils. Il est donc important d’effectuer des tests sur des appareils réels, dès que possible.

En particulier, le simulateur ne peut pas simuler les restrictions de mémoire ou les limitations de l’UC d’un appareil physique.

<a name="synchronizeanimations" />

## <a name="synchronize-animations-with-the-display-refresh"></a>Synchroniser les animations avec l’actualisation de l’affichage

Les jeux ont tendance à avoir des boucles étroites pour l’exécution de la logique du jeu et l’actualisation de l’écran. Les fréquences d’images classiques vont de trente à soixante images par seconde. Certains développeurs estiment qu’ils doivent mettre à jour l’écran autant de fois que possible par seconde, en combinant leur simulation de jeu avec des mises à jour de l’écran. Ils sont donc parfois tentés d’aller au-delà de soixante images par seconde.

Toutefois, le serveur d’affichage effectue des mises à jour de l’écran soixante fois par seconde, ce qui correspond à la limite maximale. Ainsi, toute tentative d’actualisation de l’écran qui dépasse cette limite peut entraîner des interruptions et des micro-saccades. Il est préférable de structurer le code pour que les actualisations de l’écran soient synchronisées avec l’actualisation de l’affichage. Pour ce faire, utilisez la classe [`CoreAnimation.CADisplayLink`](https://developer.xamarin.com/api/type/CoreAnimation.CADisplayLink/), qui représente un minuteur approprié aux tâches de visualisation et aux jeux s’exécutant à soixante images par seconde.

<a name="avoidtransparency" />

## <a name="avoid-core-animation-transparency"></a>Évitez la transparence Core Animation

Le fait d’éviter la transparence Core Animation permet d’améliorer le niveau de performance de la composition bitmap. En règle générale, évitez les calques transparents et les bordures floues, si possible.

<a name="avoidcodegeneration" />

## <a name="avoid-code-generation"></a>Éviter la génération de code

Vous devez éviter de générer du code de manière dynamique avec `System.Reflection.Emit` ou à l’aide du *Dynamic Language Runtime*, car le noyau iOS empêche l’exécution du code dynamique.

## <a name="summary"></a>Récapitulatif

Cet article a décrit et expliqué les techniques permettant d’accroître le niveau de performance des applications générées avec Xamarin.iOS. Ensemble, ces techniques peuvent considérablement réduire la charge de travail d’un processeur, de même que la quantité de mémoire consommée par une application.

## <a name="related-links"></a>Liens associés

- [Performances entre plateformes](~/cross-platform/deploy-test/memory-perf-best-practices.md)
