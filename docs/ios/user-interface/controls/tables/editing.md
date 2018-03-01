---
title: Modification
ms.topic: article
ms.prod: xamarin
ms.assetid: EC197F25-E865-AFA3-E5CF-B33FAB7744A0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 1ea4489cd6f9839d5d32c97aa7ded41e4f15538a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="editing"></a>Modification

Les fonctionnalités de modification de table sont activées par substitution de méthodes dans un `UITableViewSource` sous-classe. Le comportement d’édition la plus simple est le mouvement de balayage à supprimer peut être implémenté avec une seule méthode de remplacement.
Modification des plus complexes (y compris les lignes mobiles) peut faire avec la table en mode édition.

Ce guide présente les éléments suivants :

- [Effectuez un balayage à supprimer](#Swipe_to_Delete)
- [Mode Édition](#Edit_Mode)
- [Style de modification de ligne d’Insertion](#row_insertion_editing_style)

<a name="Swipe_to_delete" />

## <a name="swipe-to-delete"></a>Effectuez un balayage à supprimer

Le passage de supprimer la fonctionnalité est un mouvement naturel dans iOS que les utilisateurs s’attendent. 

 [ ![](editing-images/image10.png "Exemple de balayage à supprimer")](editing-images/image10.png)

Il existe trois substitutions de méthode qui affectent le mouvement de balayage pour afficher un **supprimer** bouton dans une cellule :

-   **CommitEditingStyle** – la source de table détecte si cette méthode est substituée et active automatiquement le mouvement de balayage à supprimer. Implémentation de la méthode doit appeler `DeleteRows` sur la `UITableView` provoquer les cellules à disparaisse et également supprimer les données sous-jacentes de votre modèle (par exemple, un tableau, dictionnaire ou base de données). 
-   **CanEditRow** – CommitEditingStyle si est substituée, toutes les lignes sont supposés pour être modifiable. Si cette méthode est implémentée et retourne la valeur false (pour certaines des lignes spécifiques, ou pour toutes les lignes) le mouvement de balayage à supprimer est alors disponible dans la cellule. 
-   **TitleForDeleteConfirmation** – le cas échéant Spécifie le texte de la **supprimer** bouton. Si cette méthode n’est pas implémentée le texte du bouton sera « Delete ». 


Ces méthodes sont implémentées dans la `TableSource` classe suit :

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

<a name="Edit_mode" />

## <a name="edit-mode"></a>Mode Édition

Lorsqu’une table est en mode édition l’utilisateur voit un widget rouge « stop » sur chaque ligne, ce qui permet de révéler un bouton Supprimer lors d’un contact. Le tableau affiche également une icône de « handle » pour indiquer que la ligne peut être déplacée pour modifier l’ordre.
Le **TableEditMode** exemple implémente ces fonctionnalités, comme indiqué.

 [ ![](editing-images/image11.png "L’exemple TableEditMode implémente ces fonctionnalités, comme indiqué")](editing-images/image11.png)

Il existe de nombreuses méthodes différentes sur `UITableViewSource` qui affectent le comportement en mode de modification d’une table :

-   **CanEditRow** : indique si chaque ligne peut être modifiée. Retourne la valeur false pour empêcher le balayage à supprimer et suppression en mode édition. 
-   **CanMoveRow** – retour true pour activer la poignée de déplacement' ' ou false pour empêcher le déplacement. 
-   **EditingStyleForRow** : lors de la table est en mode édition, la valeur de retour de cette méthode détermine si la cellule affiche l’icône de suppression rouge ou vert icône d’ajout. Retourner `UITableViewCellEditingStyle.None` si la ligne ne doit pas être modifiée. 
-   **MoveRow** – appelée lorsqu’une ligne est déplacée afin que la structure de données sous-jacente peut être modifiée pour faire correspondre les données, tel qu’il est affiché dans la table. 


L’implémentation pour les trois premières méthodes est relativement simple, sauf si vous souhaitez utiliser le `indexPath` pour modifier le comportement des lignes spécifiques, coder en dur simplement le valeurs de retour de la table entière.

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

Le `MoveRow` implémentation est un peu plus compliquée, car il a besoin de modifier la structure de données sous-jacente pour correspondre à la nouvelle commande. Étant donné que les données sont implémentées comme un `List` le code ci-dessous supprime l’élément de données à son ancien emplacement et l’insère dans le nouvel emplacement. Si les données a été stockées dans une table de base de données SQLite avec une colonne « order » (par exemple), cette méthode à la place devez effectuer certaines opérations SQL pour réorganiser les numéros de cette colonne.

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

Enfin, pour obtenir la table en mode édition, le **modifier** bouton doit appeler `SetEditing` comme suit

```csharp
table.SetEditing (true, true);
```

et lorsque l’utilisateur est terminé modification, le **fait** bouton doit désactiver le mode de modification :

```csharp
table.SetEditing (false, true);
```

<a name="Edit_mode_–_row_insertion_editing_style" />

## <a name="row-insertion-editing-style"></a>Style de modification de ligne d’Insertion

Insertion de ligne à partir de la table est une interface utilisateur rare, l’exemple principal dans les applications iOS standard est la **modifier le Contact** écran. Cette capture d’écran illustre le fonctionne de la fonctionnalité d’insertion de ligne : en cours de modification supplémentaire des lignes que (clic) est le mode insère des lignes supplémentaires dans les données. Lorsque la modification est terminée, la fichier temporaire **(Ajouter)** ligne est supprimée.

 [ ![](editing-images/image12.png "Lors de la modification est terminée, la variable temporaire ajouter une nouvelle ligne est supprimée.")](editing-images/image12.png)

Il existe de nombreuses méthodes différentes sur `UITableViewSource` qui affectent le comportement en mode de modification d’une table. Ces méthodes ont été implémentées comme suit dans l’exemple de code :

-   **EditingStyleForRow** – retourne `UITableViewCellEditingStyle.Delete` pour les lignes contenant des données et retourne `UITableViewCellEditingStyle.Insert` pour la dernière ligne (qui sera ajoutée spécifiquement pour se comporter comme un bouton Insérer). 
-   **CustomizeMoveTarget** – pendant que l’utilisateur se déplace d’une cellule de la valeur de retour de cette méthode facultatif peut remplacer de l’emplacement de leur choix. Cela signifie que vous pouvez empêcher les « suppression » à partir de la cellule dans certaines positions – telles que cet exemple qui empêche le déplacement après n’importe quelle ligne du **(Ajouter)** ligne. 
-   **CanMoveRow** – retour true pour activer la poignée de déplacement' ' ou false pour empêcher le déplacement. Dans l’exemple, la dernière ligne a le handle' move' masqué, car il est destiné au serveur en tant que bouton Insérer uniquement. 


Nous ajoutons également deux méthodes personnalisées pour ajouter la ligne 'insert' et la supprimer à nouveau lorsque n’est plus nécessaire. Elles sont appelées à partir de la **modifier** et **fait** boutons :

-   **WillBeginTableEditing** : lorsque le **modifier** bouton est touchées il appelle `SetEditing` pour que la table en mode édition. Cela déclenche la méthode WillBeginTableEditing où nous affichons les **(Ajouter)** ligne à la fin de la table d’agir comme un bouton d’insertion. 
-   **DidFinishTableEditing** : lorsque le bouton terminé est touché `SetEditing` est rappelée pour désactiver le mode édition. L’exemple de code supprime le **(Ajouter)** ligne à partir de la table lors de la modification n’est plus nécessaire. 


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

Ces deux méthodes personnalisées sont utilisées pour ajouter et supprimer les **(Ajouter)** ligne lorsque le mode d’édition de la table est activé ou désactivé :

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

Enfin, ce code instancie le **modifier** et **fait** boutons, avec les expressions lambda qui activent ou désactivent le mode d’édition lorsqu’ils sont couvertes :

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

Ce modèle de l’interface utilisateur de ligne d’insertion n’est pas utilisé très souvent, cependant, vous pouvez également utiliser le `UITableView.BeginUpdates` et `EndUpdates` méthodes pour animer l’insertion ou la suppression de cellules dans une table. La règle pour l’utilisation de ces méthodes est que la différence de valeur retournée par `RowsInSection` entre les `BeginUpdates` et `EndUpdates` appels doivent correspondre au nombre net de cellules ajoutées/supprimées avec la `InsertRows` et `DeleteRows` méthodes. Si la source de données sous-jacent n’est pas modifiée pour correspondre à des insertions/suppressions sur la vue de table, une erreur se produit.


## <a name="related-links"></a>Liens associés

- [WorkingWithTables (exemple)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
