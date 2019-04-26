---
title: Utilisation de tvOS texte et les champs de recherche dans Xamarin
description: Ce document décrit l’utilisation des champs de recherche et de texte dans une application tvOS générées avec Xamarin. Il fournit une vue d’ensemble de champs de recherche et de texte et explique les claviers, intégration de la table de montage séquentiel, les modèles de données de recherche et bien plus encore.
ms.prod: xamarin
ms.assetid: 9EE63CA6-2F31-4EE0-AAE5-82E18CFAC06C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: f1085044f2147bec0910a87f8a6174c4648ef9b8
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61162160"
---
# <a name="working-with-tvos-text-and-search-fields-in-xamarin"></a>Utilisation de tvOS texte et les champs de recherche dans Xamarin

Si nécessaire, votre application Xamarin.tvOS peut demander des petits morceaux de texte à partir de l’utilisateur (par exemple, l’ID d’utilisateur et mots de passe) à l’aide d’un champ de texte et le clavier visuel :

[![](text-fields-and-search-images/intro01.png "Exemple de champ de recherche")](text-fields-and-search-images/intro01.png#lightbox)

Vous pouvez éventuellement fournir la possibilité de recherche de mot clé de contenu de l’application à l’aide d’un champ de recherche :

[![](text-fields-and-search-images/intro02.png "Exemples de résultats de recherche")](text-fields-and-search-images/intro02.png#lightbox)

Ce document aborde les détails de l’utilisation de texte et les champs de recherche dans une application Xamarin.tvOS.

<a name="About-Text-and-Search-Fields" />

## <a name="about-text-and-search-fields"></a>À propos des champs de texte et recherche

Comme indiqué ci-dessus, si nécessaire, votre Xamarin.tvOS peut présenter un ou plusieurs champs de texte pour collecter les petites quantités de texte de l’utilisateur à l’aide d’un à l’écran (ou le clavier bluetooth facultatif selon la version de tvOS que l’utilisateur a installé). 

En outre, si votre application présente les grandes quantités de contenu à l’utilisateur (par exemple, une musique, films ou une collection d’images), vous souhaiterez inclure un champ de recherche qui permet à l’utilisateur à entrer une petite quantité de texte pour filtrer la liste des éléments disponibles.

<a name="Text-Fields" />

## <a name="text-fields"></a>Champs de texte

Dans tvOS, un champ de texte est présenté sous la forme d’une zone de saisie de hauteur fixe, angle arrondi qui fera apparaître un clavier visuel lorsque l’utilisateur clique dessus :

[![](text-fields-and-search-images/text01.png "Texte dans les champs tvOS")](text-fields-and-search-images/text01.png#lightbox)

Lorsque l’utilisateur déplace [Focus](~/ios/tvos/app-fundamentals/navigation-focus.md) à un champ de texte donné, il sera volumineuse et afficher une ombre approfondie. Vous devez en tenir à l’esprit lorsque vous concevez votre Interface utilisateur, comme les champs de texte peuvent se chevaucher d’autres éléments d’interface utilisateur lorsque le focus.

Apple a les suggestions suivantes pour l’utilisation des champs de texte :

- **Utiliser l’entrée de texte avec parcimonie** - en raison de la nature du clavier visuel, long sections de texte de saisie ou de remplir plusieurs champs de texte est fastidieux à l’utilisateur. Une meilleure solution consiste à limiter la quantité de l’entrée de texte à l’aide de listes de sélection ou [boutons](~/ios/tvos/user-interface/buttons.md).
- **Utiliser des indicateurs pour but de communiquer** -champ de texte peut afficher les indicateurs « espace réservé » lorsqu’il est vide. Le cas échéant, utiliser les indicateurs pour décrire l’objectif de votre champ de texte au lieu d’une étiquette distincte.
- **Sélectionnez le Type de clavier par défaut approprié** -tvOS fournit plusieurs types de clavier spécialement conçus différents, que vous pouvez spécifier pour le champ de texte. Par exemple, le clavier d’adresse de messagerie peut faciliter la saisie en autorisant l’utilisateur à sélectionner à partir d’une liste d’adresses récemment entrés.
- **Lorsque cela est approprié, utilisez Secure champs de texte** -A sécuriser un champ de texte présente les caractères entrés sous forme de points (au lieu des lettres réels). Utilisez toujours un champ de texte sécuriser lors de la collecte des informations sensibles telles que les mots de passe.

<a name="Keyboards" />

## <a name="keyboards"></a>Claviers

Chaque fois que l’utilisateur clique sur un champ de texte dans l’Interface utilisateur, un linéaire à l’écran clavier est affiché. L’utilisateur utilise l’aire de toucher le [Siri Remote](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) pour sélectionner des lettres individuelles à partir du clavier et entrez les informations demandées :

[![](text-fields-and-search-images/keyboard01.png "Le clavier Siri Remote")](text-fields-and-search-images/keyboard01.png#lightbox)

S’il existe plusieurs champs de texte sur l’affichage actuel, un **suivant** bouton s’affiche automatiquement pour tenir l’utilisateur sur le prochain champ de texte. Un **fait** bouton s’affiche pour le dernier champ de texte qui mettra fin à l’entrée de texte et renvoie l’utilisateur à l’écran précédent. 

À tout moment, l’utilisateur peut également appuyer sur la **Menu** bouton Siri à un emplacement distant à la fin de l’entrée de texte et de retourner à nouveau à l’écran précédent.

Apple a les suggestions suivantes sur l’utilisation à l’écran claviers :

- **Sélectionnez le Type de clavier par défaut approprié** -tvOS fournit plusieurs types de clavier spécialement conçus différents, que vous pouvez spécifier pour le champ de texte. Par exemple, le clavier d’adresse de messagerie peut faciliter la saisie en autorisant l’utilisateur à sélectionner à partir d’une liste d’adresses récemment entrés.
- **Le cas échéant, utiliser des vues d’accessoire de clavier** : en plus de la norme informations qui sont toujours affiché, optional accessoire des vues (telles que des Images ou d’étiquettes) peuvent être ajoutées pour le clavier visuel pour clarifier l’objectif de l’entrée de texte ou à aider l’utilisateur à entrer les informations nécessaires.

Pour plus d’informations sur l’utilisation avec le clavier visuel, consultez d’Apple [UIKeyboardType](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITextInputTraits_Protocol/index.html#//apple_ref/c/tdef/UIKeyboardType), [la gestion du clavier](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/KeyboardManagement/KeyboardManagement.html#//apple_ref/doc/uid/TP40009542-CH5-SW1), [affichages personnalisés pour l’entrée de données](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/InputViews/InputViews.html#//apple_ref/doc/uid/TP40009542-CH12-SW1) et [ Guide de programmation du texte à iOS](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/Introduction/Introduction.html) documentation.

<a name="Search" />

## <a name="search"></a>Rechercher

Un champ de recherche présente un écran spécialisé en fournissant un champ de texte et le clavier visuel qui permet à l’utilisateur filtrer une collection d’éléments qui s’affichent sous le clavier :

[![](text-fields-and-search-images/search01.png "Exemples de résultats de recherche")](text-fields-and-search-images/search01.png#lightbox)

Lorsque l’utilisateur entre les lettres dans le champ de recherche, les résultats ci-dessous reflète automatiquement les résultats de la recherche. À tout moment, l’utilisateur peut déplacer le Focus vers les résultats et sélectionnez un des éléments présentés.

Apple a les suggestions suivantes pour l’utilisation des champs de recherche :

- **Fournir des recherches récentes** - parce que la saisie de texte avec l’instance distante de Siri peut être fastidieux et les utilisateurs ont tendance à répéter les requêtes de recherche, envisagez d’ajouter une section de résultats de recherche récents avant les résultats actuels sous la zone de clavier.
- **Dans la mesure du possible, limitez le nombre de résultats** - parce qu’une grande liste d’éléments peut être difficile pour l’utilisateur analyser et naviguer, envisagez de limiter le nombre de résultats retournés.
- **Le cas échéant, fournissez les résultats de recherche filtre** : si le contenu fourni par votre application prête, envisagez d’ajouter des barres de l’étendue pour permettre à l’utilisateur filtrer davantage les résultats de recherche retournés.

Pour plus d’informations, consultez le site d’Apple [référence de classe UISearchController](https://developer.apple.com/library/tvos/documentation/UIKit/Reference/UISearchController/index.html).

<a name="Working-with-Text-Fields" />

## <a name="working-with-text-fields"></a>Utilisation des champs de texte

Pour travailler avec des champs de texte dans une application Xamarin.tvOS, le plus simple consiste à ajouter à la conception d’Interface utilisateur à l’aide du concepteur iOS.

Effectuez ce qui suit :

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

1. Dans le **panneau solutions**, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour modification.
1. Faites glisser un ou plusieurs **champs de texte** int l’aire de conception sur une vue : 

    [![](text-fields-and-search-images/text02.png "Un champ de texte")](text-fields-and-search-images/text02.png#lightbox)
1. Sélectionnez le **champs de texte** et à attribuer à chaque unique **nom** dans le **Widget** onglet de la **panneau Propriétés**: 

    [![](text-fields-and-search-images/text03.png "L’onglet de Widget du panneau Propriétés")](text-fields-and-search-images/text03.png#lightbox)
1. Dans le **champ de texte** section, vous pouvez définir des éléments tels que le **espace réservé** indicateur et la valeur par défaut **valeur**: 

    [![](text-fields-and-search-images/text04.png "La section de champ de texte")](text-fields-and-search-images/text04.png#lightbox)
1. Défiler vers le bas pour définir des propriétés telles que **vérification orthographique**, **mise en majuscules** et la valeur par défaut **clavier Type**: 

    [![](text-fields-and-search-images/text05.png "Contrôle orthographique, mise en majuscules et du Type de clavier par défaut")](text-fields-and-search-images/text05.png#lightbox) 
1. Enregistrez les modifications dans votre table de montage séquentiel.
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. Dans l’**Explorateur de solutions**, double-cliquez sur le fichier `Main.storyboard` pour l’ouvrir et le modifier.
1. Faites glisser un ou plusieurs **champs de texte** int l’aire de conception sur une vue : 

    [![](text-fields-and-search-images/text02-vs.png "Un champ de texte")](text-fields-and-search-images/text02-vs.png#lightbox)
1. Sélectionnez le **champs de texte** et à attribuer à chaque unique **nom** dans le **Widget** onglet de la **l’Explorateur de propriétés**: 

    [![](text-fields-and-search-images/text03-vs.png "L’onglet de Widget")](text-fields-and-search-images/text03-vs.png#lightbox)
1. Dans le **champ de texte** section, vous pouvez définir des éléments tels que le **espace réservé** indicateur et la valeur par défaut **valeur**: 

    [![](text-fields-and-search-images/text04-vs.png "La section de champ de texte")](text-fields-and-search-images/text04-vs.png#lightbox)
1. Défiler vers le bas pour définir des propriétés telles que **vérification orthographique**, **mise en majuscules** et la valeur par défaut **clavier Type**: 

    [![](text-fields-and-search-images/text05-vs.png "Contrôle orthographique, mise en majuscules et du Type de clavier par défaut")](text-fields-and-search-images/text05-vs.png#lightbox) 
1. Enregistrez les modifications dans votre table de montage séquentiel.
    
-----

Dans le code, vous pouvez obtenir ou définir la valeur d’un champ de texte à l’aide de son `Text` propriété :

```csharp
Console.WriteLine ("User ID {0} and Password {1}", UserId.Text, Password.Text);
```

Vous pouvez éventuellement utiliser le `Started` et `Ended` des événements de champ de texte pour répondre à l’entrée de texte commence et se termine.

<a name="Working-with-Search-Fields" />

## <a name="working-with-search-fields"></a>Utilisation des champs de recherche

Pour travailler avec des champs de recherche dans une application Xamarin.tvOS, le plus simple consiste à ajouter à la conception d’Interface utilisateur à l’aide du Concepteur d’Interface.

Effectuez ce qui suit :

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)
    
1. Dans le **panneau solutions**, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour modification.
1. Faites glisser un nouveau contrôleur de vue de Collection à la table de montage séquentiel pour présenter les résultats de recherche de l’utilisateur : 

    [![](text-fields-and-search-images/search02.png "Un contrôleur d’affichage de Collection")](text-fields-and-search-images/search02.png#lightbox)
1. Dans le **Widget** onglet de la **panneau Propriétés**, utilisez `SearchResultsViewController` pour le **classe** et `SearchResults` pour le **ID de Storyboard**: 

    [![](text-fields-and-search-images/search03.png "L’onglet de Widget")](text-fields-and-search-images/search03.png#lightbox)
1. Sélectionnez le **cellule Prototype** sur l’aire de conception.
1. Dans le **Widget** onglet de la **l’Explorateur de propriétés**, utilisez `SearchResultCell` pour le **classe** et `ImageCell` pour le **identificateur**: 

    [![](text-fields-and-search-images/search04.png "L’onglet de Widget")](text-fields-and-search-images/search04.png#lightbox)
1. Disposition de la conception de la **cellule Prototype** et exposer chaque élément avec un seul **nom** dans le **Widget** onglet de la **l’Explorateurdepropriétés**: 

    [![](text-fields-and-search-images/search05.png "Disposition de la conception du Prototype de cellule")](text-fields-and-search-images/search05.png#lightbox)
1. Enregistrez les modifications dans votre table de montage séquentiel.
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. Dans l’**Explorateur de solutions**, double-cliquez sur le fichier `Main.storyboard` pour l’ouvrir et le modifier.
1. Faites glisser un nouveau contrôleur de vue de Collection à la table de montage séquentiel pour présenter les résultats de recherche de l’utilisateur : 

    [![](text-fields-and-search-images/seach02-vs.png "Un contrôleur d’affichage de Collection")](text-fields-and-search-images/seach02-vs.png#lightbox)
1. Dans le **Widget** onglet de la **l’Explorateur de propriétés**, utilisez `SearchResultsViewController` pour le **classe** et `SearchResults` pour le **ID de Storyboard**: 

    [![](text-fields-and-search-images/search03-vs.png "L’onglet de Widget")](text-fields-and-search-images/search03-vs.png#lightbox)
1. Sélectionnez le **cellule Prototype** sur l’aire de conception.
1. Dans le **Widget** onglet de la **l’Explorateur de propriétés**, utilisez `SearchResultCell` pour le **classe** et `ImageCell` pour le **identificateur**: 

    [![](text-fields-and-search-images/search04-vs.png "L’onglet de Widget")](text-fields-and-search-images/search04-vs.png#lightbox)
1. Disposition de la conception de la **cellule Prototype** et exposer chaque élément avec un seul **nom** dans le **Widget** onglet de la **l’Explorateurdepropriétés**: 

    [![](text-fields-and-search-images/search05-vs.png "Disposition de la conception du Prototype de cellule")](text-fields-and-search-images/search05-vs.png#lightbox)
1. Enregistrez les modifications dans votre table de montage séquentiel.
    
-----

<a name="Provide-a-Data-Model" />

### <a name="provide-a-data-model"></a>Fournir un modèle de données

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Ensuite, vous devez fournir une classe en tant que le modèle de données pour les résultats que l’utilisateur sera recherchez. Dans le **l’Explorateur de solutions**, cliquez sur le nom du projet et sélectionnez **ajouter** > **nouveau fichier...**   >  **Général** > **classe vide** et fournir un **nom**: 

[![](text-fields-and-search-images/search06.png "Sélectionner une classe vide et indiquez un nom")](text-fields-and-search-images/search06.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Ensuite, vous devez fournir une classe en tant que le modèle de données pour les résultats que l’utilisateur sera recherchez. Dans le **l’Explorateur de solutions**, cliquez sur le nom du projet et sélectionnez **ajouter** > **un nouvel élément...**   >  **Apple** > **divers** > **classe** et fournir un **nom**: 

[![](text-fields-and-search-images/search06-vs.png "Sélectionnez la classe et fournissez un nom")](text-fields-and-search-images/search06-vs.png#lightbox)

-----

Par exemple, une application qui permet à l’utilisateur à rechercher dans une collection d’images par titre et le mot clé peut se présenter comme suit :

```csharp
using System;
using Foundation;

namespace tvText
{
    public class PictureInformation : NSObject
    {
        #region Computed Properties
        public string Title { get; set;}
        public string ImageName { get; set;}
        public string Keywords { get; set;}
        #endregion

        #region Constructors
        public PictureInformation (string title, string imageName, string keywords)
        {
            // Initialize
            this.Title = title;
            this.ImageName = imageName;
            this.Keywords = keywords;
        }
        #endregion
    }
}
```

<a name="The-Collection-View-Cell" />

### <a name="the-collection-view-cell"></a>La cellule d’affichage de Collection

Avec le modèle de données en place, vous devez modifier le **Prototype cellule** (`SearchResultViewCell.cs`) et définissez-la comme aspect se trouvent les éléments suivants :

```csharp
using Foundation;
using System;
using UIKit;

namespace tvText
{
    public partial class SearchResultViewCell : UICollectionViewCell
    {
        #region Private Variables
        private PictureInformation _pictureInfo = null;
        #endregion

        #region Computed Properties
        public PictureInformation PictureInfo {
            get { return _pictureInfo; }
            set {
                _pictureInfo = value;
                UpdateUI ();
            }
        }
        #endregion

        #region Constructors
        public SearchResultViewCell (IntPtr handle) : base (handle)
        {
            // Initialize
            UpdateUI ();
        }
        #endregion

        #region Private Methods
        private void UpdateUI ()
        {
            // Anything to process?
            if (PictureInfo == null) return;

            try {
                Picture.Image = UIImage.FromBundle (PictureInfo.ImageName);
                Picture.AdjustsImageWhenAncestorFocused = true;
                Title.Text = PictureInfo.Title;
                TextColor = UIColor.LightGray;
            } catch {
                // Ignore errors if view isn't fully loaded
            }
        }
        #endregion
    }

}
```

Le `UpdateUI` méthode doit être utilisée pour afficher des champs individuels de la **PictureInformation** éléments (le `PictureInfo` propriété) dans les éléments d’interface utilisateur nommés chaque fois que la propriété est mise à jour. Par exemple, l’Image et titre associé à l’image.

<a name="The-Collection-View-Controller" />

### <a name="the-collection-view-controller"></a>Le contrôleur d’affichage de Collection

Ensuite, modifiez le contrôleur d’affichage recherche résultats de la Collection (`SearchResultsViewController.cs`) et qu’elle ressemble à ce qui suit :

```csharp
using Foundation;
using System;
using UIKit;
using System.Collections.Generic;

namespace tvText
{
    public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
    {
        #region Constants
        public const string CellID = "ImageCell";
        #endregion

        #region Private Variables
        private string _searchFilter = "";
        #endregion

        #region Computed Properties
        public List<PictureInformation> AllPictures { get; set;}
        public List<PictureInformation> FoundPictures { get; set; }
        public string SearchFilter {
            get { return _searchFilter; }
            set {
                _searchFilter = value.ToLower();
                FindPictures ();
                CollectionView?.ReloadData ();
            }
        }
        #endregion

        #region Constructors
        public SearchResultsViewController (IntPtr handle) : base (handle)
        {
            // Initialize
            this.AllPictures = new List<PictureInformation> ();
            this.FoundPictures = new List<PictureInformation> ();
            PopulatePictures ();
            FindPictures ();

        }
        #endregion

        #region Private Methods
        private void PopulatePictures ()
        {
            // Clear list
            AllPictures.Clear ();

            // Add images
            AllPictures.Add (new PictureInformation ("Antipasta Platter","Antipasta","cheese,grapes,tomato,coffee,meat,plate"));
            AllPictures.Add (new PictureInformation ("Cheese Plate", "CheesePlate", "cheese,plate,bread"));
            AllPictures.Add (new PictureInformation ("Coffee House", "CoffeeHouse", "coffee,people,menu,restaurant,cafe"));
            AllPictures.Add (new PictureInformation ("Computer and Expresso", "ComputerExpresso", "computer,coffee,expresso,phone,notebook"));
            AllPictures.Add (new PictureInformation ("Hamburger", "Hamburger", "meat,bread,cheese,tomato,pickle,lettus"));
            AllPictures.Add (new PictureInformation ("Lasagna Dinner", "Lasagna", "salad,bread,plate,lasagna,pasta"));
            AllPictures.Add (new PictureInformation ("Expresso Meeting", "PeopleExpresso", "people,bag,phone,expresso,coffee,table,tablet,notebook"));
            AllPictures.Add (new PictureInformation ("Soup and Sandwich", "SoupAndSandwich", "soup,sandwich,bread,meat,plate,tomato,lettus,egg"));
            AllPictures.Add (new PictureInformation ("Morning Coffee", "TabletCoffee", "tablet,person,man,coffee,magazine,table"));
            AllPictures.Add (new PictureInformation ("Evening Coffee", "TabletMagCoffee", "tablet,magazine,coffee,table"));
        }

        private void FindPictures ()
        {
            // Clear list
            FoundPictures.Clear ();

            // Scan each picture for a match
            foreach (PictureInformation picture in AllPictures) {
                if (SearchFilter == "") {
                    // If no search term, everything matches
                    FoundPictures.Add (picture);
                } else if (picture.Title.Contains (SearchFilter) || picture.Keywords.Contains (SearchFilter)) {
                    // If the search term is in the title or keywords, we've found a match
                    FoundPictures.Add (picture);
                }
            }
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView)
        {
            // Only one section in this collection
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section)
        {
            // Return the number of matching pictures
            return FoundPictures.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get a new cell and return it
            var cell = collectionView.DequeueReusableCell (CellID, indexPath);
            return (UICollectionViewCell)cell;
        }

        public override void WillDisplayCell (UICollectionView collectionView, UICollectionViewCell cell, NSIndexPath indexPath)
        {
            // Grab the cell
            var currentCell = cell as SearchResultViewCell;
            if (currentCell == null)
                throw new Exception ("Expected to display a `SearchResultViewCell`.");

            // Display the current picture info in the cell
            var item = FoundPictures [indexPath.Row];
            currentCell.PictureInfo = item;
        }

        public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // If this Search Controller was presented as a modal view, close
            // it before continuing
            // DismissViewController (true, null);

            // Grab the picture being selected and report it
            var picture = FoundPictures [indexPath.Row];
            Console.WriteLine ("Selected: {0}", picture.Title);
        }

        public void UpdateSearchResultsForSearchController (UISearchController searchController)
        {
            // Save the search filter and update the Collection View
            SearchFilter = searchController.SearchBar.Text ?? string.Empty;
        }

        public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
        {
            var previousItem = context.PreviouslyFocusedView as SearchResultViewCell;
            if (previousItem != null) {
                UIView.Animate (0.2, () => {
                    previousItem.TextColor = UIColor.LightGray;
                });
            }

            var nextItem = context.NextFocusedView as SearchResultViewCell;
            if (nextItem != null) {
                UIView.Animate (0.2, () => {
                    nextItem.TextColor = UIColor.Black;
                });
            }
        }
        #endregion
    }
}
```

Tout d’abord, le `IUISearchResultsUpdating` Interface est ajoutée à la classe pour gérer le filtre de recherche contrôleur mis à jour par l’utilisateur :

```csharp
public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
```

Une constante est également définie pour spécifier l’ID de la **Prototype cellule** (qui correspond à l’ID défini dans le Concepteur d’Interface ci-dessus) qui sera utilisée ultérieurement lorsque le contrôleur de Collection demande une nouvelle cellule :

```csharp
public const string CellID = "ImageCell";
```

Stockage est créé pour la liste complète des éléments de recherche est effectuée, le terme de filtre de recherche et la liste des éléments correspondant à ce terme :

```csharp
private string _searchFilter = "";
...

public List<PictureInformation> AllPictures { get; set;}
public List<PictureInformation> FoundPictures { get; set; }
public string SearchFilter {
    get { return _searchFilter; }
    set {
        _searchFilter = value.ToLower();
        FindPictures ();
        CollectionView?.ReloadData ();
    }
}
```

Lorsque le `SearchFilter` est modifié, la mise à jour de la liste des éléments correspondants et contenu de la vue Collection est rechargé. Le `FindPictures` routine est chargée de rechercher des éléments qui correspondent au nouveau terme de recherche :

```csharp
private void FindPictures ()
{
    // Clear list
    FoundPictures.Clear ();

    // Scan each picture for a match
    foreach (PictureInformation picture in AllPictures) {
        if (SearchFilter == "") {
            // If no search term, everything matches
            FoundPictures.Add (picture);
        } else if (picture.Title.Contains (SearchFilter) || picture.Keywords.Contains (SearchFilter)) {
            // If the search term is in the title or keywords, we've found a match
            FoundPictures.Add (picture);
        }
    }
}
```

La valeur de la `SearchFilter` sera mis à jour (qui met à jour la vue de Collection de résultats) lorsque l’utilisateur modifie le filtre dans le contrôleur de recherche :

```csharp
public void UpdateSearchResultsForSearchController (UISearchController searchController)
{
    // Save the search filter and update the Collection View
    SearchFilter = searchController.SearchBar.Text ?? string.Empty;
}
```

Le `PopulatePictures` méthode remplit initialement la collection d’éléments disponibles :

```csharp
private void PopulatePictures ()
{
    // Clear list
    AllPictures.Clear ();

    // Add images
    AllPictures.Add (new PictureInformation ("Antipasta Platter","Antipasta","cheese,grapes,tomato,coffee,meat,plate"));
    ...
}
```

Cet exemple, tous les exemples de données est créé en mémoire lorsque le contrôleur d’affichage de Collection se charge. Dans une application réelle, ces données seraient probablement être lues à partir d’un base de données ou un service web, et uniquement en fonction des besoins pour empêcher de saturer le téléviseurs Apple une mémoire limitée.

Le `NumberOfSections` et `GetItemsCount` méthodes fournissent le nombre d’éléments de mise en correspondance :

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    // Only one section in this collection
    return 1;
}

public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    // Return the number of matching pictures
    return FoundPictures.Count;
}
```

Le `GetCell` méthode retourne un nouveau **Prototype cellule** (selon le `CellID` définie ci-dessus dans le Storyboard) pour chaque élément dans la vue de Collection :

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    // Get a new cell and return it
    var cell = collectionView.DequeueReusableCell (CellID, indexPath);
    return (UICollectionViewCell)cell;
}
```

Le `WillDisplayCell` méthode est appelée avant la cellule qui est affichée par conséquent, peut être configuré :

```csharp
public override void WillDisplayCell (UICollectionView collectionView, UICollectionViewCell cell, NSIndexPath indexPath)
{
    // Grab the cell
    var currentCell = cell as SearchResultViewCell;
    if (currentCell == null)
        throw new Exception ("Expected to display a `SearchResultViewCell`.");

    // Display the current picture info in the cell
    var item = FoundPictures [indexPath.Row];
    currentCell.PictureInfo = item;
}
```

Le `DidUpdateFocus` méthode fournit des commentaires visuels à l’utilisateur comme ils mettre en surbrillance les éléments dans la vue de Collection de résultats :

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    var previousItem = context.PreviouslyFocusedView as SearchResultViewCell;
    if (previousItem != null) {
        UIView.Animate (0.2, () => {
            previousItem.TextColor = UIColor.LightGray;
        });
    }

    var nextItem = context.NextFocusedView as SearchResultViewCell;
    if (nextItem != null) {
        UIView.Animate (0.2, () => {
            nextItem.TextColor = UIColor.Black;
        });
    }
}
```

Enfin, le `ItemSelected` méthode gère l’utilisateur en sélectionnant un élément (en cliquant sur l’aire de toucher à la distance Siri) dans la vue de Collection de résultats :

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    // If this Search Controller was presented as a modal view, close
    // it before continuing
    // DismissViewController (true, null);

    // Grab the picture being selected and report it
    var picture = FoundPictures [indexPath.Row];
    Console.WriteLine ("Selected: {0}", picture.Title);
}
```

Si le champ de recherche a été présenté sous la forme d’une vue de la boîte de dialogue modale (au-dessus de la vue appelant), utilisez la `DismissViewController` méthode pour fermer la vue de recherche lorsque l’utilisateur sélectionne un élément. Pour cet exemple, le champ de recherche est présenté comme le contenu d’un onglet de l’onglet Affichage, donc il n'est pas ignoré ici.

Pour plus d’informations sur les vues de Collection, consultez notre [utilisation des vues de Collection](~/ios/tvos/user-interface/collection-views.md) documentation.

<a name="Presenting the Search Field" />

### <a name="presenting-the-search-field"></a>Présentation du champ de recherche

Il existe deux méthodes principales qui un champ de recherche (et elle est associée à l’écran du clavier et les résultats de recherche) peut être présenté à l’utilisateur dans tvOS : 

- **Affichage de boîte de dialogue modale** -le champ de recherche peut être présenté sur actuel, vue et contrôleur d’affichage d’un affichage de la boîte de dialogue modale plein écran. Cela s’effectue habituellement en réponse à l’utilisateur clique sur un bouton ou un autre élément d’interface utilisateur. La boîte de dialogue est fermée lorsque l’utilisateur sélectionne un élément dans les résultats de recherche.
- **Afficher le contenu** -le champ de recherche est directement partie d’une vue donnée. Par exemple, en tant que le contenu d’un onglet de recherche dans un contrôleur d’affichage onglet.

Pour l’exemple d’une liste de recherche d’images ci-dessus, le champ de recherche est présenté sous la forme d’afficher le contenu dans l’onglet de recherche et le contrôleur d’affichage onglet recherche ressemble à ceci :

```csharp
using System;
using UIKit;

namespace tvText
{
    public partial class SecondViewController : UIViewController
    {
        #region Constants
        public const string SearchResultsID = "SearchResults";
        #endregion

        #region Computed Properties
        public SearchResultsViewController ResultsController { get; set;}
        #endregion

        #region Constructors
        public SecondViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Private Methods
        public void ShowSearchController ()
        {
            // Build an instance of the Search Results View Controller from the Storyboard
            ResultsController = Storyboard.InstantiateViewController (SearchResultsID) as SearchResultsViewController;
            if (ResultsController == null)
                throw new Exception ("Unable to instantiate a SearchResultsViewController.");

            // Create an initialize a new search controller
            var searchController = new UISearchController (ResultsController) {
                SearchResultsUpdater = ResultsController,
                HidesNavigationBarDuringPresentation = false
            };

            // Set any required search parameters
            searchController.SearchBar.Placeholder = "Enter keyword (e.g. coffee)";

            // The Search Results View Controller can be presented as a modal view
            // PresentViewController (searchController, true, null);

            // Or in the case of this sample, the Search View Controller is being
            // presented as the contents of the Search Tab directly. Use either one
            // or the other method to display the Search Controller (not both).
            var container = new UISearchContainerViewController (searchController);
            var navController = new UINavigationController (container);
            AddChildViewController (navController);
            View.Add (navController.View);
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // If the Search Controller is being displayed as the content
            // of the search tab, include it here.
            ShowSearchController ();
        }

        public override void ViewDidAppear (bool animated)
        {
            base.ViewDidAppear (animated);

            // If the Search Controller is being presented as a modal view,
            // call it here to display it over the contents of the Search
            // tab.
            // ShowSearchController ();
        }
        #endregion
    }
}
```

Tout d’abord, une constante définie qui correspond à la **identificateur de la table de montage séquentiel** qui a été assigné au contrôleur de vue de Collection de résultats de recherche dans le Concepteur d’Interface :

```csharp
public const string SearchResultsID = "SearchResults";
```

Ensuite, le `ShowSearchController` méthode crée un nouveau contrôleur de Collection de vue de recherche et affiche, il était nécessaire :

```csharp
public void ShowSearchController ()
{
    // Build an instance of the Search Results View Controller from the Storyboard
    ResultsController = Storyboard.InstantiateViewController (SearchResultsID) as SearchResultsViewController;
    if (ResultsController == null)
        throw new Exception ("Unable to instantiate a SearchResultsViewController.");

    // Create an initialize a new search controller
    var searchController = new UISearchController (ResultsController) {
        SearchResultsUpdater = ResultsController,
        HidesNavigationBarDuringPresentation = false
    };

    // Set any required search parameters
    searchController.SearchBar.Placeholder = "Enter keyword (e.g. coffee)";

    // The Search Results View Controller can be presented as a modal view
    // PresentViewController (searchController, true, null);

    // Or in the case of this sample, the Search View Controller is being
    // presented as the contents of the Search Tab directly. Use either one
    // or the other method to display the Search Controller (not both).
    var container = new UISearchContainerViewController (searchController);
    var navController = new UINavigationController (container);
    AddChildViewController (navController);
    View.Add (navController.View);
}
```

Dans la méthode ci-dessus, une fois un `SearchResultsViewController` a été instancié à partir de la table de montage séquentiel, un nouveau `UISearchController` est créé pour présenter le champ de recherche et le clavier visuel à l’utilisateur. La collection de résultats de recherche (comme défini par le `SearchResultsViewController`) s’affichera sous ce clavier.

Ensuite, le `SearchBar` est configuré avec les informations telles que le **espace réservé** indicateur. Fournit des informations à l’utilisateur sur le type de recherche est effectuée.

Le champ de recherche est ensuite présenté à l’utilisateur de deux manières :

- **Affichage de boîte de dialogue modale** - le `PresentViewController` méthode est appelée pour présenter la recherche sur la vue existante, plein écran.
- **Afficher le contenu** : un `UISearchContainerViewController` est créé pour contenir le contrôleur de recherche. Un `UINavigationController` est créé pour contenir le conteneur de recherche, puis le contrôleur de Navigation est ajouté pour le contrôleur d’affichage `AddChildViewController (navController)`et la vue présentée `View.Add (navController.View)`.

Enfin et à nouveau en fonction du type de présentation, soit le `ViewDidLoad` ou `ViewDidAppear` méthode appellera le `ShowSearchController` méthode pour présenter la recherche à l’utilisateur :

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // If the Search Controller is being displayed as the content
    // of the search tab, include it here.
    ShowSearchController ();
}

public override void ViewDidAppear (bool animated)
{
    base.ViewDidAppear (animated);

    // If the Search Controller is being presented as a modal view,
    // call it here to display it over the contents of the Search
    // tab.
    // ShowSearchController ();
}
```

Lorsque l’application est exécutée et l’onglet de recherche sélectionné par l’utilisateur, la liste complète non filtrée d’éléments s’affiche à l’utilisateur :

[![](text-fields-and-search-images/intro02.png "Résultats de la recherche par défaut")](text-fields-and-search-images/intro02.png#lightbox)

Lorsque l’utilisateur commence à entrer un terme de recherche, la liste des résultats est filtrée par ce terme et automatiquement mis à jour :

[![](text-fields-and-search-images/intro03.png "Résultats de la recherche filtrés")](text-fields-and-search-images/intro03.png#lightbox)

À tout moment, l’utilisateur peut déplacer le Focus vers un élément dans les résultats de recherche et cliquez sur la Surface Touch Siri Remote pour le sélectionner.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a présenté la conception et l’utilisation de texte et les champs de recherche à l’intérieur d’une application Xamarin.tvOS. Il vous a montré comment créer un contenu de texte et de la Collection de recherche dans le Concepteur d’Interface et il vous a montré à un champ de recherche peuvent être présenté à l’utilisateur dans tvOS deux façons différentes.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application pour tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
