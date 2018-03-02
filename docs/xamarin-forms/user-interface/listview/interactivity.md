---
title: "Interactivité de ListView"
description: "Ajouter une interactivité à votre contrôle ListView en implémentant les sélections, balayez à supprimer et tirer pour actualiser."
ms.topic: article
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 74b275b77b6a70b3d074d68b14a97c73615753d2
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="listview-interactivity"></a>Interactivité de ListView

ListView prend en charge l’interaction avec les données qu’il présente par les moyens suivants :

- [**Sélection & Drainages** ](#selectiontaps) &ndash; répondre aux clics et des sélections/désélections non contiguës d’éléments. Activer ou désactiver la sélection de ligne (activée par défaut).
- [**Actions du contexte** ](#Context_Actions) &ndash; exposent des fonctionnalités par élément, par exemple, effectuez un balayage à supprimer.
- [**Tirer pour actualiser** ](#Pull_to_Refresh) &ndash; implémenter l’idiome de tirer pour actualiser les utilisateurs proviennent d’attendre des expériences natifs.

<a name="selectiontaps" />

## <a name="selection--taps"></a>Sélection & clics
`ListView` prend en charge la sélection d’un élément à la fois. La sélection est activé par défaut. Quand un utilisateur appuie sur un élément, deux événements sont déclenchés : `ItemTapped` et `ItemSelected`. Notez que si vous appuyez deux fois sur le même élément de ne déclenche pas plusieurs `ItemSelected` événements, mais se déclenche plusieurs `ItemTapped` événements. Notez également que `ItemSelected` sera appelé si un élément est désélectionné.

N’oubliez pas que `ItemSelected` est appelé lorsque les éléments sont désélectionnés et lorsqu’ils sont sélectionnés. Cela signifie que vous devez vérifier les valeurs null `SelectedItem` dans votre `ItemSelected` Gestionnaire d’événements avant que vous puissiez l’utiliser :

```csharp
void OnSelection (object sender, SelectedItemChangedEventArgs e)
{
  if (e.SelectedItem == null) {
    return; //ItemSelected is called on deselection, which results in SelectedItem being set to null
  }
  DisplayAlert ("Item Selected", e.SelectedItem.ToString (), "Ok");
  //((ListView)sender).SelectedItem = null; //uncomment line if you want to disable the visual selection state.
}
```

### <a name="disabling-selection"></a>La désactivation de la sélection

Si vous souhaitez désactiver la sélection, gérer la `ItemSelected` événement et définissez le `SelectedItem` propriété Null :

```csharp
SelectionDemoList.ItemSelected += (sender, e) => {
    ((ListView)sender).SelectedItem = null;
};
```

La sélection activée :

![](interactivity-images/selection-default.png "ListView avec sélection activée")

Notez que sur Windows Phone, certaines cellules, y compris `SwitchCell` ne mettez pas à jour leur état visuel en réponse à la sélection.

<a name="Context_Actions" />

## <a name="context-actions"></a>Actions du contexte
Souvent, les utilisateurs souhaitent effectuer une opération sur un élément dans un `ListView`. Par exemple, considérez une liste d’adresses de messagerie dans l’application de messagerie. Sur iOS, vous pouvez passez pour supprimer un message et sur Windows Phone, vous pouvez long, appuyez sur un message, puis supprimez-le :

![](interactivity-images/context-default.png "ListView avec des Actions de contexte")

Actions de contexte peuvent être implémentées en c# et XAML. Vous trouverez ci-dessous les guides spécifiques pour les deux, mais tout d’abord nous allons examiner certains détails d’implémentation de la clé pour les deux.

Les Actions de contexte sont créées à l’aide de `MenuItem`s. Appuyez sur pour les MenuItems sont déclenchés par le MenuItem lui-même, pas le ListView. Cela est différent à partir de la gestion des événements de clic pour les cellules, où le ListView déclenche l’événement au lieu de la cellule. Étant donné que le ListView déclenche l’événement, son gestionnaire d’événements reçoit des informations clés, à laquelle l’élément a été sélectionné ou cliqué.

Par défaut, un MenuItem n’a aucun moyen de savoir quelle cellule auquel il appartient. `CommandParameter` est disponible sur `MenuItem` pour stocker des objets, tels que l’objet derrière l’écran de MenuItem ViewCell. `CommandParameter` peut être définie en XAML et c#.

### <a name="c"></a>C#  

Actions de contexte peuvent être implémentées dans un `Cell` sous-classe (tant qu’il n’est pas utilisé comme un en-tête de groupe) en créant `MenuItem`s et en les ajoutant à la `ContextActions` collection pour la cellule. Les éléments suivants sont propriétés peuvent être configurées pour l’action de contexte :

* **Texte** &ndash; la chaîne qui apparaît dans l’élément de menu.
* **Un clic sur** &ndash; l’événement lorsque l’élément est activé.
* **IsDestructive** &ndash; (facultatif) la valeur True l’élément est rendu de manière différente sur iOS.

Plusieurs actions de contexte peuvent être ajoutées à une cellule, mais seul l’un doit avoir `IsDestructive` la valeur `true`. Le code suivant illustre la façon dont les actions de contexte est ajoutées à un `ViewCell`:

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

`MenuItem`s peuvent également être créés dans une collection XAML déclarative. Le code XAML suivant montre une cellule personnalisée avec deux actions contexte implémentées :

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

<a name="Pull_to_Refresh" />

## <a name="pull-to-refresh"></a>Extraction de l’actualisation
Les utilisateurs ont en droit d’attendre que l’extraction vers le bas dans une liste de données actualisera cette liste. `ListView` prend en charge cette out of box. Pour activer la fonctionnalité d’extraction pour actualiser, définissez `IsPullToRefreshEnabled` sur true :

```csharp
listView.IsPullToRefreshEnabled = true;
```

L’extraction de tirer pour actualiser en tant que l’utilisateur :

![](interactivity-images/refresh-start.png "Extraction de ListView à actualiser en cours")

Tirer pour actualiser en tant que l’utilisateur a publié l’extraction. C’est ce que voit l’utilisateur lors de la mise à jour de liste : ![ ] (interactivity-images/refresh-in-progress.png "extraction ListView à actualiser terminée")

Notez que, à compter de Xamarin.Forms 1.4.3, actualisation de l’extraction n’est pas pris en charge sur Windows Phone 8.1. Sur Windows phone 8, tirer pour actualiser n’est pas une fonctionnalité de plateforme natifs, donc une implémentation de l’actualisation de l’extraction est fournie par Xamarin.Forms. Enfin, gardez à l’esprit que par extraction à l’actualisation ne fonctionnera pas sur Windows Phone si tous les éléments dans la liste peuvent s’ajuster à l’écran (en d’autres termes, si le défilement vertical n’est pas obligatoire).

ListView expose quelques événements qui vous permettent de répondre aux événements d’actualisation de l’extraction.

-  Le `RefreshCommand` sera appelé et le `Refreshing` événement appelé. `IsRefreshing` a la valeur `true`.
-  Vous devez exécuter le code qui est nécessaire pour actualiser le contenu de la liste, soit dans la commande ou l’événement.
-  Lors de l’actualisation terminée, appelez `EndRefresh` ou `IsRefreshing` à `false` pour indiquer à l’affichage de liste que vous avez terminé.

Le `CanExecute` propriété est respectée, qui vous donne un moyen de vérifier si la commande d’actualisation de l’extraction doit être activée.



## <a name="related-links"></a>Liens associés

- [Interactivité de ListView (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
- [notes de version 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [notes de version 1.3](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
