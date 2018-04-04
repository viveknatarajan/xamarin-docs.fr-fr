---
title: Utilisation de l’affichage des contrôleurs de fractionnement
description: Cet article décrit la conception et l’utilisation de contrôleurs d’affichage fractionné à l’intérieur d’une application Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: 21248CFB-5A94-4C19-B223-C72E0DC5F1D5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 25151eb2929e2bc61dba27a9937ffdf4ee224626
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-split-view-controllers"></a>Utilisation de l’affichage des contrôleurs de fractionnement

_Cet article décrit la conception et l’utilisation de contrôleurs d’affichage fractionné à l’intérieur d’une application Xamarin.tvOS._


Un contrôleur d’affichage fractionné présente et gère un maître et détail-View-Controller-côte, à l’écran en même temps. Affichage des contrôleurs de fractionnement utilisés pour présenter le contenu persistant, peut être actif dans la vue principale (la plus petite section sur la gauche) et liés détails dans la vue de détail (la section supérieure droite).

[![](split-views-images/intro01.png "Exemple de fractionnement")](split-views-images/intro01.png#lightbox)

<a name="About-Split-View-Controllers" />

## <a name="about-split-view-controllers"></a>Sur les contrôleurs de mode fractionné

Comme indiqué ci-dessus, un fractionnement View Controller gère un maître et un contrôleur de vue de détail qui figurent côte à côte avec le maître en cours de la vue la plus petite sur la gauche, le détail le plus grand sur la droite. 

En outre, le peut Master-View-Controller été masquée ou affichée selon les besoins : 

[![](split-views-images/intro02.png "Le contrôleur de vue masqué")](split-views-images/intro02.png#lightbox)

Fractionnement des vues contrôleurs sont utilisent souvent pour présenter une liste du contenu filtrable, avec les catégories dans la vue principale et les résultats filtrés dans la vue détaillée. Cela est généralement présenté comme une vue de Table sur la gauche et un [vue de Collection](~/ios/tvos/user-interface/collection-views.md) sur la droite.

Lorsque vous concevez une Interface utilisateur qui nécessite un contrôleur de vue de fractionnement, Apple suggère à l’aide de Master et les contrôleurs de la vue Détails qui ne changent pas (uniquement les modifications de contenu, pas la structure). Si vous n’avez pas besoin de permutation contrôleurs de la vue, il est préférable d’utiliser un contrôleur de Navigation comme base de la vue contrôleur qui a besoin de modifier (principale ou détail).

Apple a les suggestions suivantes pour travailler avec des contrôleurs de vue de fractionnement :

- **Utilisez le pourcentage de fractionnement Correct** -par défaut le contrôleur d’affichage fractionné utilise un tiers de l’écran pour l’affichage du contrôleur et les deux tiers pour le contrôleur de vue de détail. Si vous le souhaitez, vous pouvez utiliser un fractionnement 50/50. Choisissez le pourcentage correct pour afficher votre contenu équilibrée à l’écran.
- **Conserver la sélection principale** - pendant que le contenu sur le peuvent d’affichage des détails modification est la réponse à la sélection d’un utilisateur dans la vue principale, le contenu de la principale doit être corrigé. En outre, vous devez afficher clairement l’élément actuellement sélectionné dans la vue principale.
- **Utiliser un seul, unifiée de titre** -en général, que vous souhaitez utiliser un nom unique, centré dans la vue détail, au lieu d’un titre dans le détail et la vue principale.

<a name="Split-View-Controllers-and-Storyboards" />

## <a name="split-view-controllers-and-storyboards"></a>Affichage des contrôleurs de fractionnement et les animations

Pour travailler avec les contrôleurs d’affichage fractionné dans une application Xamarin.tvOS, le plus simple consiste à ajouter à l’interface utilisateur de l’application d’utiliser le concepteur iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Dans le **Solution remplissage**, double-cliquez sur le `Main.storyboard` de fichier et l’ouvre pour modification.
1. Faites glisser un **affichage des contrôleurs de fractionnement** à partir de la **boîte à outils** et déposez-la sur la vue : 

    [![](split-views-images/activity01.png "Un contrôleur d’affichage fractionné")](split-views-images/activity01.png#lightbox)
1. Par défaut, le concepteur iOS installera un contrôleur de Navigation et un contrôleur de la vue dans la vue principale. Si cela ne tient pas les exigences de votre application, simplement les supprimer.
1. Si vous ne supprimez pas la valeur par défaut le mode Masque, faites glisser un nouveau contrôleur de la vue sur l’aire de conception : 

    [![](split-views-images/activity02.png "Un contrôleur de vue")](split-views-images/activity02.png#lightbox)
1. La touche CTRL enfoncée et faites glisser à partir du contrôleur de mode de fractionnement vers le nouveau contrôleur de la vue principale. 
1. Sélectionnez **Master** à partir de la **Menu contextuel**: 

    [![](split-views-images/activity03.png "Sélectionnez maître dans le Menu contextuel")](split-views-images/activity03.png#lightbox)
1. Le contenu de votre principale et les vues de détail de la conception : 

    [![](split-views-images/activity04.png "Exemple de disposition")](split-views-images/activity04.png#lightbox)
1. Affecter **noms** dans les **onglet Widget** de la **propriétés remplissage** pour travailler avec les contrôles d’IU dans le code c#.
1. Enregistrer vos modifications et revenir à Visual Studio pour Mac.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` de fichier et l’ouvre pour modification.
1. Faites glisser un **affichage des contrôleurs de fractionnement** à partir de la **boîte à outils** et déposez-la sur la vue : 

    [![](split-views-images/activity01-vs.png "Un contrôleur d’affichage fractionné")](split-views-images/activity01-vs.png#lightbox)
1. Par défaut, le concepteur iOS ajoutera un contrôleur de Navigation et d’un contrôleur d’affichage dans la vue principale. Si cela ne tient pas les exigences de votre application, simplement les supprimer.
1. Si vous ne supprimez pas la valeur par défaut le mode Masque, faites glisser un nouveau contrôleur de la vue sur l’aire de conception : 

    [![](split-views-images/activity02-vs.png "Un contrôleur de vue")](split-views-images/activity02-vs.png#lightbox)
1. La touche CTRL enfoncée et faites glisser à partir du contrôleur de mode de fractionnement vers le nouveau contrôleur de la vue principale. 
1. Sélectionnez **Master** à partir de la **Menu contextuel**: 

    [![](split-views-images/activity03-vs.png "Sélectionnez maître dans le Menu contextuel")](split-views-images/activity03-vs.png#lightbox)
1. Le contenu de votre principale et les vues de détail de la conception : 

    [![](split-views-images/activity04.png "Disposition du contenu")](split-views-images/activity04.png#lightbox)
1. Affecter **noms** dans les **onglet Widget** de la **l’Explorateur de propriétés** pour travailler avec les contrôles d’IU dans le code c#.
1. Enregistrez les modifications apportées.
    
-----

Pour plus d’informations sur l’utilisation des plans conceptuels, veuillez consulter notre [Hello, Guide de démarrage rapide de tvOS](~/ios/tvos/get-started/hello-tvos.md).

<a name="Working-with-Split-View-Controllers" />

## <a name="working-with-split-view-controllers"></a>Utilisation de l’affichage des contrôleurs de fractionnement

Comme indiqué ci-dessus, un contrôleur d’affichage fractionné est souvent utilisé dans les situations où vous affichez le contenu filtré à l’utilisateur. Les principales catégories sont affichés sur la gauche dans la vue principale, et les résultats filtrés sur la droite dans le volet Affichage détails en fonction de sélection de l’utilisateur.

<a name="Accessing-Master-and-Detail" />

### <a name="accessing-master-and-detail"></a>L’accès à la principale et détaillée

Si vous avez besoin d’accéder par programme les Master et contrôleurs de la vue détail, utilisez le `ViewControllers ` propriété du contrôleur affichage fractionné. Par exemple :

```csharp
// Gain access to master and detail view controllers
var masterController = ViewControllers [0] as MasterViewController;
var detailController = ViewControllers [1] as DetailViewController;
```

Il est présenté sous forme de tableau, où le premier élément (0) dans la vue du contrôleur et le deuxième élément (1) sont le détail.

<a name="Accessing-Detail-from-Master" />

### <a name="accessing-detail-from-master"></a>L’accès à des détails à partir du maître

Étant donné que vous affichez généralement des informations détaillées dans la vue de détail en fonction de sélection de l’utilisateur dans le Master, vous aurez besoin d’un moyen pour accéder aux détails du serveur maître.

Pour ce faire, le plus simple consiste à exposer une propriété dans votre classe de vue du contrôleur, par exemple :

```csharp
public DetailViewController DetailController { get; set;}
```

Dans le contrôleur d’affichage fractionné substituer le `ViewDidLoad` (méthode) et attache les deux vues d’ensemble. Par exemple :

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Gain access to master and detail view controllers
    var masterController = ViewControllers [0] as MasterViewController;
    var detailController = ViewControllers [1] as DetailViewController;

    // Wire-up views
    masterController.SplitViewController = this;
    masterController.DetailController = detailController;
    detailController.SplitViewController = this;
}
```

Vous pouvez exposer des propriétés et des méthodes sur votre contrôleur de vue de détail que le maître peut utiliser pour présenter les nouvelles données en fonction des besoins.

<a name="Showing-and-Hiding-Master" />

### <a name="showing-and-hiding-master"></a>Affichage et masquage de Master

Si vous le souhaitez, vous pouvez afficher ou masquer la vue du contrôleur à l’aide de la `PreferredDisplayMode` propriété du contrôleur affichage fractionné. Par exemple :

```csharp
// Show hide split view
if (SplitViewController.DisplayMode == UISplitViewControllerDisplayMode.PrimaryHidden) {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.AllVisible;
} else {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.PrimaryHidden;
}
```

Le `UISplitViewControllerDisplayMode` enum définit comment l’affichage du contrôleur s’affiche comme l’une des opérations suivantes :

- **Automatique** -tvOS contrôle la présentation de Master et des vues détaillées.
- **PrimaryHidden** -cela masque la vue du contrôleur.
- **AllVisible** -affiche le masque et l’affichage des détails contrôleurs côte à côte. Il s’agit de la présentation par défaut normal.
- **PrimaryOverlay** -le contrôleur de la vue détail s’étend sous et n’est couverte par le maître.

Pour obtenir l’état actuel de la présentation, utilisez la `DisplayMode` propriété du contrôleur affichage fractionné.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a couvert la conception et l’utilisation de contrôleurs d’affichage fractionné à l’intérieur d’une application Xamarin.tvOS.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS les Guides Interface humaine](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
