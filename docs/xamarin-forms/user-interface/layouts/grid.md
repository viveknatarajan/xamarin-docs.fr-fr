---
title: Grille
description: Affichages présents dans les grilles.
ms.prod: xamarin
ms.assetid: 762B1802-D185-494C-B643-74EED55882FE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: 084fde62687bd9bdd84779e4486dd8a971d1acdd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="grid"></a>Grille

[`Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) prend en charge la réorganisation des vues en lignes et colonnes. Lignes et colonnes peuvent être définies à avoir une taille proportionnelle ou tailles absolues. Le `Grid` ne doivent pas être confondue avec les tables traditionnelles de mise en page et n’est pas destinée à présenter les données tabulaires. `Grid` n’a pas le concept de ligne, de colonne ou de mise en forme de cellule. Contrairement aux tableaux HTML, `Grid` est destiné exclusivement pour la disposition du contenu.

[![](grid-images/layouts-sml.png "Les dispositions de Xamarin.Forms")](grid-images/layouts.png#lightbox "Xamarin.Forms dispositions")

Cet article couvre :

- **[Objectif](#Purpose)**  &ndash; utilisations courantes pour `Grid`.
- **[L’utilisation de](#Usage)**  &ndash; comment utiliser `Grid` pour atteindre votre conception de votre choix.
  - **[Lignes et colonnes](#Rows_and_Columns)**  &ndash; spécifier des lignes et des colonnes pour le `Grid`.
  - **[Placer des vues](#Placing_Views)**  &ndash; ajouter des vues dans la grille au niveau des lignes et colonnes spécifiques.
  - **[Espacement](#Spacing)**  &ndash; configurer les espaces entre les lignes et colonnes.
  - **[Étendues](#Spans)**  &ndash; configurer les éléments de s’étendre sur plusieurs lignes ou colonnes.

![](grid-images/grid.png "Exploration de la grille")

## <a name="purpose"></a>Objectif

`Grid` peut être utilisé pour organiser des vues dans une grille. Cela est utile dans un nombre de cas :

- Réorganiser les boutons dans une application de calculatrice
- Organiser des boutons/choix dans une grille, telles qu’iOS ou Android écrans d’accueil
- Réorganiser les vues afin qu’ils soient de taille égale à une dimension (par exemple, dans certaines barres d’outils)

## <a name="usage"></a>Utilisation

Contrairement aux tables traditionnelles, `Grid` ne reconnaît pas le nombre et la taille des lignes et des colonnes à partir du contenu. Au lieu de cela, `Grid` a `RowDefinitions` et `ColumnDefinitions` collections. Ces derniers contiennent les définitions de combien de lignes et de colonnes seront disposés. Affichages sont ajoutés au `Grid` avec la ligne spécifiée et les index de colonne, qui identifient les lignes et des colonnes d’une vue doit être placée dans.

<a name="Rows_and_Columns" />

### <a name="rows-and-columns"></a>Lignes et colonnes

Les informations de ligne et de colonne sont stockées dans `Grid`de `RowDefinitions`  &  `ColumnDefinitions` propriétés, qui sont des collections de chaque de [ `RowDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RowDefinition/) et [ `ColumnDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColumnDefinition/)des objets, respectivement. `RowDefinition` a une propriété unique, `Height`, et `ColumnDefinition` a une propriété unique, `Width`. Les options pour la hauteur et la largeur sont les suivantes :

- **Auto** &ndash; automatiquement les tailles en fonction du contenu dans la ligne ou colonne. Spécifié en tant que [ `GridUnitType.Auto` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridUnitType/) en c# ou en tant que `Auto` en XAML.
- **Proportional(*)** &ndash; tailles des colonnes et des lignes sous forme de proportion de l’espace restant. Exprimé par une valeur et `GridUnitType.Star` en c# et en tant que `#*` en XAML, avec `#` en cours de la valeur souhaitée. Spécification d’une ligne/colonne avec `*` entraîne remplir l’espace disponible.
- **Absolu** &ndash; tailles des colonnes et des lignes avec des valeurs de hauteur et largeur fixes, spécifiques. Exprimé par une valeur et `GridUnitType.Absolute` en c# et en tant que `#` en XAML, avec `#` en cours de la valeur souhaitée.

> [!NOTE]
> Les valeurs de largeur pour les colonnes sont définies en tant que « * » par défaut dans Xamarin.Forms, ce qui garantit que la colonne remplissent l’espace disponible.

Imaginez une application qui a besoin de trois lignes et deux colonnes. La ligne inférieure doit être exactement les 200px en hauteur et la ligne supérieure doit être deux fois la hauteur de la ligne du milieu. La colonne de gauche doit être suffisamment large pour ajuster le contenu et la colonne de droite doit remplir l’espace restant.

En XAML :

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="2*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="200" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
</Grid>
```

En C# :

```csharp
var grid = new Grid();
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength(2, GridUnitType.Star) });
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength(200)});
grid.ColumnDefinitions.Add (new ColumnDefinition{ Width = new GridLength (200) });
```

<a name="Placing_Views" />

### <a name="placing-views-in-a-grid"></a>Placer des vues dans une grille

Pour placer des vues dans un `Grid` vous devez les ajouter comme enfants à la grille, puis spécifiez les lignes et colonnes qu’ils appartiennent dans.

En XAML, utilisez `Grid.Row` et `Grid.Column` sur chaque vue individuelle pour spécifier la sélection élective. Notez que `Grid.Row` et `Grid.Column` spécifier l’emplacement basé sur les listes de lignes et colonnes de base zéro. Cela signifie que dans une 4x4 grille, la cellule supérieure gauche est (0,0) et la cellule inférieure droite est (3,3).

Le `Grid` illustré ci-dessous contient quatre cellules :

![](grid-images/label-grid.png "Grille avec quatre affichages")

En XAML :

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
  <Label Text="Top Left" Grid.Row="0" Grid.Column="0" />
  <Label Text="Top Right" Grid.Row="0" Grid.Column="1" />
  <Label Text="Bottom Left" Grid.Row="1" Grid.Column="0" />
  <Label Text="Bottom Right" Grid.Row="1" Grid.Column="1" />
</Grid>
```

En C# :

```csharp
var grid = new Grid();

grid.RowDefinitions.Add(new RowDefinition { Height = new GridLength(1, GridUnitType.Star)});
grid.RowDefinitions.Add(new RowDefinition { Height = new GridLength(1, GridUnitType.Star)});
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(1, GridUnitType.Star)});
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(1, GridUnitType.Star)});

var topLeft = new Label { Text = "Top Left" };
var topRight = new Label { Text = "Top Right" };
var bottomLeft = new Label { Text = "Bottom Left" };
var bottomRight = new Label { Text = "Bottom Right" };

grid.Children.Add(topLeft, 0, 0);
grid.Children.Add(topRight, 0, 1);
grid.Children.Add(bottomLeft, 1, 0);
grid.Children.Add(bottomRight, 1, 1);
```

Le code ci-dessus crée une grille avec quatre étiquettes, deux colonnes et deux lignes. Notez que chaque étiquette aura la même taille et que les lignes sont développés pour utiliser tout l’espace disponible.

Dans l’exemple ci-dessus, les affichages sont ajoutés à la [ `Grid.Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.Children/) à l’aide de la collection le [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/) surcharge qui spécifie les arguments de gauche et supérieure. Lorsque vous utilisez la [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) lors de la surcharge qui spécifie à gauche, droite, haut et arguments du bas, gauche et supérieure arguments fera toujours référence à des cellules dans le [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), droite et les arguments bas peuvent apparaître pour faire référence à des cellules qui sont en dehors de la `Grid`. Il s’agit, car l’argument de droite doit toujours être supérieur à l’argument de gauche, et l’argument bas doit toujours être supérieur à l’argument top. L’exemple suivant montre le code équivalent à la fois `Add` surcharges :

```csharp
// left, top
grid.Children.Add(topLeft, 0, 0);
grid.Children.Add(topRight, 1, 0);
grid.Children.Add(bottomLeft, 0, 1);
grid.Children.Add(bottomRight, 1, 1);

// left, right, top, bottom
grid.Children.Add(topLeft, 0, 1, 0, 1);
grid.Children.Add(topRight, 1, 2, 0, 1);
grid.Children.Add(bottomLeft, 0, 1, 1, 2);
grid.Children.Add(bottomRight, 1, 2, 1, 2);
```

### <a name="spacing"></a>Espacement

`Grid` possède des propriétés pour contrôler l’espacement entre les lignes et colonnes.  Les propriétés suivantes sont disponibles pour la personnalisation de la `Grid`:

- **Espacement** &ndash; la quantité d’espace entre les colonnes.
- **RowSpacing** &ndash; la quantité d’espace entre les lignes.

Le code XAML suivant spécifie un `Grid` avec deux colonnes, une ligne et 5 px de l’espacement entre les colonnes :

```xaml
<Grid ColumnSpacing="5">
  <Grid.ColumnDefinitions>
    <ColumnDefinitions Width="*" />
    <ColumnDefinitions Width="*" />
  </Grid.ColumnDefinitions>
</Grid>
```

En C# :

```csharp
var grid = new Grid { ColumnSpacing = 5 };
grid.ColumnDefnitions.Add(new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star)});
grid.ColumnDefnitions.Add(new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star)});
```

### <a name="spans"></a>Étendues

Souvent, lorsque vous travaillez avec une grille, il existe un élément qui doit occuper plusieurs lignes ou colonnes. Considérez une application de calculatrice simple :

![](grid-images/calculator.png "Application de Calulator")

Notez que le bouton 0 s’étend sur deux colonnes, tout comme sur les calculatrices intégrées pour chaque plateforme. Pour cela, à l’aide de la `ColumnSpan` propriété qui spécifie le nombre de colonnes un élément doit occuper. Le code XAML de ce bouton :

```xaml
<Button Text = "0" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" />
```

Et en c# :

```csharp
Button zeroButton = new Button { Text = "0" };
controlGrid.Children.Add (zeroButton, 0, 4);
Grid.SetColumnSpan (zeroButton, 2);
```

Notez que dans le code, les méthodes statiques de la `Grid` classe permettent d’effectuer des modifications positionnement, y compris les modifications `ColumnSpan` et `RowSpan`. Notez que, contrairement à d’autres propriétés qui peuvent être définies à tout moment, les propriétés définies à l’aide de méthodes statiques doivent déjà être également dans la grille avant leur modification.

Le code XAML complet pour l’application Calculatrice ci-dessus est la suivante :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.CalculatorGridXAML"
Title = "Calculator - XAML"
BackgroundColor="#404040">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="plainButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#eee"/>
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
            <Style x:Key="darkerButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#ddd"/>
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
            <Style x:Key="orangeButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#E8AD00"/>
                <Setter Property="TextColor" Value="White" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <Grid x:Name="controlGrid" RowSpacing="1" ColumnSpacing="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="150" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
            <Label Text="0" Grid.Row="0" HorizontalTextAlignment="End" VerticalTextAlignment="End" TextColor="White"
        FontSize="60" Grid.ColumnSpan="4" />
            <Button Text = "C" Grid.Row="1" Grid.Column="0"
        Style="{StaticResource darkerButton}" />
            <Button Text = "+/-" Grid.Row="1" Grid.Column="1"
        Style="{StaticResource darkerButton}" />
            <Button Text = "%" Grid.Row="1" Grid.Column="2"
        Style="{StaticResource darkerButton}" />
            <Button Text = "div" Grid.Row="1" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "7" Grid.Row="2" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "8" Grid.Row="2" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "9" Grid.Row="2" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "X" Grid.Row="2" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "4" Grid.Row="3" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "5" Grid.Row="3" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "6" Grid.Row="3" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "-" Grid.Row="3" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "1" Grid.Row="4" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "2" Grid.Row="4" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "3" Grid.Row="4" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "+" Grid.Row="4" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "0" Grid.ColumnSpan="2"
        Grid.Row="5" Grid.Column="0" Style="{StaticResource plainButton}" />
            <Button Text = "." Grid.Row="5" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "=" Grid.Row="5" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

Notez que les l’étiquette en haut de la grille et le bouton zéro sont occuping plus d’une colonne. Bien qu’une disposition semblable peut être obtenue à l’aide de grilles imbriquées, le `ColumnSpan`  &  `RowSpan` approche est plus simple.

L’implémentation c# :

```csharp
public CalculatorGridCode ()
{
  Title = "Calculator - C#";
  BackgroundColor = Color.FromHex ("#404040");

  var plainButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#eee") },
      new Setter { Property = Button.TextColorProperty, Value = Color.Black },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };
  var darkerButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#ddd") },
      new Setter { Property = Button.TextColorProperty, Value = Color.Black },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };
  var orangeButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#E8AD00") },
      new Setter { Property = Button.TextColorProperty, Value = Color.White },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };

  var controlGrid = new Grid { RowSpacing = 1, ColumnSpacing = 1 };
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (150) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });

  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });

  var label = new Label {
    Text = "0",
    HorizontalTextAlignment = TextAlignment.End,
    VerticalTextAlignment = TextAlignment.End,
    TextColor = Color.White,
    FontSize = 60
  };
  controlGrid.Children.Add (label, 0, 0);

  Grid.SetColumnSpan (label, 4);

  controlGrid.Children.Add (new Button { Text = "C", Style = darkerButton }, 0, 1);
  controlGrid.Children.Add (new Button { Text = "+/-", Style = darkerButton }, 1, 1);
  controlGrid.Children.Add (new Button { Text = "%", Style = darkerButton }, 2, 1);
  controlGrid.Children.Add (new Button { Text = "div", Style = orangeButton }, 3, 1);
  controlGrid.Children.Add (new Button { Text = "7", Style = plainButton }, 0, 2);
  controlGrid.Children.Add (new Button { Text = "8", Style = plainButton }, 1, 2);
  controlGrid.Children.Add (new Button { Text = "9", Style = plainButton }, 2, 2);
  controlGrid.Children.Add (new Button { Text = "X", Style = orangeButton }, 3, 2);
  controlGrid.Children.Add (new Button { Text = "4", Style = plainButton }, 0, 3);
  controlGrid.Children.Add (new Button { Text = "5", Style = plainButton }, 1, 3);
  controlGrid.Children.Add (new Button { Text = "6", Style = plainButton }, 2, 3);
  controlGrid.Children.Add (new Button { Text = "-", Style = orangeButton }, 3, 3);
  controlGrid.Children.Add (new Button { Text = "1", Style = plainButton }, 0, 4);
  controlGrid.Children.Add (new Button { Text = "2", Style = plainButton }, 1, 4);
  controlGrid.Children.Add (new Button { Text = "3", Style = plainButton }, 2, 4);
  controlGrid.Children.Add (new Button { Text = "+", Style = orangeButton }, 3, 4);
  controlGrid.Children.Add (new Button { Text = ".", Style = plainButton }, 2, 5);
  controlGrid.Children.Add (new Button { Text = "=", Style = orangeButton }, 3, 5);

  var zeroButton = new Button { Text = "0", Style = plainButton };
  controlGrid.Children.Add (zeroButton, 0, 5);
  Grid.SetColumnSpan (zeroButton, 2);

  Content = controlGrid;
}
```


## <a name="related-links"></a>Liens associés

- [Création d’applications mobiles avec Xamarin.Forms, chapitre 17](https://developer.xamarin.com/r/xamarin-forms/book/chapter17.pdf)
- [Grid](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)
- [Disposition (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Exemple BusinessTumble (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
