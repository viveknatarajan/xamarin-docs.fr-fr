---
title: Vues de collection
description: Vues de collection autoriser le contenu à afficher à l’aide de dispositions arbitraires. Elles permettent créer aisément des mises en page de grille l’emploi, également la prise en charge des dispositions personnalisées.
ms.prod: xamarin
ms.assetid: F4B85F25-0CB5-4FEA-A3B5-D22FCDC81AE4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 75ad331a265c14892f101b1aa7956d2cde3beec8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="collection-views"></a>Vues de collection

_Vues de collection autoriser le contenu à afficher à l’aide de dispositions arbitraires. Elles permettent créer aisément des mises en page de grille l’emploi, également la prise en charge des dispositions personnalisées._

Vues de collection, disponibles dans le `UICollectionView` de classe, sont un nouveau concept dans iOS 6 qui introduisent présentant plusieurs éléments à l’écran à l’aide de dispositions. Les modèles pour fournir des données à un `UICollectionView` pour créer des éléments et d’interagir avec ces suivent des éléments du même délégation et source de données modèles couramment utilisés dans le développement d’iOS.

Toutefois, les vues de Collection fonctionnent avec un sous-système de disposition qui est indépendant de la `UICollectionView` lui-même. Par conséquent, simplement en fournissant une disposition différente peut facilement modifier la présentation d’une vue de Collection.

e/s fournit une classe de mise en page appelée `UICollectionViewFlowLayout` qui autorise des dispositions en ligne tel qu’une grille doit être créé sans aucun travail supplémentaire. En outre, des dispositions personnalisées peuvent également être créées autoriser n’importe quelle présentation que vous pouvez l’imaginer.

## <a name="uicollectionview-basics"></a>Notions de base UICollectionView

La `UICollectionView` classe est composée de trois éléments différents :

-  **Cellules** – piloté par les données des vues pour chaque élément.
-  **Affichages supplémentaires** – vues piloté par les données associées à une section.
-  **Vues de décoration** – Non pilotées par les vues créées par une mise en page

## <a name="cells"></a>Cellules

Les cellules sont des objets qui représentent un élément unique dans le jeu de données est présenté par la vue de collection. Chaque cellule est une instance de la `UICollectionViewCell` (classe), qui se compose de trois modes différents, comme indiqué dans la figure ci-dessous :

 [![](uicollectionview-images/01-uicollectionviewcell.png "Chaque cellule est composé de trois modes différents, comme illustré ici")](uicollectionview-images/01-uicollectionviewcell.png#lightbox)

La `UICollectionViewCell` classe a les propriétés suivantes pour chacun de ces vues :

-   `ContentView` – Cette vue contient le contenu de la cellule est présente. Il est restitué dans l’ordre z supérieure sur l’écran.
-   `SelectedBackgroundView` – Prise en charge pour la sélection des cellules ont intégrées. Cette vue est utilisée pour indiquer visuellement la qu’une cellule est sélectionnée. Il est rendu juste en dessous de la `ContentView` lorsqu’une cellule est sélectionnée.
-   `BackgroundView` – Cellules peuvent également afficher un arrière-plan, ce qui est présenté par le `BackgroundView` . Cette vue est restituée sous la `SelectedBackgroundView` .


En définissant le `ContentView` afin qu’il soit plus petit que le `BackgroundView` et `SelectedBackgroundView`, le `BackgroundView` peut être utilisé pour le frame de visuellement le contenu pendant la `SelectedBackgroundView` s’affichera lorsqu’une cellule est sélectionnée, comme indiqué ci-dessous :

 [![](uicollectionview-images/02-cells.png "Les éléments de l’autre cellule")](uicollectionview-images/02-cells.png#lightbox)

Les cellules de la capture d’écran ci-dessus sont créées en héritant de `UICollectionViewCell` et en définissant le `ContentView`, `SelectedBackgroundView` et `BackgroundView` propriétés, respectivement, comme indiqué dans le code suivant :

```csharp
public class AnimalCell : UICollectionViewCell
{
        UIImageView imageView;

        [Export ("initWithFrame:")]
        public AnimalCell (CGRect frame) : base (frame)
        {
            BackgroundView = new UIView{BackgroundColor = UIColor.Orange};

            SelectedBackgroundView = new UIView{BackgroundColor = UIColor.Green};

            ContentView.Layer.BorderColor = UIColor.LightGray.CGColor;
            ContentView.Layer.BorderWidth = 2.0f;
            ContentView.BackgroundColor = UIColor.White;
            ContentView.Transform = CGAffineTransform.MakeScale (0.8f, 0.8f);

            imageView = new UIImageView (UIImage.FromBundle ("placeholder.png"));
            imageView.Center = ContentView.Center;
            imageView.Transform = CGAffineTransform.MakeScale (0.7f, 0.7f);

            ContentView.AddSubview (imageView);
        }

        public UIImage Image {
            set {
                imageView.Image = value;
            }
        }
}
```

 <a name="Supplementary_Views" />


## <a name="supplementary-views"></a>Affichages supplémentaires

Les affichages supplémentaires sont qui présentent les informations associées à chaque section d’un `UICollectionView`. Comme les cellules, des vues supplémentaires sont pilotés par les données. Lorsque les cellules de présentent les données de l’élément à partir d’une source de données, des vues supplémentaires présenter les données de la section, telles que les catégories de livres dans une bibliothèque ou le style de musique dans une bibliothèque de musique.

Par exemple, une vue supplémentaires utilisable pour présenter un en-tête d’une section spécifique, comme indiqué dans la figure ci-dessous :

 [![](uicollectionview-images/02a-supplementary-view.png "Une vue supplémentaire utilisée pour présenter un en-tête d’une section spécifique, comme illustré ici")](uicollectionview-images/02a-supplementary-view.png#lightbox)

Pour utiliser une vue supplémentaires, il doit tout d’abord être enregistré dans le `ViewDidLoad` méthode :

```csharp
CollectionView.RegisterClassForSupplementaryView (typeof(Header), UICollectionElementKindSection.Header, headerId);
```

Ensuite, la vue doit être retourné à l’aide de `GetViewForSupplementaryElement`, créé à l’aide de `DequeueReusableSupplementaryView`et hérite de `UICollectionReusableView`. L’extrait de code suivant génère le SupplementaryView indiqué dans la capture d’écran ci-dessus :

```csharp
public override UICollectionReusableView GetViewForSupplementaryElement (UICollectionView collectionView, NSString elementKind, NSIndexPath indexPath)
        {
            var headerView = (Header)collectionView.DequeueReusableSupplementaryView (elementKind, headerId, indexPath);
            headerView.Text = "Supplementary View";
            return headerView;
        }

```

Affichages supplémentaires sont plus génériques que simplement des en-têtes et pieds de page.
Ils peuvent être positionnées n’importe où dans la vue de collection et peuvent être constituées de toutes les vues, ce qui leur apparence est entièrement personnalisable.

 <a name="Decoration_Views" />


## <a name="decoration-views"></a>Vues de décoration

Décoration les affichages sont purement visuel qui peut être affiché dans un `UICollectionView`. Contrairement aux cellules et des vues supplémentaires, ils ne sont pas orientées données. Ils sont toujours créés au sein de la sous-classe d’une disposition et par la suite peuvent modifier en tant que mise en page du contenu. Par exemple, une vue de décoration pourrait être utilisée pour présenter une vue d’arrière-plan défile avec le contenu dans le `UICollectionView`, comme indiqué ci-dessous :

 [![](uicollectionview-images/02c-decoration-view.png "Affichage de décoration avec un arrière-plan rouge")](uicollectionview-images/02c-decoration-view.png#lightbox)

 L’extrait de code ci-dessous modifie l’arrière-plan rouge dans les exemples `CircleLayout` classe :

 ```csharp
 public class MyDecorationView : UICollectionReusableView
    {
        [Export ("initWithFrame:")]
        public MyDecorationView (CGRect frame) : base (frame)
        {
            BackgroundColor = UIColor.Red;
        }
    }
 ```


## <a name="data-source"></a>source de données

Comme avec d’autres parties d’e/s, telles que `UITableView` et `MKMapView`, `UICollectionView` obtient ses données à partir d’un *source de données*, qui est exposée dans Xamarin.iOS via la **`UICollectionViewDataSource`** classe. Cette classe est chargée de fournir le contenu à la `UICollectionView` telles que :

-  **Cellules** – retourné à partir de `GetCell` (méthode).
-  **Affichages supplémentaires** – retourné à partir de `GetViewForSupplementaryElement` (méthode).
-  **Nombre de sections** – retourné à partir de `NumberOfSections` (méthode). La valeur par défaut est 1 si non implémenté.
-  **Nombre d’éléments par section** – retourné à partir de `GetItemsCount` (méthode).

### <a name="uicollectionviewcontroller"></a>UICollectionViewController
Pour plus de commodité, le `UICollectionViewController` classe n’est disponible. Il est automatiquement configuré pour être à la fois le délégué, qui est décrite dans la section suivante, et la source de données pour son `UICollectionView` vue.

Comme avec `UITableView`, la `UICollectionView` classe appelle uniquement sa source de données pour obtenir des cellules pour les éléments qui se trouvent sur l’écran.
Les cellules que vous faites défiler l’écran sont placés dans une file d’attente pour une réutilisation, comme l’illustre l’image suivante :

 [![](uicollectionview-images/03-cell-reuse.png "Les cellules que vous faites défiler l’écran sont placés dans une file d’attente pour une réutilisation comme indiqué ici")](uicollectionview-images/03-cell-reuse.png#lightbox)

Réutilisation de la cellule a été simplifiée avec `UICollectionView` et `UITableView`. Vous n’avez plus besoin créer une cellule directement dans la source de données si une n’est pas disponible dans la file d’attente de réutilisation, comme les cellules sont inscrits auprès du système. Si une cellule n’est pas disponible lors de l’établissement de l’appel à la file d’attente la cellule à partir de la file d’attente de réutilisation, iOS crée automatiquement en fonction du type ou d’une plume qui a été enregistré.
La même technique est également disponible pour les vues supplémentaires.

Par exemple, prenons le code suivant qui enregistre la `AnimalCell` classe :

```csharp
static NSString animalCellId = new NSString ("AnimalCell");
CollectionView.RegisterClassForCell (typeof(AnimalCell), animalCellId);
```

Lorsqu’un `UICollectionView` a besoin d’une cellule, car son élément se trouve sur l’écran, la `UICollectionView` appelle sa source de données `GetCell` (méthode). Comme pour le fonctionnement avec UITableView, cette méthode est responsable de la configuration d’une cellule à partir des données de sauvegarde, qu’il seraient un `AnimalCell` classe dans ce cas.

Le code suivant illustre une implémentation de `GetCell` qui retourne un `AnimalCell` instance :

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, Foundation.NSIndexPath indexPath)
{
        var animalCell = (AnimalCell)collectionView.DequeueReusableCell (animalCellId, indexPath);

        var animal = animals [indexPath.Row];

        animalCell.Image = animal.Image;

        return animalCell;
}
```

L’appel à `DequeReusableCell` est lorsque la cellule est soit annuler mises en attente à partir de la file d’attente de la réutilisation ou, si une cellule n’est pas disponible dans la file d’attente, créé en fonction du type inscrit dans l’appel à `CollectionView.RegisterClassForCell`.

Dans ce cas, en inscrivant le `AnimalCell` iOS va créer une nouvelle classe, `AnimalCell` en interne et le retourner lors d’un appel à la file d’attente une cellule, après laquelle il est configuré avec l’image contenue dans la classe animale et renvoyées pour affichage à le `UICollectionView`.

 <a name="Delegate" />


### <a name="delegate"></a>délégué

Le `UICollectionView` classe utilise un délégué du type `UICollectionViewDelegate` pour prendre en charge l’interaction avec le contenu dans le `UICollectionView`. Cela permet de contrôler de :

-  **Sélection de la cellule** – détermine si une cellule est sélectionnée.
-  **Mise en surbrillance de la cellule** – détermine si une cellule est actuellement en cours couvertes.
-  **Menus de la cellule** – Menu qui s’affiché pour une cellule en réponse à un mouvement de presse long.


Comme avec la source de données, le `UICollectionViewController` est configurée par défaut pour être le délégué pour le `UICollectionView`.

 <a name="Cell_HighLighting" />


#### <a name="cell-highlighting"></a>Mise en surbrillance de cellule

Lorsqu’une cellule est activée, les transitions de cellule dans un état en surbrillance, et il n’est pas sélectionnée jusqu'à ce que l’utilisateur soulève leur doigt à partir de la cellule. Cela permet une modification temporaire de l’apparence de la cellule avant qu’il est réellement sélectionné. Lors de la sélection, la cellule `SelectedBackgroundView` s’affiche. La figure ci-dessous montre l’état mis en surbrillance juste avant la sélection se produit :

 [![](uicollectionview-images/04-cell-highlight.png "Cette figure illustre l’état mis en surbrillance juste avant la sélection se produit.")](uicollectionview-images/04-cell-highlight.png#lightbox)

Pour implémenter la mise en surbrillance, le `ItemHighlighted` et `ItemUnhighlighted` méthodes de la `UICollectionViewDelegate` peut être utilisé. Par exemple, le code suivant s’applique à un arrière-plan jaune de la `ContentView` lorsque la cellule est mise en surbrillance et un arrière-plan blanc lors de la non mise en surbrillance, comme illustré dans l’image ci-dessus :

```csharp
public override void ItemHighlighted (UICollectionView collectionView, NSIndexPath indexPath)
{
        var cell = collectionView.CellForItem(indexPath);
        cell.ContentView.BackgroundColor = UIColor.Yellow;
}

public override void ItemUnhighlighted (UICollectionView collectionView, NSIndexPath indexPath)
{
        var cell = collectionView.CellForItem(indexPath);
        cell.ContentView.BackgroundColor = UIColor.White;
}
```

 <a name="Disabling_Selection" />


#### <a name="disabling-selection"></a>La désactivation de la sélection

La sélection est activée par défaut dans `UICollectionView`. Pour désactiver la sélection, substituez `ShouldHighlightItem` et retourne la valeur false, comme indiqué ci-dessous :

```csharp
public override bool ShouldHighlightItem (UICollectionView collectionView, NSIndexPath indexPath)
{
        return false;
}
```

Lors de la mise en surbrillance est désactivée, le processus de sélection d’une cellule est également désactivé. En outre, il existe également un `ShouldSelectItem` méthode qui contrôle la sélection directement, bien que si `ShouldHighlightItem` est implémenté et retourne la valeur false, `ShouldSelectItem` n’est pas appelée.

 `ShouldSelectItem` permet la sélection être activée ou désactivée sur la base d’un élément par élément, lorsque `ShouldHighlightItem` n’est pas implémentée. Il permet également de mettre en surbrillance sans sélection, si `ShouldHighlightItem` est implémenté et retourne la valeur true, tout en `ShouldSelectItem` retourne la valeur false.

 <a name="Cell_Menus" />


#### <a name="cell-menus"></a>Menus de cellule

Chaque cellule dans un `UICollectionView` ne peut afficher un menu qui permet de couper, copier et coller éventuellement être pris en charge. Pour créer un menu Edition sur une cellule :

1.  Substituer `ShouldShowMenu` et retourner la valeur true si l’élément doit afficher un menu.
1.  Substituer `CanPerformAction` et retourner la valeur true pour chaque action que l’élément peut effectuer, qui sera une des opérations couper, copier ou coller.
1.  Substituer `PerformAction` pour effectuer la modification, copie de l’opération de collage.


La capture d’écran suivante affiche le menu lorsqu’une cellule est enfoncée longue :

 [![](uicollectionview-images/04a-menu.png "Cette capture d’écran afficher le menu lorsqu’une cellule est long enfoncée")](uicollectionview-images/04a-menu.png#lightbox)

 <a name="Layout" />


## <a name="layout"></a>Mise en page

`UICollectionView` prend en charge un système de disposition qui permet le positionnement de tous ses éléments, les cellules, supplémentaires de vues et décoration devant être gérés indépendamment de la `UICollectionView` lui-même.
Vous utilisez le système de disposition, une application peut prendre en charge des dispositions tel que celui de grille, nous l’avez vu dans cet article, ainsi que fournir des dispositions personnalisées.

 <a name="Layout_Basics" />


### <a name="layout-basics"></a>Principes de base de mise en page

Mises en page dans un `UICollectionView` sont définis dans une classe qui hérite de `UICollectionViewLayout`. L’implémentation de la mise en page est chargée de créer les attributs de disposition pour chaque élément dans le `UICollectionView`. Il existe deux façons de créer une disposition :

-  Utiliser la fonction intégrée `UICollectionViewFlowLayout` .
-  Fournir une disposition personnalisée en héritant de `UICollectionViewLayout` .


 <a name="Flow_Layout" />


### <a name="flow-layout"></a>Mise en page fluide

La `UICollectionViewFlowLayout` classe fournit une disposition basée sur une ligne qui appropriée pour organiser le contenu dans une grille de cellules comme nous l’avons vu.

Pour utiliser une disposition de flux :

-  Créez une instance de `UICollectionViewFlowLayout` :


```csharp
var layout = new UICollectionViewFlowLayout ();
```

-  Passez l’instance au constructeur de la `UICollectionView` :


```csharp
simpleCollectionViewController = new SimpleCollectionViewController (layout);
```

C’est tout ce qui est nécessaire à la disposition de contenu dans une grille. En outre, lorsque l’orientation est modifié, le `UICollectionViewFlowLayout` gère la réorganisation du contenu correctement, comme indiqué ci-dessous :

 [![](uicollectionview-images/05-layout-orientation.png "Exemple de modifications de l’orientation")](uicollectionview-images/05-layout-orientation.png#lightbox)

 <a name="Section_Inset" />


#### <a name="section-inset"></a>Incrustation de section

Pour fournir l’espace autour de la `UIContentView`, dispositions ont un `SectionInset` propriété de type `UIEdgeInsets`. Par exemple, le code suivant fournit une mémoire tampon de 50 pixels autour de chaque section de la `UIContentView` lors de la mise en forme un `UICollectionViewFlowLayout`:

```csharp
var layout = new UICollectionViewFlowLayout ();
layout.SectionInset = new UIEdgeInsets (50,50,50,50);
```

Cela entraîne l’espacement de la section comme indiqué ci-dessous :

 [![](uicollectionview-images/06-sectioninset.png "Espacement autour de la section, comme illustré ici")](uicollectionview-images/06-sectioninset.png#lightbox)

 <a name="Subclassing_UICollectionViewFlowLayout" />


#### <a name="subclassing-uicollectionviewflowlayout"></a>Sous-classement UICollectionViewFlowLayout

Dans l’édition à l’aide `UICollectionViewFlowLayout` directement, il peut également être sous-classé pour personnaliser davantage la disposition du contenu le long d’une ligne. Par exemple, cela peut servir à créer une structure qui n’encapsule pas les cellules dans une grille, mais crée à la place d’une seule ligne avec un effet de défilement horizontal, comme indiqué ci-dessous :

 [![](uicollectionview-images/07-line-layout.png "Une seule ligne avec un effet de défilement horizontale")](uicollectionview-images/07-line-layout.png#lightbox)

Cette implémentation en sous-classant `UICollectionViewFlowLayout` requiert :

-  L’initialisation de toutes les propriétés de disposition qui s’appliquent à la disposition de lui-même ou tous les éléments de la mise en page dans le constructeur.
-  Substitution `ShouldInvalidateLayoutForBoundsChange` , qui retourne true ainsi que quand les limites de la `UICollectionView` change, la disposition des cellules sont recalculées. Il est utilisé dans ce cas Vérifiez que le code de transformation à appliquer à la cellule centermost sera appliquée pendant le défilement.
-  Substitution de `TargetContentOffset` pour rendre le centermost Aligner au centre de la cellule la `UICollectionView` en tant que le défilement s’arrête.
-  Substitution de `LayoutAttributesForElementsInRect` pour retourner un tableau de `UICollectionViewLayoutAttributes` . Chaque `UICollectionViewLayoutAttribute` contient des informations sur la disposition de l’élément particulier, y compris les propriétés telles que son `Center` , `Size` , `ZIndex` et `Transform3D` .


Le code suivant montre une telle implémentation :

```csharp
using System;
using CoreGraphics;
using Foundation;
using UIKit;
using CoreGraphics;
using CoreAnimation;

namespace SimpleCollectionView
{
    public class LineLayout : UICollectionViewFlowLayout
    {
        public const float ITEM_SIZE = 200.0f;
        public const int ACTIVE_DISTANCE = 200;
        public const float ZOOM_FACTOR = 0.3f;

        public LineLayout ()
        {
            ItemSize = new CGSize (ITEM_SIZE, ITEM_SIZE);
            ScrollDirection = UICollectionViewScrollDirection.Horizontal;
            SectionInset = new UIEdgeInsets (400,0,400,0);
            MinimumLineSpacing = 50.0f;
        }

        public override bool ShouldInvalidateLayoutForBoundsChange (CGRect newBounds)
        {
            return true;
        }

        public override UICollectionViewLayoutAttributes[] LayoutAttributesForElementsInRect (CGRect rect)
        {
            var array = base.LayoutAttributesForElementsInRect (rect);
            var visibleRect = new CGRect (CollectionView.ContentOffset, CollectionView.Bounds.Size);

            foreach (var attributes in array) {
                if (attributes.Frame.IntersectsWith (rect)) {
                    float distance = (float)(visibleRect.GetMidX () - attributes.Center.X);
                    float normalizedDistance = distance / ACTIVE_DISTANCE;
                    if (Math.Abs (distance) < ACTIVE_DISTANCE) {
                        float zoom = 1 + ZOOM_FACTOR * (1 - Math.Abs (normalizedDistance));
                        attributes.Transform3D = CATransform3D.MakeScale (zoom, zoom, 1.0f);
                        attributes.ZIndex = 1;
                    }
                }
            }
            return array;
        }

        public override CGPoint TargetContentOffset (CGPoint proposedContentOffset, CGPoint scrollingVelocity)
        {
            float offSetAdjustment = float.MaxValue;
            float horizontalCenter = (float)(proposedContentOffset.X + (this.CollectionView.Bounds.Size.Width / 2.0));
            CGRect targetRect = new CGRect (proposedContentOffset.X, 0.0f, this.CollectionView.Bounds.Size.Width, this.CollectionView.Bounds.Size.Height);
            var array = base.LayoutAttributesForElementsInRect (targetRect);
            foreach (var layoutAttributes in array) {
                float itemHorizontalCenter = (float)layoutAttributes.Center.X;
                if (Math.Abs (itemHorizontalCenter - horizontalCenter) < Math.Abs (offSetAdjustment)) {
                    offSetAdjustment = itemHorizontalCenter - horizontalCenter;
                }
            }
            return new CGPoint (proposedContentOffset.X + offSetAdjustment, proposedContentOffset.Y);
        }

    }
}
```

 <a name="Custom_Layout" />


### <a name="custom-layout"></a>Disposition personnalisée

Outre l’utilisation de `UICollectionViewFlowLayout`, dispositions peuvent également être entièrement personnalisées en héritant directement à partir de `UICollectionViewLayout`.

Les principales méthodes à substituer sont :

-   `PrepareLayout` – Utilisé pour effectuer des calculs géométriques initiales qui seront utilisés durant le processus de mise en page.
-   `CollectionViewContentSize` : Retourne la taille de la zone utilisée pour afficher le contenu.
-   `LayoutAttributesForElementsInRect` – À mesure que l’exemple UICollectionViewFlowLayout indiqué précédemment, cette méthode est utilisée pour fournir des informations pour le `UICollectionView` quant à la disposition de chaque élément. Cependant, contrairement à la `UICollectionViewFlowLayout` , lorsque vous créez une disposition personnalisée, vous pouvez placer des éléments de votre choix.


Par exemple, le même contenu peut être présenté dans une disposition circulaire comme indiqué ci-dessous :

 [![](uicollectionview-images/08-circle-layout.png "Circulaire personnalisée mise en page, comme indiqué ici")](uicollectionview-images/08-circle-layout.png#lightbox)

La chose puissante sur les dispositions est que pour modifier à partir de la mise en page de grille, à une disposition de défilement horizontale et par la suite à cette mise en page circulaire requiert uniquement la classe de mise en page fournie pour le `UICollectionView` être modifié. Rien dans le `UICollectionView`, son délégué ou source de données les modifications du code du tout.


## <a name="changes-in-ios-9"></a>Modifications dans iOS 9


Dans iOS 9, la vue de collection (`UICollectionView`) désormais prend en charge faites glisser la réorganisation des éléments en dehors de la zone en ajoutant un nouveau module de reconnaissance de mouvement par défaut et plusieurs nouvelles méthodes de prise en charge.

À l’aide de ces nouvelles méthodes, vous pouvez facilement implémenter glisser pour les réorganiser dans votre vue de collection et avez la possibilité de personnalisation de l’apparence des éléments au cours de n’importe quelle étape du processus de réorganisation des éléments.

[![](uicollectionview-images/intro01.png "Un exemple du processus de réorganisation des éléments")](uicollectionview-images/intro01.png#lightbox)

Dans cet article, nous allons mettre en œuvre glisser pour réorganiser dans une application Xamarin.iOS ainsi que certaines autres modifications iOS 9 a le contrôle de vue de collection :

- [Facile de réorganisation des éléments](#Easy-Reordering-of-Items)
    - [Exemple de méthode simple](#Simple-Reordering-Example)
    - [À l’aide d’un module de reconnaissance de mouvement personnalisée](#Using-a-Custom-Gesture-Recognizer)
    - [Dispositions personnalisées et réorganisation](#Custom-Layouts-and-Reording)
- [Afficher les modifications de collection](#Collection-View-Changes)

<a name="Easy-Reordering-of-Items" />

## <a name="reordering-of-items"></a>Réorganisation des éléments

Comme indiqué ci-dessus, une des modifications plus importantes à la vue de collection dans iOS 9 a été l’ajout de fonctionnalités de glisser à réorganiser facilement prêtes à l’emploi.

Dans iOS 9, le moyen le plus rapide pour ajouter la réorganisation à une vue de collection est d’utiliser un `UICollectionViewController`.
Le contrôleur de vue de collection a maintenant un `InstallsStandardGestureForInteractiveMovement` propriété, qui ajoute une norme *reconnaissance de mouvement* qui prend en charge le glisser-déplacer pour réorganiser les éléments dans la collection.
Étant donné que la valeur par défaut est `true`, il vous suffit de mettre en œuvre la `MoveItem` méthode de la `UICollectionViewDataSource` classe pour prendre en charge du glisser à réorganiser. Par exemple :

```csharp
public override void MoveItem (UICollectionView collectionView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
    // Reorder our list of items
    ...
}
```
<a name="Simple-Reordering-Example" />

### <a name="simple-reordering-example"></a>Exemple de méthode simple

Un exemple rapide, démarrez un nouveau projet de Xamarin.iOS et de modifier le **Main.storyboard** fichier. Faites glisser un `UICollectionViewController` sur l’aire de conception :

[![](uicollectionview-images/quick01.png "Ajout d’un UICollectionViewController")](uicollectionview-images/quick01.png#lightbox)

Sélectionnez la vue de Collection (il peut être plus facile de le faire à partir de la structure du document). Dans l’onglet Disposition de la zone Propriétés, définissez les tailles suivantes, comme illustré dans la capture d’écran ci-dessous :

- **Taille de la cellule**: largeur – 60 | Hauteur : 60
- **Taille de l’en-tête**: largeur – 0 | Hauteur : 0
- **Taille de pied de page**: largeur – 0 | Hauteur : 0
- **Espacement de min**: pour les cellules – 8 | Lignes : 8
- **Section des marges**: haut – 16 | Bas – 16 | Gauche – 16 | Droite : 16

[![](uicollectionview-images/quick04.png "Définissez les tailles de la vue de Collection")](uicollectionview-images/quick04.png#lightbox)

Ensuite, modifiez la valeur par défaut de cellule :
    - Modifier sa couleur d’arrière-plan bleu
    - Ajouter une étiquette en tant que le titre de la cellule
    - Définir l’identificateur de réutilisation à **cellule**

[![](uicollectionview-images/quick02.png "Modifier la cellule de la valeur par défaut")](uicollectionview-images/quick02.png#lightbox)

Ajoutez des contraintes pour que les étiquettes centrée à l’intérieur de la cellule, comme la modification de la taille :

Dans le **remplissage de la propriété** pour le _CollectionViewCell_ et définir le **classe** à `TextCollectionViewCell`:

[![](uicollectionview-images/quick05.png "La valeur de la classe TextCollectionViewCell")](uicollectionview-images/quick05.png#lightbox)

Définir le **vue de Collection réutilisable** à `Cell`:

[![](uicollectionview-images/quick06.png "La valeur de la vue de Collection réutilisable cellule")](uicollectionview-images/quick06.png#lightbox)

Enfin, sélectionnez l’étiquette et nommez-le `TextLabel`:

[![](uicollectionview-images/quick07.png "étiquette du nom TextLabel")](uicollectionview-images/quick07.png#lightbox)

Modifier la `TextCollectionViewCell` classe et ajoutez les propriétés suivantes. :

```csharp
using System;
using Foundation;
using UIKit;

namespace CollectionView
{
    public partial class TextCollectionViewCell : UICollectionViewCell
    {
        #region Computed Properties
        public string Title {
            get { return TextLabel.Text; }
            set { TextLabel.Text = value; }
        }
        #endregion

        #region Constructors
        public TextCollectionViewCell (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

Ici le `Text` de l’étiquette est exposée en tant que le titre de la cellule, afin qu’il peut être définie à partir du code.

Ajoutez une nouvelle classe c# au projet et appelez-le `WaterfallCollectionSource`. Modifiez le fichier et qu’elle ressemble à ce qui suit :

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;

namespace CollectionView
{
    public class WaterfallCollectionSource : UICollectionViewDataSource
    {
        #region Computed Properties
        public WaterfallCollectionView CollectionView { get; set;}
        public List<int> Numbers { get; set; } = new List<int> ();
        #endregion

        #region Constructors
        public WaterfallCollectionSource (WaterfallCollectionView collectionView)
        {
            // Initialize
            CollectionView = collectionView;

            // Init numbers collection
            for (int n = 0; n < 100; ++n) {
                Numbers.Add (n);
            }
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView) {
            // We only have one section
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section) {
            // Return the number of items
            return Numbers.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get a reusable cell and set {~~it's~>its~~} title from the item
            var cell = collectionView.DequeueReusableCell ("Cell", indexPath) as TextCollectionViewCell;
            cell.Title = Numbers [(int)indexPath.Item].ToString();

            return cell;
        }

        public override bool CanMoveItem (UICollectionView collectionView, NSIndexPath indexPath) {
            // We can always move items
            return true;
        }

        public override void MoveItem (UICollectionView collectionView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
        {
            // Reorder our list of items
            var item = Numbers [(int)sourceIndexPath.Item];
            Numbers.RemoveAt ((int)sourceIndexPath.Item);
            Numbers.Insert ((int)destinationIndexPath.Item, item);
        }
        #endregion
    }
}
```

Cette classe est la source de données de la vue de collection et fournir les informations pour chaque cellule dans la collection.
Notez que le `MoveItem` méthode est implémentée pour autoriser la faire glisser des éléments dans la collection sont réorganisés.

Ajoutez une autre nouvelle classe c# au projet et appelez-le `WaterfallCollectionDelegate`. Modifier ce fichier et le rendre l’aspect suivant :

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;

namespace CollectionView
{
    public class WaterfallCollectionDelegate : UICollectionViewDelegate
    {
        #region Computed Properties
        public WaterfallCollectionView CollectionView { get; set;}
        #endregion

        #region Constructors
        public WaterfallCollectionDelegate (WaterfallCollectionView collectionView)
        {

            // Initialize
            CollectionView = collectionView;

        }
        #endregion

        #region Overrides Methods
        public override bool ShouldHighlightItem (UICollectionView collectionView, NSIndexPath indexPath) {
            // Always allow for highlighting
            return true;
        }

        public override void ItemHighlighted (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get cell and change to green background
            var cell = collectionView.CellForItem(indexPath);
            cell.ContentView.BackgroundColor = UIColor.FromRGB(183,208,57);
        }

        public override void ItemUnhighlighted (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get cell and return to blue background
            var cell = collectionView.CellForItem(indexPath);
            cell.ContentView.BackgroundColor = UIColor.FromRGB(164,205,255);
        }
        #endregion
    }
}
```

Ceci agira en tant que délégué pour la vue de collection. Méthodes ont été substituées pour mettre en surbrillance une cellule lorsque l’utilisateur interagit avec lui dans la vue de collection.

Ajoutez une dernière classe c# au projet et appelez-le `WaterfallCollectionView`. Modifier ce fichier et le rendre l’aspect suivant :

```csharp
using System;
using UIKit;
using System.Collections.Generic;
using Foundation;

namespace CollectionView
{
    [Register("WaterfallCollectionView")]
    public class WaterfallCollectionView : UICollectionView
    {

        #region Constructors
        public WaterfallCollectionView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Initialize
            DataSource = new WaterfallCollectionSource(this);
            Delegate = new WaterfallCollectionDelegate(this);

        }
        #endregion
    }
}
```

Notez que `DataSource` et `Delegate` que nous avons créés ci-dessus sont définies lors de la vue de collection est construite à partir de sa table de montage séquentiel (ou **.xib** fichier).

Modifier la **Main.storyboard** fichier et sélectionnez la vue de collection, puis basculer vers le **propriétés**. Définir le **classe** à personnalisé `WaterfallCollectionView` classe que nous avons défini ci-dessus :

Enregistrer les modifications apportées à l’interface utilisateur et exécuter l’application.
Si l’utilisateur sélectionne un élément dans la liste et la fait glisser vers un nouvel emplacement, les autres éléments seront anime automatiquement lors de leur déplacement à l’écart de l’élément.
Lorsque l’utilisateur dépose l’élément dans un nouvel emplacement, il tiendrons à cet emplacement. Par exemple :

[![](uicollectionview-images/intro01.png "Un exemple de faire glisser un élément vers un nouvel emplacement")](uicollectionview-images/intro01.png#lightbox)

<a name="Using-a-Custom-Gesture-Recognizer" />

### <a name="using-a-custom-gesture-recognizer"></a>À l’aide d’un module de reconnaissance de mouvement personnalisée

Dans les cas où vous ne pouvez pas utiliser un `UICollectionViewController` et vous devez utiliser une expression régulière `UIViewController`, ou si vous souhaitez prendre plus de contrôle sur les mouvements de glisser-déplacer, vous pouvez créer votre propre module de reconnaissance de mouvement personnalisé et l’ajouter à la vue de Collection lorsque la charge de la vue. Par exemple :

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Create a custom gesture recognizer
    var longPressGesture = new UILongPressGestureRecognizer ((gesture) => {

        // Take action based on state
        switch(gesture.State) {
        case UIGestureRecognizerState.Began:
            var selectedIndexPath = CollectionView.IndexPathForItemAtPoint(gesture.LocationInView(View));
            if (selectedIndexPath !=null) {
                CollectionView.BeginInteractiveMovementForItem(selectedIndexPath);
            }
            break;
        case UIGestureRecognizerState.Changed:
            CollectionView.UpdateInteractiveMovementTargetPosition(gesture.LocationInView(View));
            break;
        case UIGestureRecognizerState.Ended:
            CollectionView.EndInteractiveMovement();
            break;
        default:
            CollectionView.CancelInteractiveMovement();
            break;
        }

    });

    // Add the custom recognizer to the collection view
    CollectionView.AddGestureRecognizer(longPressGesture);
}
```

Ici, nous utilisons plusieurs nouvelles méthodes ajoutées à la vue de collection pour implémenter et de contrôler l’opération de glissement :

 - `BeginInteractiveMovementForItem` -Marque le début d’une opération de déplacement.
 - `UpdateInteractiveMovementTargetPosition` -Est envoyé comme emplacement de l’élément est mis à jour.
 - `EndInteractiveMovement` -Marque la fin du déplacement d’un élément.
 - `CancelInteractiveMovement` -Marque l’utilisateur de l’annulation de l’opération de déplacement.

Lorsque l’application est exécutée, l’opération de glissement fonctionne exactement comme la valeur par défaut faites glisser le module de reconnaissance de mouvement qui est fourni avec la vue de collection.

<a name="Custom-Layouts-and-Reording" />

### <a name="custom-layouts-and-reordering"></a>Dispositions personnalisées et réorganisation

Dans iOS 9, plusieurs nouvelles méthodes ont été ajoutés pour travailler avec des dispositions glisser pour réorganiser et personnalisées dans une vue de collection. Pour Explorer cette fonctionnalité, vous allez ajouter une disposition personnalisée à la collection.

Tout d’abord, ajoutez une nouvelle classe c# appelée `WaterfallCollectionLayout` au projet. Modifier et qu’elle ressemble à ce qui suit :

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;
using CoreGraphics;

namespace CollectionView
{
    [Register("WaterfallCollectionLayout")]
    public class WaterfallCollectionLayout : UICollectionViewLayout
    {
        #region Private Variables
        private int columnCount = 2;
        private nfloat minimumColumnSpacing = 10;
        private nfloat minimumInterItemSpacing = 10;
        private nfloat headerHeight = 0.0f;
        private nfloat footerHeight = 0.0f;
        private UIEdgeInsets sectionInset = new UIEdgeInsets(0, 0, 0, 0);
        private WaterfallCollectionRenderDirection itemRenderDirection = WaterfallCollectionRenderDirection.ShortestFirst;
        private Dictionary<nint,UICollectionViewLayoutAttributes> headersAttributes = new Dictionary<nint, UICollectionViewLayoutAttributes>();
        private Dictionary<nint,UICollectionViewLayoutAttributes> footersAttributes = new Dictionary<nint, UICollectionViewLayoutAttributes>();
        private List<CGRect> unionRects = new List<CGRect>();
        private List<nfloat> columnHeights = new List<nfloat>();
        private List<UICollectionViewLayoutAttributes> allItemAttributes = new List<UICollectionViewLayoutAttributes>();
        private List<List<UICollectionViewLayoutAttributes>> sectionItemAttributes = new List<List<UICollectionViewLayoutAttributes>>();
        private nfloat unionSize = 20;
        #endregion

        #region Computed Properties
        [Export("ColumnCount")]
        public int ColumnCount {
            get { return columnCount; }
            set {
                WillChangeValue ("ColumnCount");
                columnCount = value;
                DidChangeValue ("ColumnCount");

                InvalidateLayout ();
            }
        }

        [Export("MinimumColumnSpacing")]
        public nfloat MinimumColumnSpacing {
            get { return minimumColumnSpacing; }
            set {
                WillChangeValue ("MinimumColumnSpacing");
                minimumColumnSpacing = value;
                DidChangeValue ("MinimumColumnSpacing");

                InvalidateLayout ();
            }
        }

        [Export("MinimumInterItemSpacing")]
        public nfloat MinimumInterItemSpacing {
            get { return minimumInterItemSpacing; }
            set {
                WillChangeValue ("MinimumInterItemSpacing");
                minimumInterItemSpacing = value;
                DidChangeValue ("MinimumInterItemSpacing");

                InvalidateLayout ();
            }
        }

        [Export("HeaderHeight")]
        public nfloat HeaderHeight {
            get { return headerHeight; }
            set {
                WillChangeValue ("HeaderHeight");
                headerHeight = value;
                DidChangeValue ("HeaderHeight");

                InvalidateLayout ();
            }
        }

        [Export("FooterHeight")]
        public nfloat FooterHeight {
            get { return footerHeight; }
            set {
                WillChangeValue ("FooterHeight");
                footerHeight = value;
                DidChangeValue ("FooterHeight");

                InvalidateLayout ();
            }
        }

        [Export("SectionInset")]
        public UIEdgeInsets SectionInset {
            get { return sectionInset; }
            set {
                WillChangeValue ("SectionInset");
                sectionInset = value;
                DidChangeValue ("SectionInset");

                InvalidateLayout ();
            }
        }

        [Export("ItemRenderDirection")]
        public WaterfallCollectionRenderDirection ItemRenderDirection {
            get { return itemRenderDirection; }
            set {
                WillChangeValue ("ItemRenderDirection");
                itemRenderDirection = value;
                DidChangeValue ("ItemRenderDirection");

                InvalidateLayout ();
            }
        }
        #endregion

        #region Constructors
        public WaterfallCollectionLayout ()
        {
        }

        public WaterfallCollectionLayout(NSCoder coder) : base(coder) {

        }
        #endregion

        #region Public Methods
        public nfloat ItemWidthInSectionAtIndex(int section) {

            var width = CollectionView.Bounds.Width - SectionInset.Left - SectionInset.Right;
            return (nfloat)Math.Floor ((width - ((ColumnCount - 1) * MinimumColumnSpacing)) / ColumnCount);
        }
        #endregion

        #region Override Methods
        public override void PrepareLayout ()
        {
            base.PrepareLayout ();

            // Get the number of sections
            var numberofSections = CollectionView.NumberOfSections();
            if (numberofSections == 0)
                return;

            // Reset collections
            headersAttributes.Clear ();
            footersAttributes.Clear ();
            unionRects.Clear ();
            columnHeights.Clear ();
            allItemAttributes.Clear ();
            sectionItemAttributes.Clear ();

            // Initialize column heights
            for (int n = 0; n < ColumnCount; n++) {
                columnHeights.Add ((nfloat)0);
            }

            // Process all sections
            nfloat top = 0.0f;
            var attributes = new UICollectionViewLayoutAttributes ();
            var columnIndex = 0;
            for (nint section = 0; section < numberofSections; ++section) {
                // Calculate section specific metrics
                var minimumInterItemSpacing = (MinimumInterItemSpacingForSection == null) ? MinimumColumnSpacing :
                    MinimumInterItemSpacingForSection (CollectionView, this, section);

                // Calculate widths
                var width = CollectionView.Bounds.Width - SectionInset.Left - SectionInset.Right;
                var itemWidth = (nfloat)Math.Floor ((width - ((ColumnCount - 1) * MinimumColumnSpacing)) / ColumnCount);

                // Calculate section header
                var heightHeader = (HeightForHeader == null) ? HeaderHeight :
                    HeightForHeader (CollectionView, this, section);

                if (heightHeader > 0) {
                    attributes = UICollectionViewLayoutAttributes.CreateForSupplementaryView (UICollectionElementKindSection.Header, NSIndexPath.FromRowSection (0, section));
                    attributes.Frame = new CGRect (0, top, CollectionView.Bounds.Width, heightHeader);
                    headersAttributes.Add (section, attributes);
                    allItemAttributes.Add (attributes);

                    top = attributes.Frame.GetMaxY ();
                }

                top += SectionInset.Top;
                for (int n = 0; n < ColumnCount; n++) {
                    columnHeights [n] = top;
                }

                // Calculate Section Items
                var itemCount = CollectionView.NumberOfItemsInSection(section);
                List<UICollectionViewLayoutAttributes> itemAttributes = new List<UICollectionViewLayoutAttributes> ();

                for (nint n = 0; n < itemCount; n++) {
                    var indexPath = NSIndexPath.FromRowSection (n, section);
                    columnIndex = NextColumnIndexForItem (n);
                    var xOffset = SectionInset.Left + (itemWidth + MinimumColumnSpacing) * (nfloat)columnIndex;
                    var yOffset = columnHeights [columnIndex];
                    var itemSize = (SizeForItem == null) ? new CGSize (0, 0) : SizeForItem (CollectionView, this, indexPath);
                    nfloat itemHeight = 0.0f;

                    if (itemSize.Height > 0.0f && itemSize.Width > 0.0f) {
                        itemHeight = (nfloat)Math.Floor (itemSize.Height * itemWidth / itemSize.Width);
                    }

                    attributes = UICollectionViewLayoutAttributes.CreateForCell (indexPath);
                    attributes.Frame = new CGRect (xOffset, yOffset, itemWidth, itemHeight);
                    itemAttributes.Add (attributes);
                    allItemAttributes.Add (attributes);
                    columnHeights [columnIndex] = attributes.Frame.GetMaxY () + MinimumInterItemSpacing;
                }
                sectionItemAttributes.Add (itemAttributes);

                // Calculate Section Footer
                nfloat footerHeight = 0.0f;
                columnIndex = LongestColumnIndex();
                top = columnHeights [columnIndex] - MinimumInterItemSpacing + SectionInset.Bottom;
                footerHeight = (HeightForFooter == null) ? FooterHeight : HeightForFooter(CollectionView, this, section);

                if (footerHeight > 0) {
                    attributes = UICollectionViewLayoutAttributes.CreateForSupplementaryView (UICollectionElementKindSection.Footer, NSIndexPath.FromRowSection (0, section));
                    attributes.Frame = new CGRect (0, top, CollectionView.Bounds.Width, footerHeight);
                    footersAttributes.Add (section, attributes);
                    allItemAttributes.Add (attributes);
                    top = attributes.Frame.GetMaxY ();
                }

                for (int n = 0; n < ColumnCount; n++) {
                    columnHeights [n] = top;
                }
            }

            var i =0;
            var attrs = allItemAttributes.Count;
            while(i < attrs) {
                var rect1 = allItemAttributes [i].Frame;
                i = (int)Math.Min (i + unionSize, attrs) - 1;
                var rect2 = allItemAttributes [i].Frame;
                unionRects.Add (CGRect.Union (rect1, rect2));
                i++;
            }

        }

        public override CGSize CollectionViewContentSize {
            get {
                if (CollectionView.NumberOfSections () == 0) {
                    return new CGSize (0, 0);
                }

                var contentSize = CollectionView.Bounds.Size;
                contentSize.Height = columnHeights [0];
                return contentSize;
            }
        }

        public override UICollectionViewLayoutAttributes LayoutAttributesForItem (NSIndexPath indexPath)
        {
            if (indexPath.Section >= sectionItemAttributes.Count) {
                return null;
            }

            if (indexPath.Item >= sectionItemAttributes [indexPath.Section].Count) {
                return null;
            }

            var list = sectionItemAttributes [indexPath.Section];
            return list [(int)indexPath.Item];
        }

        public override UICollectionViewLayoutAttributes LayoutAttributesForSupplementaryView (NSString kind, NSIndexPath indexPath)
        {
            var attributes = new UICollectionViewLayoutAttributes ();

            switch (kind) {
            case "header":
                attributes = headersAttributes [indexPath.Section];
                break;
            case "footer":
                attributes = footersAttributes [indexPath.Section];
                break;
            }

            return attributes;
        }

        public override UICollectionViewLayoutAttributes[] LayoutAttributesForElementsInRect (CGRect rect)
        {
            var begin = 0;
            var end = unionRects.Count;
            List<UICollectionViewLayoutAttributes> attrs = new List<UICollectionViewLayoutAttributes> ();


            for (int i = 0; i < end; i++) {
                if (rect.IntersectsWith(unionRects[i])) {
                    begin = i * (int)unionSize;
                }
            }

            for (int i = end - 1; i >= 0; i--) {
                if (rect.IntersectsWith (unionRects [i])) {
                    end = (int)Math.Min ((i + 1) * (int)unionSize, allItemAttributes.Count);
                    break;
                }
            }

            for (int i = begin; i < end; i++) {
                var attr = allItemAttributes [i];
                if (rect.IntersectsWith (attr.Frame)) {
                    attrs.Add (attr);
                }
            }

            return attrs.ToArray();
        }

        public override bool ShouldInvalidateLayoutForBoundsChange (CGRect newBounds)
        {
            var oldBounds = CollectionView.Bounds;
            return (newBounds.Width != oldBounds.Width);
        }
        #endregion

        #region Private Methods
        private int ShortestColumnIndex() {
            var index = 0;
            var shortestHeight = nfloat.MaxValue;
            var n = 0;

            // Scan each column for the shortest height
            foreach (nfloat height in columnHeights) {
                if (height < shortestHeight) {
                    shortestHeight = height;
                    index = n;
                }
                ++n;
            }

            return index;
        }

        private int LongestColumnIndex() {
            var index = 0;
            var longestHeight = nfloat.MinValue;
            var n = 0;

            // Scan each column for the shortest height
            foreach (nfloat height in columnHeights) {
                if (height > longestHeight) {
                    longestHeight = height;
                    index = n;
                }
                ++n;
            }

            return index;
        }

        private int NextColumnIndexForItem(nint item) {
            var index = 0;

            switch (ItemRenderDirection) {
            case WaterfallCollectionRenderDirection.ShortestFirst:
                index = ShortestColumnIndex ();
                break;
            case WaterfallCollectionRenderDirection.LeftToRight:
                index = ColumnCount;
                break;
            case WaterfallCollectionRenderDirection.RightToLeft:
                index = (ColumnCount - 1) - ((int)item / ColumnCount);
                break;
            }

            return index;
        }
        #endregion

        #region Events
        public delegate CGSize WaterfallCollectionSizeDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, NSIndexPath indexPath);
        public delegate nfloat WaterfallCollectionFloatDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, nint section);
        public delegate UIEdgeInsets WaterfallCollectionEdgeInsetsDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, nint section);

        public event WaterfallCollectionSizeDelegate SizeForItem;
        public event WaterfallCollectionFloatDelegate HeightForHeader;
        public event WaterfallCollectionFloatDelegate HeightForFooter;
        public event WaterfallCollectionEdgeInsetsDelegate InsetForSection;
        public event WaterfallCollectionFloatDelegate MinimumInterItemSpacingForSection;
        #endregion
    }
}
```

Cela peut être utilisé de classe pour fournir un personnalisé de deux colonnes, la disposition du type de cascade pour la vue de collection.
Le code utilise le codage de la clé-valeur (via la `WillChangeValue` et `DidChangeValue` méthodes) pour fournir la liaison de données pour notre propriétés calculées dans cette classe.

Modifiez ensuite le `WaterfallCollectionSource` et apportez les modifications et additions suivantes :

```csharp
private Random rnd = new Random();
...

public List<nfloat> Heights { get; set; } = new List<nfloat> ();
...

public WaterfallCollectionSource (WaterfallCollectionView collectionView)
{
    // Initialize
    CollectionView = collectionView;

    // Init numbers collection
    for (int n = 0; n < 100; ++n) {
        Numbers.Add (n);
        Heights.Add (rnd.Next (0, 100) + 40.0f);
    }
}
```

Cela créera une hauteur aléatoire pour chacun des éléments qui seront affichera dans la liste.

Modifiez ensuite la `WaterfallCollectionView` classe et ajoutez la propriété d’assistance suivante :

```csharp
public WaterfallCollectionSource Source {
    get { return (WaterfallCollectionSource)DataSource; }
}
```

Cela rend plus facile à obtenir à notre source de données (et la hauteur de l’élément) à partir de la disposition personnalisée.

Enfin, modifier le contrôleur de la vue et ajoutez le code suivant :

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    var waterfallLayout = new WaterfallCollectionLayout ();

    // Wireup events
    waterfallLayout.SizeForItem += (collectionView, layout, indexPath) => {
        var collection = collectionView as WaterfallCollectionView;
        return new CGSize((View.Bounds.Width-40)/3,collection.Source.Heights[(int)indexPath.Item]);
    };

    // Attach the custom layout to the collection
    CollectionView.SetCollectionViewLayout(waterfallLayout, false);
}
```

Cela crée une instance de notre disposition personnalisée, définit l’événement pour fournir la taille de chaque élément et attache la nouvelle disposition à la vue de collection.

Si nous exécutons l’application Xamarin.iOS à nouveau, la vue de collection ressemble maintenant à ce qui suit :

[![](uicollectionview-images/custom01.png "La vue de collection doit maintenant ressembler à ceci")](uicollectionview-images/custom01.png#lightbox)

Nous pouvons toujours glisser pour réorganiser les éléments avant, mais les éléments seront désormais taille pour s’ajuster à leur nouvel emplacement lorsqu’ils sont déplacés.

## <a name="collection-view-changes"></a>Afficher les modifications de collection

Dans les sections suivantes, nous allons prendre examine en détail les modifications apportées à chaque classe dans la vue de collection par iOS 9.

### <a name="uicollectionview"></a>UICollectionView

Les compléments ou les modifications suivantes ont été apportées à la `UICollectionView` classe IOS 9 :

 - `BeginInteractiveMovementForItem` – Marque le début d’une opération glisser.
 - `CancelInteractiveMovement` – Informe de la collection de vue que l’utilisateur a annulé une opération glisser.
 - `EndInteractiveMovement` – Informe de la collection de vue que l’utilisateur a terminé une opération glisser.
 - `GetIndexPathsForVisibleSupplementaryElements` : Renvoie la `indexPath` d’un en-tête ou un pied de page dans une section de vue de collection.
 - `GetSupplementaryView` – Retourne l’en-tête donné ou un pied de page.
 - `GetVisibleSupplementaryViews` : Retourne une liste de tous les en-tête visible et les pieds de page.
 - `UpdateInteractiveMovementTargetPosition` – Informe de la collection de vue que l’utilisateur a été déplacé, ou correspond à un déplacement, un élément pendant une opération glisser.

### <a name="uicollectionviewcontroller"></a>UICollectionViewController

Les compléments ou les modifications suivantes ont été apportées à la `UICollectionViewController` classe dans iOS 9 :

 - `InstallsStandardGestureForInteractiveMovement` – If `true` la reconnaissance de mouvement nouvelle qui prend en charge automatiquement les glisser à réorganiser sera utilisée.
 - `CanMoveItem` – Informe la vue de collection, si un élément donné peut être glisser réorganisée.
 - `GetTargetContentOffset` – Permet d’obtenir le décalage d’un élément d’une collection donnée.
 - `GetTargetIndexPathForMove` : Obtient le `indexPath` d’un élément donné pour une opération glisser.
 - `MoveItem` – Déplace l’ordre d’un élément donné dans la liste.


### <a name="uicollectionviewdatasource"></a>UICollectionViewDataSource

Les compléments ou les modifications suivantes ont été apportées à la `UICollectionViewDataSource` classe dans iOS 9 :

 - `CanMoveItem` – Informe la vue de collection, si un élément donné peut être glisser réorganisée.
 - `MoveItem` – Déplace l’ordre d’un élément donné dans la liste.

### <a name="uicollectionviewdelegate"></a>UICollectionViewDelegate

Les compléments ou les modifications suivantes ont été apportées à la `UICollectionViewDelegate` classe dans iOS 9 :

 - `GetTargetContentOffset` – Permet d’obtenir le décalage d’un élément d’une collection donnée.
 - `GetTargetIndexPathForMove` : Obtient le `indexPath` d’un élément donné pour une opération glisser.

### <a name="uicollectionviewflowlayout"></a>UICollectionViewFlowLayout

Les compléments ou les modifications suivantes ont été apportées à la `UICollectionViewFlowLayout` classe dans iOS 9 :

 - `SectionFootersPinToVisibleBounds` – Y colle les pieds de page de section pour les limites de vue de collection visible.
 - `SectionHeadersPinToVisibleBounds` – Y colle les en-têtes de section pour les limites de vue de collection visible.

### <a name="uicollectionviewlayout"></a>UICollectionViewLayout

Les compléments ou les modifications suivantes ont été apportées à la `UICollectionViewLayout` classe dans iOS 9 :

 - `GetInvalidationContextForEndingInteractiveMovementOfItems` – Retourne le contexte d’invalidation à la fin d’une opération glisser lorsque l’utilisateur termine l’opération glisser ou annule.
 - `GetInvalidationContextForInteractivelyMovingItems` – Retourne le contexte de l’invalidation au début d’une opération glisser.
 - `GetLayoutAttributesForInteractivelyMovingItem` : Obtient les attributs de disposition pour un élément donné tout en faisant glisser un élément.
 - `GetTargetIndexPathForInteractivelyMovingItem` : Renvoie la `indexPath` de l’élément qui se trouve au point spécifié lorsque vous faites glisser un élément.

### <a name="uicollectionviewlayoutattributes"></a>UICollectionViewLayoutAttributes

Les compléments ou les modifications suivantes ont été apportées à la `UICollectionViewLayoutAttributes` classe dans iOS 9 :

 - `CollisionBoundingPath` : Retourne le chemin d’accès de collision de deux éléments pendant une opération glisser.
 - `CollisionBoundsType` – Retourne le type de collision (comme un `UIDynamicItemCollisionBoundsType`) qui s’est produite pendant une opération glisser.

### <a name="uicollectionviewlayoutinvalidationcontext"></a>UICollectionViewLayoutInvalidationContext

Les compléments ou les modifications suivantes ont été apportées à la `UICollectionViewLayoutInvalidationContext` classe dans iOS 9 :

 - `InteractiveMovementTarget` – Retourne l’élément de la cible d’une opération glisser.
 - `PreviousIndexPathsForInteractivelyMovingItems` : Renvoie la `indexPaths` d’autres éléments impliqués dans une opération de glissement pour réorganiser l’opération.
 - `TargetIndexPathsForInteractivelyMovingItems` : Renvoie la `indexPaths` d’éléments qui sont reclassées suite à une opération de glissement à réorganiser.

### <a name="uicollectionviewsource"></a>UICollectionViewSource

Les compléments ou les modifications suivantes ont été apportées à la `UICollectionViewSource` classe dans iOS 9 :

 - `CanMoveItem` – Informe la vue de collection, si un élément donné peut être glisser réorganisée.
 - `GetTargetContentOffset` : Retourne les décalages des éléments qui seront déplacées via une opération de glissement à réorganiser.
 - `GetTargetIndexPathForMove` : Renvoie la `indexPath` d’un élément qui est déplacé lors d’une opération de glissement à réorganiser.
 - `MoveItem` – Déplace l’ordre d’un élément donné dans la liste.

## <a name="summary"></a>Récapitulatif

Cet article a couvert les modifications apportées aux vues de collection dans iOS 9 et décrit comment les implémenter dans Xamarin.iOS.
Il couvert l’implémentation d’une action de glissement pour réorganiser simple dans une vue de collection ; à l’aide d’un module de reconnaissance de mouvement personnalisée avec glisser à réorganiser ; et faites glisser pour réorganiser incidence sur une mise en page de vue de collection personnalisée.

## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [Exemple de vue de collection](https://developer.xamarin.com/samples/monotouch/ios9/CollectionView/)
- [SimpleCollectionView (exemple)](https://developer.xamarin.com/samples/SimpleCollectionView/)
- [Événements, protocoles et délégués](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Utilisation des Tables et des cellules](~/ios/user-interface/controls/tables/index.md)
