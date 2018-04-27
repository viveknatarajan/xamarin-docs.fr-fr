---
title: StackLayout
description: StackLayout permet de présenter des collections de vues sur une dimension.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: c27f94302037e4e19c9d72131e7137c8a4004d5c
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2018
---
# <a name="stacklayout"></a>StackLayout

`StackLayout` organise les vues dans une ligne de dimension (« pile »), horizontalement ou verticalement. Vues dans un `StackLayout` peut être dimensionnée en fonction de l’espace dans la disposition à l’aide des options de disposition. Positionnement est déterminé par l’ordre des vues ont été ajoutés à la disposition et les options de disposition des vues.

[![](stack-layout-images/layouts-sml.png "Les dispositions de Xamarin.Forms")](stack-layout-images/layouts.png#lightbox "Xamarin.Forms dispositions")

## <a name="purpose"></a>Objectif

`StackLayout` est moins complexe que les autres vues. Interfaces de linéaires simples peuvent être créées en ajoutant simplement des vues à un `StackLayout`et créés en imbriquant des interfaces plus complexes.

## <a name="usage--behavior"></a>L’utilisation et du comportement

### <a name="spacing"></a>Espacement

Par défaut, `StackLayout` ajoute une marge 6px entre les vues. Cela peut être contrôlé ou défini pour aucune marge en définissant le `Spacing` propriété sur StackLayout. Le texte suivant montre comment définir l’espacement et l’effet des options d’espacement différent :

En XAML :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.StackLayoutDemo"
Title="StackLayout Demo">
  <ContentPage.Content>
    <StackLayout Spacing="10" x:Name="layout">
      <Button Text="StackLayout" VerticalOptions="Start"
        HorizontalOptions="FillAndExpand" />
      <BoxView Color="Yellow" VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView Color="Green" VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView HeightRequest="75" Color="Blue" VerticalOptions="End"
        HorizontalOptions="FillAndExpand" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

En C# :

```csharp
public class StackLayoutCode : ContentPage
{
  public StackLayoutCode ()
  {
    var layout = new StackLayout ();
    var button = new Button { Text = "StackLayout", VerticalOptions = LayoutOptions.Start,
     HorizontalOptions = LayoutOptions.FillAndExpand };
    var yellowBox = new BoxView { Color = Color.Yellow, VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var greenBox = new BoxView { Color = Color.Green, VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var blueBox = new BoxView { Color = Color.Blue, VerticalOptions = LayoutOptions.FillAndExpand,
      HorizontalOptions = LayoutOptions.FillAndExpand, HeightRequest = 75 };

    layout.Children.Add(button);
    layout.Children.Add(yellowBox);
    layout.Children.Add(greenBox);
    layout.Children.Add(blueBox);
    layout.Spacing = 10;
    Content = layout;
  }
}
```

Espacement = 0 :

![](stack-layout-images/spacing-zero.png "StackLayout avec espacement = 0")

Espacement de dix :

![](stack-layout-images/spacing-ten.png "StackLayout avec espacement = 10")

### <a name="sizing"></a>Dimensionnement

La taille d’une vue dans un StackLayout varie selon les demandes de hauteur et la largeur et les options de disposition. `StackLayout` impose le remplissage. Les éléments suivants `LayoutOption`s entraîne des vues occuper la quantité d’espace est disponible à partir de la mise en page :

- **CenterAndExpand** &ndash; centre la vue de la mise en page et se développe pour autant lui donnera la disposition de l’espace.
- **EndAndExpand** &ndash; place la vue à la fin de la mise en page (en bas ou une limite la plus à droite) et se développe pour autant lui donnera la disposition de l’espace.
- **FillAndExpand** &ndash; place la vue afin qu’il ne dispose d’aucun remplissage et occupe autant d’espace que lui donnera la disposition.
- **StartAndExpand** &ndash; place la vue au début de la mise en page et occupe autant d’espace que donne le parent.

Pour plus d’informations, consultez [Expansion](~/xamarin-forms/user-interface/layouts/layout-options.md#expansion).

### <a name="positioning"></a>Positionnement

Vues dans un StackLayout peut être positionnées et dimensionnées en utilisant `LayoutOptions`. Chaque vue peut être donné `VerticalOptions` et `HorizontalOptions`, définissant la manière dont les vues se positionner par rapport à la disposition. Les prédéfinies suivantes `LayoutOptions` sont disponibles :

- **Centre** &ndash; centre la vue dans la disposition.
- **Fin** &ndash; place la vue à la fin de la mise en page (en bas ou une limite la plus à droite).
- **Remplir** &ndash; place la vue de sorte qu’il ne dispose d’aucun remplissage.
- **Démarrer** &ndash; place la vue au début de la mise en page.

Le code suivant illustre la définition des options de mise en page :

En XAML :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.StackLayoutDemo"
Title="StackLayout Demo">
  <ContentPage.Content>
    <StackLayout x:Name="layout">
      <Button VerticalOptions="Start"
        HorizontalOptions="FillAndExpand" />
      <BoxView VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView HeightRequest="75" VerticalOptions="End"
        HorizontalOptions="FillAndExpand" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

En C# :

```csharp
public class StackLayoutCode : ContentPage
{
  public StackLayoutCode ()
  {
    var layout = new StackLayout ();
    var button = new Button { VerticalOptions = LayoutOptions.Start,
     HorizontalOptions = LayoutOptions.FillAndExpand };
    var oneBox = new BoxView { VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var twoBox = new BoxView {  VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var threeBox = new BoxView {  VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };

    layout.Children.Add(button);
    layout.Children.Add(oneBox);
    layout.Children.Add(twoBox);
    layout.Children.Add(threeBox);
    Content = layout;
  }
}
```

Pour plus d’informations, consultez [alignement](~/xamarin-forms/user-interface/layouts/layout-options.md#alignment).

## <a name="exploring-a-complex-layout"></a>Exploration d’une disposition complexe

Forces et faiblesses pour créer des dispositions particuliers ont chacun des dispositions. Tout au long de cette série d’articles de la mise en page, un exemple d’application a été créé avec la même disposition implémentée à l’aide de trois dispositions différentes.

Considérez le code XAML suivant :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.StackLayoutPage"
BackgroundColor="Maroon"
Title="StackLayouts">
    <ContentPage.Content>
    <ScrollView>
      <StackLayout Spacing="0" Padding="0" BackgroundColor="Maroon">
        <BoxView HorizontalOptions="FillAndExpand" HeightRequest="100"
          VerticalOptions="Start" Color="Gray" />
        <Button BorderRadius="30" HeightRequest="60" WidthRequest="60"
          BackgroundColor="Red" HorizontalOptions="Center" VerticalOptions="Start" />
        <StackLayout HeightRequest="100" VerticalOptions="Start" HorizontalOptions="FillAndExpand" Spacing="20" BackgroundColor="Maroon">
          <Label Text="User Name" FontSize="28" HorizontalOptions="Center"
            VerticalOptions="Center" FontAttributes="Bold" />
          <Entry Text="Bio + Hashtags" TextColor="White"
            BackgroundColor="Maroon" HorizontalOptions="FillAndExpand" VerticalOptions="CenterAndExpand" />
        </StackLayout>
        <StackLayout Orientation="Horizontal" HeightRequest="50" BackgroundColor="White" Padding="5">
          <StackLayout Spacing="0" BackgroundColor="White" Orientation="Horizontal" HorizontalOptions="Start">
            <BoxView BackgroundColor="Black" WidthRequest="40" HeightRequest="40"  HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
            <Label FontSize="14" TextColor="Black" Text="Accent Color" HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
          </StackLayout>
          <StackLayout Spacing="0" BackgroundColor="White" Orientation="Horizontal" HorizontalOptions="EndAndExpand">
            <BoxView BackgroundColor="Maroon" WidthRequest="40" HeightRequest="40" HorizontalOptions="Start" VerticalOptions="Center" />
            <Label FontSize="14" TextColor="Black" Text="Primary Color" HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
          </StackLayout>
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Age:" TextColor="White" HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100" />
          <Entry HorizontalOptions="FillAndExpand" Text="35" TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Interests:" TextColor="White"
          HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100" />
          <Entry HorizontalOptions="FillAndExpand" Text="Xamarin.Forms" TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Ask me about:" TextColor="White"
          HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100"/>
          <Entry HorizontalOptions="FillAndExpand" Text="Xamarin, C#, .NET, Mono..." TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
      </StackLayout>
    </ScrollView>
    </ContentPage.Content>
</ContentPage>

```

Le code ci-dessus génère le schéma suivant :

![](stack-layout-images/stack.png "StackLayout complexe")

Notez que `StackLayouts`s sont imbriquées, car dans certains cas d’imbrication des dispositions peut être plus facile à présenter tous les éléments dans la même disposition. Notez également que, étant donné que `StackLayout` ne prend en charge les éléments qui se chevauchent, la page n’ont des fonctionnalités utiles de disposition trouvés dans les pages pour les autres mises en page.



## <a name="related-links"></a>Liens associés

- [LayoutOptions](~/xamarin-forms/user-interface/layouts/layout-options.md)
- [Disposition (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Exemple BusinessTumble (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
