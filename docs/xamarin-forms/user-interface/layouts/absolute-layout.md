---
title: DispositionAbsolue
description: DispositionAbsolue permet de créer des interfaces utilisateur de pixel parfaite.
ms.prod: xamarin
ms.assetid: 01A5CCE0-AD45-4806-84FD-72C007005B38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: 506a9a4916cf2cf9105d59f56648e339d664a3d2
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848385"
---
# <a name="absolutelayout"></a>DispositionAbsolue

[`AbsoluteLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) Positionne et d’une taille proportionnelle à son propre taille et la position ou en valeurs absolues des éléments enfants. Vues enfants peuvent être positionnées et taille à l’aide de valeurs proportionnelles ou statiques et proportionnel et des valeurs statiques peuvent être mélangés.

[![](absolute-layout-images/layouts-sml.png "Les dispositions de Xamarin.Forms")](absolute-layout-images/layouts.png#lightbox "Xamarin.Forms dispositions")

Cet article couvre :

- **[Objectif](#Purpose)**  &ndash; utilisations courantes pour `AbsoluteLayout`.
- **[L’utilisation de](#Usage)**  &ndash; comment utiliser `AbsoluteLayout` pour atteindre votre conception de votre choix.
  - **[Dispositions proportionnelles](#Proportional_Layouts)**  &ndash; comprendre comment proportionnels valeurs fonctionnent dans un `AbsoluteLayout`.
  - **[Spécification de valeurs](#Specifying_Values)**  &ndash; comprendre comment proportionnel et les valeurs absolues sont spécifiées.
  - **[Valeurs proportionnels](#Proportional_Values)**  &ndash; comprendre comment proportionnels valeurs de travail.
    - **[Valeurs absolues](#Absolute_Values)**  &ndash; comprendre comment fonctionnent les valeurs absolues.

<a name="Purpose" />

## <a name="purpose"></a>Objectif

En raison du modèle de positionnement de `AbsoluteLayout`, la mise en page rend relativement simple de placer des éléments pour qu’ils soient vidage avec n’importe quel côté de la disposition ou centré. Des tailles proportionnels et positions des éléments dans un `AbsoluteLayout` peuvent s’adapter automatiquement à n’importe quelle taille d’affichage. Pour les éléments où uniquement la position, mais pas la taille doit mettre à l’échelle, les valeurs absolues et proportionnelles peuvent être mélangés.

`AbsoluteLayout` peut être utilisé en tout lieu éléments doivent être positionnés dans une vue et s’avère particulièrement utile lors de l’alignement des éléments à bords.

<a name="Usage" />

## <a name="usage"></a>Utilisation

<a name="Proportional_Layouts" />

### <a name="proportional-layouts"></a>Dispositions proportionnelles

`AbsoluteLayout` possède un modèle d’ancrage unique par laquelle le point d’ancrage de l’élément est positionné par rapport à son élément que l’élément est positionné par rapport à la mise en page lorsque le positionnement proportionnelle est utilisé. Lorsque le positionnement absolu est utilisé, le point d’ancrage est à (0,0) dans la vue. Cela a deux conséquences importantes :

- Impossible de positionner les éléments hors de l’écran à l’aide de valeurs proportionnels.
- Éléments peuvent être positionnées de manière fiable sur n’importe quel côté de la mise en page ou dans le centre, quelle que soit la taille de la mise en page ou du périphérique.

`AbsoluteLayout`, comme `RelativeLayout`, est en mesure de placer des éléments de sorte qu’elles se chevauchent.

Notez dans la capture d’écran suivante, le point d’ancrage de la zone est un point blanc. Notez la relation entre le point d’ancrage et la zone en parcourant la disposition :

![](absolute-layout-images/anchor-start.png "Point d’ancrage au début")
![](absolute-layout-images/anchor-center.png "ancre au centre")
![](absolute-layout-images/anchor-end.png "ancre à fin")

<a name="Specifying_Values" />

### <a name="specifying-values"></a>Spécification de valeurs

Vues dans une `AbsoluteLayout` sont positionnées à l’aide de quatre valeurs :

- **X** &ndash; la position (horizontale) de x du point d’ancrage de la vue
- **Y** &ndash; la position y (verticale) d’ancrage de la vue
- **Largeur** &ndash; la largeur de la vue
- **Hauteur** &ndash; la hauteur de la vue

Chacune de ces valeurs peut être défini comme un [proportionnel](#Proportional_Values) valeur ou une [absolu](#Absolute_Values) valeur.

Les valeurs sont spécifiées comme une combinaison de limites et d’un indicateur. `LayoutBounds` est un [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/) composé de quatre valeurs : `x`, `y`, `width`, `height`.

### <a name="absolutelayoutflags"></a>AbsoluteLayoutFlags

[`AbsoluteLayoutFlags`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayoutFlags/) Spécifie la façon dont les valeurs seront interprétées et comprend les options prédéfinies suivantes :

- **Aucun** &ndash; interprète toutes les valeurs en tant qu’absolue. Ceci est la valeur par défaut si aucun indicateur de mise en page n’est spécifiés.
- **Tous les** &ndash; interprète toutes les valeurs en tant que proportionnel.
- **WidthProportional** &ndash; interprète la `Width` valeur proportionnelle, ainsi que toutes les autres valeurs en tant qu’absolue.
- **HeightProportional** &ndash; interprète uniquement la valeur de hauteur comme proportionnel avec toutes les valeurs absolues.
- **XProportional** &ndash; interprète la `X` valeur proportionnelle, tout en traitant tous les autres valeurs en tant qu’absolue.
- **YProportional** &ndash; interprète la `Y` valeur proportionnelle, tout en traitant tous les autres valeurs en tant qu’absolue.
- **PositionProportional** &ndash; interprète le `X` et `Y` valeurs sous la forme proportionnelle, tandis que les valeurs de taille sont interprétés comme absolue.
- **SizeProportional** &ndash; interprète le `Width` et `Height` valeurs sous la forme proportionnelle alors que les valeurs de position sont absolus.

En XAML, les limites et les indicateurs sont définies en tant que partie de la définition de vues au sein de la mise en page, à l’aide de la `AbsoluteLayout.LayoutBounds` propriété. Limites sont définies sous forme de liste séparée par des virgules des valeurs, `X`, `Y`, `Width`, et `Height`, dans cet ordre. Les indicateurs sont également spécifiées dans la déclaration de vues dans la disposition à l’aide de la `AbsoluteLayout.LayoutFlags` propriété. Notez que les indicateurs peuvent être combinées en XAML à l’aide d’une liste séparée par des virgules. Prenons l'exemple suivant :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.AbsoluteLayoutExploration"
Title="Absolute Layout Exploration">
    <ContentPage.Content>
        <AbsoluteLayout>
            <Label Text="I'm centered on iPhone 4 but no other device"
                AbsoluteLayout.LayoutBounds="115,150,100,100" LineBreakMode="WordWrap"  />
            <Label Text="I'm bottom center on every device."
                AbsoluteLayout.LayoutBounds=".5,1,.5,.1" AbsoluteLayout.LayoutFlags="All"
                LineBreakMode="WordWrap"  />
            <BoxView Color="Olive"  AbsoluteLayout.LayoutBounds="1,.5, 25, 100"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
            <BoxView Color="Red" AbsoluteLayout.LayoutBounds="0,.5,25,100"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
            <BoxView Color="Blue" AbsoluteLayout.LayoutBounds=".5,0,100,25"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

![](absolute-layout-images/exploration.png "Exemples de DispositionAbsolue")

Notez les points suivants :

- L’étiquette dans le centre est positionnée à l’aide des valeurs de taille et la position absolues. Par conséquent, il apparaît centré sur 4 s les iPhone et inférieure, mais pas centré sur les appareils plus grandes.
- Le texte en bas de la disposition est positionné à l’aide des valeurs de taille et position proportionnels. Il apparaît toujours au centre en bas de la mise en page, mais sa taille augmentera avec une taille de page supérieure.
- Trois couleurs `BoxView`s sont positionnés sur les bords supérieur, gauche et droit de l’écran à l’aide de sa position et taille absolue.

La commande suivante permet d’obtenir la même disposition en c# :

```csharp
public class AbsoluteLayoutExplorationCode : ContentPage
{
    public AbsoluteLayoutExplorationCode ()
    {
        Title = "Absolute Layout Exploration - Code";
        var layout = new AbsoluteLayout();

        var centerLabel = new Label {
        Text = "I'm centered on iPhone 4 but no other device.",
        LineBreakMode = LineBreakMode.WordWrap};

        AbsoluteLayout.SetLayoutBounds (centerLabel, new Rectangle (115, 159, 100, 100));
        // No need to set layout flags, absolute positioning is the default

        var bottomLabel = new Label { Text = "I'm bottom center on every device.", LineBreakMode = LineBreakMode.WordWrap };
        AbsoluteLayout.SetLayoutBounds (bottomLabel, new Rectangle (.5, 1, .5, .1));
        AbsoluteLayout.SetLayoutFlags (bottomLabel, AbsoluteLayoutFlags.All);

        var rightBox = new BoxView{ Color = Color.Olive };
        AbsoluteLayout.SetLayoutBounds (rightBox, new Rectangle (1, .5, 25, 100));
        AbsoluteLayout.SetLayoutFlags (rightBox, AbsoluteLayoutFlags.PositionProportional);

        var leftBox = new BoxView{ Color = Color.Red };
        AbsoluteLayout.SetLayoutBounds (leftBox, new Rectangle (0, .5, 25, 100));
        AbsoluteLayout.SetLayoutFlags (leftBox, AbsoluteLayoutFlags.PositionProportional);

        var topBox = new BoxView{ Color = Color.Blue };
        AbsoluteLayout.SetLayoutBounds (topBox, new Rectangle (.5, 0, 100, 25));
        AbsoluteLayout.SetLayoutFlags (topBox, AbsoluteLayoutFlags.PositionProportional);

        layout.Children.Add (bottomLabel);
        layout.Children.Add (centerLabel);
        layout.Children.Add (rightBox);
        layout.Children.Add (leftBox);
        layout.Children.Add (topBox);

        Content = layout;
    }
}
```
<a name="Proportional_Values" />

### <a name="proportional-values"></a>Valeurs proportionnels

Valeurs proportionnels définissent une relation entre une mise en page et une vue. Cette relation définit la position ou valeur d’échelle d’une vue enfant sous forme de proportion de la valeur correspondante de la disposition du parent. Ces valeurs sont exprimées en tant que `double`s avec des valeurs comprises entre 0 et 1.

Valeurs proportionnels sont utilisées à la position et des affichages de taille de la mise en page. Par conséquent, lors de la largeur d’une vue est définie comme une proportion, la valeur de largeur résultant est la proportion multipliée par le `AbsoluteLayout`de largeur. Par exemple, avec un `AbsoluteLayout` de largeur `500` et un affichage défini pour une largeur proportionnelle de.5, la largeur de la vue rendue sera 250 (500 x.5.

Pour utiliser des valeurs proportionnelles, définissez `LayoutBounds` à l’aide de (x, y) proportions et tailles proportionnels, puis définissez `LayoutFlags` à `All`.

En XAML :

```xaml
<Label Text="I'm bottom center on every device."
  AbsoluteLayout.LayoutBounds=".5,1,.5,.1" AbsoluteLayout.LayoutFlags="All"  />
```

En C# :

```csharp
var label = new Label {Text = "I'm bottom center on every device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(.5,1,.5,.1));
AbsoluteLayout.SetLayoutFlags(label, AbsoluteLayoutFlags.All);
```

<a name="Absolute_Values" />

### <a name="absolute-values"></a>Valeurs absolues

Valeurs absolues définir explicitement la position de vues dans la disposition. Contrairement aux valeurs proportionnels, les valeurs absolues sont capables de positionnement et le redimensionnement d’une vue qui ne tient pas dans les limites de la mise en page.

À l’aide des valeurs absolues de positionnement peut s’avérer dangereux si la taille de la disposition n’est pas connue. Lors de l’utilisation de positions absolues, un élément dans le centre de l’écran à une seule taille pourrait être décalée à n’importe quelle autre taille. Il est important de tester votre application sur les différentes tailles d’écran de vos appareils pris en charge.

Pour utiliser les valeurs de disposition absolue, définissez `LayoutBounds` à l’aide de (x, y) coordonnées et tailles, puis définissez `LayoutFlags` à `None`.

En XAML :

```xaml
<Label Text="I'm centered on iPhone 4 but no other device."
  AbsoluteLayout.LayoutBounds="115,150,100,100"  />
```

En C# :

```csharp
var label = new Label {Text = "I'm centered on iPhone 4 but no other device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(115,150,100,100));
```

## <a name="exploring-a-complex-layout"></a>Exploration d’une disposition complexe

Forces et faiblesses pour créer des dispositions particuliers ont chacun des dispositions. Tout au long de cette série d’articles de la mise en page, un exemple d’application a été créé avec la même disposition implémentée à l’aide de trois dispositions différentes.

Considérez le code XAML suivant :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.AbsoluteLayoutPage"
Title="AbsoluteLayout">
    <ContentPage.ToolbarItems>
        <ToolbarItem Text="Save" />
    </ContentPage.ToolbarItems>
    <ContentPage.Content>
        <ScrollView>
            <AbsoluteLayout BackgroundColor="Maroon">
                <BoxView BackgroundColor="Gray" AbsoluteLayout.LayoutBounds="0
                    0,1,100" AbsoluteLayout.LayoutFlags="XProportional,YProportional,WidthProportional" />
                <Button BackgroundColor="Maroon"
                    AbsoluteLayout.LayoutBounds=".5,55,70,70" AbsoluteLayout.LayoutFlags="XProportional" BorderRadius="35" />
                <Button BackgroundColor="Red" AbsoluteLayout.LayoutBounds=".5
                    60,60,60" AbsoluteLayout.LayoutFlags="XProportional" BorderRadius="30" />
                <Label Text="User Name" FontAttributes="Bold" FontSize="26"
                    TextColor="Black" HorizontalTextAlignment="Center" AbsoluteLayout.LayoutBounds=".5,140,1,40" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                <Entry Text="Bio + Hashtags" TextColor="White"
                    BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds=".5,180,1,40" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                <AbsoluteLayout BackgroundColor="White"
                        AbsoluteLayout.LayoutBounds="0, 220, 1, 50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional">
                    <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,0,.5,1" AbsoluteLayout.LayoutFlags="WidthProportional,HeightProportional">
                        <Button BackgroundColor="Black" BorderRadius="20"
                            AbsoluteLayout.LayoutBounds="5,.5,40,40" AbsoluteLayout.LayoutFlags="YProportional" />
                        <Label Text="Accent Color" TextColor="Black"
                            AbsoluteLayout.LayoutBounds="50,.55,1,25" AbsoluteLayout.LayoutFlags="YProportional,WidthProportional" />
                    </AbsoluteLayout>
                    <AbsoluteLayout AbsoluteLayout.LayoutBounds="1,0,.5,1" AbsoluteLayout.LayoutFlags="WidthProportional,HeightProportional,XProportional">
                        <Button BackgroundColor="Maroon" BorderRadius="20"
                            AbsoluteLayout.LayoutBounds="5,.5,40,40" AbsoluteLayout.LayoutFlags="YProportional" />
                        <Label Text="Primary Color" TextColor="Black"
                            AbsoluteLayout.LayoutBounds="50,.55,1,25" AbsoluteLayout.LayoutFlags="YProportional,WidthProportional" />
                    </AbsoluteLayout>
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,270,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Age:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="35" TextColor="White" BackgroundColor="Maroon"
                        AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,320,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Interests:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="Xamarin.Forms" TextColor="White"
                        BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,370,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Ask me about:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="Xamarin, C#, .NET, Mono" TextColor="White"
                        BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
            </AbsoluteLayout>
        </ScrollView>
    </ContentPage.Content>
</ContentPage>
```

Le code ci-dessus génère le schéma suivant :

![](absolute-layout-images/abs.png "DispositionAbsolue complexe")

Notez que `AbsoluteLayout`s sont imbriquées, car dans certains cas d’imbrication des dispositions peut être plus facile à présenter tous les éléments dans la même disposition.

## <a name="related-links"></a>Liens associés

- [Création d’applications mobiles avec Xamarin.Forms, chapitre 14](https://developer.xamarin.com/r/xamarin-forms/book/chapter14.pdf)
- [AbsoluteLayout](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/)
- [Disposition (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Exemple BusinessTumble (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
