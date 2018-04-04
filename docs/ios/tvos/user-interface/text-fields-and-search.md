---
title: Utilisation de texte et les champs de recherche
description: Cet article décrit la conception et l’utilisation de texte et les champs de recherche à l’intérieur d’une application Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: 9EE63CA6-2F31-4EE0-AAE5-82E18CFAC06C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 220c6e3d1c6f358c67a2f596c977f4d2132298a8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-text-and-search-fields"></a>Utilisation de texte et les champs de recherche

_Cet article décrit la conception et l’utilisation de texte et les champs de recherche à l’intérieur d’une application Xamarin.tvOS._



À la demande, votre application Xamarin.tvOS peut demander des petits éléments de texte à partir de l’utilisateur (par exemple, l’ID utilisateur et mots de passe) à l’aide d’un champ de texte et le clavier visuel :

[![](text-fields-and-search-images/intro01.png "Exemple de champ de recherche")](text-fields-and-search-images/intro01.png#lightbox)

Vous pouvez éventuellement fournir la capacité de recherche de mot clé de contenu de l’application à l’aide d’un champ de recherche :

[![](text-fields-and-search-images/intro02.png "Exemples de résultats de recherche")](text-fields-and-search-images/intro02.png#lightbox)

Ce document décrit les détails de l’utilisation de texte et les champs de recherche dans une application Xamarin.tvOS.

<a name="About-Text-and-Search-Fields" />

## <a name="about-text-and-search-fields"></a>À propos des champs de texte et recherche

Comme indiqué précédemment, si nécessaire, votre Xamarin.tvOS peut présenter un ou plusieurs champs de texte pour collecter les petites quantités de texte à partir de l’utilisateur à l’aide d’un à l’écran (ou le clavier bluetooth facultatif selon la version de tvOS, l’utilisateur a installé). 

En outre, si votre application présente les grandes quantités de contenu à l’utilisateur (par exemple, musique, vidéos ou une collection d’images), vous souhaiterez inclure un champ de recherche qui permet à l’utilisateur à entrer une petite quantité de texte pour filtrer la liste des éléments disponibles.

<a name="Text-Fields" />

## <a name="text-fields"></a>Champs de texte

Dans tvOS, un champ de texte est présenté sous la forme d’une zone de saisie de hauteur fixe, angle arrondi qui affiche un clavier visuel lorsque l’utilisateur clique dessus :

[![](text-fields-and-search-images/text01.png "Texte dans les champs tvOS")](text-fields-and-search-images/text01.png#lightbox)

Lorsque l’utilisateur déplace [Focus](~/ios/tvos/app-fundamentals/navigation-focus.md) à un champ de texte donné, il sera volumineuse et affichent une ombre approfondie. Vous devrez esprit cela lors de la conception de votre Interface utilisateur, comme les champs de texte peuvent se chevaucher d’autres éléments de l’interface utilisateur quand il est actif.

Apple a les suggestions suivantes pour l’utilisation des champs de texte :

- **Utiliser l’entrée de texte avec parcimonie** - en raison de la nature du clavier visuel, long sections de texte de saisie ou remplissez plusieurs champs de texte est fastidieux à l’utilisateur. Une meilleure solution consiste à limiter la quantité de saisie de texte à l’aide de listes de sélection ou [boutons](~/ios/tvos/user-interface/buttons.md).
- **Utiliser des indicateurs pour communiquer l’objectif** -champ de texte peut afficher les indicateurs « espace réservé » lorsqu’il est vide. Le cas échéant, utilisez les indicateurs pour décrire l’objectif de votre champ de texte au lieu d’une étiquette distincte.
- **Sélectionnez le Type de clavier par défaut approprié** -tvOS fournit plusieurs types de clavier spécialisées différents, que vous pouvez spécifier pour le champ de texte. Par exemple, le clavier d’adresse de messagerie peut faciliter la saisie en permettant à l’utilisateur de sélectionner à partir d’une liste d’adresses récemment entrés.
- **Lorsque cela est approprié, utilisez Secure des champs de texte** -un champ de texte sécurisé présente les caractères entrés sous forme de points (au lieu des lettres réels). Utilisez toujours un champ de texte sécurisé lorsque vous collectez des informations sensibles telles que les mots de passe.

<a name="Keyboards" />

## <a name="keyboards"></a>Claviers

Chaque fois que l’utilisateur clique sur un champ de texte dans l’Interface utilisateur, une liste linéaire à l’écran clavier est affiché. L’utilisateur utilise la Surface se touchent le [Siri distant](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) pour sélectionner des lettres individuelles à partir du clavier et entrez les informations demandées :

[![](text-fields-and-search-images/keyboard01.png "Le clavier à distance de Siri")](text-fields-and-search-images/keyboard01.png#lightbox)

S’il existe plusieurs champs de texte sur l’affichage actuel, un **suivant** bouton s’affiche automatiquement pour tenir l’utilisateur dans le champ de texte suivant. A **fait** bouton est affiché pour le dernier champ de texte qui se terminer de saisie de texte et l’utilisateur à l’écran précédent. 

À tout moment, l’utilisateur peut également sélectionner le **Menu** bouton sur l’élément distant Siri à arrêter l’entrée de texte et revenir à nouveau à l’écran précédent.

Apple a les suggestions suivantes sur l’utilisation de l’écran claviers :

- **Sélectionnez le Type de clavier par défaut approprié** -tvOS fournit plusieurs types de clavier spécialisées différents, que vous pouvez spécifier pour le champ de texte. Par exemple, le clavier d’adresse de messagerie peut faciliter la saisie en permettant à l’utilisateur de sélectionner à partir d’une liste d’adresses récemment entrés.
- **Lorsque cela est approprié, utilisez le clavier accessoire vues** : outre la norme informations sont toujours affichée, facultatif accessoire des vues (telles que des Images ou des étiquettes) peuvent être ajoutées au clavier visuel pour clarifier l’objectif de l’entrée de texte ou à aider l’utilisateur à entrer les informations requises.

Pour plus d’informations sur l’utilisation du clavier visuel, veuillez consulter d’Apple [UIKeyboardType](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITextInputTraits_Protocol/index.html#//apple_ref/c/tdef/UIKeyboardType), [la gestion du clavier](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/KeyboardManagement/KeyboardManagement.html#//apple_ref/doc/uid/TP40009542-CH5-SW1), [affichages personnalisés pour l’entrée de données](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/InputViews/InputViews.html#//apple_ref/doc/uid/TP40009542-CH12-SW1) et [ Guide de programmation du texte à iOS](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/Introduction/Introduction.html) documentation.

<a name="Search" />

## <a name="search"></a>Rechercher

Un champ de recherche présente un écran spécialisé qui fournit un champ de texte et le clavier visuel qui permet à l’utilisateur filtrer une collection d’éléments qui s’affichent sous le clavier :

[![](text-fields-and-search-images/search01.png "Exemples de résultats de recherche")](text-fields-and-search-images/search01.png#lightbox)

Lorsque l’utilisateur entre les lettres dans le champ de recherche, les résultats ci-dessous reflètent automatiquement les résultats de la recherche. À tout moment, l’utilisateur peut déplacer le Focus vers les résultats et sélectionnez un des éléments présentés.

Apple a les suggestions suivantes pour l’utilisation des champs de recherche :

- **Fournir des recherches récentes** - parce que la saisie de texte avec Siri distant peut être fastidieuse et les utilisateurs ont tendance à répéter des demandes de recherche, envisagez d’ajouter une section de résultats de la recherche récente avant les résultats actuels sous la zone de clavier.
- **Lorsque cela est Possible, limitez le nombre de résultats** - car une longue liste d’éléments peut être difficile à analyser et accédez, pensez à limiter le nombre de résultats retournés pour l’utilisateur.
- **Si nécessaire, des filtres de fournir les résultats de recherche** : si le contenu fourni par votre application prête, envisagez d’ajouter des barres de l’étendue pour permettre à l’utilisateur filtrer davantage les résultats de recherche.

Pour plus d’informations, consultez le site d’Apple [référence de classe UISearchController](https://developer.apple.com/library/tvos/documentation/UIKit/Reference/UISearchController/index.html).

<a name="Working-with-Text-Fields" />

## <a name="working-with-text-fields"></a>Utilisation des champs de texte

Pour travailler avec des champs de texte dans une application Xamarin.tvOS, le plus simple consiste à ajouter à la conception de l’Interface utilisateur à l’aide du Concepteur d’iOS.

Effectuez ce qui suit :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Dans le **Solution remplissage**, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour le modifier.
1. Faites glisser un ou plusieurs **des champs de texte** int l’aire de conception sur une vue : 

    [![](text-fields-and-search-images/text02.png "Un champ de texte")](text-fields-and-search-images/text02.png#lightbox)
1. Sélectionnez le **des champs de texte** et à attribuer à chaque unique **nom** dans le **Widget** onglet de la **propriétés remplissage**: 

    [![](text-fields-and-search-images/text03.png "L’onglet du Widget de la zone de propriétés")](text-fields-and-search-images/text03.png#lightbox)
1. Dans le **champ de texte** section, vous pouvez définir des éléments tels que les **espace réservé** indicateur et la valeur par défaut **valeur**: 

    [![](text-fields-and-search-images/text04.png "La section champs de texte")](text-fields-and-search-images/text04.png#lightbox)
1. Faites défiler vers le bas pour définir les propriétés **orthographique**, **mise en majuscules** et la valeur par défaut **clavier Type**: 

    [![](text-fields-and-search-images/text05.png "Contrôle orthographique, mise en majuscules et le Type de clavier par défaut")](text-fields-and-search-images/text05.png#lightbox) 
1. Enregistrez les modifications dans votre plan conceptuel.
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. Dans l’**Explorateur de solutions**, double-cliquez sur le fichier `Main.storyboard` pour l’ouvrir et le modifier.
1. Faites glisser un ou plusieurs **des champs de texte** int l’aire de conception sur une vue : 

    [![](text-fields-and-search-images/text02-vs.png "Un champ de texte")](text-fields-and-search-images/text02-vs.png#lightbox)
1. Sélectionnez le **des champs de texte** et à attribuer à chaque unique **nom** dans les **Widget** onglet de la **l’Explorateur de propriétés**: 

    [![](text-fields-and-search-images/text03-vs.png "L’onglet du Widget")](text-fields-and-search-images/text03-vs.png#lightbox)
1. Dans le **champ de texte** section, vous pouvez définir des éléments tels que les **espace réservé** indicateur et la valeur par défaut **valeur**: 

    [![](text-fields-and-search-images/text04-vs.png "La section champs de texte")](text-fields-and-search-images/text04-vs.png#lightbox)
1. Faites défiler vers le bas pour définir les propriétés **orthographique**, **mise en majuscules** et la valeur par défaut **clavier Type**: 

    [![](text-fields-and-search-images/text05-vs.png "Contrôle orthographique, mise en majuscules et le Type de clavier par défaut")](text-fields-and-search-images/text05-vs.png#lightbox) 
1. Enregistrez les modifications dans votre plan conceptuel.
    
-----

Dans le code, vous pouvez obtenir ou définir la valeur d’un champ de texte à l’aide de son `Text` propriété :

```csharp
Console.WriteLine ("User ID {0} and Password {1}", UserId.Text, Password.Text);
```

Vous pouvez éventuellement utiliser le `Started` et `Ended` événements pour répondre à l’entrée de texte de début et de fin de champ de texte.

<a name="Working-with-Search-Fields" />

## <a name="working-with-search-fields"></a>Utilisation des champs de recherche

Pour travailler avec des champs de recherche dans une application Xamarin.tvOS, le plus simple consiste à ajouter à la conception de l’Interface utilisateur à l’aide du Concepteur de l’Interface.

Effectuez ce qui suit :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)
    
1. Dans le **Solution remplissage**, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour le modifier.
1. Faites glisser un nouveau contrôleur de vue de Collection pour la table de montage séquentiel pour présenter les résultats de recherche de l’utilisateur : 

    [![](text-fields-and-search-images/search02.png "Un contrôleur de vue de Collection")](text-fields-and-search-images/search02.png#lightbox)
1. Dans le **Widget** onglet de la **propriétés remplissage**, utiliser `SearchResultsViewController` pour le **classe** et `SearchResults` pour le **ID de la table de montage séquentiel**: 

    [![](text-fields-and-search-images/search03.png "L’onglet du Widget")](text-fields-and-search-images/search03.png#lightbox)
1. Sélectionnez le **cellule Prototype** sur l’aire de conception.
1. Dans le **Widget** onglet de la **l’Explorateur de propriétés**, utilisez `SearchResultCell` pour le **classe** et `ImageCell` pour la **identificateur**: 

    [![](text-fields-and-search-images/search04.png "L’onglet du Widget")](text-fields-and-search-images/search04.png#lightbox)
1. Disposition de la conception de la **cellule Prototype** et exposer chaque élément avec un seul **nom** dans le **Widget** onglet de la **l’Explorateurdepropriétés**: 

    [![](text-fields-and-search-images/search05.png "Disposition de la conception du Prototype de cellule")](text-fields-and-search-images/search05.png#lightbox)
1. Enregistrez les modifications dans votre plan conceptuel.
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. Dans l’**Explorateur de solutions**, double-cliquez sur le fichier `Main.storyboard` pour l’ouvrir et le modifier.
1. Faites glisser un nouveau contrôleur de vue de Collection pour la table de montage séquentiel pour présenter les résultats de recherche de l’utilisateur : 

    [![](text-fields-and-search-images/seach02-vs.png "Un contrôleur de vue de Collection")](text-fields-and-search-images/seach02-vs.png#lightbox)
1. Dans le **Widget** onglet de la **l’Explorateur de propriétés**, utiliser `SearchResultsViewController` pour le **classe** et `SearchResults` pour la **ID de table de montage séquentiel**: 

    [![](text-fields-and-search-images/search03-vs.png "L’onglet du Widget")](text-fields-and-search-images/search03-vs.png#lightbox)
1. Sélectionnez le **cellule Prototype** sur l’aire de conception.
1. Dans le **Widget** onglet de la **l’Explorateur de propriétés**, utilisez `SearchResultCell` pour le **classe** et `ImageCell` pour la **identificateur**: 

    [![](text-fields-and-search-images/search04-vs.png "L’onglet du Widget")](text-fields-and-search-images/search04-vs.png#lightbox)
1. Disposition de la conception de la **cellule Prototype** et exposer chaque élément avec un seul **nom** dans le **Widget** onglet de la **l’Explorateurdepropriétés**: 

    [![](text-fields-and-search-images/search05-vs.png "Disposition de la conception du Prototype de cellule")](text-fields-and-search-images/search05-vs.png#lightbox)
1. Enregistrez les modifications dans votre plan conceptuel.
    
-----

<a name="Provide-a-Data-Model" />

### <a name="provide-a-data-model"></a>Fournir un modèle de données

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Ensuite, vous devez fournir une classe en tant que le modèle de données pour les résultats que l’utilisateur sera recherchez. Dans le **l’Explorateur de solutions**, cliquez sur le nom du projet et sélectionnez **ajouter** > **nouveau fichier...**   >  **Général** > **classe vide** et fournir un **nom**: 

[![](text-fields-and-search-images/search06.png "Sélectionnez une classe vide et fournissez un nom")](text-fields-and-search-images/search06.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

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

### <a name="the-collection-view-cell"></a>La cellule de vue de Collection

Avec le modèle de données en place, vous devez modifier le **Prototype cellule** (`SearchResultViewCell.cs`) et le rendre apparence se trouvent les éléments suivants :

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

Le `UpdateUI` méthode servira pour afficher les champs individuels de la **PictureInformation** éléments (le `PictureInfo` propriété) dans les éléments d’interface utilisateur nommés chaque fois que la propriété est mise à jour. Par exemple, l’Image et titre associé à l’image.

<a name="The-Collection-View-Controller" />

### <a name="the-collection-view-controller"></a>Le contrôleur de vue de Collection

Modifiez ensuite le contrôleur de vue de Collection de résultats recherche (`SearchResultsViewController.cs`) et qu’elle ressemble à ce qui suit :

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

Tout d’abord, le `IUISearchResultsUpdating` Interface est ajouté à la classe pour gérer le filtre de recherche contrôleur en cours de mise à jour par l’utilisateur :

```csharp
public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
```

Une constante est également définie pour spécifier l’ID de la **Prototype cellule** (qui correspond à celui défini dans le Concepteur d’Interface ci-dessus) qui sera utilisée ultérieurement lorsque le contrôleur de Collection demande une nouvelle cellule :

```csharp
public const string CellID = "ImageCell";
```

Stockage est créé pour la liste complète des éléments de recherche est effectuée, le terme de filtre de recherche et une liste d’éléments répondant à ce terme :

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

Lorsque le `SearchFilter` est modifiée, la mise à jour de la liste des éléments de mise en correspondance et le contenu de la vue de Collection est rechargé. Le `FindPictures` routine est responsable de la recherche d’éléments qui correspondent au nouveau terme de recherche :

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

La valeur de la `SearchFilter` sera mise à jour (qui sera mise à jour la vue de Collection de résultats) lorsque l’utilisateur modifie le filtre dans le contrôleur de recherche :

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

Cet exemple, tous les exemples de données est créé dans la mémoire lors de la charge par le contrôleur de la vue de Collection. Dans une application réelle, ces données seront probablement lus à partir d’un service web ou de la base de données, et que si nécessaire pour empêcher que saturer le téléviseur Apple une mémoire limitée.

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

Le `GetCell` méthode retourne un nouveau **Prototype cellule** (selon le `CellID` défini ci-dessus dans le plan conceptuel) pour chaque élément dans la vue de Collection :

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    // Get a new cell and return it
    var cell = collectionView.DequeueReusableCell (CellID, indexPath);
    return (UICollectionViewCell)cell;
}
```

Le `WillDisplayCell` méthode est appelée avant la cellule d’affichage afin qu’il peut être configuré :

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

Le `DidUpdateFocus` méthode fournit une rétroaction visuelle à l’utilisateur comme ils mettre en surbrillance des éléments dans la vue de Collection de résultats :

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

Enfin, le `ItemSelected` méthode gère l’utilisateur de sélectionner un élément (en cliquant sur l’aire de se touchent à la distance Siri) dans la vue de Collection de résultats :

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

Si le champ de recherche a été présenté sous la forme d’une vue de la boîte de dialogue modale (en haut de la vue appelant), utilisez la `DismissViewController` méthode pour fermer la vue de recherche lorsque l’utilisateur sélectionne un élément. Pour cet exemple, le champ de recherche s’affichent en tant que le contenu d’un onglet de l’onglet Affichage, afin qu’il n'est pas en cours de rejet ici.

Pour plus d’informations sur les vues de Collection, consultez notre [utilisation des vues de Collection](~/ios/tvos/user-interface/collection-views.md) documentation.

<a name="Presenting the Search Field" />

### <a name="presenting-the-search-field"></a>Présentation de la zone de recherche

Il existe deux méthodes principales qui un champ de recherche (et qui lui sont associés le clavier visuel et les résultats de la recherche) peuvent être présentées à l’utilisateur dans tvOS : 

- **Affichage de boîte de dialogue modale** -le champ de recherche peuvent être présenté sur en cours, vue et contrôleur de vue comme une vue de la boîte de dialogue modale plein écran. Cela est généralement effectuée en réponse à l’utilisateur clique sur un bouton ou un autre élément d’interface utilisateur. La boîte de dialogue est fermée lorsque l’utilisateur sélectionne un élément dans les résultats de recherche.
- **Afficher le contenu** -le champ de recherche fait partie directe d’une vue donnée. Par exemple, en tant que le contenu d’un onglet de recherche dans un onglet View Controller.

Pour l’exemple d’une liste d’images ci-dessus, le champ de recherche est présenté sous la forme d’afficher le contenu dans l’onglet de recherche et le contrôleur de vue onglet recherche ressemble à ceci :

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

Tout d’abord, une constante est définie qui correspond à la **identificateur de la table de montage séquentiel** qui a été assigné au contrôleur de vue de Collection de résultats de recherche dans le Concepteur de l’Interface :

```csharp
public const string SearchResultsID = "SearchResults";
```

Ensuite, le `ShowSearchController` méthode crée un nouveau contrôleur de Collection de vue de recherche et il était nécessaire d’affiche :

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

Dans la méthode ci-dessus, une fois un `SearchResultsViewController` a été instancié à partir de la table de montage séquentiel, un nouveau `UISearchController` est créé pour présenter le champ de recherche et le clavier visuel à l’utilisateur. La collection de résultats de recherche (comme défini par le `SearchResultsViewController`) s’affiche sous ce clavier.

Ensuite, le `SearchBar` est configurée avec des informations telles que la **espace réservé** indicateur. Fournit des informations à l’utilisateur sur le type de recherche est effectuée.

Puis le champ de recherche est présenté à l’utilisateur de deux manières :

- **Affichage de boîte de dialogue modale** - le `PresentViewController` méthode est appelée pour présenter la recherche sur la vue existante, plein écran.
- **Afficher le contenu** - un `UISearchContainerViewController` est créé pour contenir le contrôleur de recherche. A `UINavigationController` est créé pour contenir le conteneur de recherche, puis le contrôleur de Navigation est ajouté à la vue contrôleur `AddChildViewController (navController)`et la vue présentée `View.Add (navController.View)`.

Enfin et à nouveau selon le type de présentation, soit le `ViewDidLoad` ou `ViewDidAppear` méthode appellera la `ShowSearchController` méthode pour présenter la recherche à l’utilisateur :

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

Lorsque l’application est exécutée et l’onglet de recherche sélectionnées par l’utilisateur, la liste complète non filtrée d’éléments est présentée à l’utilisateur :

[![](text-fields-and-search-images/intro02.png "Résultats de la recherche par défaut")](text-fields-and-search-images/intro02.png#lightbox)

Dès que l’utilisateur à entrer un terme de recherche, la liste des résultats est filtrée par ce terme et automatiquement mis à jour :

[![](text-fields-and-search-images/intro03.png "Résultats de recherche")](text-fields-and-search-images/intro03.png#lightbox)

À tout moment, l’utilisateur peut déplacer le Focus vers un élément dans les résultats de recherche et cliquez sur la Surface tactile Remote Siri pour le sélectionner.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a couvert la conception et l’utilisation de texte et les champs de recherche à l’intérieur d’une application Xamarin.tvOS. Il vous a montré comment créer le contenu de texte et de la Collection de recherche dans le Concepteur de l’Interface et il a montré à deux manières d’un champ de recherche peut être présenté à l’utilisateur dans tvOS.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS les Guides Interface humaine](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
