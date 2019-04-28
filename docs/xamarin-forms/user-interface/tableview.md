---
title: Xamarin.Forms TableView
description: Cet article explique comment utiliser la classe Xamarin.Forms TableView pour présenter des menus déroulants, les paramètres et les formulaires de saisie dans les applications.
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2018
ms.openlocfilehash: c18eba873dc1a1dae36c401507d55652ed233b00
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61194582"
---
# <a name="xamarinforms-tableview"></a>Xamarin.Forms TableView

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)

[`TableView`](xref:Xamarin.Forms.TableView) est une vue pour afficher des listes permettant le défilement de données ou des choix où il existe des lignes qui ne partagent pas le même modèle. Contrairement aux [ListView](~/xamarin-forms/user-interface/listview/index.md), `TableView` n’a pas le concept d’un `ItemsSource`, de sorte que les éléments doivent être ajoutées manuellement en tant qu’enfants.

![](tableview-images/tableview-all-sml.png "Exemple de TableView")

<a name="Use_Cases" />

## <a name="use-cases"></a>Cas d’usage

[`TableView`](xref:Xamarin.Forms.TableView) est utile quand :

- présentation d’une liste de paramètres,
- collecte des données dans un formulaire, ou
- affichage des données présentées différemment à partir de la ligne à ligne (par exemple, nombres, des pourcentages et images).

[`TableView`](xref:Xamarin.Forms.TableView) gère le défilement et la disposition des lignes dans les sections attrayantes, un besoin commun pour les scénarios ci-dessus. Le `TableView` contrôle utilise de chaque plateforme sous-jacent vue équivalente lorsqu’il est disponible, la création d’une apparence native pour chaque plateforme.

<a name="TableView_Structure" />

## <a name="structure"></a>Structure

Éléments dans un [ `TableView` ](xref:Xamarin.Forms.TableView) sont organisées en sections. À la racine de la `TableView` est la [ `TableRoot` ](xref:Xamarin.Forms.TableRoot), qui est parent à un ou plusieurs [ `TableSection` ](xref:Xamarin.Forms.TableSection) instances. Chaque [ `TableSection` ](xref:Xamarin.Forms.TableSection) se compose d’un en-tête et un ou plusieurs [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) instances :

```xaml
<TableView Intent="Settings">
    <TableRoot>
        <TableSection Title="Ring">
            <SwitchCell Text="New Voice Mail" />
            <SwitchCell Text="New Mail" On="true" />
        </TableSection>
    </TableRoot>
</TableView>
```

Le code C# équivalent est :

```csharp
Content = new TableView
{
    Root = new TableRoot
    {
        new TableSection("Ring")
        {
          // TableSection constructor takes title as an optional parameter
          new SwitchCell { Text = "New Voice Mail" },
          new SwitchCell { Text = "New Mail", On = true }
        }
    },
    Intent = TableIntent.Settings
};
```

<a name="TableView_Appearance" />

## <a name="appearance"></a>Apparence

[`TableView`](xref:Xamarin.Forms.TableView) expose les [ `Intent` ](xref:Xamarin.Forms.TableView.Intent) propriété, qui peut être définie à un de le [ `TableIntent` ](xref:Xamarin.Forms.TableIntent) membres de l’énumération :

- `Data` – à utiliser lors de l’affichage des entrées de données. Notez que [ListView](~/xamarin-forms/user-interface/listview/index.md) peut être une meilleure option pour le défilement des listes de données.
- `Form` – à utiliser lorsque le TableView agit comme un formulaire.
- `Menu` – à utiliser lors de la présentation d’un menu des sélections.
- `Settings` – à utiliser lors de l’affichage d’une liste de paramètres de configuration.

Le [ `TableIntent` ](xref:Xamarin.Forms.TableIntent) valeur que vous choisissez peut affecter la [ `TableView` ](xref:Xamarin.Forms.TableView) apparaît sur chaque plateforme. Même s’il existe n'efface pas les différences, il est recommandé de sélectionner le `TableIntent` qui correspond le mieux à comment vous avez l’intention d’utiliser la table.

En outre, la couleur du texte affiché pour chaque [ `TableSection` ](xref:Xamarin.Forms.TableSection) peut être modifié en définissant le `TextColor` propriété à un [ `Color` ](xref:Xamarin.Forms.Color).

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>Cellules intégrés

Xamarin.Forms est fourni avec les cellules intégrées pour collecter et afficher des informations. Bien que [ `ListView` ](xref:Xamarin.Forms.ListView) et [ `TableView` ](xref:Xamarin.Forms.TableView) peut utiliser toutes les cellules de mêmes, [ `SwitchCell` ](xref:Xamarin.Forms.SwitchCell) et [ `EntryCell` ](xref:Xamarin.Forms.EntryCell)sont les plus pertinentes pour un `TableView` scénario.

Consultez [apparence d’une cellule ListView](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) pour une description détaillée de [TextCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) et [ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell).

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell) est le contrôle à utiliser pour présenter et capture un activé/désactivé ou `true` / `false` état. Il définit les propriétés suivantes :

- `Text` : texte à afficher à côté du commutateur.
- `On` : indique si le commutateur est affiché en tant qu’on ou off.
- `OnColor` – le [ `Color` ](xref:Xamarin.Forms.Color) du commutateur lorsqu’il est à la position on.

Toutes ces propriétés peuvent être liés.

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell) expose également le `OnChanged` événement, ce qui vous permet de répondre aux modifications de l’état de la cellule.

![](tableview-images/switch-cell.png "Exemple de SwitchCell")

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell

[`EntryCell`](xref:Xamarin.Forms.EntryCell) est utile lorsque vous avez besoin afficher des données de texte que l’utilisateur peut modifier. Il définit les propriétés suivantes :

- `Keyboard` – Le clavier à afficher lors de la modification. Il existe des options pour les éléments tels que des valeurs numériques, e-mail, numéros de téléphone, etc. [Consultez la documentation sur les API](xref:Xamarin.Forms.Keyboard).
- `Label` – Le texte de l’étiquette à afficher à gauche de la zone de texte.
- `LabelColor` – La couleur du texte d’étiquette.
- `Placeholder` : Texte à afficher dans le champ d’entrée lorsqu’il est null ou vide. Ce texte disparaît lorsque l’entrée de texte commence.
- `Text` – Le texte dans le champ d’entrée.
- `HorizontalTextAlignment` – L’alignement horizontal du texte. Peut être, gauche ou droite centré. [Consultez la documentation sur les API](xref:Xamarin.Forms.TextAlignment).

[`EntryCell`](xref:Xamarin.Forms.EntryCell) expose également le `Completed` événement, qui est déclenché quand l’utilisateur appuie sur le bouton « terminé » sur le clavier lors de la modification de texte.

![](tableview-images/entry-cell.png "Exemple de EntryCell")

<a name="Custom_Cells" />

## <a name="custom-cells"></a>Cellules personnalisées

Les cellules intégrées ne suffisent pas, les cellules personnalisés permet de présenter et de capturer des données de la façon appropriée pour votre application. Par exemple, vous souhaiterez présenter un curseur pour autoriser un utilisateur de choisir l’opacité d’une image.

Toutes les cellules personnalisés doivent dériver de [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), la même classe de base que tous de la cellule intégrée types utilisent.

Il s’agit d’un exemple d’une cellule personnalisée :

![](tableview-images/custom-cell.png "Exemple de cellule personnalisée")

L’exemple suivant montre le XAML utilisé pour créer le [ `TableView` ](xref:Xamarin.Forms.TableView) dans les captures d’écran ci-dessus :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DemoTableView.TablePage"
             Title="TableView">
      <TableView Intent="Settings">
          <TableRoot>
              <TableSection Title="Getting Started">
                  <ViewCell>
                      <StackLayout Orientation="Horizontal">
                          <Image Source="bulb.png" />
                          <Label Text="left"
                                 TextColor="#f35e20" />
                          <Label Text="right"
                                 HorizontalOptions="EndAndExpand"
                                 TextColor="#503026" />
                      </StackLayout>
                  </ViewCell>
              </TableSection>
          </TableRoot>
      </TableView>
</ContentPage>
```

Le code C# équivalent est :

```csharp
var table = new TableView();
table.Intent = TableIntent.Settings;
var layout = new StackLayout() { Orientation = StackOrientation.Horizontal };
layout.Children.Add (new Image() { Source = "bulb.png"});
layout.Children.Add (new Label()
{
    Text = "left",
    TextColor = Color.FromHex("#f35e20"),
    VerticalOptions = LayoutOptions.Center
});
layout.Children.Add (new Label ()
{
    Text = "right",
    TextColor = Color.FromHex ("#503026"),
    VerticalOptions = LayoutOptions.Center,
    HorizontalOptions = LayoutOptions.EndAndExpand
});
table.Root = new TableRoot ()
{
    new TableSection("Getting Started")
    {
        new ViewCell() {View = layout}
    }
};
Content = table;
```

L’élément racine sous le [ `TableView` ](xref:Xamarin.Forms.TableView) est la [ `TableRoot` ](xref:Xamarin.Forms.TableRoot)et qu’il existe un [ `TableSection` ](xref:Xamarin.Forms.TableSection) immédiatement en dessous de la `TableRoot`. Le [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) est défini directement sous le `TableSection`et un [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) est utilisé pour gérer la disposition de la cellule personnalisée, bien que toute disposition peut être utilisée ici.

> [!NOTE]
> Contrairement aux [ `ListView` ](xref:Xamarin.Forms.ListView), [ `TableView` ](xref:Xamarin.Forms.TableView) ne nécessite pas que personnalisé (ou toute) cellules sont définies dans un `ItemTemplate`.

## <a name="row-height"></a>Hauteur de ligne

Le [ `TableView` ](xref:Xamarin.Forms.TableView) classe a deux propriétés qui peuvent être utilisées pour modifier la hauteur des cellules :

- [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight) : définit la hauteur de chaque ligne à un `int`.
- [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) – les lignes ont différentes hauteurs si la valeur `true`. Notez que lorsque vous définissez cette propriété pour `true`, hauteurs de lignes seront automatiquement calculées et appliqués par Xamarin.Forms.

Lorsque la hauteur du contenu dans une cellule dans un [ `TableView` ](xref:Xamarin.Forms.TableView) est modifié, la ligne hauteur est implicitement mis à jour sur Android et la plateforme universelle Windows (UWP). Toutefois, sur iOS il doit être forcé à mettre à jour en définissant le [ `HasUnevenRows` ](xref:Xamarin.Forms.TableView.HasUnevenRows) propriété `true` et en appelant le [ `Cell.ForceUpdateSize` ](xref:Xamarin.Forms.Cell.ForceUpdateSize) (méthode).

L’exemple XAML suivant montre un [ `TableView` ](xref:Xamarin.Forms.TableView) qui contient un [ `ViewCell` ](xref:Xamarin.Forms.ViewCell):

```xaml
<ContentPage ...>
    <TableView ...
               HasUnevenRows="true">
        <TableRoot>
            ...
            <TableSection ...>
                ...
                <ViewCell x:Name="_viewCell"
                          Tapped="OnViewCellTapped">
                    <Grid Margin="15,0">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="Auto" />
                        </Grid.RowDefinitions>
                        <Label Text="Tap this cell." />
                        <Label x:Name="_target"
                               Grid.Row="1"
                               Text="The cell has changed size."
                               IsVisible="false" />
                    </Grid>
                </ViewCell>
            </TableSection>
        </TableRoot>
    </TableView>
</ContentPage>
```

Lorsque le [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) est activé par un clic, le `OnViewCellTapped` Gestionnaire d’événements est exécuté :

```csharp
void OnViewCellTapped(object sender, EventArgs e)
{
    _target.IsVisible = !_target.IsVisible;
    _viewCell.ForceUpdateSize();
}
```

Le `OnViewCellTapped` Gestionnaire d’événements affiche ou masque la seconde [ `Label` ](xref:Xamarin.Forms.Label) dans le [ `ViewCell` ](xref:Xamarin.Forms.ViewCell)et met à jour explicitement de taille de la cellule en appelant le [ `Cell.ForceUpdateSize` ](xref:Xamarin.Forms.Cell.ForceUpdateSize) (méthode).

Les captures d’écran suivantes montrent la cellule avant d’être mis sur écoute sur :

![](tableview-images/cell-beforeresize.png "ViewCell avant d’en cours de redimensionnement")

Les captures d’écran suivantes montrent la cellule après être mis sur écoute sur :

![](tableview-images/cell-afterresize.png "ViewCell après redimensionnement")

> [!IMPORTANT]
> Il est fort probable de dégradation des performances si cette fonctionnalité est utilisée de manière excessive.

## <a name="related-links"></a>Liens connexes

- [TableView (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)
