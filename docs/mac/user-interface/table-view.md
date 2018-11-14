---
title: Vues de table dans Xamarin.Mac
description: Cet article aborde l’utilisation des vues de table dans une application Xamarin.Mac. Il décrit la création de vues de table dans Xcode et Interface Builder et interagir avec elles dans le code.
ms.prod: xamarin
ms.assetid: 3B55B858-4769-4331-966A-7F53B3B7C720
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: c6d7971c8418c8038f5fcbfcf0e8f2acd928edcb
ms.sourcegitcommit: d09391c315336d36496880ef465a72b8974f2ac7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51579893"
---
# <a name="table-views-in-xamarinmac"></a>Vues de table dans Xamarin.Mac

_Cet article aborde l’utilisation des vues de table dans une application Xamarin.Mac. Il décrit la création de vues de table dans Xcode et Interface Builder et interagir avec elles dans le code._

Lorsque vous travaillez avec C# et .NET dans une application Xamarin.Mac, que vous avez accès à la même Table qui affiche un développeur travaillant *Objective-C* et *Xcode* est. Comme Xamarin.Mac s’intègre directement à Xcode, vous pouvez utiliser de Xcode _Interface Builder_ pour créer et gérer vos vues de Table (ou éventuellement pour les créer directement en code c#).

Une vue de Table affiche les données dans un format tabulaire qui contient une ou plusieurs colonnes d’informations dans plusieurs lignes. Selon le type de vue de Table en cours de création, l’utilisateur peut trier par colonne, réorganiser les colonnes, ajouter des colonnes, supprimer des colonnes ou modifier les données contenues dans la table.

[![](table-view-images/intro01.png "Un exemple de table")](table-view-images/intro01.png#lightbox)

Dans cet article, nous traiterons les principes fondamentaux de l’utilisation des vues de Table dans une application Xamarin.Mac. Il est fortement recommandé que vous travaillez via le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) et [Outlets et Actions](~/mac/get-started/hello-mac.md#outlets-and-actions) sections, tel qu’il couvre les principaux concepts et techniques que nous utilisons dans cet article.

Vous pouvez souhaiter de jeter un coup de œil à la [C# exposition de classes / méthodes vers Objective-C](~/mac/internals/how-it-works.md) section de la [éléments internes de Xamarin.Mac](~/mac/internals/how-it-works.md) de document, il explique le `Register` et `Export` commandes utilisé pour AutoEventWireup vos classes c# pour les objets Objective-C et les éléments d’interface utilisateur.

<a name="Introduction_to_Table_Views" />

## <a name="introduction-to-table-views"></a>Introduction aux vues de Table

Une vue de Table affiche les données dans un format tabulaire qui contient une ou plusieurs colonnes d’informations dans plusieurs lignes. Vues de table sont affichées à l’intérieur des vues à défilement (`NSScrollView`) et à partir de macOS 10.7, vous pouvez utiliser les `NSView` au lieu de cellules (`NSCell`) pour afficher les lignes et colonnes. Ceci dit, vous pouvez toujours utiliser `NSCell` Toutefois, vous allez généralement sous-classe `NSTableCellView` et créer vos lignes personnalisées et les colonnes.

Une vue de Table ne stocke pas ses propres données, au lieu de cela, elle s’appuie sur une Source de données (`NSTableViewDataSource`) pour fournir les lignes et les colonnes requises sur une en fonction des besoins.

Comportement d’une vue de la Table peut être personnalisé en fournissant une sous-classe du délégué de vue de Table (`NSTableViewDelegate`) pour prendre en charge la gestion de colonne de table, tapez pour sélectionner des fonctionnalités, la sélection de ligne modification, suivi personnalisé et des vues personnalisées pour des colonnes individuelles et lignes.

Lorsque vous créez des vues de Table, Apple suggère les éléments suivants :

* Autoriser l’utilisateur de trier la table en cliquant sur les en-têtes de colonne.
* Créer des en-têtes de colonnes qui sont des noms ou des expressions nominales courte qui décrivent les données affichées dans cette colonne.

Pour plus d’informations, consultez le [vues contenu](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) section d’Apple [X Human Interface Guidelines pour OS](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Creating-and-Maintaining-Table-Views-in-Xcode" />

## <a name="creating-and-maintaining-table-views-in-xcode"></a>Création et la gestion des vues de Table dans Xcode

Lorsque vous créez une nouvelle application Xamarin.Mac Cocoa, vous obtenez une fenêtre vide, standard par défaut. Ce paramètre, windows est défini dans un `.storyboard` fichier automatiquement inclus dans le projet. Pour modifier votre conception de windows, dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` fichier :

[![](table-view-images/edit01.png "En sélectionnant le storyboard principal")](table-view-images/edit01.png#lightbox)

La conception de la fenêtre s’ouvre dans Xcode Interface Builder :

[![](table-view-images/edit02.png "Modification de l’interface utilisateur dans Xcode")](table-view-images/edit02.png#lightbox)

Type `table` dans le **l’inspecteur de bibliothèque** zone de recherche pour le rendre plus facile à trouver les contrôles d’affichage de Table :

[![](table-view-images/edit03.png "Sélectionnez une vue de Table dans la bibliothèque")](table-view-images/edit03.png#lightbox)

Faites glisser d’une vue de Table sur le contrôleur d’affichage dans le **Éditeur d’Interface**, rendent la remplir la zone de contenu du contrôleur d’affichage et affectez-lui la valeur où il augmente et diminue avec la fenêtre dans le **éditeur de contrainte**:

[![](table-view-images/edit04.png "Modification des contraintes")](table-view-images/edit04.png#lightbox)

Sélectionnez la vue de Table dans le **hiérarchie des interfaces** et les propriétés suivantes sont disponibles dans le **inspecteur d’attributs**:

[![](table-view-images/edit05.png "L’inspecteur d’attributs")](table-view-images/edit05.png#lightbox)

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
> Sauf si vous mettez à jour une application Xamarin.Mac héritée, `NSView` en affichages de tableau doit être utilisés sur `NSCell` en fonction des vues de Table. `NSCell` est considéré comme hérité et de ne pas être gérée à l’avenir.

Sélectionnez une colonne de Table dans le **hiérarchie des interfaces** et les propriétés suivantes sont disponibles dans le **inspecteur d’attributs**:

[![](table-view-images/edit06.png "L’inspecteur d’attributs")](table-view-images/edit06.png#lightbox)

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

[![](table-view-images/edit07.png "L’inspecteur d’attributs")](table-view-images/edit07.png#lightbox)

Il s’agit de toutes les propriétés d’une vue standard. Vous avez également la possibilité de redimensionner les lignes pour cette colonne ici.

Sélectionnez une cellule d’affichage Table (par défaut, il s’agit d’un `NSTextField`) dans le **hiérarchie des interfaces** et les propriétés suivantes sont disponibles dans le **inspecteur d’attributs**:

[![](table-view-images/edit08.png "L’inspecteur d’attributs")](table-view-images/edit08.png#lightbox)

Vous aurez toutes les propriétés d’un champ de texte standard pour définir ici. Par défaut, un champ de texte standard est utilisé pour afficher les données d’une cellule dans une colonne.

Sélectionnez une vue de cellule de Table (`NSTableFieldCell`) dans le **hiérarchie des interfaces** et les propriétés suivantes sont disponibles dans le **inspecteur d’attributs**:

[![](table-view-images/edit09.png "L’inspecteur d’attributs")](table-view-images/edit09.png#lightbox)

Plus important ici sont les suivantes :

- **Disposition** - sélectionnez la façon dont les cellules de cette colonne sont disposés.
- **Utilise le Mode à ligne unique** : si `true`, la cellule est limitée à une seule ligne.
- **La largeur de première exécution disposition** : si `true`, la cellule préféreront la largeur définie pour elle (manuellement ou automatiquement) quand il est affiché à la première fois que l’application est exécutée.
- **Action** -contrôle le moment où la modification **Action** est envoyé pour la cellule.
- **Comportement** -définit si une cellule est sélectionnable ou modifiable.
- **Texte riche** : si `true`, la cellule peut afficher du texte mis en forme et de style.
- **Annuler** : si `true`, la cellule assume la responsabilité pour qu’il est annuler le comportement.

Sélectionnez la vue de cellule de Table (`NSTableFieldCell`) en bas d’une colonne de Table dans le **hiérarchie des interfaces**:

[![](table-view-images/edit10.png "En sélectionnant la vue de cellule de Table")](table-view-images/edit10.png#lightbox)

Cela vous permet de modifier la vue de cellule de Table utilisée comme base _modèle_ pour toutes les cellules créés pour la colonne donnée.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Ajoute des Actions et Outlets

Juste comme tout autre contrôle de l’interface utilisateur Cocoa, nous devons exposer notre Table de vue et il est de colonnes et des cellules à c# code à l’aide de **Actions** et **Outlets** (basé sur les fonctionnalités requises).

Le processus est identique pour n’importe quel élément d’affichage de la Table que nous souhaitons exposer :

1. Basculez vers le **éditeur de l’Assistant** et vérifiez que le `ViewController.h` fichier est sélectionné : 

    [![](table-view-images/edit11.png "L’éditeur de l’Assistant")](table-view-images/edit11.png#lightbox)
2. Sélectionnez la vue de Table dans le **hiérarchie des interfaces**, cliquez et faites glisser vers le `ViewController.h` fichier.
3. Créer un **Outlet** pour la vue de Table appelée `ProductTable`: 

    [![](table-view-images/edit13.png "Configuration d’un Outlet")](table-view-images/edit13.png#lightbox)
4. Créer **Outlets** pour les colonnes de tables aussi appelé `ProductColumn` et `DetailsColumn`: 

    [![](table-view-images/edit14.png "Configuration d’un Outlet")](table-view-images/edit14.png#lightbox)
5. Enregistrer les modifications et revenir à Visual Studio pour Mac se synchroniser avec Xcode.

Nous allons ensuite écrire à l’affichage de code des données pour la table lors de l’application est exécutée.

<a name="Populating_the_Table_View" />

## <a name="populating-the-table-view"></a>Remplissage de l’affichage de la Table

Avec notre vue Table conçue dans Interface Builder et exposées via un **Outlet**, nous devons ensuite créer le code c# pour le remplir.

Tout d’abord, nous allons créer un nouveau `Product` classe pour contenir les informations pour les lignes individuelles. Dans le **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **nouveau fichier...** Sélectionnez **général** > **classe vide**, entrez `Product` pour le **nom** et cliquez sur le **New** bouton :

[![](table-view-images/populate01.png "Création d’une classe vide")](table-view-images/populate01.png#lightbox)

Rendre le `Product.cs` fichier ressemble ce qui suit :

```csharp
using System;

namespace MacTables
{
    public class Product
    {
        #region Computed Properties
        public string Title { get; set;} = "";
        public string Description { get; set;} = "";
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

Ensuite, nous devons créer une sous-classe de `NSTableDataSource` pour fournir les données pour notre table comme il est demandé. Dans le **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **nouveau fichier...** Sélectionnez **général** > **classe vide**, entrez `ProductTableDataSource` pour le **nom** et cliquez sur le **New** bouton.

Modifier la `ProductTableDataSource.cs` de fichier et qu’elle ressemble à ce qui suit :

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacTables
{
    public class ProductTableDataSource : NSTableViewDataSource
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Constructors
        public ProductTableDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override nint GetRowCount (NSTableView tableView)
        {
            return Products.Count;
        }
        #endregion
    }
}

```

Cette classe dispose d’un stockage pour les éléments de notre vue de la Table et remplace le `GetRowCount` pour retourner le nombre de lignes dans la table.

Enfin, nous devons créer une sous-classe de `NSTableDelegate` pour fournir le comportement de notre table. Dans le **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **nouveau fichier...** Sélectionnez **général** > **classe vide**, entrez `ProductTableDelegate` pour le **nom** et cliquez sur le **New** bouton.

Modifier la `ProductTableDelegate.cs` de fichier et qu’elle ressemble à ce qui suit :

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacTables
{
    public class ProductTableDelegate: NSTableViewDelegate
    {
        #region Constants 
        private const string CellIdentifier = "ProdCell";
        #endregion

        #region Private Variables
        private ProductTableDataSource DataSource;
        #endregion

        #region Constructors
        public ProductTableDelegate (ProductTableDataSource datasource)
        {
            this.DataSource = datasource;
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)tableView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Product":
                view.StringValue = DataSource.Products [(int)row].Title;
                break;
            case "Details":
                view.StringValue = DataSource.Products [(int)row].Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

Lorsque nous créons une instance de la `ProductTableDelegate`, nous passons également dans une instance de la `ProductTableDataSource` qui fournit les données pour la table. Le `GetViewForItem` méthode est chargée de retourner une vue (données) pour afficher la cellule pour une colonne donnée et une ligne. Si possible, une vue existante sera réutilisée pour afficher la cellule, si ce n’est pas une nouvelle vue doit être créée.

Pour remplir la table, nous allons modifier le `ViewController.cs` et effectuez le `AwakeFromNib` méthode ressemble ce qui suit :

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create the Product Table Data Source and populate it
    var DataSource = new ProductTableDataSource ();
    DataSource.Products.Add (new Product ("Xamarin.iOS", "Allows you to develop native iOS Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Android", "Allows you to develop native Android Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Mac", "Allows you to develop Mac native Applications in C#"));

    // Populate the Product Table
    ProductTable.DataSource = DataSource;
    ProductTable.Delegate = new ProductTableDelegate (DataSource);
}
```

Si nous exécutons l’application, ce qui suit s’affiche :

[![](table-view-images/populate02.png "Exécution d’une application exemple")](table-view-images/populate02.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Tri par colonne

Nous allons autoriser l’utilisateur de trier les données dans la table en cliquant sur un en-tête de colonne. Tout d’abord, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour modification dans l’Interface Builder. Sélectionnez le `Product` colonne, entrez `Title` pour le **clé de tri**, `compare:` pour le **sélecteur** et sélectionnez `Ascending` pour le **ordre**:

[![](table-view-images/sort01.png "Définition de la clé de tri")](table-view-images/sort01.png#lightbox)

Sélectionnez le `Details` colonne, entrez `Description` pour le **clé de tri**, `compare:` pour le **sélecteur** et sélectionnez `Ascending` pour le **ordre**:

[![](table-view-images/sort02.png "Définition de la clé de tri")](table-view-images/sort02.png#lightbox)

Enregistrez vos modifications et revenir à Visual Studio pour Mac se synchroniser avec Xcode.

Maintenant nous allons modifier le `ProductTableDataSource.cs` fichier, puis ajoutez les méthodes suivantes :

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
    case "Description":
        if (ascending) {
            Products.Sort ((x, y) => x.Description.CompareTo (y.Description));
        } else {
            Products.Sort ((x, y) => -1 * x.Description.CompareTo (y.Description));
        }
        break;
    }

}

public override void SortDescriptorsChanged (NSTableView tableView, NSSortDescriptor[] oldDescriptors)
{
    // Sort the data
    if (oldDescriptors.Length > 0) {
        // Update sort
        Sort (oldDescriptors [0].Key, oldDescriptors [0].Ascending);
    } else {
        // Grab current descriptors and update sort
        NSSortDescriptor[] tbSort = tableView.SortDescriptors; 
        Sort (tbSort[0].Key, tbSort[0].Ascending); 
    }
            
    // Refresh table
    tableView.ReloadData ();
}
```

Le `Sort` méthode nous permettent de trier les données dans la Source de données basée sur une donnée `Product` champ de classe dans l’ordre croissant ou décroissant. Substituées `SortDescriptorsChanged` méthode sera appelée chaque fois que l’utilisation clique sur un en-tête de colonne. Il sera passé le **clé** valeur que nous avons défini dans le Générateur d’Interface et l’ordre de tri pour cette colonne.

Si nous pouvons exécuter l’application et que vous cliquez sur dans les en-têtes de colonnes, les lignes seront triées par cette colonne :

[![](table-view-images/sort03.png "Exécution d’une application exemple")](table-view-images/sort03.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Sélection de ligne

Si vous souhaitez autoriser l’utilisateur à sélectionner une seule ligne, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour modification dans l’Interface Builder. Sélectionnez la vue de Table dans le **hiérarchie des interfaces** et désactivez le **plusieurs** case à cocher dans la **inspecteur d’attributs**:

[![](table-view-images/select01.png "L’inspecteur d’attributs")](table-view-images/select01.png#lightbox)

Enregistrez vos modifications et revenir à Visual Studio pour Mac se synchroniser avec Xcode.


Ensuite, modifiez le `ProductTableDelegate.cs` fichier, puis ajoutez la méthode suivante :

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

Cela permettra à l’utilisateur de sélectionner une seule ligne dans la vue de Table. Retourner `false` pour le `ShouldSelectRow` pour les lignes que vous voulez l’utilisateur soit en mesure de sélectionner ou `false` pour chaque ligne, si vous ne souhaitez pas l’utilisateur soit en mesure de sélectionner toutes les lignes.

La vue de Table (`NSTableView`) contient les méthodes suivantes pour travailler avec la sélection de ligne :

- `DeselectRow(nint)` -Désélectionne la ligne donnée dans la table.
- `SelectRow(nint,bool)` -Sélectionne la ligne donnée. Passer `false` pour le deuxième paramètre ne sélectionner qu’une seule ligne à la fois.
- `SelectedRow` -Retourne la ligne actuelle sélectionnée dans la table.
- `IsRowSelected(nint)` -Retourne `true` si la ligne donnée est sélectionnée.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Sélection de plusieurs lignes

Si vous souhaitez autoriser l’utilisateur à sélectionner un plusieurs lignes, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour modification dans l’Interface Builder. Sélectionnez la vue de Table dans le **hiérarchie des interfaces** et vérifiez le **plusieurs** case à cocher dans la **inspecteur d’attributs**:

[![](table-view-images/select02.png "L’inspecteur d’attributs")](table-view-images/select02.png#lightbox)

Enregistrez vos modifications et revenir à Visual Studio pour Mac se synchroniser avec Xcode.


Ensuite, modifiez le `ProductTableDelegate.cs` fichier, puis ajoutez la méthode suivante :

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

Cela permettra à l’utilisateur de sélectionner une seule ligne dans la vue de Table. Retourner `false` pour le `ShouldSelectRow` pour les lignes que vous voulez l’utilisateur soit en mesure de sélectionner ou `false` pour chaque ligne, si vous ne souhaitez pas l’utilisateur soit en mesure de sélectionner toutes les lignes.

La vue de Table (`NSTableView`) contient les méthodes suivantes pour travailler avec la sélection de ligne :

- `DeselectAll(NSObject)` -Désélectionne toutes les lignes de la table. Utilisez `this` pour le premier paramètre à envoyer dans l’objet qui effectue la sélection. 
- `DeselectRow(nint)` -Désélectionne la ligne donnée dans la table.
- `SelectAll(NSobject)` : Sélectionne toutes les lignes dans la table. Utilisez `this` pour le premier paramètre à envoyer dans l’objet qui effectue la sélection.
- `SelectRow(nint,bool)` -Sélectionne la ligne donnée. Passer `false` pour le deuxième paramètre effacer la sélection et sélectionnez une seule ligne, transmettre `true` pour étendre la sélection et inclure cette ligne.
- `SelectRows(NSIndexSet,bool)` -Sélectionne l’ensemble de lignes donné. Passer `false` pour le deuxième paramètre effacer la sélection, puis sélectionnez uniquement ces lignes, passer `true` pour étendre la sélection et inclure ces lignes.
- `SelectedRow` -Retourne la ligne actuelle sélectionnée dans la table.
- `SelectedRows` -Retourne un `NSIndexSet` contenant les index des lignes sélectionnées.
- `SelectedRowCount` -Retourne le nombre de lignes sélectionnées.
- `IsRowSelected(nint)` -Retourne `true` si la ligne donnée est sélectionnée.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Type à sélectionner la ligne

Si vous souhaitez autoriser l’utilisateur à taper un caractère avec la vue de la Table sélectionnée et sélectionnez la première ligne qui a ce caractère, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour modification dans l’Interface Builder. Sélectionnez la vue de Table dans le **hiérarchie des interfaces** et vérifiez le **Type Sélectionnez** case à cocher dans la **inspecteur d’attributs**:

[![](table-view-images/type01.png "Définition du type de sélection")](table-view-images/type01.png#lightbox)

Enregistrez vos modifications et revenir à Visual Studio pour Mac se synchroniser avec Xcode.

Maintenant nous allons modifier le `ProductTableDelegate.cs` fichier, puis ajoutez la méthode suivante :

```csharp
public override nint GetNextTypeSelectMatch (NSTableView tableView, nint startRow, nint endRow, string searchString)
{
    nint row = 0;
    foreach(Product product in DataSource.Products) {
        if (product.Title.Contains(searchString)) return row;

        // Increment row counter
        ++row;
    }

    // If not found select the first row
    return 0;
}
```

Le `GetNextTypeSelectMatch` méthode prend le donné `searchString` et retourne la ligne de la première `Product` qui possède cette chaîne dans son `Title`.

Si nous pouvons exécuter l’application et que vous tapez un caractère, une ligne est sélectionnée :

[![](table-view-images/type02.png "Exécution d’une application exemple")](table-view-images/type02.png#lightbox)

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Réorganisation des colonnes

Si vous souhaitez autoriser l’utilisateur à faire glisser réorganiser les colonnes dans la vue de Table, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour modification dans l’Interface Builder. Sélectionnez la vue de Table dans le **hiérarchie des interfaces** et vérifiez le **réorganisation** case à cocher dans la **inspecteur d’attributs**:

[![](table-view-images/reorder01.png "L’inspecteur d’attributs")](table-view-images/reorder01.png#lightbox)

Si nous donner une valeur pour le **Autosave** propriété et vérifiez la **les informations de colonne** champ, les modifications que nous apportons à disposition de la table seront automatiquement enregistrées pour nous et restaurée la prochaine fois que l’application est exécuté.

Enregistrez vos modifications et revenir à Visual Studio pour Mac se synchroniser avec Xcode.

Maintenant nous allons modifier le `ProductTableDelegate.cs` fichier, puis ajoutez la méthode suivante :

```csharp
public override bool ShouldReorder (NSTableView tableView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

Le `ShouldReorder` méthode doit retourner `true` pour n’importe quelle colonne il souhaite autoriser à être glisser réorganisés dans le `newColumnIndex`, sinon retour `false`;

Si nous exécutons l’application, nous pouvons faire glisser les en-têtes de colonnes pour réorganiser les colonnes :

[![](table-view-images/reorder02.png "Un exemple des colonnes permutées")](table-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Modification des cellules

Si vous souhaitez autoriser l’utilisateur à modifier les valeurs pour une cellule donnée, modifier le `ProductTableDelegate.cs` du fichier et changer la `GetViewForItem` méthode comme suit :

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{
    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTextField view = (NSTextField)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTextField ();
        view.Identifier = tableColumn.Title;
        view.BackgroundColor = NSColor.Clear;
        view.Bordered = false;
        view.Selectable = false;
        view.Editable = true;

        view.EditingEnded += (sender, e) => {
                    
            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.Tag].Title = view.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.Tag].Description = view.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

Maintenant si nous exécutons l’application, l’utilisateur peut modifier les cellules dans la vue de Table :

[![](table-view-images/editing01.png "Un exemple de modification d’une cellule")](table-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Table_Views" />

## <a name="using-images-in-table-views"></a>L’utilisation d’Images dans les vues de Table

Pour inclure une image dans le cadre de la cellule dans un `NSTableView`, vous devrez modifier la façon dont les données sont retournées par la vue Table `NSTableViewDelegate's` `GetViewForItem` méthode à utiliser un `NSTableCellView` au lieu du type `NSTextField`. Exemple :

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
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
        view.TextField.Editable = true;

        view.TextField.EditingEnded += (sender, e) => {

            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.TextField.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tags.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

Pour plus d’informations, consultez le [à l’aide d’Images avec des vues Table](~/mac/app-fundamentals/image.md) section de notre [fonctionne avec Image](~/mac/app-fundamentals/image.md) documentation.

<a name="Adding-a-Delete-Button-to-a-Row" />

## <a name="adding-a-delete-button-to-a-row"></a>Ajout d’un bouton Supprimer à une ligne

En fonction des exigences de votre application, il peut arriver dans lequel vous devez fournir un bouton d’action pour chaque ligne dans la table. Un exemple de cela, nous allons développer l’exemple de vue de la Table créée ci-dessus pour inclure un **supprimer** bouton sur chaque ligne.

Tout d’abord, modifiez le `Main.storyboard` dans Xcode Interface Builder, sélectionnez la vue de Table et augmenter le nombre de colonnes à trois (3). Ensuite, modifiez le **titre** de la nouvelle colonne à `Action`:

[![](table-view-images/delete01.png "Modification du nom de colonne")](table-view-images/delete01.png#lightbox)

Enregistrez les modifications dans la table de montage séquentiel et revenir à Visual Studio pour Mac synchroniser les modifications.

Ensuite, modifiez le `ViewController.cs` fichier, puis ajoutez la méthode publique suivante :

```csharp
public void ReloadTable ()
{
    ProductTable.ReloadData ();
}
```

Dans le même fichier, modifier la création du nouveau délégué de vue de Table à l’intérieur de `ViewDidLoad` méthode comme suit :

```csharp
// Populate the Product Table
ProductTable.DataSource = DataSource;
ProductTable.Delegate = new ProductTableDelegate (this, DataSource);
```

À présent, modifiez le `ProductTableDelegate.cs` fichier à inclure une connexion privée avec le contrôleur d’affichage et à mettre le contrôleur en tant que paramètre lors de la création d’une nouvelle instance du délégué :

```csharp
#region Private Variables
private ProductTableDataSource DataSource;
private ViewController Controller;
#endregion

#region Constructors
public ProductTableDelegate (ViewController controller, ProductTableDataSource datasource)
{
    this.Controller = controller;
    this.DataSource = datasource;
}
#endregion
```

Ensuite, ajoutez la nouvelle méthode privée suivante à la classe :

```csharp
private void ConfigureTextField (NSTableCellView view, nint row)
{
    // Add to view
    view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
    view.AddSubview (view.TextField);

    // Configure
    view.TextField.BackgroundColor = NSColor.Clear;
    view.TextField.Bordered = false;
    view.TextField.Selectable = false;
    view.TextField.Editable = true;

    // Wireup events
    view.TextField.EditingEnded += (sender, e) => {

        // Take action based on type
        switch (view.Identifier) {
        case "Product":
            DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
            break;
        case "Details":
            DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
            break;
        }
    };

    // Tag view
    view.TextField.Tag = row;
}
```

Cela prend toutes les configurations d’affichage de texte qui ont été précédemment effectuées dans le `GetViewForItem` (méthode) et les place dans un emplacement unique, pouvant être appelé (étant donné que la dernière colonne de la table n’inclut pas un affichage de texte, mais un bouton).

Enfin, modifiez le `GetViewForItem` (méthode) et qu’elle ressemble à ce qui suit :

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();

        // Configure the view
        view.Identifier = tableColumn.Title;

        // Take action based on title
        switch (tableColumn.Title) {
        case "Product":
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
            ConfigureTextField (view, row);
            break;
        case "Details":
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
            ConfigureTextField (view, row);
            break;
        case "Action":
            // Create new button
            var button = new NSButton (new CGRect (0, 0, 81, 16));
            button.SetButtonType (NSButtonType.MomentaryPushIn);
            button.Title = "Delete";
            button.Tag = row;

            // Wireup events
            button.Activated += (sender, e) => {
                // Get button and product
                var btn = sender as NSButton;
                var product = DataSource.Products [(int)btn.Tag];

                // Configure alert
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Informational,
                    InformativeText = $"Are you sure you want to delete {product.Title}? This operation cannot be undone.",
                    MessageText = $"Delete {product.Title}?",
                };
                alert.AddButton ("Cancel");
                alert.AddButton ("Delete");
                alert.BeginSheetForResponse (Controller.View.Window, (result) => {
                    // Should we delete the requested row?
                    if (result == 1001) {
                        // Remove the given row from the dataset
                        DataSource.Products.RemoveAt((int)btn.Tag);
                        Controller.ReloadTable ();
                    }
                });
            };

            // Add to view
            view.AddSubview (button);
            break;
        }

    }

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tag.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        view.TextField.Tag = row;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        view.TextField.Tag = row;
        break;
    case "Action":
        foreach (NSView subview in view.Subviews) {
            var btn = subview as NSButton;
            if (btn != null) {
                btn.Tag = row;
            }
        }
        break;
    }

    return view;
}
```

Nous allons examiner plusieurs sections de ce code plus en détail. Premièrement, si un nouveau `NSTableViewCell` est en cours de création action est pris en fonction du nom de la colonne. Pour les deux premières colonnes (**produit** et **détails**), la nouvelle `ConfigureTextField` méthode est appelée.

Pour le **Action** colonne, un nouveau `NSButton` est créé et ajouté à la cellule en tant que Sub vue :

```csharp
// Create new button
var button = new NSButton (new CGRect (0, 0, 81, 16));
button.SetButtonType (NSButtonType.MomentaryPushIn);
button.Title = "Delete";
button.Tag = row;
...

// Add to view
view.AddSubview (button);
```

Le bouton `Tag` propriété est utilisée pour contenir le nombre de la ligne qui est en cours de traitement. Ce nombre sera utilisé ultérieurement lorsque l’utilisateur demande une ligne à supprimer dans le bouton `Activated` événement :

```csharp
// Wireup events
button.Activated += (sender, e) => {
    // Get button and product
    var btn = sender as NSButton;
    var product = DataSource.Products [(int)btn.Tag];

    // Configure alert
    var alert = new NSAlert () {
        AlertStyle = NSAlertStyle.Informational,
        InformativeText = $"Are you sure you want to delete {product.Title}? This operation cannot be undone.",
        MessageText = $"Delete {product.Title}?",
    };
    alert.AddButton ("Cancel");
    alert.AddButton ("Delete");
    alert.BeginSheetForResponse (Controller.View.Window, (result) => {
        // Should we delete the requested row?
        if (result == 1001) {
            // Remove the given row from the dataset
            DataSource.Products.RemoveAt((int)btn.Tag);
            Controller.ReloadTable ();
        }
    });
};
```

Lors du démarrage du Gestionnaire d’événements, nous obtenons le bouton et le produit qui se trouve sur la ligne de table donnée. Une alerte est ensuite présentée à l’utilisateur de confirmer la suppression de la ligne. Si l’utilisateur choisit de supprimer la ligne, la ligne donnée est supprimée de la Source de données et la table est rechargée :

```csharp
// Remove the given row from the dataset
DataSource.Products.RemoveAt((int)btn.Tag);
Controller.ReloadTable ();
```

Enfin, si la cellule d’affichage de tableau est réutilisée au lieu d’en cours de création nouveau, le code suivant configure basé sur la colonne en cours de traitement :

```csharp
// Setup view based on the column selected
switch (tableColumn.Title) {
case "Product":
    view.ImageView.Image = NSImage.ImageNamed ("tag.png");
    view.TextField.StringValue = DataSource.Products [(int)row].Title;
    view.TextField.Tag = row;
    break;
case "Details":
    view.TextField.StringValue = DataSource.Products [(int)row].Description;
    view.TextField.Tag = row;
    break;
case "Action":
    foreach (NSView subview in view.Subviews) {
        var btn = subview as NSButton;
        if (btn != null) {
            btn.Tag = row;
        }
    }
    break;
}

```

Pour le **Action** colonne, toutes les vues de Sub sont analysés jusqu'à ce que le `NSButton` est trouvé, il est `Tag` propriété est mise à jour pour faire pointer vers la ligne actuelle.

Avec ces modifications en place, lors de l’application est exécutée chaque ligne aura une **supprimer** bouton :

[![](table-view-images/delete02.png "La vue de table avec des boutons de suppression")](table-view-images/delete02.png#lightbox)

Lorsque l’utilisateur clique sur un **supprimer** bouton, une alerte s’affiche demandant de supprimer la ligne donnée :

[![](table-view-images/delete03.png "Une alerte de ligne delete")](table-view-images/delete03.png#lightbox)

Si l’utilisateur choisit de suppression, la ligne sera supprimée et la table est redessinée :

[![](table-view-images/delete04.png "La table une fois que la ligne est supprimée.")](table-view-images/delete04.png#lightbox)

<a name="Data_Binding_Table_Views" />

## <a name="data-binding-table-views"></a>Vues de Table de liaison de données

À l’aide de techniques de codage de clé-valeur et une liaison de données dans votre application Xamarin.Mac, vous pouvez considérablement réduire la quantité de code que vous devez écrire et maintenir pour remplir et utiliser des éléments d’interface utilisateur. Vous avez également l’avantage de découplage davantage vos données de sauvegarde (_modèle de données_) à partir de votre front end Interface d’utilisateur (_Model-View-Controller_), conduit à la plus facile à gérer, une application plus flexible conception.

Codage de clé-valeur (KVM) est un mécanisme permettant l’accès aux propriétés d’un objet indirectement, à l’aide de clés (spécialement mise en forme des chaînes) pour identifier les propriétés au lieu d’y accéder via des variables d’instance ou des méthodes d’accesseur (`get/set`). En implémentant la clé-valeur de codage des accesseurs conformes dans votre application Xamarin.Mac, vous avez accès à d’autres fonctionnalités macOS comme clé-valeur en observant (KVO), liaison de données, les données principales, Cocoa les liaisons et scriptability.

Pour plus d’informations, consultez le [la liaison de données de vue Table](~/mac/app-fundamentals/databinding.md#Table_View_Data_Binding) section de notre [une liaison de données et le codage de clé-valeur](~/mac/app-fundamentals/databinding.md) documentation.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a examiné en détail à l’utilisation des vues de Table dans une application Xamarin.Mac. Nous avons vu les différents types et utilise des vues de Table, comment créer et mettre à jour les vues de Table dans l’Interface Builder de Xcode et comment travailler avec des vues de Table dans le code c#.

## <a name="related-links"></a>Liens associés

- [MacTables (exemple)](https://developer.xamarin.com/samples/mac/MacTables/)
- [MacImages (exemple)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Modes Plan](~/mac/user-interface/outline-view.md)
- [Listes de sources](~/mac/user-interface/source-list.md)
- [Liaison de données et codage de clé-valeur](~/mac/app-fundamentals/databinding.md)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [NSTableView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSTableView_Class/index.html#//apple_ref/doc/uid/TP40004125)
- [NSTableViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSTableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008622)
- [NSTableViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSTableDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004178)
