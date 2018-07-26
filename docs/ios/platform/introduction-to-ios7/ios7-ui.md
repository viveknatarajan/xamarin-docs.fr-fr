---
title: iOS 7 présentation de l’Interface utilisateur
description: iOS 7 introduit une multitude de modifications de l’interface utilisateur. Cet article met en évidence certaines des modifications plus volumineuses, à la fois dans l’apparence visuelle des contrôles et les API qui prennent en charge la nouvelle conception.
ms.prod: xamarin
ms.assetid: FADCEA7C-8968-42A1-9E9E-F4BBAB7BCF2C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 7e7725418ed104bd9be014b80d20fd62de144ca5
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242288"
---
# <a name="ios-7-user-interface-overview"></a>iOS 7 présentation de l’Interface utilisateur

_iOS 7 introduit une multitude de modifications de l’interface utilisateur. Cet article met en évidence certaines des modifications plus volumineuses, à la fois dans l’apparence visuelle des contrôles et les API qui prennent en charge la nouvelle conception._

iOS 7 se concentre sur le contenu sur chrome. Éléments d’interface utilisateur dans iOS 7 suppriment chrome en supprimant des attributs tels que les bordures superflus, les barres d’état et les barres de navigation, ce qui réduit la quantité d’espace utilisé par les modes de contenu à l’écran. Dans iOS 7, le contenu est conçu pour utiliser tout l’écran.

iOS 7 présente plusieurs autres modifications : couleur est utilisée pour distinguer les éléments d’interface utilisateur, à la place d’attributs tels que les bordures de bouton. Nombreux éléments, tels que les barres de navigation et des barres d’état, sont désormais flous et translucides ou transparentes, avec des vues contenus zone en dessous de prise. Ces vues contenus rendu via les barres de flous, transmettre un sentiment de profondeur dans l’interface utilisateur.

Cet article décrit plusieurs des modifications aux éléments d’interface utilisateur dans iOS que 7, ainsi que diverses API liées à la nouvelle conception d’interface utilisateur.

## <a name="view-and-control-changes"></a>Afficher et contrôler les modifications

Toutes les vues dans UIKit est conforme à la nouvelle apparence d’iOS 7. Cette section met en évidence certaines des modifications à ces vues, ainsi que les API associées qui ont été modifiés pour prendre en charge la nouvelle interface utilisateur.

### <a name="uibutton"></a>UIButton

Boutons créés à partir de la `UIButton` classe sont désormais sans bordure, avec aucun arrière-plan par défaut, comme indiqué ci-dessous :

 ![](ios7-ui-images/button.png "Exemple UIButton")

Le `UIButtonType.RoundedRect` style a été déconseillé. Si utilisé dans iOS 7, `UIButtonType.RoundedRect` entraîne `UIButtonType.System` utilisé, ce qui génère le style de bouton par défaut sans arrière-plan ou les bords visibles, comme indiqué ci-dessus.

### <a name="uibarbuttonitem"></a>UIBarButtonItem

Semblable à `UIButton`, barre de boutons sont également sans bordure, par défaut au nouveau `UIBarButtonItemStyle.Plain` style indiqué ci-dessous :

 ![](ios7-ui-images/barbuttonplain.png "Exemple UIBarButtonItem")

En outre, le `UIBarButtonItemStyle.Bordered` style a été déconseillé. Paramètre `UIBarButtonItemStyle.Bordered` dans iOS 7 entraîne la `UIBarButtonItemStyle.Plain` de style qui est utilisé.

Le `UIBarButtonItemStyle.Done` style n’a pas été déconseillé. Toutefois, il crée également un bouton sans bordure, uniquement avec un style de texte en gras, comme indiqué :

 ![](ios7-ui-images/barbuttondone.png "UIBarButtonItem exemple dans le style terminé")

### <a name="uialertview"></a>UIAlertView

En plus de la modification du style pour la nouvelle iOS 7 apparence, affichages des alertes plus en charge la personnalisation via sous-affichage. Même si `UIAlertView` hérite `UIView`, l’appel `AddSubview` sur un `UIAlertView` n’a aucun effet. Considérons par exemple le code suivant :

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

Cela génère un affichage des alertes standard, avec le sous-affichage qui sont ignoré, comme indiqué ci-dessous :

 ![](ios7-ui-images/alert.png "Exemple UIAlertView")
 
 Remarque : UIAlertView a été déconseillé dans iOS 8. Afficher le [contrôleur d’alerte](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller) Recipe (Recette) sur l’utilisation d’un affichage des alertes dans iOS 8 et versions ultérieures.

### <a name="uisegmentedcontrol"></a>UISegmentedControl

Contrôles segmentés dans iOS 7 sont transparents et prendre en charge de la couleur de teinte. La couleur de teinte est utilisée pour la couleur de texte et la bordure. Lorsqu’un segment est sélectionné, la couleur est échangée entre l’arrière-plan et le texte, avec la couleur de teinte utilisée pour mettre en évidence le segment sélectionné, comme indiqué ci-dessous :

 ![](ios7-ui-images/segmentedcontrol.png "Exemple UISegmentedControl")

En outre, le `UISegmentedControlStyle` a été déconseillée dans iOS 7.

### <a name="picker-views"></a>Vues du sélecteur

L’API pour les vues de sélecteur est en grande partie inchangée ; Toutefois, les instructions de conception 7 iOS indiquent désormais vues de sélecteur doivent être présentés en ligne plutôt que comme vues d’entrée est animé de bas de l’écran ou via un nouveau contrôleur placé pile d’un contrôleur de navigation, comme dans iOS précédentes versions. Ceci peut être observé dans l’application de calendrier système :

 ![](ios7-ui-images/inlinepicker.png "Ceci peut être observé dans l’application de calendrier")

### <a name="uisearchdisplaycontroller"></a>UISearchDisplayController

La barre de recherche apparaît désormais dans le volet de navigation barre lorsque le `UISearchDisplayController.DisplaysSearchBarInNavigationBar` propriété est définie sur true. Lorsque la valeur false : la valeur par défaut, la barre de navigation est masquée lorsque le contrôleur de la recherche s’affiche.

La capture d’écran suivante montre la barre de recherche au sein d’un `UISearchDisplayController`:

 ![](ios7-ui-images/searchbar.png "Exemple UISearchDisplayController")

### <a name="uitableview"></a>UITableView

Les API autour `UITableView` sont principalement inchangé ; Toutefois, le style a changé considérablement pour se conformer à la nouvelle conception d’interface utilisateur. La hiérarchie d’affichage interne est également quelque peu différente. Cette modification n’affecte pas la plupart des applications, mais il est important d’être conscient.

#### <a name="grouped-table-style"></a>Style de Table d’enregistrements regroupés

Le style groupé modifié a mis à jour avec le contenu maintenant extension sur les bords de l’écran comme indiqué ci-dessous :

 ![](ios7-ui-images/table1.png "Style de Table d’enregistrements regroupés exemple")

#### <a name="separatorinset"></a>SeparatorInset

Les séparateurs de lignes peuvent être mis en retrait en définissant le `UITableVIewCell.SeparatorInset` propriété. Par exemple, le code suivant serait être utilisé pour mettre en retrait les cellules à partir du bord gauche :

```csharp
cell.SeparatorInset = new UIEdgeInsets (0, 50, 0, 0);
```

Ceci se produit dans la vue de table avec les cellules mises en retrait comme indiqué ci-dessous :

 ![](ios7-ui-images/separatorinset.png "Exemple UITableView SeparatorInset")

#### <a name="table-button-styles"></a>Styles de bouton de table

Tous les boutons différents utilisés dans les affichages de tableau ont changé. La capture d’écran suivante présente une vue de table en mode de modification :

 ![](ios7-ui-images/table2.png "Cette capture d’écran présente une vue de table en mode de modification")

### <a name="additional-control-changes"></a>Modifications de contrôle supplémentaires

Autres contrôles UIKit ont changé, y compris les curseurs, les commutateurs et les exécutions pas à pas. Ces modifications sont purement visuel. Pour plus d’informations, reportez-vous à Apple [Guide de Transition de l’interface utilisateur iOS 7](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/TransitionGuide/index.html).

## <a name="general-user-interface-changes"></a>Modifications de généraux de l’Interface utilisateur

En plus des modifications dans UIKit, iOS 7 propose des modifications visuelles à l’interface utilisateur, y compris :

-  Contenu de plein écran
-  Apparence de la barre
-  Couleur de teinte

<a name="fullscreen" />

### <a name="full-screen-content"></a>Contenu de plein écran

iOS 7 est conçu pour permettre aux applications de tirer parti de l’écran entier. Contrôleurs d’affichage apparaissent désormais avec chevauchement par une barre d’état et la barre de navigation - s’il en existe - et non pas sous les barres d’état et de navigation.

Lorsque vous vous préparez votre application pour iOS 7, vous pouvez réaligner sous-vues visuellement à l’aide *Interface Builder* ou *Xamarin iOS concepteur*. Vous pouvez également utiliser une des nouvelles API pour aider à gérer par programmation de contenu de plein écran. Ces API est présentés ci-dessous.

#### <a name="toplayoutguide-and-bottomlayoutguide"></a>TopLayoutGuide et BottomLayoutGuide

 `TopLayoutGuide` et `BottomLayoutGuide` servent de référence pour les où doit commencer ou se terminer, afin que le contenu n’est pas recouverte par un translucide `UIKit` barre, comme dans l’exemple suivant :

 [![](ios7-ui-images/clipped.png "Exemple de contenu ne chevauché ne pas par une barre de UIKit translucide")](ios7-ui-images/clipped.png#lightbox)

Ces API peut être utilisé pour calculer le déplacement d’un affichage à partir du haut ou le bas de l’écran et ajuster le positionnement du contenu en conséquence :

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

Nous pouvons utiliser la valeur calculée ci-dessus pour définir notre `ImageView`de déplacement à partir du haut de l’écran, par conséquent, l’image entière est visible :

 [![](ios7-ui-images/good2.png "Déplacement de ImageViews exemple à partir du haut de l’écran")](ios7-ui-images/good2.png#lightbox)

Reportez-vous à la [afficheur d’images](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) pour obtenir un exemple fonctionnel.

La valeur de déplacement est générée dynamiquement une fois que la vue a été ajoutée à la hiérarchie, par conséquent, une tentative de lecture `TopLayoutGuide` et `BottomLayoutGuide` valeurs dans `ViewDidLoad` retourne 0. Calculer la valeur une fois que la vue a chargé - par exemple, dans le `ViewDidLayoutSubviews`.

> [!IMPORTANT]
> `TopLayoutGuide` et `BottomLayoutGuide` sont déconseillés dans iOS 11 en faveur de la nouvelle disposition de zone protégée. Apple indique qu’à l’aide de la zone protégée est compatible avec la version d’iOS antérieure à iOS 11. Pour plus d’informations, consultez le [mise à jour de votre application pour iOS 11](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md#fullscreen) guide.

#### <a name="edgesforextendedlayout"></a>EdgesForExtendedLayout

Cette API spécifie quels bords d’une vue, il convient en plein écran, indépendamment de la barre translucidité. Dans iOS 7, les barres d’outils et les barres de navigation apparaissent disposés en vue du contrôleur - Contrairement à dans iOS précédentes versions, où ils n’occupent le même espace. L’application de Photos iOS 7 illustre la valeur par défaut `UIViewController.EdgesForExtendedLayout` valeur, `UIRectEdge.All`. Ce paramètre remplit toutes les quatre bords dans la vue avec du contenu, de création de l’effet qui se chevauchent et plein écran :

 [![](ios7-ui-images/photos.png "Exemple EdgesForExtendedLayout")](ios7-ui-images/photos.png#lightbox)

En appuyant sur l’image supprime les barres et montre l’image plein écran :

 [![](ios7-ui-images/photos2.png "EdgesForExtendedLayout avec les barres supprimé")](ios7-ui-images/photos2.png#lightbox)

Étant donné que le contenu de plein écran est la valeur par défaut, les applications configurées pour iOS 6 aura la partie de la vue découpée, comme dans la capture d’écran ci-dessous :

 [![](ios7-ui-images/clipped.png "Applications configurées pour iOS 6 aura une partie de la vue découpée, comme dans cette capture d’écran")](ios7-ui-images/clipped.png#lightbox)

Modification de la `UIViewController.EdgesForExtendedLayout` ajuste de propriété pour ce comportement. Nous pouvons spécifier que la vue ne remplissent pas les bords, afin d’éviter l’affichage du contenu dans l’espace occupé par la navigation ou les barres d’outils (à chaque orientation) de notre vue :

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
    this.EdgesForExtendedLayout = UIRectEdge.None;
}
```

Dans notre application, nous allons voir de la vue est repositionnée à nouveau, pour que l’image entière soit visible :

 [![](ios7-ui-images/good.png "Exemple avec l’image entière visible")](ios7-ui-images/good.png#lightbox)

Notez que lorsque les effets de la `TopLayoutGuide/BottomLayoutGuide` et `EdgesForExtendedLayout` API sont similaires, ils sont destinés à remplir des objectifs différents. Modification de la `EdgesForExtendedLayout` paramètre à partir de la valeur par défaut peut corriger les vues découpées dans les applications conçues pour iOS 6, mais une conception de bonne iOS 7 doit respecter l’esthétique de plein écran et fournir l’expérience d’affichage, la partie de confiance sur plein écran `TopLayoutGuide` et `BottomLayoutGuide`pour positionner correctement le contenu qui est destiné à être manipulé dans un emplacement à l’aise pour l’utilisateur.

Reportez-vous à la [afficheur d’images](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) pour obtenir un exemple fonctionnel.

### <a name="status-and-navigation-bars"></a>État et les barres de Navigation

La barre d’état et les barres de navigation sont restitués avec la transparence. Barres d’état sont transparents, tandis que les barres d’outils et les barres de navigation sont translucide et floue pour transmettre le sentiment de profondeur dans l’interface utilisateur. La capture d’écran suivante montre cette flou et la transparence, où la couleur d’arrière-plan bleu de la vue de collection par le biais des barres de l’état et la navigation, leur donnant un aspect bleu clair :

 ![](ios7-ui-images/transparent-navbar.png "Exemple d’état et barres de flou de Navigation")

#### <a name="status-bar-styles"></a>Styles de barre d’état

En même temps que le flou et de transparence, le premier plan d’une barre d’état peut être clair ou sombre (foncé étant la valeur par défaut). Le style de barre d’état peut être défini à partir du contrôleur d’affichage. Un contrôleur d’affichage permettre également définir si la barre d’état est masquée ou affichée.

Par exemple, le code suivant substitue le `PreferredStatusBarStyle` (méthode) d’un contrôleur d’affichage pour que la barre d’état s’affiche un arrière-plan clair :

```csharp
public override UIStatusBarStyle PreferredStatusBarStyle ()
{
    return UIStatusBarStyle.LightContent;
}
```

Cela entraîne la barre d’état apparaissent comme suit :

 ![](ios7-ui-images/light-status-bar.png "Exemple de barre d’état")

Pour masquer la barre d’état à partir du code du contrôleur d’affichage, substituez `PrefersStatusBarHidden`, comme illustré ci-dessous :

```csharp
public override bool PrefersStatusBarHidden ()
{
    return true;
}
```

Cela masque la barre d’état :

 ![](ios7-ui-images/status-bar-hidden.png "Barre d’état masquée")

### <a name="tint-color"></a>Couleur de teinte

Boutons sont maintenant affichés sous forme de texte de moins de chrome. La couleur du texte peut être contrôlée à l’aide de la nouvelle `TintColor` propriété sur `UIView`. Définition de la `TintColor` applique la hiérarchie de la vue entière de la vue qui définit la couleur. Pour appliquer un `TintColor`tout au long d’une application, définissez-le sur le `Window`. Vous pouvez également détecter lorsque la couleur de teinte modifiée la `UIView.TintColorDidChange` (méthode).

Par exemple, la capture d’écran suivante montre l’effet de la modification de la couleur de teinte sur Affichage d’un contrôleur navigation d’en violet :

 ![](ios7-ui-images/tint-color.png "Couleur de teinte violette sur une vue de contrôleurs de navigation")

La couleur de teinte est applicable aux images également lorsque la `RenderingMode` est défini sur `UIImageRenderingMode.AlwaysTemplate`.

> [!IMPORTANT]
> Couleur de teinte ne peut pas être définie à l’aide de `UIAppearance`.


### <a name="dynamic-type"></a>Type dynamique

Dans iOS 7, l’utilisateur peut spécifier de taille du texte dans les paramètres du système. Avec un type dynamique, la police est ajustée dynamiquement aspect indépendamment de la taille. `UIFont.PreferredFontForTextStyle` doit être utilisé pour obtenir une police qui est optimisée pour la taille contrôlée par l’utilisateur.

## <a name="summary"></a>Récapitulatif

Cet article décrit les modifications apportées aux éléments d’interface utilisateur dans iOS 7. Il examine plusieurs des modifications apportées aux vues et aux contrôles dans UIKit, mise en surbrillance de l’élément visuel change ainsi que les modifications aux API connexes. Enfin, il introduit de nouvelles API pour travailler avec le mode plein écran type de contenu, nouvelle prise en charge de couleur teinte et dynamique.

## <a name="related-links"></a>Liens associés

- [Afficheur d’images (exemple)](https://developer.xamarin.com/samples/monotouch/iOS7-ui-updates)
