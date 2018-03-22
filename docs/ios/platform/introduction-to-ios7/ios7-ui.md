---
title: "iOS 7 présentation de l’Interface utilisateur"
description: "iOS 7 introduit une multitude de modifications de l’interface utilisateur. Cet article présente certaines des modifications plus volumineuses, à la fois dans l’apparence visuelle des contrôles et les API qui prennent en charge la nouvelle conception."
ms.topic: article
ms.prod: xamarin
ms.assetid: FADCEA7C-8968-42A1-9E9E-F4BBAB7BCF2C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 3d70aff4df91120402e2987598b8973172b46245
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="ios-7-user-interface-overview"></a>iOS 7 présentation de l’Interface utilisateur

_iOS 7 introduit une multitude de modifications de l’interface utilisateur. Cet article présente certaines des modifications plus volumineuses, à la fois dans l’apparence visuelle des contrôles et les API qui prennent en charge la nouvelle conception._

iOS 7 se concentre sur le contenu sur chrome. Éléments d’interface utilisateur dans iOS 7 souligner chrome en supprimant des attributs, tels que les bordures superflus, les barres d’état et les barres de navigation, qui réduisent la quantité d’espace utilisé par les modes de contenu à l’écran. Dans iOS 7, le contenu est conçu pour utiliser la totalité de l’écran.

iOS 7 présente plusieurs autres modifications : couleur est utilisée pour distinguer les éléments d’interface utilisateur, à la place d’attributs, tels que les bordures du bouton. Plusieurs éléments, tels que les barres de navigation et les barres d’état, sont désormais flou et translucide ou transparent, affichage du contenu zone en dessous de prise. Ces vues contenus rendu via les barres floues, de transporter une impression de profondeur dans l’interface utilisateur.

Cet article décrit plusieurs des modifications aux éléments d’interface utilisateur dans iOS que 7, ainsi que diverses API liées à la nouvelle conception d’interface utilisateur.

## <a name="view-and-control-changes"></a>Afficher et contrôler les modifications

Toutes les vues dans UIKit est conforme à la nouvelle apparence d’iOS 7. Cette section présente certaines des modifications à ces vues, ainsi que les API associées qui ont été modifiés pour prendre en charge la nouvelle interface utilisateur.

### <a name="uibutton"></a>UIButton

Boutons créés à partir de la `UIButton` classe sont désormais sans bordure, avec aucun arrière-plan par défaut, comme indiqué ci-dessous :

 ![](ios7-ui-images/button.png "Exemple UIButton")

Le `UIButtonType.RoundedRect` style a été déconseillé. Si utilisé dans iOS 7, `UIButtonType.RoundedRect` entraîne `UIButtonType.System` utilisée, ce qui donne le style de bouton par défaut sans en arrière-plan ou les bords visibles, comme indiqué ci-dessus.

### <a name="uibarbuttonitem"></a>UIBarButtonItem

Semblable à `UIButton`, barre de boutons sont également sans bordure, par défaut au nouveau `UIBarButtonItemStyle.Plain` style indiqué ci-dessous :

 ![](ios7-ui-images/barbuttonplain.png "Exemple UIBarButtonItem")

En outre, le `UIBarButtonItemStyle.Bordered` style a été déconseillé. Paramètre `UIBarButtonItemStyle.Bordered` dans iOS 7 entraîne la `UIBarButtonItemStyle.Plain` de style qui est utilisé.

Le `UIBarButtonItemStyle.Done` style n’a pas été déconseillé. Toutefois, il crée également un bouton sans bordure, uniquement avec un style de texte en gras, comme indiqué :

 ![](ios7-ui-images/barbuttondone.png "UIBarButtonItem d’exemple dans le style terminé")

### <a name="uialertview"></a>UIAlertView

En plus de la modification de style pour la nouvelle iOS 7 apparence, affichages des alertes plus en charge la personnalisation via sous-affichage. Bien que `UIAlertView` hérite `UIView`, l’appel `AddSubview` sur un `UIAlertView` n’a aucun effet. Considérons par exemple le code suivant :

```csharp
UIBarButtonItem button = new UIBarButtonItem ("Bar Button", UIBarButtonItemStyle.Plain, (s,e) =>
{
    UIAlertView alert = new UIAlertView ("Title", "Message", null, "Cancel", "OK");

    alert.AddSubview (new UIView () {
        Frame = new CGRect(50, 50,100, 100),
        BackgroundColor = UIColor.Green
    });

    alert.Show ();
});
```

Cela génère un affichage des alertes standard, avec le sous-affichage ignoré, comme indiqué ci-dessous :

 ![](ios7-ui-images/alert.png "Exemple UIAlertView")
 
 Remarque : UIAlertView a été déconseillée dans iOS 8. Afficher le [contrôleur alerte](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/) recette sur l’utilisation d’un affichage des alertes dans iOS 8 et versions ultérieures.

### <a name="uisegmentedcontrol"></a>UISegmentedControl

Les contrôles segmentés dans iOS 7 sont transparents et prend en charge de la couleur de teinte. La couleur de teinte est utilisée pour la couleur du texte et la bordure. Lorsqu’un segment est sélectionné, la couleur est échangée entre l’arrière-plan et le texte, avec la couleur de teinte utilisée pour mettre en surbrillance le segment sélectionné, comme indiqué ci-dessous :

 ![](ios7-ui-images/segmentedcontrol.png "Exemple UISegmentedControl")

En outre, la `UISegmentedControlStyle` a été déconseillée dans iOS 7.

### <a name="picker-views"></a>Vues du sélecteur

L’API pour les vues de sélecteur est en grande partie inchangée ; Toutefois, iOS 7 la conception d’état maintenant doivent être présentés inline dans les vues de sélecteur plutôt que comme animée de vues d’entrée de bas de l’écran ou via un nouveau contrôleur d’envoyées dans la pile d’un contrôleur de navigation comme dans iOS précédentes versions. Ceci peut être observé dans l’application de calendrier système :

 ![](ios7-ui-images/inlinepicker.png "Ceci peut être observé dans l’application de calendrier système")

### <a name="uisearchdisplaycontroller"></a>UISearchDisplayController

La barre de recherche est à présent affichée dans le volet de navigation barre lorsque le `UISearchDisplayController.DisplaysSearchBarInNavigationBar` est définie sur true. Lorsque la valeur false - la valeur par défaut, la barre de navigation est masquée lorsque le contrôleur de recherche s’affiche.

La capture d’écran suivante montre la barre de recherche au sein d’un `UISearchDisplayController`:

 ![](ios7-ui-images/searchbar.png "Exemple UISearchDisplayController")

### <a name="uitableview"></a>UITableView

Les API autour `UITableView` sont principalement inchangé ; Toutefois, le style a changé considérablement pour être conforme à la nouvelle conception d’interface utilisateur. La hiérarchie interne est également quelque peu différente. Cette modification n’affecte pas la plupart des applications, mais il est un élément à connaître.

#### <a name="grouped-table-style"></a>Style de tableau groupée

Le style groupé modifié a mis à jour avec le contenu maintenant étendre les bords de l’écran comme indiqué ci-dessous :

 ![](ios7-ui-images/table1.png "Style de tableau groupée Sample")

#### <a name="separatorinset"></a>SeparatorInset

Séparateurs de lignes peuvent être mis en retrait en définissant le `UITableVIewCell.SeparatorInset` propriété. Par exemple, le code suivant serait permet de mettre en retrait les cellules à partir du bord gauche :

```csharp
cell.SeparatorInset = new UIEdgeInsets (0, 50, 0, 0);
```

Cela se produit dans la vue de la table avec les cellules mises en retrait comme indiqué ci-dessous :

 ![](ios7-ui-images/separatorinset.png "Exemple UITableView SeparatorInset")

#### <a name="table-button-styles"></a>Styles de bouton de table

Tous les boutons différents utilisés dans les affichages de tableau ont changé. La capture d’écran suivante présente une vue de table en mode de modification :

 ![](ios7-ui-images/table2.png "Cette capture d’écran présente une vue de table en mode de modification")

### <a name="additional-control-changes"></a>Contrôle des modifications

Autres contrôles UIKit ont changé, y compris les curseurs, les commutateurs et les exécutions pas à pas. Ces modifications sont purement visuel. Pour plus d’informations, reportez-vous à Apple [Guide de Transition de l’interface utilisateur iOS 7](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/TransitionGuide/index.html).

## <a name="general-user-interface-changes"></a>Modifications généraux de l’Interface utilisateur

Outre les modifications dans UIKit, iOS 7 présente diverses modifications visuelles à l’interface utilisateur, y compris :

-  Contenu de l’écran complet
-  Apparence de la barre
-  Couleur de teinte

<a name="fullscreen" />

### <a name="full-screen-content"></a>Contenu de plein écran

iOS 7 est conçu pour permettent aux applications de tirer parti de l’écran entier. Affichage des contrôleurs apparaissent maintenant avec chevauchement par une barre d’état et la barre de navigation - s’il en existe - et non pas sous les barres d’état et la navigation.

Lorsque vous préparez votre application pour iOS 7, vous pouvez réalignez sous-vues visuellement à l’aide de *Interface Générateur* ou *Xamarin iOS concepteur*. Vous pouvez également utiliser une des nouvelles API pour aider à gérer le contenu de plein écran par programmation. Ces API est présentés ci-dessous.

#### <a name="toplayoutguide-and-bottomlayoutguide"></a>TopLayoutGuide et BottomLayoutGuide

 `TopLayoutGuide` et `BottomLayoutGuide` servir de référence pour les où doit commencer ou se terminer, afin que le contenu n’est pas chevauché par un translucide `UIKit` barre, comme dans l’exemple suivant :

 [![](ios7-ui-images/clipped.png "Exemple de contenu ne chevauché ne pas par une barre UIKit translucide")](ios7-ui-images/clipped.png#lightbox)

Ces API peut être utilisé pour calculer le déplacement d’une vue du haut ou le bas de l’écran et ajuster en conséquence de positionnement du contenu :

```csharp
public override void ViewDidLayoutSubviews ()
{
    base.ViewDidLayoutSubviews ();

    if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
        nfloat displacement_y = this.TopLayoutGuide.Length;

        //load subviews with displacement
    }

}
```

Nous pouvons utiliser la valeur calculée ci-dessus pour définir notre `ImageView`de déplacement à partir du haut de l’écran, par conséquent, l’ensemble de l’image est visible :

 [![](ios7-ui-images/good2.png "Déplacement de ImageViews exemple à partir du haut de l’écran")](ios7-ui-images/good2.png#lightbox)

Reportez-vous à la [afficheur d’images](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) pour obtenir un exemple fonctionnel.

La valeur de déplacement est générée dynamiquement une fois que la vue a été ajoutée à la hiérarchie, par conséquent, la tentative de lecture `TopLayoutGuide` et `BottomLayoutGuide` valeurs `ViewDidLoad` retourne 0. Calculer la valeur après l’affichage a chargé - par exemple, dans le `ViewDidLayoutSubviews`.

> [!IMPORTANT]
> `TopLayoutGuide` et `BottomLayoutGuide` sont déconseillés dans iOS 11 en faveur de la nouvelle disposition de la zone de sécurité. Apple indiqué qu’à l’aide de la zone de sans échec est compatible avec la version d’iOS antérieure à iOS 11. Pour plus d’informations, consultez la [mise à jour de votre application pour iOS 11](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md#fullscreen) guide.

#### <a name="edgesforextendedlayout"></a>EdgesForExtendedLayout

Cette API indique quels bords d’une vue doivent être étendus en plein écran, indépendamment de la transparence de la barre. Dans iOS 7, les barres d’outils et les barres de navigation apparaissent disposés en vue du contrôleur - Contrairement à dans iOS précédentes versions, où ils n’occupent le même espace. L’application de Photos iOS 7 illustre la valeur par défaut `UIViewController.EdgesForExtendedLayout` valeur, `UIRectEdge.All`. Ce paramètre remplit les quatre bords dans la vue de contenu, de création de l’effet qui se chevauchent et plein écran :

 [![](ios7-ui-images/photos.png "Exemple EdgesForExtendedLayout")](ios7-ui-images/photos.png#lightbox)

Si vous appuyez sur l’image supprime les barres et montre l’image plein écran :

 [![](ios7-ui-images/photos2.png "EdgesForExtendedLayout avec les barres supprimé")](ios7-ui-images/photos2.png#lightbox)

Contenu de plein écran est la valeur par défaut, les applications configurées pour iOS 6 aura donc partie de la vue détourée, comme dans la capture d’écran ci-dessous :

 [![](ios7-ui-images/clipped.png "Applications configurées pour iOS 6 aura une partie de la vue détourée, comme dans cette capture d’écran")](ios7-ui-images/clipped.png#lightbox)

Modification de la `UIViewController.EdgesForExtendedLayout` propriété ajuste pour ce comportement. Nous pouvons spécifier que la vue ne remplisse pas les bords, notre vue sera ainsi éviter d’affichage du contenu dans l’espace occupé par la navigation ou les barres d’outils (à chaque orientation) :

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
    this.EdgesForExtendedLayout = UIRectEdge.None;
}
```

Dans notre application, nous verrons que la vue est repositionnée à nouveau, l’ensemble de l’image est visible :

 [![](ios7-ui-images/good.png "Exemple avec l’ensemble de l’image visible")](ios7-ui-images/good.png#lightbox)

Notez que pendant les effets de la `TopLayoutGuide/BottomLayoutGuide` et `EdgesForExtendedLayout` API est similaires, ils sont conçus pour remplir des objectifs différents. Modification de la `EdgesForExtendedLayout` paramètre à partir de la valeur par défaut peut résoudre les vues découpées dans les applications conçues pour iOS 6, mais une conception bon iOS 7 doit respecter l’esthétique plein écran et fournir l’expérience d’affichage, la partie de confiance sur le plein écran `TopLayoutGuide` et `BottomLayoutGuide`pour positionner correctement le contenu qui est destiné à être manipulé dans un endroit à l’aise pour l’utilisateur.

Reportez-vous à la [afficheur d’images](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) pour obtenir un exemple fonctionnel.

### <a name="status-and-navigation-bars"></a>État et les barres de Navigation

La barre d’état et les barres de navigation sont rendus avec une transparence. Barres d’état sont transparents, barres d’outils et les barres de navigation sont translucide et floue pour acheminer l’impression de profondeur dans l’interface utilisateur. La capture d’écran suivante montre cette flou et transparence, où la couleur d’arrière-plan bleu de la vue de collection par le biais des barres de l’état et la navigation, en leur donnant une apparence bleue claire :

 ![](ios7-ui-images/transparent-navbar.png "Exemple d’état et barre flou de Navigation")

#### <a name="status-bar-styles"></a>Styles de barre d’état

Ainsi que la transparence et de flou, au premier plan de la barre d’état peut être clair ou sombre (sombre étant la valeur par défaut). Le style de barre d’état peut être défini à partir du contrôleur de la vue. Un contrôleur de la vue peut également définir si la barre d’état est masquée ou affichée.

Par exemple, le code suivant substitue la `PreferredStatusBarStyle` méthode d’un contrôleur d’affichage pour que la barre d’état s’affiche un arrière-plan clair :

```csharp
public override UIStatusBarStyle PreferredStatusBarStyle ()
{
    return UIStatusBarStyle.LightContent;
}
```

Cela provoque la barre d’état apparaissent comme ci-dessous :

 ![](ios7-ui-images/light-status-bar.png "Exemple de barre d’état")

Pour masquer la barre d’état à partir du code du contrôleur de la vue, vous devez substituer `PrefersStatusBarHidden`, comme indiqué ci-dessous :

```csharp
public override bool PrefersStatusBarHidden ()
{
    return true;
}
```

Cela masque la barre d’état :

 ![](ios7-ui-images/status-bar-hidden.png "Barre d’état masquée")

### <a name="tint-color"></a>Couleur de teinte

Boutons sont affichent sous forme de texte sans chrome. La couleur du texte peut être contrôlée à l’aide de la nouvelle `TintColor` propriété sur `UIView`. Définition de la `TintColor` applique la couleur de la vue qui définit la hiérarchie entière. Pour appliquer un `TintColor`tout au long d’une application, définissez sur le `Window`. Vous pouvez également détecter lorsque la couleur de teinte modifiée la `UIView.TintColorDidChange` (méthode).

Par exemple, la capture d’écran suivante montre l’effet de la modification de la couleur de teinte dans la vue d’un contrôleur navigation en violet :

 ![](ios7-ui-images/tint-color.png "Couleur de teinte violette sur une vue de contrôleurs de navigation")

La couleur de teinte permettre être appliquée aux images ainsi lorsque la `RenderingMode` a la valeur `UIImageRenderingMode.AlwaysTemplate`.

> [!IMPORTANT]
> Couleur de teinte ne peut pas être définie à l’aide de `UIAppearance`.


### <a name="dynamic-type"></a>Type dynamique

Dans iOS 7, l’utilisateur peut spécifier de taille du texte dans les paramètres du système. Avec le type dynamique, la police est ajustée dynamiquement bonnes, quelle que soit la taille. `UIFont.PreferredFontForTextStyle` doit être utilisé pour obtenir une police est optimisée pour la taille contrôlée par l’utilisateur.

## <a name="summary"></a>Récapitulatif

Cet article décrit les modifications apportées aux éléments d’interface utilisateur dans iOS 7. Il examine plusieurs modifications apportées aux affichages et aux contrôles dans UIKit, mise en surbrillance de l’élément visuel change ainsi que les modifications pour les API associées. Enfin, il présente les nouvelles API pour travailler avec le contenu de l’écran complet, nouvelle prise en charge de couleur teinte et type dynamique.

## <a name="related-links"></a>Liens associés

- [Afficheur d’images (exemple)](https://developer.xamarin.com/samples/monotouch/iOS7-ui-updates)
