---
title: Définition ItemsSource propriété d’un sélecteur
description: L’affichage du sélecteur est un contrôle pour la sélection d’un élément de texte dans une liste de données. Cet article explique comment remplir un sélecteur de données en définissant la propriété ItemsSource et répondre à la sélection d’éléments par l’utilisateur.
ms.prod: xamarin
ms.assetid: 8ECF390C-9DB2-4441-B9A3-101AE7E5AEC5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: bf3940bc1bc0318bad4d785388f9dc9292af80ca
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30789037"
---
# <a name="setting-a-pickers-itemssource-property"></a>Définition ItemsSource propriété d’un sélecteur

_L’affichage du sélecteur est un contrôle pour la sélection d’un élément de texte dans une liste de données. Cet article explique comment remplir un sélecteur de données en définissant la propriété ItemsSource et répondre à la sélection d’éléments par l’utilisateur._

Xamarin.Forms 2.3.4 a amélioré la [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) affichage en ajoutant la possibilité de le remplir avec des données en définissant sa [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) propriété et pour récupérer l’élément sélectionné à partir de la [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) propriété. En outre, la couleur du texte de l’élément sélectionné peut être modifiée en définissant le [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.TextColor/) propriété un [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/).

## <a name="populating-a-picker-with-data"></a>Remplissage d’un sélecteur de données

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) peuvent être remplis avec des données en définissant sa [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) propriété un `IList` collection. Chaque élément dans la collection de type doit être de, ou dérivé, `object`. Les éléments peuvent être ajoutés en XAML en initialisant le `ItemsSource` propriété à partir d’un tableau d’éléments :

```xaml
<Picker x:Name="picker" Title="Select a monkey">
  <Picker.ItemsSource>
    <x:Array Type="{x:Type x:String}">
      <x:String>Baboon</x:String>
      <x:String>Capuchin Monkey</x:String>
      <x:String>Blue Monkey</x:String>
      <x:String>Squirrel Monkey</x:String>
      <x:String>Golden Lion Tamarin</x:String>
      <x:String>Howler Monkey</x:String>
      <x:String>Japanese Macaque</x:String>
    </x:Array>
  </Picker.ItemsSource>
</Picker>
```

> [!NOTE]
> Notez que la `x:Array` élément requiert une `Type` attribut indiquant le type des éléments du tableau.

Le code c# équivalent est indiqué ci-dessous :

```csharp
var monkeyList = new List<string>();
monkeyList.Add("Baboon");
monkeyList.Add("Capuchin Monkey");
monkeyList.Add("Blue Monkey");
monkeyList.Add("Squirrel Monkey");
monkeyList.Add("Golden Lion Tamarin");
monkeyList.Add("Howler Monkey");
monkeyList.Add("Japanese Macaque");

var picker = new Picker { Title = "Select a monkey" };
picker.ItemsSource = monkeyList;
```

## <a name="responding-to-item-selection"></a>Réponse à la sélection de l’élément

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) prend en charge la sélection d’un élément à la fois. Lorsqu’un utilisateur sélectionne un élément, le [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) se déclenche des événements, le [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) propriété est mis à jour vers un entier qui représente l’index de l’élément sélectionné dans la liste et le [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) propriété est mis à jour vers la `object` représentant l’élément sélectionné. Le [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) propriété est un numéro de base zéro qui indique l’élément de l’utilisateur sélectionné. Si aucun élément n’est sélectionné, ce qui est le cas lorsque le [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) est tout d’abord créé et initialisé, `SelectedIndex` sera -1.

> [!NOTE]
> Élément de comportement de la sélection dans un [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) peuvent être personnalisés sur iOS avec une plateforme spécifique. Pour plus d’informations, consultez [sélection d’éléments de contrôle de sélecteur de](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode).

L’exemple de code suivant montre comment récupérer le [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) valeur de propriété à partir de la [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) en XAML :

```xaml
<Label Text="{Binding Source={x:Reference picker}, Path=SelectedItem}" />
```

Le code c# équivalent est indiqué ci-dessous :

```csharp
var monkeyNameLabel = new Label();
monkeyNameLabel.SetBinding(Label.TextProperty, new Binding("SelectedItem", source: picker));
```

En outre, un gestionnaire d’événements peut être exécutée lorsque la [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) se déclenche des événements :

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs e)
{
  var picker = (Picker)sender;
  int selectedIndex = picker.SelectedIndex;

  if (selectedIndex != -1)
  {
    monkeyNameLabel.Text = (string)picker.ItemsSource[selectedIndex];
  }
}
```

Cette méthode obtient le [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) valeur de propriété et utilise la valeur pour récupérer l’élément sélectionné de la [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) collection. Ceci est fonctionnellement équivalent à la récupération de l’élément sélectionné de la [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) propriété. Notez que chaque élément dans le `ItemsSource` collection est de type `object`et par conséquent, doivent être converties en un `string` pour l’affichage.

> [!NOTE]
> A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) peuvent être initialisés pour afficher un élément spécifique en définissant le [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) ou [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) propriétés. Toutefois, ces propriétés doivent être définies après l’initialisation du [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) collection.

## <a name="populating-a-picker-with-data-using-data-binding"></a>Remplissage d’un sélecteur de données à l’aide de la liaison de données

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) peuvent être également remplis avec des données à l’aide de la liaison de données pour lier son [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) propriété un `IList` collection. En XAML, cela est réalisé à le [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) extension de balisage :

```xaml
<Picker Title="Select a monkey" ItemsSource="{Binding Monkeys}" ItemDisplayBinding="{Binding Name}" />
```

Le code c# équivalent est indiqué ci-dessous :

```csharp
var picker = new Picker { Title = "Select a monkey" };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.ItemDisplayBinding = new Binding("Name");
```

Le [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) les données de propriété est liée à la `Monkeys` propriété du modèle de la vue connectée, qui retourne un `IList<Monkey>` collection. Montre l’exemple de code suit le `Monkey` (classe), qui contient les quatre propriétés :

```csharp
public class Monkey
{
  public string Name { get; set; }
  public string Location { get; set; }
  public string Details { get; set; }
  public string ImageUrl { get; set; }
}
```

Lors de la liaison à une liste d’objets, le [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) doit être informé de la propriété à afficher à partir de chaque objet. Cela s’effectue en définissant le [ `ItemDisplayBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemDisplayBinding/) propriété à la propriété requise à partir de chaque objet. Dans les exemples de code ci-dessus, le `Picker` est configuré pour afficher chaque `Monkey.Name` valeur de propriété.

### <a name="responding-to-item-selection"></a>Réponse à la sélection de l’élément

Liaison de données peut être utilisée pour définir un objet à la [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) la valeur de propriété lorsqu’il est modifié :

```xaml
<Picker Title="Select a monkey"
        ItemsSource="{Binding Monkeys}"
        ItemDisplayBinding="{Binding Name}"
        SelectedItem="{Binding SelectedMonkey}" />
<Label Text="{Binding SelectedMonkey.Name}" ... />
<Label Text="{Binding SelectedMonkey.Location}" ... />
<Image Source="{Binding SelectedMonkey.ImageUrl}" ... />
<Label Text="{Binding SelectedMonkey.Details}" ... />
```

Le code c# équivalent est indiqué ci-dessous :

```csharp
var picker = new Picker { Title = "Select a monkey" };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.SetBinding(Picker.SelectedItemProperty, "SelectedMonkey");
picker.ItemDisplayBinding = new Binding("Name");

var nameLabel = new Label { ... };
nameLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Name");

var locationLabel = new Label { ... };
locationLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Location");

var image = new Image { ... };
image.SetBinding(Image.SourceProperty, "SelectedMonkey.ImageUrl");

var detailsLabel = new Label();
detailsLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Details");
```

Le [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) les données de propriété est liée à la `SelectedMonkey` propriété du modèle de la vue connectée, ce qui est de type `Monkey`. Par conséquent, lorsque l’utilisateur sélectionne un élément dans le [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), le `SelectedMonkey` propriété sera définie au `Monkey` objet. Le `SelectedMonkey` données d’objet s’affiche dans l’interface utilisateur par [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) et [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) vues :

![](populating-itemssource-images/monkeys.png "Sélection d’éléments de sélecteur")

> [!NOTE]
> Notez que la [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) et [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) deux propriétés prend en charge des liaisons bidirectionnelles par défaut.

## <a name="summary"></a>Récapitulatif

Le [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) vue est un contrôle pour la sélection d’un élément de texte dans une liste de données. Cet article a expliqué comment remplir un `Picker` avec des données en définissant le [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) propriété et comment répondre à la sélection d’éléments par l’utilisateur. Cette approche, qui a été introduite dans Xamarin.Forms 2.3.4, est l’approche recommandée pour interagir avec un `Picker`.


## <a name="related-links"></a>Liens associés

- [Démonstration de sélecteur (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)
- [Singe application (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/MonkeyAppPicker/)
- [Sélecteur pouvant être lié (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindablePicker/)
- [Picker](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
