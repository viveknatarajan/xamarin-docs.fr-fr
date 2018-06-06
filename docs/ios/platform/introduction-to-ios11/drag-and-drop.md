---
title: Glisser- déposer dans Xamarin.iOS
description: Ce document décrit comment implémenter une opération glisser-déplacer dans les applications de Xamarin.iOS à l’aide de l’API introduites dans iOS 11. En particulier, elle décrit l’activation de glisser- déposer dans UITableView.
ms.prod: xamarin
ms.assetid: 0D39C4C3-D169-42F8-B3FA-7F98CF0B6F1F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/05/2016
ms.openlocfilehash: 7c41f96dae88047e64ec1e74838e3efab55958cc
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786962"
---
# <a name="drag-and-drop-in-xamarinios"></a>Glisser- déposer dans Xamarin.iOS

_Implémentation de glisser -déplacer pour iOS 11_

iOS 11 inclut le glisser-déplacer pour copier des données entre applications sur un iPad. Les utilisateurs peuvent sélectionner et faites glisser tous les types de contenu à partir d’applications positionnées côte à côte ou en faisant glisser sur une icône d’application qui déclenchera l’application pour ouvrir et que les données à supprimer :

![Faites glisser et déposez l’exemple d’application personnalisée dans une application de notes de publication](drag-and-drop-images/drag-drop-sml.png)

> [!NOTE]
> Glisser -déplacer est disponible uniquement dans la même application sur un iPhone.

Envisagez la prise en charge du glisser opérations déposer n’importe où le contenu peut être créé ou modifié :

- Contrôles de texte prend en charge la fonction glisser- déposer pour toutes les applications générées sur iOS 11, sans aucun travail supplémentaire.
- Vues des tables et vues de collection des améliorations dans iOS 11 qui simplifient l’ajout de glisser et déposez le comportement.
- Toute autre vue peut être effectuée pour prendre en charge du glisser-déplacer avec une personnalisation supplémentaire.

Lors de l’ajout de glisser- déposer prend en charge à vos applications, vous pouvez fournir différents niveaux de fidélité contenue ; par exemple, vous pouvez fournir un texte mis en forme et la version en texte brut des données afin que l’application réceptrice peut choisir qui s’intègre mieux cible. Il est également possible de personnaliser la visualisation de glisser et également pour permettre le déplacement de plusieurs éléments à la fois.

## <a name="drag-and-drop-with-text-controls"></a>Faites glisser et déposez les contrôles de texte

`UITextView` et `UITextField` prennent en charge automatiquement en faisant glisser le texte sélectionné et déplacer du texte contenu dans.

<a name="uitableview" />

## <a name="drag-and-drop-with-uitableview"></a>Glisser -déplacer avec UITableView

`UITableView` a la gestion intégrée de glisser- déposer des interactions avec les lignes de la table, exigeant uniquement quelques méthodes pour activer le comportement par défaut.

Il existe deux interfaces impliquées :

- `IUITableViewDragDelegate` : Informations des packages lorsqu’une opération de glissement est initialisée dans la vue de la table.
- `IUITableViewDropDelegate` – Traite les informations lors de la suppression d’une tentative et à finaliser.

Dans le [DragAndDropTableView exemple](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/) ces deux interfaces sont implémentés sur le `UITableViewController` (classe), ainsi que la source de données et de délégué. Elles sont affectées la `ViewDidLoad` méthode :

```csharp
this.TableView.DragDelegate = this;
this.TableView.DropDelegate = this;
```

Le code minimal requis pour ces deux interfaces est expliqué ci-dessous.

### <a name="table-view-drag-delegate"></a>Table vue glisser délégué

La seule méthode _requis_ pour prendre en charge le faisant glisser une ligne d’une vue de table est `GetItemsForBeginningDragSession`. Si l’utilisateur commence à faire glisser une ligne, cette méthode est appelée.

Vous trouverez ci-dessous une implémentation. Il récupère les données associées à la ligne déplacée, qu’elle l’encode et configure une `NSItemProvider` qui détermine la façon dont les applications traite la partie « suppression » de l’opération (par exemple, si elles peuvent traiter le type de données, `PlainText`, dans l’exemple) :

```csharp
public UIDragItem[] GetItemsForBeginningDragSession (UITableView tableView,
  IUIDragSession session, NSIndexPath indexPath)
{
  // gets the 'information' to be dragged
  var placeName = model.PlaceNames[indexPath.Row];
  // convert to NSData representation
  var data = NSData.FromString(placeName, NSStringEncoding.UTF8);
  // create an NSItemProvider to describe the data
  var itemProvider = new NSItemProvider();
  itemProvider.RegisterDataRepresentation(UTType.PlainText,
                                NSItemProviderRepresentationVisibility.All,
                                (completion) =>
  {
    completion(data, null);
    return null;
  });
  // wrap in a UIDragItem
  return new UIDragItem[] { new UIDragItem(itemProvider) };
}
```

Il existe de nombreuses méthodes facultatif sur le délégué de glissement qui peut être implémenté pour personnaliser le comportement de glisser, tel que fournir plusieurs représentations sous forme de données qui peut être exploitée dans les applications cibles (tels que du texte mis en forme, ainsi que du texte brut, ou un vecteur et bitmap des versions d’un dessin). Vous pouvez également fournir des représentations sous forme de données personnalisés à utiliser lors de glisser -déplacer dans la même application.

### <a name="table-view-drop-delegate"></a>Délégué de Drop table vue

Les méthodes sur le délégué de la liste sont appelées lorsqu’une opération de glissement se produit sur une vue de table, ou qu’il se termine au-dessus de lui. Les méthodes requises déterminent si les données sont autorisées à supprimer, et les actions prises si la suppression est terminée :

- `CanHandleDropSession` – Pendant une opération de glissement est en cours, et éventuellement en cours de suppression sur l’application, cette méthode détermine si les données glissées sont autorisées à supprimer.
- `DropSessionDidUpdate` – Pendant l’opération glisser est en cours, cette méthode est appelée pour déterminer quelle action est destinée. Pour plus d’informations à partir de la vue table glissé sur la session de glissement et le chemin d’accès d’index possibles peuvent utilisés pour déterminer le comportement et une rétroaction visuelle à l’utilisateur.
- `PerformDrop` – Lorsque l’utilisateur termine la liste déroulante (par levage leur doigt), cette méthode extrait les données déplacées et modifie la vue de table pour ajouter les données dans une nouvelle ligne (ou lignes).

#### <a name="canhandledropsession"></a>CanHandleDropSession

`CanHandleDropSession` Indique si la vue de table peut accepter les données glissées. Dans cet extrait de code, `CanLoadObjects` est utilisé pour vérifier que cette vue de table peut accepter des données de chaîne.

```csharp
public bool CanHandleDropSession(UITableView tableView, IUIDropSession session)
{
  return session.CanLoadObjects(typeof(NSString));
}
```

#### <a name="dropsessiondidupdate"></a>DropSessionDidUpdate

Le `DropSessionDidUpdate` méthode est appelée à plusieurs reprises pendant que l’opération de glissement est en cours d’exécution, pour fournir des signaux visuels à l’utilisateur.

Dans le code ci-dessous, `HasActiveDrag` est utilisé pour déterminer si l’opération dans l’affichage actuel de la table. Dans ce cas, uniquement les lignes uniques sont autorisés à déplacer.
Si l’opération glisser provient d’une autre source, une opération de copie sera indiquée :

```csharp
public UITableViewDropProposal DropSessionDidUpdate(UITableView tableView, IUIDropSession session, NSIndexPath destinationIndexPath)
{
  // The UIDropOperation.Move operation is available only for dragging within a single app.
  if (tableView.HasActiveDrag)
  {
    if (session.Items.Length > 1)
    {
        return new UITableViewDropProposal(UIDropOperation.Cancel);
    } else {
        return new UITableViewDropProposal(UIDropOperation.Move, UITableViewDropIntent.InsertAtDestinationIndexPath);
    }
  } else {
    return new UITableViewDropProposal(UIDropOperation.Copy, UITableViewDropIntent.InsertAtDestinationIndexPath);
  }
}
```

L’opération de suppression peut prendre l’une des `Cancel`, `Move`, ou `Copy`.

L’objectif de la liste peut être pour insérer une nouvelle ligne, ou ajouter/ajouter des données à une ligne existante.

#### <a name="performdrop"></a>PerformDrop

Le `PerformDrop` méthode est appelée lorsque l’utilisateur termine l’opération et modifie la source de données et la vue table afin de refléter les données déplacées.

```csharp
public void PerformDrop(UITableView tableView, IUITableViewDropCoordinator coordinator)
{
  NSIndexPath indexPath, destinationIndexPath;
  if (coordinator.DestinationIndexPath != null)
  {
    indexPath = coordinator.DestinationIndexPath;
    destinationIndexPath = indexPath;
  }
  else
  {
    // Get last index path of table view
    var section = tableView.NumberOfSections() - 1;
    var row = tableView.NumberOfRowsInSection(section);
    destinationIndexPath = NSIndexPath.FromRowSection(row, section);
  }
  coordinator.Session.LoadObjects(typeof(NSString), (items) =>
  {
    // Consume drag items
    List<string> stringItems = new List<string>();
    foreach (var i in items)
    {
      var q = NSString.FromHandle(i.Handle);
      stringItems.Add(q.ToString());
    }
    var indexPaths = new List<NSIndexPath>();
    for (var j = 0; j < stringItems.Count; j++)
    {
      var indexPath1 = NSIndexPath.FromRowSection(destinationIndexPath.Row + j, destinationIndexPath.Section);
      model.AddItem(stringItems[j], indexPath1.Row);
      indexPaths.Add(indexPath1);
    }
    tableView.InsertRows(indexPaths.ToArray(), UITableViewRowAnimation.Automatic);
  });
}
```

Code supplémentaire peut être ajouté pour charger des objets de données volumineuses de façon asynchrone.

### <a name="testing-drag-and-drop"></a>Test de glisser- déposer

Vous devez utiliser un iPad pour tester le [exemple](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/).
Ouvrez l’exemple avec une autre application (par exemple, les Notes) et faites glisser les lignes et le texte entre eux :

![capture d’écran de l’opération de glissement en cours](drag-and-drop-images/01-sml.png)


## <a name="related-links"></a>Liens associés

- [Faites glisser et déposez les instructions de l’Interface utilisateur (Apple)](https://developer.apple.com/ios/human-interface-guidelines/interaction/drag-and-drop/)
- [Faites glisser et déposez l’exemple d’affichage de tableau](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/)
- [Exemple de vue de Collection par glisser- déposer](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCollectionView)
- [Présentation de glisser-déplacer (WWDC) (vidéo)](https://developer.apple.com/videos/play/wwdc2017/203/)
- [Glisser -déplacer avec la collecte et l’affichage de tableau (WWDC) (vidéo)](https://developer.apple.com/videos/play/wwdc2017/223/)
