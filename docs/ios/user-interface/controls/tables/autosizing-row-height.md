---
title: Hauteur de ligne de redimensionnement automatique dans Xamarin.iOS
description: Ce document décrit comment ajouter des lignes de la vue table dont hauteurs varient en fonction du contenu pour les applications Xamarin.iOS. Il aborde la disposition de la cellule dans le concepteur iOS et la hauteur de redimensionnement automatique de l’activation.
ms.prod: xamarin
ms.assetid: CE45A385-D40A-482A-90A0-E8382C2BFFB9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: e4446abc73817eb0672cd10a69ff6f738de0c1e1
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116470"
---
# <a name="auto-sizing-row-height-in-xamarinios"></a>Hauteur de ligne de redimensionnement automatique dans Xamarin.iOS

À compter d’iOS 8, Apple ajouté la possibilité de créer une vue de Table (`UITableView`) qui peut automatiquement augmenter ou réduire la hauteur d’une ligne donnée, selon la taille de son contenu à l’aide de la disposition automatique, les Classes de taille et les contraintes.

iOS 11 a ajouté la possibilité pour les lignes à développer automatiquement. En-têtes, pieds de page et cellules peuvent désormais être dimensionnées automatiquement en fonction de leur contenu. Toutefois, si votre table est créée dans le concepteur iOS, Interface Builder, ou si elle a résolu les hauteurs de lignes vous devez activer manuellement self dimensionnement des cellules, comme décrit dans ce guide.

## <a name="cell-layout-in-the-ios-designer"></a>Disposition de la cellule dans le concepteur iOS

Ouvrez la table de montage séquentiel pour la vue de Table que vous souhaitez avoir un redimensionnement automatique de la ligne pour dans le concepteur, iOS sélectionnez la cellule *Prototype* et concevoir la disposition de la cellule. Exemple :

[![](autosizing-row-height-images/table01.png "Conception de Prototype de la cellule")](autosizing-row-height-images/table01.png#lightbox)

Pour chaque élément dans le Prototype, ajouter des contraintes pour conserver les éléments dans la position correcte pendant le redimensionnement de la vue de Table pour la rotation ou iOS différentes tailles d’écran de périphérique. Par exemple, l’épinglage le `Title` vers le haut, gauche et droit de la cellule *affichage du contenu*:

[![](autosizing-row-height-images/table02.png "Épingler le titre vers le haut, gauche et droite de l’affichage de contenu des cellules")](autosizing-row-height-images/table02.png#lightbox)

Dans le cas de notre exemple de table, le petit `Label` (sous le `Title`) est le champ que vous pouvez réduire et développer pour augmenter ou diminuer la hauteur de ligne. Pour obtenir cet effet, ajoutez les contraintes suivantes pour épingler la gauche, droite, haut et bas de l’étiquette :

[![](autosizing-row-height-images/table03.png "Ces contraintes d’épinglage de gauche, droite, haut et bas de l’étiquette")](autosizing-row-height-images/table03.png#lightbox)

Maintenant que nous avons complètement contraint les éléments dans la cellule, nous devons vous préciser quel élément doit être étiré. Pour ce faire, définissez la **contenu priorité Hugging** et **priorité de résistance de la Compression de contenu** autant que nécessaire dans le **disposition** section du panneau Propriétés :

[![](autosizing-row-height-images/table03a.png "La section disposition du panneau Propriétés")](autosizing-row-height-images/table03a.png#lightbox)

Définissez l’élément que vous souhaitez développer pour avoir un **inférieur** valeur de priorité Hugging et un **inférieure** valeur de priorité de résistance de Compression.

Ensuite, nous devons sélectionner le Prototype de cellule et lui donner une valeur unique **identificateur**:

[![](autosizing-row-height-images/table04.png "En donnant le Prototype de cellule un identificateur unique")](autosizing-row-height-images/table04.png#lightbox)

Dans le cas de notre exemple, `GrowCell`. Nous allons utiliser cette valeur ultérieurement, lorsque nous remplir la table.

> [!IMPORTANT]
> Si votre table contient plus d’un type de cellule (**Prototype**), vous devez vous assurer de chaque type possède son propre `Identifier` pour le redimensionnement de ligne automatique fonctionne.

Pour chaque élément de notre Prototype de la cellule, affecter un **nom** pour l’exposer à C# code. Exemple :

[![](autosizing-row-height-images/table05.png "Attribuez un nom pour l’exposer à C# code")](autosizing-row-height-images/table05.png#lightbox)

Ensuite, ajoutez une classe personnalisée pour le `UITableViewController`, le `UITableView` et `UITableCell` (Prototype). Exemple : 

[![](autosizing-row-height-images/table06.png "Ajout d’une classe personnalisée pour le UITableViewController, le UITableView et le UITableCell")](autosizing-row-height-images/table06.png#lightbox)

Enfin, pour vous assurer que tous les prévus contenu s’affiche dans notre étiquette, définissez la **lignes** propriété `0`:

[![](autosizing-row-height-images/table06.png "La propriété de lignes est définie sur 0")](autosizing-row-height-images/table06a.png#lightbox)

Avec l’interface utilisateur définie, nous allons ajouter le code pour activer le redimensionnement de hauteur de ligne automatique.

## <a name="enabling-auto-resizing-height"></a>L’activation de hauteur de redimensionnement automatique

Dans le de source de données notre Table de vue (`UITableViewDatasource`) ou Source (`UITableViewSource`), lorsque nous la file d’attente d’une cellule, nous devons utiliser le `Identifier` que nous avons défini dans le concepteur. Exemple :

```csharp
public string CellID {
    get { return "GrowCell"; }
}
...

public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (CellID, indexPath) as GrowRowTableCell;
    var item = Items [indexPath.Row];

    // Setup
    cell.Image = UIImage.FromFile(item.ImageName);
    cell.Title = item.Title;
    cell.Description = item.Description;

    return cell;
}
```

Par défaut, la vue Table définira pour le redimensionnement automatique de hauteur de ligne. Pour garantir cela, le `RowHeight` propriété doit être définie sur `UITableView.AutomaticDimension`. Nous devons également définir le `EstimatedRowHeight` propriété dans notre `UITableViewController`. Exemple :

```csharp
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Initialize table
    TableView.DataSource = new GrowRowTableDataSource(this);
    TableView.Delegate = new GrowRowTableDelegate (this);
    TableView.RowHeight = UITableView.AutomaticDimension;
    TableView.EstimatedRowHeight = 40f;
    TableView.ReloadData ();
}
```

Cette estimation ne doit pas être exacte, une estimation de la hauteur moyenne de chaque ligne dans la vue de Table.

Avec ce code en place, lors de l’application est exécutée, chaque ligne sera réduire et augmenter en fonction de la hauteur de la dernière étiquette dans le Prototype de la cellule. Exemple :

[![](autosizing-row-height-images/table07.png "Un exemple de table exécuter")](autosizing-row-height-images/table07.png#lightbox)


## <a name="related-links"></a>Liens associés

- [GrowRowTable (exemple)](https://developer.xamarin.com/samples/monotouch/GrowRowTable/)
