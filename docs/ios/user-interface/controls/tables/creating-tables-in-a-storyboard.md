---
title: Utilisation de Tables dans le concepteur iOS
description: Dans les sections précédentes, nous Explorer développement à l’aide de Tables. Dans ce cas, la cinquième et dernière section, nous ce que nous avons appris jusqu'à présent d’agrégation et créer une application de liste de base à l’aide d’un plan conceptuel.
ms.prod: xamarin
ms.assetid: D8416E10-481A-0B6E-4081-B146E6358004
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 869719b1d3514fee4b45f61dcdb34a0bcf54f7b2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-tables-in-the-ios-designer"></a>Utilisation de Tables dans le concepteur iOS

Storyboards WYSIWYG permettent de créer des applications iOS et sont prises en charge dans Visual Studio sur Mac et Windows. Pour plus d’informations sur les plans conceptuels, reportez-vous à la [Introduction à Storyboards](~/ios/user-interface/storyboards/index.md) document. Storyboards permettent également de modifier les dispositions de cellule *dans* la table, ce qui simplifie le développement des tables et des cellules

Lorsque vous configurez les propriétés d’une vue de table dans le concepteur iOS, il existe deux types de contenu de la cellule que vous pouvez choisir de : **dynamique** ou **statique** contenu de Prototype.

<a name="Prototype_Content" />

## <a name="dynamic-prototype-content"></a>Contenu dynamique de Prototype

Un `UITableView` avec un prototype de contenu est généralement destiné à afficher une liste de données où la cellule prototype (ou les cellules, vous peuvent définir plusieurs) sont réutilisées pour chaque élément dans la liste. Les cellules n’avez pas besoin d’être instanciées, ils sont obtenus dans le `GetView` en appelant le `DequeueReusableCell` méthode de son `UITableViewSource`.

 <a name="Static_Content" />


## <a name="static-content"></a>Contenu statique

`UITableView`s avec un contenu statique permet des tables à être conçues à droite sur l’aire de conception. Les cellules peuvent être déplacés dans la table et personnalisés en modifiant des propriétés et ajout de contrôles.

 <a name="Creating_a_Storyboard-driven_app" />


## <a name="creating-a-storyboard-driven-app"></a>Création d’une application pilotée par table de montage séquentiel

L’exemple StoryboardTable contient une simple application maître / détail qui utilise les deux types de UITableView dans un plan conceptuel. Le reste de cette section décrit comment créer un exemple de liste de petites tâches qui ressemble à ceci lorsque vous avez terminé :

 [![Exemples d’écrans](creating-tables-in-a-storyboard-images/image13a.png)](creating-tables-in-a-storyboard-images/image13a.png#lightbox)

L’interface utilisateur sera généré avec un plan conceptuel, et les deux écrans utilisera un UITableView. L’écran principal utilise *contenu de prototype* à la disposition de la ligne et le détail écran utilise *contenu statique* pour créer un formulaire de saisie de données à l’aide de dispositions de cellule personnalisé.

## <a name="walkthrough"></a>Procédure pas à pas

Créer une nouvelle solution dans Visual Studio en utilisant **(créer) un nouveau projet... > unique vue App(C#)**et l’appeler _StoryboardTables_.

 [![Créer une boîte de dialogue Nouveau projet](creating-tables-in-a-storyboard-images/npd.png)](creating-tables-in-a-storyboard-images/npd.png#lightbox)

La solution s’ouvre avec certains fichiers c# et un `Main.storyboard` fichier déjà créé. Double-cliquez sur le `Main.storyboard` fichier pour l’ouvrir dans le concepteur iOS.

<a name="Modifying_the_Storyboard" />

## <a name="modifying-the-storyboard"></a>Modification de la table de montage séquentiel

Le plan conceptuel est modifié en trois étapes :

-  Tout d’abord, disposition requise afficher les contrôleurs et définir leurs propriétés.
-  Ensuite, créez votre interface utilisateur en faisant glisser des objets dans votre affichage.
-  Enfin, ajoutez la classe UIKit requise à chaque vue et donnez un nom à différents contrôles afin qu’ils peuvent être référencées dans le code.


Une fois l’animation terminée, le code peut être ajouté pour rendre tous les éléments de travail.

<a name="Layout_The_View_Controllers" />

### <a name="layout-the-view-controllers"></a>Disposition de l’affichage des contrôleurs

La première modification à la table de montage séquentiel est la suppression de la vue de détail existante et la remplacer par un UITableViewController. Procédez comme suit :

1.  Sélectionnez la barre en bas de la vue de contrôleur et le supprimer.
2.  Faites glisser un **Navigation contrôleur** et un **Table View Controller** sur le plan conceptuel à partir de la boîte à outils. 
3.  Créer un segue à partir du contrôleur d’affichage racine pour le second contrôleur de vue de Table qui vient d’être ajouté. Pour créer le segue, contrôle + glisser *à partir de la cellule de détail* à la UITableViewController récemment ajouté. Choisissez l’option **afficher*** sous **Segue de sélection**. 
4.  Sélectionnez le nouveau segue que vous avez créé et lui donner un identificateur de référence ce segue dans le code. Cliquez sur le segue et entrez `TaskSegue` pour le **identificateur** dans les **propriétés remplissage**, comme suit :    
  [![Dénomination segue dans Panneau de configuration de propriété](creating-tables-in-a-storyboard-images/image16a-sml.png)](creating-tables-in-a-storyboard-images/image16a.png#lightbox) 

5. Ensuite, configurez les deux vues de Table en les sélectionnant et à l’aide de la zone de propriétés. Veillez à sélectionner la vue et pas de vue contrôleur : vous pouvez utiliser la structure du Document pour aider à la sélection.

6.  Modifier le contrôleur de la vue racine pour être **contenu : Prototypes dynamique** (la vue sur l’aire de conception est étiquetée **Prototype contenu** ) :

    [![Définition de la propriété de contenu pour les prototypes dynamiques](creating-tables-in-a-storyboard-images/image17a.png)](creating-tables-in-a-storyboard-images/image17a.png#lightbox)

7.  Modifier la nouvelle **UITableViewController** être **contenu : cellules statique**. 


8. La nouvelle UITableViewController doit avoir son nom de classe et un identificateur défini. Sélectionnez le contrôleur d’affichage et le type _TaskDetailViewController_ pour le **classe** dans le **propriétés remplissage** – cela créera un nouveau `TaskDetailViewController.cs` fichier de la Solution Remplissage. Entrez le **StoryboardID** en tant que _détail_, comme illustré dans l’exemple ci-dessous. Cela sera utilisée ultérieurement pour charger cette vue dans le code c# :  

    [![Définition de l’ID de la table de montage séquentiel](creating-tables-in-a-storyboard-images/image18a.png)](creating-tables-in-a-storyboard-images/image18a.png#lightbox)

9. L’aire de conception de plan conceptuel doit maintenant ressembler à ceci (titre de contrôleur racine de la vue navigation de l’objet a été modifié au « Tableau de tâches ») :

    [![Aire de conception](creating-tables-in-a-storyboard-images/image20a-sml.png)](creating-tables-in-a-storyboard-images/image20a.png#lightbox)  



<a name="Create_the_UI" />

### <a name="create-the-ui"></a>Créer l'interface utilisateur

Maintenant que les vues et est parfait pour sont configurés, les éléments d’interface utilisateur doivent être ajoutés.

#### <a name="root-view-controller"></a>Contrôleur d’affichage racine

Tout d’abord, sélectionnez la cellule de prototype dans le contrôleur de la vue et définir le **identificateur** en tant que _taskcell_, comme illustré ci-dessous. Celui-ci doit servir plus tard dans le code pour récupérer une instance de cette UITableViewCell :

 [![définition de l’identificateur de cellule](creating-tables-in-a-storyboard-images/image22a-sml.png)](creating-tables-in-a-storyboard-images/image22a.png#lightbox)

Ensuite, vous devez créer un bouton qui ajoute de nouvelles tâches, comme illustré ci-dessous :

[![élément de bouton dans la barre de navigation de la barre](creating-tables-in-a-storyboard-images/image23-sml.png)](creating-tables-in-a-storyboard-images/image23.png#lightbox)

Effectuez ce qui suit : 

-  Faites glisser un **élément de bouton de barre de** à partir de la boîte à outils vers la _côté droit de la barre de navigation_.
-  Dans le **propriétés remplissage**, sous **élément de bouton de barre de** sélectionnez **identificateur : ajouter** (pour le rendre un *+* plus le bouton). 
-  Pour pouvoir être identifié dans le code à un stade ultérieur, attribuez-lui un nom. Notez que vous devez lui donner contrôleur racine de la vue un nom de classe (par exemple **ItemViewController**) pour vous permettre de définir le nom de l’élément de bouton de barre.


#### <a name="taskdetail-view-controller"></a>Contrôleur d’affichage de TaskDetail

L’affichage des détails nécessite beaucoup plus de travail. Cellules de vue de tableau doivent être de faire glisser la vue et ensuite remplie avec des étiquettes, des affichages de texte et des boutons. La capture d’écran ci-dessous montre l’interface utilisateur terminé avec deux sections. Une section a trois cellules, trois légendes un commutateur et deux champs de texte, tandis que la deuxième section comporte une cellule contenant deux boutons :

 [![mise en page de la vue Détails](creating-tables-in-a-storyboard-images/image24a-sml.png)](creating-tables-in-a-storyboard-images/image24a.png#lightbox)

Les étapes permettant de générer la disposition complète sont :

Sélectionnez la vue de la table et ouvrez le **remplissage de la propriété**. Mettre à jour les propriétés suivantes :

-  **Sections**: _2_ 
-  **Style**: _regroupées_
-  **Séparateur**: _None_
-  **Sélection**: _aucune sélection_

Sélectionnez la section supérieure et, sous **Propriétés > Section de vue Table** modifier **lignes** à _3_, comme illustré ci-dessous :


 [![définition de la section supérieure à trois lignes](creating-tables-in-a-storyboard-images/image29-sml.png)](creating-tables-in-a-storyboard-images/image29.png#lightbox)

Pour chaque cellule ouvrir le **propriétés remplissage** et définissez :

-  **Style**: _personnalisé_
-  **Identificateur**: choisir un identificateur unique pour chaque cellule (par exemple). «_titre_«, »_notes_«, »_fait_»).
-  Faites glisser les contrôles requis pour générer la disposition indiquée dans la capture d’écran (placer **UILabel**, **UITextField** et **UISwitch** sur les cellules corrects et définir les étiquettes en conséquence, Internet Explorer. Titre, Notes et terminé).


Dans la deuxième section, définissez **lignes** à _1_ et saisissez la poignée de redimensionnement inférieure de la cellule pour le rendre plus grands.

-  **Définir l’identificateur de**: une valeur unique (par exemple). « Enregistrer »). 
-  **Définir l’arrière-plan**: _supprimer la couleur_ .
-  Faites glisser deux boutons sur la cellule et de définir leurs titres correctement (par exemple, _enregistrer_ et _supprimer_), comme illustré ci-dessous :

   [![deux boutons de paramètre dans la section inférieure](creating-tables-in-a-storyboard-images/image30-sml.png)](creating-tables-in-a-storyboard-images/image30.png#lightbox)

À ce stade vous pouvez également définir des contraintes sur vos cellules et des contrôles pour assurer une disposition adaptative.

### <a name="adding-uikit-class-and-naming-controls"></a>Ajout d’UIKit classe et la dénomination des contrôles

Il existe quelques étapes finales pour la création de notre plan conceptuel. Tout d’abord nous devons nommer chacun de ses contrôles sous **identité > nom** pour qu’ils puissent être utilisées ultérieurement dans le code. Nom comme suit :

-  **Titre UITextField** : _Text_
-  **Notes UITextField** : _NotesText_
-  **UISwitch** : _DoneSwitch_
-  **Supprimer UIButton** : _DeleteButton_
-  **Enregistrer UIButton** : _SaveButton_


<a name="Adding_Code" />

## <a name="adding-code"></a>Ajout d’un Code

Le reste du travail est effectué dans Visual Studio sur Mac ou Windows à l’aide de c#. Notez que les noms des propriétés utilisées dans le code reflètent celles définies dans la procédure ci-dessus.

Tout d’abord, nous voulons créer un `Chores` (classe), qui fournit un moyen pour obtenir et définir la valeur d’ID, nom, commentaires et fait Boolean, afin que nous pouvons utiliser ces valeurs dans l’ensemble de l’application.

Dans votre `Chores` classe ajoutez le code suivant :

```csharp
public class Chores {
    public int Id { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
  }
```

Ensuite, créez un `RootTableSource` classe qui hérite de `UITableViewSource`. 

La différence entre ceci et une vue-table de montage séquentiel table est que le `GetView` méthode n’a pas besoin d’instancier des cellules – `theDequeueReusableCell` méthode retourne toujours une instance de la cellule prototype (avec un identificateur correspondant).

Le code ci-dessous est issu le `RootTableSource.cs` fichier :

```csharp
public class RootTableSource : UITableViewSource
{
// there is NO database or storage of Tasks in this example, just an in-memory List<>
Chores[] tableItems;
string cellIdentifier = "taskcell"; // set in the Storyboard

    public RootTableSource(Chores[] items)
    {
        tableItems = items;
    }

public override nint RowsInSection(UITableView tableview, nint section)
{
  return tableItems.Length;
}

public override UITableViewCell GetCell(UITableView tableView, NSIndexPath indexPath)
{
  // in a Storyboard, Dequeue will ALWAYS return a cell, 
  var cell = tableView.DequeueReusableCell(cellIdentifier);
  // now set the properties as normal
  cell.TextLabel.Text = tableItems[indexPath.Row].Name;
  if (tableItems[indexPath.Row].Done)
    cell.Accessory = UITableViewCellAccessory.Checkmark;
  else
    cell.Accessory = UITableViewCellAccessory.None;
  return cell;
}
public Chores GetItem(int id)
{
  return tableItems[id];
}
```

Pour utiliser le `RootTableSource` de classe, créez une nouvelle collection dans le `ItemViewController`du constructeur :

```csharp
chores = new List<Chore> {
      new Chore {Name="Groceries", Notes="Buy bread, cheese, apples", Done=false},
      new Chore {Name="Devices", Notes="Buy Nexus, Galaxy, Droid", Done=false}
    };
```

Dans `ViewWillAppear` passez la collection à la source et attribuer à la vue de table :

```csharp
public override void ViewWillAppear(bool animated)
{
    base.ViewWillAppear(animated);

    TableView.Source = new RootTableSource(chores.ToArray());
}
```

Si vous exécutez l’application maintenant, l’écran principal sera désormais charger et afficher une liste de deux tâches. Lorsqu’une tâche est affectée le segue définie par le plan conceptuel entraînera l’écran de détails s’affiche, mais il n’affichera pas les données pour le moment.

Pour « envoyer un paramètre' dans un segue, remplacer le `PrepareForSegue` (méthode) et définir des propriétés sur le `DestinationViewController` (le `TaskDetailViewController` dans cet exemple). La classe de contrôleur de vue de Destination sera été instanciée mais n’est pas encore affiché pour l’utilisateur, cela signifie que vous pouvez définir des propriétés sur la classe, mais pas modifier les contrôles d’interface utilisateur :

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
    {
      if (segue.Identifier == "TaskSegue") { // set in Storyboard
        var navctlr = segue.DestinationViewController as TaskDetailViewController;
        if (navctlr != null) {
          var source = TableView.Source as RootTableSource;
          var rowPath = TableView.IndexPathForSelectedRow;
          var item = source.GetItem(rowPath.Row);
          navctlr.SetTask (this, item); // to be defined on the TaskDetailViewController
        }
      }
    }
```

Dans `TaskDetailViewController` le `SetTask` méthode affecte ses paramètres aux propriétés afin qu’ils peuvent être référencés dans ViewWillAppear. Impossible de modifier les propriétés du contrôle dans `SetTask` car peut ne pas exister lorsque `PrepareForSegue` est appelée :

```csharp
Chore currentTask {get;set;}
    public ItemViewController Delegate {get;set;} // will be used to Save, Delete later

public override void ViewWillAppear (bool animated)
    {
      base.ViewWillAppear (animated);
      TitleText.Text = currentTask.Name;
      NotesText.Text = currentTask.Notes;
      DoneSwitch.On = currentTask.Done;
    }

    // this will be called before the view is displayed
    public void SetTask (ItemViewController d, Chore task) {
      Delegate = d;
      currentTask = task;
    }
```

Le segue maintenant ouvre l’écran de détails et afficher les informations de la tâche sélectionnée. Malheureusement, il n’existe pas d’implémentation pour la **enregistrer** et **supprimer** boutons. Avant d’implémenter les boutons, ajoutez ces méthodes à **ItemViewController.cs** pour mettre à jour les données sous-jacentes et fermer l’écran de détails :

```csharp
public void SaveTask(Chores chore)
{
  var oldTask = chores.Find(t => t.Id == chore.Id);
        NavigationController.PopViewController(true);
}

public void DeleteTask(Chores chore)
{
  var oldTask = chores.Find(t => t.Id == chore.Id);
  chores.Remove(oldTask);
        NavigationController.PopViewController(true);
}
```

Ensuite, vous devez ajouter du bouton `TouchUpInside` Gestionnaire d’événements à la `ViewDidLoad` méthode **TaskDetailViewController.cs**. Le `Delegate` propriété à référencer le `ItemViewController` a été créé spécifiquement pour que nous pouvons appeler `SaveTask` et `DeleteTask`, laquelle fermer cette vue dans le cadre de leur fonctionnement :

```csharp
SaveButton.TouchUpInside += (sender, e) => {
        currentTask.Name = TitleText.Text;
        currentTask.Notes = NotesText.Text;
        currentTask.Done = DoneSwitch.On;
        Delegate.SaveTask(currentTask);
      };

DeleteButton.TouchUpInside += (sender, e) => Delegate.DeleteTask(currentTask);
```

La dernière pièce restante de fonctionnalités permettant de créer est la création de nouvelles tâches. Dans **ItemViewController.cs** ajouter une méthode qui crée de nouvelles tâches et ouvre la vue de détail. Pour instancier une vue à partir d’une table de montage séquentiel, utilisez le `InstantiateViewController` méthode avec le `Identifier` de cette vue - dans cet exemple à 'detail' :

```csharp
public void CreateTask () 
    {
      // first, add the task to the underlying data
      var newId = chores[chores.Count - 1].Id + 1;
      var newChore = new Chore{Id = newId};
      chores.Add (newChore);

      // then open the detail view to edit it
      var detail = Storyboard.InstantiateViewController("detail") as TaskDetailViewController;
      detail.SetTask (this, newChore);
      NavigationController.PushViewController (detail, true);
    }
```

Enfin, rattachez le bouton dans la barre de navigation dans **ItemViewController.cs**de `ViewDidLoad` méthode à appeler :

```csharp
AddButton.Clicked += (sender, e) => CreateTask ();
```

Ainsi se termine l’exemple de table de montage séquentiel – l’application terminée présente comme suit :

[![Application terminée](creating-tables-in-a-storyboard-images/image28a.png)](creating-tables-in-a-storyboard-images/image28a.png#lightbox)

L’exemple montre :

-  Création d’une table avec un Prototype de contenu où les cellules sont définies pour une réutilisation afficher des listes de données. 
-  Création d’une table avec un contenu statique pour créer un formulaire d’entrée. Cela inclus la modification du style de table et l’ajout de sections, des cellules et des contrôles d’interface utilisateur. 
-  Comment créer un segue et remplacez le `PrepareForSegue` méthode pour notifier la vue cible de tous les paramètres qu’il requiert. 
-  Chargement des vues de table de montage séquentiel directement avec le `Storyboard.InstantiateViewController` (méthode).



## <a name="related-links"></a>Liens associés

- [StoryboardTable (exemple)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [Introduction aux storyboards](~/ios/user-interface/storyboards/index.md)
