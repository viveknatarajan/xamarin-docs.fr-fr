---
title: watchOS contrôles de Table dans Xamarin
description: Ce document décrit comment utiliser les contrôles de table watchOS dans Xamarin. Il aborde l’ajout d’une table, l’ajout d’un contrôleur de ligne, la création et la remplir des lignes, en réponse aux clics et bien plus encore.
ms.prod: xamarin
ms.assetid: 7C14126D-9591-4387-A588-3C4521F11C55
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: cd5e7299874bbfb1b652315a549b9d067d58e9a0
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109391"
---
# <a name="watchos-table-controls-in-xamarin"></a>watchOS contrôles de Table dans Xamarin

Le watchOS `WKInterfaceTable` contrôle est beaucoup plus simple que son équivalent iOS, mais il joue un rôle similaire. Il crée une liste déroulante de lignes qui peut avoir des dispositions personnalisées, et qui répondent aux événements tactiles.

![](table-images/table-list-sml.png "Liste de table de suivi") ![](table-images/table-detail-sml.png)
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="adding-a-table"></a>Ajout d’une Table

Faites glisser le **Table** contrôle dans une scène. Par défaut, il ressemblera à ce (montrant une disposition de ligne non spécifiée) :

[![](table-images/add-table-sml.png "Ajout d’une table")](table-images/add-table.png#lightbox)

Nommez la table le **propriétés** du pavé **nom** zone, afin qu’il puisse être référencé dans le code.

## <a name="add-a-row-controller"></a>Ajouter un contrôleur de ligne

La table inclut automatiquement une seule ligne, représentée par un contrôleur de la ligne qui contienne un **groupe** contrôle par défaut.

Pour définir le **classe** pour le contrôleur de la ligne, sélectionnez la ligne dans le **structure du Document** et tapez un nom de classe dans le **propriétés** panneau :

[![](table-images/add-row-controller-sml.png "Entrer un nom de classe dans le panneau Propriétés")](table-images/add-row-controller.png#lightbox)

Une fois que la classe de contrôleur de la ligne est définie, l’IDE crée un correspondant C# fichier dans le projet. Faire glisser des contrôles (par exemple, les étiquettes) sur la ligne et les nommer afin qu’ils peuvent être référencés dans le code.




## <a name="create-and-populate-rows"></a>Créer et remplir des lignes

`SetNumberOfRows` crée des classes de contrôleur de la ligne pour chaque ligne, à l’aide de la `Identifier` pour sélectionner celui qui convient. Si vous avez donné à votre contrôleur de ligne personnalisé `Identifier`, modifier **par défaut** dans l’extrait de code ci-dessous à l’identificateur que vous avez utilisé. Le `RowController` *pour chaque ligne* est créé lorsque `SetNumberOfRows` est appelée et la table affichée.

```csharp
myTable.SetNumberOfRows ((nint)rows.Count, "default");
        // loads row controller by identifier
```

> [!IMPORTANT]
> Lignes de la table ne sont pas virtualisés, comme c’est dans iOS. Essayez de limiter le nombre de lignes (Apple recommande de moins de 20).

Une fois les lignes ont été créées, vous devez remplir chaque cellule (comme `GetCell` dans iOS). Cet extrait de code à partir de la [WatchTables exemple](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/) met à jour l’étiquette dans chaque ligne

```csharp
for (var i = 0; i < rows.Count; i++) {
    var elementRow = (RowController)myTable.GetRowController (i);
    elementRow.myRowLabel.SetText (rows [i]);
}
```

> [!IMPORTANT]
> À l’aide de `SetNumberOfRows` et boucle ensuite à l’aide de `GetRowController` , la table entière à envoyer à la surveillance. Sur les vues suivantes de la table, si vous avez besoin ajouter ou supprimer des lignes spécifiques utilisent `InsertRowsAt` et `RemoveRowsAt` pour de meilleures performances.


## <a name="respond-to-taps"></a>Répondre aux clics

Vous pouvez répondre à la sélection de ligne de deux manières différentes :

- implémenter le `DidSelectRow` méthode sur le contrôleur d’interface, ou
- Créez un segue sur le plan conceptuel et implémentez `GetContextForSegue` si vous souhaitez que la sélection de ligne pour ouvrir une autre séquence.

### <a name="didselectrow"></a>DidSelectRow

Pour gérer par programme la sélection de ligne, vous devez implémenter le `DidSelectRow` (méthode). Pour ouvrir une nouvelle scène, utilisez `PushController` et transmettez l’identificateur de la scène et le contexte de données à utiliser :

```csharp
public override void DidSelectRow (WKInterfaceTable table, nint rowIndex)
{
    var rowData = rows [(int)rowIndex];
    Console.WriteLine ("Row selected:" + rowData);
    // if selection should open a new scene
    PushController ("secondInterface", rows[(int)rowIndex]);
}
```

### <a name="getcontextforsegue"></a>GetContextForSegue

Faites glisser un segue sur le plan conceptuel à partir de votre ligne de table à une autre scène (maintenez la **contrôle** enfoncée tout en faisant glisser).
Veillez à sélectionner le segue et lui donner un identificateur le **propriétés** pad (tel que `secondLevel` dans l’exemple ci-dessous).

Dans le contrôleur d’interface, implémentez le `GetContextForSegue` méthode et retournez le contexte de données qui doit être fourni à la scène est présentée par le segue.

```csharp
public override NSObject GetContextForSegue (string segueIdentifier, WKInterfaceTable table, nint rowIndex)
{
    if (segueIdentifier == "secondLevel") {
        return new NSString (rows[(int)rowIndex]);
    }
    return null;
}
```

Ces données sont passées à la scène de la table de montage séquentiel cible dans sa `Awake` (méthode).

## <a name="multiple-row-types"></a>Plusieurs Types de ligne

Par défaut, le contrôle de table a un seul type de ligne que vous pouvez concevoir. Pour ajouter la ligne plus « modèles » utilisent le **lignes** zone le **propriétés** panneau pour créer plusieurs contrôleurs de ligne :

![](table-images/prototype-rows1.png "Définition du nombre de lignes de Prototype")

Définition de la **lignes** propriété **3** créera des espaces réservés de ligne supplémentaire pour faire glisser des contrôles en. Pour chaque ligne, définissez la **classe** nom dans le **propriétés** panneau pour vous assurer de la classe de contrôleur de ligne est créée.

![](table-images/prototype-rows2.png "Les lignes de prototype dans le Concepteur")

Pour remplir une table avec l’utilisation de types différents de ligne le `SetRowTypes` méthode pour spécifier le type de contrôleur de ligne à utiliser pour chaque ligne dans la table. Utilisez les identificateurs de la ligne pour spécifier quel contrôleur de ligne doit être utilisé pour chaque ligne.

Le nombre d’éléments dans ce tableau doit correspondre au nombre de lignes que devant figurer dans la table :

```csharp
myTable.SetRowTypes (new [] {"type1", "default", "default", "type2", "default"});
```

Lors du remplissage d’une table avec plusieurs contrôleurs de ligne, vous devez effectuer le suivi de quel type attendu que vous remplissez l’interface utilisateur :

```csharp
for (var i = 0; i < rows.Count; i++) {
    if (i == 0) {
        var elementRow = (Type1RowController)myTable.GetRowController (i);
        // populate UI controls
    } else if (i == 3) {
        var elementRow = (Type2RowController)myTable.GetRowController (i);
        // populate UI controls
    } else {
        var elementRow = (DefaultRowController)myTable.GetRowController (i);
        // populate UI controls
    }
}
```


## <a name="vertical-detail-paging"></a>Pagination de détail verticale

watchOS 3 a introduit une nouvelle fonctionnalité pour les tables : la possibilité de faire défiler les pages de détail associées à chaque ligne, sans avoir à revenir en arrière à la table et choisissez une autre ligne. Les écrans de détails peuvent être parcourus en faisant glisser vers de haut et le bas, ou à l’aide de la couronne numérique.

![](table-images/table-scroll-sml.png "Exemple de pagination de détail verticale") ![](table-images/table-detail-sml.png)

> [!IMPORTANT]
> Cette fonctionnalité est actuellement uniquement disponible en modifiant le plan conceptuel dans Xcode Interface Builder.

Pour activer cette fonctionnalité, sélectionnez le `WKInterfaceTable` sur l’aire de conception et de la graduation la **Vertical en détail la pagination** option :

![](table-images/vertical-detail-paging-sml.png "En sélectionnant l’option de pagination de détail verticale")

En tant que [expliquée par Apple](https://developer.apple.com/reference/watchkit/wkinterfacetable#1682023) la navigation de la table doit utiliser Enchaînements pour la fonctionnalité de pagination fonctionne. Réécrire tout code existant qui utilise `PushController` à utiliser Enchaînements à la place.

<a name="add_row_controller" />

## <a name="appendix-row-controller-code-example"></a>Annexe : Exemple de Code de contrôleur de ligne

L’IDE crée automatiquement deux fichiers de code lorsqu’un contrôleur de ligne est créé dans le concepteur. Le code dans ces fichiers générés est indiqué ci-dessous pour référence.

Le premier est nommé pour la classe, par exemple **RowController.cs**, comme suit :

```csharp
using System;
using Foundation;

namespace WatchTablesExtension
{
    public partial class RowController : NSObject
    {
        public RowController ()
        {
        }
    }
}
```

L’autre **. designer.cs** fichier est une définition de classe partielle qui contient les prises de courant et les actions qui sont créées sur l’aire du concepteur, comme dans cet exemple avec un `WKInterfaceLabel` contrôle :

```csharp
using Foundation;
using System;
using System.CodeDom.Compiler;
using UIKit;

namespace WatchTables.OnWatchExtension
{
    [Register ("RowController")]
    partial class RowController
    {
        [Outlet]
        [GeneratedCode ("iOS Designer", "1.0")]
        public WatchKit.WKInterfaceLabel MyLabel { get; set; }

        void ReleaseDesignerOutlets ()
        {
            if (MyLabel != null) {
                MyLabel.Dispose ();
                MyLabel = null;
            }
        }
    }
}
```

Les outlets et actions déclarées ici peuvent ensuite être référencées dans le code - toutefois le **. designer.cs** fichier ne doit pas être modifié directement.



## <a name="related-links"></a>Liens associés

- [WatchTables (exemple)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/)
- [WatchKitCatalog (exemple)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Document de Table d’Apple](https://developer.apple.com/reference/watchkit/wkinterfacetable)
