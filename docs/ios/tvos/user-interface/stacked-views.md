---
title: "Utilisation d’une vue empilée"
description: "Cet article décrit la conception et l’utilisation d’empilées une vue à l’intérieur d’une application Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 00B07F85-F30B-4DD4-8664-A61D0A1CDB0E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: efc0da0b0f0a800069efb00a402361a8b0cd7f1d
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="working-with-stacked-view"></a>Utilisation d’une vue empilée

_Cet article décrit la conception et l’utilisation d’empilées une vue à l’intérieur d’une application Xamarin.tvOS._


Le contrôle de la vue de la pile (`UIStackView`) tire parti de la puissance de disposition automatique et les Classes de taille pour gérer une pile de sous-vues, horizontalement ou verticalement, qui dynamiquement répond aux modifications de contenu et la taille de l’écran de l’appareil Apple TV.

La disposition de tous les sous-vues attaché à une vue de la pile sont gérées par en fonction des propriétés de développeur défini comme axe, distribution, alignement et l’espacement :

[![](stacked-views-images/stacked01.png "Diagramme de disposition sous-affichage")](stacked-views-images/stacked01.png#lightbox)

Lorsque vous utilisez un `UIStackView` dans une application Xamarin.tvOS, le développeur peut définir soit des sous-vues soit à l’intérieur d’un plan conceptuel dans le concepteur iOS, ou en ajoutant et supprimant des sous-vues en code c#.

## <a name="about-stacked-view-controls"></a>À propos des contrôles de vue empilées 

Le `UIStackView` est conçu comme une vue non-rendu conteneur et par conséquent, il n’est pas dessiné à la zone de dessin, comme les autres sous-classes de `UIView`. Définition des propriétés telles que `BackgroundColor` ou la substitution `DrawRect` n’a aucun effet visuel.

Il existe plusieurs propriétés qui contrôlent la façon dont une vue pile réorganiser sa collection de sous-vues :

- **Axe** – détermine si la vue pile organise les sous-vues soit **horizontalement** ou **verticalement**.
- **Alignement** – contrôle comment les sous-vues sont alignées dans la vue de la pile.
- **Distribution** – contrôle la façon dont les sous-vues sont dimensionnés dans la vue de la pile.
- **Espacement** : contrôle l’espace minimal entre chaque sous-affichage dans la vue de la pile.
- **Planification Relative** : si `true`, l’espacement vertical de chaque sous-affichage sera dérivé de sa ligne de base.
- **Mise en page marges relatif** – place les sous-vues par rapport aux marges de disposition standard.

En règle générale, vous allez utiliser une vue de la pile pour réorganiser un petit nombre de sous-vues. Interfaces utilisateur plus complexes peuvent être créés par l’imbrication d’une ou plusieurs vues de pile à l’intérieur de l’autre.

Vous pouvez affiner l’apparence des interfaces utilisateur en ajoutant des contraintes supplémentaires pour les sous-vues (par exemple pour le contrôle de la hauteur ou largeur). Toutefois, soyez vigilant ne pas à inclure les contraintes en conflit à celles introduites par la vue pile lui-même.

<a name="Auto-Layout-and-Size-Classes" />

## <a name="auto-layout-and-size-classes"></a>Les Classes de taille et de mise en page automatique

Lorsqu’un sous-affichage est ajouté à une vue de la pile sa disposition est entièrement contrôlée par cette vue de la pile à l’aide de la disposition automatique et les Classes de taille pour positionner et redimensionner les vues réorganisées.

La vue de la pile sera _code confidentiel_ le premier et le dernier sous-affichage dans sa collection le **haut** et **bas** bords pour les vues de pile verticale ou **gauche**et **droite** bords pour les vues de pile horizontale. Si vous définissez la `LayoutMarginsRelativeArrangement` propriété `true`, puis la vue épingle les sous-vues aux marges pertinentes au lieu du bord.

La vue pile utilise de la fermeture du sous-affichage `IntrinsicContentSize` propriété lors du calcul de la taille sous-vues le long de la période définie par `Axis` (à l’exception de la `FillEqually Distribution`). Le `FillEqually Distribution` redimensionne toutes les sous-vues afin qu’ils soient de la même taille, donc remplir le long de la vue de la pile du `Axis`.

À l’exception de la `Fill Alignment`, la vue pile utilise de la fermeture du sous-affichage `IntrinsicContentSize` propriété pour le calcul de taille de la vue perpendiculaire à la donnée `Axis`. Pour le `Fill Alignment`, toutes les sous-vues sont dimensionnés pour qu’elles remplissent la vue pile perpendiculairement à la donnée `Axis`.

<a name="Positioning-and-Sizing-the-Stack-View" />

## <a name="positioning-and-sizing-the-stack-view"></a>Positionnement et le redimensionnement de la vue de la pile

Alors que la vue de la pile a un contrôle total sur la disposition de n’importe quel sous-affichage (en fonction des propriétés telles que `Axis` et `Distribution`), vous devez quand même placer la vue de la pile (`UIStackView`) dans la vue parent à l’aide des Classes de taille et de disposition automatique.

En règle générale, cela signifie que l’épinglage d’au moins deux bords de la vue de la pile pour développer et réduire, définissant ainsi sa position. Sans les contraintes supplémentaires, la vue de la pile sont automatiquement redimensionnée pour s’ajuster à tous ses sous-vues comme suit :

* La taille, le long de son `Axis` sera la somme de toutes les tailles de sous-affichage et tout l’espace qui a été défini entre chaque sous-affichage.
* Si le `LayoutMarginsRelativeArrangement` propriété est `true`, la taille de pile vues inclut également l’espace pour les marges.
* La taille perpendiculairement à le `Axis` a la valeur la plus grande sous-affichage dans la collection.

En outre, vous pouvez spécifier des contraintes pour la vue de pile **hauteur** et **largeur**. Dans ce cas, les sous-vues seront disposés (taille) pour remplir l’espace spécifié par la vue pile, comme déterminé par le `Distribution` et `Alignment` propriétés.

Si le `BaselineRelativeArrangement` propriété `true`, les sous-vues seront disposés en fonction de la ligne de base du sous-affichage premier ou dernier, au lieu d’utiliser le **haut**, **bas** ou **Center* -  **Y** position. Elles sont calculées comme suit sur le contenu de la vue de la pile :

* Une vue de la pile verticale retourne le premier sous-affichage pour la première ligne de base et le dernier de la dernière. Si un de ces sous-vues sont eux-mêmes des vues de la pile, puis leur ligne de base du premier ou dernier sera utilisé.
* Une vue pile Horizontal utilisera sa plus grand sous-affichage pour à la fois la première et la dernière ligne de base. Si la vue la plus haute est également une vue de la pile, il utilisera sa plus grand sous-affichage en tant que la ligne de base.

> [!IMPORTANT]
> Alignement de la ligne de base ne fonctionne pas sur les tailles de sous-affichage élargi ou compressé, comme la ligne de base correspond à une position incorrecte. Pour l’alignement de la ligne de base, vérifiez que le sous-affichage **hauteur** correspond à la vue contenu intrinsèque **hauteur**.




<a name="Common-Stack-View-Uses" />

## <a name="common-stack-view-uses"></a>Utilisations de la vue de pile

Il existe plusieurs types de mise en page qui fonctionnent correctement avec les contrôles de vue de la pile. En fonction d’Apple, en Voici quelques-unes des utilisations plus courantes :

- **Définir la taille le long de l’axe** : par épinglage les deux bords le long de la vue de pile `Axis` et les bords adjacents pour définir la position, la pile de vue augmentera le long de l’axe en fonction de l’espace défini par son sous-vues.
*  **Définir Position du sous-affichage** – augmente dans les deux dimensions en fonction de son conteneur sous-vues en épinglant à bords adjacents de la vue de la pile à la vue de son parent, la vue de la pile.
- **Définir la taille et la Position de la pile** – en épinglant les quatre bords de la vue de la pile à la vue parent, la vue pile organise les sous-vues en fonction de l’espace défini dans la vue de la pile.
*  **Définir la taille perpendiculaires l’axe** : par épinglage perpendiculaire les deux bords de la vue de la pile `Axis` et les bords de l’axe pour définir la position, la pile de vue augmentera perpendiculairement à l’axe en fonction de l’espace défini par son sous-vues.

<a name="Stack-Views-and-Storyboards" />

## <a name="stack-views-and-storyboards"></a>Vues de la pile et les animations

Pour utiliser des vues de la pile dans une application Xamarin.tvOS, le plus simple consiste à ajouter à l’interface utilisateur de l’application d’utiliser le concepteur iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Dans le **Solution remplissage**, en double-cliquant sur le `Main.storyboard` de fichier et l’ouvre pour modification.
1. Concevoir la disposition de vos éléments individuels que vous vous apprêtez à ajouter à la vue de la pile : 

    [![](stacked-views-images/layout01.png "Exemple de mise en page d’élément")](stacked-views-images/layout01.png#lightbox)
1. Ajouter toutes les contraintes nécessaires aux éléments afin de que s’adapter correctement. Cette étape est importante, une fois que l’élément est ajouté à la vue de la pile.
1. Vérifiez le nombre de copies (quatre dans ce cas) requis : 

    [![](stacked-views-images/layout02.png "Le nombre de copies requis")](stacked-views-images/layout02.png#lightbox)
1. Faites glisser un **vue pile** à partir de la **boîte à outils** et déposez-la sur la vue : 

    [![](stacked-views-images/layout03.png "Une vue de la pile")](stacked-views-images/layout03.png#lightbox)
1. Sélectionnez la vue de la pile, dans le **onglet Widget** de la **propriétés remplissage** sélectionnez **remplir** pour le **alignement**, **remplir Tout aussi** pour le **Distribution** et entrez `25` pour le **espacement**: 

    [![](stacked-views-images/layout04.png "L’onglet du Widget")](stacked-views-images/layout04.png#lightbox)
1. Position de la vue de la pile à l’écran où vous le souhaitez et ajoutez des contraintes pour le conserver dans l’emplacement requis.
1. Sélectionnez les éléments individuels et faites-les glisser dans la vue de la pile : 

    [![](stacked-views-images/layout05.png "Les éléments individuels dans la vue de la pile")](stacked-views-images/layout05.png#lightbox)
1. La disposition sera ajustée et les éléments seront disposés dans la vue de la pile selon les attributs que vous avez précédemment défini.
1. Affecter **noms** dans les **onglet Widget** de la **l’Explorateur de propriétés** pour travailler avec les contrôles d’IU dans le code c#.
1. Enregistrez les modifications apportées.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Dans le **l’Explorateur de solutions**, en double-cliquant sur le `Main.storyboard` de fichier et l’ouvre pour modification.
1. Concevoir la disposition de vos éléments individuels que vous vous apprêtez à ajouter à la vue de la pile : 

    [![](stacked-views-images/layout01.png "Exemple de disposition élément")](stacked-views-images/layout01.png#lightbox)
1. Ajouter toutes les contraintes nécessaires aux éléments afin de que s’adapter correctement. Cette étape est importante, une fois que l’élément est ajouté à la vue de la pile.
1. Vérifiez le nombre de copies (quatre dans ce cas) requis : 

    [![](stacked-views-images/layout02.png "Le nombre de copies requis")](stacked-views-images/layout02.png#lightbox)
1. Faites glisser un **vue pile** à partir de la **boîte à outils** et déposez-la sur la vue : 

    [![](stacked-views-images/layout03-vs.png "Une vue de la pile")](stacked-views-images/layout03-vs.png#lightbox)
1. Sélectionnez la vue de la pile, dans le **onglet Widget** de la **l’Explorateur de propriétés** sélectionnez **remplir** pour le **alignement**, **remplir Tout aussi** pour le **Distribution** et entrez `25` pour le **espacement**: 

    [![](stacked-views-images/layout04-vs.png "L’onglet du Widget")](stacked-views-images/layout04-vs.png#lightbox)
1. Position de la vue de la pile à l’écran où vous le souhaitez et ajoutez des contraintes pour le conserver dans l’emplacement requis.
1. Sélectionnez les éléments individuels et faites-les glisser dans la vue de la pile : 

    [![](stacked-views-images/layout05-vs.png "Les éléments individuels dans la vue de la pile")](stacked-views-images/layout05-vs.png#lightbox)
1. La disposition sera ajustée et les éléments seront disposés dans la vue de la pile selon les attributs que vous avez précédemment défini.
1. Affecter **noms** dans les **onglet Widget** de la **l’Explorateur de propriétés** pour travailler avec les contrôles d’IU dans le code c#.
1. Enregistrez les modifications apportées.

-----

> [!IMPORTANT]
> Bien qu’il soit possible d’affecter des actions telles que `TouchUpInside` à un élément d’interface utilisateur (comme un `UIButton`) dans le concepteur lors de la création d’un gestionnaire d’événements d’iOS, il ne sera jamais appelée car Apple TV n’a pas une fonction tactile d’écran ou de prendre en charge les événements tactiles. Vous devez toujours utiliser la valeur par défaut `Action Type` lors de la création d’Actions de tvOS éléments d’interface utilisateur.

Pour plus d’informations sur l’utilisation des plans conceptuels, veuillez consulter notre [Hello, Guide de démarrage rapide de tvOS](~/ios/tvos/get-started/hello-tvos.md).

Dans le cas de notre exemple, nous avons exposé un prise et l’Action pour le contrôle de Segment et une sortie pour chaque carte « lecteur ». Dans le code, nous masquer et afficher le lecteur basé sur le segment actuel. Exemple :

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

Lorsque l’application est exécutée, les quatre éléments seront répartis dans la vue de la pile :

[![](stacked-views-images/layout06.png "Lorsque l’application est exécutée, les quatre éléments seront également distribuées en vue de la pile")](stacked-views-images/layout06.png#lightbox)

Si le nombre de lecteurs est réduit, les vues inutilisés sont masqués et la vue pile ajuster la disposition en fonction de :

[![](stacked-views-images/layout07.png "Si le nombre de lecteurs est réduit, les vues inutilisés sont masqués et la vue pile ajuster la disposition en fonction de")](stacked-views-images/layout07.png#lightbox)

<a name="Populate-a-Stack-View-from-Code" />

### <a name="populate-a-stack-view-from-code"></a>Remplir une vue de la pile à partir du Code

Outre la définition totalement le contenu et la disposition d’une vue de la pile dans le concepteur iOS, vous pouvez créer et supprimer dynamiquement du code c#.

Prenons l’exemple suivant qui utilise une vue de la pile pour gérer des « étoiles » dans une revue (de 1 à 5) :

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

Examinons quelques éléments de ce code en détail. Tout d’abord, nous utilisons un `if` instructions pour vérifier qu’il n’est pas plus de cinq « étoiles » ou inférieur à zéro.

Pour ajouter une nouvelle « étoile » nous charger son image et définir son **Mode contenu** à **Aspect de l’échelle ajuster**:

```csharp
var icon = new UIImageView (new UIImage("icon.png"));
icon.ContentMode = UIViewContentMode.ScaleAspectFit;
```

Cela permet de conserver l’élément « étoile » ne soit pas déformé lorsqu’il est ajouté à la vue de la pile.

Ensuite, nous ajouter la nouvelle icône « en étoile » à la collection de sous-vues la vue de la pile :

```csharp
RatingView.AddArrangedSubview(icon);
```

Vous remarquerez que nous avons ajouté la `UIImageView` à la `UIStackView`de `ArrangedSubviews` propriété et non le `SubView`. N’importe quelle vue que vous souhaitez que l’affichage de pile pour contrôler sa disposition doit être ajouté à la `ArrangedSubviews` propriété.

Pour supprimer un sous-affichage à partir d’une vue de la pile, nous obtenons d’abord le sous-affichage à supprimer :

```csharp
var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];
```

Puis nous avons besoin supprimer les deux le `ArrangedSubviews` collection et la vue de Super :

```csharp
// Remove from stack and screen
RatingView.RemoveArrangedSubview(icon);
icon.RemoveFromSuperview();
```

Supprime un sous-affichage uniquement le `ArrangedSubviews` collection interprète hors de contrôle de la vue de la pile, mais ne le supprime pas de l’écran.

<a name="Dynamically-Changing-Content" />

## <a name="dynamically-changing-content"></a>Modifier dynamiquement le contenu

Une vue de la pile ajuste automatiquement la disposition des sous-vues chaque fois qu’un sous-affichage est ajouté, supprimé ou masqué. La disposition soit également ajustée si n’importe quelle propriété de l’affichage de la pile est ajustée (telles que son `Axis`).

Changements de disposition peuvent être animées en les plaçant dans un bloc d’Animation, par exemple :

```csharp
// Animate stack
UIView.Animate(0.25, ()=>{
    // Adjust stack view
    RatingView.LayoutIfNeeded();
});
```

Grand nombre des propriétés de la vue de la pile peuvent être spécifiés à l’aide des Classes de taille dans un plan conceptuel. Ces propriétés seront automatiquement animé est réponse aux modifications de taille ou orientation.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a traité conception et utilisation de vue d’empilées à l’intérieur d’une application Xamarin.tvOS.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS les Guides Interface humaine](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
