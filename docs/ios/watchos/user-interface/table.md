---
title: Contrôle de table
ms.prod: xamarin
ms.assetid: 7C14126D-9591-4387-A588-3C4521F11C55
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 3f596b424292dd0f4db08429bc7c5d4a885d3954
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="table-control"></a>Contrôle de table

Le watchOS `WKInterfaceTable` contrôle est beaucoup plus simple que son équivalent iOS, mais effectue un rôle similaire. Il crée une liste déroulante de lignes qui peut avoir des dispositions personnalisées, et qui répondent aux événements tactiles.

![](table-images/table-list-sml.png "Liste de table de suivi") ![](table-images/table-detail-sml.png)
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="adding-a-table"></a>Ajout d’une Table

Faites glisser le **Table** contrôle dans une scène. Par défaut, il ressemblera cette (avec une disposition de ligne non spécifiée) :

[![](table-images/add-table-sml.png "Ajout d’une table")](table-images/add-table.png#lightbox)

Donnez un nom à la table dans le **propriétés** du remplissage **nom** zone, afin qu’il peut apparaître dans le code.

## <a name="add-a-row-controller"></a>Ajouter un contrôleur de ligne

La table inclut automatiquement une ligne unique, représentée par un contrôleur de la ligne qui contienne un **groupe** contrôle par défaut.

Pour définir le **classe** pour le contrôleur de ligne, sélectionnez la ligne dans le **structure du Document** et tapez un nom de classe dans le **propriétés** remplissage :

[![](table-images/add-row-controller-sml.png "Entrez un nom de classe dans la zone de propriétés")](table-images/add-row-controller.png#lightbox)

Une fois que la classe de contrôleur de la ligne est définie, l’IDE créera un fichier c# correspondant dans le projet. Faites glisser les contrôles (par exemple, les étiquettes) sur la ligne et les nommer afin qu’ils peuvent être référencés dans le code.




## <a name="create-and-populate-rows"></a>Créer et remplir des lignes

`SetNumberOfRows` crée des classes de contrôleur de la ligne pour chaque ligne, à l’aide de la `Identifier` à appropriée. Si vous donnez à votre contrôleur de ligne personnalisé `Identifier`, modifiez **par défaut** dans l’extrait de code ci-dessous à l’identificateur que vous avez utilisé. Le `RowController` *pour chaque ligne* est créé lorsque `SetNumberOfRows` est appelée et la table affichée.

```csharp
myTable.SetNumberOfRows ((nint)rows.Count, "default");
        // loads row controller by identifier
```

> [!IMPORTANT]
> Lignes de la table ne sont pas virtualisés comme dans iOS. Essayez de limiter le nombre de lignes (Apple recommande de moins de 20).

Une fois que les lignes ont été créées, vous devez remplir chaque cellule (comme `GetCell` dans iOS). Cet extrait de code à partir de la [WatchTables exemple](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/) met à jour l’étiquette dans chaque ligne.

```csharp
for (var i = 0; i < rows.Count; i++) {
    var elementRow = (RowController)myTable.GetRowController (i);
    elementRow.myRowLabel.SetText (rows [i]);
}
```

> [!IMPORTANT]
> À l’aide de `SetNumberOfRows` et puis boucle à l’aide de `GetRowController` , la table entière à envoyer à la surveillance. Sur les vues suivantes de la table, si vous avez besoin ajouter ou supprimer des lignes spécifiques utilisent `InsertRowsAt` et `RemoveRowsAt` pour de meilleures performances.


## <a name="respond-to-taps"></a>Répondre aux clics

Vous pouvez répondre à la sélection de ligne de deux manières différentes :

- implémenter la `DidSelectRow` méthode sur le contrôleur d’interface, ou
- créer un segue sur le plan conceptuel et implémenter `GetContextForSegue` si vous souhaitez que la sélection de ligne pour ouvrir une autre séquence.

### <a name="didselectrow"></a>DidSelectRow

Pour gérer par programme la sélection de ligne, vous devez implémenter le `DidSelectRow` (méthode). Pour ouvrir une nouvelle séquence, utilisez `PushController` et de transmettre les identificateur de la scène et le contexte de données à utiliser :

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

Faites glisser un segue sur le plan conceptuel à partir de votre ligne de la table à une autre séquence (maintenez la **contrôle** enfoncée tout en faisant glisser).
Veillez à sélectionner le segue et lui donner un identificateur le **propriétés** remplissage (tel que `secondLevel` dans l’exemple ci-dessous).

Dans le contrôleur d’interface, implémentez le `GetContextForSegue` (méthode) et retourner le contexte de données qui doit-elle être fourni dans la scène est présentée par le segue.

```csharp
public override NSObject GetContextForSegue (string segueIdentifier, WKInterfaceTable table, nint rowIndex)
{
    if (segueIdentifier == "secondLevel") {
        return new NSString (rows[(int)rowIndex]);
    }
    return null;
}
```

Ces données sont passées à la séquence d’animation cible dans sa `Awake` (méthode).

## <a name="multiple-row-types"></a>Plusieurs Types de lignes

Par défaut, le contrôle de table a un seul type de ligne que vous pouvez concevoir. Pour ajouter la ligne plus « modèles » utilisent le **lignes** zone le **propriétés** remplissage pour créer plusieurs contrôleurs de ligne :

![](table-images/prototype-rows1.png "Définition du nombre de lignes de Prototype")

Définition de la **lignes** propriété **3** va créer des espaces réservés de ligne supplémentaire pour faire glisser des contrôles en. Pour chaque ligne, définissez la **classe** dans les **propriétés** remplissage pour vous assurer de la classe de contrôleur de ligne est créée.

![](table-images/prototype-rows2.png "Les lignes de prototype dans le Concepteur")

Pour remplir une table avec l’utilisation de types différents de ligne la `SetRowTypes` méthode pour spécifier le type de contrôleur de ligne à utiliser pour chaque ligne de la table. Utilisez les identificateurs de la ligne pour spécifier le contrôleur de ligne doit être utilisé pour chaque ligne.

Le nombre d’éléments dans ce tableau doit correspondre au nombre de lignes que devant figurer dans la table :

```csharp
myTable.SetRowTypes (new [] {"type1", "default", "default", "type2", "default"});
```

Lors du remplissage d’une table avec plusieurs contrôleurs de ligne, vous devez effectuer le suivi de quel type vous attendez que le chargement de l’interface utilisateur :

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


## <a name="vertical-detail-paging"></a>Pagination de détail vertical

watchOS 3 a introduit une nouvelle fonctionnalité pour les tables : la possibilité de faire défiler les pages détaillées relatives à chaque ligne, sans avoir à revenir en arrière à la table et choisissez une autre ligne. Les écrans de détail peuvent défiler en passant en haut et bas, ou en utilisant la couronne numérique.

![](table-images/table-scroll-sml.png "Exemple de pagination de détail vertical") ![](table-images/table-detail-sml.png)

> [!IMPORTANT]
> Cette fonctionnalité est actuellement uniquement disponible en modifiant le plan conceptuel dans Xcode Interface Builder.

Pour activer cette fonctionnalité, sélectionnez le `WKInterfaceTable` sur l’aire de conception et de graduations le **Vertical en détail la pagination** option :

![](table-images/vertical-detail-paging-sml.png "L’option de pagination de détail Vertical")

En tant que [expliqué par Apple](https://developer.apple.com/reference/watchkit/wkinterfacetable#1682023) la navigation dans les tables doit utiliser est parfait pour la fonctionnalité de pagination à utiliser. Rédigez à nouveau tout code existant qui utilise `PushController` à utiliser est parfait pour à la place.

<a name="add_row_controller" />

## <a name="appendix-row-controller-code-example"></a>Annexe : Exemple de Code de contrôleur de ligne

L’IDE crée automatiquement deux fichiers de code lors de la création d’un contrôleur de ligne dans le concepteur. Le code dans ces fichiers générés est indiqué ci-dessous pour référence.

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

L’autre **. designer.cs** fichier est une définition de classe partielle qui contient les prises de courant et les actions qui sont créées sur l’aire du concepteur, telles que cet exemple avec un `WKInterfaceLabel` contrôle :

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

Les sorties et les actions déclarées ici peuvent ensuite être référencées dans le code - toutefois le **. designer.cs** fichier ne doit pas être modifié directement.



## <a name="related-links"></a>Liens associés

- [WatchTables (exemple)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/)
- [WatchKitCatalog (exemple)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Document de Table d’Apple](https://developer.apple.com/reference/watchkit/wkinterfacetable)
