---
title: Modification des Tables avec Xamarin.iOS
description: Ce document décrit comment modifier des tables dans Xamarin.iOS. Il aborde balayez pour supprimer, modifier le mode et l’insertion de ligne.
ms.prod: xamarin
ms.assetid: EC197F25-E865-AFA3-E5CF-B33FAB7744A0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 1267de341a88130c18254f414d2fbb1c42595a0c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61195900"
---
# <a name="editing-tables-with-xamarinios"></a>Modification des Tables avec Xamarin.iOS

Les fonctionnalités de modification de table sont activées en remplaçant les méthodes dans un `UITableViewSource` sous-classe. Le comportement d’édition la plus simple est le mouvement de balayage à supprimer peut être implémenté avec une substitution de méthode unique.
Modification de plus complexe (y compris les lignes déplacement) peut faire avec la table en mode édition.

## <a name="swipe-to-delete"></a>Effectuez un balayage à supprimer

Le passage à supprimer la fonctionnalité est un mouvement naturels dans iOS que les utilisateurs attendent. 

 [![](editing-images/image10.png "Exemple de passage à supprimer")](editing-images/image10.png#lightbox)

Il existe trois substitutions de méthode qui affectent le mouvement de balayage pour afficher un **supprimer** bouton dans une cellule :

-   **CommitEditingStyle** – la source de table détecte si cette méthode est substituée et active automatiquement le mouvement de balayage à supprimer. Implémentation de la méthode doit appeler `DeleteRows` sur la `UITableView` pour provoquer des cellules à disparaître et également de supprimer les données sous-jacentes à partir de votre modèle (par exemple, un tableau, dictionnaire ou base de données). 
-   **CanEditRow** – si CommitEditingStyle est substitué, toutes les lignes sont supposées pour être modifiable. Si cette méthode est implémentée et retourne la valeur false (pour certaines des lignes spécifiques ou pour toutes les lignes) le mouvement de balayage à supprimer est alors disponible dans cette cellule. 
-   **TitleForDeleteConfirmation** – le cas échéant Spécifie le texte de la **supprimer** bouton. Si cette méthode n’est pas implémentée le texte du bouton sera « Supprimer ». 


Ces méthodes sont implémentées dans le `TableSource` classe suit :

```csharp
public override void CommitEditingStyle (UITableView tableView, UITableViewCellEditingStyle editingStyle, Foundation.NSIndexPath indexPath)
{
    switch (editingStyle) {
        case UITableViewCellEditingStyle.Delete:
            // remove the item from the underlying data source
            tableItems.RemoveAt(indexPath.Row);
            // delete the row from the table
            tableView.DeleteRows (new NSIndexPath[] { indexPath }, UITableViewRowAnimation.Fade);
            break;
        case UITableViewCellEditingStyle.None:
            Console.WriteLine ("CommitEditingStyle:None called");
            break;
    }
}
public override bool CanEditRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you wish to disable editing for a specific indexPath or for all rows
}
public override string TitleForDeleteConfirmation (UITableView tableView, NSIndexPath indexPath)
{   // Optional - default text is 'Delete'
    return "Trash (" + tableItems[indexPath.Row].SubHeading + ")";
}
```

Pour cet exemple le `UITableViewSource` a été mis à jour pour utiliser un `List<TableItem>` (au lieu d’un tableau de chaînes) comme source de données, car il prend en charge Ajout et suppression d’éléments de la collection.


## <a name="edit-mode"></a>Mode d’édition

Lorsqu’une table est en mode édition l’utilisateur voit un widget rouge « stop » sur chaque ligne, ce qui fait apparaître un bouton Supprimer au toucher. Le tableau affiche également une icône de « handle » pour indiquer que la ligne peut être déplacée pour modifier l’ordre.
Le **TableEditMode** exemple implémente ces fonctionnalités, comme indiqué.

 [![](editing-images/image11.png "L’exemple TableEditMode implémente ces fonctionnalités, comme indiqué")](editing-images/image11.png#lightbox)

Il existe plusieurs méthodes différentes sur `UITableViewSource` qui affectent le comportement de mode de modification d’une table :

-   **CanEditRow** : indique si chaque ligne peut être modifiée. Retourne la valeur false pour empêcher le passage à supprimer et suppression en mode édition. 
-   **CanMoveRow** – retour true pour activer le « poignée de déplacement » ou la valeur false pour empêcher le déplacement. 
-   **EditingStyleForRow** : lors de la table est en mode édition, la valeur de retour à partir de cette méthode détermine si la cellule affiche l’icône de suppression rouge ou vert icône d’ajout. Retourner `UITableViewCellEditingStyle.None` si la ligne ne doit pas être modifiable. 
-   **MoveRow** – appelée lorsqu’une ligne est déplacée afin que la structure de données sous-jacente peut être modifiée pour correspondre aux données tel qu’il est affiché dans la table. 


L’implémentation pour les trois premières méthodes est relativement simple :, sauf si vous souhaitez utiliser le `indexPath` pour modifier le comportement de lignes spécifiques, simplement coder en dur le valeurs de retour pour la table entière.

```csharp
public override bool CanEditRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you wish to disable editing for a specific indexPath or for all rows
}
public override bool CanMoveRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you don't allow re-ordering
}
public override UITableViewCellEditingStyle EditingStyleForRow (UITableView tableView, NSIndexPath indexPath)
{
    return UITableViewCellEditingStyle.Delete; // this example doesn't support Insert
}
```

Le `MoveRow` implémentation est un peu plus compliquée, car il a besoin de modifier la structure de données sous-jacente pour correspondre à la nouvelle commande. Étant donné que les données sont implémentées comme un `List` le code ci-dessous supprime l’élément de données à son ancien emplacement et l’insère dans le nouvel emplacement. Si les données a été stockées dans une table de base de données SQLite avec une colonne « order » (par exemple), cette méthode doit à la place effectuer certaines opérations SQL pour réorganiser les numéros de cette colonne.

```csharp
public override void MoveRow (UITableView tableView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
    var item = tableItems[sourceIndexPath.Row];
    var deleteAt = sourceIndexPath.Row;
    var insertAt = destinationIndexPath.Row;
    
    // are we inserting 
    if (destinationIndexPath.Row < sourceIndexPath.Row) {
        // add one to where we delete, because we're increasing the index by inserting
        deleteAt += 1;
    } else {
        // add one to where we insert, because we haven't deleted the original yet
        insertAt += 1;
    }
    tableItems.Insert (insertAt, item);
    tableItems.RemoveAt (deleteAt);
}
```

Enfin, pour obtenir la table en mode édition, la **modifier** bouton doit appeler `SetEditing` comme suit

```csharp
table.SetEditing (true, true);
```

et lorsque l’utilisateur est terminé modification, le **fait** bouton doit désactiver le mode d’édition :

```csharp
table.SetEditing (false, true);
```


## <a name="row-insertion-editing-style"></a>Style de modification de ligne d’Insertion

Insertion de ligne à partir de la table est une interface utilisateur rare : l’exemple principal dans les applications iOS standard est la **/modifier un Contact** écran. Cette capture d’écran illustre le fonctionnement de la fonctionnalité d’insertion de ligne : en cours de modification mode, il existe une autre ligne que (clic) insère des lignes supplémentaires dans les données. Lorsque la modification est terminée, la fichier temporaire **(Ajouter)** ligne est supprimée.

 [![](editing-images/image12.png "Lors de la modification est terminée, la fichier temporaire ajouter une nouvelle ligne est supprimée.")](editing-images/image12.png#lightbox)

Il existe plusieurs méthodes différentes sur `UITableViewSource` qui affectent le comportement de mode de modification d’une table. Ces méthodes ont été implémentées comme suit dans l’exemple de code :

-   **EditingStyleForRow** – retourne `UITableViewCellEditingStyle.Delete` pour les lignes contenant des données et retourne `UITableViewCellEditingStyle.Insert` pour la dernière ligne (qui sera ajoutée spécifiquement à se comporter comme un bouton d’insertion). 
-   **CustomizeMoveTarget** : tandis que l’utilisateur déplace une cellule de la valeur de retour à partir de cette méthode facultative peut substituer leur choix de l’emplacement. Cela signifie que vous pouvez les empêcher de « suppression » la cellule dans certaines positions – comme dans cet exemple qui empêche toute ligne d’être déplacé après le **(Ajouter)** ligne. 
-   **CanMoveRow** – retour true pour activer le « poignée de déplacement » ou la valeur false pour empêcher le déplacement. Dans l’exemple, la dernière ligne a la « poignée de déplacement' masquée, car il est destiné au serveur comme un bouton d’insertion. 


Nous avons également ajouter deux méthodes personnalisées pour ajouter la ligne 'insert' et les supprimer à nouveau lorsque n’est plus nécessaire. Elles sont appelées à partir de la **modifier** et **fait** boutons :

-   **WillBeginTableEditing** : lorsque le **modifier** bouton est touchées il appelle `SetEditing` pour que la table en mode édition. Cela déclenche la méthode WillBeginTableEditing où nous affichons le **(Ajouter)** ligne à la fin de la table d’agir comme un bouton d’insertion. 
-   **DidFinishTableEditing** : lorsque le bouton terminé est touché `SetEditing` est rappelée pour désactiver le mode d’édition. L’exemple de code supprime le **(Ajouter)** ligne à partir de la table lors de la modification n’est plus nécessaire. 


Ces substitutions de méthode sont implémentées dans l’exemple de fichier **TableEditModeAdd/Code/TableSource.cs**:

```csharp
public override UITableViewCellEditingStyle EditingStyleForRow (UITableView tableView, NSIndexPath indexPath)
{
    if (tableView.Editing) {
        if (indexPath.Row == tableView.NumberOfRowsInSection (0) - 1)
            return UITableViewCellEditingStyle.Insert;
        else
            return UITableViewCellEditingStyle.Delete;
    } else // not in editing mode, enable swipe-to-delete for all rows
        return UITableViewCellEditingStyle.Delete;
}
public override NSIndexPath CustomizeMoveTarget (UITableView tableView, NSIndexPath sourceIndexPath, NSIndexPath proposedIndexPath)
{
    var numRows = tableView.NumberOfRowsInSection (0) - 1; // less the (add new) one
    if (proposedIndexPath.Row >= numRows)
        return NSIndexPath.FromRowSection(numRows - 1, 0);
    else
        return proposedIndexPath;
} 
public override bool CanMoveRow (UITableView tableView, NSIndexPath indexPath)
{
    return indexPath.Row < tableView.NumberOfRowsInSection (0) - 1;
}
```

Ces deux méthodes personnalisées sont utilisées pour ajouter et supprimer les **(Ajouter)** ligne lors de la table du mode d’édition est activé ou désactivé :

```csharp
public void WillBeginTableEditing (UITableView tableView)
{
    tableView.BeginUpdates ();
    // insert the 'ADD NEW' row at the end of table display
    tableView.InsertRows (new NSIndexPath[] { 
            NSIndexPath.FromRowSection (tableView.NumberOfRowsInSection (0), 0) 
        }, UITableViewRowAnimation.Fade);
    // create a new item and add it to our underlying data (it is not intended to be permanent)
    tableItems.Add (new TableItem ("(add new)"));
    tableView.EndUpdates (); // applies the changes
}
public void DidFinishTableEditing (UITableView tableView)
{
    tableView.BeginUpdates ();
    // remove our 'ADD NEW' row from the underlying data
    tableItems.RemoveAt ((int)tableView.NumberOfRowsInSection (0) - 1); // zero based :)
    // remove the row from the table display
    tableView.DeleteRows (new NSIndexPath[] { NSIndexPath.FromRowSection (tableView.NumberOfRowsInSection (0) - 1, 0) }, UITableViewRowAnimation.Fade);
    tableView.EndUpdates (); // applies the changes
}
```

Enfin, ce code instancie le **modifier** et **fait** boutons, avec les expressions lambda qui activent ou désactivent le mode d’édition lorsqu’ils sont touchées :

```csharp
done = new UIBarButtonItem(UIBarButtonSystemItem.Done, (s,e)=>{
    table.SetEditing (false, true);
    NavigationItem.RightBarButtonItem = edit;
    tableSource.DidFinishTableEditing(table);
});
edit = new UIBarButtonItem(UIBarButtonSystemItem.Edit, (s,e)=>{
    if (table.Editing)
        table.SetEditing (false, true); // if we've half-swiped a row
    tableSource.WillBeginTableEditing(table);
    table.SetEditing (true, true);
    NavigationItem.LeftBarButtonItem = null;
    NavigationItem.RightBarButtonItem = done;
});
```

Ce modèle de l’interface utilisateur de ligne d’insertion n’est pas utilisé très souvent, cependant, vous pouvez également utiliser le `UITableView.BeginUpdates` et `EndUpdates` méthodes pour animer l’insertion ou la suppression de cellules dans une table. La règle pour l’utilisation de ces méthodes est que la différence de valeur retourné par `RowsInSection` entre le `BeginUpdates` et `EndUpdates` appels doivent correspondre au nombre net de cellules ajoutées/supprimées avec le `InsertRows` et `DeleteRows` méthodes. Si la source de données sous-jacent n’est pas modifiée pour correspondre à des insertions/suppressions sur la vue de table, une erreur se produit.


## <a name="related-links"></a>Liens associés

- [WorkingWithTables (sample)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
