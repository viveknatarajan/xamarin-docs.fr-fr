---
title: Utilisation des vues d’empilées tvOS dans Xamarin
description: Ce document décrit comment vous le connaissiez tvOS vues empilées dans une application conçue avec Xamarin. Il fournit une vue d’ensemble de vues empilées et décrit la disposition automatique, positionnement et dimensionnement d’un affichage empilé, common utilise, l’intégration avec des Storyboards et bien plus encore.
ms.prod: xamarin
ms.assetid: 00B07F85-F30B-4DD4-8664-A61D0A1CDB0E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: f51ed3d6dbbfc8a7e430c2949485838a7471e545
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61161302"
---
# <a name="working-with-tvos-stacked-views-in-xamarin"></a>Utilisation des vues d’empilées tvOS dans Xamarin

Le contrôle de la vue de la pile (`UIStackView`) tire parti de la puissance de disposition automatique et des Classes de taille pour gérer une pile de sous-vues, horizontalement ou verticalement, qui répond dynamiquement aux modifications de contenu et de la taille d’écran de l’appareil Apple TV.

La disposition de tous les sous-vues attachée à une vue de la pile sont gérées par en fonction des propriétés de développeur défini comme axe, distribution, alignement et espacement :

[![](stacked-views-images/stacked01.png "Diagramme de disposition sous-affichage")](stacked-views-images/stacked01.png#lightbox)

Lorsque vous utilisez un `UIStackView` dans une application Xamarin.tvOS, le développeur peut définir soit les sous-vues soit à l’intérieur d’une table de montage séquentiel dans le concepteur iOS, ou en ajoutant et supprimant des sous-vues dans C# code.

## <a name="about-stacked-view-controls"></a>À propos des contrôles d’affichage empilé 

Le `UIStackView` est conçu comme une vue non rendu conteneur et par conséquent, il n’est pas dessiné dans le canevas, comme d’autres sous-classes de `UIView`. Définition des propriétés telles que `BackgroundColor` ou la substitution `DrawRect` n’a aucun effet visuel.

Il existe plusieurs propriétés qui contrôlent la façon dont une vue de la pile réorganiser sa collection de sous-vues :

- **Axe** – détermine si la vue pile organise les sous-vues soit **horizontalement** ou **verticalement**.
- **Alignement** – contrôle la manière dont les sous-affichages sont alignées dans la vue de la pile.
- **Distribution** – contrôle la façon dont les sous-affichages sont dimensionnés au sein de la vue de la pile.
- **Espacement** – contrôle l’espace minimal entre chaque sous-affichage dans la vue de la pile.
- **Ligne de base relatif** : si `true`, l’espacement vertical de chaque sous-affichage sera dérivé de sa ligne de base.
- **Mise en page marges relatif** – place les sous-vues relatif aux marges mise en forme standard.

En règle générale, vous allez utiliser une vue de la pile pour organiser un petit nombre de sous-affichages. Les Interfaces utilisateur plus complexes peuvent être créés par l’imbrication d’une ou plusieurs vues de pile à l’intérieur de l’autre.

Vous pouvez affiner l’apparence des interfaces utilisateur en ajoutant des contraintes supplémentaires pour les sous-affichages (par exemple pour le contrôle de la largeur ou hauteur). Toutefois, il convient ne pas à inclure les contraintes en conflit à celles introduites par la vue pile lui-même.

<a name="Auto-Layout-and-Size-Classes" />

## <a name="auto-layout-and-size-classes"></a>Classes de taille et de disposition automatique

Lorsqu’un sous-affichage est ajouté à une vue de la pile sa disposition est entièrement contrôlée par cette vue de la pile à l’aide des Classes de taille et de disposition automatique pour positionner et dimensionner les vues réorganisées.

La vue de la pile sera _code confidentiel_ le premier et dernier sous-affichage dans sa collection pour le **haut** et **bas** bords pour les vues de pile verticale ou **gauche**et **droite** bords pour les vues de pile horizontale. Si vous définissez la `LayoutMarginsRelativeArrangement` propriété `true`, puis la vue épingle les sous-affichages entre les marges pertinentes au lieu de la périphérie.

La vue pile utilise le sous-affichage `IntrinsicContentSize` propriété lors du calcul de la taille de sous-vues le long de l’élément défini `Axis` (à l’exception de la `FillEqually Distribution`). Le `FillEqually Distribution` redimensionne tous les sous-vues afin qu’ils soient de la même taille, donc remplissant la vue pile le long de la `Axis`.

À l’exception de la `Fill Alignment`, la vue pile utilise le sous-affichage `IntrinsicContentSize` propriété pour le calcul de taille de la vue perpendiculaire à la donnée `Axis`. Pour le `Fill Alignment`, tous les sous-affichages sont dimensionnés pour qu’elles remplissent la vue pile perpendiculaire à la donnée `Axis`.

<a name="Positioning-and-Sizing-the-Stack-View" />

## <a name="positioning-and-sizing-the-stack-view"></a>Positionnement et dimensionnement de la vue de la pile

Alors que la vue de la pile a un contrôle total sur la disposition de n’importe quel sous-affichage (selon les propriétés telles que `Axis` et `Distribution`), vous devez toujours positionner la vue de la pile (`UIStackView`) au sein de la vue parent à l’aide des Classes de taille et de disposition automatique.

En règle générale, cela signifie que l’épinglage au moins deux bords de l’affichage de la pile pour être accrus ou réduits, définissant ainsi sa position. Sans contraintes supplémentaires, la vue de la pile sont automatiquement redimensionnée pour s’ajuster à tous ses sous-vues comme suit :

* La taille le long de son `Axis` sera la somme de toutes les tailles de sous-affichage et n’importe quel espace qui a été défini entre chaque sous-affichage.
* Si le `LayoutMarginsRelativeArrangement` propriété est `true`, la taille de pile vues inclut également de place pour les marges.
* La taille perpendiculaire à la `Axis` a la valeur la plus grande sous-affichage dans la collection.

En outre, vous pouvez spécifier des contraintes pour la vue pile **hauteur** et **largeur**. Dans ce cas, les sous-affichages sont disposées (taille) pour remplir l’espace spécifié par la vue pile, comme déterminé par le `Distribution` et `Alignment` propriétés.

Si le `BaselineRelativeArrangement` propriété est `true`, les sous-affichages sont disposées en fonction de la ligne de base du sous-affichage premier ou dernier, au lieu d’utiliser le **haut**, **bas** ou **Center* -  **Y** position. Ceux-ci sont calculées sur le contenu de la vue pile comme suit :

* Une vue de la pile verticale renvoie le premier sous-affichage pour la première ligne de base et le dernier pour la dernière. Si une de ces sous-vues sont eux-mêmes des vues de la pile, puis leur ligne de base du premier ou dernier servira.
* Un affichage Horizontal de la pile utilisera sa plus grand sous-affichage pour les deux la première et la dernière ligne de base. Si la vue la plus haute est également une vue de la pile, il utilisera sa plus grand sous-affichage en tant que la ligne de base.

> [!IMPORTANT]
> Alignement de ligne de base ne fonctionne pas sur les tailles de sous-affichage étirée ou compressée que la ligne de base correspond à une position incorrecte. Pour l’alignement de ligne de base, vérifiez que le sous-affichage **hauteur** correspond à la vue contenu intrinsèque **hauteur**.




<a name="Common-Stack-View-Uses" />

## <a name="common-stack-view-uses"></a>Utilisations courantes de vue de pile

Il existe plusieurs types de disposition qui fonctionnent correctement avec les contrôles d’affichage de la pile. En fonction d’Apple, voici quelques-unes des utilisations plus courantes :

- **Définir la taille le long de l’axe** – en épinglant les deux bords le long de la vue pile `Axis` et les bords adjacents pour définir la position, la pile vue augmentera le long de l’axe en fonction de l’espace défini par ses sous-vues.
*  **Définir Position du sous-affichage** – en épinglant à bords adjacents de la vue de la pile à la vue de son parent, la vue pile augmentera dans les deux dimensions en fonction de son conteneur sous-vues.
- **Définir la taille et la Position de la pile** – en épinglant toutes les quatre bords de la vue de la pile à la vue parent, la vue pile organise les sous-vues en fonction de l’espace défini dans la vue de la pile.
*  **Définir la taille perpendiculaires l’axe** – en épinglant les deux perpendiculairement bords à la vue pile `Axis` et l’autre des bords sur l’axe pour définir la position, la pile vue augmentera perpendiculaire à l’axe en fonction de l’espace défini par ses sous-vues.

<a name="Stack-Views-and-Storyboards" />

## <a name="stack-views-and-storyboards"></a>Vues de la pile et Storyboards

Pour utiliser des vues de la pile dans une application Xamarin.tvOS le plus simple consiste à les ajouter à l’interface utilisateur de l’application à l’aide du concepteur iOS.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

1. Dans le **panneau solutions**, en double-cliquant sur le `Main.storyboard` de fichier et l’ouvrir pour modification.
1. Concevoir la disposition de vos éléments individuels que vous vous apprêtez à ajouter à la vue de la pile : 

    [![](stacked-views-images/layout01.png "Exemple de disposition d’élément")](stacked-views-images/layout01.png#lightbox)
1. Ajoutez toutes les contraintes requises pour les éléments pour vous assurer qu’ils l’échelle correctement. Cette étape est importante, une fois que l’élément est ajouté à la vue de la pile.
1. Vérifiez le nombre de copies (quatre dans ce cas) requis : 

    [![](stacked-views-images/layout02.png "Le nombre de copies requis")](stacked-views-images/layout02.png#lightbox)
1. Faites glisser un **vue pile** à partir de la **boîte à outils** et déposez-la sur la vue : 

    [![](stacked-views-images/layout03.png "Une vue de la pile")](stacked-views-images/layout03.png#lightbox)
1. Sélectionnez la vue de la pile, dans le **onglet Widget** de la **panneau Propriétés** sélectionnez **remplir** pour le **alignement**, **remplir Tout aussi** pour le **Distribution** et entrez `25` pour le **espacement**: 

    [![](stacked-views-images/layout04.png "L’onglet de Widget")](stacked-views-images/layout04.png#lightbox)
1. Positionner la vue de la pile sur l’écran où vous le souhaitez et ajoutez des contraintes pour le conserver dans l’emplacement requis.
1. Sélectionnez les éléments individuels et faites-les glisser dans la vue de la pile : 

    [![](stacked-views-images/layout05.png "Les éléments individuels dans la vue de la pile")](stacked-views-images/layout05.png#lightbox)
1. La mise en page soient ajustée et les éléments seront disposés dans la vue pile basée sur les attributs que vous avez précédemment défini.
1. Affecter **noms** dans le **onglet Widget** de la **l’Explorateur de propriétés** pour travailler avec les contrôles d’IU dans C# code.
1. Enregistrez les modifications apportées.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Dans le **l’Explorateur de solutions**, en double-cliquant sur le `Main.storyboard` de fichier et l’ouvrir pour modification.
1. Concevoir la disposition de vos éléments individuels que vous vous apprêtez à ajouter à la vue de la pile : 

    [![](stacked-views-images/layout01.png "Exemple de disposition élément")](stacked-views-images/layout01.png#lightbox)
1. Ajoutez toutes les contraintes requises pour les éléments pour vous assurer qu’ils l’échelle correctement. Cette étape est importante, une fois que l’élément est ajouté à la vue de la pile.
1. Vérifiez le nombre de copies (quatre dans ce cas) requis : 

    [![](stacked-views-images/layout02.png "Le nombre de copies requis")](stacked-views-images/layout02.png#lightbox)
1. Faites glisser un **vue pile** à partir de la **boîte à outils** et déposez-la sur la vue : 

    [![](stacked-views-images/layout03-vs.png "Une vue de la pile")](stacked-views-images/layout03-vs.png#lightbox)
1. Sélectionnez la vue de la pile, dans le **onglet Widget** de la **l’Explorateur de propriétés** sélectionnez **remplir** pour le **alignement**, **remplir Tout aussi** pour le **Distribution** et entrez `25` pour le **espacement**: 

    [![](stacked-views-images/layout04-vs.png "L’onglet de Widget")](stacked-views-images/layout04-vs.png#lightbox)
1. Positionner la vue de la pile sur l’écran où vous le souhaitez et ajoutez des contraintes pour le conserver dans l’emplacement requis.
1. Sélectionnez les éléments individuels et faites-les glisser dans la vue de la pile : 

    [![](stacked-views-images/layout05-vs.png "Les éléments individuels dans la vue de la pile")](stacked-views-images/layout05-vs.png#lightbox)
1. La mise en page soient ajustée et les éléments seront disposés dans la vue pile basée sur les attributs que vous avez précédemment défini.
1. Affecter **noms** dans le **onglet Widget** de la **l’Explorateur de propriétés** pour travailler avec les contrôles d’IU dans C# code.
1. Enregistrez les modifications apportées.

-----

> [!IMPORTANT]
> Bien qu’il soit possible d’affecter des actions telles que `TouchUpInside` à un élément d’interface utilisateur (comme un `UIButton`) dans le concepteur lorsque vous créez un gestionnaire d’événements d’iOS, il ne sera jamais appelée car Apple TV n’a pas une pression tactile écran ou de prendre en charge les événements tactiles. Vous devez toujours utiliser la valeur par défaut `Action Type` lors de la création d’Actions pour tvOS éléments d’interface utilisateur.

Pour plus d’informations sur l’utilisation des tables de montage séquentiel, consultez notre [Hello, tvOS Guide de démarrage rapide](~/ios/tvos/get-started/hello-tvos.md).

Dans le cas de notre exemple, nous avons exposé un Outlet et l’Action pour le contrôle du Segment et un Outlet pour chaque carte « lecteur ». Dans le code, nous masquer et afficher le lecteur basé sur le segment actuel. Exemple :

```csharp
partial void PlayerCountChanged (Foundation.NSObject sender) {

    // Take Action based on the segment
    switch(PlayerCount.SelectedSegment) {
    case 0:
        Player1.Hidden = false;
        Player2.Hidden = true;
        Player3.Hidden = true;
        Player4.Hidden = true;
        break;
    case 1:
        Player1.Hidden = false;
        Player2.Hidden = false;
        Player3.Hidden = true;
        Player4.Hidden = true;
        break;
    case 2:
        Player1.Hidden = false;
        Player2.Hidden = false;
        Player3.Hidden = false;
        Player4.Hidden = true;
        break;
    case 3:
        Player1.Hidden = false;
        Player2.Hidden = false;
        Player3.Hidden = false;
        Player4.Hidden = false;
        break;
    }
}
```

Lorsque l’application est exécutée, les quatre éléments sont tout aussi distribuées dans notre vue pile :

[![](stacked-views-images/layout06.png "Quand l’application est exécutée, les quatre éléments seront être répartis dans notre vue pile")](stacked-views-images/layout06.png#lightbox)

Si le nombre de lecteurs est réduit, les vues inutilisés sont masqués et la vue pile ajuster la disposition en fonction de :

[![](stacked-views-images/layout07.png "Si le nombre de lecteurs est réduit, les vues inutilisés sont masqués et la vue pile ajuster la disposition pour tenir")](stacked-views-images/layout07.png#lightbox)

<a name="Populate-a-Stack-View-from-Code" />

### <a name="populate-a-stack-view-from-code"></a>Remplir une vue de la pile à partir du Code

Outre la définition totalement le contenu et la disposition d’une vue de la pile dans le concepteur iOS, vous pouvez créer et supprimer dynamiquement de C# code.

Prenons l’exemple suivant qui utilise une vue de la pile pour gérer des « étoiles » dans une révision (1 à 5) :

```csharp
public int Rating { get; set;} = 0;
...

partial void IncreaseRating (Foundation.NSObject sender) {

    // Maximum of 5 "stars"
    if (++Rating > 5 ) {
        // Abort
        Rating = 5;
        return;
    }

    // Create new rating icon and add it to stack
    var icon = new UIImageView (new UIImage("icon.png"));
    icon.ContentMode = UIViewContentMode.ScaleAspectFit;
    RatingView.AddArrangedSubview(icon);

    // Animate stack
    UIView.Animate(0.25, ()=>{
        // Adjust stack view
        RatingView.LayoutIfNeeded();
    });

}

partial void DecreaseRating (Foundation.NSObject sender) {

    // Minimum of zero "stars"
    if (--Rating < 0) {
        // Abort
        Rating =0;
        return;
    }

    // Get the last subview added
    var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];

    // Remove from stack and screen
    RatingView.RemoveArrangedSubview(icon);
    icon.RemoveFromSuperview();

    // Animate stack
    UIView.Animate(0.25, ()=>{
        // Adjust stack view
        RatingView.LayoutIfNeeded();
    });
}
```

Jetons un œil à quelques éléments de ce code en détail. Tout d’abord, nous utilisons un `if` instructions pour vérifier qu’il n’est pas plus de cinq « étoiles » ou inférieur à zéro.

Pour ajouter un nouveau « star » nous charger son image et définir son **Mode contenu** à **mise à l’échelle Aspect ajuster**:

```csharp
var icon = new UIImageView (new UIImage("icon.png"));
icon.ContentMode = UIViewContentMode.ScaleAspectFit;
```

Cela permet de conserver l’icône « en étoile » de la distorsion lorsqu’il est ajouté à la vue de la pile.

Ensuite, nous ajoutons la nouvelle icône « en étoile » à la collection de la vue de la pile de sous-vues :

```csharp
RatingView.AddArrangedSubview(icon);
```

Vous remarquerez que nous avons ajouté la `UIImageView` à la `UIStackView`de `ArrangedSubviews` propriété et non le `SubView`. N’importe quelle vue que vous souhaitez que l’affichage de pile pour contrôler sa disposition doit être ajouté à la `ArrangedSubviews` propriété.

Pour supprimer un sous-affichage à partir d’une vue de la pile, nous obtenons d’abord le sous-affichage à supprimer :

```csharp
var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];
```

Puis nous avons besoin pour le supprimer à la fois le `ArrangedSubviews` collection et la vue de Super :

```csharp
// Remove from stack and screen
RatingView.RemoveArrangedSubview(icon);
icon.RemoveFromSuperview();
```

Supprime un sous-affichage uniquement le `ArrangedSubviews` collection le supprime du contrôle de la vue de la pile, mais ne le supprime pas de l’écran.

<a name="Dynamically-Changing-Content" />

## <a name="dynamically-changing-content"></a>Modifier dynamiquement le contenu

Une vue de la pile s’ajuste automatiquement la disposition des sous-vues chaque fois qu’un sous-affichage est ajouté, supprimé ou masqué. La mise en page soient également ajustée si n’importe quelle propriété de la vue de la pile est ajustée (telles que son `Axis`).

Les changements de disposition peuvent être animés, en les plaçant dans un bloc d’Animation, par exemple :

```csharp
// Animate stack
UIView.Animate(0.25, ()=>{
    // Adjust stack view
    RatingView.LayoutIfNeeded();
});
```

La plupart des propriétés de la vue pile peuvent être spécifiés à l’aide des Classes de taille au sein d’une table de montage séquentiel. Ces propriétés seront automatiquement animé est la réponse aux modifications de taille ou orientation.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a présenté la conception et l’utilisation de la vue empilées à l’intérieur d’une application Xamarin.tvOS.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application pour tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
