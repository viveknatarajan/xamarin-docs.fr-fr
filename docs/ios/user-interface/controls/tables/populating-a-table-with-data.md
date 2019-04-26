---
title: Remplissage d’une Table avec des données dans Xamarin.iOS
description: Ce document décrit comment remplir une table avec des données dans une application Xamarin.iOS. Il aborde UITableViewSource, réutilisation de la cellule, ajout d’un index et les en-têtes et pieds de page.
ms.prod: xamarin
ms.assetid: 6FE64DDF-1029-EB9B-6EEC-1C7DFDFDF3AF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 5363e3a2210bdcf1efb870ac808ecb37584de6a7
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61037249"
---
# <a name="populating-a-table-with-data-in-xamarinios"></a>Remplissage d’une Table avec des données dans Xamarin.iOS

Pour ajouter des lignes à une `UITableView` vous devez implémenter un `UITableViewSource` sous-classe et que vous remplacez les méthodes qui permet d’afficher la table appelle à se remplir lui-même.

Ce guide couvre :

- Sous-classement d’un UITableViewSource
- Réutilisation de cellule
- Ajout d’un Index
- Ajout d’en-têtes et pieds de page


<a name="Subclassing_UITableViewSource" />

## <a name="subclassing-uitableviewsource"></a>Sous-classement UITableViewSource

Un `UITableViewSource` sous-classe est affectée à chaque `UITableView`. La vue table interroge la classe source pour déterminer comment afficher lui-même (par exemple, le nombre de lignes est requis et la hauteur de chaque ligne s’il diffère de la valeur par défaut). Plus important encore, la source fournit à chaque vue de la cellule remplie avec des données.

Il existe uniquement deux méthodes obligatoires nécessaires pour créer une table à afficher des données :

-   **RowsInSection** – retour un [ `nint` ](https://developer.xamarin.com/guides/cross-platform/macios/nativetypes/) le nombre total de lignes de données de la table doit s’afficher.
-   **GetCell** – retour un `UITableCellView` rempli avec les données de l’index de ligne correspondant transmis à la méthode.


L’exemple de fichier BasicTable **TableSource.cs** comporte l’implémentation la plus simple possible de `UITableViewSource`. Qu’il accepte un tableau de chaînes à afficher dans la table et retourne un style de cellule par défaut contenant chaque chaîne, vous pouvez le voir dans l’extrait de code ci-dessous :

```csharp
public class TableSource : UITableViewSource {

        string[] TableItems;
        string CellIdentifier = "TableCell";

        public TableSource (string[] items)
        {
            TableItems = items;
        }

        public override nint RowsInSection (UITableView tableview, nint section)
        {
            return TableItems.Length;
        }

        public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
        {
            UITableViewCell cell = tableView.DequeueReusableCell (CellIdentifier);
            string item = TableItems[indexPath.Row];

            //---- if there are no cells to reuse, create a new one
            if (cell == null)
            { cell = new UITableViewCell (UITableViewCellStyle.Default, CellIdentifier); }

            cell.TextLabel.Text = item;

            return cell;
        }
}
```

Un `UITableViewSource` pouvez utiliser toute structure de données, à partir d’un tableau de chaînes simples (comme indiqué dans cet exemple) pour une liste de <> ou autre collection. L’implémentation de `UITableViewSource` méthodes isole la table à partir de la structure de données sous-jacente.

Pour utiliser cette sous-classe, créez un tableau de chaînes pour construire la source, puis affectez-le à une instance de `UITableView`:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    table = new UITableView(View.Bounds); // defaults to Plain style
    string[] tableItems = new string[] {"Vegetables","Fruits","Flower Buds","Legumes","Bulbs","Tubers"};
    table.Source = new TableSource(tableItems);
    Add (table);
}
```

La table résultante ressemble à ceci :

 [![](populating-a-table-with-data-images/image3.png "Exemple de table en cours d’exécution")](populating-a-table-with-data-images/image3.png#lightbox)

La plupart des tables autoriser l’utilisateur à toucher une ligne pour la sélectionner et effectuer d’autres actions (par exemple, écouter une chanson ou appeler un contact présentant un autre écran). Pour ce faire, il existe quelques éléments que nous devons faire. Tout d’abord, nous allons créer un AlertController pour afficher un message lorsque l’utilisateur clique sur une ligne en ajoutant le code suivant à la `RowSelected` méthode :

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("Row Selected", tableItems[indexPath.Row], UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    ...

    tableView.DeselectRow (indexPath, true);
}
```

Ensuite, créez une instance de notre contrôleur d’affichage :

```csharp
HomeScreen owner;
```
Ajoutez un constructeur pour votre classe UITableViewSource qui prend un contrôleur d’affichage en tant que paramètre et l’enregistre dans un champ :

```csharp
public TableSource (string[] items, HomeScreen owner)
{
    ...
    this.owner = owner;

}
```
Modifiez la méthode ViewDidLoad où la classe UITableViewSource est créée pour passer la `this` référence :

```csharp
table.Source = new TableSource(tableItems, this);
```
Enfin, de retour dans votre `RowSelected` méthode, appelez `PresentViewController` sur le champ mis en cache :

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    ...
    owner.PresentViewController (okAlertController, true, null);

    ...
}
```


Maintenant l’utilisateur peut se toucher une ligne et une alerte s’affiche :



 [![](populating-a-table-with-data-images/image4.png "L’alerte sélectionnée de ligne")](populating-a-table-with-data-images/image4.png#lightbox)


## <a name="cell-reuse"></a>Réutilisation de cellule

Cet exemple sont seulement six éléments, donc il n’existe aucun réutilisation cellule requis. Lors de l’affichage des centaines voire des milliers de lignes, toutefois, il serait une perte de mémoire pour créer des centaines voire des milliers de `UITableViewCell` objets lorsque seuls quelques tient sur l’écran à la fois.

Pour éviter cette situation, lorsqu’une cellule disparaît de l’écran, que son affichage est placé dans une file d’attente pour une réutilisation. Comme l’utilisateur fait défiler, la table appelle `GetCell` pour demander de nouvelles vues à afficher : permet de réutiliser une cellule existante (qui n’est pas actuellement affichée) suffit d’appeler le `DequeueReusableCell` (méthode). Si une cellule est disponible pour une réutilisation que sera retourné, sinon une valeur null est retournée et votre code doit créer une nouvelle instance de la cellule.

Cet extrait de code à partir de l’exemple illustre le modèle :

```csharp
// request a recycled cell to save memory
UITableViewCell cell = tableView.DequeueReusableCell (cellIdentifier);
// if there are no cells to reuse, create a new one
if (cell == null)
    cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
```

Le `cellIdentifier` crée efficacement les files d’attente distinctes pour différents types de cellule. Dans cet exemple, toutes les cellules rechercher la même codée en dur donc qu’un seul identificateur est utilisé. Si vous rencontrez des différents types de cellule ils doivent chacun avoir une chaîne d’identificateur différents, à la fois lorsqu’ils sont instanciés et quand ils sont demandés à partir de la file d’attente de réutilisation.

### <a name="cell-reuse-in-ios-6"></a>Réutilisation de cellule dans iOS 6 +

iOS 6 ajouté un modèle de réutilisation de cellule similaire à l’introduction d’une des vues de Collection. Bien que le modèle existant de réutilisation illustré ci-dessus est toujours pris en charge pour la compatibilité descendante, ce nouveau modèle est préférable car elle supprime la nécessité pour la vérification de valeur null sur la cellule.

Avec le nouveau modèle, une application enregistre la classe de cellule ou xib à utiliser par un appel `RegisterClassForCellReuse` ou `RegisterNibForCellReuse` dans le constructeur du contrôleur. Ensuite, lors la cellule de retrait dans les `GetCell` (méthode), il vous suffit d’appel `DequeueReusableCell` en passant l’identificateur que vous avez inscrit pour la classe de cellule ou xib et le chemin d’accès de l’index.

Par exemple, le code suivant inscrit une classe de cellule personnalisée dans un UITableViewController :

```csharp
public class MyTableViewController : UITableViewController
{
  static NSString MyCellId = new NSString ("MyCellId");

  public MyTableViewController ()
  {
    TableView.RegisterClassForCellReuse (typeof(MyCell), MyCellId);
  }
  ...
}
```

Avec la classe MaCellule inscrite, la cellule permettre être retirés de celle-ci dans le `GetCell` méthode de la `UITableViewSource` sans avoir besoin pour la vérification supplémentaire null, comme indiqué ci-dessous :

```csharp
class MyTableSource : UITableViewSource
{
  public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
  {
    // if cell is not available in reuse pool, iOS will create one automatically
    // no need to do null check and create cell manually
    var cell = (MyCell) tableView.DequeueReusableCell (MyCellId, indexPath);

    // do whatever you need to with cell, such as assigning properties, etc.

    return cell;
  }
}
```

N’oubliez pas, lorsque vous utilisez le nouveau modèle de réutilisation avec une classe de cellule personnalisée, vous devez implémenter le constructeur qui prend un `IntPtr`, comme indiqué dans l’extrait de code ci-dessous, sinon Objective-C ne sera en mesure de construire une instance de la classe de cellule :

```csharp
public class MyCell : UITableViewCell
{
  public MyCell (IntPtr p):base(p)
  {
  }
  ...
}
```

Vous pouvez voir des exemples des rubriques expliqués plus haut dans la **BasicTable** exemple lié à cet article.

<a name="Adding_an_Index" />

## <a name="adding-an-index"></a>Ajout d’un Index

Un index permet à l’utilisateur de parcourir de longues listes, généralement classés par ordre alphabétique bien que vous pouvez indexer par les critères que vous le souhaitez. Le **BasicTableIndex** exemple charge une liste beaucoup plus longue d’éléments d’un fichier pour illustrer l’index. Chaque élément dans l’index correspond à une section de la table.

 [![](populating-a-table-with-data-images/image5.png "L’affichage de l’Index")](populating-a-table-with-data-images/image5.png#lightbox)

Pour prendre en charge 'sections' les données sous-jacentes de la table doivent être regroupée, donc l’exemple BasicTableIndex crée un `Dictionary<>` à partir du tableau de chaînes à l’aide de la première lettre de chaque élément en tant que clé de dictionnaire :

```csharp
indexedTableItems = new Dictionary<string, List<string>>();
foreach (var t in items) {
    if (indexedTableItems.ContainsKey (t[0].ToString ())) {
        indexedTableItems[t[0].ToString ()].Add(t);
    } else {
        indexedTableItems.Add (t[0].ToString (), new List<string>() {t});
    }
}
keys = indexedTableItems.Keys.ToArray ();
```

Le `UITableViewSource` sous-classe doit ensuite les méthodes suivantes, ajoutées ou modifiées pour utiliser le `Dictionary<>` :

-   **NumberOfSections** – cette méthode est facultative, par défaut, ce tableau suppose une section. Lors de l’affichage d’un index de cette méthode doit retourner le nombre d’éléments dans l’index (par exemple, 26 si l’index contient toutes les lettres de l’alphabet anglais).
-   **RowsInSection** – retourne le nombre de lignes dans une section donnée.
-   **SectionIndexTitles** – retourne le tableau de chaînes qui doit être utilisée pour afficher l’index. L’exemple de code retourne un tableau de lettres.


Les méthodes de mise à jour dans l’exemple de fichier **BasicTableIndex/TableSource.cs** ressembler à ceci :

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return keys.Length;
}
public override nint RowsInSection (UITableView tableview, nint section)
{
    return indexedTableItems[keys[section]].Count;
}
public override string[] SectionIndexTitles (UITableView tableView)
{
    return keys;
}
```

Les index sont en général uniquement utilisés avec le style de table simple.


<a name="Adding_Headers_and_Footers" />

## <a name="adding-headers-and-footers"></a>Ajout d’en-têtes et pieds de page

En-têtes et pieds de page peuvent être utilisés pour regrouper visuellement des lignes dans une table. La structure de données requis est très similaire à l’ajout d’un index : un `Dictionary<>` fonctionne vraiment bien. Au lieu d’utiliser l’alphabet pour regrouper les cellules, cet exemple permet de regrouper les légumes par type Botanique.
La sortie ressemble à ceci :

 [![](populating-a-table-with-data-images/image6.png "Exemples d’en-têtes et pieds de page")](populating-a-table-with-data-images/image6.png#lightbox)

Pour afficher des en-têtes et pieds de page le `UITableViewSource` sous-classe requiert ces méthodes supplémentaires :

-   **TitleForHeader** – renvoie le texte à utiliser comme en-tête
-   **TitleForFooter** – renvoie le texte à utiliser en tant que le pied de page.


Les méthodes de mise à jour dans l’exemple de fichier **BasicTableHeaderFooter/Code/TableSource.cs** ressembler à ceci :

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    return keys[section];
}
public override string TitleForFooter (UITableView tableView, nint section)
{
    return indexedTableItems[keys[section]].Count + " items";
}
```

Vous pouvez personnaliser davantage l’apparence de l’en-tête et le pied de page dans une vue de l’objet, à l’aide de la `GetViewForHeader` et `GetViewForFooter` substitutions des méthodes sur `UITableViewSource`.


## <a name="related-links"></a>Liens associés

- [WorkingWithTables (sample)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
