---
title: Utilisation de tvOS Navigation et le Focus dans Xamarin
description: Cet article explique le concept de Focus et comment elle est utilisée pour présenter et de gérer la Navigation à l’intérieur d’une application Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: DD72E95F-AE9B-47D2-B132-5FA5FBD8026E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 3cb8d1c1d92146e70056c6cf562f2fa1cb028e7c
ms.sourcegitcommit: 946ce514fd6575aa6b93ff24181e02a60b24b106
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58677870"
---
# <a name="working-with-tvos-navigation-and-focus-in-xamarin"></a>Utilisation de tvOS Navigation et le Focus dans Xamarin

_Cet article explique le concept de Focus et comment elle est utilisée pour présenter et de gérer la Navigation à l’intérieur d’une application Xamarin.tvOS._


Cet article décrit le concept de [Focus](#Focus-and-Selection) et comment il est utilisé pour gérer [Navigation](#Navigation) dans l’Interface utilisateur d’une application Xamarin.tvOS. Nous allons examiner comment les contrôles de Navigation de tvOS intégrés utiliser Focus, la mise en surbrillance et la sélection pour fournir la Navigation dans l’Interface utilisateur de votre application Xamarin.tvOS.

[![](navigation-focus-images/intro01.png "Navigation dans l’Interface utilisateur des applications tvOS")](navigation-focus-images/intro01.png#lightbox)

Ensuite, nous allons examiner comment le Focus peut être utilisé avec [parallaxe](#Focus-and-Parallax) et *Images superposées* pour fournir des indices visuels pour l’état actuel de la Navigation à l’utilisateur final.

Enfin, nous examinerons l’utilisation de [Focus](#Working-with-Focus), [mises à jour le Focus](#Working-with-Focus-Updates), [Focus Guides](#Working-with-Focus-Guides), [le Focus dans les Collections](#Working-with-Focus-in-Collections) et [ L’activation de parallaxe](#enabling-parallax) sur les vues d’Image dans vos applications Xamarin.tvOS.

<a name="Navigation" />

## <a name="navigation"></a>Navigation

Les utilisateurs de votre application Xamarin.tvOS pas d’interagir avec son interface directement en tant qu’avec iOS où ils appuyer sur les images sur l’écran du périphérique, mais indirectement à partir sur la place à l’aide du [Siri Remote](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote). Vous devez gardez cela à l’esprit lorsque vous concevez l’Interface utilisateur de votre application afin qu’il transite naturellement, mais conserve l’utilisateur étant immergé dans l’expérience de l’Apple TV.

Une application tvOS réussie implémente la navigation d’une manière prenant en charge sans problème l’objectif de l’application et la structure des données qu’il présente sans attirant votre attention sur le volet de navigation. Concevez votre navigation afin que leur semble naturelle et familière sans domine l’Interface utilisateur ou de dessin axée sur le contenu et l’expérience utilisateur des applications.

[![](navigation-focus-images/nav01.png "L’application de paramètres tvOS")](navigation-focus-images/nav01.png#lightbox)

Bien que généralement à l’aide de Apple TV, l’utilisateur parcourt un ensemble empilé d’écrans, chacune présentant un ensemble donné de contenu. À son tour, chaque nouvel écran peut entraîner un ou plusieurs écrans secondaires de contenu à l’aide de contrôles d’interface utilisateur standards comme [boutons](~/ios/tvos/user-interface/buttons.md), [barres d’onglets](~/ios/tvos/user-interface/tab-bars.md), Tables, [vues de Collection](~/ios/tvos/user-interface/collection-views.md) ou [ Fractionner les affichages](~/ios/tvos/user-interface/split-views.md).

Avec chaque nouvel écran de données, l’utilisateur navigue plus loin dans cette pile d’écrans. À l’aide de la **Menu** bouton sur la télécommande Siri, ils peuvent naviguer vers l’arrière via la pile pour revenir à un écran précédent ou le Menu principal.

Apple suggère en n’oubliant pas les éléments suivants lors de la conception de la barre de navigation pour votre application tvOS :

- **Disposition de votre Navigation pour rendre les contenu recherche rapide et facile** -les utilisateurs à accéder au contenu au sein de votre application dans le plus petit nombre de coefficients, clique sur et fait glisser sa que possible. Simplifier votre navigation et organisez le contenu pour les nombres minimales d’écrans.
- **Créer une Interface fluide à l’aide de Touch** -vous assurer qu’un utilisateur peut déplacer entre _peut recevoir le focus des éléments_ avec friction minimale en utilisant le plus petit nombre de mouvements possible.
- **Conception avec le Focus dans l’esprit** -étant donné que l’utilisateur interagit avec le contenu à travers la pièce, dont ils ont besoin de déplacer le Focus à un élément d’Interface utilisateur avant d’interagir avec lui à l’aide de l’instance distante de Siri. Les utilisateurs obtiennent frustrés avec votre application si elle nécessite trop de mouvements pour qu’ils puissent atteindre leurs objectifs.
- **Permettent une Navigation vers l’arrière via le bouton de Menu** : pour créer une expérience simple et familier, permettre aux utilisateurs de naviguer vers l’arrière à l’aide de la Siri Remote **Menu** bouton. En appuyant sur la **Menu** bouton doit toujours renvoyer à l’écran précédent ou revenir au Menu principal de l’application. Au niveau supérieur de l’application, en appuyant sur la **Menu** bouton doit retourner à l’écran d’accueil de Apple TV.
- **En général, évitez d’afficher un bouton Précédent** - parce que, en appuyant sur la **Menu** bouton sur l’instance distante de Siri navigue vers l’arrière dans la pile de l’écran, éviter l’affichage d’un contrôle supplémentaire qui duplique ce comportement. Une exception à cette règle concerne les écrans ou écrans avec des actions destructrices (par exemple, la suppression du contenu) d’achat où un **Annuler** bouton s’affiche également.
- **Afficher des regroupements volumineux sur un seul écran, et non plusieurs** -le Siri à distance est conçue pour faciliter le déplacement d’une grande collection de contenu rapide et facile à l’aide de mouvements. Si votre application fonctionne avec une grande collection d’éléments pouvant être actif, envisagez de les conserver sur un seul écran au lieu de les fractionner dans nombreux écrans qui requièrent une navigation plus sur la partie de l’utilisateur.
- **Utiliser des contrôles Standard pour la Navigation** -là encore, pour créer une expérience utilisateur simple et familier, autant que possible, utilisez intégré `UIKit` contrôles tels que les contrôles de Page, barres d’onglets, contrôles segmentés, vues des tables, vues de Collection et fractionnement Vues pour la navigation de votre application. Étant donné qu’utilisateur êtes déjà familiarisé avec ces éléments, ils intuitivement sera en mesure de parcourir votre application.
- **Favoriser la Navigation horizontale du contenu** -en raison de la nature de l’Apple TV, passant de gauche à droite de la télécommande Siri est plus naturelle que haut et bas. Envisagez cette option lorsque vous concevez la disposition du contenu pour votre application.

<a name="Focus-and-Selection" />

## <a name="focus-and-selection"></a>La sélection

Sur Apple TV, une image, bouton ou autre élément d’interface utilisateur est considéré comme _focus_ lorsqu’il est la cible de la navigation en cours.

[![](navigation-focus-images/focus01.png "Exemple de la sélection")](navigation-focus-images/focus01.png#lightbox)

À la différence des appareils iOS où l’utilisateur interagit directement avec des éléments sur un écran tactile de l’appareil, les utilisateurs interagissent avec les éléments de tvOS à partir à travers la pièce à l’aide de l’instance distante de Siri. Pour présenter et gérer l’interaction utilisateur, l’Apple TV utilise un _Focus_ en fonction de modèle.

À l’aide de mouvements et bouton clique sur le [Siri Remote](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote), l’utilisateur déplace le focus à partir d’un élément d’interface utilisateur à un autre. Une fois que le focus est déplacé jusqu'à l’élément souhaité, l’utilisateur clique pour sélectionner le contenu ou pour activer l’action représentée par cet élément.

En tant que le focus change, animations subtiles et des effets (par exemple, l’effet parallaxe sur les images) sont utilisés pour identifier clairement l’élément d’Interface utilisateur qui a actuellement le focus.

Apple a les suggestions suivantes pour travailler avec la sélection :

- **Utiliser des contrôles d’interface utilisateur intégrée pour les effets de mouvement** : À l’aide de `UIKit` et l’API de Focus dans l’Interface utilisateur, le modèle de Focus applique automatiquement le mouvement de la valeur par défaut et les effets visuels à vos éléments d’interface utilisateur. Cela semble votre application native et familiers aux utilisateurs de la plateforme Apple TV et autorisant le déplacement fluide et intuitif entre les éléments pouvant être actif.
- **Déplacer le Focus dans les Directions attendu** -sur Apple TV, presque tous les éléments utilise la manipulation d’indirecte. Par exemple, l’utilisateur utilise le Siri Remote pour déplacer le focus et l’interface pour garder l’élément ayant actuellement le focus visible fait défiler automatiquement le système. Si votre application implémente ce type d’interaction, assurez-vous que le focus se déplace dans la direction du mouvement de l’utilisateur. Par conséquent, si vous passez de l’utilisateur directement sur le focus Siri Remote doit déplacer vers la droite (ce qui peut entraîner l’écran faire défiler vers la gauche). La seule exception à cette règle est en plein écran des éléments qui utilisent une manipulation directe (où faire glisser des remonte l’élément).
- **Vérifiez que l’élément actif est évidente** -étant donné que vos utilisateurs interagissent avec vos éléments d’interface utilisateur à distance, il est essentiel que l’élément ayant actuellement le focus faire ressortir. En général, cela est géré automatiquement par intégré `UIKit` éléments. Pour les contrôles personnalisés, utilisez les fonctionnalités telles que la taille de l’élément ou d’occulter pour afficher le focus.
- **Utilisez parallaxe à rendre actif éléments réactifs** - faible, circulaires effectuant des gestes sur l’instance distante de Siri entraînent le déplacement des gentiment, en temps réel de l’élément ayant le focus. Cela [effet parallaxe](#Focus-and-Parallax) est intégré à `UIKit` _Images superposées_ pour donner une idée de la connexion à l’élément ayant le focus à l’utilisateur.
- **Créer des éléments pouvant prendre le focus de la taille appropriée** -éléments volumineux avec un grand nombre espacement sont plus faciles à sélectionner et accédez à des éléments plus petits.
- **Conception d’élément d’interface utilisateur pour rechercher une bonne soit essentiellement ou Focused** -généralement Apple TV représente l’élément actif en augmentant sa taille. Assurez-vous que les éléments d’interface utilisateur de vos applications l’aspect souhaités à n’importe quelle taille de la présentation et, si nécessaire, fournissent des ressources pour les éléments de taille supérieure.
- **Représentent les modifications de Focus avec fluidité** -utilisez animation sans heurts en fondu entre un élément **Focused** et **Focused** état pour conserver les transitions d’être dissonant.
- **Ne pas afficher un curseur** -les utilisateurs s’attendent à naviguer de l’interface utilisateur de votre application à l’aide de Focus et ne pas en déplaçant un curseur autour de l’écran. Votre Interface utilisateur doit toujours utiliser le modèle de Focus pour présenter une expérience utilisateur cohérente.

<a name="Working-with-Focus" />

### <a name="working-with-focus"></a>Utilisation de Focus

Il peut arriver que vous souhaitez créer un contrôle personnalisé qui peut devenir un élément peut recevoir le focus. Si donc remplacer le `CanBecomeFocused` propriété et retour `true`, sinon retour `false`. Exemple :

```csharp
public class myView : UIView
{
    public override bool CanBecomeFocused {
        get {return true;}
    }
}
```

À tout moment, vous pouvez utiliser la `Focused` propriété d’un `UIKit` contrôle pour voir si elle est l’élément actuel. Si `true` l’élément d’interface utilisateur possède actuellement le focus, sinon il n’existe pas. Exemple :

```csharp
// Is my view in focus?
if (myView.Focused) {
    // Do something
    ...
}
```

Bien que vous ne pouvez pas directement déplacer le focus vers un autre élément d’interface utilisateur par le biais de code, vous pouvez spécifier l’élément d’interface obtient d’abord le focus lorsqu’un écran est chargé en définissant son `PreferredFocusedView` propriété `true`. Exemple :

```csharp
// Make the play button the starting focus item
playButton.PreferredFocusedView = true;
```

<a name="Working-with-Focus-Updates" />

### <a name="working-with-focus-updates"></a>Utilisation des mises à jour le Focus 

Lorsque l’utilisateur provoque le focus de décalage à partir d’un élément d’interface utilisateur à un autre (par exemple, en réponse à un mouvement de la télécommande Siri) un _mise à jour de focus_ sera envoyé à l’élément perd le focus et l’élément obtenant le focus.

Des éléments personnalisés qui héritent de `UIView` ou `UIViewController`, vous pouvez remplacer plusieurs méthodes pour travailler avec le focus est mise à jour :

- **DidUpdateFocus** -cette méthode sera appelée chaque fois que la vue gagne ou perd le focus.
- **ShouldUpdateFocus** -Utilisez cette méthode pour définir où le focus est autorisé à déplacer.

Pour demander que le moteur le Focus se déplace le focus revenir à la `PreferredFocusedView` élément d’interface utilisateur, appel le `SetNeedsUpdateFocus` méthode du contrôleur d’affichage.

> [!IMPORTANT]
> Appel `SetNeedsUpdateFocus` n’a d’effet si elle est appelée sur le contrôleur d’affichage contient la vue qui a actuellement le focus.




<a name="Working-with-Focus-Guides" />

### <a name="working-with-focus-guides"></a>Utilisation des Guides de Focus

Le moteur de Focus intégrées à tvOS est parfaitement pour gérer des déplacements entre des éléments qui se trouvent sur un quadrillage horizontal et vertical. En règle générale, vous avez besoin pour ne rien faire plus que d’ajouter que les éléments d’interface utilisateur pour votre conception de l’interface et le moteur de Focus automatiquement traitera le déplacement entre ces éléments sans intervention des développeurs.

Toutefois, il peut arriver, en raison de l’outils de conception de votre interface utilisateur, lorsque les éléments d’interface utilisateur ne croyez pas sur un quadrillage horizontal et vertical et peuvent être inaccessibles, car elles sont en diagonale entre eux. Cela se produit, car le moteur de Focus a été conçu pour gérer simple, vers le bas, gauche et droite mouvement entre les éléments d’interface utilisateur.

Effectuez la mise en page de l’interface utilisateur suivant pour obtenir un exemple :

 [![](navigation-focus-images/guide01.png "Utilisation de l’exemple des Guides de Focus")](navigation-focus-images/guide01.png#lightbox)
 
Étant donné que le **Infos** bouton n’est pas un quadrillage horizontal et vertical avec le **acheter** bouton celui-ci deviendra inaccessible à l’utilisateur. Toutefois, cela peut être facilement corrigé à l’aide un _Focus Guide_ pour fournir des indications de déplacement au moteur de Focus. 

Un Guide de Focus (`UIFocusGuide`) expose une zone non visible de la vue en tant que ///Focusable au moteur de Focus, permettant ainsi le Focus d’être redirigé vers une autre vue.

Par conséquent, pour résoudre l’exemple ci-dessus, le code suivant peut être ajouté au contrôleur d’affichage pour créer un repère le Focus entre la **Infos** et **acheter** boutons :

```csharp
public UIFocusGuide FocusGuide = new UIFocusGuide ();
...

public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Add Focus Guide to layout
    View.AddLayoutGuide (FocusGuide);

    // Define Focus Guide that will allow the user to move
    // between the More Info and Buy buttons.
    FocusGuide.LeftAnchor.ConstraintEqualTo (BuyButton.LeftAnchor).Active = true;
    FocusGuide.TopAnchor.ConstraintEqualTo (MoreInfoButton.TopAnchor).Active = true;
    FocusGuide.WidthAnchor.ConstraintEqualTo (BuyButton.WidthAnchor).Active = true;
    FocusGuide.HeightAnchor.ConstraintEqualTo (MoreInfoButton.HeightAnchor).Active = true;
}
```

Tout d’abord, un nouveau `UIFocusGuide` est créé et ajouté à la collection de repère de disposition de la vue à l’aide du `AddLayoutGuide` (méthode).

Ensuite, le Guide de Focus en haut, gauche, la largeur et hauteur ancres sont ajustées relatif à la **Infos** et **acheter** boutons à positionner entre eux. Consultez :

[![](navigation-focus-images/guide02.png "Guide de Focus d’exemple")](navigation-focus-images/guide02.png#lightbox)

Il est également important de noter que les nouvelles contraintes sont en cours d’activation tels qu’ils sont créés en définissant leurs `Active` propriété `true`:

```csharp
FocusGuide.LeftAnchor.ConstraintEqualTo (...).Active = true;
```

Avec le nouveau Guide de Focus établie et ajoutée à la vue, le contrôleur d’affichage `DidUpdateFocus` méthode peut être substituée et le code suivant est ajouté pour vous déplacer entre les **Infos** et **acheter** boutons :

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    base.DidUpdateFocus (context, coordinator);

    // Get next focusable item from context
    var nextFocusableItem = context.NextFocusedView;

    // Anything to process?
    if (nextFocusableItem == null) return;

    // Decide the next focusable item based on the current
    // item with focus
    if (nextFocusableItem == MoreInfoButton) {
        // Move from the More Info to Buy button
        FocusGuide.PreferredFocusedView = BuyButton;
    } else if (nextFocusableItem == BuyButton) {
        // Move from the Buy to the More Info button
        FocusGuide.PreferredFocusedView = MoreInfoButton;
    } else {
        // No valid move
        FocusGuide.PreferredFocusedView = null;
    }
}
```

Tout d’abord, cette get de code la `NextFocusedView` à partir de la `UIFocusUpdateContext` qui n’a été transmise (`context`). Si cette vue est `null`, aucun traitement n’est nécessaire, puis la méthode s’est arrêté.

Ensuite, le `nextFocusableItem` est évaluée. Si elle correspond à un le **Infos** ou **acheter** boutons, le Focus est envoyé au bouton opposé à l’aide du Guide de Focus `PreferredFocusedView` propriété. Exemple :

```csharp
// Move from the More Info to Buy button
FocusGuide.PreferredFocusedView = BuyButton;
```

Dans le cas où aucun bouton est la source du changement de Focus, le `PreferredFocusedView` propriété est désactivée :

```csharp
// No valid move
FocusGuide.PreferredFocusedView = null;
```

<a name="Working-with-Focus-in-Collections" />

### <a name="working-with-focus-in-collections"></a>Utilisation avec le Focus dans les Collections

Lorsque vous décidez ou non un élément individuel peut être actif dans un `UICollectionView` ou un `UITableView`, vous allez substituer les méthodes de la `UICollectionViewDelegate` ou `UITableViewDelegate` respectivement. Exemple :

```csharp
public class CardHandDelegate : UICollectionViewDelegateFlowLayout
{
    ...
    public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
    {
        if (indexPath == null) {
            return false;
        } else {
            var controller = collectionView as CardHandViewController;
            return !controller.Hand [indexPath.Row].IsFaceDown;
        }
    }
}
```

Le `CanFocusItem` retourne de la méthode `true` si l’élément actuel peut être en mode focus, sinon elle renvoie `false`.

Si vous souhaitez un `UICollectionView` ou un `UITableView` pour mémoriser et restaure le focus au dernier élément lorsqu’il perd et reprend le focus, définissez le `RemembersLastFocusedIndexPath` propriété `true`.

<a name="Focus-and-Parallax" />

## <a name="focus-and-parallax"></a>Le focus et parallaxe

Comme indiqué ci-dessus, un élément d’Interface utilisateur est considéré comme _focus_ lorsqu’il est l’élément actuel pendant un événement de navigation. En général comme un élément entre dans le focus, sa taille est légèrement augmenté et son élévation visuellement à l’aide d’une ombre. 

Si l’utilisateur effectue un mouvement circulaire et lent Siri à un emplacement distant, l’élément actif sera sway en temps réel en réponse à ce déplacement. Comme le sway se produit, un reflet lumineux est appliquée à son image rendre la surface s’affichent à briller. Après une durée d’inactivité donnée, tout contenu hors du focus s’estompe et l’élément actif augmentera encore plus important. 

Ces effets fonctionnent ensemble pour fournir une connexion mentale entre le contenu sur l’écran et l’utilisateur qui interagit avec l’Apple TV depuis votre canapé.

Sur Apple TV, cet effet parallaxe est utilisé dans tout le système pour transmettre une idée de profondeur 3D et dynamics ou des éléments dans le focus. tvOS utilise une série de transparent, [Images superposées](~/ios/tvos/app-fundamentals/icons-images.md#Layered-Images) qu’il déplace et s’adapte dynamiquement pour créer cet effet.

Images superposées sont requis pour l’icône de votre application tvOS et est pris en charge pour le contenu dynamique étagère de haut. Bien que non obligatoire, Apple suggère fortement implémentant des Images en couches dans tout autre élément pouvant être actif dans l’interface utilisateur de votre application.

### <a name="enabling-parallax"></a>L’activation de parallaxe

Le `UIImageView` contrôle (ou tout contrôle qui hérite de `UIImageView`) prend automatiquement en charge l’effet parallaxe. Par défaut, cette prise en charge est désactivée pour l’activer, utilisez le code suivant :

```csharp
myImageView.AdjustsImageWhenAncestorFocused = true;
```

Avec cette propriété définie sur `true`, la vue de l’Image obtient automatiquement l’effet parallaxe lorsqu’il est sélectionné par l’utilisateur et le focus.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a présenté le concept de Focus et comment elle est utilisée pour gérer la Navigation dans l’Interface utilisateur d’une application Xamarin.tvOS. Il examine l’utilisation de Focus, la mise en surbrillance et la sélection pour fournir une navigation dans les contrôles de Navigation de tvOS intégrés. Ensuite, il examiné comment le Focus peut utiliser avec parallaxe et Images en couches pour fournir des indices visuels pour l’état actuel de la Navigation à l’utilisateur final. Enfin, il a examiné travailler avec le Focus, mises à jour le Focus, le Focus dans les Collections et l’activation de parallaxe.




## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application pour tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
