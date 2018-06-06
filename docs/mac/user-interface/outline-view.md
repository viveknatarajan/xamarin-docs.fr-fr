---
title: Mode plan dans Xamarin.Mac
description: Cet article décrit l’utilisation des vues hiérarchiques dans une application Xamarin.Mac. Il décrit la création et gérez les vues hiérarchiques dans Xcode et Interface générateur et leur utilisation par programmation.
ms.prod: xamarin
ms.assetid: 043248EE-11DA-4E96-83A3-08824A4F2E01
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: a12eee5f473ffdc6a235faeb55c0a3d6754f4629
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792825"
---
# <a name="outline-views-in-xamarinmac"></a>Mode plan dans Xamarin.Mac

_Cet article décrit l’utilisation des vues hiérarchiques dans une application Xamarin.Mac. Il décrit la création et gérez les vues hiérarchiques dans Xcode et Interface générateur et leur utilisation par programmation._

Lorsque vous travaillez avec c# et .NET dans une application Xamarin.Mac, vous avez accès au même plan vues dont un développeur travaillant dans *Objective-C* et *Xcode* est. Xamarin.Mac s’intègre directement avec Xcode, vous pouvez utiliser de Xcode _Interface Générateur_ pour créer et gérer vos vues hiérarchiques (ou éventuellement de les créer directement dans le code C#).

Mode plan est un type de Table qui permet à l’utilisateur de développer ou réduire des lignes de données hiérarchiques. Comme une vue de Table, une vue hiérarchique affiche les données pour un ensemble d’éléments associés, avec des lignes représentant des éléments et des colonnes représentant les attributs de ces éléments. Contrairement à une vue de Table, dans une vue hiérarchique des éléments ne sont pas dans une liste plate, ils sont organisés dans une hiérarchie, tels que les fichiers et dossiers sur un disque dur.

[![](outline-view-images/populate03.png "Un exemple d’application exécuter")](outline-view-images/populate03.png#lightbox)

Dans cet article, nous aborderons les principes fondamentaux de l’utilisation des vues de plan dans une application Xamarin.Mac. Il est fortement recommandé que vous parcourez le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Générateur](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) et [prises et Actions](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sections, telle qu’elle couvre les principaux concepts et techniques que nous utiliserons dans cet article.

Vous souhaiterez peut-être examiner la [classes exposition c# / méthodes pour Objective-C](~/mac/internals/how-it-works.md) section de la [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) de document, elle explique la `Register` et `Export` commandes permet de câble des vos classes c# pour les objets Objective-C et les éléments d’interface utilisateur.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-outline-views"></a>Présentation de montrer les vues

Mode plan est un type de Table qui permet à l’utilisateur de développer ou réduire des lignes de données hiérarchiques. Comme une vue de Table, une vue hiérarchique affiche les données pour un ensemble d’éléments associés, avec des lignes représentant des éléments et des colonnes représentant les attributs de ces éléments. Contrairement à une vue de Table, dans une vue hiérarchique des éléments ne sont pas dans une liste plate, ils sont organisés dans une hiérarchie, tels que les fichiers et dossiers sur un disque dur.

Si un élément dans une vue hiérarchique contient d’autres éléments, peuvent être développé ou réduit par l’utilisateur. Un élément peut être développé affiche un triangle, qui pointe vers la droite lorsque l’élément est réduit et qu’il pointe vers le bas lorsque l’élément est développé. En cliquant sur le triangle de provoque l’élément développer ou réduire.

Le mode plan (`NSOutlineView`) est une sous-classe de la vue de Table (`NSTableView`) et par conséquent, beaucoup de son comportement hérite de sa classe parente. Par conséquent, de nombreuses opérations prises en charge par une vue de Table, telles que la sélection des lignes ou colonnes, le repositionnement des colonnes en faisant glisser les en-têtes de colonne, etc., sont également pris en charge un mode plan. Une application Xamarin.Mac possède le contrôle de ces fonctionnalités et pouvez configurer les paramètres du mode plan (soit dans le code ou le constructeur d’Interface) pour autoriser ou interdire certaines opérations.

Mode plan ne stocke pas ses propres données, au lieu de cela, elle s’appuie sur une Source de données (`NSOutlineViewDataSource`) pour fournir les lignes et colonnes nécessaires sur un en fonction des besoins.

Comportement de la vue d’un plan peut être personnalisé en fournissant une sous-classe du délégué vue hiérarchique (`NSOutlineViewDelegate`) pour prendre en charge la gestion de colonne de structure, tapez pour sélectionner des fonctionnalités, la sélection de ligne, la modification, suivi personnalisé et des vues personnalisées de chaque les colonnes et lignes.

Étant donné que le mode plan de son comportement et les fonctionnalités partage avec une vue de Table, vous pouvez souhaiter passer par notre [tableaux](~/mac/user-interface/table-view.md) documentation avant de continuer avec cet article.

<a name="Creating_and_Maintaining_Outline_Views_in_Xcode" />

## <a name="creating-and-maintaining-outline-views-in-xcode"></a>Création et la maintenance des vues hiérarchiques dans Xcode

Lorsque vous créez une nouvelle application/Cocoa de Xamarin.Mac, vous obtenez une fenêtre vide, standard par défaut. Ce paramètre, windows est défini dans un `.storyboard` fichier inclus automatiquement dans le projet. Pour modifier votre conception de windows, dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` fichier :

[![](outline-view-images/edit01.png "En sélectionnant le storyboard principal")](outline-view-images/edit01.png#lightbox)

La conception de la fenêtre s’ouvre dans le Générateur de Xcode Interface :

[![](outline-view-images/edit02.png "Modification de l’interface utilisateur dans Xcode")](outline-view-images/edit02.png#lightbox)

Type `outline` dans les **l’inspecteur de bibliothèque** zone de recherche pour le rendre plus facile à trouver les contrôles du mode plan :

[![](outline-view-images/edit03.png "Sélection d’un mode plan à partir de la bibliothèque")](outline-view-images/edit03.png#lightbox)

Faites glisser un mode plan sur le contrôleur de la vue dans le **éditeur de l’Interface**, rendre remplir la zone de contenu de la vue de contrôleur et affectez-lui la valeur où il augmente et diminue à la fenêtre de la **éditeur de contrainte**:

[![](outline-view-images/edit04.png "Modification des contraintes")](outline-view-images/edit04.png#lightbox)

Sélectionnez le mode plan dans le **hiérarchie de l’Interface** et les propriétés suivantes sont disponibles dans le **inspecteur de l’attribut**:

[![](outline-view-images/edit05.png "L’inspecteur de l’attribut")](outline-view-images/edit05.png#lightbox)

- **Montrer la colonne** -la colonne de Table dans laquelle les données hiérarchiques sont affichées.
- **Colonne de structure du AutoSave** - si `true`, la colonne de structure du sera automatiquement enregistrée et restaurée entre les exécutions de l’application.
- **Mise en retrait** -le montant à mettre en retrait les colonnes sous un élément développé.
- **Mise en retrait les cellules suit** - si `true`, seront en retrait la marque de mise en retrait ainsi que les cellules.
- **Enregistrement automatique des éléments de développé** - si `true`, l’état développé/réduit des éléments est automatiquement enregistré et restauré entre les exécutions de l’application.
- **Mode de contenu** -vous permet d’utiliser deux vues (`NSView`) ou les cellules (`NSCell`) pour afficher les données dans les lignes et colonnes. À compter de macOS 10.7, vous devez utiliser des vues.
- **Flotte regrouper des lignes** - si `true`, la vue de la Table doit dessiner les cellules comme s’ils sont flottants.
- **Colonnes** -définit le nombre de colonnes affichées.
- **En-têtes** - si `true`, les colonnes ont des en-têtes.
- **Réorganisation des** - si `true`, l’utilisateur sera en mesure de faire glisser réorganiser les colonnes dans la table.
- **Redimensionnement** - si `true`, l’utilisateur sera en mesure de faire glisser pour redimensionner les colonnes, les en-têtes de colonne.
- **Dimensionnement de colonnes** -contrôle la manière dont la table est automatiquement la taille des colonnes.
- **Mettez en surbrillance** -contrôle le type de mise en surbrillance de la table utilise lorsqu’une cellule est sélectionnée.
- **Remplacement des lignes** - si `true`jamais autres ligne aura une couleur d’arrière-plan.
- **Quadrillage horizontal** -sélectionne le type de bordure entre les cellules horizontalement.
- **Quadrillage vertical** -sélectionne le type de bordure dessinée verticalement entre les cellules.
- **Couleur de la grille** -définit la couleur de bordure de cellule.
- **Arrière-plan** -définit la couleur d’arrière-plan de cellule.
- **Sélection** -vous permettent de contrôler la façon dont l’utilisateur peut sélectionner des cellules dans la table en tant que :
    - **Plusieurs** - si `true`, l’utilisateur peut sélectionner plusieurs lignes et colonnes.
    - **Colonne** - si `true`, l’utilisateur peut sélectionner des colonnes.
    - **Tapez sélectionner** - si `true`, l’utilisateur peut taper un caractère pour sélectionner une ligne.
    - **Vide** - si `true`, l’utilisateur n’est pas nécessaire pour sélectionner une ligne ou colonne, la table autorise pour aucune sélection du tout.
- **Cette fonctionnalité** -le nom que le format des tables est automatiquement enregistrer sous.
- **Informations sur la colonne** - si `true`, l’ordre et la largeur des colonnes seront automatiquement enregistrés.
- **Les sauts de ligne** - sélectionnez la façon dont la cellule gère les sauts de ligne.
- **Tronque la dernière ligne Visible** - si `true`, la cellule sera tronquée dans le données peuvent ne tient ne pas à l’intérieur de ses limites.

> [!IMPORTANT]
> Sauf si vous mettez à jour une application héritée de Xamarin.Mac, `NSView` en mode plan doit être utilisé sur `NSCell` en fonction des vues de Table. `NSCell` est considéré comme hérités et ne pas être gérée à l’avenir.

Sélectionnez une colonne de Table dans le **hiérarchie de l’Interface** et les propriétés suivantes sont disponibles dans le **inspecteur de l’attribut**:

[![](outline-view-images/edit06.png "L’inspecteur de l’attribut")](outline-view-images/edit06.png#lightbox)

- **Titre** -définit le titre de la colonne.
- **Alignement** -définir l’alignement du texte dans les cellules.
- **Police de titre** -sélectionne la police pour le texte d’en-tête de la cellule.
- **Clé de tri** -est la clé utilisée pour trier les données dans la colonne. Laissez vide si l’utilisateur ne peut pas trier cette colonne.
- **Sélecteur** -est la **Action** utilisé pour effectuer le tri. Laissez vide si l’utilisateur ne peut pas trier cette colonne.
- **Commande** -est l’ordre de tri pour les données de colonnes.
- **Redimensionnement** -sélectionne le type de redimensionnement de la colonne.
- **Modifiable** - si `true`, l’utilisateur peut modifier les cellules d’un tableau de cellules en fonction.
- **Masqué** - si `true`, la colonne est masquée.

Vous pouvez également redimensionner la colonne en faisant glisser son handle (centré verticalement sur la droite de la colonne) à gauche ou à droite.

Nous allons sélectionner la chaque colonne dans notre Table de vue et donner la première colonne un **titre** de `Product` et la seconde `Details`.

Sélectionnez un affichage de la cellule de tableau (`NSTableViewCell`) dans le **hiérarchie de l’Interface** et les propriétés suivantes sont disponibles dans le **inspecteur de l’attribut**:

[![](outline-view-images/edit07.png "L’inspecteur de l’attribut")](outline-view-images/edit07.png#lightbox)

Il s’agit de toutes les propriétés d’une vue standard. Vous avez également la possibilité de redimensionner les lignes de cette colonne ici.

Sélectionnez une cellule de la vue Table (par défaut, il s’agit d’un `NSTextField`) dans le **hiérarchie de l’Interface** et les propriétés suivantes sont disponibles dans le **inspecteur de l’attribut**:

[![](outline-view-images/edit08.png "L’inspecteur de l’attribut")](outline-view-images/edit08.png#lightbox)

Vous avez toutes les propriétés d’un champ de texte standard pour définir ici. Par défaut, un champ de texte standard est utilisé pour afficher les données d’une cellule dans une colonne.

Sélectionnez un affichage de la cellule de tableau (`NSTableFieldCell`) dans le **hiérarchie de l’Interface** et les propriétés suivantes sont disponibles dans le **inspecteur de l’attribut**:

[![](outline-view-images/edit09.png "L’inspecteur de l’attribut")](outline-view-images/edit09.png#lightbox)

La plus importante ici sont les suivantes :

- **Disposition** - sélectionnez la façon dont sont disposés de cellules de cette colonne.
- **Utilise le Mode à ligne unique** - si `true`, la cellule est limitée à une seule ligne.
- **Largeur de disposition première exécution** - si `true`, la cellule préfèreront la largeur définie pour elle (manuellement ou automatiquement) lorsque elle est affichée lors de la première exécution de l’application.
- **Action** -contrôle le moment où la modification **Action** est envoyé pour la cellule.
- **Comportement** -définit si une cellule est sélectionnable ou modifiable.
- **Rich Text** - si `true`, la cellule peut afficher du texte mis en forme et de style.
- **Annuler** - si `true`, la cellule prend la responsabilité pour annuler le comportement.

Sélectionnez la vue de la cellule de Table (`NSTableFieldCell`) en bas d’une colonne de Table dans le **hiérarchie de l’Interface**:

[![](outline-view-images/edit11.png "Sélection de la vue de cellule de table")](outline-view-images/edit10.png#lightbox)

Cela vous permet de modifier la vue de cellule de Table utilisée comme base _modèle_ pour toutes les cellules sont créés pour la colonne spécifiée.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Ajout d’Actions et les points de vente

Juste comme tout autre contrôle de l’interface utilisateur/Cocoa, nous avons besoin exposer notre mode plan et il est de colonnes et des cellules à c# code à l’aide de **Actions** et **prises** (basé sur les fonctionnalités requises).

Le processus est le même pour tout élément de mode plan que vous souhaitez exposer :

1. Basculez vers le **Assistant éditeur** et vérifiez que le `ViewController.h` fichier est sélectionné : 

    [![](outline-view-images/edit11.png "En sélectionnant le fichier .h correct")](outline-view-images/edit11.png#lightbox)
2. Sélectionnez le mode plan à partir de la **hiérarchie de l’Interface**, la touche CTRL enfoncée, puis faites glisser vers le `ViewController.h` fichier.
3. Créer un **prise** pour le mode plan appelé `ProductOutline`: 

    [![](outline-view-images/edit13.png "Configuration d’une prise de courant")](outline-view-images/edit13.png#lightbox)
4. Créer **prises** pour les colonnes de tables aussi appelé `ProductColumn` et `DetailsColumn`: 

    [![](outline-view-images/edit14.png "Configuration d’une prise de courant")](outline-view-images/edit14.png#lightbox)
5. Enregistrer les modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.

Nous allons ensuite écrire à l’affichage du code des données pour le plan lors de l’application est exécutée.

<a name="Populating_the_Table_View" />

## <a name="populating-the-outline-view"></a>Remplir le mode plan

Avec le mode de notre plan conçu dans le Générateur de Interface et exposées via un **prise**, ensuite, nous avons besoin créer le code c# pour le remplir.

Tout d’abord, nous allons créer un nouveau `Product` classe pour contenir les informations pour les lignes individuelles et les groupes de produits de sub. Dans le **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **nouveau fichier...** Sélectionnez **général** > **classe vide**, entrez `Product` pour le **nom** et cliquez sur le **nouveau** bouton :

[![](outline-view-images/populate01.png "Création d’une classe vide")](outline-view-images/populate01.png#lightbox)

Rendre le `Product.cs` fichier ressemble les éléments suivants :

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

Ensuite, nous avons besoin créer une sous-classe de `NSOutlineDataSource` pour fournir les données pour notre plan comme il est demandé. Dans le **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **nouveau fichier...** Sélectionnez **général** > **classe vide**, entrez `ProductOutlineDataSource` pour le **nom** et cliquez sur le **nouveau** bouton.

Modifier la `ProductTableDataSource.cs` de fichier et le rendre l’aspect suivant :

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

Cette classe dispose de stockage pour les éléments de la vue de notre plan et remplace le `GetChildrenCount` pour retourner le nombre de lignes dans la table. Le `GetChild` retourne un élément parent ou enfant spécifique (comme requis par le mode plan) et le `ItemExpandable` définit l’élément spécifié comme un parent ou enfant.

Enfin, nous devons créer une sous-classe de `NSOutlineDelegate` pour fournir le comportement de notre plan. Dans le **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **nouveau fichier...** Sélectionnez **général** > **classe vide**, entrez `ProductOutlineDelegate` pour le **nom** et cliquez sur le **nouveau** bouton.

Modifier la `ProductOutlineDelegate.cs` de fichier et le rendre l’aspect suivant :

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

Lorsque nous créons une instance de la `ProductOutlineDelegate`, nous passons également dans une instance de la `ProductOutlineDataSource` qui fournit les données pour le plan. Le `GetView` méthode est chargée de retourner une vue (données) pour afficher la cellule pour une colonne donnée et une ligne. Si possible, une vue existante sera réutilisée pour afficher la cellule, si ce n’est pas une nouvelle vue doit être créée.

Pour remplir le plan, nous allons modifier le `MainWindow.cs` et effectuez le `AwakeFromNib` détaillée de la méthode comme suit :

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

Si nous exécutons l’application, le texte suivant s’affiche :

[![](outline-view-images/populate02.png "L’affichage réduit")](outline-view-images/populate02.png#lightbox)

Si nous développez un nœud en mode plan, il doit ressembler à ce qui suit :

[![](outline-view-images/populate03.png "La vue développée")](outline-view-images/populate03.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Tri par colonne

Nous allons permettre à l’utilisateur de trier les données dans le plan en cliquant sur un en-tête de colonne. Tout d’abord, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour le modifier dans le Générateur de l’Interface. Sélectionnez le `Product` colonne, entrez `Title` pour le **la clé de tri**, `compare:` pour le **sélecteur** et sélectionnez `Ascending` pour le **commande**:

[![](outline-view-images/sort01.png "Définition de la clé de tri")](outline-view-images/sort01.png#lightbox)

Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.

Maintenant nous allons modifier le `ProductOutlineDataSource.cs` et ajoutez les méthodes suivantes :

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

Le `Sort` méthode nous permet de trier les données dans la Source de données basée sur une donnée `Product` champ de classe dans l’ordre croissant ou décroissant. Le substituée `SortDescriptorsChanged` méthode sera appelée chaque fois que l’utilisation clique sur un en-tête de colonne. Il sera passé le **clé** valeur que nous avons défini dans le constructeur d’Interface et l’ordre de tri pour cette colonne.

Si nous exécuter l’application et cliquez sur les en-têtes de colonnes, les lignes sont triées par cette colonne :

[![](outline-view-images/sort02.png "Exemple de sortie triée")](outline-view-images/sort02.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Sélection de ligne

Si vous souhaitez autoriser l’utilisateur à sélectionner une seule ligne, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour le modifier dans le Générateur de l’Interface. Sélectionnez le mode plan dans le **hiérarchie de l’Interface** et désactivez la **plusieurs** case à cocher dans la **inspecteur de l’attribut**:

[![](outline-view-images/select01.png "L’inspecteur de l’attribut")](outline-view-images/select01.png#lightbox)

Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.


Modifiez ensuite le `ProductOutlineDelegate.cs` et ajoutez la méthode suivante :

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Ainsi, l’utilisateur de sélectionner une seule ligne en mode plan. Retourner `false` pour le `ShouldSelectItem` pour des éléments que vous ne souhaitez pas l’utilisateur à sélectionner ou `false` pour chaque élément, si vous ne souhaitez pas l’utilisateur de sélectionner tous les éléments.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Sélection de plusieurs lignes

Si vous souhaitez autoriser l’utilisateur à sélectionner un plusieurs lignes, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour le modifier dans le Générateur de l’Interface. Sélectionnez le mode plan dans le **hiérarchie de l’Interface** et vérifiez la **plusieurs** case à cocher dans la **inspecteur de l’attribut**:

[![](outline-view-images/select02.png "L’inspecteur de l’attribut")](outline-view-images/select02.png#lightbox)

Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.


Modifiez ensuite le `ProductOutlineDelegate.cs` et ajoutez la méthode suivante :

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Ainsi, l’utilisateur de sélectionner une seule ligne en mode plan. Retourner `false` pour le `ShouldSelectRow` pour des éléments que vous ne souhaitez pas l’utilisateur à sélectionner ou `false` pour chaque élément, si vous ne souhaitez pas l’utilisateur de sélectionner tous les éléments.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Type pour sélectionner la ligne

Si vous souhaitez autoriser l’utilisateur à taper un caractère avec le mode plan sélectionné et sélectionnez la première ligne qui a ce caractère, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour le modifier dans le Générateur de l’Interface. Sélectionnez le mode plan dans le **hiérarchie de l’Interface** et vérifiez la **Type Sélectionnez** case à cocher dans la **inspecteur de l’attribut**:

[![](outline-view-images/type01.png "Modifier le type de ligne")](outline-view-images/type01.png#lightbox)

Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.

Maintenant nous allons modifier le `ProductOutlineDelegate.cs` et ajoutez la méthode suivante :

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

Le `GetNextTypeSelectMatch` méthode prend le donné `searchString` et retourne l’élément de la première `Product` qui possède cette chaîne dans ses `Title`.

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Réorganisation des colonnes

Si vous souhaitez autoriser l’utilisateur à faire glisser réorganiser les colonnes en mode plan, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour le modifier dans le Générateur de l’Interface. Sélectionnez le mode plan dans le **hiérarchie de l’Interface** et vérifiez la **réorganisation des** case à cocher dans la **inspecteur de l’attribut**:

[![](outline-view-images/reorder01.png "L’inspecteur de l’attribut")](outline-view-images/reorder01.png#lightbox)

Si nous donner une valeur pour le **enregistrement automatique** propriété et la vérification de la **les informations de colonne** champ, toutes les modifications apportées à la disposition de la table seront automatiquement enregistrées pour nous et restauré la prochaine fois que l’application est exécuté.

Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.

Maintenant nous allons modifier le `ProductOutlineDelegate.cs` et ajoutez la méthode suivante :

```csharp
public override bool ShouldReorder (NSOutlineView outlineView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

Le `ShouldReorder` méthode doit retourner `true` pour les colonnes que vous voulez autoriser à être glisser réorganisées dans la `newColumnIndex`, sinon retour `false`;

Si nous exécutons l’application, nous pouvons faire glisser autour des en-têtes de colonne pour réorganiser les colonnes :

[![](outline-view-images/reorder02.png "Exemple de réorganisation des colonnes")](outline-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Modification des cellules

Si vous souhaitez autoriser l’utilisateur à modifier les valeurs d’une cellule donnée, modifiez le `ProductOutlineDelegate.cs` de fichiers et de modifier le `GetViewForItem` méthode comme suit :

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

## <a name="using-images-in-outline-views"></a>L’utilisation d’Images dans les vues de plan

Pour inclure une image dans le cadre de la cellule dans une `NSOutlineView`, vous devez modifier la façon dont les données sont retournées par la vue de structure du `NSTableViewDelegate's` `GetView` méthode à utiliser un `NSTableCellView` au lieu du type `NSTextField`. Exemple :

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

Pour plus d’informations, consultez la [à l’aide des Images avec des vues hiérarchiques](~/mac/app-fundamentals/image.md) section de notre [utilisation de l’Image](~/mac/app-fundamentals/image.md) documentation.

<a name="Data_Binding_Outline_Views" />

## <a name="data-binding-outline-views"></a>Vues de plan de liaison de données

À l’aide de techniques de codage de la clé-valeur et la liaison de données dans votre application Xamarin.Mac, vous pouvez réduire considérablement la quantité de code que vous devez écrire et conserver pour remplir et utiliser des éléments d’interface utilisateur. Vous avez également l’avantage de découplage davantage de vos données de sauvegarde (_modèle de données_) à partir de votre premier plan mettre fin à l’Interface utilisateur (_Model-View-Controller_), début au plus facile à gérer, une application plus souple conception.

Clé-valeur de codage (KVM) est un mécanisme pour accéder aux propriétés d’un objet indirectement, à l’aide de clés (spécialement mise en forme des chaînes) pour identifier les propriétés au lieu d’y accéder via des variables d’instance ou des méthodes d’accesseur (`get/set`). En implémentant la clé-valeur de codage des accesseurs conformes dans votre application Xamarin.Mac, pour accéder à d’autres fonctionnalités macOS telles que la clé-valeur en observant (KVO), la liaison de données, données de base, les liaisons/Cocoa et scriptability.

Pour plus d’informations, consultez la [une liaison de données hiérarchique vue](~/mac/app-fundamentals/databinding.md#Outline_View_Data_Binding) section de notre [une liaison de données et le codage de la clé-valeur](~/mac/app-fundamentals/databinding.md) documentation.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a pris une présentation détaillée sur l’utilisation des vues de plan dans une application Xamarin.Mac. Nous avons vu les différents types et utilise des modes plan, comment créer et gérer de mode plan dans le Générateur de Xcode Interface et l’utilisation des vues de la structure du code c#.

## <a name="related-links"></a>Liens associés

- [MacOutlines (exemple)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [MacImages (exemple)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Vues Table](~/mac/user-interface/table-view.md)
- [Listes de sources](~/mac/user-interface/source-list.md)
- [Liaison de données et codage de clé-valeur](~/mac/app-fundamentals/databinding.md)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Présentation de montrer les vues](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
