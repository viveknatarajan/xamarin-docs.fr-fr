---
title: Interactivité de ListView
description: Cet article explique comment ajouter de l’interactivité à un ListView Xamarin.Forms en implémentant les sélections, les actions de contexte et les extraire pour actualiser.
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/27/2019
ms.openlocfilehash: 1447526ef925431e3cad5f36d4ce7a528c8ab07c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61386682"
---
# <a name="listview-interactivity"></a>Interactivité de ListView

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)

[`ListView`](xref:Xamarin.Forms.ListView) prend en charge l’interaction avec les données que qui y figurent.

<a name="selectiontaps" />

## <a name="selection--taps"></a>Sélection & tap

Le [ `ListView` ](xref:Xamarin.Forms.ListView) en mode de sélection est contrôlé en définissant le [ `ListView.SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) propriété une valeur de la [ `ListViewSelectionMode` ](xref:Xamarin.Forms.ListViewSelectionMode) énumération :

- [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) Indique qu’un seul élément peut être sélectionné, avec l’élément sélectionné est mis en surbrillance. Valeur par défaut.
- [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) Indique que les éléments ne peuvent pas être sélectionnés.

Lorsqu’un utilisateur appuie sur un élément, deux événements sont déclenchés :

- [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) se déclenche lorsqu’un nouvel élément est sélectionné.
- [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) se déclenche lorsque l’utilisateur appuie sur un élément.

En appuyant sur le même élément à deux reprises activeront toutes deux [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) les événements, mais sera uniquement déclencher un seul [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) événement.

> [!NOTE]
> Le [ `ItemTappedEventArgs` ](xref:Xamarin.Forms.ItemTappedEventArgs) (classe), qui contient les arguments d’événement pour le [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) a des événements, [ `Group` ](xref:Xamarin.Forms.ItemTappedEventArgs.Group) et [ `Item` ](xref:Xamarin.Forms.ItemTappedEventArgs.Item) propriétés et un `ItemIndex` propriété dont la valeur représente l’index de la [ `ListView` ](xref:Xamarin.Forms.ListView) de l’élément sur lequel. De même, le [ `SelectedItemChangedEventArgs` ](xref:Xamarin.Forms.SelectedItemChangedEventArgs) (classe), qui contient les arguments d’événement pour le [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) événement, a un [ `SelectedItem` ](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) propriété et un `SelectedItemIndex` propriété dont la valeur représente l’index de la `ListView` de l’élément sélectionné.

Lorsque le [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) propriété est définie sur [ `Single` ](xref:Xamarin.Forms.ListViewSelectionMode.Single), les éléments dans le [ `ListView` ](xref:Xamarin.Forms.ListView) peut être sélectionné, le [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) et [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) événement n’est déclenché et le [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) propriété est définie à la valeur de l’élément sélectionné.

Lorsque le [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) propriété est définie sur [ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None), les éléments dans le [ `ListView` ](xref:Xamarin.Forms.ListView) ne peut pas être sélectionné, le [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) événement ne sera pas déclenché et le [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) propriété restera `null`. Toutefois, [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) événements seront toujours déclenchés et l’élément sur lequel apparaît en surbrillance brièvement pendant le drainage.

Lorsqu’un élément a été sélectionné et le [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) propriété est modifiée à partir de [ `Single` ](xref:Xamarin.Forms.ListViewSelectionMode.Single) à [ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None), le [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) propriété sera définie `null` et [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) événement est déclenché avec un `null` élément.

Le montrent les captures d’écran suivants un [ `ListView` ](xref:Xamarin.Forms.ListView) avec le mode de sélection par défaut :

![](interactivity-images/selection-default.png "ListView avec sélection activée")

### <a name="disabling-selection"></a>La désactivation de la sélection

Pour désactiver [ `ListView` ](xref:Xamarin.Forms.ListView) du jeu de sélection le [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) propriété [ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None):

```xaml
<ListView ... SelectionMode="None" />
```

```csharp
var listView = new ListView { ... SelectionMode = ListViewSelectionMode.None };
```

<a name="Context_Actions" />

## <a name="context-actions"></a>Actions contextuelles

Souvent, les utilisateurs souhaitent effectuer une opération sur un élément dans un `ListView`. Par exemple, considérez une liste d’e-mails dans l’application de messagerie. Sur iOS, vous pouvez balayer pour supprimer un message ::

![](interactivity-images/context-default.png "ListView avec des Actions contextuelles")

Actions de contexte peuvent être implémentées en C# et XAML. Vous trouverez ci-dessous des guides pour les deux, mais tout d’abord nous allons examiner certains détails d’implémentation de la clé pour les deux.

Actions contextuelles sont créées à l’aide de `MenuItem`s. Appuyez sur pour MenuItems sont déclenchés par le MenuItem lui-même, pas le ListView. Cela est différent à partir de la gestion des événements de clic pour les cellules, où le ListView déclenche l’événement plutôt que la cellule. Étant donné que le ListView déclenche l’événement, son gestionnaire d’événements reçoit des informations de clé, à laquelle l’élément a été sélectionné ou appuyé sur.

Par défaut, un MenuItem n’a aucun moyen de savoir quelle cellule auquel il appartient. `CommandParameter` est disponible sur `MenuItem` pour stocker les objets, tels que l’objet derrière l’écran de MenuItem ViewCell. `CommandParameter` peut être définie en XAML et C#.

### <a name="c"></a>C#  

Actions de contexte peuvent être implémentées dans un `Cell` sous-classe (à condition qu’il n’est pas utilisé comme un en-tête de groupe) en créant `MenuItem`s et en les ajoutant à la `ContextActions` collection pour la cellule. Les éléments suivants sont propriétés peuvent être configurées pour l’action de contexte :

* **Texte** &ndash; la chaîne qui apparaît dans l’élément de menu.
* **Un clic sur** &ndash; l’événement lorsque l’élément est activé.
* **IsDestructive** &ndash; (facultatif) true lors de l’élément est rendu de différemment sur iOS.

Plusieurs actions de contexte peuvent être ajoutées à une cellule, mais seul l’un doit avoir `IsDestructive` défini sur `true`. Le code suivant illustre la façon dont les actions contextuelles étaient ajoutées à un `ViewCell`:

```csharp
var moreAction = new MenuItem { Text = "More" };
moreAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
moreAction.Clicked += async (sender, e) => {
    var mi = ((MenuItem)sender);
    Debug.WriteLine("More Context Action clicked: " + mi.CommandParameter);
};

var deleteAction = new MenuItem { Text = "Delete", IsDestructive = true }; // red background
deleteAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
deleteAction.Clicked += async (sender, e) => {
    var mi = ((MenuItem)sender);
    Debug.WriteLine("Delete Context Action clicked: " + mi.CommandParameter);
};
// add to the ViewCell's ContextActions property
ContextActions.Add (moreAction);
ContextActions.Add (deleteAction);
```

### <a name="xaml"></a>XAML

`MenuItem`s peut également être créé dans une collection XAML déclarative. Le XAML ci-dessous illustre une cellule personnalisée avec deux actions contextuelles implémentées :

```xaml
<ListView x:Name="ContextDemoList">
  <ListView.ItemTemplate>
    <DataTemplate>
      <ViewCell>
         <ViewCell.ContextActions>
            <MenuItem Clicked="OnMore" CommandParameter="{Binding .}"
               Text="More" />
            <MenuItem Clicked="OnDelete" CommandParameter="{Binding .}"
               Text="Delete" IsDestructive="True" />
         </ViewCell.ContextActions>
         <StackLayout Padding="15,0">
              <Label Text="{Binding title}" />
         </StackLayout>
      </ViewCell>
    </DataTemplate>
  </ListView.ItemTemplate>
</ListView>
```

Dans le fichier code-behind, vérifiez que le `Clicked` méthodes sont implémentées :

```csharp
public void OnMore (object sender, EventArgs e) {
    var mi = ((MenuItem)sender);
    DisplayAlert("More Context Action", mi.CommandParameter + " more context action", "OK");
}

public void OnDelete (object sender, EventArgs e) {
    var mi = ((MenuItem)sender);
    DisplayAlert("Delete Context Action", mi.CommandParameter + " delete context action", "OK");
}
```

> [!NOTE]
> Le `NavigationPageRenderer` pour Android propose un substituable `UpdateMenuItemIcon` méthode qui peut être utilisé pour charger des icônes à partir d’un personnalisé `Drawable`. Ce remplacement permet d’utiliser les images SVG sous forme d’icônes sur `MenuItem` instances sur Android.

<a name="Pull_to_Refresh" />

## <a name="pull-to-refresh"></a>Extraire pour actualiser

Les utilisateurs s’attendent que l’extraction vers le bas de la liste de données actualisera cette liste. [`ListView`](xref:Xamarin.Forms.ListView) prend en charge cette out-of-the-box. Pour activer la fonctionnalité d’extraction pour actualiser, définissez [ `IsPullToRefreshEnabled` ](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) à `true`:

```xaml
<ListView ...
          IsPullToRefreshEnabled="true" />
```

Le code C# équivalent est :

```csharp
listView.IsPullToRefreshEnabled = true;
```

Une boucle de progression s’affiche lors de l’actualisation, qui est par défaut le noir. Toutefois, la couleur du compteur peut être modifiée sur iOS et Android en définissant le `RefreshControlColor` propriété à un [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<ListView ...
          IsPullToRefreshEnabled="true"
          RefreshControlColor="Red" />
```

Le code C# équivalent est :

```csharp
listView.RefreshControlColor = Color.Red;
```

Les captures d’écran suivantes montrent les extraire pour actualiser comme l’extraction de l’utilisateur :

![](interactivity-images/refresh-start.png "Extraction de ListView pour actualiser en cours d’exécution")

Les captures d’écran suivantes montrent les extraire pour actualiser une fois que l’utilisateur a relâché l’extraction, le compteur est affichée alors que le [ `ListView` ](xref:Xamarin.Forms.ListView) met à jour :

![](interactivity-images/refresh-in-progress.png "Extraction de ListView pour l’actualisation terminée")

[`ListView`](xref:Xamarin.Forms.ListView) se déclenche le [ `Refreshing` ](xref:Xamarin.Forms.ListView.Refreshing) événement pour lancer l’actualisation et le [ `IsRefreshing` ](xref:Xamarin.Forms.ListView.IsRefreshing) propriété sera définie `true`. Le code est nécessaire pour actualiser le contenu de la `ListView` doit ensuite être exécutées par le Gestionnaire d’événements pour le `Refreshing` événement, ou par la méthode exécutée par le [ `RefreshCommand` ](xref:Xamarin.Forms.ListView.RefreshCommand). Une fois le `ListView` est actualisé, le `IsRefreshing` propriété doit être définie sur `false`, ou le [ `EndRefresh` ](xref:Xamarin.Forms.ListView.EndRefresh) méthode doit être appelée pour indiquer que l’actualisation est terminée.

> [!NOTE]
> Lorsque vous définissez un [ `RefreshCommand` ](xref:Xamarin.Forms.ListView.RefreshCommand), le `CanExecute` méthode de la commande peut être spécifiée pour activer ou désactiver la commande.

## <a name="related-links"></a>Liens associés

- [Interactivité de ListView (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
