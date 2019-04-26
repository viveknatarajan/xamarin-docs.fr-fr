---
title: Modes plan dans Xamarin.Mac
description: Cet article aborde l’utilisation des modes plan dans une application Xamarin.Mac. Il décrit la création et gestion des modes plan dans Xcode et Interface Builder et leur utilisation par programmation.
ms.prod: xamarin
ms.assetid: 043248EE-11DA-4E96-83A3-08824A4F2E01
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: fd97dbbe102c5a755c4a8974cf1a952c0050ac7c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61292697"
---
# <a name="outline-views-in-xamarinmac"></a>Modes plan dans Xamarin.Mac

_Cet article aborde l’utilisation des modes plan dans une application Xamarin.Mac. Il décrit la création et gestion des modes plan dans Xcode et Interface Builder et leur utilisation par programmation._

Lorsque vous travaillez avec C# et .NET dans une application Xamarin.Mac, que vous avez accès à la structure du même vues dont un développeur travaillant *Objective-C* et *Xcode* est. Comme Xamarin.Mac s’intègre directement à Xcode, vous pouvez utiliser de Xcode _Interface Builder_ pour créer et gérer vos modes de plan (ou éventuellement pour les créer directement en code c#).

Un mode plan est un type de Table qui permet à l’utilisateur développer ou réduire des lignes de données hiérarchiques. Comme une vue de Table, une vue hiérarchique affiche les données pour un ensemble d’éléments associés, avec des lignes représentant des éléments individuels et des colonnes représentant les attributs de ces éléments. Contrairement à une vue de Table dans une vue hiérarchique des éléments ne sont pas dans une liste plate, ils sont organisés dans une hiérarchie, comme les fichiers et dossiers sur un disque dur.

[![](outline-view-images/populate03.png "Exécution d’une application exemple")](outline-view-images/populate03.png#lightbox)

Dans cet article, nous traiterons les notions de base de l’utilisation des modes plan dans une application Xamarin.Mac. Il est fortement recommandé que vous travaillez via le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) et [Outlets et Actions](~/mac/get-started/hello-mac.md#outlets-and-actions) sections, tel qu’il couvre les principaux concepts et techniques que nous utilisons dans cet article.

Vous pouvez souhaiter de jeter un coup de œil à la [C# exposition de classes / méthodes vers Objective-C](~/mac/internals/how-it-works.md) section de la [éléments internes de Xamarin.Mac](~/mac/internals/how-it-works.md) de document, il explique le `Register` et `Export` commandes utilisé pour AutoEventWireup vos classes c# pour les objets Objective-C et les éléments d’interface utilisateur.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-outline-views"></a>Introduction aux modes plan

Un mode plan est un type de Table qui permet à l’utilisateur développer ou réduire des lignes de données hiérarchiques. Comme une vue de Table, une vue hiérarchique affiche les données pour un ensemble d’éléments associés, avec des lignes représentant des éléments individuels et des colonnes représentant les attributs de ces éléments. Contrairement à une vue de Table dans une vue hiérarchique des éléments ne sont pas dans une liste plate, ils sont organisés dans une hiérarchie, comme les fichiers et dossiers sur un disque dur.

Si un élément dans une vue hiérarchique contient d’autres éléments, il peut être développée ou réduite par l’utilisateur. Un élément pouvant être développé affiche un triangle, qui pointe vers la droite quand l’élément est réduit et pointe vers le bas lorsque l’élément est développé. En cliquant sur le triangle de provoque l’élément développer ou réduire.

Le mode plan (`NSOutlineView`) est une sous-classe de la vue de Table (`NSTableView`) et par conséquent, une grande partie de son comportement hérite sa classe parente. Par conséquent, de nombreuses opérations prises en charge par une vue de Table, telles que la sélection des lignes ou colonnes, repositionnement des colonnes en faisant glisser les en-têtes de colonnes, etc., sont également pris en charge par un mode plan. Une application Xamarin.Mac possède le contrôle de ces fonctionnalités et pouvez configurer les paramètres du mode plan (soit dans le code ou d’Interface Builder) pour autoriser ou interdire certaines opérations.

Une vue hiérarchique ne stocke pas ses propres données, au lieu de cela, elle s’appuie sur une Source de données (`NSOutlineViewDataSource`) pour fournir les lignes et les colonnes requises sur une en fonction des besoins.

Comportement d’un mode plan peut être personnalisé en fournissant une sous-classe du délégué de mode plan (`NSOutlineViewDelegate`) pour prendre en charge la gestion de colonne de structure, tapez pour sélectionner des fonctionnalités, la sélection de ligne, la modification, suivi personnalisé et des vues personnalisées de chaque les colonnes et lignes.

Dans la mesure où une vue hiérarchique une grande partie de son comportement et les fonctionnalités partage avec une vue de Table, vous pouvez souhaiter passer par notre [affichages tableau](~/mac/user-interface/table-view.md) documentation avant de poursuivre cet article.

<a name="Creating_and_Maintaining_Outline_Views_in_Xcode" />

## <a name="creating-and-maintaining-outline-views-in-xcode"></a>Création et gestion des modes plan dans Xcode

Lorsque vous créez une nouvelle application Xamarin.Mac Cocoa, vous obtenez une fenêtre vide, standard par défaut. Ce paramètre, windows est défini dans un `.storyboard` fichier automatiquement inclus dans le projet. Pour modifier votre conception de windows, dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` fichier :

[![](outline-view-images/edit01.png "En sélectionnant le storyboard principal")](outline-view-images/edit01.png#lightbox)

La conception de la fenêtre s’ouvre dans Xcode Interface Builder :

[![](outline-view-images/edit02.png "Modification de l’interface utilisateur dans Xcode")](outline-view-images/edit02.png#lightbox)

Type `outline` dans le **l’inspecteur de bibliothèque** zone de recherche pour le rendre plus facile à trouver les contrôles de mode plan :

[![](outline-view-images/edit03.png "Sélection d’un mode plan à partir de la bibliothèque")](outline-view-images/edit03.png#lightbox)

Faire glisser une vue hiérarchique du contrôleur d’affichage dans le **Éditeur d’Interface**, rendent la remplir la zone de contenu du contrôleur d’affichage et affectez-lui la valeur où il augmente et diminue avec la fenêtre dans le **éditeur de contrainte**:

[![](outline-view-images/edit04.png "Modification des contraintes")](outline-view-images/edit04.png#lightbox)

Sélectionnez le mode plan dans le **hiérarchie des interfaces** et les propriétés suivantes sont disponibles dans le **inspecteur d’attributs**:

[![](outline-view-images/edit05.png "L’inspecteur d’attributs")](outline-view-images/edit05.png#lightbox)

- **Décrit la colonne** -la colonne de Table dans laquelle les données hiérarchiques sont affichées.
- **Colonne de structure AutoSave** : si `true`, la colonne de structure est automatiquement enregistrée et restaurée entre les exécutions de l’application.
- **Mise en retrait** -la quantité pour mettre en retrait les colonnes sous un élément développé.
- **Mise en retrait suit cellules** : si `true`, seront en retrait la marque de mise en retrait, ainsi que les cellules.
- **Éléments de développé AutoSave** : si `true`, l’état développé/réduit des éléments est automatiquement enregistré et restauré entre les exécutions de l’application.
- **Mode de contenu** -vous permet d’utiliser deux vues (`NSView`) ou les cellules (`NSCell`) pour afficher les données dans les lignes et colonnes. À compter de macOS 10.7, vous devez utiliser des vues.
- **Regrouper des lignes de flotte** : si `true`, la vue Table dessinera cellules groupées comme s’ils sont flottants.
- **Colonnes** -définit le nombre de colonnes affichées.
- **En-têtes** : si `true`, les colonnes ont des en-têtes.
- **Réorganisation** : si `true`, l’utilisateur sera en mesure de faire glisser réorganiser les colonnes dans la table.
- **Redimensionnement** : si `true`, l’utilisateur sera en mesure de faire glisser pour redimensionner des colonnes, les en-têtes de colonne.
- **Dimensionnement de colonnes** -contrôle la façon dont la table est automatiquement la taille des colonnes.
- **Mettez en surbrillance** -contrôles le type de mise en surbrillance de la table utilise lorsqu’une cellule est sélectionnée.
- **Remplacement des lignes** : si `true`, jamais autres ligne aura une couleur d’arrière-plan différente.
- **Quadrillage horizontal** -sélectionne le type de bordure dessinée entre les cellules horizontalement.
- **Quadrillage vertical** -sélectionne le type de bordure dessinée entre les cellules verticalement.
- **Couleur de la grille** -définit la couleur de bordure de cellule.
- **Arrière-plan** -définit la couleur d’arrière-plan de cellule.
- **Sélection** -vous permettent de contrôler la façon dont l’utilisateur peut sélectionner des cellules dans la table en tant que :
    - **Plusieurs** : si `true`, l’utilisateur peut sélectionner plusieurs lignes et colonnes.
    - **Colonne** : si `true`, l’utilisateur peut sélectionner des colonnes.
    - **Tapez sélectionner** : si `true`, l’utilisateur peut taper un caractère pour sélectionner une ligne.
    - **Vide** : si `true`, l’utilisateur n’est pas obligé de sélectionner une ligne ou une colonne, la table autorise pour aucune sélection du tout.
- **Enregistrement automatique** -le nom que le format des tables est automatiquement enregistrer sous.
- **Informations sur les colonnes** : si `true`, l’ordre et la largeur des colonnes sont automatiquement enregistrés.
- **Sauts de ligne** - sélectionnez la façon dont la cellule gère des sauts de ligne.
- **Tronque la dernière ligne Visible** : si `true`, la cellule est tronquée dans le données peuvent ne tiennent ne pas dans ses limites.

> [!IMPORTANT]
> Sauf si vous mettez à jour une application Xamarin.Mac héritée, `NSView` en modes de plan doit être utilisés sur `NSCell` en fonction des vues de Table. `NSCell` est considéré comme hérité et de ne pas être gérée à l’avenir.

Sélectionnez une colonne de Table dans le **hiérarchie des interfaces** et les propriétés suivantes sont disponibles dans le **inspecteur d’attributs**:

[![](outline-view-images/edit06.png "L’inspecteur d’attributs")](outline-view-images/edit06.png#lightbox)

- **Titre** -définit le titre de la colonne.
- **Alignement** -définir l’alignement du texte dans les cellules.
- **Police du titre** -sélectionne la police pour le texte d’en-tête de la cellule.
- **Clé de tri** -est la clé utilisée pour trier les données dans la colonne. Laissez vide si l’utilisateur ne peut pas trier cette colonne.
- **Sélecteur de** -est la **Action** utilisé pour effectuer le tri. Laissez vide si l’utilisateur ne peut pas trier cette colonne.
- **Commande** -est de l’ordre de tri pour les données de colonnes.
- **Redimensionnement** -sélectionne le type de redimensionnement pour la colonne.
- **Modifiable** : si `true`, l’utilisateur peut modifier les cellules dans une table en fonction de cellule.
- **Masqué** : si `true`, la colonne est masquée.

Vous pouvez également redimensionner la colonne en faisant glisser sa poignée (centré verticalement sur la droite de la colonne) à gauche ou à droite.

Nous allons sélectionner la chaque colonne dans notre vue Table et donnez à la première colonne un **titre** de `Product` et la deuxième `Details`.

Sélectionnez une vue de cellule de Table (`NSTableViewCell`) dans le **hiérarchie des interfaces** et les propriétés suivantes sont disponibles dans le **inspecteur d’attributs**:

[![](outline-view-images/edit07.png "L’inspecteur d’attributs")](outline-view-images/edit07.png#lightbox)

Il s’agit de toutes les propriétés d’une vue standard. Vous avez également la possibilité de redimensionner les lignes pour cette colonne ici.

Sélectionnez une cellule d’affichage Table (par défaut, il s’agit d’un `NSTextField`) dans le **hiérarchie des interfaces** et les propriétés suivantes sont disponibles dans le **inspecteur d’attributs**:

[![](outline-view-images/edit08.png "L’inspecteur d’attributs")](outline-view-images/edit08.png#lightbox)

Vous aurez toutes les propriétés d’un champ de texte standard pour définir ici. Par défaut, un champ de texte standard est utilisé pour afficher les données d’une cellule dans une colonne.

Sélectionnez une vue de cellule de Table (`NSTableFieldCell`) dans le **hiérarchie des interfaces** et les propriétés suivantes sont disponibles dans le **inspecteur d’attributs**:

[![](outline-view-images/edit09.png "L’inspecteur d’attributs")](outline-view-images/edit09.png#lightbox)

Plus important ici sont les suivantes :

- **Disposition** - sélectionnez la façon dont les cellules de cette colonne sont disposés.
- **Utilise le Mode à ligne unique** : si `true`, la cellule est limitée à une seule ligne.
- **La largeur de première exécution disposition** : si `true`, la cellule préféreront la largeur définie pour elle (manuellement ou automatiquement) quand il est affiché à la première fois que l’application est exécutée.
- **Action** -contrôle le moment où la modification **Action** est envoyé pour la cellule.
- **Comportement** -définit si une cellule est sélectionnable ou modifiable.
- **Texte riche** : si `true`, la cellule peut afficher du texte mis en forme et de style.
- **Annuler** : si `true`, la cellule assume la responsabilité pour qu’il est annuler le comportement.

Sélectionnez la vue de cellule de Table (`NSTableFieldCell`) en bas d’une colonne de Table dans le **hiérarchie des interfaces**:

[![](outline-view-images/edit11.png "En sélectionnant la vue de cellule de table")](outline-view-images/edit10.png#lightbox)

Cela vous permet de modifier la vue de cellule de Table utilisée comme base _modèle_ pour toutes les cellules créés pour la colonne donnée.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Ajoute des Actions et Outlets

Juste comme tout autre contrôle de l’interface utilisateur Cocoa, nous devons exposer notre mode plan et il est de colonnes et des cellules à c# code à l’aide de **Actions** et **Outlets** (basé sur les fonctionnalités requises).

Le processus est identique pour n’importe quel élément de mode plan que nous souhaitons exposer :

1. Basculez vers le **éditeur de l’Assistant** et vérifiez que le `ViewController.h` fichier est sélectionné : 

    [![](outline-view-images/edit11.png "En sélectionnant le fichier .h correct")](outline-view-images/edit11.png#lightbox)
2. Sélectionnez le mode plan à partir de la **hiérarchie des interfaces**, cliquez et faites glisser vers le `ViewController.h` fichier.
3. Créer un **Outlet** pour le mode plan appelé `ProductOutline`: 

    [![](outline-view-images/edit13.png "Configuration d’un Outlet")](outline-view-images/edit13.png#lightbox)
4. Créer **Outlets** pour les colonnes de tables aussi appelé `ProductColumn` et `DetailsColumn`: 

    [![](outline-view-images/edit14.png "Configuration d’un Outlet")](outline-view-images/edit14.png#lightbox)
5. Enregistrer les modifications et revenir à Visual Studio pour Mac se synchroniser avec Xcode.

Nous allons ensuite écrire à l’affichage de code des données pour le plan lors de l’application est exécutée.

<a name="Populating_the_Table_View" />

## <a name="populating-the-outline-view"></a>Remplissage de l’affichage hiérarchique

Avec notre mode plan conçu dans Interface Builder et exposées via un **Outlet**, nous devons ensuite créer le code c# pour le remplir.

Tout d’abord, nous allons créer un nouveau `Product` classe pour contenir les informations pour les lignes individuelles et les groupes de produits de sub. Dans le **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **nouveau fichier...** Sélectionnez **général** > **classe vide**, entrez `Product` pour le **nom** et cliquez sur le **New** bouton :

[![](outline-view-images/populate01.png "Création d’une classe vide")](outline-view-images/populate01.png#lightbox)

Rendre le `Product.cs` fichier ressemble ce qui suit :

```csharp
using System;
using Foundation;
using System.Collections.Generic;

namespace MacOutlines
{
    public class Product : NSObject
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Computed Properties
        public string Title { get; set;} = "";
        public string Description { get; set;} = "";
        public bool IsProductGroup {
            get { return (Products.Count > 0); }
        }
        #endregion

        #region Constructors
        public Product ()
        {
        }

        public Product (string title, string description)
        {
            this.Title = title;
            this.Description = description;
        }
        #endregion
    }
}
```

Ensuite, nous devons créer une sous-classe de `NSOutlineDataSource` pour fournir les données de notre plan comme cela est demandé. Dans le **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **nouveau fichier...** Sélectionnez **général** > **classe vide**, entrez `ProductOutlineDataSource` pour le **nom** et cliquez sur le **New** bouton.

Modifier la `ProductTableDataSource.cs` de fichier et qu’elle ressemble à ce qui suit :

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacOutlines
{
    public class ProductOutlineDataSource : NSOutlineViewDataSource
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Constructors
        public ProductOutlineDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override nint GetChildrenCount (NSOutlineView outlineView, NSObject item)
        {
            if (item == null) {
                return Products.Count;
            } else {
                return ((Product)item).Products.Count;
            }

        }

        public override NSObject GetChild (NSOutlineView outlineView, nint childIndex, NSObject item)
        {
            if (item == null) {
                return Products [childIndex];
            } else {
                return ((Product)item).Products [childIndex];
            }
                
        }

        public override bool ItemExpandable (NSOutlineView outlineView, NSObject item)
        {
            if (item == null) {
                return Products [0].IsProductGroup;
            } else {
                return ((Product)item).IsProductGroup;
            }
        
        }
        #endregion
    }
}
```

Cette classe dispose d’un stockage pour les éléments de notre mode plan et remplace le `GetChildrenCount` pour retourner le nombre de lignes dans la table. Le `GetChild` retourne un élément parent ou enfant spécifique (comme requis par le mode plan) et le `ItemExpandable` définit l’élément spécifié comme un parent ou enfant.

Enfin, nous devons créer une sous-classe de `NSOutlineDelegate` pour fournir le comportement de notre plan. Dans le **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **nouveau fichier...** Sélectionnez **général** > **classe vide**, entrez `ProductOutlineDelegate` pour le **nom** et cliquez sur le **New** bouton.

Modifier la `ProductOutlineDelegate.cs` de fichier et qu’elle ressemble à ce qui suit :

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacOutlines
{
    public class ProductOutlineDelegate : NSOutlineViewDelegate
    {
        #region Constants 
        private const string CellIdentifier = "ProdCell";
        #endregion

        #region Private Variables
        private ProductOutlineDataSource DataSource;
        #endregion

        #region Constructors
        public ProductOutlineDelegate (ProductOutlineDataSource datasource)
        {
            this.DataSource = datasource;
        }
        #endregion

        #region Override Methods

        public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)outlineView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Cast item
            var product = item as Product;

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Product":
                view.StringValue = product.Title;
                break;
            case "Details":
                view.StringValue = product.Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

Lorsque nous créons une instance de la `ProductOutlineDelegate`, nous passons également dans une instance de la `ProductOutlineDataSource` qui fournit les données pour le contour. Le `GetView` méthode est chargée de retourner une vue (données) pour afficher la cellule pour une colonne donnée et une ligne. Si possible, une vue existante sera réutilisée pour afficher la cellule, si ce n’est pas une nouvelle vue doit être créée.

Pour remplir le contour, nous allons modifier le `MainWindow.cs` et effectuez le `AwakeFromNib` méthode ressemble ce qui suit :

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create data source and populate
    var DataSource = new ProductOutlineDataSource ();

    var Vegetables = new Product ("Vegetables", "Greens and Other Produce");
    Vegetables.Products.Add (new Product ("Cabbage", "Brassica oleracea - Leaves, axillary buds, stems, flowerheads"));
    Vegetables.Products.Add (new Product ("Turnip", "Brassica rapa - Tubers, leaves"));
    Vegetables.Products.Add (new Product ("Radish", "Raphanus sativus - Roots, leaves, seed pods, seed oil, sprouting"));
    Vegetables.Products.Add (new Product ("Carrot", "Daucus carota - Root tubers"));
    DataSource.Products.Add (Vegetables);

    var Fruits = new Product ("Fruits", "Fruit is a part of a flowering plant that derives from specific tissues of the flower");
    Fruits.Products.Add (new Product ("Grape", "True Berry"));
    Fruits.Products.Add (new Product ("Cucumber", "Pepo"));
    Fruits.Products.Add (new Product ("Orange", "Hesperidium"));
    Fruits.Products.Add (new Product ("Blackberry", "Aggregate fruit"));
    DataSource.Products.Add (Fruits);

    var Meats = new Product ("Meats", "Lean Cuts");
    Meats.Products.Add (new Product ("Beef", "Cow"));
    Meats.Products.Add (new Product ("Pork", "Pig"));
    Meats.Products.Add (new Product ("Veal", "Young Cow"));
    DataSource.Products.Add (Meats);

    // Populate the outline
    ProductOutline.DataSource = DataSource;
    ProductOutline.Delegate = new ProductOutlineDelegate (DataSource);

}
```

Si nous exécutons l’application, ce qui suit s’affiche :

[![](outline-view-images/populate02.png "L’affichage réduit")](outline-view-images/populate02.png#lightbox)

Si nous allons développer un nœud en mode plan, il ressemble à ce qui suit :

[![](outline-view-images/populate03.png "La vue développée")](outline-view-images/populate03.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Tri par colonne

Nous allons autoriser l’utilisateur de trier les données dans le plan en cliquant sur un en-tête de colonne. Tout d’abord, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour modification dans l’Interface Builder. Sélectionnez le `Product` colonne, entrez `Title` pour le **clé de tri**, `compare:` pour le **sélecteur** et sélectionnez `Ascending` pour le **ordre**:

[![](outline-view-images/sort01.png "Définition de l’ordre de clé de tri")](outline-view-images/sort01.png#lightbox)

Enregistrez vos modifications et revenir à Visual Studio pour Mac se synchroniser avec Xcode.

Maintenant nous allons modifier le `ProductOutlineDataSource.cs` fichier, puis ajoutez les méthodes suivantes :

```csharp
public void Sort(string key, bool ascending) {

    // Take action based on key
    switch (key) {
    case "Title":
        if (ascending) {
            Products.Sort ((x, y) => x.Title.CompareTo (y.Title));
        } else {
            Products.Sort ((x, y) => -1 * x.Title.CompareTo (y.Title));
        }
        break;
    }
}

public override void SortDescriptorsChanged (NSOutlineView outlineView, NSSortDescriptor[] oldDescriptors)
{
    // Sort the data
    Sort (oldDescriptors [0].Key, oldDescriptors [0].Ascending);
    outlineView.ReloadData ();
}
```

Le `Sort` méthode nous permettent de trier les données dans la Source de données basée sur une donnée `Product` champ de classe dans l’ordre croissant ou décroissant. Substituées `SortDescriptorsChanged` méthode sera appelée chaque fois que l’utilisation clique sur un en-tête de colonne. Il sera passé le **clé** valeur que nous avons défini dans le Générateur d’Interface et l’ordre de tri pour cette colonne.

Si nous pouvons exécuter l’application et que vous cliquez sur dans les en-têtes de colonnes, les lignes seront triées par cette colonne :

[![](outline-view-images/sort02.png "Exemple de sortie triée")](outline-view-images/sort02.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Sélection de ligne

Si vous souhaitez autoriser l’utilisateur à sélectionner une seule ligne, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour modification dans l’Interface Builder. Sélectionnez le mode plan dans le **hiérarchie des interfaces** et désactivez le **plusieurs** case à cocher dans la **inspecteur d’attributs**:

[![](outline-view-images/select01.png "L’inspecteur d’attributs")](outline-view-images/select01.png#lightbox)

Enregistrez vos modifications et revenir à Visual Studio pour Mac se synchroniser avec Xcode.


Ensuite, modifiez le `ProductOutlineDelegate.cs` fichier, puis ajoutez la méthode suivante :

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Cela permettra à l’utilisateur de sélectionner une seule ligne en mode plan. Retourner `false` pour le `ShouldSelectItem` pour des éléments que vous voulez l’utilisateur soit en mesure de sélectionner ou `false` pour chaque élément, si vous ne souhaitez pas l’utilisateur soit en mesure de sélectionner tous les éléments.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Sélection de plusieurs lignes

Si vous souhaitez autoriser l’utilisateur à sélectionner un plusieurs lignes, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour modification dans l’Interface Builder. Sélectionnez le mode plan dans le **hiérarchie des interfaces** et vérifiez le **plusieurs** case à cocher dans la **inspecteur d’attributs**:

[![](outline-view-images/select02.png "L’inspecteur d’attributs")](outline-view-images/select02.png#lightbox)

Enregistrez vos modifications et revenir à Visual Studio pour Mac se synchroniser avec Xcode.


Ensuite, modifiez le `ProductOutlineDelegate.cs` fichier, puis ajoutez la méthode suivante :

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Cela permettra à l’utilisateur de sélectionner une seule ligne en mode plan. Retourner `false` pour le `ShouldSelectRow` pour des éléments que vous voulez l’utilisateur soit en mesure de sélectionner ou `false` pour chaque élément, si vous ne souhaitez pas l’utilisateur soit en mesure de sélectionner tous les éléments.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Type à sélectionner la ligne

Si vous souhaitez autoriser l’utilisateur à taper un caractère avec le mode plan sélectionné et sélectionnez la première ligne qui a ce caractère, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour modification dans l’Interface Builder. Sélectionnez le mode plan dans le **hiérarchie des interfaces** et vérifiez le **Type Sélectionnez** case à cocher dans la **inspecteur d’attributs**:

[![](outline-view-images/type01.png "Modifier le type de ligne")](outline-view-images/type01.png#lightbox)

Enregistrez vos modifications et revenir à Visual Studio pour Mac se synchroniser avec Xcode.

Maintenant nous allons modifier le `ProductOutlineDelegate.cs` fichier, puis ajoutez la méthode suivante :

```csharp
public override NSObject GetNextTypeSelectMatch (NSOutlineView outlineView, NSObject startItem, NSObject endItem, string searchString)
{
    foreach(Product product in DataSource.Products) {
        if (product.Title.Contains (searchString)) {
            return product;
        }
    }

    // Not found
    return null;
}
```

Le `GetNextTypeSelectMatch` méthode prend le donné `searchString` et retourne l’élément de la première `Product` qui possède cette chaîne dans son `Title`.

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Réorganisation des colonnes

Si vous souhaitez autoriser l’utilisateur à faire glisser réorganiser les colonnes en mode plan, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour modification dans l’Interface Builder. Sélectionnez le mode plan dans le **hiérarchie des interfaces** et vérifiez le **réorganisation** case à cocher dans la **inspecteur d’attributs**:

[![](outline-view-images/reorder01.png "L’inspecteur d’attributs")](outline-view-images/reorder01.png#lightbox)

Si nous donner une valeur pour le **Autosave** propriété et vérifiez la **les informations de colonne** champ, les modifications que nous apportons à disposition de la table seront automatiquement enregistrées pour nous et restaurée la prochaine fois que l’application est exécuté.

Enregistrez vos modifications et revenir à Visual Studio pour Mac se synchroniser avec Xcode.

Maintenant nous allons modifier le `ProductOutlineDelegate.cs` fichier, puis ajoutez la méthode suivante :

```csharp
public override bool ShouldReorder (NSOutlineView outlineView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

Le `ShouldReorder` méthode doit retourner `true` pour n’importe quelle colonne il souhaite autoriser à être glisser réorganisés dans le `newColumnIndex`, sinon retour `false`;

Si nous exécutons l’application, nous pouvons faire glisser les en-têtes de colonnes pour réorganiser les colonnes :

[![](outline-view-images/reorder02.png "Exemple de réorganisation des colonnes")](outline-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Modification des cellules

Si vous souhaitez autoriser l’utilisateur à modifier les valeurs pour une cellule donnée, modifier le `ProductOutlineDelegate.cs` du fichier et changer la `GetViewForItem` méthode comme suit :

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTextField view = (NSTextField)outlineView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTextField ();
        view.Identifier = tableColumn.Title;
        view.BackgroundColor = NSColor.Clear;
        view.Bordered = false;
        view.Selectable = false;
        view.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.StringValue;
            break;
        case "Details":
            prod.Description = view.StringValue;
            break; 
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.StringValue = product.Title;
        break;
    case "Details":
        view.StringValue = product.Description;
        break;
    }

    return view;
}
```

Maintenant si nous exécutons l’application, l’utilisateur peut modifier les cellules dans la vue de Table :

[![](outline-view-images/editing01.png "Un exemple de modification des cellules")](outline-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Outline_Views" />

## <a name="using-images-in-outline-views"></a>L’utilisation d’Images dans des modes plan

Pour inclure une image dans le cadre de la cellule dans un `NSOutlineView`, vous devrez modifier la façon dont les données sont retournées par la vue hiérarchique `NSTableViewDelegate's` `GetView` méthode à utiliser un `NSTableCellView` au lieu du type `NSTextField`. Exemple :

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)outlineView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.TextField.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.TextField.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.TextField.StringValue;
            break;
        case "Details":
            prod.Description = view.TextField.StringValue;
            break; 
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed (product.IsProductGroup ? "tags.png" : "tag.png");
        view.TextField.StringValue = product.Title;
        break;
    case "Details":
        view.TextField.StringValue = product.Description;
        break;
    }

    return view;
}
```

Pour plus d’informations, consultez le [à l’aide d’Images des modes plan](~/mac/app-fundamentals/image.md) section de notre [fonctionne avec Image](~/mac/app-fundamentals/image.md) documentation.

<a name="Data_Binding_Outline_Views" />

## <a name="data-binding-outline-views"></a>Modes de plan de liaison de données

À l’aide de techniques de codage de clé-valeur et une liaison de données dans votre application Xamarin.Mac, vous pouvez considérablement réduire la quantité de code que vous devez écrire et maintenir pour remplir et utiliser des éléments d’interface utilisateur. Vous avez également l’avantage de découplage davantage vos données de sauvegarde (_modèle de données_) à partir de votre front end Interface d’utilisateur (_Model-View-Controller_), conduit à la plus facile à gérer, une application plus flexible conception.

Codage de clé-valeur (KVM) est un mécanisme permettant l’accès aux propriétés d’un objet indirectement, à l’aide de clés (spécialement mise en forme des chaînes) pour identifier les propriétés au lieu d’y accéder via des variables d’instance ou des méthodes d’accesseur (`get/set`). En implémentant la clé-valeur de codage des accesseurs conformes dans votre application Xamarin.Mac, vous avez accès à d’autres fonctionnalités macOS comme clé-valeur en observant (KVO), liaison de données, les données principales, Cocoa les liaisons et scriptability.

Pour plus d’informations, consultez le [une liaison de données hiérarchique vue](~/mac/app-fundamentals/databinding.md#Outline_View_Data_Binding) section de notre [une liaison de données et le codage de clé-valeur](~/mac/app-fundamentals/databinding.md) documentation.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a examiné en détail à l’utilisation des modes plan dans une application Xamarin.Mac. Nous avons vu les différents types et utilise des modes plan, comment créer et gérer des modes plan dans Xcode Interface Builder et l’utilisation des modes plan dans le code c#.

## <a name="related-links"></a>Liens associés

- [MacOutlines (sample)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [MacImages (exemple)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Vues Table](~/mac/user-interface/table-view.md)
- [Listes de sources](~/mac/user-interface/source-list.md)
- [Liaison de données et codage de clé-valeur](~/mac/app-fundamentals/databinding.md)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Introduction aux modes plan](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
