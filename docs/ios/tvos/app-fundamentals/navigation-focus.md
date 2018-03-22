---
title: Utilisation de Navigation et le Focus
description: "Cet article décrit le concept de Focus et comment il est utilisé pour présenter et gérer la Navigation à l’intérieur d’une application Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: DD72E95F-AE9B-47D2-B132-5FA5FBD8026E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: fe1358d330c2a0fd94016853cedeabe094c394da
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="working-with-navigation-and-focus"></a>Utilisation de Navigation et le Focus

_Cet article décrit le concept de Focus et comment il est utilisé pour présenter et gérer la Navigation à l’intérieur d’une application Xamarin.tvOS._


Cet article décrit le concept de [Focus](#Focus-and-Selection) et comment il est utilisé pour gérer [Navigation](#Navigation) dans l’Interface utilisateur d’une application Xamarin.tvOS. Nous allons aborder l’utilisation de Focus, mise en surbrillance et la sélection pour fournir la Navigation dans l’Interface utilisateur de votre application Xamarin.tvOS dans les contrôles de Navigation de tvOS intégrés.

[![](navigation-focus-images/intro01.png "applications tvOS Navigation de l’Interface utilisateur")](navigation-focus-images/intro01.png#lightbox)

Ensuite, nous allons examiner comment le Focus peut être utilisé avec [parallaxe](#Focus-and-Parallax) et *en couche des Images* pour fournir des indices visuels à l’utilisateur final pour l’état actuel de la Navigation.

Enfin, nous allons nous intéresser à l’utilisation de [Focus](#Working-with-Focus), [mises à jour le Focus](#Working-with-Focus-Updates), [Focus Guides](#Working-with-Focus-Guides), [le Focus dans des Collections](#Working-with-Focus-in-Collections) et [ L’activation de parallaxe](#Enabling-Parallax) sur les vues d’Image dans vos applications Xamarin.tvOS.

<a name="Navigation" />

## <a name="navigation"></a>Navigation

Les utilisateurs de votre application Xamarin.tvOS pas interagir avec son interface directement en tant qu’iOS où ils appuyer sur les images sur l’écran du périphérique, mais indirectement à partir de sur l’espace à l’aide du [Siri distant](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote). Vous devrez Gardez cela à l’esprit lorsque vous concevez l’Interface utilisateur de votre application afin qu’il transite naturellement, mais conserve l’utilisateur immergé dans l’expérience d’Apple TV.

Une application réussie tvOS implémente la navigation de manière fluide prend en charge l’objectif de l’application et la structure des données, qu'il s’affiche sans attirer l’attention sur la barre de navigation. Concevoir votre navigation afin qu’il est naturel et familier sans dominent l’Interface utilisateur ou éloignez le contenu et l’expérience utilisateur applications le focus de dessin.

[![](navigation-focus-images/nav01.png "L’application de paramètres tvOS")](navigation-focus-images/nav01.png#lightbox)

Alors que, généralement à l’aide d’un Apple TV, l’utilisateur navigue dans un ensemble empilé d’écrans, chacune présentant un ensemble donné de contenu. À son tour, chaque nouvel écran peut entraîner un ou plusieurs écrans sous-chemin de contenu à l’aide de contrôles d’interface utilisateur standards comme [boutons](~/ios/tvos/user-interface/buttons.md), [onglet barres](~/ios/tvos/user-interface/tab-bars.md), Tables, [vues de Collection](~/ios/tvos/user-interface/collection-views.md) ou [ Fractionner les affichages](~/ios/tvos/user-interface/split-views.md).

Avec chaque nouvel écran de données, l’utilisateur navigue plus loin dans cette pile d’écrans. À l’aide de la **Menu** bouton sur Siri distant, ils peuvent naviguer vers l’arrière dans la pile pour revenir à un écran précédent ou le Menu principal.

Apple suggère en n’oubliant pas les éléments suivants lorsque vous créez le volet de navigation pour votre application tvOS :

- **Mise en page votre Navigation pour rendre les contenu recherche rapide et facile** -les utilisateurs à accéder au contenu au sein de votre application dans le plus petit nombre de coefficients, clique sur et fait glisser sa que possible. De simplifier votre navigation et organiser le contenu pour le nombre minimal d’écrans.
- **Créer une Interface fluide à l’aide de Touch** -vous assurer qu’un utilisateur peut déplacer entre _éléments peut être actif_ avec friction minimale en utilisant le plus petit nombre de mouvements possible.
- **Conception et en accordant à l’esprit** -étant donné que l’utilisateur interagit avec le contenu dans la salle, ils ont besoin de déplacer le Focus à un élément d’Interface utilisateur avant d’interagir avec lui à l’aide de l’instance distante de Siri. Les utilisateurs obtiennent déçus avec votre application si elle nécessite trop de mouvements pour eux à leurs objectifs.
- **Permettent une Navigation vers l’arrière via le bouton de Menu** : pour créer une expérience simple et familier, permettre aux utilisateurs de naviguer vers l’arrière à l’aide de la Siri Remote **Menu** bouton. En appuyant sur la **Menu** bouton doit toujours renvoyer à l’écran précédent ou revenir au Menu principal de l’application. Au niveau supérieur de l’application, en appuyant sur la **Menu** bouton doit retourner à l’écran d’accueil TV Apple.
- **En général, évitez d’afficher un bouton Précédent** - parce que, en appuyant sur la **Menu** bouton sur l’instance distante Siri navigue vers l’arrière dans la pile de l’écran, évitez d’affichage d’un contrôle supplémentaire qui permet de dupliquer ce comportement. Une exception à cette règle est pour l’achat d’écrans ou écrans avec des actions de destruction (comme la suppression de contenu) où un **Annuler** bouton s’affiche également.
- **Afficher des regroupements volumineux sur un seul écran, au lieu de nombreux** -le Siri à distance a été conçu pour rendre le déplacement d’une grande collection de contenu rapide et facile à l’aide de mouvements. Si votre application fonctionne avec une grande collection d’éléments peut être actif, envisagez de les conserver sur un seul écran, au lieu de les fractionner en plusieurs écrans qui requièrent une navigation plus sur la partie de l’utilisateur.
- **Utiliser des contrôles Standard pour la Navigation** -là encore, pour créer une expérience utilisateur simple et familier, dans la mesure du possible, utilisez intégré `UIKit` contrôles tels que les contrôles de Page onglet Barres, segmenté, vues des tables, vues de Collection et les commandes Split Vues pour la navigation de votre application. Étant donné que les utilisateurs sont déjà familiarisés avec ces éléments, il pourra intuitivement naviguer dans votre application.
- **Favoriser la Navigation de contenu horizontale** -en raison de la nature de l’Apple TV, passant de gauche à droite sur l’instance distante de Siri est plus naturelle que haut et bas. Envisagez cette option lorsque vous concevez la disposition du contenu pour votre application.

<a name="Focus-and-Selection" />

## <a name="focus-and-selection"></a>La sélection

Sur l’Apple TV, une image, bouton ou autre élément d’interface utilisateur est considéré comme _focus_ lorsqu’il est la cible de la navigation en cours.

[![](navigation-focus-images/focus01.png "Exemple de la sélection")](navigation-focus-images/focus01.png#lightbox)

À la différence des appareils iOS où l’utilisateur interagit directement avec des éléments sur l’écran tactile du périphérique, les utilisateurs interagissent avec les éléments tvOS à partir de sur l’espace à l’aide de l’instance distante de Siri. Pour présenter et gérer l’interaction utilisateur, l’Apple TV utilise un _Focus_ en fonction du modèle.

À l’aide du bouton et des mouvements appuie sur le [Siri distant](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote), l’utilisateur déplace le focus à partir d’un élément d’interface utilisateur à un autre. Une fois que le focus est déplacé à l’élément souhaité, l’utilisateur clique pour sélectionner le contenu ou d’activer l’action représentée par cet élément.

En tant que le focus change, effets (par exemple, l’effet parallaxe sur des images) et les animations subtiles servent à identifier clairement l’élément d’Interface utilisateur qui a actuellement le focus.

Apple a les suggestions suivantes pour travailler avec la sélection :

- **Utiliser des contrôles d’interface utilisateur intégrée pour les effets de mouvement** : À l’aide de `UIKit` et l’API de Focus dans votre Interface utilisateur, le modèle de Focus applique automatiquement le mouvement de la valeur par défaut et les effets visuels à vos éléments de l’interface utilisateur. Ainsi votre application semble natif et familière aux utilisateurs de la plateforme Apple TV et permet un déplacement fluide et intuitive entre les éléments peut être actif.
- **Déplacer le Focus dans attendu de Directions** -sur Apple TV, presque chaque élément utilise la manipulation d’indirecte. Par exemple, l’utilisateur utilise l’élément distant Siri pour déplacer le focus et l’interface pour conserver l’élément ayant actuellement le focus visible fait défiler automatiquement le système. Si votre application implémente ce type d’interaction, assurez-vous que le focus se déplace dans le sens du mouvement de l’utilisateur. Par conséquent, si vous passez de l’utilisateur à droite dans le focus à distance de Siri doit déplacer vers la droite (ce qui peut entraîner l’écran pour faire défiler vers la gauche). La seule exception à cette règle est plein écran les éléments qui utilisent une manipulation directe (où le glissement de remonte l’élément).
- **Assurez-vous que l’élément actif est évidente** -étant donné que les utilisateurs interagissent avec vos éléments d’interface utilisateur à distance, il est essentiel que soit l’élément ayant actuellement le focus. En général, cet aspect est géré automatiquement par intégré `UIKit` éléments. Pour les contrôles personnalisés, utilisez les fonctionnalités telles que la taille de l’élément ou d’occulter pour afficher le focus.
- **Utilisez parallaxe à rendre actif éléments réactive** : petite, circulaires mouvements sur l’instance distante de Siri entraîner le déplacement des abordable et en temps réel de l’élément ayant le focus. Cela [l’effet parallaxe](#Focus-and-Parallax) est intégrée à `UIKit` _en couche des Images_ pour donner une idée de la connexion à l’élément ayant le focus à l’utilisateur.
- **Créer des éléments de la taille appropriée peut être actif** -éléments de taille importante avec espacement suffisamment sont plus faciles à sélectionner et parcourir les éléments plus petits.
- **Conception d’élément d’interface utilisateur pour rechercher une bonne soit actif ou Focused** -généralement Apple TV représente l’élément actif en augmentant sa taille. Assurez-vous que les éléments d’interface utilisateur de vos applications superbes à n’importe quelle taille de présentation et, si nécessaire, fournissent des ressources pour les éléments de tailles supérieure.
- **Représentent les modifications de Focus efficacement** -utilisez animation disparaître progressivement entre un élément **actif** et **Focused** état pour conserver les transitions d’être glacial.
- **Ne pas afficher un curseur** -les utilisateurs attendent naviguer dans l’interface utilisateur de votre application à l’aide de Focus et non par le déplacement d’un curseur autour de l’écran. Votre Interface utilisateur doit toujours utiliser le Focus modèle pour présenter une expérience utilisateur cohérente.

<a name="Working-with-Focus" />

### <a name="working-with-focus"></a>Utilisation avec Focus

Il peut arriver que vous souhaitez créer un contrôle personnalisé qui peut devenir un élément peut être actif. Si donc remplacer le `CanBecomeFocused` propriété et retour `true`, sinon retour `false`. Exemple :

```csharp
public class myView : UIView
{
    public override bool CanBecomeFocused {
        get {return true;}
    }
}
```

À tout moment, vous pouvez utiliser la `Focused` propriété d’un `UIKit` contrôle pour voir si elle est l’élément actuel. Si `true` l’élément d’interface utilisateur a actuellement le focus, sinon il n’existe pas. Exemple :

```csharp
// Is my view in focus?
if (myView.Focused) {
    // Do something
    ...
}
```

Pendant que vous ne pouvez pas directement déplacer le focus vers un autre élément d’interface utilisateur via le code, vous pouvez spécifier l’élément d’interface obtient d’abord le focus lorsqu’un écran est chargé en définissant son `PreferredFocusedView` propriété `true`. Exemple :

```csharp
// Make the play button the starting focus item
playButton.PreferredFocusedView = true;
```

<a name="Working-with-Focus-Updates" />

### <a name="working-with-focus-updates"></a>Utilisation des mises à jour le Focus 

Lorsque l’utilisateur provoque le focus sur le décalage à partir d’un élément d’interface utilisateur à un autre (par exemple, en réponse à un mouvement sur l’instance distante Siri) un _mise à jour de focus_ sera envoyé à l’élément perd le focus et l’élément obtention du focus.

Pour des éléments personnalisés qui héritent de `UIView` ou `UIViewController`, vous pouvez substituer des méthodes pour travailler avec le focus est mise à jour :

- **DidUpdateFocus** -cette méthode sera appelée à tout moment la vue les gains ou perd le focus.
- **ShouldUpdateFocus** -Utilisez cette méthode pour définir où le focus est autorisé à déplacer.

Pour demander que le moteur le Focus se déplace le focus jusqu'à la `PreferredFocusedView` élément d’interface utilisateur, appel de la `SetNeedsUpdateFocus` méthode du contrôleur d’affichage.

> [!IMPORTANT]
> Appel de `SetNeedsUpdateFocus` n’a d’effet si le contrôleur de vue il est appelé sur contient la vue qui possède actuellement le focus.




<a name="Working-with-Focus-Guides" />

### <a name="working-with-focus-guides"></a>Utilisation des Guides de Focus

Le moteur de Focus intégrées tvOS est une bonne solution pour la gestion des déplacements entre des éléments qui se trouvent sur un quadrillage horizontal et vertical. En règle générale, vous devez ne rien faire plus que d’ajouter que les éléments d’interface utilisateur pour votre conception de l’interface et le moteur de Focus gère automatiquement le déplacement entre ces éléments sans intervention des développeurs.

Toutefois, il peut arriver, en raison de l’outils de conception de votre interface utilisateur, lorsque les éléments d’interface utilisateur ne se trouvent sur un quadrillage horizontal et vertical et peuvent être inaccessibles, car ils sont en diagonale entre eux. Cela se produit, car le moteur de Focus a été conçu pour gérer simple haut, bas, gauche et droite déplacement entre les éléments d’interface utilisateur.

Effectuez la mise en page de l’interface utilisateur suivant pour obtenir un exemple :

 [![](navigation-focus-images/guide01.png "Utilisation de l’exemple de repères de Focus")](navigation-focus-images/guide01.png#lightbox)
 
Étant donné que la **informations** bouton n’est pas un quadrillage horizontal et vertical avec le **acheter** bouton il serait inaccessible à l’utilisateur. Toutefois, cela peut être facilement corrigée à l’aide un _Focus Guide_ pour fournir des indicateurs de mouvements au moteur de Focus. 

Un repère de Focus (`UIFocusGuide`) expose une zone non visible de la vue en tant que ///Focusable au moteur de Focus, permettant ainsi le Focus à être redirigées vers une autre vue.

Par conséquent, pour résoudre l’exemple ci-dessus, le code suivant pu être ajouté au contrôleur de vue pour créer un repère de Focus entre le **informations** et **acheter** boutons :

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

Tout d’abord, un nouveau `UIFocusGuide` est créé et ajouté à la collection du Guide de mise en page de l’affichage à l’aide du `AddLayoutGuide` (méthode).

Ensuite, le Guide de Focus haut, gauche, largeur et hauteur ancres sont ajustées relatif à la **informations** et **acheter** boutons à placer entre eux. Consultez :

[![](navigation-focus-images/guide02.png "Guide de Focus d’exemple")](navigation-focus-images/guide02.png#lightbox)

Il est également important de noter que les nouvelles contraintes sont en cours d’activation comme elles sont créées en définissant leurs `Active` propriété `true`:

```csharp
FocusGuide.LeftAnchor.ConstraintEqualTo (...).Active = true;
```

Dans le Guide de Focus nouveau établie et ajoutée à la vue, le contrôleur de la vue `DidUpdateFocus` méthode peut être substituée et le code suivant est ajouté pour vous déplacer entre les **informations** et **acheter** boutons :

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

Tout d’abord, cette get de code la `NextFocusedView` à partir de la `UIFocusUpdateContext` qui a été passé (`context`). Si cette vue est `null`, aucun traitement n’est nécessaire, puis la méthode s’est arrêté.

Ensuite, le `nextFocusableItem` est évaluée. Si elle correspond à un le **informations** ou **acheter** boutons, le Focus est envoyé au bouton opposé à l’aide du Guide de Focus `PreferredFocusedView` propriété. Exemple :

```csharp
// Move from the More Info to Buy button
FocusGuide.PreferredFocusedView = BuyButton;
```

Dans le cas où aucun bouton est la source de l’équipe le Focus, le `PreferredFocusedView` propriété est désactivée :

```csharp
// No valid move
FocusGuide.PreferredFocusedView = null;
```

<a name="Working-with-Focus-in-Collections" />

### <a name="working-with-focus-in-collections"></a>Utilisation de Focus dans des Collections

Lorsque vous décidez ou non un élément individuel peut être actif dans un `UICollectionView` ou un `UITableView`, vous devez substituer les méthodes de la `UICollectionViewDelegate` ou `UITableViewDelegate` respectivement. Exemple :

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

Le `CanFocusItem` méthode retourne `true` si l’élément actuel peut être activé, sinon il renvoie `false`.

Si vous souhaitez un `UICollectionView` ou un `UITableView` pour mémoriser et à restaurer le focus au dernier élément lorsqu’il perd et reprend, définissez la `RemembersLastFocusedIndexPath` propriété `true`.

<a name="Focus-and-Parallax" />

## <a name="focus-and-parallax"></a>Le focus et parallaxe

Comme indiqué ci-dessus, un élément d’Interface utilisateur est considéré comme étant _focus_ lorsqu’il est l’élément actif pendant un événement de navigation. En général, comme un élément apparaît, sa taille augmente légèrement, et son élévation visuellement à l’aide d’une ombre. 

Si l’utilisateur effectue un mouvement lent, circulaire sur Siri à distance, l’élément actif sera balancement en temps réel en réponse à ce déplacement. Le balancement se produit, un reflet lumineux est appliquée à son image de rendre la surface apparaissent se traduisent. Après un délai d’inactivité donné, tout contenu hors du focus estompe et l’élément actif augmentera supérieur. 

Ces effets fonctionnent ensemble pour fournir une connexion changer entre le contenu sur l’écran et l’utilisateur qui interagit avec l’Apple TV membre.

Sur l’Apple TV, cet effet parallaxe est utilisé dans l’ensemble du système pour transmettre une idée de profondeur 3D et dynamics pour les éléments dans le focus. tvOS utilise une série de transparent, [en couche des Images](~/ios/tvos/app-fundamentals/icons-images.md#Layered-Images) qu’il déplace et ajuste dynamiquement pour créer cet effet.

Les Images en couches sont requis pour l’icône de votre application tvOS et est pris en charge pour le contenu en haut de l’étagère dynamique. Bien que non obligatoire, Apple recommande vivement d’implémentation en couche des Images dans tous les autres éléments dans l’interface utilisateur de votre application peut être actif.

### <a name="enabling-parallax"></a>L’activation de parallaxe

Le `UIImageView` contrôle (ou n’importe quel contrôle hérite `UIImageView`) prend en charge automatiquement l’effet parallaxe. Par défaut, cette prise en charge est désactivée pour l’activer, utilisez le code suivant :

```csharp
myImageView.AdjustsImageWhenAncestorFocused = true;
```

Avec cette propriété la valeur `true`, l’Image recevra automatiquement l’effet parallaxe lorsqu’il est sélectionné par l’utilisateur et le focus.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a couvert le concept de Focus et comment il est utilisé pour gérer la Navigation dans l’Interface utilisateur d’une application Xamarin.tvOS. Il examiner l’utilisation de Focus, mise en surbrillance et la sélection pour fournir une navigation dans les contrôles de Navigation de tvOS intégrés. Ensuite, il examiné comment le Focus peuvent être utilisé avec parallaxe et les Images de couches pour fournir des indices visuels à l’utilisateur final pour l’état actuel de la Navigation. Enfin, elle examinait travailler avec le Focus, les mises à jour le Focus, le Focus dans l’activation de parallaxe et les Collections.




## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS les Guides Interface humaine](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
