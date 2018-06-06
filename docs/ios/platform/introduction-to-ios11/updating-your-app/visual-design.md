---
title: Mises à jour de conception visuelles dans iOS 11
description: Ce document décrit la conception visuelle des mises à jour introduites dans iOS 11. Il traite des modifications apportées aux barres de navigation, recherche contrôleurs, les marges, plein écran contenu et les vues de table.
ms.prod: xamarin
ms.assetid: 7C300B94-0FAF-492E-A326-877419A1824B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 5bc29e021e30910c7fcc4c3b451d0a4dfe187d05
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787849"
---
# <a name="visual-design-updates-in-ios-11"></a>Mises à jour de conception visuelles dans iOS 11

Dans iOS 11, Apple a introduit de nouvelles modifications visuelles, y compris les mises à jour de la barre de navigation, barre de recherche et les vues de table. En outre les améliorations ont été apportées pour permettre une plus grande flexibilité sur les marges et le contenu de l’écran complet. Ces modifications sont traitées dans ce guide.

## <a name="uikit"></a>UIKit

Barres de UIKit ont été adaptées dans iOS 11 pour les rendre plus accessible pour les utilisateurs finaux.

Modification de ce type est un nouvel affichage HUD qui apparaît lorsqu’un utilisateur long-appuie sur une barre élément. Pour ce faire, définissez la `largeContentSizeImage` propriété sur `UIBarItem` et ajouter une image plus grande via un [catalogue](~/ios/app-fundamentals/images-icons/displaying-an-image.md):

```csharp
barItem.LargeContentSizeImage = UIImage.FromBundle("AccessibleImage");
```

### <a name="navigation-bar"></a>Barre de navigation
iOS 11 a introduit de nouvelles fonctionnalités permettant de faciliter la lecture des titres de barre de navigation. Applications peuvent afficher ce titre supérieure en assignant le `PrefersLargeTitles` true à la propriété :

```csharp
NavigationController.NavigationBar.PrefersLargeTitles = true;
```

Effectue des titres de plus grandes valeur dans votre application _tous les_ titres de barres de navigation entre votre application agrandir, comme illustré dans la capture d’écran suivante :

![Titre de navigation de grande taille](visual-design-images/image7.png)

Pour contrôler quand un grand titre s’affiche sur une barre de navigation, définissez la `LargeTitleDisplayMode` sur l’élément de navigation pour `Always`, `Never`, ou `Automatic`.

### <a name="search-controller"></a>Contrôleur de recherche

iOS 11 rend plus facile d’ajouter un contrôleur recherche directement à la barre de navigation. Une fois que vous avez créé un contrôleur de recherche, ajoutez-le à votre barre de navigation avec le `SearchController` propriété :

```csharp
NavigationItem.SearchController = searchController;
```

[![Titre de navigation de grande taille avec barre de recherche](visual-design-images/image8-sml.png)](visual-design-images/image8-sml.png#lightbox)

Selon les fonctionnalités de votre application, vous pouvez ou ne voulez ne peut-être pas la barre de recherche pour masquer lorsqu’un utilisateur fait défiler une liste. Vous pouvez modifier ce paramètre à l’aide de la `HidesSearchBarWhenScrolling` propriété.

## <a name="margins"></a>Marges

Apple a créé une nouvelle propriété – `directionalLayoutMargins` – qui peut être utilisé pour définir l’espace entre les vues et sous-vues. Utilisez `directionalLayoutMargins` avec `leading` ou `trailing` marges. Que le système soit une langue de gauche à droite ou de droite à gauche, l’espacement dans votre application est définie correctement par iOS.

Dans iOS 10 et avant, toutes les vues avaient une taille de marge minimale à laquelle ils seraient aligner. iOS 11 a introduit l’option de remplacement à l’aide de ce `ViewRespectsSystemMinimumLayoutMargins`. Par exemple, définissez cette propriété à false permet de vous permettent d’ajuster vos marges edge à zéro :

```csharp
ViewRespectsSystemMinimumLayoutMargins = false;
View.LayoutMargins = UIEdgeInsets.Zero;
```
![Marge d’image montrant avec zéro incrustation dans ios 11](visual-design-images/image9.png)

<a name="fullscreen" />

## <a name="full-screen-content"></a>Contenu de l’écran complet

iOS 7 [introduite](~/ios/platform/introduction-to-ios7/ios7-ui.md#fullscreen) `topLayoutGuide` et `bottomLayoutGuide` comme un moyen de limiter vos vues afin qu’ils ne sont pas masquées par des barres UIKit et se trouvent dans une zone visible de l’écran. Ils ont été déconseillées dans iOS 11 en faveur de la _zone admissible_.

La zone de sans échec est une nouvelle façon de réfléchir à l’espace visible de votre application et comment ajouter des contraintes entre une vue et une vue super. Par exemple, considérez l’illustration suivante :

[![Visual Studio en haut de la zone sécurisée et le guide de mise en page bas](visual-design-images/image10-sml.png)](visual-design-images/image10.png#lightbox)

Précédemment, si vous avez ajouté une vue et souhaitiez qu’il soit visible dans la zone verte ci-dessus, vous devez contraindre à la _bas_ de la `TopLayoutGuide` et _haut_ de la `BottomLayoutGuide`. Dans iOS 11, vous devez à la place contraindre à la _haut_ et _bas_ de la zone de sans échec. Voici un exemple :

```csharp
var safeGuide = View.SafeAreaLayoutGuide;
imageView.TopAnchor.ConstraintEqualTo(safeGuide.TopAnchor).Active = true;
safeGuide.BottomAnchor.ConstraintEqualTo(imageView.BottomAnchor).Active = true;
```

## <a name="table-view"></a>Table de vue

Le UITableView a eu un nombre de modifications importantes, iOS 11.

Par défaut, les en-têtes, les pieds de page et les cellules sont maintenant redimensionnés automatiquement en fonction de leur contenu. Pour désactiver cet ensemble de comportement de redimensionnement automatique les `EstimatedRowHeight`, `EstimatedSectionHeaderHeight`, ou `EstimatedSectionFooterHeight` à zéro.

Toutefois, dans certains cas (par exemple quand Ajout UITableViewController dans le concepteur iOS ou lors de l’utilisation de Storboards existant dans le Générateur de Interface) vous devrez peut-être activer manuellement les cellules de dimensionnement automatique. Pour ce faire, assurez-vous que vous avez défini les propriétés suivantes sur l’affichage tableau de cellules, des en-têtes et pieds de page, respectivement :

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

iOS 11 a étendu la fonctionnalité des actions de ligne. `UISwipeActionsConfiguration` a été introduit pour définir un ensemble d’actions qui doit avoir lieu lorsque l’utilisateur fait glisser sa dans les deux sens sur une ligne dans une table. Ce comportement est semblable à celui de la Mail.app natif. Pour plus d’informations, consultez la [les Actions de ligne](~/ios/user-interface/controls/tables/row-action.md) guide.

Les vues de table ont prise en charge de glisser- déposer dans iOS 11. Pour plus d’informations, consultez la [glisser- déposer](~/ios/platform/introduction-to-ios11/drag-and-drop.md#uitableview) guide.


## <a name="related-links"></a>Liens associés

- [Nouveautés dans iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Page du produit mis à jour App Store (Apple)](https://developer.apple.com/app-store/product-page/)
- [Mise à jour de votre application pour iOS 11 (WWDC) (vidéo)](https://developer.apple.com/videos/play/wwdc2017/204/)
