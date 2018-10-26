---
title: Utilisation de Tables dans le concepteur iOS
description: Dans les sections précédentes, nous avons exploré développement à l’aide de Tables. Dans ce cas, la cinquième et dernière section, nous ce que nous avons appris jusqu'à présent, d’agrégation et créer une application de liste de base à l’aide d’une table de montage séquentiel.
ms.prod: xamarin
ms.assetid: D8416E10-481A-0B6E-4081-B146E6358004
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: d161a267c8ffa5040327db8e6e4f867a324b04f2
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105804"
---
# <a name="working-with-tables-in-the-ios-designer"></a>Utilisation de Tables dans le concepteur iOS

Storyboards WYSIWYG permettent de créer des applications iOS et sont prises en charge dans Visual Studio sur Mac et Windows. Pour plus d’informations sur les tables de montage séquentiel, reportez-vous à la [Introduction aux Storyboards](~/ios/user-interface/storyboards/index.md) document. Tables de montage séquentiel vous permettent également de modifier les dispositions de cellule *dans* la table, ce qui simplifie le développement des tables et des cellules

Lors de la configuration des propriétés d’une vue de table dans le concepteur iOS, il existe deux types de contenu de la cellule que vous pouvez choisir parmi : **dynamique** ou **statique** contenu du Prototype.

<a name="Prototype_Content" />

## <a name="dynamic-prototype-content"></a>Contenu du Prototype dynamique

Un `UITableView` avec prototype de contenu est généralement destiné à afficher une liste de données où la cellule prototype (ou les cellules, vous peuvent définir plusieurs) sont réutilisées pour chaque élément dans la liste. Les cellules n’avez pas besoin être instanciés, ils sont obtenus dans le `GetView` méthode en appelant le `DequeueReusableCell` méthode de son `UITableViewSource`.

 <a name="Static_Content" />


## <a name="static-content"></a>Contenu statique

`UITableView`s avec un contenu statique permet créer des tables à être conçu directement sur l’aire de conception. Cellules peuvent être déplacés dans la table et personnalisées en modifiant les propriétés et l’ajout de contrôles.

 <a name="Creating_a_Storyboard-driven_app" />


## <a name="creating-a-storyboard-driven-app"></a>Création d’une application basée sur une table de montage séquentiel

L’exemple StoryboardTable contient une simple application maître / détail qui utilise les deux types de UITableView dans une table de montage séquentiel. Le reste de cette section décrit comment créer un exemple de liste de petites tâches qui ressemble à ceci lorsque vous avez terminé :

 [![Exemples d’écrans](creating-tables-in-a-storyboard-images/image13a.png)](creating-tables-in-a-storyboard-images/image13a.png#lightbox)

L’interface utilisateur doit être créé avec un storyboard, et les deux écrans utilisera un UITableView. L’écran principal utilise *contenu de prototype* à disposition de la ligne et le détail de l’écran utilise *contenu statique* pour créer un formulaire de saisie de données à l’aide de dispositions de cellule personnalisé.

## <a name="walkthrough"></a>Procédure pas à pas

Créer une nouvelle solution dans Visual Studio en utilisant **(créer) un nouveau projet... > application avec affichage unique (C#)** et appelez-le _StoryboardTables_.

 [![Créer une boîte de dialogue Nouveau projet](creating-tables-in-a-storyboard-images/npd.png)](creating-tables-in-a-storyboard-images/npd.png#lightbox)

La solution s’ouvre avec certains C# fichiers et un `Main.storyboard` fichier déjà créé. Double-cliquez sur le `Main.storyboard` fichier pour l’ouvrir dans le concepteur iOS.

<a name="Modifying_the_Storyboard" />

## <a name="modifying-the-storyboard"></a>Modification de la table de montage séquentiel

Le plan conceptuel sera modifié en trois étapes :

-  Tout d’abord, disposition requise afficher les contrôleurs et définir leurs propriétés.
-  En second lieu, créez votre interface utilisateur en faisant glisser des objets dans votre vue
-  Enfin, ajoutez la classe UIKit requise à chaque vue et donnez un nom à différents contrôles afin qu’ils peuvent être référencées dans le code.


Une fois que l’animation est terminée, le code peut être ajouté pour que tout fonctionne.

<a name="Layout_The_View_Controllers" />

### <a name="layout-the-view-controllers"></a>Mise en page les contrôleurs d’affichage

La première modification à la table de montage séquentiel est la suppression de la vue de détail existante et en la remplaçant par une UITableViewController. Procédez comme suit :

1.  Sélectionnez la barre en bas du contrôleur d’affichage et supprimez-le.
2.  Faites glisser un **contrôleur de Navigation** et un **contrôleur d’affichage Table** sur le plan conceptuel à partir de la boîte à outils. 
3.  Créer un segue entre le contrôleur d’affichage racine et le deuxième contrôleur d’affichage de Table qui vient d’être ajouté. Pour créer le segue, contrôle + faire glisser *à partir de la cellule de détail* à la UITableViewController nouvellement ajouté. Choisissez l’option **afficher*** sous **sélection Segue**. 
4.  Sélectionnez le nouveau segue que vous avez créé et lui donner un identificateur pour référencer ce segue dans le code. Cliquez sur le segue, puis entrez `TaskSegue` pour le **identificateur** dans le **panneau Propriétés**, comme suit :    
  [![Nommage segue dans Panneau de propriétés](creating-tables-in-a-storyboard-images/image16a-sml.png)](creating-tables-in-a-storyboard-images/image16a.png#lightbox) 

5. Ensuite, configurez les deux vues de Table en les sélectionnant et en utilisant le panneau Propriétés. Veillez à sélectionner la vue et pas contrôleur d’affichage : vous pouvez utiliser la structure du Document pour faciliter la sélection.

6.  Modifier le contrôleur d’affichage racine pour être **contenu : Prototypes dynamique** (la vue sur l’aire de conception est étiquetée **Prototype contenu** ) :

    [![Définissant la propriété de contenu pour les prototypes dynamiques](creating-tables-in-a-storyboard-images/image17a.png)](creating-tables-in-a-storyboard-images/image17a.png#lightbox)

7.  Modifier la nouvelle **UITableViewController** être **contenu : cellules statique**. 


8. La nouvelle UITableViewController doit avoir son nom de classe et un identificateur défini. Sélectionnez le contrôleur d’affichage et le type _TaskDetailViewController_ pour le **classe** dans le **panneau Propriétés** – cela créera un nouveau `TaskDetailViewController.cs` fichier dans la Solution Panneau. Entrez le **StoryboardID** comme _détail_, comme illustré dans l’exemple ci-dessous. Il servira ultérieurement pour charger cette vue dans C# code :  

    [![Définition de l’ID de Storyboard](creating-tables-in-a-storyboard-images/image18a.png)](creating-tables-in-a-storyboard-images/image18a.png#lightbox)

9. L’aire de conception de table de montage séquentiel doit maintenant ressembler à ceci (titre du contrôleur d’affichage racine navigation de l’élément a été modifié pour « Tableau corvée ») :

    [![Aire de conception](creating-tables-in-a-storyboard-images/image20a-sml.png)](creating-tables-in-a-storyboard-images/image20a.png#lightbox)  



<a name="Create_the_UI" />

### <a name="create-the-ui"></a>Créer l'interface utilisateur

Maintenant que les vues et les enchaînements sont configurés, les éléments d’interface utilisateur doivent être ajoutés.

#### <a name="root-view-controller"></a>Contrôleur d’affichage racine

Tout d’abord, sélectionnez la cellule de prototype dans le contrôleur d’affichage maître et définissez le **identificateur** comme _taskcell_, comme illustré ci-dessous. Cela sera utilisé ultérieurement dans le code pour récupérer une instance de cette UITableViewCell :

 [![définition de l’identificateur de cellule](creating-tables-in-a-storyboard-images/image22a-sml.png)](creating-tables-in-a-storyboard-images/image22a.png#lightbox)

Ensuite, vous devez créer un bouton qui ajoute de nouvelles tâches, comme illustré ci-dessous :

[![élément de bouton dans la barre de navigation de la barre](creating-tables-in-a-storyboard-images/image23-sml.png)](creating-tables-in-a-storyboard-images/image23.png#lightbox)

Effectuez ce qui suit : 

-  Faites glisser un **élément de bouton de barre de** à partir de la boîte à outils vers le _côté droit de la barre de navigation_.
-  Dans le **panneau Propriétés**, sous **élément de bouton de barre de** sélectionnez **identificateur : ajouter** (pour le rendre un *+* ainsi que de bouton). 
-  Donnez-lui un nom afin qu’il peut être identifié dans le code à un stade ultérieur. Notez que vous devez donner le contrôleur d’affichage racine un nom de classe (par exemple **ItemViewController**) pour vous permettre de définir le nom de l’élément de bouton de barre.


#### <a name="taskdetail-view-controller"></a>Contrôleur d’affichage de TaskDetail

La vue détail nécessite beaucoup plus de travail. Cellules de vue de table doivent être glissé sur la vue et ensuite remplie avec des étiquettes, des affichages de texte et des boutons. La capture d’écran ci-dessous montre l’interface utilisateur terminé avec deux sections. Une section a trois cellules, trois étiquettes un commutateur et deux champs de texte, tandis que la deuxième section comporte une cellule contenant deux boutons :

 [![disposition de la vue Détails](creating-tables-in-a-storyboard-images/image24a-sml.png)](creating-tables-in-a-storyboard-images/image24a.png#lightbox)

Les étapes pour générer la disposition complète sont :

Sélectionnez la vue de table et ouvrez le **remplissage de la propriété**. Mettre à jour les propriétés suivantes :

-  **Sections**: _2_ 
-  **Style**: _regroupés_
-  **Séparateur**: _None_
-  **Sélection**: _aucune sélection_

Sélectionnez la section supérieure et, sous **Propriétés > Section d’affichage Table** modifier **lignes** à _3_, comme illustré ci-dessous :


 [![définition de la section supérieure à trois lignes](creating-tables-in-a-storyboard-images/image29-sml.png)](creating-tables-in-a-storyboard-images/image29.png#lightbox)

Pour chaque cellule ouvert le **panneau Propriétés** et définissez :

-  **Style**: _personnalisé_
-  **Identificateur**: choisissez un identificateur unique pour chaque cellule (par exemple). «_titre_«, »_notes_«, »_fait_»).
-  Faites glisser les contrôles requis pour produire la disposition indiquée dans la capture d’écran (placer **UILabel**, **champ UITextField** et **UISwitch** sur les cellules corrects et définir les étiquettes en conséquence, Internet Explorer. Titre, des Notes et terminé).


Dans la deuxième section, définissez **lignes** à _1_ et saisissez la poignée de redimensionnement inférieure de la cellule à agrandir.

-  **Définir l’identificateur**: une valeur unique (par exemple). « Enregistrer »). 
-  **Définir l’arrière-plan**: _supprimer la couleur_ .
-  Faites glisser deux boutons sur la cellule et définir leurs titres de façon appropriée (par exemple, _enregistrer_ et _supprimer_), comme illustré ci-dessous :

   [![définition des deux boutons dans la section inférieure](creating-tables-in-a-storyboard-images/image30-sml.png)](creating-tables-in-a-storyboard-images/image30.png#lightbox)

À ce stade vous souhaiterez également définir des contraintes sur vos cellules et des contrôles pour assurer une disposition adaptative.

### <a name="adding-uikit-class-and-naming-controls"></a>Ajout de classe de UIKit et contrôles d’affectation de noms

Il existe quelques étapes finales dans la création de notre table de montage séquentiel. Tout d’abord nous devons nommer chacun de nos contrôles sous **identité > nom** afin de pouvoir être utilisés dans le code par la suite. Nommez comme suit :

-  **Titre du champ UITextField** : _Text_
-  **Notes de champ UITextField** : _NotesText_
-  **UISwitch** : _DoneSwitch_
-  **Supprimer UIButton** : _DeleteButton_
-  **Enregistrer UIButton** : _SaveButton_


<a name="Adding_Code" />

## <a name="adding-code"></a>Ajout de Code

Le reste du travail est effectué dans Visual Studio sur Mac ou Windows avec C#. Notez que les noms de propriété utilisés dans le code reflètent celles définies dans la procédure ci-dessus.

Tout d’abord, nous voulons créer un `Chores` (classe), qui fournit un moyen pour obtenir et définir la valeur d’ID, nom, notes de publication et la fait booléenne, afin que nous pouvons utiliser ces valeurs dans toute l’application.

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

La différence entre ceci et une vue de table de-table de montage séquentiel est que le `GetView` (méthode) n’a pas besoin d’instancier des cellules – `theDequeueReusableCell` méthode retourne toujours une instance de la cellule de prototype (avec identificateur correspondant).

Le code ci-dessous provient du `RootTableSource.cs` fichier :

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

Pour utiliser le `RootTableSource` class, créer une nouvelle collection dans le `ItemViewController`du constructeur :

```csharp
chores = new List<Chore> {
      new Chore {Name="Groceries", Notes="Buy bread, cheese, apples", Done=false},
      new Chore {Name="Devices", Notes="Buy Nexus, Galaxy, Droid", Done=false}
    };
```

Dans `ViewWillAppear` transmettre la collection à la source et affecter à la vue de table :

```csharp
public override void ViewWillAppear(bool animated)
{
    base.ViewWillAppear(animated);

    TableView.Source = new RootTableSource(chores.ToArray());
}
```

Si vous exécutez l’application maintenant, l’écran principal sera désormais charger et afficher une liste de deux tâches. Lorsqu’une tâche est touchée le segue défini par la table de montage séquentiel entraîne l’écran de détails s’affiche, mais il n’affichera pas toutes les données pour le moment.

Pour « envoyer un paramètre » dans un segue, remplacer le `PrepareForSegue` (méthode) et définir des propriétés sur le `DestinationViewController` (le `TaskDetailViewController` dans cet exemple). La classe de contrôleur d’affichage de Destination sera ont été instanciée, mais n’est pas encore affiché à l’utilisateur : cela signifie que vous pouvez définir des propriétés sur la classe, mais pas modifier les contrôles d’interface utilisateur :

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

Dans `TaskDetailViewController` le `SetTask` méthode affecte ses paramètres aux propriétés afin qu’ils peuvent être référencés dans ViewWillAppear. Les propriétés du contrôle ne peut pas être modifiées dans `SetTask` , car n’existe ne peut-être pas lorsque `PrepareForSegue` est appelée :

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

Le segue sera désormais ouvrir l’écran de détails et afficher les informations de la tâche sélectionnée. Malheureusement, il n’a pas d’implémentation pour le **enregistrer** et **supprimer** boutons. Avant d’implémenter les boutons, ajoutez ces méthodes à **ItemViewController.cs** pour mettre à jour les données sous-jacentes et de fermer l’écran de détails :

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

Ensuite, vous devez ajouter le bouton `TouchUpInside` Gestionnaire d’événements à la `ViewDidLoad` méthode de **TaskDetailViewController.cs**. Le `Delegate` propriété à référencer le `ItemViewController` a été créé spécifiquement nous pouvons donc appeler `SaveTask` et `DeleteTask`, laquelle fermer cette vue dans le cadre de leur fonctionnement :

```csharp
SaveButton.TouchUpInside += (sender, e) => {
        currentTask.Name = TitleText.Text;
        currentTask.Notes = NotesText.Text;
        currentTask.Done = DoneSwitch.On;
        Delegate.SaveTask(currentTask);
      };

DeleteButton.TouchUpInside += (sender, e) => Delegate.DeleteTask(currentTask);
```

La dernière partie restante de fonctionnalités permettant de créer est la création de nouvelles tâches. Dans **ItemViewController.cs** ajouter une méthode qui crée de nouvelles tâches et ouvre la vue de détail. Pour instancier une vue à partir d’une table de montage séquentiel, utilisez le `InstantiateViewController` méthode avec le `Identifier` de cette vue - dans cet exemple sera 'detail' :

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

Enfin, associer le bouton dans la barre de navigation dans **ItemViewController.cs**de `ViewDidLoad` méthode à appeler :

```csharp
AddButton.Clicked += (sender, e) => CreateTask ();
```

Ainsi se termine l’exemple de table de montage séquentiel – l’application terminée ressemble à ceci :

[![Application terminée](creating-tables-in-a-storyboard-images/image28a.png)](creating-tables-in-a-storyboard-images/image28a.png#lightbox)

L’exemple montre :

-  Création d’une table avec contenu Prototype où les cellules sont définies pour une réutilisation afficher des listes de données. 
-  Création d’une table avec le contenu statique pour créer un formulaire d’entrée. Cela inclus la modification du style de table et l’ajout de sections, les cellules et les contrôles d’interface utilisateur. 
-  Comment créer un segue et remplacer le `PrepareForSegue` méthode pour notifier la vue cible de tous les paramètres qu’il requiert. 
-  Chargement des vues de table de montage séquentiel directement avec le `Storyboard.InstantiateViewController` (méthode).



## <a name="related-links"></a>Liens associés

- [StoryboardTable (exemple)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [Introduction aux storyboards](~/ios/user-interface/storyboards/index.md)
