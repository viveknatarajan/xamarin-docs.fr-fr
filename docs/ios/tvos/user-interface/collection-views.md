---
title: Utilisation de tvOS dans Xamarin, les vues de Collection
description: Ce document décrit comment utiliser des vues de collection dans une application de tvOS générée avec Xamarin. Elle traite des dispositions de vue de collection, création de cellules et des vues supplémentaires, répondre aux événements utilisateur et bien plus encore.
ms.prod: xamarin
ms.assetid: 5125C4C7-2DDF-4C19-A362-17BB2B079178
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 9b411ac6bb8d1492511f9e5d2234731ae64c3a82
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789276"
---
# <a name="working-with-tvos-collection-views-in-xamarin"></a>Utilisation de tvOS dans Xamarin, les vues de Collection

Autoriser les vues de collection pour un groupe de contenu à afficher à l’aide de dispositions arbitraires. À l’aide de la prise en charge intégrée, elles permettent pour les dispositions de grille ou linéaire facilite la création, la prise en charge également des dispositions personnalisées.

[![](collection-views-images/collection01.png "Exemple de vue de collection")](collection-views-images/collection01.png#lightbox)

La vue de Collection maintient une collection d’éléments à l’aide d’un délégué et une Source de données pour fournir une interaction de l’utilisateur et le contenu de la collection. Étant donné que la vue de Collection est basée sur un sous-système de disposition qui est indépendante de la vue proprement dite, en fournissant une disposition différente peut facilement modifier la présentation de données à la volée de la vue de Collection.

<a name="About-Collection-Views" />

## <a name="about-collection-views"></a>À propos des vues de Collection

Comme indiqué précédemment, une vue de Collection (`UICollectionView`) gère une collection ordonnée d’éléments et présente les éléments avec des dispositions personnalisables. Vues de collection fonctionnent de manière similaire aux vues de Table (`UITableView`), sauf qu’ils peuvent utiliser des dispositions pour les éléments présents dans plusieurs qu’une seule colonne.

Lorsque vous utilisez une vue de Collection de tvOS, votre application est chargée de fournir les données associées à la collection à l’aide d’une Source de données (`UICollectionViewDataSource`). Données de vue de collection peuvent éventuellement être organisées et présentées dans différents groupes (Sections).

La vue de Collection présente les éléments à l’écran à l’aide d’une cellule (`UICollectionViewCell`) qui fournit la présentation d’une information donnée à partir de la collection (par exemple, une image et son titre).

Le cas échéant, des vues supplémentaires peuvent être ajoutées à la présentation de la vue de Collection pour agir en tant qu’en-tête et de pieds de page pour les Sections et les cellules. Disposition de l’affichage de la Collection est chargée de définir le placement de ces vues, ainsi que les cellules individuelles.

La vue de Collection peut répondre à une interaction utilisateur à l’aide d’un délégué (`UICollectionViewDelegate`). Ce délégué est également chargé de déterminer si une cellule donnée peut obtenir le focus, si une cellule a été mis en surbrillance ou si vous avez sélectionné. Dans certains cas, le délégué détermine la taille des cellules individuelles.

<a name="Collection-View-Layouts" />

## <a name="collection-view-layouts"></a>Mises en page de vue de collection

Des fonctionnalités clés d’une vue de Collection est son séparation entre les données présentées et sa disposition. Une mise en page de la vue de Collection (`UICollectionViewLayout`) est chargé de fournir l’emplacement des cellules (et toutes les vues supplémentaires) et l’organisation avec de présentation à l’écran de l’affichage de la Collection.

Les cellules individuelles sont créés par la vue de Collection à partir de sa Source de données attachée sont organisées et affichées par la mise en page de vue de Collection donnée.

La mise en page de la vue de Collection est normalement fourni lors de la création de la vue de Collection. Toutefois, vous pouvez modifier la disposition de la vue de Collection à tout moment et la présentation à l’écran de données de l’affichage de la Collection sera automatiquement mise à jour à l’aide de la nouvelle disposition fournie.

La mise en page de la vue de Collection fournit plusieurs méthodes qui peuvent être utilisés pour animer la transition entre deux dispositions différentes (par défaut qu'aucune animation n’est effectuée). En outre, les dispositions de vue de Collection peut fonctionner avec reconnaissances de mouvement à davantage animer intervention de l’utilisateur qui entraîne une modification dans la disposition.

<a name="Creating-Cells-and-Supplementary-Views" />

## <a name="creating-cells-and-supplementary-views"></a>Création de cellules et des vues supplémentaires

Source de données d’une vue de Collection n’est pas uniquement chargée de fournir les données de sauvegarde de la collection à l’élément, mais également les cellules qui sont utilisées pour afficher le contenu.

Étant donné que les vues de Collection ont été conçus pour gérer de grandes collections d’éléments, les cellules individuelles peuvent être dépilés et réutilisées pour éviter de saturer les limitations de mémoire. Il existe deux méthodes différentes pour les vues de retrait :

- `DequeueReusableCell` -Crée ou retourne une cellule du type donné (comme spécifié dans la table de montage séquentiel de l’application).
- `DequeueReusableSupplementaryView` -Crée ou retourne une vue supplémentaire du type donné (comme spécifié dans la table de montage séquentiel de l’application).

Avant d’appeler une de ces méthodes, vous devez inscrire la classe de la table de montage séquentiel ou `.xib` fichier utilisé pour créer un affichage de la cellule avec la vue de Collection. Exemple :

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    ...
}
```

Où `typeof(CityCollectionViewCell)` fournit la classe qui prend en charge l’affichage et `CityViewDatasource.CardCellId` fournit l’ID utilisé lors de la cellule (ou vue) est dépilé.

Une fois que la cellule est retirée, vous le configurer avec les données de l’élément que correspondant, revenez à la vue de Collection pour l’affichage.

<a name="About-Collection-View-Controllers" />

## <a name="about-collection-view-controllers"></a>Sur les contrôleurs de vue de Collection

Un contrôleur de vue de Collection (`UICollectionViewController`) est un contrôleur de vue spécialisée (`UIViewController`) qui fournit le comportement suivant :

- Il est responsable du chargement de la vue de Collection à partir de son Storyboard ou `.xib` de fichiers et de l’instanciation de la vue. Si créé dans le code, il crée automatiquement une nouvelle vue de Collection non configuré.
- Une fois la vue de Collection est chargée, le contrôleur de tente de charger sa Source de données et le délégué à partir de la table de montage séquentiel ou `.xib` fichier. Si aucun n’est pas disponible, il définit lui-même en tant que la source des deux.
- Garantit que les données sont chargées avant que la vue de collection est remplie sur le premier affiché et recharge et effacer la sélection sur chaque affichage suivante.

En outre, le contrôleur de la vue de Collection fournit les méthodes substituables qui peuvent être utilisés pour gérer le cycle de vie de la vue de Collection comme `AwakeFromNib` et `ViewWillDisplay`.

<a name="Collection-Views-and-Storyboards" />

## <a name="collection-views-and-storyboards"></a>Plans conceptuels et les vues de collection

Pour travailler avec une vue de Collection dans votre application Xamarin.tvOS, le plus simple est d’ajouter un à sa table de montage séquentiel. Un exemple rapide, nous allons créer un exemple d’application qui présente une image, title et un bouton de sélection. Si l’utilisateur sur le bouton de sélection, une vue de Collection s’affichera permettant aux utilisateurs de choisir une nouvelle image. Lorsqu’une image est choisie, la vue de Collection est fermée et la nouvelle image et titre seront affichera.

Nous allons effectuer les opérations suivantes :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

    
1. Démarrer une nouvelle **tvOS d’affichage simple application** dans Visual Studio pour Mac.
1. Dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` de fichier et l’ouvrir dans le concepteur iOS.
1. Ajouter une vue de l’Image, une étiquette et un bouton à la vue existante et les configurer pour présenter l’aspect suivant : 

    [![](collection-views-images/collection02.png "Mise en page de l’exemple")](collection-views-images/collection02.png#lightbox)
1. Affecter un **nom** à l’affichage de l’Image et de l’étiquette dans le **onglet Widget** de la **l’Explorateur de propriétés**. Exemple : 

    [![](collection-views-images/collection03.png "Le nom du paramètre")](collection-views-images/collection03.png#lightbox)
1. Ensuite, faites glisser un contrôleur de vue de Collection sur le plan conceptuel : 

    [![](collection-views-images/collection04.png "Un contrôleur de vue de Collection")](collection-views-images/collection04.png#lightbox)
1. Contrôle, faites glisser le bouton vers le contrôleur de vue de Collection, puis sélectionnez **Push** à partir de la fenêtre contextuelle : 

    [![](collection-views-images/collection05.png "Sélectionnez par émission de données à partir de la fenêtre contextuelle")](collection-views-images/collection05.png#lightbox)
1. Lorsque l’application est exécutée, cela rend la vue de Collection être afficher chaque fois que l’utilisateur clique sur le bouton.
1. Sélectionnez la vue de Collection et entrez les valeurs suivantes dans le **onglet Disposition** de la **l’Explorateur de propriétés**: 

    [![](collection-views-images/collection06.png "L’Explorateur de propriétés")](collection-views-images/collection06.png#lightbox)
1. Contrôle la taille des cellules individuelles et les bordures séparant les cellules et le bord extérieur de la vue de Collection.
1. Sélectionnez le contrôleur de la vue de Collection et de sa classe de la valeur `CityCollectionViewController` dans les **onglet Widget**: 

    [![](collection-views-images/collection07.png "La valeur de la classe CityCollectionViewController")](collection-views-images/collection07.png#lightbox)
1. Sélectionnez la vue de Collection et de sa classe de la valeur `CityCollectionView` dans les **onglet Widget**: 

    [![](collection-views-images/collection08.png "La valeur de la classe CityCollectionView")](collection-views-images/collection08.png#lightbox)
1. Sélectionnez la cellule de la vue de Collection et de sa classe de la valeur `CityCollectionViewCell` dans les **onglet Widget**: 

    [![](collection-views-images/collection09.png "La valeur de la classe CityCollectionViewCell")](collection-views-images/collection09.png#lightbox)
1. Dans le **onglet Widget** vous assurer que le **disposition** est `Flow` et **sens de défilement** est `Vertical` pour la vue de Collection : 

    [![](collection-views-images/collection10.png "L’onglet du Widget")](collection-views-images/collection10.png#lightbox)
1. Sélectionnez la cellule de la vue de Collection et définissez son **identité** à `CityCell` dans les **onglet Widget**: 

    [![](collection-views-images/collection11.png "Définissez l’identité sur CityCell")](collection-views-images/collection11.png#lightbox)
1. Enregistrez les modifications apportées.
    

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

    
1. Démarrer une nouvelle **tvOS d’affichage simple application** dans Visual Studio.
1. Dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` de fichier et l’ouvrir dans le concepteur iOS.
1. Ajouter une vue de l’Image, une étiquette et un bouton à la vue existante et les configurer pour présenter l’aspect suivant : 

    [![](collection-views-images/collection02vs.png "Configurez la mise en page")](collection-views-images/collection02vs.png#lightbox)
1. Affecter un **nom** à l’affichage de l’Image et de l’étiquette dans le **onglet Widget** de la **l’Explorateur de propriétés**. Exemple : 

    [![](collection-views-images/collection03vs.png "L’Explorateur de propriétés")](collection-views-images/collection03vs.png#lightbox)
1. Ensuite, faites glisser un contrôleur de vue de Collection sur le plan conceptuel : 

    [![](collection-views-images/collection04vs.png "Un contrôleur de vue de Collection")](collection-views-images/collection04vs.png#lightbox)
1. Contrôle, faites glisser le bouton vers le contrôleur de vue de Collection, puis sélectionnez **Push** à partir de la fenêtre contextuelle : 

    [![](collection-views-images/collection05vs.png "Sélectionnez par émission de données à partir de la fenêtre contextuelle")](collection-views-images/collection05vs.png#lightbox)
1. Lorsque l’application est exécutée, cela rend la vue de Collection être afficher chaque fois que l’utilisateur clique sur le bouton.
1. Sélectionnez la vue de Collection et dans le **onglet Disposition** de la **l’Explorateur de propriétés** Entrez le **largeur** en tant que _361_ et  **Hauteur** comme _256_ 
1. Contrôle la taille des cellules individuelles et les bordures séparant les cellules et le bord extérieur de la vue de Collection.
1. Sélectionnez le contrôleur de la vue de Collection et de sa classe de la valeur `CityCollectionViewController` dans les **onglet Widget**: 

    [![](collection-views-images/collection07vs.png "La valeur de la classe CityCollectionViewController")](collection-views-images/collection07vs.png#lightbox)
1. Sélectionnez la vue de Collection et de sa classe de la valeur `CityCollectionView` dans les **onglet Widget**: 

    [![](collection-views-images/collection08vs.png "La valeur de la classe CityCollectionView")](collection-views-images/collection08vs.png#lightbox)
1. Sélectionnez la cellule de la vue de Collection et de sa classe de la valeur `CityCollectionViewCell` dans les **onglet Widget**: 

    [![](collection-views-images/collection09vs.png "La valeur de la classe CityCollectionViewCell")](collection-views-images/collection09vs.png#lightbox)
1. Dans le **onglet Widget** vous assurer que le **disposition** est `Flow` et **sens de défilement** est `Vertical` pour la vue de Collection : 

    [![](collection-views-images/collection10vs.png "Onglet estimée Widget")](collection-views-images/collection10vs.png#lightbox)
1. Sélectionnez la cellule de la vue de Collection et définissez son **identité** à `CityCell` dans les **onglet Widget**: 

    [![](collection-views-images/collection11vs.png "Définissez l’identité sur CityCell")](collection-views-images/collection11vs.png#lightbox)
1. Enregistrez les modifications apportées.
    

-----

Si nous avions choisi `Custom` pour la vue de Collection **disposition**, nous pourrions avoir spécifié une disposition personnalisée. Apple offre intégré `UICollectionViewFlowLayout` et `UICollectionViewDelegateFlowLayout` qui peut facilement présenter des données dans une disposition Grille (ils sont utilisés par le `flow` style de disposition). 

Pour plus d’informations sur l’utilisation des plans conceptuels, veuillez consulter notre [Hello, Guide de démarrage rapide de tvOS](~/ios/tvos/get-started/hello-tvos.md).

<a name="Providing-Data-for-the-Collection-View" />

## <a name="providing-data-for-the-collection-view"></a>Fournit les données pour la vue de Collection

Maintenant que nous avons notre vue de Collection (et la Collection-View-Controller) ajouté à notre plan conceptuel, nous devons fournir les données de la collection. 

<a name="The-Data-Model" />

### <a name="the-data-model"></a>Le modèle de données

Tout d’abord, nous allons créer un modèle pour nos données qui contient le nom de fichier pour l’image à afficher, le titre et un indicateur pour autoriser la ville à sélectionner.

Créer un `CityInfo` de classe et le rendre l’aspect suivant :

```csharp
using System;

namespace tvCollection
{
    public class CityInfo
    {
        #region Computed Properties
        public string ImageFilename { get; set; }
        public string Title { get; set; }
        public bool CanSelect{ get; set; }
        #endregion

        #region Constructors
        public CityInfo (string filename, string title, bool canSelect)
        {
            // Initialize
            this.ImageFilename = filename;
            this.Title = title;
            this.CanSelect = canSelect;
        }
        #endregion
    }
}
```

### <a name="the-collection-view-cell"></a>La cellule de vue de Collection

Nous devons maintenant définir la façon dont les données seront affiche pour chaque cellule. Modifier la `CityCollectionViewCell.cs` fichier (créée automatiquement à partir de votre fichier d’animation) et le rendre l’aspect suivant :

```csharp
using System;
using Foundation;
using UIKit;
using CoreGraphics;

namespace tvCollection
{
    public partial class CityCollectionViewCell : UICollectionViewCell
    {
        #region Private Variables
        private CityInfo _city;
        #endregion

        #region Computed Properties
        public UIImageView CityView { get ; set; }
        public UILabel CityTitle { get; set; }

        public CityInfo City {
            get { return _city; }
            set {
                _city = value;
                CityView.Image = UIImage.FromFile (City.ImageFilename);
                CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
                CityTitle.Text = City.Title;
            }
        }
        #endregion

        #region Constructors
        public CityCollectionViewCell (IntPtr handle) : base (handle)
        {
            // Initialize
            CityView = new UIImageView(new CGRect(22, 19, 320, 171));
            CityView.AdjustsImageWhenAncestorFocused = true;
            AddSubview (CityView);

            CityTitle = new UILabel (new CGRect (22, 209, 320, 21)) {
                TextAlignment = UITextAlignment.Center,
                TextColor = UIColor.White,
                Alpha = 0.0f
            };
            AddSubview (CityTitle);
        }
        #endregion
    

    }
}
```

Pour notre application tvOS, nous affichera une image et un titre facultatif. Si la ville donnée ne peut pas être sélectionnée, nous allons estompage l’affichage de l’image à l’aide du code suivant :

```csharp
CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
```

Lorsque la cellule contenant l’image soit actif par l’utilisateur, nous voulons utiliser la fonction intégrée l’effet parallaxe sur ce dernier en définissant la propriété suivante :

```csharp
CityView.AdjustsImageWhenAncestorFocused = true;
```

Pour plus d’informations sur la Navigation et le Focus, consultez notre [utilisation de Navigation et le Focus](~/ios/tvos/app-fundamentals/navigation-focus.md) et [Siri distants et les contrôleurs Bluetooth](~/ios/tvos/platform/remote-bluetooth.md) documentation.


<a name="The-Collection-View-Data-Provider" />

### <a name="the-collection-view-data-provider"></a>Le fournisseur de données de vue de Collection

Avec notre modèle de données créé et notre disposition cellule définie, nous allons créer une Source de données pour la vue de Collection. La Source de données sera responsable non seulement les données de sauvegarde, mais également dépilage les cellules pour afficher les cellules individuelles à l’écran.

Créer un `CityViewDatasource` de classe et le rendre l’aspect suivant :

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;
using ObjCRuntime;

namespace tvCollection
{
    public class CityViewDatasource : UICollectionViewDataSource
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Static Constants
        public static NSString CardCellId = new NSString ("CityCell");
        #endregion

        #region Computed Properties
        public List<CityInfo> Cities { get; set; } = new List<CityInfo>();
        public CityCollectionView ViewController { get; set; }
        #endregion

        #region Constructors
        public CityViewDatasource (CityCollectionView controller)
        {
            // Initialize
            this.ViewController = controller;
            PopulateCities ();
        }
        #endregion

        #region Public Methods
        public void PopulateCities() {

            // Clear existing cities
            Cities.Clear();

            // Add new cities
            Cities.Add(new CityInfo("City01.jpg", "Houses by Water", false));
            Cities.Add(new CityInfo("City02.jpg", "Turning Circle", true));
            Cities.Add(new CityInfo("City03.jpg", "Skyline at Night", true));
            Cities.Add(new CityInfo("City04.jpg", "Golden Gate Bridge", true));
            Cities.Add(new CityInfo("City05.jpg", "Roads by Night", true));
            Cities.Add(new CityInfo("City06.jpg", "Church Domes", true));
            Cities.Add(new CityInfo("City07.jpg", "Mountain Lights", true));
            Cities.Add(new CityInfo("City08.jpg", "City Scene", false));
            Cities.Add(new CityInfo("City09.jpg", "House in Winter", true));
            Cities.Add(new CityInfo("City10.jpg", "By the Lake", true));
            Cities.Add(new CityInfo("City11.jpg", "At the Dome", true));
            Cities.Add(new CityInfo("City12.jpg", "Cityscape", true));
            Cities.Add(new CityInfo("City13.jpg", "Model City", true));
            Cities.Add(new CityInfo("City14.jpg", "Taxi, Taxi!", true));
            Cities.Add(new CityInfo("City15.jpg", "On the Sidewalk", true));
            Cities.Add(new CityInfo("City16.jpg", "Midnight Walk", true));
            Cities.Add(new CityInfo("City17.jpg", "Lunchtime Cafe", true));
            Cities.Add(new CityInfo("City18.jpg", "Coffee Shop", true));
            Cities.Add(new CityInfo("City19.jpg", "Rustic Tavern", true));
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView)
        {
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section)
        {
            return Cities.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            var cityCell = (CityCollectionViewCell)collectionView.DequeueReusableCell (CardCellId, indexPath);
            var city = Cities [indexPath.Row];

            // Initialize city
            cityCell.City = city;

            return cityCell;
        }
        #endregion
    }
}
```

Permettent d’examiner cette classe en détail. Tout d’abord, nous hériter `UICollectionViewDataSource` et fournir un raccourci à l’ID de cellules (que nous avons attribué dans le concepteur iOS) :

```csharp
public static NSString CardCellId = new NSString ("CityCell");
```

Ensuite, nous fournir un stockage pour nos données de collection et fournir une classe pour remplir les données :

```csharp
public List<CityInfo> Cities { get; set; } = new List<CityInfo>();
...

public void PopulateCities() {

    // Clear existing cities
    Cities.Clear();

    // Add new cities
    Cities.Add(new CityInfo("City01.jpg", "Houses by Water", false));
    Cities.Add(new CityInfo("City02.jpg", "Turning Circle", true));
    ...
}
```

Ensuite, nous remplaçons le `NumberOfSections` (méthode) et retourner le nombre de sections (groupes d’éléments) qui permet d’afficher la Collection a. Dans ce cas, il n'existe qu’un seul :

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    return 1;
}
```

Ensuite, nous renvoyons le nombre d’éléments dans la collection à l’aide du code suivant :

```csharp
public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    return Cities.Count;
}
```

Enfin, nous dequeue une cellule réutilisable lors de la demande de la vue de Collection avec le code suivant :

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    var cityCell = (CityCollectionViewCell)collectionView.DequeueReusableCell (CardCellId, indexPath);
    var city = Cities [indexPath.Row];

    // Initialize city
    cityCell.City = city;

    return cityCell;
}
```

Une fois que nous obtenons une cellule de la vue de Collection de notre `CityCollectionViewCell` type, nous remplir avec l’élément donné.

<a name="Responding-to-User-Events" />

## <a name="responding-to-user-events"></a>Répondre aux événements utilisateur

Comme vous souhaitez que l’utilisateur soit en mesure de sélectionner un élément dans la collection, nous devons fournir un délégué de la vue de Collection pour gérer cette interaction. Et nous devons fournir un moyen d’informer les notre vue appelante quel élément de l’utilisateur a sélectionné.

<a name="The-App-Delegate" />

### <a name="the-app-delegate"></a>Le délégué de l’application

Nous avons besoin d’un moyen pour associer l’élément actuellement sélectionné à partir de la vue de Collection à la vue appelante. Nous utiliserons une propriété personnalisée sur notre `AppDelegate`. Modifier le `AppDelegate.cs` et ajoutez le code suivant :

```csharp
public CityInfo SelectedCity { get; set;} = new CityInfo("City02.jpg", "Turning Circle", true);
```

Cela définit la propriété et définit la ville par défaut qui sera affichée initialement. Nous allons consommer ultérieurement, cette propriété pour afficher la sélection de l’utilisateur et de permettre la sélectionner à modifier.

<a name="The-Collection-View-Delegate" />

### <a name="the-collection-view-delegate"></a>Le délégué de vue de Collection

Ensuite, ajoutez un nouveau `CityViewDelegate` classe au projet et rendre l’aspect suivant :


```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;

namespace tvCollection
{
    public class CityViewDelegate : UICollectionViewDelegateFlowLayout
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        public CityViewDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
        {
            return new CGSize (361, 256);
        }

        public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
        {
            if (indexPath == null) {
                return false;
            } else {
                var controller = collectionView as CityCollectionView;
                return controller.Source.Cities[indexPath.Row].CanSelect;
            }
        }

        public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
        {
            var controller = collectionView as CityCollectionView;
            App.SelectedCity = controller.Source.Cities [indexPath.Row];

            // Close Collection
            controller.ParentController.DismissViewController(true,null);
        }
        #endregion
    }
}
```

Jetons un œil plus près à cette classe. Tout d’abord, nous hériter `UICollectionViewDelegateFlowLayout`. La raison pour laquelle nous héritent de cette classe et non le `UICollectionViewDelegate` est que nous utilisons la fonction intégrée `UICollectionViewFlowLayout` pour présenter les éléments et non un type de disposition personnalisée.

Ensuite, nous retourner la taille pour les éléments individuels à l’aide de ce code :

```csharp
public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
{
    return new CGSize (361, 256);
}
```

Ensuite, vous décidez si une cellule donnée peut obtenir le focus à l’aide du code suivant : 

```csharp
public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
{
    if (indexPath == null) {
        return false;
    } else {
        var controller = collectionView as CityCollectionView;
        return controller.Source.Cities[indexPath.Row].CanSelect;
    }
}
```

Nous vérifions si un élément de données de sauvegarde a son `CanSelect` indicateur défini sur `true` et retourne cette valeur. Pour plus d’informations sur la Navigation et le Focus, consultez notre [utilisation de Navigation et le Focus](~/ios/tvos/app-fundamentals/navigation-focus.md) et [Siri distants et les contrôleurs Bluetooth](~/ios/tvos/platform/remote-bluetooth.md) documentation.

Enfin, nous répondre à l’utilisateur de sélectionner un élément avec le code suivant :

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    var controller = collectionView as CityCollectionView;
    App.SelectedCity = controller.Source.Cities [indexPath.Row];

    // Close Collection
    controller.ParentController.DismissViewController(true,null);
}
```

Ici, nous avons défini le `SelectedCity` propriété de notre `AppDelegate` à l’élément que l’utilisateur a sélectionné et nous fermer le contrôleur de vue de Collection, retourner à la vue qui nous appelée. Nous n’avons pas défini le `ParentController` propriété notre de vue de Collection encore, nous allons faire autrement ensuite.

<a name="Configuring-the-Collection-View" />

## <a name="configuring-the-collection-view"></a>Configuration de la vue de Collection

Nous devons maintenant modifier la vue de Collection et affecter notre Source de données et d’un délégué. Modifier la `CityCollectionView.cs` fichier (créée automatiquement pour vous à partir de notre plan conceptuel) et le rendre l’aspect suivant :

```csharp
using System;
using Foundation;
using UIKit;

namespace tvCollection
{
    public partial class CityCollectionView : UICollectionView
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public CityViewDatasource Source {
            get { return DataSource as CityViewDatasource;}
        }

        public CityCollectionViewController ParentController { get; set;}
        #endregion

        #region Constructors
        public CityCollectionView (IntPtr handle) : base (handle)
        {
            // Initialize
            RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
            DataSource = new CityViewDatasource (this);
            Delegate = new CityViewDelegate ();
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections ()
        {
            return 1;
        }

        public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
        {
            var previousItem = context.PreviouslyFocusedView as CityCollectionViewCell;
            if (previousItem != null) {
                Animate (0.2, () => {
                    previousItem.CityTitle.Alpha = 0.0f;
                });
            }

            var nextItem = context.NextFocusedView as CityCollectionViewCell;
            if (nextItem != null) {
                Animate (0.2, () => {
                    nextItem.CityTitle.Alpha = 1.0f;
                });
            }
        }
        #endregion
    }
}
```

Tout d’abord, nous fournissons un raccourci pour accéder à notre `AppDelegate`: 

```csharp
public static AppDelegate App {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
```

Ensuite, nous fournissons un raccourci pour la Source de données de la vue de Collection et une propriété pour accéder au contrôleur de vue de Collection (utilisé par notre délégué ci-dessus pour fermer la collection lorsque l’utilisateur effectue une sélection) :

```csharp
public CityViewDatasource Source {
    get { return DataSource as CityViewDatasource;}
}

public CityCollectionViewController ParentController { get; set;}
```

Ensuite, nous utilisons le code suivant pour initialiser la vue de Collection et affecter notre classe de cellule, la Source de données et le délégué :

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    DataSource = new CityViewDatasource (this);
    Delegate = new CityViewDelegate ();
}
```

Enfin, nous souhaitons le titre de l’image soit visible uniquement lorsque l’utilisateur a mis en surbrillance (actif). Cela se fait par le code suivant :

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    var previousItem = context.PreviouslyFocusedView as CityCollectionViewCell;
    if (previousItem != null) {
        Animate (0.2, () => {
            previousItem.CityTitle.Alpha = 0.0f;
        });
    }

    var nextItem = context.NextFocusedView as CityCollectionViewCell;
    if (nextItem != null) {
        Animate (0.2, () => {
            nextItem.CityTitle.Alpha = 1.0f;
        });
    }
}
```

Nous avons défini la perméabilité de l’élément précédent perd le focus à zéro (0) et la perméabilité de l’élément suivant obtenir le focus à 100 %. Ces transition obtenir également animés.


## <a name="configuring-the-collection-view-controller"></a>Configuration du contrôleur de vue de Collection

Maintenant, nous devons effectuer la configuration finale sur notre vue de Collection et d’autoriser le contrôleur définir la propriété que nous avons défini pour la vue de Collection peut être fermée une fois que l’utilisateur effectue une sélection.

Modifier la `CityCollectionViewController.cs` fichier (créé automatiquement à partir de notre plan conceptuel) et le rendre l’aspect suivant :

```csharp
// This file has been autogenerated from a class added in the UI designer.

using System;

using Foundation;
using UIKit;

namespace tvCollection
{
    public partial class CityCollectionViewController : UICollectionViewController
    {
        #region Computed Properties
        public CityCollectionView Collection {
            get { return CollectionView as CityCollectionView; }
        }
        #endregion

        #region Constructors
        public CityCollectionViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Save link to controller
            Collection.ParentController = this;
        }
        #endregion
    }
}

```

## <a name="putting-it-all-together"></a>Assemblage 

Maintenant que nous avons tous les éléments mis au point pour remplir et contrôler la vue de Collection, nous devons effectuer les modifications finales dans notre vue principale pour rassembler tous les éléments.

Modifier la `ViewController.cs` fichier (créé automatiquement à partir de notre plan conceptuel) et le rendre l’aspect suivant :

```csharp
using System;
using Foundation;
using UIKit;
using tvCollection;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update image with the currently selected one
            CityView.Image = UIImage.FromFile(App.SelectedCity.ImageFilename);
            BackgroundView.Image = CityView.Image;
            CityTitle.Text = App.SelectedCity.Title;
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion
    }
}
```

Le code suivant affiche l’élément sélectionné de la `SelectedCity` propriété de la `AppDelegate` et elle affiche à nouveau lorsque l’utilisateur a effectué une sélection à partir de la vue de Collection :

```csharp
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update image with the currently selected one
    CityView.Image = UIImage.FromFile(App.SelectedCity.ImageFilename);
    BackgroundView.Image = CityView.Image;
    CityTitle.Text = App.SelectedCity.Title;
}
```

<a name="Testing-the-app" />

## <a name="testing-the-app"></a>Test de l’application

Avec tous les éléments en place, si vous générez et exécutez l’application, la vue principale s’affiche avec la ville par défaut :

[![](collection-views-images/run01.png "L’écran principal")](collection-views-images/run01.png#lightbox)

Si l’utilisateur cliquer sur le **sélectionner une vue** bouton, la vue de Collection s’affichera :

[![](collection-views-images/run02.png "La vue de collection")](collection-views-images/run02.png#lightbox)

Une ville qui a son `CanSelect` propriété la valeur `false` apparaît grisé alors que l’utilisateur ne sera pas en mesure de définir le focus à ce dernier. Lorsque l’utilisateur met en surbrillance un élément (rendre actif) le titre est affiché et ils peuvent utiliser l’effet parallaxe à l’inclinaison plusieurs l’image en 3D.

Lorsque l’utilisateur clique sur une image de sélection, la vue de Collection est fermée et la vue principale s’affiche de nouveau avec la nouvelle image :

[![](collection-views-images/run03.png "Une nouvelle image sur l’écran d’accueil")](collection-views-images/run03.png#lightbox)

<a name="Creating-Custom-Layout-and-Reordering-Items" />

## <a name="creating-custom-layout-and-reordering-items"></a>Création d’une disposition personnalisée et la réorganisation des éléments

Une des fonctionnalités clés de l’utilisation d’une vue de Collection est la capacité de créer des dispositions personnalisées. Étant donné que tvOS hérite d’iOS, le processus de création d’une disposition personnalisée est le même. Veuillez consulter notre [Introduction aux vues de Collection](~/ios/user-interface/controls/uicollectionview.md) documentation pour plus d’informations.

Récemment ajoutés aux vues de Collection pour iOS 9 a la possibilité de facilitent la réorganisation des éléments dans la collection. Là encore, tvOS 9 étant un sous-ensemble d’iOS 9, cela les même façon. Veuillez consulter notre [afficher les modifications de Collection](~/ios/user-interface/controls/uicollectionview.md) document pour plus d’informations.


<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a couvert conception et utilisation des vues de Collection à l’intérieur d’une application Xamarin.tvOS. Tout d’abord, il décrit tous les éléments qui composent la vue de Collection. Ensuite, elle a montré comment concevoir et implémenter une vue de Collection à l’aide d’un plan conceptuel. Enfin, est fourni des liens vers des informations sur la création des dispositions personnalisées et de réorganiser les éléments.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS les Guides Interface humaine](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
