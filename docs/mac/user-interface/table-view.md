---
title: Vues de table
description: Cet article décrit l’utilisation des vues de table dans une application Xamarin.Mac. Il décrit la création de vues de table dans Xcode Interface Builder et interagir avec elles dans le code.
ms.prod: xamarin
ms.assetid: 3B55B858-4769-4331-966A-7F53B3B7C720
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: c274405613f079cb61ad9c96497a9effdc7173f5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="table-views"></a>Vues de table

_Cet article décrit l’utilisation des vues de table dans une application Xamarin.Mac. Il décrit la création de vues de table dans Xcode Interface Builder et interagir avec elles dans le code._

Lorsque vous travaillez avec c# et .NET dans une application Xamarin.Mac, vous avez accès à la même Table qui affiche un développeur travaillant dans *Objective-C* et *Xcode* est. Xamarin.Mac s’intègre directement avec Xcode, vous pouvez utiliser de Xcode _Interface Générateur_ pour créer et gérer vos affichages de tableau (ou éventuellement de les créer directement dans le code C#).

Une vue de Table affiche les données dans un format tabulaire qui contient une ou plusieurs colonnes d’informations dans plusieurs lignes. Selon le type d’affichage de tableau en cours de création, l’utilisateur peut trier par colonne, réorganiser les colonnes, ajouter des colonnes, supprimer des colonnes ou modifier les données contenues dans la table.

[![](table-view-images/intro01.png "Exemple de table")](table-view-images/intro01.png#lightbox)

Dans cet article, nous aborderons les principes fondamentaux de l’utilisation des vues de Table dans une application Xamarin.Mac. Il est fortement recommandé que vous parcourez le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Générateur](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) et [prises et Actions](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sections, telle qu’elle couvre les principaux concepts et techniques que nous utiliserons dans cet article.

Vous souhaiterez peut-être examiner la [classes exposition c# / méthodes pour Objective-C](~/mac/internals/how-it-works.md) section de la [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) de document, elle explique la `Register` et `Export` commandes permet de câble des vos classes c# pour les objets Objective-C et les éléments d’interface utilisateur.

<a name="Introduction_to_Table_Views" />

## <a name="introduction-to-table-views"></a>Introduction aux vues de Table

Une vue de Table affiche les données dans un format tabulaire qui contient une ou plusieurs colonnes d’informations dans plusieurs lignes. Vues de table sont affichent à l’intérieur des vues à défilement (`NSScrollView`) et à partir de macOS 10.7, vous pouvez utiliser les `NSView` au lieu de cellules (`NSCell`) pour afficher les lignes et colonnes. Ceci dit, vous pouvez toujours utiliser `NSCell` Toutefois, vous allez généralement sous-classe `NSTableCellView` et créer vos lignes personnalisées et les colonnes.

Une vue de Table ne stocke pas ses propres données, au lieu de cela, elle s’appuie sur une Source de données (`NSTableViewDataSource`) pour fournir les lignes et colonnes nécessaires sur un en fonction des besoins.

Comportement de la vue d’une Table peut être personnalisé en fournissant une sous-classe du délégué vue Table (`NSTableViewDelegate`) pour prendre en charge la gestion de colonne de table, tapez pour sélectionner des fonctionnalités, la sélection de ligne modification, suivi personnalisé et des vues personnalisées pour des colonnes individuelles et lignes.

Lorsque vous créez des vues de Table, Apple suggère les éléments suivants :

* Autoriser l’utilisateur de trier la table en cliquant sur un en-têtes de colonne.
* Créer des en-têtes de colonnes qui sont des noms ou des expressions nominales court qui décrivent les données sont présentées dans cette colonne.

Pour plus d’informations, consultez la [affichage du contenu](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) section d’Apple [indications de l’Interface du système d’exploitation X humaine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Creating-and-Maintaining-Table-Views-in-Xcode" />

## <a name="creating-and-maintaining-table-views-in-xcode"></a>Création et la maintenance des vues de Table dans Xcode

Lorsque vous créez une nouvelle application/Cocoa de Xamarin.Mac, vous obtenez une fenêtre vide, standard par défaut. Ce paramètre, windows est défini dans un `.storyboard` fichier inclus automatiquement dans le projet. Pour modifier votre conception de windows, dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` fichier :

[![](table-view-images/edit01.png "En sélectionnant le storyboard principal")](table-view-images/edit01.png#lightbox)

La conception de la fenêtre s’ouvre dans le Générateur de Xcode Interface :

[![](table-view-images/edit02.png "Modification de l’interface utilisateur dans Xcode")](table-view-images/edit02.png#lightbox)

Type `table` dans les **l’inspecteur de bibliothèque** zone de recherche pour le rendre plus facile à trouver les contrôles de la Table de vue :

[![](table-view-images/edit03.png "Sélectionnez une vue de Table dans la bibliothèque")](table-view-images/edit03.png#lightbox)

Faites glisser une Table de vue sur le contrôleur de la vue dans le **éditeur de l’Interface**, rendre remplir la zone de contenu de la vue de contrôleur et affectez-lui la valeur où il augmente et diminue à la fenêtre de la **éditeur de contrainte**:

[![](table-view-images/edit04.png "Modification des contraintes")](table-view-images/edit04.png#lightbox)

Sélectionnez la vue de Table dans le **hiérarchie de l’Interface** et les propriétés suivantes sont disponibles dans le **inspecteur de l’attribut**:

[![](table-view-images/edit05.png "L’inspecteur de l’attribut")](table-view-images/edit05.png#lightbox)

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
> Sauf si vous mettez à jour une application héritée de Xamarin.Mac, `NSView` basée sur des vues de Table doit être utilisées sur `NSCell` en fonction des vues de Table. `NSCell` est considéré comme hérités et ne pas être gérée à l’avenir.

Sélectionnez une colonne de Table dans le **hiérarchie de l’Interface** et les propriétés suivantes sont disponibles dans le **inspecteur de l’attribut**:

[![](table-view-images/edit06.png "L’inspecteur de l’attribut")](table-view-images/edit06.png#lightbox)

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

[![](table-view-images/edit07.png "L’inspecteur de l’attribut")](table-view-images/edit07.png#lightbox)

Il s’agit de toutes les propriétés d’une vue standard. Vous avez également la possibilité de redimensionner les lignes de cette colonne ici.

Sélectionnez une cellule de la vue Table (par défaut, il s’agit d’un `NSTextField`) dans le **hiérarchie de l’Interface** et les propriétés suivantes sont disponibles dans le **inspecteur de l’attribut**:

[![](table-view-images/edit08.png "L’inspecteur de l’attribut")](table-view-images/edit08.png#lightbox)

Vous avez toutes les propriétés d’un champ de texte standard pour définir ici. Par défaut, un champ de texte standard est utilisé pour afficher les données d’une cellule dans une colonne.

Sélectionnez un affichage de la cellule de tableau (`NSTableFieldCell`) dans le **hiérarchie de l’Interface** et les propriétés suivantes sont disponibles dans le **inspecteur de l’attribut**:

[![](table-view-images/edit09.png "L’inspecteur de l’attribut")](table-view-images/edit09.png#lightbox)

La plus importante ici sont les suivantes :

- **Disposition** - sélectionnez la façon dont sont disposés de cellules de cette colonne.
- **Utilise le Mode à ligne unique** - si `true`, la cellule est limitée à une seule ligne.
- **Largeur de disposition première exécution** - si `true`, la cellule préfèreront la largeur définie pour elle (manuellement ou automatiquement) lorsque elle est affichée lors de la première exécution de l’application.
- **Action** -contrôle le moment où la modification **Action** est envoyé pour la cellule.
- **Comportement** -définit si une cellule est sélectionnable ou modifiable.
- **Rich Text** - si `true`, la cellule peut afficher du texte mis en forme et de style.
- **Annuler** - si `true`, la cellule prend la responsabilité pour annuler le comportement.

Sélectionnez la vue de la cellule de Table (`NSTableFieldCell`) en bas d’une colonne de Table dans le **hiérarchie de l’Interface**:

[![](table-view-images/edit10.png "Sélection de la vue de cellule de Table")](table-view-images/edit10.png#lightbox)

Cela vous permet de modifier la vue de cellule de Table utilisée comme base _modèle_ pour toutes les cellules sont créés pour la colonne spécifiée.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Ajout d’Actions et les points de vente

Juste comme tout autre contrôle de l’interface utilisateur/Cocoa, nous avons besoin exposer notre Table de vue et il est de colonnes et des cellules à c# code à l’aide de **Actions** et **prises** (basé sur les fonctionnalités requises).

Le processus est le même que pour n’importe quel élément d’affichage de la Table que nous souhaitons pour exposer :

1. Basculez vers le **Assistant éditeur** et vérifiez que le `ViewController.h` fichier est sélectionné : 

    [![](table-view-images/edit11.png "L’éditeur de l’Assistant")](table-view-images/edit11.png#lightbox)
2. Sélectionnez la vue de Table dans le **hiérarchie de l’Interface**, la touche CTRL enfoncée, puis faites glisser vers le `ViewController.h` fichier.
3. Créer un **prise** pour la vue Table appelée `ProductTable`: 

    [![](table-view-images/edit13.png "Configuration d’une prise de courant")](table-view-images/edit13.png#lightbox)
4. Créer **prises** pour les colonnes de tables aussi appelé `ProductColumn` et `DetailsColumn`: 

    [![](table-view-images/edit14.png "Configuration d’une prise de courant")](table-view-images/edit14.png#lightbox)
5. Enregistrer les modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.

Nous allons ensuite écrire à l’affichage du code des données pour la table lors de l’application est exécutée.

<a name="Populating_the_Table_View" />

## <a name="populating-the-table-view"></a>Remplissage de la vue de Table

Avec notre Table vue conçu dans le Générateur de Interface et exposées via un **prise**, ensuite, nous avons besoin créer le code c# pour le remplir.

Tout d’abord, nous allons créer un nouveau `Product` classe pour contenir les informations pour les lignes individuelles. Dans le **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **nouveau fichier...** Sélectionnez **général** > **classe vide**, entrez `Product` pour le **nom** et cliquez sur le **nouveau** bouton :

[![](table-view-images/populate01.png "Création d’une classe vide")](table-view-images/populate01.png#lightbox)

Rendre le `Product.cs` fichier ressemble les éléments suivants :

```csharp
using System;

namespace MacTables
{
    public class Product
    {
        #region Computed Propoperties
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

Ensuite, nous avons besoin créer une sous-classe de `NSTableDataSource` pour fournir les données pour notre table comme il est demandé. Dans le **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **nouveau fichier...** Sélectionnez **général** > **classe vide**, entrez `ProductTableDataSource` pour le **nom** et cliquez sur le **nouveau** bouton.

Modifier la `ProductTableDataSource.cs` de fichier et le rendre l’aspect suivant :

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

Cette classe dispose de stockage pour les éléments de la vue de notre Table et remplace le `GetRowCount` pour retourner le nombre de lignes dans la table.

Enfin, nous devons créer une sous-classe de `NSTableDelegate` pour fournir le comportement de notre table. Dans le **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **nouveau fichier...** Sélectionnez **général** > **classe vide**, entrez `ProductTableDelegate` pour le **nom** et cliquez sur le **nouveau** bouton.

Modifier la `ProductTableDelegate.cs` de fichier et le rendre l’aspect suivant :

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

Lorsque nous créons une instance de la `ProductTableDelegate`, nous passons également dans une instance de la `ProductTableDataSource` qui fournit les données de la table. Le `GetViewForItem` méthode est chargée de retourner une vue (données) pour afficher la cellule pour une colonne donnée et une ligne. Si possible, une vue existante sera réutilisée pour afficher la cellule, si ce n’est pas une nouvelle vue doit être créée.

Pour remplir la table, permet de modifier le `ViewController.cs` et effectuez le `AwakeFromNib` détaillée de la méthode comme suit :

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

Si nous exécutons l’application, le texte suivant s’affiche :

[![](table-view-images/populate02.png "Un exemple d’application de s’exécuter")](table-view-images/populate02.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Tri par colonne

Nous allons permettre à l’utilisateur de trier les données dans la table en cliquant sur un en-tête de colonne. Tout d’abord, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour le modifier dans le Générateur de l’Interface. Sélectionnez le `Product` colonne, entrez `Title` pour le **la clé de tri**, `compare:` pour le **sélecteur** et sélectionnez `Ascending` pour le **commande**:

[![](table-view-images/sort01.png "Définition de la clé de tri")](table-view-images/sort01.png#lightbox)

Sélectionnez le `Details` colonne, entrez `Description` pour le **la clé de tri**, `compare:` pour le **sélecteur** et sélectionnez `Ascending` pour le **commande**:

[![](table-view-images/sort02.png "Définition de la clé de tri")](table-view-images/sort02.png#lightbox)

Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.

Maintenant nous allons modifier le `ProductTableDataSource.cs` et ajoutez les méthodes suivantes :

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

Le `Sort` méthode nous permet de trier les données dans la Source de données basée sur une donnée `Product` champ de classe dans l’ordre croissant ou décroissant. Le substituée `SortDescriptorsChanged` méthode sera appelée chaque fois que l’utilisation clique sur un en-tête de colonne. Il sera passé le **clé** valeur que nous avons défini dans le constructeur d’Interface et l’ordre de tri pour cette colonne.

Si nous exécuter l’application et cliquez sur les en-têtes de colonnes, les lignes sont triées par cette colonne :

[![](table-view-images/sort03.png "Un exemple d’application exécuter")](table-view-images/sort03.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Sélection de ligne

Si vous souhaitez autoriser l’utilisateur à sélectionner une seule ligne, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour le modifier dans le Générateur de l’Interface. Sélectionnez la vue de Table dans le **hiérarchie de l’Interface** et désactivez la **plusieurs** case à cocher dans la **inspecteur de l’attribut**:

[![](table-view-images/select01.png "L’inspecteur de l’attribut")](table-view-images/select01.png#lightbox)

Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.


Modifiez ensuite le `ProductTableDelegate.cs` et ajoutez la méthode suivante :

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

Ainsi, l’utilisateur de sélectionner une seule ligne dans la vue de la Table. Retourner `false` pour le `ShouldSelectRow` pour les lignes que vous ne souhaitez pas l’utilisateur à sélectionner ou `false` pour chaque ligne, si vous ne souhaitez pas l’utilisateur pour pouvoir sélectionner les lignes.

La vue de Table (`NSTableView`) contient les méthodes suivantes pour travailler avec la sélection de ligne :

- `DeselectRow(nint)` -Désélectionne la ligne donnée dans la table.
- `SelectRow(nint,bool)` -Sélectionne la ligne donnée. Passer `false` pour le second paramètre ne sélectionner qu’une seule ligne à la fois.
- `SelectedRow` -Retourne la ligne actuelle sélectionnée dans la table.
- `IsRowSelected(nint)` -Retourne `true` si la ligne donnée est sélectionnée.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Sélection de plusieurs lignes

Si vous souhaitez autoriser l’utilisateur à sélectionner un plusieurs lignes, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour le modifier dans le Générateur de l’Interface. Sélectionnez la vue de Table dans le **hiérarchie de l’Interface** et vérifiez la **plusieurs** case à cocher dans la **inspecteur de l’attribut**:

[![](table-view-images/select02.png "L’inspecteur de l’attribut")](table-view-images/select02.png#lightbox)

Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.


Modifiez ensuite le `ProductTableDelegate.cs` et ajoutez la méthode suivante :

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

Ainsi, l’utilisateur de sélectionner une seule ligne dans la vue de la Table. Retourner `false` pour le `ShouldSelectRow` pour les lignes que vous ne souhaitez pas l’utilisateur à sélectionner ou `false` pour chaque ligne, si vous ne souhaitez pas l’utilisateur pour pouvoir sélectionner les lignes.

La vue de Table (`NSTableView`) contient les méthodes suivantes pour travailler avec la sélection de ligne :

- `DeselectAll(NSObject)` -Désactive toutes les lignes de la table. Utilisez `this` pour le premier paramètre envoyer de l’objet qui effectue la sélection. 
- `DeselectRow(nint)` -Désélectionne la ligne donnée dans la table.
- `SelectAll(NSobject)` -Sélectionner toutes les lignes dans la table. Utilisez `this` pour le premier paramètre envoyer de l’objet qui effectue la sélection.
- `SelectRow(nint,bool)` -Sélectionne la ligne donnée. Passer `false` pour le deuxième paramètre, désactivez la sélection et sélectionnez une seule ligne, passer `true` pour étendre la sélection et inclure cette ligne.
- `SelectRows(NSIndexSet,bool)` -Sélectionne l’ensemble de lignes donné. Passer `false` pour le deuxième paramètre effacer la sélection, puis sélectionnez uniquement ces lignes, passer `true` pour étendre la sélection et inclure ces lignes.
- `SelectedRow` -Retourne la ligne actuelle sélectionnée dans la table.
- `SelectedRows` -Retourne un `NSIndexSet` contenant les index des lignes sélectionnées.
- `SelectedRowCount` -Retourne le nombre de lignes sélectionnées.
- `IsRowSelected(nint)` -Retourne `true` si la ligne donnée est sélectionnée.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Type pour sélectionner la ligne

Si vous souhaitez autoriser l’utilisateur à taper un caractère avec la vue de la Table sélectionnée et sélectionnez la première ligne qui a ce caractère, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour le modifier dans le Générateur de l’Interface. Sélectionnez la vue de Table dans le **hiérarchie de l’Interface** et vérifiez la **Type Sélectionnez** case à cocher dans la **inspecteur de l’attribut**:

[![](table-view-images/type01.png "La définition du type de sélection")](table-view-images/type01.png#lightbox)

Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.

Maintenant nous allons modifier le `ProductTableDelegate.cs` et ajoutez la méthode suivante :

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

Le `GetNextTypeSelectMatch` méthode prend le donné `searchString` et retourne la ligne de la première `Product` qui possède cette chaîne dans ses `Title`.

Si nous exécuter l’application et un caractère de type, une ligne est sélectionnée :

[![](table-view-images/type02.png "Un exemple d’application de s’exécuter")](table-view-images/type02.png#lightbox)

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Réorganisation des colonnes

Si vous souhaitez autoriser l’utilisateur à faire glisser réorganiser les colonnes dans la vue de la Table, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour le modifier dans le Générateur de l’Interface. Sélectionnez la vue de Table dans le **hiérarchie de l’Interface** et vérifiez la **réorganisation des** case à cocher dans la **inspecteur de l’attribut**:

[![](table-view-images/reorder01.png "L’inspecteur de l’attribut")](table-view-images/reorder01.png#lightbox)

Si nous donner une valeur pour le **enregistrement automatique** propriété et la vérification de la **les informations de colonne** champ, toutes les modifications apportées à la disposition de la table seront automatiquement enregistrées pour nous et restauré la prochaine fois que l’application est exécuté.

Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.

Maintenant nous allons modifier le `ProductTableDelegate.cs` et ajoutez la méthode suivante :

```csharp
public override bool ShouldReorder (NSTableView tableView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

Le `ShouldReorder` méthode doit retourner `true` pour les colonnes que vous voulez autoriser à être glisser réorganisées dans la `newColumnIndex`, sinon retour `false`;

Si nous exécutons l’application, nous pouvons faire glisser autour des en-têtes de colonne pour réorganiser les colonnes :

[![](table-view-images/reorder02.png "Un exemple des colonnes réorganisés")](table-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Modification des cellules

Si vous souhaitez autoriser l’utilisateur à modifier les valeurs d’une cellule donnée, modifiez le `ProductTableDelegate.cs` de fichiers et de modifier le `GetViewForItem` méthode comme suit :

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

Pour inclure une image dans le cadre de la cellule dans une `NSTableView`, vous devez modifier la façon dont les données sont retournées par la vue de Table `NSTableViewDelegate's` `GetViewForItem` méthode à utiliser un `NSTableCellView` au lieu du type `NSTextField`. Par exemple :

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

Pour plus d’informations, consultez la [à l’aide des Images avec des vues de Table](~/mac/app-fundamentals/image.md) section de notre [utilisation de l’Image](~/mac/app-fundamentals/image.md) documentation.

<a name="Adding-a-Delete-Button-to-a-Row" />

## <a name="adding-a-delete-button-to-a-row"></a>Ajout d’un bouton Supprimer d’une ligne

Selon les besoins de votre application, il peut arriver dans lequel vous devez fournir un bouton d’action pour chaque ligne dans la table. Comme exemple, examinons l’exemple de vue de la Table créée ci-dessus pour inclure un **supprimer** bouton sur chaque ligne.

Tout d’abord, modifiez le `Main.storyboard` dans le Générateur de Xcode Interface, sélectionnez la vue de Table et augmenter le nombre de colonnes à trois (3). Ensuite, modifiez le **titre** de la nouvelle colonne à `Action`:

[![](table-view-images/delete01.png "Modification du nom de colonne")](table-view-images/delete01.png#lightbox)

Enregistrez les modifications dans la table de montage séquentiel et revenir à Visual Studio pour Mac synchroniser les modifications.

Modifiez ensuite le `ViewController.cs` et ajoutez la méthode publique suivante :

```csharp
public void ReloadTable ()
{
    ProductTable.ReloadData ();
}
```

Dans le même fichier, modifier la création du nouveau délégué vue Table à l’intérieur de `ViewDidLoad` méthode comme suit :

```csharp
// Populate the Product Table
ProductTable.DataSource = DataSource;
ProductTable.Delegate = new ProductTableDelegate (this, DataSource);
```

À présent, modifiez le `ProductTableDelegate.cs` fichier à inclure une connexion privée pour le contrôleur de la vue et à mettre le contrôleur en tant que paramètre lors de la création d’une nouvelle instance de délégué :

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

Cette opération prend toutes les configurations d’affichage de texte qui ont été précédemment effectuées dans le `GetViewForItem` (méthode) et les place dans un emplacement unique et peut être appelé (depuis la dernière colonne de la table n’est pas une vue de texte, mais un bouton).

Enfin, modifiez le `GetViewForItem` (méthode) et le rendre l’aspect suivant :

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
                var btw = sender as NSButton;
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
            var btw = subview as NSButton;
            if (btw != null) {
                btn.Tag = row;
            }
        }
        break;
    }

    return view;
}
```

Examinons plusieurs sections de ce code plus en détail. Premièrement, si un nouveau `NSTableViewCell` est en cours de création action est pris en fonction du nom de la colonne. Pour les deux premières colonnes (**produit** et **détails**), la nouvelle `ConfigureTextField` méthode est appelée.

Pour le **Action** , une nouvelle colonne `NSButton` est créé et ajouté à la cellule en tant que Sub vue :

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

Du bouton `Tag` propriété est utilisée pour contenir le numéro de la ligne qui est en cours de traitement. Cette valeur sera utilisée ultérieurement lorsque l’utilisateur demande une ligne à supprimer dans du bouton `Activated` événement :

```csharp
// Wireup events
button.Activated += (sender, e) => {
    // Get button and product
    var btw = sender as NSButton;
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

Lors du démarrage du Gestionnaire d’événements, nous obtenons le bouton et le produit qui se trouve sur la ligne de table donnée. Ensuite, une alerte est présentée à l’utilisateur de confirmer la suppression de la ligne. Si l’utilisateur choisit supprimer la ligne, la ligne donnée est supprimée de la Source de données et la table est rechargée :

```csharp
// Remove the given row from the dataset
DataSource.Products.RemoveAt((int)btn.Tag);
Controller.ReloadTable ();
```

Enfin, si la cellule du tableau est réutilisée au lieu d’en cours de création nouveau, le code suivant configure en fonction de la colonne en cours de traitement :

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
        var btw = subview as NSButton;
        if (btw != null) {
            btn.Tag = row;
        }
    }
    break;
}

```

Pour le **Action** colonne, toutes les vues de Sub sont analysés jusqu'à ce que le `NSButton` est trouvé, il est `Tag` propriété est mise à jour pour pointer sur la ligne actuelle.

Avec ces modifications en place, lorsque l’application est exécutée chaque ligne aura une **supprimer** bouton :

[![](table-view-images/delete02.png "La vue de table avec des boutons de suppression")](table-view-images/delete02.png#lightbox)

Lorsque l’utilisateur clique sur un **supprimer** bouton, une alerte s’affichera comme leur demandant de supprimer la ligne donnée :

[![](table-view-images/delete03.png "Une alerte de ligne de suppression")](table-view-images/delete03.png#lightbox)

Si l’utilisateur choisit de suppression, la ligne sera supprimée et la table sera redessinée :

[![](table-view-images/delete04.png "La table une fois que la ligne est supprimée.")](table-view-images/delete04.png#lightbox)

<a name="Data_Binding_Table_Views" />

## <a name="data-binding-table-views"></a>Vues de Table de liaison de données

À l’aide de techniques de codage de la clé-valeur et la liaison de données dans votre application Xamarin.Mac, vous pouvez réduire considérablement la quantité de code que vous devez écrire et conserver pour remplir et utiliser des éléments d’interface utilisateur. Vous avez également l’avantage de découplage davantage de vos données de sauvegarde (_modèle de données_) à partir de votre premier plan mettre fin à l’Interface utilisateur (_Model-View-Controller_), début au plus facile à gérer, une application plus souple conception.

Clé-valeur de codage (KVM) est un mécanisme pour accéder aux propriétés d’un objet indirectement, à l’aide de clés (spécialement mise en forme des chaînes) pour identifier les propriétés au lieu d’y accéder via des variables d’instance ou des méthodes d’accesseur (`get/set`). En implémentant la clé-valeur de codage des accesseurs conformes dans votre application Xamarin.Mac, pour accéder à d’autres fonctionnalités macOS telles que la clé-valeur en observant (KVO), la liaison de données, données de base, les liaisons/Cocoa et scriptability.

Pour plus d’informations, consultez la [une liaison de données Table vue](~/mac/app-fundamentals/databinding.md#Table_View_Data_Binding) section de notre [une liaison de données et le codage de la clé-valeur](~/mac/app-fundamentals/databinding.md) documentation.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a pris une présentation détaillée sur l’utilisation des vues de Table dans une application Xamarin.Mac. Nous avons vu les différents types et utilise des vues des tables, comment créer et tenir à jour les vues des tables dans le Générateur de Xcode Interface et l’utilisation des vues de Table dans le code c#.

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
