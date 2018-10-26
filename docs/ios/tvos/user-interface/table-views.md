---
title: Utilisation de tvOS des affichages de tableau dans Xamarin
description: Cet article décrit la conception et l’utilisation des vues des tables et des contrôleurs d’affichage de tableau à l’intérieur d’une application Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: D8F80FA9-6400-4DB7-AFC9-A28A54AD04E8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: a8b31d9f2e9c8921420b1fd50c4092273962f54b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105894"
---
# <a name="working-with-tvos-table-views-in-xamarin"></a>Utilisation de tvOS des affichages de tableau dans Xamarin

_Cet article décrit la conception et l’utilisation des vues des tables et des contrôleurs d’affichage de tableau à l’intérieur d’une application Xamarin.tvOS._

Dans tvOS, une vue de Table est présentée comme une seule colonne de défilement de lignes qui peuvent éventuellement être organisés en groupes ou des sections. Vues de table doivent être utilisées lorsque vous avez besoin afficher une grande quantité de données efficacement à l’utilisateur, en un clair pour comprendre la façon.

Vues de table sont généralement affichés dans un côté d’un [mode fractionné](~/ios/tvos/user-interface/split-views.md) en tant que le volet de navigation, avec les détails de l’élément sélectionné est affiché dans le côté opposé :

[![](table-views-images/intro01.png "Exemple de vue de table")](table-views-images/intro01.png#lightbox)

<a name="About-Table-Views" />

## <a name="about-table-views"></a>Sur les vues de Table

Un `UITableView` affiche une seule colonne de lignes de défilement sous la forme d’une liste hiérarchique des informations qui peuvent éventuellement être organisées en groupes ou des sections : 

[![](table-views-images/table01.png "Un élément sélectionné")](table-views-images/table01.png#lightbox)

Apple a les suggestions suivantes pour utiliser des tables :

- **Être conscient de la largeur** -tentent de trouver l’équilibre approprié compte tenu de largeurs de votre table. Si la table est trop large, il peut être difficile à analyser à distance et peut prendre en dehors de la zone de contenu disponible. Si la table est trop étroite, cela peut provoquer les informations seront tronquées ou retour à la ligne, à nouveau ce peut être difficile pour l’utilisateur qui lit à travers la pièce.
- **Afficher le contenu de Table rapidement** : pour les grandes listes de données, le contenu du chargement différé et commencer à afficher des informations dès que la table est présentée à l’utilisateur. Si la table prend de temps à se charger, l’utilisateur peut perdre qui vous intéresse dans votre application ou de la réflexion, qu'il est verrouillé.
- **Informer l’utilisateur de longs chargements contenu** : si un temps de chargement du tableau long est inévitable, présente un [barre de progression ou d’un indicateur d’activité](~/ios/tvos/user-interface/progress-indicators.md) afin qu’ils connaissent l’application n’a pas verrouillé.

<a name="Table-Cell-Types" />

## <a name="table-view-cell-types"></a>Types de cellule d’affichage table

Un `UITableViewCell` est utilisé pour représenter les lignes de données dans la vue de Table individuelles. Apple a défini plusieurs Types de cellule de Table par défaut :

- **Par défaut** - cette présente de type une option de l’Image sur le côté gauche de la cellule et le titre d’aligné à gauche à droite. 
- **Sous-titre** - cette présente type titre aligné à gauche sur la première ligne et une plus petite aligné à gauche sous-titre sur la ligne suivante.
- **La valeur 1** -ce type présente un titre aligné à gauche et un sous-titre de couleur plus claire, aligné à droite sur la même ligne.
- **La valeur 2** -ce type présente un titre aligné à droite et un sous-titre de couleur plus claire, aligné à gauche sur la même ligne.

Tous les Types de cellule de vue de Table par défaut prennent également en charge les éléments graphiques tels que les indicateurs de la divulgation ou coches. 

En outre, vous pouvez définir un **personnalisé** Type de cellule de Table de vue et présente un _Prototype cellule_, que vous créez dans le Concepteur d’Interface ou par le biais de code.

Apple a les suggestions suivantes pour travailler avec des cellules de vue de Table :

- **Éviter le découpage du texte** -conserver des lignes de texte courte afin qu’ils ne se retrouvent pas tronquée. Tronquée mots ou expressions sont difficiles à l’utilisateur de l’analyse à partir de la salle.
- **Prendre en compte l’état de ligne de focus** - parce qu’une ligne devient plus important, et bien plus encore arrondi des angles en cas de focus, que vous avez besoin tester l’apparence de votre de la cellule dans tous les États. Images ou du texte peut devenir tronqué ou regarder incorrect dans l’état focus.
- **Utilisez modifiable Tables avec parcimonie** -déplacement ou la suppression de lignes de la Table est plus beaucoup de temps sur tvOS à iOS. Vous devez déterminer avec soin si cette fonctionnalité ajoutera ou d’un point à partir de votre application tvOS.
- **Création personnalisée cellule Types où approprié** : alors que les Types de cellule d’affichage Table intégrés sont idéales dans de nombreuses situations, envisagez de créer des Types de cellule personnalisée pour des informations non standard pour offrir un meilleur contrôle et mieux présenter les informations à l’utilisateur.

<a name="Working-With-Table-Views" />

## <a name="working-with-table-views"></a>Utilisation des vues de Table

Pour utiliser des vues de Table dans une application Xamarin.tvOS le plus simple consiste à créer et modifier leur apparence dans le Concepteur d’Interface.

Pour commencer, effectuez les étapes suivantes :

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)
    
1. Dans Visual Studio pour Mac, démarrez un nouveau projet d’application tvOS et sélectionnez **tvOS** > **application** > **application avec affichage unique** et cliquez sur le  **Suivant** bouton : 

    [![](table-views-images/table02.png "Sélectionnez l’application avec affichage unique")](table-views-images/table02.png#lightbox)
1. Entrez un **nom** pour l’application et cliquez sur **suivant**: 

    [![](table-views-images/table03.png "Entrez un nom pour l’application")](table-views-images/table03.png#lightbox)
1. Soit ajuster le **nom_projet** et **nom de la Solution** ou accepter les valeurs par défaut et cliquez sur le **créer** bouton permettant de créer la nouvelle solution : 

    [![](table-views-images/table04.png "Le nom du projet et le nom de la Solution")](table-views-images/table04.png#lightbox)
1. Dans le **panneau solutions**, double-cliquez sur le `Main.storyboard` fichier pour l’ouvrir dans le concepteur iOS : 

    [![](table-views-images/table05.png "Fichier Main.storyboard")](table-views-images/table05.png#lightbox)
1. Sélectionnez et supprimez le **contrôleur d’affichage par défaut**: 

    [![](table-views-images/table06.png "Sélectionnez et supprimez le contrôleur d’affichage par défaut")](table-views-images/table06.png#lightbox)
1. Sélectionnez un **contrôleur d’affichage fractionné** à partir de la **boîte à outils** et faites-le glisser sur l’aire de conception.
1. Par défaut, vous obtiendrez un [mode fractionné](~/ios/tvos/user-interface/split-views.md) avec un **contrôleur d’affichage de Navigation** et un **contrôleur d’affichage Table** dans la partie gauche et un **contrôleur d’affichage** dans la partie droite. Il s’agit de l’utilisation de suggestions d’Apple d’une vue de Table dans tvOS : 

    [![](table-views-images/table08.png "Ajouter un mode fractionné")](table-views-images/table08.png#lightbox)
1. Vous devez sélectionner chaque partie de la vue de Table et attribuez-lui un personnalisé **nom de la classe** dans le **Widget** onglet de la **l’Explorateur de propriétés** afin que vous pouvez y accéder ultérieurement dans C#code. Par exemple, le **contrôleur d’affichage Table**: 

    [![](table-views-images/table09.png "Attribuer un nom de classe")](table-views-images/table09.png#lightbox)
1. Vérifiez que vous créez une classe personnalisée pour le **contrôleur d’affichage Table**, le **Table vue** et n’importe quel **Prototype cellules**. Visual Studio pour Mac ajoute les classes personnalisées à l’arborescence du projet lors de leur création : 

    [![](table-views-images/table10.png "Les classes personnalisées dans l’arborescence du projet")](table-views-images/table10.png#lightbox)
1. Ensuite, sélectionnez la vue de Table dans l’aire de conception et ajuster ses propriétés en fonction des besoins. Comme le nombre de **Prototype cellules** et **Style** (brut ou groupées) : 

    [![](table-views-images/table11.png "L’onglet de widget")](table-views-images/table11.png#lightbox)
1. Pour chaque **Prototype cellule**, sélectionnez-le, puis attribuer une valeur unique **identificateur** dans le **Widget** onglet de la **l’Explorateur de propriétés**. Cette étape est _très important_ car vous en aurez besoin cet identificateur ultérieurement lorsque vous remplissez la table. Par exemple `AttrCell`: 

    [![](table-views-images/table12.png "L’onglet de Widget")](table-views-images/table12.png#lightbox)
1. Vous pouvez également sélectionner pour présenter la cellule comme l’un de la [par défaut des Types de cellule d’affichage de tableau](#Table-View-Cell-Types) via la **Style** liste déroulante ou affectez-lui la valeur **personnalisé** et utiliser l’aire de conception à disposition de la cellule en faisant glisser dans les autres widgets d’interface utilisateur à partir de la **boîte à outils**: 

    [![](table-views-images/table13.png "La disposition des cellules")](table-views-images/table13.png#lightbox)
1. Affecter une valeur unique **nom** à chaque élément d’interface utilisateur dans la conception de la cellule de Prototype dans le **Widget** onglet de la **l’Explorateur de propriétés** donc vous pouvez y accéder ultérieurement dans C# code : 

    [![](table-views-images/table14.png "Attribuer un nom")](table-views-images/table14.png#lightbox)
1. Répétez l’étape ci-dessus pour toutes les cellules de Prototype dans la vue de Table.
1. Ensuite, affectez des classes personnalisées pour le reste de votre conception d’interface utilisateur, la disposition de l’affichage des détails et l’affecter unique **noms** à chaque élément d’interface utilisateur dans les détails de vue afin que vous pouvez y accéder dans C# également. Par exemple : 

    [![](table-views-images/table15.png "La disposition de l’interface utilisateur")](table-views-images/table15.png#lightbox)
1. Enregistrez vos modifications dans la table de montage séquentiel.
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. Dans Visual Studio, démarrez un nouveau projet d’application tvOS et sélectionnez **tvOS** > **application avec affichage unique** et entrez un nom pour votre application. Cliquez sur le **OK** bouton pour créer une nouvelle solution : 

    [![](table-views-images/table02-vs.png "Sélectionnez l’application avec affichage unique")](table-views-images/table02-vs.png#lightbox)
1. Dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` fichier pour l’ouvrir dans le concepteur iOS : 

    [![](table-views-images/table05-vs.png "Fichier Main.storyboard")](table-views-images/table05-vs.png#lightbox)
1. Sélectionnez et supprimez le **contrôleur d’affichage par défaut**: 

    [![](table-views-images/table06-vs.png "Sélectionnez et supprimez le contrôleur d’affichage par défaut")](table-views-images/table06-vs.png#lightbox)
1. Sélectionnez un **contrôleur d’affichage fractionné** à partir de la **boîte à outils** et faites-le glisser sur l’aire de conception : 

    [![](table-views-images/table07-vs.png "Un contrôleur d’affichage fractionné")](table-views-images/table07-vs.png#lightbox)
1. Par défaut, vous obtiendrez un [mode fractionné](~/ios/tvos/user-interface/split-views.md) avec un **contrôleur d’affichage de Navigation** et un **contrôleur d’affichage Table** dans la partie gauche et un **contrôleur d’affichage** dans la partie droite. Il s’agit de l’utilisation de suggestions d’Apple d’une vue de Table dans tvOS : 

    [![](table-views-images/table08-vs.png "Disposition de l’interface utilisateur")](table-views-images/table08-vs.png#lightbox)
1. Vous devez sélectionner chaque partie de la vue de Table et attribuez-lui un personnalisé **nom de la classe** dans le **Widget** onglet de la **l’Explorateur de propriétés** afin que vous pouvez y accéder ultérieurement dans C#code. Par exemple, le **contrôleur d’affichage Table**: 

    [![](table-views-images/table09-vs.png "L’onglet de Widget")](table-views-images/table09-vs.png#lightbox)
1. Vérifiez que vous créez une classe personnalisée pour le **contrôleur d’affichage Table**, le **Table vue** et n’importe quel **Prototype cellules**. Visual Studio pour Mac ajoute les classes personnalisées à l’arborescence du projet lors de leur création : 

    [![](table-views-images/table10-vs.png "Les classes personnalisées dans l’arborescence du projet")](table-views-images/table10-vs.png#lightbox)
1. Ensuite, sélectionnez la vue de Table dans l’aire de conception et ajuster ses propriétés en fonction des besoins. Comme le nombre de **Prototype cellules** et **Style** (brut ou groupées) : 

    [![](table-views-images/table11-vs.png "L’onglet de Widget")](table-views-images/table11-vs.png#lightbox)
1. Pour chaque **Prototype cellule**, sélectionnez-le, puis attribuer une valeur unique **identificateur** dans le **Widget** onglet de la **l’Explorateur de propriétés**. Cette étape est _très important_ car vous en aurez besoin cet identificateur ultérieurement lorsque vous remplissez la table. Par exemple `AttrCell`: 

    [![](table-views-images/table12-vs.png "Affecter un identificateur")](table-views-images/table12-vs.png#lightbox)
1. Vous pouvez également sélectionner pour présenter la cellule comme l’un de la [par défaut des Types de cellule d’affichage de tableau](#Table-View-Cell-Types) via la **Style** liste déroulante ou affectez-lui la valeur **personnalisé** et utiliser l’aire de conception à disposition de la cellule en faisant glisser dans les autres widgets d’interface utilisateur à partir de la **boîte à outils**: 

    [![](table-views-images/table13-vs.png "Le menu déroulant de Style")](table-views-images/table13-vs.png#lightbox)
1. Affecter une valeur unique **nom** à chaque élément d’interface utilisateur dans la conception de la cellule de Prototype dans le **Widget** onglet de la **l’Explorateur de propriétés** donc vous pouvez y accéder ultérieurement dans C# code : 

    [![](table-views-images/table14-vs.png "L’onglet de Widget")](table-views-images/table14-vs.png#lightbox)
1. Répétez l’étape ci-dessus pour toutes les cellules de Prototype dans la vue de Table.
1. Ensuite, affectez des classes personnalisées pour le reste de votre conception d’interface utilisateur, la disposition de l’affichage des détails et l’affecter unique **noms** à chaque élément d’interface utilisateur dans les détails de vue afin que vous pouvez y accéder dans C# également. Par exemple : 

    [![](table-views-images/table15.png "La disposition de l’interface utilisateur")](table-views-images/table15.png#lightbox)
1. Enregistrez vos modifications dans la table de montage séquentiel.
    
-----

<a name="Designing-a-Data-Model" />

## <a name="designing-a-data-model"></a>Conception d’un modèle de données

Pour rendre l’utilisation avec les informations de l’affichage de Table affiche plus facile et faciliter la présentation d’informations détaillées (comme l’utilisateur sélectionne ou met en surbrillance des lignes dans la vue Table), créez une classe personnalisée ou des classes d’agir en tant que le modèle de données pour les informations présentées .

Prenons l’exemple d’une application de réservation de voyages qui contient une liste de **villes**, chacun contenant une liste unique de **Attractions** que l’utilisateur peut sélectionner. L’utilisateur sera en mesure de marquer une attraction comme un *favori*, sélectionnez cette option pour obtenir *Directions* pour une utilisation sur et *livre un vol* dans une ville donnée.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Pour créer le modèle de données pour un **Attraction**, avec le bouton droit sur le nom du projet dans le **panneau solutions** et sélectionnez **ajouter** > **nouveau fichier...** . Entrez `AttractionInformation` pour le **nom** et cliquez sur le **New** bouton : 

[![](table-views-images/data01.png "Entrez AttractionInformation pour le nom")](table-views-images/data01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Pour créer le modèle de données pour un **Attraction**, avec le bouton droit sur le nom du projet dans le **l’Explorateur de solutions** et sélectionnez **ajouter** > **un nouvel élément ...** . Sélectionnez **classe** et entrez `AttractionInformation` pour le **nom** et cliquez sur le **ajouter** bouton : 

[![](table-views-images/data01-vs.png "Sélectionnez la classe et entrez AttractionInformation pour le nom")](table-views-images/data01-vs.png#lightbox)

-----

Modifier la `AttractionInformation.cs` de fichier et qu’elle ressemble à ce qui suit :

```csharp
using System;
using Foundation;

namespace tvTable
{
    public class AttractionInformation : NSObject
    {
        #region Computed Properties
        public CityInformation City { get; set;}
        public string Name { get; set;}
        public string Description { get; set;}
        public string ImageName { get; set;}
        public bool IsFavorite { get; set;}
        public bool AddDirections { get; set;}
        #endregion

        #region Constructors
        public AttractionInformation (string name, string description, string imageName)
        {
            // Initialize
            this.Name = name;
            this.Description = description;
            this.ImageName = imageName;
        }
        #endregion
    }
}
```

Cette classe fournit les propriétés pour stocker les informations sur une donnée **Attraction**.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Ensuite, avec le bouton droit sur le nom du projet dans le **panneau solutions** à nouveau et sélectionnez **ajouter** > **nouveau fichier...** . Entrez `CityInformation` pour le **nom** et cliquez sur le **New** bouton : 

[![](table-views-images/data02.png "Entrez CityInformation pour le nom")](table-views-images/data02.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Ensuite, avec le bouton droit sur le nom du projet dans le **l’Explorateur de solutions** à nouveau et sélectionnez **ajouter** > **un nouvel élément...** . Entrez `CityInformation` pour le **nom** et cliquez sur le **ajouter** bouton : 

[![](table-views-images/data02-vs.png "Entrez CityInformation pour le nom")](table-views-images/data02-vs.png#lightbox)

-----

Modifier la `CityInformation.cs` de fichier et qu’elle ressemble à ce qui suit :

```csharp
using System;
using System.Collections.Generic;
using Foundation;

namespace tvTable
{
    public class CityInformation : NSObject
    {
        #region Computed Properties
        public string Name { get; set; }
        public List<AttractionInformation> Attractions { get; set;}
        public bool FlightBooked { get; set;}
        #endregion

        #region Constructors
        public CityInformation (string name)
        {
            // Initialize
            this.Name = name;
            this.Attractions = new List<AttractionInformation> ();
        }
        #endregion

        #region Public Methods
        public void AddAttraction (AttractionInformation attraction)
        {
            // Mark as belonging to this city
            attraction.City = this;

            // Add to collection
            Attractions.Add (attraction);
        }

        public void AddAttraction (string name, string description, string imageName)
        {
            // Create attraction
            var attraction = new AttractionInformation (name, description, imageName);

            // Mark as belonging to this city
            attraction.City = this;

            // Add to collection
            Attractions.Add (attraction);
        }
        #endregion
    }
}
```

Cette classe conserve toutes les informations concernant une destination **Ville**, une collection de **Attractions** de cette ville et fournit deux méthodes d’assistance (`AddAttraction`) pour le rendre plus facile d’ajouter des centres d’intérêt à la Ville.

<a name="The-Table-Data-Source" />

## <a name="the-table-view-data-source"></a>La Source de données de vue de Table

Chaque vue Table requiert une Source de données (`UITableViewDataSource`) pour fournir les données de la Table et de générer les lignes nécessaires, comme requis par la vue de Table.

Pour l’exemple ci-dessus, cliquez sur le nom du projet dans le **l’Explorateur de solutions**, sélectionnez **ajouter** > **nouveau fichier...**  et appelez-le `AttractionTableDatasource` et cliquez sur le **New** bouton permettant de créer. Ensuite, modifiez le `AttractionTableDatasource.cs` de fichier et qu’elle ressemble à ce qui suit :

```csharp
using System;
using System.Collections.Generic;
using UIKit;

namespace tvTable
{
    public class AttractionTableDatasource : UITableViewDataSource
    {
        #region Constants
        const string CellID = "AttrCell";
        #endregion

        #region Computed Properties
        public AttractionTableViewController Controller { get; set;}
        public List<CityInformation> Cities { get; set;}
        #endregion

        #region Constructors
        public AttractionTableDatasource (AttractionTableViewController controller)
        {
            // Initialize
            this.Controller = controller;
            this.Cities = new List<CityInformation> ();
            PopulateCities ();
        }
        #endregion

        #region Public Methods
        public void PopulateCities ()
        {
            // Clear existing
            Cities.Clear ();

            // Define cities and attractions
            var Paris = new CityInformation ("Paris");
            Paris.AddAttraction ("Eiffel Tower", "Is a wrought iron lattice tower on the Champ de Mars in Paris, France.", "EiffelTower");
            Paris.AddAttraction ("Musée du Louvre", "is one of the world's largest museums and a historic monument in Paris, France.", "Louvre");
            Paris.AddAttraction ("Moulin Rouge", "French for 'Red Mill', is a cabaret in Paris, France.", "MoulinRouge");
            Paris.AddAttraction ("La Seine", "Is a 777-kilometre long river and an important commercial waterway within the Paris Basin.", "RiverSeine");
            Cities.Add (Paris);

            var SanFran = new CityInformation ("San Francisco");
            SanFran.AddAttraction ("Alcatraz Island", "Is located in the San Francisco Bay, 1.25 miles (2.01 km) offshore from San Francisco.", "Alcatraz");
            SanFran.AddAttraction ("Golden Gate Bridge", "Is a suspension bridge spanning the Golden Gate strait between San Francisco Bay and the Pacific Ocean", "GoldenGateBridge");
            SanFran.AddAttraction ("San Francisco", "Is the cultural, commercial, and financial center of Northern California.", "SanFrancisco");
            SanFran.AddAttraction ("Telegraph Hill", "Is primarily a residential area, much quieter than adjoining North Beach.", "TelegraphHill");
            Cities.Add (SanFran);

            var Houston = new CityInformation ("Houston");
            Houston.AddAttraction ("City Hall", "It was constructed in 1938-1939, and is located in Downtown Houston.", "CityHall");
            Houston.AddAttraction ("Houston", "Is the most populous city in Texas and the fourth most populous city in the US.", "Houston");
            Houston.AddAttraction ("Texas Longhorn", "Is a breed of cattle known for its characteristic horns, which can extend to over 6 ft.", "LonghornCattle");
            Houston.AddAttraction ("Saturn V Rocket", "was an American human-rated expendable rocket used by NASA between 1966 and 1973.", "Rocket");
            Cities.Add (Houston);
        }
        #endregion

        #region Override Methods
        public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            // Get cell
            var cell = tableView.DequeueReusableCell (CellID) as AttractionTableCell;

            // Populate cell
            cell.Attraction = Cities [indexPath.Section].Attractions [indexPath.Row];

            // Return new cell
            return cell;
        }

        public override nint NumberOfSections (UITableView tableView)
        {
            // Return number of cities
            return Cities.Count;
        }

        public override nint RowsInSection (UITableView tableView, nint section)
        {
            // Return the number of attractions in the given city
            return Cities [(int)section].Attractions.Count;
        }

        public override string TitleForHeader (UITableView tableView, nint section)
        {
            // Get the name of the current city
            return Cities [(int)section].Name;
        }
        #endregion
    }
}
```

Jetons un œil à quelques sections de la classe en détail.

Tout d’abord, nous avons défini une constante contenant l’identificateur unique de la cellule de Prototype (c’est le même identificateur affecté dans le Concepteur d’Interface ci-dessus), ajouté un raccourci vers le contrôleur de vue de Table et créé le stockage de nos données :

```csharp
const string CellID = "AttrCell";
public AttractionTableViewController Controller { get; set;}
public List<CityInformation> Cities { get; set;}
```

Ensuite, nous enregistrer le contrôleur d’affichage Table, puis créer et remplir notre source de données (à l’aide de modèles de données défini ci-dessus) quand la classe est créée :

```csharp
public AttractionTableDatasource (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
    this.Cities = new List<CityInformation> ();
    PopulateCities ();
}
```

Pour cet exemple, le `PopulateCities` méthode crée simplement les objets de modèle de données en mémoire mais ils pourraient facilement être lus à partir d’un base de données ou un service web dans une application réelle :

```csharp
public void PopulateCities ()
{
    // Clear existing
    Cities.Clear ();

    // Define cities and attractions
    var Paris = new CityInformation ("Paris");
    Paris.AddAttraction ("Eiffel Tower", "Is a wrought iron lattice tower on the Champ de Mars in Paris, France.", "EiffelTower");
    ...
}
```

Le `NumberOfSections` méthode renvoie le nombre de Sections de la table :

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    // Return number of cities
    return Cities.Count;
}
```

Pour **brut** présentent des vues de Table, retourne toujours 1.

Le `RowsInSection` méthode retourne le nombre de lignes dans la Section actuelle :

```csharp
public override nint RowsInSection (UITableView tableView, nint section)
{
    // Return the number of attractions in the given city
    return Cities [(int)section].Attractions.Count;
}
```

Là encore, pour **brut** vues de Table, retourne le nombre total d’éléments dans la source de données.

Le `TitleForHeader` méthode retourne le titre pour certains Section :

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    // Get the name of the current city
    return Cities [(int)section].Name;
}
```

Pour un **brut** vue Table type, ne renseignez pas le titre (`""`).

Enfin, lorsque demandée par la vue de Table, créer et remplir une cellule de Prototype à l’aide de la `GetCell` méthode : 

```csharp
public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    // Get cell
    var cell = tableView.DequeueReusableCell (CellID) as AttractionTableCell;

    // Populate cell
    cell.Attraction = Cities [indexPath.Section].Attractions [indexPath.Row];

    // Return new cell
    return cell;
}
```

Pour plus d’informations sur l’utilisation avec un `UITableViewDatasource`, consultez d’Apple [UITableViewDatasource](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40006941) documentation.

<a name="The-Table-View-Delegate" />

## <a name="the-table-view-delegate"></a>Le délégué de la vue Table

Chaque vue Table requiert un délégué (`UITableViewDelegate`) pour répondre à l’interaction de l’utilisateur ou d’autres événements système sur la Table.

Pour l’exemple ci-dessus, cliquez sur le nom du projet dans le **l’Explorateur de solutions**, sélectionnez **ajouter** > **nouveau fichier...**  et appelez-le `AttractionTableDelegate` et cliquez sur le **New** bouton permettant de créer. Ensuite, modifiez le `AttractionTableDelegate.cs` de fichier et qu’elle ressemble à ce qui suit :

```csharp
using System;
using System.Collections.Generic;
using UIKit;

namespace tvTable
{
    public class AttractionTableDelegate : UITableViewDelegate
    {
        #region Computed Properties
        public AttractionTableViewController Controller { get; set;}
        #endregion

        #region Constructors
        public AttractionTableDelegate (AttractionTableViewController controller)
        {
            // Initializw
            this.Controller = controller;
        }
        #endregion

        #region Override Methods
        public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
            attraction.IsFavorite = (!attraction.IsFavorite);

            // Update UI
            Controller.TableView.ReloadData ();
        }

        public override bool CanFocusRow (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            // Inform caller of highlight change
            RaiseAttractionHighlighted (Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row]);
            return true;
        }
        #endregion

        #region Events
        public delegate void AttractionHighlightedDelegate (AttractionInformation attraction);
        public event AttractionHighlightedDelegate AttractionHighlighted;

        internal void RaiseAttractionHighlighted (AttractionInformation attraction)
        {
            // Inform caller
            if (this.AttractionHighlighted != null) this.AttractionHighlighted (attraction);
        }
        #endregion
    }
}
```

Jetons un œil à plusieurs sections de cette classe dans les détails.

Tout d’abord, nous créons un raccourci vers le contrôleur d’affichage Table lorsque la classe est créée :

```csharp
public AttractionTableViewController Controller { get; set;}
...

public AttractionTableDelegate (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
}
```

Ensuite, lorsqu’une ligne est sélectionnée (l’utilisateur clique sur la Surface de toucher de l’Apple à distance) nous souhaitons marquer le **Attraction** représenté par la ligne sélectionnée en tant que favori :

```csharp
public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
    attraction.IsFavorite = (!attraction.IsFavorite);

    // Update UI
    Controller.TableView.ReloadData ();
}
```

Ensuite, lorsque l’utilisateur met en surbrillance une ligne (en lui attribuant le Focus à l’aide de la Surface de toucher à distance Apple) que nous voulons présenter les détails de la **Attraction** représenté par cette ligne dans la section Détails de notre contrôleur d’affichage fractionné :

```csharp
public override bool CanFocusRow (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    // Inform caller of highlight change
    RaiseAttractionHighlighted (Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row]);
    return true;
}
...

public delegate void AttractionHighlightedDelegate (AttractionInformation attraction);
public event AttractionHighlightedDelegate AttractionHighlighted;

internal void RaiseAttractionHighlighted (AttractionInformation attraction)
{
    // Inform caller
    if (this.AttractionHighlighted != null) this.AttractionHighlighted (attraction);
}
```

Le `CanFocusRow` méthode est appelée pour chaque ligne qui doit obtenir le Focus dans la vue de Table. Retourner `true` si la ligne peut obtenir le Focus, sinon retourner `false`. Dans le cas de cet exemple, nous avons créé un personnalisé `AttractionHighlighted` événement sera déclenché sur chaque ligne qu’il reçoit le Focus.

Pour plus d’informations sur l’utilisation avec un `UITableViewDelegate`, consultez d’Apple [UITableViewDelegate](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40006942) documentation.

<a name="The-Table-View-Cell" />

## <a name="the-table-view-cell"></a>La cellule d’affichage de Table

Pour chaque cellule de Prototype que vous avez ajouté à la vue de Table dans le Concepteur d’Interface, vous avez également créé une instance personnalisée de la cellule d’affichage de Table (`UITableViewCell`) vous permet de remplir la nouvelle cellule (ligne), tel qu’il est créé.

Pour l’exemple d’application, double-cliquez sur le `AttractionTableCell.cs` fichier à ouvrir pour modification et qu’elle ressemble à ce qui suit :

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionTableCell : UITableViewCell
    {
        #region Private Variables
        private AttractionInformation _attraction = null;
        #endregion

        #region Computed Properties
        public AttractionInformation Attraction {
            get { return _attraction; }
            set {
                _attraction = value;
                UpdateUI ();
            }
        }
        #endregion

        #region Constructors
        public AttractionTableCell (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Private Methods
        private void UpdateUI ()
        {
            // Trap all errors
            try {
                Title.Text = Attraction.Name;
                Favorite.Hidden = (!Attraction.IsFavorite);
            } catch {
                // Since the UI might not be fully loaded, ignore
                // all errors at this point
            }
        }
        #endregion
    }
}
```

Cette classe fournit un stockage pour l’objet de modèle de données d’Attraction (`AttractionInformation` tel que défini ci-dessus) affiché dans la ligne donnée :

```csharp
private AttractionInformation _attraction = null;
...

public AttractionInformation Attraction {
    get { return _attraction; }
    set {
        _attraction = value;
        UpdateUI ();
    }
}
```

Le `UpdateUI` méthode remplit les Widgets d’interface utilisateur (qui ont été ajoutées au prototype de la cellule dans le Concepteur d’Interface) en fonction des besoins :

```csharp
private void UpdateUI ()
{
    // Trap all errors
    try {
        Title.Text = Attraction.Name;
        Favorite.Hidden = (!Attraction.IsFavorite);
    } catch {
        // Since the UI might not be fully loaded, ignore
        // all errors at this point
    }
}
```

Pour plus d’informations sur l’utilisation avec un `UITableViewCell`, consultez d’Apple [UITableViewCell](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewCell_Class/index.html#//apple_ref/doc/uid/TP40006938) documentation.

<a name="The-Table-View-Controller" />

## <a name="the-table-view-controller"></a>Le contrôleur d’affichage de Table

Un contrôleur d’affichage Table (`UITableViewController`) gère une vue de Table qui a été ajouté à une table de montage séquentiel via le Concepteur d’Interface.

Pour l’exemple d’application, double-cliquez sur le `AttractionTableViewController.cs` fichier à ouvrir pour modification et qu’elle ressemble à ce qui suit :

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionTableViewController : UITableViewController
    {
        #region Computed Properties
        public AttractionTableDatasource Datasource {
            get { return TableView.DataSource as AttractionTableDatasource; }
        }

        public AttractionTableDelegate TableDelegate {
            get { return TableView.Delegate as AttractionTableDelegate; }
        }
        #endregion

        #region Constructors
        public AttractionTableViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Setup table
            TableView.DataSource = new AttractionTableDatasource (this);
            TableView.Delegate = new AttractionTableDelegate (this);
            TableView.ReloadData ();
        }
        #endregion
    }
}
```

Examinons plus en détail à cette classe. Tout d’abord, nous avons créé des raccourcis pour le rendre plus facile d’accéder à la vue de Table `DataSource` et `TableDelegate`. Nous allons utiliser ceux ultérieurement pour la communication entre la vue de Table dans la partie gauche de la vue de fractionnement et de la vue de détails à droite.

Enfin, lorsque la vue de Table est chargée en mémoire, nous créons des instances de la `AttractionTableDatasource` et `AttractionTableDelegate` (tous deux créé ci-dessus) et les attacher à la vue de Table.

Pour plus d’informations sur l’utilisation avec un `UITableViewController`, consultez d’Apple [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523) documentation.

<a name="Pulling-it-All-Together" />

## <a name="pulling-it-all-together"></a>Extraction de tous les éléments

Comme indiqué au début de ce document, les vues de Table sont généralement affichées dans un côté d’un [mode fractionné](~/ios/tvos/user-interface/split-views.md) en tant que le volet de navigation, avec les détails de l’élément sélectionné est affiché dans le côté opposé. Exemple : 

[![](table-views-images/intro01.png "Exécution de l’application exemple")](table-views-images/intro01.png#lightbox)

Dans la mesure où il s’agit d’un modèle standard de tvOS, examinons les étapes finales à tout rassembler et ont les côtés gauche et droite de la vue de fractionnement interagissent entre eux.

<a name="The-Detail-View" />

### <a name="the-detail-view"></a>La vue détaillée

Pour l’exemple de l’application de voyage présentées ci-dessus, une classe personnalisée (`AttractionViewController`) est défini pour le contrôleur d’affichage standard présentées dans la partie droite de la vue de fractionnement en tant que la vue détail :

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionViewController : UIViewController
    {
        #region Private Variables
        private AttractionInformation _attraction = null;
        #endregion

        #region Computed Properties
        public AttractionInformation Attraction {
            get { return _attraction; }
            set {
                _attraction = value;
                UpdateUI ();
            }
        }

        public MasterSplitView SplitView { get; set;}
        #endregion

        #region Constructors
        public AttractionViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Public Methods
        public void UpdateUI ()
        {
            // Trap all errors
            try {
                City.Text = Attraction.City.Name;
                Title.Text = Attraction.Name;
                SubTitle.Text = Attraction.Description;

                IsFlighBooked.Hidden = (!Attraction.City.FlightBooked);
                IsFavorite.Hidden = (!Attraction.IsFavorite);
                IsDirections.Hidden = (!Attraction.AddDirections);
                BackgroundImage.Image = UIImage.FromBundle (Attraction.ImageName);
                AttractionImage.Image = BackgroundImage.Image;
            } catch {
                // Since the UI might not be fully loaded, ignore
                // all errors at this point
            }
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Ensure the UI Updates
            UpdateUI ();
        }
        #endregion

        #region Actions
        partial void BookFlight (NSObject sender)
        {
            // Ask user to book flight
            AlertViewController.PresentOKCancelAlert ("Book Flight",
                                                      string.Format ("Would you like to book a flight to {0}?", Attraction.City.Name),
                                                      this,
                                                      (ok) => {
                Attraction.City.FlightBooked = ok;
                IsFlighBooked.Hidden = (!Attraction.City.FlightBooked);
            });
        }

        partial void GetDirections (NSObject sender)
        {
            // Ask user to add directions
            AlertViewController.PresentOKCancelAlert ("Add Directions",
                                                     string.Format ("Would you like to add directions to {0} to you itinerary?", Attraction.Name),
                                                     this,
                                                     (ok) => {
                                                         Attraction.AddDirections = ok;
                                                         IsDirections.Hidden = (!Attraction.AddDirections);
                                                     });
        }

        partial void MarkFavorite (NSObject sender)
        {
            // Flip favorite state
            Attraction.IsFavorite = (!Attraction.IsFavorite);
            IsFavorite.Hidden = (!Attraction.IsFavorite);

            // Reload table
            SplitView.Master.TableController.TableView.ReloadData ();
        }
        #endregion
    }
}
```

Ici, nous avons fourni le **Attraction** (`AttractionInformation`) qui est affiché sous la forme d’une propriété et créé un `UpdateUI` méthode qui remplit les Widgets d’interface utilisateur ajoutée à la vue dans le Concepteur d’Interface.

Nous avons également défini un raccourci vers le contrôleur de vue de fractionnement (`SplitView`) que nous allons utiliser pour communiquer les modifications dans la vue de Table (`AcctractionTableView`).

Enfin, les Actions personnalisées (événements) ont été ajoutées aux trois `UIButton` instances créées dans le Concepteur d’Interface, qui lui permet de marquer une attraction comme un _favori_, obtenir _Directions_ à un attraction et _livre un vol_ dans une ville donnée.

<a name="The-Navigation-View-Controller" />

### <a name="the-navigation-view-controller"></a>Le contrôleur d’affichage de Navigation

Étant donné que le contrôleur d’affichage de tableau est imbriqué dans un contrôleur d’affichage de Navigation dans la partie gauche de la vue de fractionnement, le contrôleur d’affichage de Navigation a été affecté à une classe personnalisée (`MasterNavigationController`) dans le Concepteur d’Interface et définie comme suit :

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class MasterNavigationController : UINavigationController
    {
        #region Computed Properties
        public MasterSplitView SplitView { get; set;}
        public AttractionTableViewController TableController {
            get { return TopViewController as AttractionTableViewController; }
        }
        #endregion

        #region Constructors
        public MasterNavigationController (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

Là encore, cette classe définit uniquement quelques raccourcis pour faciliter la communiquer à travers les deux côtés du contrôleur d’affichage fractionné :

* `SplitView` -Est un lien vers le contrôleur d’affichage fractionné (`MainSpiltViewController`) auquel appartient le contrôleur d’affichage de Navigation.
* `TableController` -Obtient le contrôleur d’affichage Table (`AttractionTableViewController`) qui est présentée sous la vue de haut dans le contrôleur d’affichage de Navigation.

<a name="The-Split-View-Controller" />

### <a name="the-split-view-controller"></a>Le contrôleur d’affichage fractionné

Étant donné que le contrôleur d’affichage fractionné est la base de notre application, nous avons créé une classe personnalisée (`MasterSplitViewController`) pour lui dans le Concepteur d’Interface et il définie comme suit :

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class MasterSplitView : UISplitViewController
    {
        #region Computed Properties
        public AttractionViewController Details {
            get { return ViewControllers [1] as AttractionViewController; }
        }

        public MasterNavigationController Master {
            get { return ViewControllers [0] as MasterNavigationController; }
        }
        #endregion

        #region Constructors
        public MasterSplitView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            Master.SplitView = this;
            Details.SplitView = this;

            // Wire-up events
            Master.TableController.TableDelegate.AttractionHighlighted += (attraction) => {
                // Display new attraction
                Details.Attraction = attraction;
            };
        }
        #endregion
    }
}
```

Tout d’abord, nous créons des raccourcis vers les **détails** côté de la vue de fractionnement (`AttractionViewController`) et à la **Master** côté (`MasterNavigationController`). Là encore, cela rend plus facile pour la communication entre les deux côtés ultérieurement.

Ensuite, lorsque le mode fractionné est chargé en mémoire, nous attacher le contrôleur d’affichage fractionné pour les deux côtés de la vue de fractionnement et de répondre à l’utilisateur une attraction dans la vue de la Table de mise en surbrillance (`AttractionHighlighted`) en affichant la nouveau attraction dans le **détails**  côté de la vue de fractionnement.

Veuillez consulter la [tvTables](https://developer.xamarin.com/samples/monotouch/tvos/tvTable/) exemple d’application pour une implémentation complète de vues de Table à l’intérieur d’un affichage fractionné.

## <a name="table-views-in-detail"></a>Vues de table en détail

Étant donné que tvOS repose sur iOS, les vues de Table et les contrôleurs d’affichage de Table sont conçus et se comportent de manière similaire. Pour plus d’informations sur l’utilisation de la vue de Table dans une application Xamarin, consultez notre iOS [utilisation des Tables et cellules](~/ios/user-interface/controls/tables/index.md) documentation.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a présenté la conception et l’utilisation des vues de Table à l’intérieur d’une application Xamarin.tvOS. Et a présenté un exemple de travail avec une vue de Table à l’intérieur d’un affichage fractionné, qui est l’utilisation classique d’une vue de Table dans une application tvOS.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application pour tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
