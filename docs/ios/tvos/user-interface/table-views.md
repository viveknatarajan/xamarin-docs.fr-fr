---
title: Utilisation des vues de Table
description: "Cet article décrit la conception et l’utilisation des vues de Table et affichage des contrôleurs de Table à l’intérieur d’une application Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: D8F80FA9-6400-4DB7-AFC9-A28A54AD04E8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 26a73c2536bf4b4959928bfb27958b1a10734bf5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-table-views"></a>Utilisation des vues de Table

_Cet article décrit la conception et l’utilisation des vues de Table et affichage des contrôleurs de Table à l’intérieur d’une application Xamarin.tvOS._

TvOS, une vue de Table s’affiche en tant qu’une seule colonne de défilement de lignes qui peuvent éventuellement être organisés en groupes ou des sections. Vues de table doivent être utilisés lorsque vous avez besoin afficher une grande quantité de données efficacement à l’utilisateur, dans un clear pour comprendre la façon.

Vues de table sont généralement affichées dans un des côtés d’un [mode fractionné](~/ios/tvos/user-interface/split-views.md) en tant que le volet de navigation, avec les détails de l’élément sélectionné est affiché dans le côté opposé :

[ ![](table-views-images/intro01.png "Vue d’exemples de table")](table-views-images/intro01.png)

<a name="About-Table-Views" />

## <a name="about-table-views"></a>À propos des affichages de tableau

A `UITableView` affiche une seule colonne de lignes de défilement sous la forme d’une liste hiérarchique des informations qui peuvent éventuellement être organisées en groupes ou des sections : 

[ ![](table-views-images/table01.png "Un élément sélectionné")](table-views-images/table01.png)

Apple a les suggestions suivantes pour travailler avec des tables :

- **Prenant en charge de largeur** -essayer l’équilibre correct dans la largeur du tableau. Si la table est trop grande, il peut être difficile à analyser à distance et peut prendre en dehors de la zone de contenu disponible. Si la table est trop étroite, cela peut provoquer les informations seront tronquées ou wrap, à nouveau ce peut être difficile pour l’utilisateur à lire dans la salle.
- **Afficher le contenu de Table rapidement** - pour les grandes listes de données, le contenu du chargement différé et commencer à afficher les informations dès que la table est présentée à l’utilisateur. Si la table est trop longue à charger, l’utilisateur peut perdre l’intérêt dans votre application ou de la réflexion, qu'il est verrouillé.
- **Informer de Long contenu charges utilisateur** - si un temps de chargement du tableau long est inévitable, présente une [barre de progression ou le voyant d’activité](~/ios/tvos/user-interface/progress-indicators.md) afin qu’ils connaissent l’application n’a pas verrouillé.

<a name="Table-Cell-Types" />

## <a name="table-view-cell-types"></a>Afficher les Types cellule de table

A `UITableViewCell` est utilisé pour représenter les lignes de données dans la vue de la Table. Apple a défini plusieurs Types de cellules de Table par défaut :

- **Par défaut** - ce que présente le type une option de l’Image sur le côté gauche de la cellule et le titre d’aligné à gauche à droite. 
- **Sous-titre** - ce que présente le type titre aligné à gauche sur la première ligne et une plus petite aligné à gauche sous-titre sur la ligne suivante.
- **La valeur 1** -ce type présente un titre aligné à gauche avec un sous-titre de couleur plus claire, aligné à droite sur la même ligne.
- **La valeur 2** -ce type présente un titre aligné à droite avec un sous-titre de couleur plus claire, aligné à gauche sur la même ligne.

Tous les Types de cellule de vue de Table par défaut prennent également en charge les éléments graphiques comme des indicateurs de divulgation d’informations ou coches. 

En outre, vous pouvez définir un **personnalisé** le Type de cellule d’affichage tableau et présenter un _Prototype cellule_, que vous créez dans le Concepteur de l’Interface ou via le code.

Apple a les suggestions suivantes pour travailler avec des cellules de vue de Table :

- **Évitez le texte extrait** -conserver les lignes de texte courte afin qu’elles ne finissent tronquée. Tronquée mots ou expressions sont difficiles à l’utilisateur de l’analyse à partir de la salle.
- **Envisagez de l’état de ligne de focus** - parce qu’une ligne augmente, plus l’arrondi angles dans le focus, vous devez tester l’apparence de votre cellule dans tous les États. Images ou le texte peut être tronqué ou recherchez incorrect dans l’état actif.
- **Utilisez modifiable Tables avec parcimonie** -déplacement ou la suppression de lignes de la Table est plus de temps sur tvOS à iOS. Vous devez déterminer avec soin si cette fonctionnalité ajoutera ou d’un point à partir de votre application tvOS.
- **Création personnalisée cellule Types où approprié** - tandis que les Types de cellule vue Table intégrés sont parfaites pour nombreuses situations, envisagez de créer des Types de cellule personnalisés pour des informations non standard pour offrir un meilleur contrôle et représenter au mieux les informations à l’utilisateur.

<a name="Working-With-Table-Views" />

## <a name="working-with-table-views"></a>Utilisation des vues de Table

Pour utiliser des vues de Table dans une application Xamarin.tvOS, le plus simple consiste à créer et modifier leur apparence dans le Concepteur de l’Interface.

Pour commencer, effectuez les étapes suivantes :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)
    
1. Dans Visual Studio pour Mac, démarrez un nouveau projet d’application de tvOS, puis sélectionnez **tvOS** > **application** > **application vue unique** et cliquez sur le  **Suivant** bouton : 

    [ ![](table-views-images/table02.png "Sélectionner l’application de la vue unique")](table-views-images/table02.png)
1. Entrez un **nom** pour l’application et cliquez sur **suivant**: 

    [ ![](table-views-images/table03.png "Entrez un nom pour l’application")](table-views-images/table03.png)
1. Soit ajuster le **nom du projet** et **nom de la Solution** ou accepter les valeurs par défaut et cliquez sur le **créer** bouton permettant de créer la nouvelle solution : 

    [ ![](table-views-images/table04.png "Le nom du projet et le nom de la Solution")](table-views-images/table04.png)
1. Dans le **Solution remplissage**, double-cliquez sur le `Main.storyboard` fichier pour l’ouvrir dans le concepteur iOS : 

    [ ![](table-views-images/table05.png "Le fichier Main.storyboard")](table-views-images/table05.png)
1. Sélectionnez et supprimez le **contrôleur d’affichage par défaut**: 

    [ ![](table-views-images/table06.png "Sélectionnez et supprimez le contrôleur de la vue par défaut")](table-views-images/table06.png)
1. Sélectionnez un **fractionnement-View-Controller** à partir de la **boîte à outils** et faites-le glisser sur l’aire de conception.
1. Par défaut, vous obtiendrez un [mode fractionné](~/ios/tvos/user-interface/split-views.md) avec un **Navigation-View-Controller** et un **Table-View-Controller** dans la partie gauche et un **View Controller** dans la partie droite. Il s’agit d’utilisation de suggestions d’Apple d’une vue de Table dans tvOS : 

    [ ![](table-views-images/table08.png "Ajouter un affichage fractionné")](table-views-images/table08.png)
1. Vous devez sélectionner chaque partie de la vue de Table et affectez-le personnalisé **nom de la classe** dans les **Widget** onglet de la **l’Explorateur de propriétés** afin que vous pouvez y accéder ultérieurement en c# code. Par exemple, le **Table-View-Controller**: 

    [ ![](table-views-images/table09.png "Attribuer un nom de classe")](table-views-images/table09.png)
1. Vérifiez que vous créez une classe personnalisée pour le **Table-View-Controller**, le **Table vue** et n’importe quel **les cellules de Prototype**. Visual Studio pour Mac ajoutera les classes personnalisées à l’arborescence du projet comme elles sont créées : 

    [ ![](table-views-images/table10.png "Les classes personnalisées dans l’arborescence du projet")](table-views-images/table10.png)
1. Ensuite, sélectionnez la vue de Table dans l’aire de conception et ajuster ses propriétés en fonction des besoins. Comme le nombre de **Prototype cellules** et **Style** (brut ou groupées) : 

    [ ![](table-views-images/table11.png "L’onglet du widget")](table-views-images/table11.png)
1. Pour chaque **Prototype cellule**, sélectionnez-le, puis attribuer une valeur unique **identificateur** dans les **Widget** onglet de la **l’Explorateur de propriétés**. Cette étape est _très important_ que vous aurez besoin ultérieurement cet identificateur lorsque vous remplissez la table. Par exemple `AttrCell`: 

    [ ![](table-views-images/table12.png "L’onglet du Widget")](table-views-images/table12.png)
1. Vous pouvez également choisir de présenter la cellule en tant qu’un de la [par défaut des Types de cellule d’affichage de tableau](#Table-View-Cell-Types) via la **Style** liste déroulante ou la valeur **personnalisé** et utiliser l’aire de conception pour la cellule de disposition en faisant glisser dans les autres widgets d’interface utilisateur à partir de la **boîte à outils**: 

    [ ![](table-views-images/table13.png "La disposition des cellules")](table-views-images/table13.png)
1. Affecter une valeur unique **nom** à chaque élément d’interface utilisateur dans la conception de la cellule de Prototype dans le **Widget** onglet de la **l’Explorateur de propriétés** donc vous pouvez y accéder ultérieurement dans le code c# : 

    [ ![](table-views-images/table14.png "Attribuez un nom")](table-views-images/table14.png)
1. Répétez l’étape ci-dessus pour toutes les cellules de Prototype dans la vue de la Table.
1. Ensuite, affecter des classes personnalisées pour le reste de votre interface utilisateur, l’affichage des détails de disposition assign unique **noms** à chaque élément d’interface utilisateur dans les détails de l’afficher afin que vous pouvez également y accéder en c#. Par exemple : 

    [ ![](table-views-images/table15.png "La disposition de l’interface utilisateur")](table-views-images/table15.png)
1. Enregistrez vos modifications dans la table de montage séquentiel.
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. Dans Visual Studio, démarrez un nouveau projet d’application de tvOS, puis sélectionnez **tvOS** > **unique afficher l’application** et entrez un nom pour votre application. Cliquez sur le **OK** bouton permettant de créer une nouvelle solution : 

    [ ![](table-views-images/table02-vs.png "Sélectionner l’application de la vue unique")](table-views-images/table02-vs.png)
1. Dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` fichier pour l’ouvrir dans le concepteur iOS : 

    [ ![](table-views-images/table05-vs.png "Le fichier Main.storyboard")](table-views-images/table05-vs.png)
1. Sélectionnez et supprimez le **contrôleur d’affichage par défaut**: 

    [ ![](table-views-images/table06-vs.png "Sélectionnez et supprimez le contrôleur de la vue par défaut")](table-views-images/table06-vs.png)
1. Sélectionnez un **fractionnement-View-Controller** à partir de la **boîte à outils** et faites-le glisser sur l’aire de conception : 

    [ ![](table-views-images/table07-vs.png "Un contrôleur d’affichage fractionné")](table-views-images/table07-vs.png)
1. Par défaut, vous obtiendrez un [mode fractionné](~/ios/tvos/user-interface/split-views.md) avec un **Navigation-View-Controller** et un **Table-View-Controller** dans la partie gauche et un **View Controller** dans la partie droite. Il s’agit d’utilisation de suggestions d’Apple d’une vue de Table dans tvOS : 

    [ ![](table-views-images/table08-vs.png "Disposition de l’interface utilisateur")](table-views-images/table08-vs.png)
1. Vous devez sélectionner chaque partie de la vue de Table et affectez-le personnalisé **nom de la classe** dans les **Widget** onglet de la **l’Explorateur de propriétés** afin que vous pouvez y accéder ultérieurement en c# code. Par exemple, le **Table-View-Controller**: 

    [ ![](table-views-images/table09-vs.png "L’onglet du Widget")](table-views-images/table09-vs.png)
1. Vérifiez que vous créez une classe personnalisée pour le **Table-View-Controller**, le **Table vue** et n’importe quel **les cellules de Prototype**. Visual Studio pour Mac ajoutera les classes personnalisées à l’arborescence du projet comme elles sont créées : 

    [ ![](table-views-images/table10-vs.png "Les classes personnalisées dans l’arborescence du projet")](table-views-images/table10-vs.png)
1. Ensuite, sélectionnez la vue de Table dans l’aire de conception et ajuster ses propriétés en fonction des besoins. Comme le nombre de **Prototype cellules** et **Style** (brut ou groupées) : 

    [ ![](table-views-images/table11-vs.png "L’onglet du Widget")](table-views-images/table11-vs.png)
1. Pour chaque **Prototype cellule**, sélectionnez-le, puis attribuer une valeur unique **identificateur** dans les **Widget** onglet de la **l’Explorateur de propriétés**. Cette étape est _très important_ que vous aurez besoin ultérieurement cet identificateur lorsque vous remplissez la table. Par exemple `AttrCell`: 

    [ ![](table-views-images/table12-vs.png "Attribuer un identificateur")](table-views-images/table12-vs.png)
1. Vous pouvez également choisir de présenter la cellule en tant qu’un de la [par défaut des Types de cellule d’affichage de tableau](#Table-View-Cell-Types) via la **Style** liste déroulante ou la valeur **personnalisé** et utiliser l’aire de conception pour la cellule de disposition en faisant glisser dans les autres widgets d’interface utilisateur à partir de la **boîte à outils**: 

    [ ![](table-views-images/table13-vs.png "La liste déroulante de Style")](table-views-images/table13-vs.png)
1. Affecter une valeur unique **nom** à chaque élément d’interface utilisateur dans la conception de la cellule de Prototype dans le **Widget** onglet de la **l’Explorateur de propriétés** donc vous pouvez y accéder ultérieurement dans le code c# : 

    [ ![](table-views-images/table14-vs.png "L’onglet du Widget")](table-views-images/table14-vs.png)
1. Répétez l’étape ci-dessus pour toutes les cellules de Prototype dans la vue de la Table.
1. Ensuite, affecter des classes personnalisées pour le reste de votre interface utilisateur, l’affichage des détails de disposition assign unique **noms** à chaque élément d’interface utilisateur dans les détails de l’afficher afin que vous pouvez également y accéder en c#. Par exemple : 

    [ ![](table-views-images/table15.png "La disposition de l’interface utilisateur")](table-views-images/table15.png)
1. Enregistrez vos modifications dans la table de montage séquentiel.
    
-----

<a name="Designing-a-Data-Model" />

## <a name="designing-a-data-model"></a>Vous concevez un modèle de données

Pour rendre le travail avec les informations de la Table affichage plus facile et pour faciliter la présentation des informations détaillées (comme l’utilisateur sélectionne ou met en surbrillance des lignes de la vue de Table), créez une classe personnalisée ou des classes en tant que le modèle de données pour obtenir les informations présentées .

Prenons l’exemple d’une application de réservation de voyages qui contient une liste de **villes**, chacun contenant une liste unique de **centres d’intérêt** que l’utilisateur peut sélectionner. L’utilisateur sera en mesure de marquer une attraction comme un *favoris*, sélectionnez cette option pour obtenir *Directions* pour une utilisation sur et *livre un vol* dans une ville donnée.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Pour créer le modèle de données pour un **Attraction**, avec le bouton droit sur le nom du projet dans le **Solution remplissage** et sélectionnez **ajouter** > **nouveau fichier...** . Entrez `AttractionInformation` pour le **nom** et cliquez sur le **nouveau** bouton : 

[ ![](table-views-images/data01.png "Entrez AttractionInformation pour le nom")](table-views-images/data01.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pour créer le modèle de données pour un **Attraction**, avec le bouton droit sur le nom du projet dans le **l’Explorateur de solutions** et sélectionnez **ajouter** > **un nouvel élément ...** . Sélectionnez **classe** et entrez `AttractionInformation` pour le **nom** et cliquez sur le **ajouter** bouton : 

[ ![](table-views-images/data01-vs.png "Sélectionnez et entrez AttractionInformation pour le nom")](table-views-images/data01-vs.png)

-----

Modifier la `AttractionInformation.cs` de fichier et le rendre l’aspect suivant :

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

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Ensuite, avec le bouton droit sur le nom du projet dans le **Solution remplissage** à nouveau et sélectionnez **ajouter** > **nouveau fichier...** . Entrez `CityInformation` pour le **nom** et cliquez sur le **nouveau** bouton : 

[ ![](table-views-images/data02.png "Entrez CityInformation pour le nom")](table-views-images/data02.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ensuite, avec le bouton droit sur le nom du projet dans le **l’Explorateur de solutions** à nouveau, puis sélectionnez **ajouter** > **un nouvel élément...** . Entrez `CityInformation` pour le **nom** et cliquez sur le **ajouter** bouton : 

[ ![](table-views-images/data02-vs.png "Entrez CityInformation pour le nom")](table-views-images/data02-vs.png)

-----

Modifier la `CityInformation.cs` de fichier et le rendre l’aspect suivant :

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

Cette classe contient toutes les informations sur une destination **Ville**, une collection de **centres d’intérêt** pour ville et fournit deux méthodes d’assistance (`AddAttraction`) pour le rendre plus facile d’ajouter des sites à la Ville.

<a name="The-Table-Data-Source" />

## <a name="the-table-view-data-source"></a>La Source de données de vue de Table

Chaque vue Table requiert une Source de données (`UITableViewDataSource`) pour fournir les données de la Table et de générer les lignes nécessaires comme requis par la vue de la Table.

Pour l’exemple ci-dessus, cliquez sur le nom du projet dans le **l’Explorateur de solutions**, sélectionnez **ajouter** > **nouveau fichier...**  et appelez-le `AttractionTableDatasource` et cliquez sur le **nouveau** bouton permettant de créer. Modifiez ensuite le `AttractionTableDatasource.cs` de fichier et le rendre l’aspect suivant :

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

Examinons certaines sections de la classe en détail.

Tout d’abord, nous avons défini par une constante contenant l’identificateur unique de la cellule Prototype (c’est le même identificateur affecté dans le Concepteur d’Interface ci-dessus), ajouter un raccourci vers le contrôleur de la vue de Table et créé de stockage pour vos données :

```csharp
const string CellID = "AttrCell";
public AttractionTableViewController Controller { get; set;}
public List<CityInformation> Cities { get; set;}
```

Ensuite, nous enregistrer le contrôleur de la vue Table, puis créez et remplir notre source de données (à l’aide de modèles de données définis ci-dessus) lorsque la classe est créée :

```csharp
public AttractionTableDatasource (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
    this.Cities = new List<CityInformation> ();
    PopulateCities ();
}
```

Par exemple, le `PopulateCities` méthode crée simplement des objets de modèle de données dans la mémoire mais il peuvent facilement être lus à partir d’un service web ou de la base de données dans une application réelle :

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

Le `NumberOfSections` méthode retourne le nombre de Sections dans le tableau :

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

Là encore, pour **brut** vues des tables, retourner le nombre total d’éléments dans la source de données.

Le `TitleForHeader` méthode retourne le titre pour Section :

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    // Get the name of the current city
    return Cities [(int)section].Name;
}
```

Pour un **brut** affichage tableau de type, ne renseignez pas le titre (`""`).

Enfin, lorsqu’il est demandé par la vue de Table, créer et remplir une cellule de Prototype à l’aide de la `GetCell` méthode : 

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

Pour plus d’informations sur l’utilisation d’un `UITableViewDatasource`, consultez Apple [UITableViewDatasource](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40006941) documentation.

<a name="The-Table-View-Delegate" />

## <a name="the-table-view-delegate"></a>Le délégué de la vue Table

Chaque vue Table requiert un délégué (`UITableViewDelegate`) pour répondre à l’interaction de l’utilisateur ou d’autres événements système sur la Table.

Pour l’exemple ci-dessus, cliquez sur le nom du projet dans le **l’Explorateur de solutions**, sélectionnez **ajouter** > **nouveau fichier...**  et appelez-le `AttractionTableDelegate` et cliquez sur le **nouveau** bouton permettant de créer. Modifiez ensuite le `AttractionTableDelegate.cs` de fichier et le rendre l’aspect suivant :

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

Jetons un œil sur plusieurs sections de cette classe dans les détails.

Tout d’abord, nous créer un raccourci vers le contrôleur de la vue de Table lors de la création de la classe :

```csharp
public AttractionTableViewController Controller { get; set;}
...

public AttractionTableDelegate (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
}
```

Ensuite, lorsqu’une ligne est sélectionnée (l’utilisateur clique sur la Surface tactile d’Apple à distance) nous souhaitons marquer le **Attraction** représenté par la ligne sélectionnée en tant que favori :

```csharp
public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
    attraction.IsFavorite = (!attraction.IsFavorite);

    // Update UI
    Controller.TableView.ReloadData ();
}
```

Ensuite, lorsque l’utilisateur met en surbrillance une ligne (en lui donnant le Focus à l’aide de la Surface de Touch distant Apple) que nous souhaitons présenter les détails de la **Attraction** représenté par cette ligne dans la section Détails de notre contrôleur d’affichage fractionné :

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

Le `CanFocusRow` méthode est appelée pour chaque ligne qui doit recevoir le Focus dans la vue de la Table. Retourner `true` si la ligne peut obtenir le Focus, sinon retourne `false`. Dans le cas de cet exemple, nous avons créé un personnalisé `AttractionHighlighted` les événements qui seront déclenchés sur chaque ligne lorsqu’il reçoit le Focus.

Pour plus d’informations sur l’utilisation d’un `UITableViewDelegate`, consultez Apple [UITableViewDelegate](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40006942) documentation.

<a name="The-Table-View-Cell" />

## <a name="the-table-view-cell"></a>La cellule du tableau

Pour chaque cellule de Prototype que vous avez ajouté à la vue de Table dans le Concepteur de l’Interface, vous avez également créé une instance personnalisée de la cellule du tableau (`UITableViewCell`) vous permet de remplir la nouvelle cellule (ligne) lors de sa création.

Pour l’exemple d’application, double-cliquez sur le `AttractionTableCell.cs` fichier à ouvrir pour le modifier et le rendre l’aspect suivant :

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

Cette classe fournit le stockage pour l’objet de modèle de données Attraction (`AttractionInformation` tel qu’indiqué ci-dessus) affichés dans la ligne donnée :

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

Pour plus d’informations sur l’utilisation d’un `UITableViewCell`, consultez Apple [UITableViewCell](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewCell_Class/index.html#//apple_ref/doc/uid/TP40006938) documentation.

<a name="The-Table-View-Controller" />

## <a name="the-table-view-controller"></a>Le contrôleur de la vue Table

Un contrôleur de vue de Table (`UITableViewController`) gère une vue de Table qui a été ajouté à un plan conceptuel via le Concepteur de l’Interface.

Pour l’exemple d’application, double-cliquez sur le `AttractionTableViewController.cs` fichier à ouvrir pour le modifier et le rendre l’aspect suivant :

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

Jetons un œil plus près à cette classe. Tout d’abord, nous avons créé des raccourcis pour le rendre plus facile à accéder à la vue de Table `DataSource` et `TableDelegate`. Nous allons utiliser celles ultérieurement pour la communication entre la vue de Table dans la partie gauche de la vue de fractionnement et de la vue des détails à droite.

Enfin, lorsque la vue de Table sont chargée en mémoire, nous créons des instances de la `AttractionTableDatasource` et `AttractionTableDelegate` (à la fois créé ci-dessus) et les attacher à la vue de la Table.

Pour plus d’informations sur l’utilisation d’un `UITableViewController`, consultez Apple [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523) documentation.

<a name="Pulling-it-All-Together" />

## <a name="pulling-it-all-together"></a>Extraction de tous les éléments

Comme indiqué au début de ce document, les vues de Table sont généralement affichées dans un des côtés d’un [mode fractionné](~/ios/tvos/user-interface/split-views.md) en tant que le volet de navigation, avec les détails de l’élément sélectionné est affiché dans le côté opposé. Exemple : 

[ ![](table-views-images/intro01.png "Exemple d’application de s’exécuter")](table-views-images/intro01.png)

Dans la mesure où il s’agit d’un modèle standard de tvOS, examinons les dernières étapes pour rassembler tous les éléments et ont les côtés gauche et droite de l’affichage fractionné interagissent entre eux.

<a name="The-Detail-View" />

### <a name="the-detail-view"></a>L’affichage des détails

L’exemple de l’application de voyage présentée ci-dessus, une classe personnalisée (`AttractionViewController`) est défini pour le contrôleur de vue standard présentées dans la partie droite de l’affichage fractionné en tant que l’affichage des détails :

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

Ici, nous avons fourni le **Attraction** (`AttractionInformation`) qui est affiché sous la forme d’une propriété et créé un `UpdateUI` méthode qui remplit les Widgets d’interface utilisateur ajoutées à la vue dans le Concepteur de l’Interface.

Nous avons également défini un raccourci vers le contrôleur d’affichage fractionné (`SplitView`) que nous allons utiliser pour communiquer les modifications dans la vue de Table (`AcctractionTableView`).

Enfin, les Actions personnalisées (événements) ont été ajoutées aux trois `UIButton` instances créées dans le Concepteur de l’Interface, qui permet à l’utilisateur marquer une attraction comme un _favoris_, obtenir _Directions_ à un attraction et _livre un vol_ dans une ville donnée.

<a name="The-Navigation-View-Controller" />

### <a name="the-navigation-view-controller"></a>Le contrôleur de mode de Navigation

Étant donné que le contrôleur de la vue de Table est imbriqué dans un contrôleur de vue de Navigation dans la partie gauche de l’affichage fractionné, le contrôleur de la vue Navigation a été affecté à une classe personnalisée (`MasterNavigationController`) dans le Concepteur de l’Interface et défini comme suit :

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

Là encore, cette classe définit seulement quelques raccourcis pour faciliter la communiquer à travers les deux côtés du contrôleur de mode de fractionnement :

* `SplitView` -Est un lien vers le contrôleur d’affichage fractionné (`MainSpiltViewController`) auquel appartient le contrôleur de la vue Navigation.
* `TableController` -Obtient le contrôleur de la vue de Table (`AttractionTableViewController`) qui est présentée sous forme de la vue en haut dans le contrôleur de la vue Navigation.

<a name="The-Split-View-Controller" />

### <a name="the-split-view-controller"></a>Le contrôleur d’affichage fractionné

Étant donné que le contrôleur de mode de fractionnement est la base de notre application, nous avons créé une classe personnalisée (`MasterSplitViewController`) pour lui dans le Concepteur de l’Interface et défini comme suit :

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

Tout d’abord, nous créons des raccourcis vers les **détails** côte du mode fractionné (`AttractionViewController`) et le **Master** côté (`MasterNavigationController`). Là encore, cela rend plus facile pour la communication entre les deux côtés ultérieurement.

Ensuite, lorsque le mode fractionné est chargé en mémoire, nous joindre le contrôleur d’affichage fractionné pour les deux côtés de la vue de fractionnement et répondre à l’utilisateur une attraction dans la vue de la Table de mise en surbrillance (`AttractionHighlighted`) en affichant la nouveau attraction dans le **détails**  côte du mode fractionné.

Consultez le [tvTables](https://developer.xamarin.com/samples/monotouch/tvos/tvTable/) exemple d’application pour une implémentation complète des affichages de tableau à l’intérieur d’un affichage fractionné.

## <a name="table-views-in-detail"></a>Vues des tables en détail

Étant donné que tvOS est basés sur iOS, vues des tables et des contrôleurs de la vue Table sont conçus et se comportent de la même manière. Pour plus d’informations sur l’utilisation de la Table de vue dans une application Xamarin, consultez notre iOS [utilisation de Tables et des cellules](~/ios/user-interface/controls/tables/index.md) documentation.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a couvert conception et utilisation des vues de Table à l’intérieur d’une application Xamarin.tvOS. Et présente un exemple de l’utilisation de la vue d’une Table à l’intérieur d’un affichage fractionné, qui est l’utilisation typique d’une vue de Table dans une application tvOS.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS les Guides Interface humaine](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
