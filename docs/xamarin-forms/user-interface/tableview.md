---
title: TableView
description: Présente les menus déroulants, des paramètres et des formulaires de saisie.
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 312472fdfae65bc62b76f4295a13760236dededc
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847655"
---
# <a name="tableview"></a>TableView

[TableView](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) est un affichage pour l’affichage des listes permettant le défilement des données ou des choix lorsqu’il existe des lignes qui ne partagent pas le même modèle. Contrairement aux [ListView](~/xamarin-forms/user-interface/listview/index.md), TableView n’a pas le concept d’un `ItemsSource`, de sorte que les éléments doivent être ajoutés manuellement en tant qu’enfants.

Ce guide est composé des sections suivantes :

- **[Cas d’usage](#Use_Cases)**  &ndash; quand utiliser TableView au lieu de ListView ou un affichage personnalisé.
- **[Structure de TableView](#TableView_Structure)**  &ndash; la hiérarchie des affichages qui est nécessaire au sein d’un objet TableView.
- **[TableView apparence](#TableView_Appearance)**  &ndash; les options de personnalisation pour TableView.
- **[Cellules intégrés](#Built-In_Cells)**  &ndash; options cellule intégrés, y compris [EntryCell](#EntryCell) et [SwitchCell](#SwitchCell).
- **[Cellules personnalisés](#Custom_Cells)**  &ndash; comment rendre votre propre cellules personnalisés.

![](tableview-images/tableview-all-sml.png "Exemple de TableView")

<a name="Use_Cases" />

## <a name="use-cases"></a>Cas d’usage

TableView est utile dans les cas suivants :

- présentation d’une liste de paramètres,
- collecte des données dans un formulaire, ou
- affichage des données présentées différemment à partir de la ligne à la ligne (par exemple, les nombres, les pourcentages et les images).

TableView gère le défilement et disposer des lignes dans les sections attrayantes, un besoin commun pour les scénarios ci-dessus. Le `TableView` contrôle utilise de chaque plateforme sous-jacent vue équivalente lorsqu’il est disponible, la création d’un aspect natif pour chaque plateforme.

<a name="TableView_Structure" />

## <a name="tableview-structure"></a>Structure de TableView

Les éléments dans un `TableView` sont organisées en sections. À la racine de la `TableView` est la `TableRoot`, qui est le parent d’un ou plusieurs `TableSections`:

```csharp
Content = new TableView {
    Root = new TableRoot {
        new TableSection...
    },
    Intent = TableIntent.Settings
};
```

Chaque `TableSection` se compose d’un en-tête et un ou plusieurs ViewCells. Nous voyons ici le `TableSection`de `Title` propriété *« Anneau »* dans le constructeur :

```csharp
var section = new TableSection ("Ring") { //TableSection constructor takes title as an optional parameter
    new SwitchCell {Text = "New Voice Mail"},
    new SwitchCell {Text = "New Mail", On = true}
};
```

Pour accomplir la même disposition comme ci-dessus en XAML :

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

<a name="TableView_Appearance" />

## <a name="tableview-appearance"></a>TableView apparence

TableView expose le `Intent` propriété, qui est une énumération des options suivantes :

- **Données** &ndash; à utiliser lors de l’affichage des entrées de données. Notez que [ListView](~/xamarin-forms/user-interface/listview/index.md) peut être préférable d’opter pour le défilement des listes de données.
- **Formulaire** &ndash; à utiliser lorsque le TableView agit comme un formulaire.
- **Menu** &ndash; à utiliser lors de la présentation d’un menu de sélections.
- **Paramètres** &ndash; à utiliser lors de l’affichage d’une liste de paramètres de configuration.

Le `TableIntent` vous choisissez peut avoir une incidence sur la `TableView` s’affiche sur chaque plateforme. Même s’il existe n’efface ne pas les différences, il est recommandé de sélectionner le `TableIntent` qui correspond le mieux à comment vous comptez utiliser la table.

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>Cellules intégrés

Xamarin.Forms est fourni avec les cellules intégrées pour collecter et afficher des informations. Bien que ListView et TableView peuvent utiliser toutes les cellules de mêmes, les éléments suivants sont le plus approprié pour un scénario TableView :

- **SwitchCell** &ndash; de présentation et de capturer l’état d’un vrai/faux, ainsi que d’une étiquette de texte.
- **EntryCell** &ndash; de présentation et de capture de texte.

Consultez [apparence de la cellule ListView](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) pour obtenir une description détaillée de [TextCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) et [ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell).

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell
[`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/) est le contrôle à utiliser pour présenter et capture une activé/désactivé ou `true` / `false` état.

SwitchCells ont une ligne de texte à modifier et une propriété activé/désactivé. Ces deux propriétés peuvent être liés.

- `Text` &ndash; texte à afficher à côté du commutateur.
- `On` &ndash; Indique si le commutateur est affiché comme on ou off.

Notez que la `SwitchCell` expose le `OnChanged` événement, ce qui vous permet de répondre aux modifications d’état de la cellule.

![](tableview-images/switch-cell.png "Exemple de SwitchCell")

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell
[`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/) est utile lorsque vous avez besoin afficher les données texte que l’utilisateur peut modifier. `EntryCell`s offrent les propriétés suivantes peuvent être personnalisées :

- `Keyboard` &ndash; Le clavier à afficher lors de la modification. Il existe des options pour les éléments tels que les valeurs numériques, par courrier électronique, les numéros de téléphone, etc. [Consultez la documentation API](http://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/).
- `Label` &ndash; Le texte d’étiquette à afficher à droite de la zone de texte.
- `LabelColor` &ndash; La couleur du texte d’étiquette.
- `Placeholder` &ndash; Texte à afficher dans le champ d’entrée lorsqu’il est null ou vide. Ce texte disparaît lors de la saisie de texte commence.
- `Text` &ndash; Le texte dans le champ d’entrée.
- `HorizontalTextAlignment` &ndash; L’alignement horizontal du texte. Peut être, gauche ou droite, centré. [Consultez la documentation API](http://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/).

Notez que `EntryCell` expose le `Completed` événement, qui est déclenché quand l’utilisateur appuie sur 'done' sur le clavier lors de la modification du texte.

![](tableview-images/entry-cell.png "Exemple de EntryCell")

<a name="Custom_Cells" />

## <a name="custom-cells"></a>Cellules personnalisées
Lorsque les cellules intégrées ne suffisent pas, les cellules personnalisées peuvent servir à présenter et capturer les données de la façon appropriée pour votre application. Par exemple, vous souhaiterez présenter un curseur pour autoriser un utilisateur à choisir l’opacité d’une image.

Toutes les cellules personnalisés doivent dériver de [ `ViewCell` ](http://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), la même classe de base que tous les de la cellule intégrée types d’utilisation.

Il s’agit d’un exemple d’une cellule personnalisée :

![](tableview-images/custom-cell.png "Exemple de cellule personnalisé")

### <a name="xaml"></a>XAML
Le code XAML pour créer la disposition ci-dessus est ci-dessous :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="DemoTableView.TablePage" Title="TableView">
    <ContentPage.Content>
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
    </ContentPage.Content>
</ContentPage>

```

Le code XAML ci-dessus effectue beaucoup. Nous allons décomposer :

- L’élément racine sous la `TableView` est le `TableRoot`.
- Il existe un `TableSection` immédiatement sous le `TableRoot`.
- Le `ViewCell` est défini directement sous la valeur TableSection. Contrairement aux `ListView`, `TableView` ne nécessite pas que personnalisé (ou toute) cellules sont définies dans un `ItemTemplate`.
- Un StackLayout est utilisé pour gérer la disposition de la cellule personnalisée. Toute disposition peut être utilisée ici.

### <a name="cnum"></a>C&num;

Étant donné que `TableView` fonctionne avec les données statiques ou des données sont modifiées manuellement, il n’a pas le concept d’un modèle d’élément. Au lieu de cela, les cellules personnalisées peuvent être manuellement créés et placés dans la table. Notez que la technique de création personnalisé de la cellule qui hérite de `ViewCell`, puis ajoutez-le à la `TableView` comme vous le feriez une cellule intégrée, est également pris en charge.
Voici le code c# pour accomplir la disposition ci-dessus :

```csharp
var table = new TableView();
table.Intent = TableIntent.Settings;
var layout = new StackLayout() { Orientation = StackOrientation.Horizontal };
layout.Children.Add (new Image() {Source = "bulb.png"});
layout.Children.Add (new Label() {
    Text = "left",
    TextColor = Color.FromHex("#f35e20"),
    VerticalOptions = LayoutOptions.Center
});
layout.Children.Add (new Label () {
    Text = "right",
    TextColor = Color.FromHex ("#503026"),
    VerticalOptions = LayoutOptions.Center,
    HorizontalOptions = LayoutOptions.EndAndExpand
});
table.Root = new TableRoot () {
    new TableSection("Getting Started") {
        new ViewCell() {View = layout}
    }
};

Content = table;
```

C# ci-dessus effectue beaucoup. Nous allons décomposer :

- L’élément racine sous la `TableView` est le `TableRoot`.
- Il existe un `TableSection` immédiatement sous le `TableRoot`.
- Le `ViewCell` est défini directement sous la valeur TableSection. Contrairement aux `ListView`, `TableView` ne nécessite pas que personnalisé (ou toute) cellules sont définies dans un `ItemTemplate`.
- Un StackLayout est utilisé pour gérer la disposition de la cellule personnalisée. Toute disposition peut être utilisée ici.

Notez qu’une classe pour la cellule personnalisée n’est jamais définie. Au lieu de cela, le `ViewCell`de propriété de la vue est définie pour une instance particulière de `ViewCell`.



## <a name="related-links"></a>Liens associés

- [TableView (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)
