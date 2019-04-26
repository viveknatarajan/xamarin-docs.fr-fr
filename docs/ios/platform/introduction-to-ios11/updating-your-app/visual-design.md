---
title: Mises à jour de conception visuelles dans iOS 11
description: Ce document décrit la conception visuelle mises à jour introduit dans iOS 11. Il traite des modifications apportées aux barres de navigation, recherche contrôleurs, les marges, contenu de plein écran et les vues de table.
ms.prod: xamarin
ms.assetid: 7C300B94-0FAF-492E-A326-877419A1824B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/13/2016
ms.openlocfilehash: c6351f2c25f8e31181c761aea1b471315a8a05e8
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61400202"
---
# <a name="visual-design-updates-in-ios-11"></a>Mises à jour de conception visuelles dans iOS 11

Dans iOS 11, Apple a introduit de nouvelles modifications visuelles, y compris les mises à jour de la barre de navigation, barre de recherche et les vues de table. En outre des améliorations ont été apportées pour permettre une plus grande flexibilité sur les marges et du contenu de plein écran. Ces modifications sont traitées dans ce guide. 

Pour plus d’informations en particulier sur la conception de l’iPhone X, regardez d’Apple [conception pour l’iPhone X](https://developer.apple.com/videos/play/fall2017/801/) vidéo.

## <a name="uikit"></a>UIKit

Barres de UIKit ont été adaptés dans iOS 11 pour les rendre plus accessibles pour les utilisateurs finaux.

Modification de ce type est un nouvel affichage HUD qui apparaît lorsqu’un utilisateur pressions de longue durée sur une barre élément. Pour ce faire, définissez la `largeContentSizeImage` propriété sur `UIBarItem` et ajouter une image plus grande via un [catalogue](~/ios/app-fundamentals/images-icons/displaying-an-image.md):

```csharp
barItem.LargeContentSizeImage = UIImage.FromBundle("AccessibleImage");
```

### <a name="navigation-bar"></a>Barre de navigation
iOS 11 a introduit de nouvelles fonctionnalités permettant de faciliter la lecture des titres de barre de navigation. En affectant les applications peuvent afficher ce titre supérieure la `PrefersLargeTitles` true à la propriété :

```csharp
NavigationController.NavigationBar.PrefersLargeTitles = true;
```

Titres de plus grande de paramètre dans votre application rend _tous les_ titres de barres de navigation entre votre application paraître plus grand, comme illustré dans la capture d’écran suivante :

![Titre de navigation de grande taille](visual-design-images/image7.png)

Pour contrôler quand un grand titre s’affiche sur une barre de navigation, définissez la `LargeTitleDisplayMode` sur l’élément de navigation pour `Always`, `Never`, ou `Automatic`.

### <a name="search-controller"></a>Contrôleur de recherche

iOS 11 rend plus facile d’ajouter un contrôleur de recherche directement à la barre de navigation. Une fois que vous avez créé un contrôleur de recherche, ajoutez-le à votre barre de navigation avec le `SearchController` propriété :

```csharp
NavigationItem.SearchController = searchController;
```

[![Titre de navigation de grande taille avec barre de recherche](visual-design-images/image8-sml.png)](visual-design-images/image8-sml.png#lightbox)

Selon les fonctionnalités de votre application, vous pouvez ou souhaitez ne peut-être pas que la barre de recherche pour masquer lorsqu’un utilisateur parcourt une liste. Vous pouvez le régler à l’aide de la `HidesSearchBarWhenScrolling` propriété.

## <a name="margins"></a>Marges

Apple a créé une nouvelle propriété – `directionalLayoutMargins` – qui peut être utilisé pour définir l’espace entre les vues et sous-vues. Utilisez `directionalLayoutMargins` avec `leading` ou `trailing` encarts. Que le système soit une langue de gauche à droite ou de droite à gauche, l’espacement dans votre application est définie correctement par iOS.

Dans iOS 10 et avant, toutes les vues avaient une taille de marge minimale à laquelle ils seraient aligner. iOS 11 a introduit la possibilité de remplacer cela à l’aide `ViewRespectsSystemMinimumLayoutMargins`. Par exemple, définissez cette propriété à false permet de vous permettent d’ajuster votre encarts edge à zéro :

```csharp
ViewRespectsSystemMinimumLayoutMargins = false;
View.LayoutMargins = UIEdgeInsets.Zero;
```
![Marge d’image montrant avec zéro incrustation dans ios 11](visual-design-images/image9.png)

<a name="fullscreen" />

## <a name="full-screen-content"></a>Contenu de plein écran

iOS 7 [introduite](~/ios/platform/introduction-to-ios7/ios7-ui.md#fullscreen) `topLayoutGuide` et `bottomLayoutGuide` comme un moyen de limiter vos vues afin qu’ils ne sont pas masquées par des barres de UIKit et se trouvent dans une zone visible de l’écran. Ils ont été déconseillées dans iOS 11 en faveur de la _zone protégée_.

La zone protégée est une nouvelle façon de penser que l’espace de votre application et comment les contraintes sont ajoutées entre une vue et une super vue visible. Par exemple, considérez l’image suivante :

[![Visual Studio en haut de la zone protégée et repère de disposition inférieure](visual-design-images/image10-sml.png)](visual-design-images/image10.png#lightbox)

Auparavant, si vous avez ajouté une vue et souhaitions qu’elle soit visible dans la zone verte ci-dessus, vous devez pouvoir limiter le _bas_ de la `TopLayoutGuide` et le _haut_ de la `BottomLayoutGuide`. Dans iOS 11, vous devez à la place le contraindre à la _haut_ et _bas_ de la zone protégée. Voici un exemple :

```csharp
var safeGuide = View.SafeAreaLayoutGuide;
imageView.TopAnchor.ConstraintEqualTo(safeGuide.TopAnchor).Active = true;
safeGuide.BottomAnchor.ConstraintEqualTo(imageView.BottomAnchor).Active = true;
```

## <a name="table-view"></a>Vue de table

Le UITableView a un nombre de modifications importantes, dans iOS 11.

Par défaut, en-têtes, pieds de page et les cellules sont désormais automatiquement dimensionnées en fonction de leur contenu. Pour désactiver ce comportement de redimensionnement automatique de jeu la `EstimatedRowHeight`, `EstimatedSectionHeaderHeight`, ou `EstimatedSectionFooterHeight` à zéro.

Toutefois, dans certains cas (par exemple quand Ajout UITableViewController dans le concepteur iOS, ou lorsque vous utilisez Storboards existant dans Interface Builder) vous devrez peut-être activer manuellement les cellules de dimensionnement automatique. Pour ce faire, vérifiez que vous avez défini les propriétés suivantes sur la vue de table pour les cellules, les en-têtes et pieds de page, respectivement :

```csharp
// Cells
TableView.RowHeight = UITableView.AutomaticDimension;
TableView.EstimatedRowHeight = UITableView.AutomaticDimension;

// Header
TableView.SectionHeaderHeight = UITableView.AutomaticDimension;
TableView.EstimatedSectionHeaderHeight = 40f;

//Footer
TableView.SectionFooterHeight = UITableView.AutomaticDimension;
TableView.EstimatedSectionFooterHeight = 40f;

```

iOS 11 a étendu la fonctionnalité des actions de ligne. `UISwipeActionsConfiguration` a été introduit pour définir un ensemble d’actions qui doit avoir lieu lorsque l’utilisateur fait glisser sa dans les deux directions sur une ligne dans un tableau. Ce comportement est semblable à celle de la Mail.app natif. Pour plus d’informations, consultez le [Actions de ligne](~/ios/user-interface/controls/tables/row-action.md) guide.

Les vues de table ont prise en charge de glisser -déplacer dans iOS 11. Pour plus d’informations, consultez le [glisser -déplacer](~/ios/platform/introduction-to-ios11/drag-and-drop.md#uitableview) guide.


## <a name="related-links"></a>Liens associés

- [Quelles sont les nouveautés dans iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Page du produit mis à jour App Store (Apple)](https://developer.apple.com/app-store/product-page/)
- [Conception de l’iPhone X (Apple) (vidéo)](https://developer.apple.com/videos/play/fall2017/801/)
- [La mise à jour de votre application pour iOS 11 (WWDC) (vidéo)](https://developer.apple.com/videos/play/wwdc2017/204/)

