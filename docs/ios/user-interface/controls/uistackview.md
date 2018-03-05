---
title: Vue de la pile
description: "Cet article couvre à l’aide du nouveau contrôle UIStackView dans une application Xamarin.iOS à gérer un ensemble de sous-vues, que ce soit une pile organisée horizontalement ou verticalement."
ms.topic: article
ms.prod: xamarin
ms.assetid: 20246E87-2A49-438A-9BD7-756A1B50A617
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 4555906512ecc36e3387f1b2483753e7f50a51ae
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="stack-view"></a>Vue de la pile

_Cet article couvre à l’aide du nouveau contrôle UIStackView dans une application Xamarin.iOS à gérer un ensemble de sous-vues, que ce soit une pile organisée horizontalement ou verticalement._

> [!IMPORTANT]
> Veuillez noter que StackView est pris en charge dans le concepteur iOS, vous pouvez rencontrer des bogues de facilité d’utilisation lors de l’utilisation du canal Stable. Basculer les canaux bêta ou Alpha doivent permettre de résoudre ce problème. Nous avons décidé de présenter cette procédure pas à pas à l’aide de Xcode jusqu'à ce que les correctifs requis sont implémentées dans le canal Stable.

Le contrôle de la vue de la pile (`UIStackView`) tire parti de la puissance de disposition automatique et les Classes de taille pour gérer une pile de sous-vues, horizontalement ou verticalement, qui répond dynamiquement à la taille d’écran et l’orientation de l’appareil iOS.

La disposition de tous les sous-vues attaché à une vue de la pile sont gérées par en fonction des propriétés de développeur défini comme axe, distribution, alignement et l’espacement :

[ ![](uistackview-images/stacked01.png "Diagramme de mise en page de vue de pile")](uistackview-images/stacked01.png)

Lorsque vous utilisez un `UIStackView` dans une application Xamarin.iOS, le développeur peut définir les sous-vues soit à l’intérieur d’un plan conceptuel dans le concepteur iOS, ou en ajoutant et supprimant des sous-vues en code c#.

Ce document se compose de deux parties : un démarrage rapide pour vous aider à implémenter votre pile afficher et puis certains plus de détails techniques sur son fonctionnement.

## <a name="uistackview-quickstart"></a>Démarrage rapide de UIStackView

Comme une introduction rapide à le `UIStackView` (contrôle), nous allons créer une interface simple qui permet à l’utilisateur à entrer une évaluation à partir de 1 à 5. Nous utilisons deux vues de la pile : une pour organiser l’interface verticalement sur son écran et l’autre pour réorganiser les icônes d’évaluation 1-5 horizontalement à l’écran.

### <a name="define-the-ui"></a>Définir l’interface utilisateur

Démarrer un nouveau projet de Xamarin.iOS et de modifier le **Main.storyboard** fichier dans le Générateur de Xcode Interface. Tout d’abord, faites glisser un seul **Vertical vue pile** sur la **View Controller**:

[ ![](uistackview-images/quick01.png "Faites glisser une seule vue pile verticale sur le contrôleur d’affichage")](uistackview-images/quick01.png)

Dans le **inspecteur de l’attribut**, définissez les options suivantes :

[ ![](uistackview-images/quick02.png "Définir les options de vue de la pile")](uistackview-images/quick02.png)

Où :

- **Axe** – détermine si la vue pile organise les sous-vues soit **horizontalement** ou **verticalement**.
- **Alignement** – contrôle comment les sous-vues sont alignées dans la vue de la pile.
- **Distribution** – contrôle la façon dont les sous-vues sont dimensionnés dans la vue de la pile.
- **Espacement** : contrôle l’espace minimal entre chaque sous-affichage dans la vue de la pile.
- **Planification Relative** – si elle est activée, l’espacement vertical de chaque sous-affichage sera dérivé de sa ligne de base.
- **Mise en page marges relatif** – place les sous-vues par rapport aux marges de disposition standard.

Lorsque vous travaillez avec une vue de la pile, vous pouvez considérer le **alignement** en tant que le **X** et **Y** emplacement de la fermeture du sous-affichage et **Distribution** comme le **Hauteur** et **largeur**.

> [!IMPORTANT]
> **Remarque :** `UIStackView` est conçu comme une vue non-rendu conteneur et par conséquent, il n’est pas dessiné à la zone de dessin, comme les autres sous-classes de `UIView`. Par conséquent, définissant les propriétés telles que `BackgroundColor` ou la substitution `DrawRect` n’a aucun effet visuel.

Continuer à disposition interface de l’application en ajoutant une étiquette, ImageView, deux boutons et une vue pile Horizontal afin qu’il ressemble à ceci :

[ ![](uistackview-images/quick03.png "Disposition de l’interface utilisateur de vue pile")](uistackview-images/quick03.png)

Configurer l’affichage Horizontal de la pile avec les options suivantes :

[ ![](uistackview-images/quick04.png "Configurez les options de vue de la pile horizontale")](uistackview-images/quick04.png)

Car nous ne voulons pas l’icône qui représente chaque « point » dans l’évaluation étiré lorsqu’il est ajouté à la vue pile horizontale, nous avons défini le **alignement** à **Center** et  **Distribution** à **remplir tout aussi**.

Enfin, des câbles suivantes **prises** et **Actions**:

[ ![](uistackview-images/quick05.png "Les prises de vue pile et les Actions")](uistackview-images/quick05.png)

### <a name="populate-a-uistackview-from-code"></a>Remplir un UIStackView à partir du Code

Revenez à Visual Studio pour Mac et modifier le **ViewController.cs** et ajoutez le code suivant :

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

### <a name="testing-the-ui"></a>Test de l’interface utilisateur

Avec tous les éléments d’interface utilisateur et requis code en place, vous pouvez maintenant exécuter et tester l’interface. Lorsque l’interface utilisateur s’affiche, tous les éléments dans la vue pile verticale seront régulièrement espacés de haut en bas.

Lorsque l’utilisateur appuie sur le **augmenter la note** bouton, un autre « étoile » est ajoutée à l’écran (jusqu'à un maximum de 5) :

[ ![](uistackview-images/intro01.png "L’exemple d’application de s’exécuter")](uistackview-images/intro01.png)

Les « étoiles » seront automatiquement centrées et répartis dans l’affichage Horizontal de la pile. Lorsque l’utilisateur appuie sur le **baisser la note** bouton, une « étoile » est supprimé (jusqu'à ce qu’aucun sont à gauche).

## <a name="stack-view-details"></a>Afficher les détails de pile

Maintenant que nous avons une idée générale de ce que le `UIStackView` le contrôle est et comment il fonctionne, jetons un œil plus approfondie à certaines de ses fonctionnalités et les détails.

### <a name="auto-layout-and-size-classes"></a>Les Classes de taille et de mise en page automatique

Comme nous l’avons vu, lorsqu’un sous-affichage est ajouté à une vue de la pile sa disposition est entièrement contrôlé par cette vue de la pile à l’aide de la disposition automatique et les Classes de taille pour positionner et redimensionner les vues réorganisées.

La vue de la pile sera _code confidentiel_ le premier et le dernier sous-affichage dans sa collection le **haut** et **bas** bords pour les vues de pile verticale ou **gauche**et **droite** bords pour les vues de pile horizontale. Si vous définissez la `LayoutMarginsRelativeArrangement` propriété `true`, puis la vue épingle les sous-vues aux marges pertinentes au lieu du bord.

La vue pile utilise de la fermeture du sous-affichage `IntrinsicContentSize` propriété lors du calcul de la taille sous-vues le long de la période définie par `Axis` (à l’exception de la `FillEqually Distribution`). Le `FillEqually Distribution` redimensionne toutes les sous-vues afin qu’ils soient de la même taille, donc remplir le long de la vue de la pile du `Axis`.

À l’exception de la `Fill Alignment`, la vue pile utilise de la fermeture du sous-affichage `IntrinsicContentSize` propriété pour le calcul de taille de la vue perpendiculaire à la donnée `Axis`. Pour le `Fill Alignment`, toutes les sous-vues sont dimensionnés pour qu’elles remplissent la vue pile perpendiculairement à la donnée `Axis`.

### <a name="positioning-and-sizing-the-stack-view"></a>Positionnement et le redimensionnement de la vue de la pile

Alors que la vue de la pile a un contrôle total sur la disposition de n’importe quel sous-affichage (en fonction des propriétés telles que `Axis` et `Distribution`), vous devez quand même placer la vue de la pile (`UIStackView`) dans la vue parent à l’aide des Classes de taille et de disposition automatique.

En règle générale, cela signifie que l’épinglage d’au moins deux bords de la vue de la pile pour développer et réduire, définissant ainsi sa position. Sans les contraintes supplémentaires, la vue de la pile sont automatiquement redimensionnée pour s’ajuster à tous ses sous-vues comme suit :

 - La taille, le long de son `Axis` sera la somme de toutes les tailles de sous-affichage et tout l’espace qui a été défini entre chaque sous-affichage.
 - Si le `LayoutMarginsRelativeArrangement` propriété est `true`, la taille de pile vues inclut également l’espace pour les marges.
 - La taille perpendiculairement à le `Axis` a la valeur la plus grande sous-affichage dans la collection.

En outre, vous pouvez spécifier des contraintes pour la vue de pile **hauteur** et **largeur**. Dans ce cas, les sous-vues seront disposés (taille) pour remplir l’espace spécifié par la vue pile, comme déterminé par le `Distribution` et `Alignment` propriétés.

Si le `BaselineRelativeArrangement` propriété `true`, les sous-vues seront disposés en fonction de la ligne de base du sous-affichage premier ou dernier, au lieu d’utiliser le **haut**, **bas** ou **Center* -  **Y** position. Elles sont calculées comme suit sur le contenu de la vue de la pile :

 - Une vue de la pile verticale retourne le premier sous-affichage pour la première ligne de base et le dernier de la dernière. Si un de ces sous-vues sont eux-mêmes des vues de la pile, puis leur ligne de base du premier ou dernier sera utilisé.
 - Une vue pile Horizontal utilisera sa plus grand sous-affichage pour à la fois la première et la dernière ligne de base. Si la vue la plus haute est également une vue de la pile, il utilisera sa plus grand sous-affichage en tant que la ligne de base.

> [!IMPORTANT]
> **Remarque :** alignement de la ligne de base ne fonctionne pas sur les tailles de sous-affichage élargi ou compressé comme la ligne de base correspond à une position incorrecte. Pour l’alignement de la ligne de base, vérifiez que le sous-affichage **hauteur** correspond à la vue contenu intrinsèque **hauteur**.

### <a name="common-stack-view-uses"></a>Utilisations de la vue de pile

Il existe plusieurs types de mise en page qui fonctionnent correctement avec les contrôles de vue de la pile. En fonction d’Apple, en Voici quelques-unes des utilisations plus courantes :

- **Définir la taille le long de l’axe** : par épinglage les deux bords le long de la vue de pile `Axis` et les bords adjacents pour définir la position, la pile de vue augmentera le long de l’axe en fonction de l’espace défini par son sous-vues.
 -  **Définir Position du sous-affichage** – augmente dans les deux dimensions en fonction de son conteneur sous-vues en épinglant à bords adjacents de la vue de la pile à la vue de son parent, la vue de la pile.
- **Définir la taille et la Position de la pile** – en épinglant les quatre bords de la vue de la pile à la vue parent, la vue pile organise les sous-vues en fonction de l’espace défini dans la vue de la pile.
 -  **Définir la taille perpendiculaires l’axe** : par épinglage perpendiculaire les deux bords de la vue de la pile `Axis` et les bords de l’axe pour définir la position, la pile de vue augmentera perpendiculairement à l’axe en fonction de l’espace défini par son sous-vues.

### <a name="managing-the-appearance"></a>Gestion de l’affichage

Le `UIStackView` est conçu comme une vue non-rendu conteneur et par conséquent, il n’est pas dessiné à la zone de dessin, comme les autres sous-classes de `UIView`. Définition des propriétés telles que `BackgroundColor` ou la substitution `DrawRect` n’a aucun effet visuel.

Il existe plusieurs propriétés qui contrôlent la façon dont une vue pile réorganiser sa collection de sous-vues :

- **Axe** – détermine si la vue pile organise les sous-vues soit **horizontalement** ou **verticalement**.
- **Alignement** – contrôle comment les sous-vues sont alignées dans la vue de la pile.
- **Distribution** – contrôle la façon dont les sous-vues sont dimensionnés dans la vue de la pile.
- **Espacement** : contrôle l’espace minimal entre chaque sous-affichage dans la vue de la pile.
- **Planification Relative** : si `true`, l’espacement vertical de chaque sous-affichage sera dérivé de sa ligne de base.
- **Mise en page marges relatif** – place les sous-vues par rapport aux marges de disposition standard.

En règle générale, vous allez utiliser une vue de la pile pour réorganiser un petit nombre de sous-vues. Interfaces utilisateur plus complexes peuvent être créés par l’imbrication d’une ou plusieurs vues de pile à l’intérieur de l’autre (comme nous l’avons fait le [UIStackView Quickstart](#UIStackView-Quickstart) ci-dessus).

Vous pouvez affiner l’apparence des interfaces utilisateur en ajoutant des contraintes supplémentaires pour les sous-vues (par exemple pour le contrôle de la hauteur ou largeur). Toutefois, soyez vigilant ne pas à inclure les contraintes en conflit à celles introduites par la vue pile lui-même.

### <a name="maintaining-arranged-views-and-sub-views-consistency"></a>Maintenance organisées vues et la cohérence des vues Sub

La vue de la pile pour vous assurer que ses `ArrangedSubviews` propriété est toujours un sous-ensemble de ses `Subviews` propriété à l’aide des règles suivantes :

 - Si un sous-affichage est ajouté à la `ArrangedSubviews` collection, il est automatiquement ajoutée à la `Subviews` collection (sauf si elle fait déjà partie de cette collection).
 - Si un sous-affichage est supprimé de la `Subviews` collection (supprimée de l’affichage), il est également supprimé de la `ArrangedSubviews` collection.
 - Suppression d’un sous-affichage à partir de la `ArrangedSubviews` collection ne supprime pas de la `Subviews` collection. Par conséquent, il est n’est plus disposé par la vue de la pile, mais sera toujours visible sur l’écran.

Le `ArrangedSubviews` regroupement est toujours un sous-ensemble de la `Subview` collection, toutefois l’ordre des sous-vues individuels au sein de chaque collection est distincte et contrôlée par le texte suivant :

 - L’ordre des sous-vues dans le `ArrangedSubviews` collection déterminer leur ordre d’affichage de la pile.
 - L’ordre des sous-vues dans le `Subview` collection détermine leur ordre de plan (ou superposition) dans la vue arrière vers l’avant.

### <a name="dynamically-changing-content"></a>Modifier dynamiquement le contenu

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

## <a name="summary"></a>Récapitulatif

Cet article a traité la nouvelle `UIStackView` contrôle (pour iOS 9) pour gérer un ensemble de sous-vues dans une pile organisée horizontalement ou verticalement dans une application Xamarin.iOS.
Il a commencé avec un exemple simple d’utilisation des vues de la pile pour créer une interface utilisateur et s’est terminé avec une présentation plus détaillée de la pile des vues et leurs propriétés et les fonctionnalités.



## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 pour les développeurs](https://developer.apple.com/ios/pre-release/)
- [Nouveautés dans iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Référence de UIStackView](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIStackView_Class_Reference/)
- [Présentation de UIStackView (vidéo)](https://university.xamarin.com/lightninglectures/introducing-uistackview-on-ios9)
