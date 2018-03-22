---
title: Hauteur de ligne de redimensionnement automatique
ms.topic: article
ms.prod: xamarin
ms.assetid: CE45A385-D40A-482A-90A0-E8382C2BFFB9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: f1b35905d14086dcfc0cb749c8e4cc7de1608dd5
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="auto-sizing-row-height"></a>Hauteur de ligne de redimensionnement automatique

Depuis iOS 8, Apple ajouté la possibilité de créer une vue de Table (`UITableView`) qui peut augmenter et réduire la hauteur d’une ligne donnée, selon la taille de son contenu à l’aide de la disposition automatique, les Classes de taille et les contraintes automatiquement.

iOS 11 a ajouté la possibilité pour les lignes développer automatiquement. Cellules, en-têtes et pieds de page peuvent maintenant être redimensionnées automatiquement en fonction de leur contenu. Toutefois, si votre table est créée dans l’Interface générateur, iOS concepteur, ou si elle a résolu les hauteurs de lignes vous devez activer manuellement self dimensionnement des cellules, comme décrit dans ce guide.

## <a name="cell-layout-in-the-ios-designer"></a>Disposition de la cellule dans le concepteur iOS

Ouvrez le plan conceptuel pour l’affichage de la Table que vous souhaitez disposer de redimensionnement automatique de la ligne pour dans le concepteur, iOS sélectionnez la cellule *Prototype* et concevoir la disposition de la cellule. Exemple :

[![](autosizing-row-height-images/table01.png "Création du Prototype de la cellule")](autosizing-row-height-images/table01.png#lightbox)

Pour chaque élément dans le Prototype, ajouter des contraintes pour conserver les éléments dans la position correcte, comme l’affichage de la Table est redimensionnée pour rotation ou iOS différentes tailles d’écran de périphérique. Par exemple, l’épinglage le `Title` vers le haut, gauche et droite de la cellule *affichage du contenu*:

[![](autosizing-row-height-images/table02.png "Épingler le titre en haut, gauche et droite de l’affichage de contenu des cellules")](autosizing-row-height-images/table02.png#lightbox)

Dans le cas de notre exemple de table, la petite `Label` (sous le `Title`) est le champ que vous pouvez réduire et s’agrandir pour augmenter ou diminuer la hauteur de ligne. Pour parvenir à cet effet, ajoutez les contraintes suivantes pour épingler gauche, droite, haut et bas de l’étiquette :

[![](autosizing-row-height-images/table03.png "Ces contraintes pour épingler gauche, droite, haut et bas de l’étiquette")](autosizing-row-height-images/table03.png#lightbox)

Maintenant que nous avons entièrement restreint les éléments dans la cellule, nous devons préciser que l’élément doit être étirée. Pour ce faire, définissez la **contenu priorité Hugging** et **priorité de résistance à la Compression de contenu** en fonction des besoins dans les **disposition** section de la zone de propriétés :

[![](autosizing-row-height-images/table03a.png "La section disposition de la zone de propriétés")](autosizing-row-height-images/table03a.png#lightbox)

Définir l’élément que vous souhaitez développer pour avoir un **inférieure** valeur de priorité de Hugging et un **inférieure** valeur de priorité de résistance à la Compression.

Ensuite, nous devons sélectionner le Prototype de la cellule et lui donner un unique **identificateur**:

[![](autosizing-row-height-images/table04.png "En donnant le Prototype de la cellule en un identificateur unique")](autosizing-row-height-images/table04.png#lightbox)

Dans le cas de notre exemple, `GrowCell`. Nous allons utiliser cette valeur ultérieurement, lorsque nous remplir la table.

> [!IMPORTANT]
> Si votre table contient plus d’un type de cellule (**Prototype**), vous devez vous assurer de chaque type possède son propre `Identifier` pour le redimensionnement de ligne automatique fonctionne.

Pour chaque élément de notre Prototype de la cellule, vous devez affecter un **nom** pour l’exposer au code c#. Exemple :

[![](autosizing-row-height-images/table05.png "Attribuez un nom pour l’exposer au code c#")](autosizing-row-height-images/table05.png#lightbox)

Ensuite, ajoutez une classe personnalisée pour le `UITableViewController`, le `UITableView` et `UITableCell` (Prototype). Exemple : 

[![](autosizing-row-height-images/table06.png "Ajout d’une classe personnalisée pour le UITableViewController, le UITableView et le UITableCell")](autosizing-row-height-images/table06.png#lightbox)

Enfin, pour vous assurer que tous les attendue de contenu est affiché dans notre étiquette, définissez la **lignes** propriété `0`:

[![](autosizing-row-height-images/table06.png "La propriété de lignes est définie sur 0")](autosizing-row-height-images/table06a.png#lightbox)

Avec l’interface utilisateur définie, vous allez ajouter le code pour activer le redimensionnement de hauteur de ligne automatique.

## <a name="enabling-auto-resizing-height"></a>L’activation de hauteur de redimensionnement automatique

Dans le de source de données notre Table de vue (`UITableViewDatasource`) ou la Source (`UITableViewSource`), lorsque nous dequeue une cellule que nous devons utiliser la `Identifier` que nous avons défini dans le concepteur. Exemple :

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

Par défaut, la vue de la Table seront définie pour la hauteur de ligne de redimensionnement automatique. Pour garantir cela, le `RowHeight` propriété doit être définie sur `UITableView.AutomaticDimension`. Nous devons également définir le `EstimatedRowHeight` propriété dans notre `UITableViewController`. Exemple :

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

Cette estimation ne doit pas nécessairement être exacte, une estimation de la hauteur moyenne de chaque ligne dans la vue de la Table.

Avec ce code en place, lorsque l’application est exécutée, chaque ligne sera réduire et augmenter en fonction de la hauteur de la dernière étiquette dans le Prototype de la cellule. Exemple :

[![](autosizing-row-height-images/table07.png "Un exemple de table exécuter")](autosizing-row-height-images/table07.png#lightbox)


## <a name="related-links"></a>Liens associés

- [GrowRowTable (sample)](https://developer.xamarin.com/samples/monotouch/GrowRowTable/)
