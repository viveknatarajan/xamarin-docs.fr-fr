---
title: RelativeLayout
description: RelativeLayout permet de créer des interfaces utilisateur que n’importe quelle taille d’écran à l’échelle.
ms.prod: xamarin
ms.assetid: 2530BCB8-01B8-4C4F-BF14-CA53659F1B5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: 2edd7a982d0307475933a234e7e1fbb8dd8d6a34
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="relativelayout"></a>RelativeLayout

`RelativeLayout` est utilisé pour la position et les vues de taille par rapport aux propriétés des vues frères ou de mise en page. Contrairement aux `AbsoluteLayout`, `RelativeLayout` n’a pas le concept de l’ancre de déplacement et ne dispose pas des fonctions de positionnement des éléments par rapport à bas ou les bords droit de la disposition. `RelativeLayout` prend en charge de positionnement des éléments en dehors de ses propres limites.

[![](relative-layout-images/layouts-sml.png "Les dispositions de Xamarin.Forms")](relative-layout-images/layouts.png#lightbox "Xamarin.Forms dispositions")

## <a name="purpose"></a>Objectif

`RelativeLayout` peut être utilisé pour positionner à l’écran par rapport à la disposition générale de deux vues.

![](relative-layout-images/flag.png "Exploration du RelativeLayout")

## <a name="usage"></a>Utilisation

### <a name="understanding-constraints"></a>Présentation des contraintes

Positionnement et le redimensionnement d’une vue dans un `RelativeLayout` est effectuée avec les contraintes. Une expression de contrainte peut inclure les informations suivantes :

- **Type** &ndash; si la contrainte est par rapport au parent ou à une autre vue.
- **Propriété** &ndash; la propriété à utiliser comme base pour la contrainte.
- **Facteur** &ndash; le facteur à appliquer à la valeur de propriété.
- **Constante** &ndash; la valeur à utiliser en tant que décalage de la valeur.
- **ElementName** &ndash; le nom de la vue de la contrainte est associée.

En XAML, les contraintes sont exprimées sous forme `ConstraintExpression`s. Prenons l'exemple suivant :

```xaml
<BoxView Color="Green" WidthRequest="50" HeightRequest="50"
    RelativeLayout.XConstraint =
      "{ConstraintExpression Type=RelativeToParent,
                             Property=Width,
                             Factor=0.5,
                             Constant=-100}"
    RelativeLayout.YConstraint =
      "{ConstraintExpression Type=RelativeToParent,
                             Property=Height,
                             Factor=0.5,
                             Constant=-100}" />
```

En c#, les contraintes sont exprimées un peu différemment, à l’aide de fonctions au lieu des expressions sur la vue. Les contraintes sont spécifiés en tant qu’arguments à la mise en page `Add` méthode :

```csharp
layout.Children.Add(box, Constraint.RelativeToParent((parent) =>
    {
      return (.5 * parent.Width) - 100;
    }),
    Constraint.RelativeToParent((parent) =>
    {
        return (.5 * parent.Height) - 100;
    }),
    Constraint.Constant(50), Constraint.Constant(50));
```

Notez les aspects suivants de la disposition ci-dessus :

- Le `x` et `y` les contraintes sont spécifiées avec leurs contraintes.
- En c#, les contraintes relatives sont définies en tant que fonctions. Comme les concepts `Factor` ne sont pas, mais peuvent être implémentées manuellement.
- La zone de `x` coordonnée est définie en tant que la moitié de la largeur de la page parente, -100.
- La zone de `y` coordonnée est définie en tant que la moitié de la hauteur de la page parente, -100.

> [!NOTE]
> En raison du mode contraintes sont définies, il est possible d’effectuer des mises en page plus complexes dans c# que peut être spécifié avec XAML.

Les deux exemples ci-dessus définissent les contraintes en tant que `RelativeToParent` &ndash; , autrement dit, leurs valeurs sont par rapport à l’élément parent. Il est également possible de définir des contraintes par rapport à une autre vue. Cela permet pour les dispositions plus intuitives (pour le développeur) et que vous pouvez apporter plus clairement l’objectif de votre code de mise en page.

Envisagez une disposition où un élément doit être inférieure à une autre de 20 pixels. Si les deux éléments sont définis avec des valeurs constantes, la limite inférieure peut avoir son `Y` contrainte définie en tant que constante qui est de 20 pixels supérieure à la `Y` contrainte de l’élément supérieur. Cette approche se situe courte si l’élément supérieur est placé à l’aide d’une part, afin que la taille en pixels n’est pas connue. Dans ce cas, la restriction de l’élément en fonction de la position d’un autre élément est plus robuste :

```xaml
<RelativeLayout>
    <BoxView Color="Red" x:Name="redBox"
        RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent,
            Property=Height,Factor=.15,Constant=0}"
        RelativeLayout.WidthConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Width,Factor=1,Constant=0}"
        RelativeLayout.HeightConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Height,Factor=.8,Constant=0}" />
    <BoxView Color="Blue"
        RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToView,
            ElementName=redBox,Property=Y,Factor=1,Constant=20}"
        RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToView,
            ElementName=redBox,Property=X,Factor=1,Constant=20}"
        RelativeLayout.WidthConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Width,Factor=.5,Constant=0}"
        RelativeLayout.HeightConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Height,Factor=.5,Constant=0}" />
</RelativeLayout>
```

Pour accomplir la même disposition en c# :

```csharp
layout.Children.Add (redBox, Constraint.RelativeToParent ((parent) => {
        return parent.X;
    }), Constraint.RelativeToParent ((parent) => {
        return parent.Y * .15;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Width;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Height * .8;
    }));
layout.Children.Add (blueBox, Constraint.RelativeToView (redBox, (Parent, sibling) => {
        return sibling.X + 20;
    }), Constraint.RelativeToView (blueBox, (parent, sibling) => {
        return sibling.Y + 20;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Width * .5;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Height * .5;
    }));
```

Ce code produit la sortie suivante, avec la position de la zone bleue déterminée _relatif_ à la position de la zone rouge :

![](relative-layout-images/red-blue-box.png "RelativeLayout avec BoxViews rouge et bleu")

### <a name="sizing"></a>Dimensionnement

Vues disposés par `RelativeLayout` ont deux options pour spécifier leur taille :

- `HeightRequest & WidthRequest`
- `RelativeLayout.WidthConstraint` & `RelativeLayout.HeightConstraint`

`HeightRequest` et `WidthRequest` spécifier la destination de hauteur et largeur de la vue, mais peut être substituée par des dispositions en fonction des besoins. `WidthConstraint` et `HeightConstraint` prennent en charge la définition de la hauteur et la largeur en tant que valeur par rapport aux propriétés de la mise en page ou d’une autre vue, ou comme une valeur constante.

## <a name="exploring-a-complex-layout"></a>Exploration d’une disposition complexe
Forces et faiblesses pour créer des dispositions particuliers ont chacun des dispositions. Tout au long de cette série d’articles de la mise en page, un exemple d’application a été créé avec la même disposition implémentée à l’aide de trois dispositions différentes.

Considérez le code XAML suivant :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.RelativeLayoutPage"
BackgroundColor="Maroon"
Title="RelativeLayout">
    <ContentPage.Content>
    <ScrollView>
      <RelativeLayout>
        <BoxView Color="Gray" HeightRequest="100"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" />
        <Button BorderRadius="35" x:Name="imageCircleBack"
            BackgroundColor="Maroon" HeightRequest="70" WidthRequest="70" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent,Property=Width, Factor=.5, Constant = -35}" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Factor=0, Property=Y, Constant=70}" />
        <Button BorderRadius="30" BackgroundColor="Red" HeightRequest="60"
            WidthRequest="60" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToView, ElementName=imageCircleBack, Property=X, Factor=1,Constant=5}" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Factor=0, Property=Y, Constant=75}" />
        <Label Text="User Name" FontAttributes="Bold" FontSize="26"
            HorizontalTextAlignment="Center" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=140}" RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" />
        <Entry Text="Bio + Hashtags" TextColor="White" BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=180}" RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" />
        <RelativeLayout BackgroundColor="White" RelativeLayout.YConstraint="
            {ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=220}" HeightRequest="60" RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" >
            <BoxView BackgroundColor="Black" WidthRequest="50"
                HeightRequest="50" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=5}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=X, Factor=0, Constant=5}" />
            <BoxView BackgroundColor="Maroon" WidthRequest="50"
                HeightRequest="50" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=5}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.5, Constant=}" />
            <Label FontSize="14" TextColor="Black" Text="Accent Color"
                RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=20}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=X, Factor=0, Constant=60}" />
            <Label FontSize="14" TextColor="Black" Text="Primary Color"
                RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=20}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.5, Constant=55}" />
        </RelativeLayout>
        <RelativeLayout Padding="5,0,0,0">
          <Label FontSize="14" Text="Age:" TextColor="White"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=305}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0, Constant=10}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=.25,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
          <Entry Text="35" TextColor="White" BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=280}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.3, Constant=0}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=0.75,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
        </RelativeLayout>
        <RelativeLayout  Padding="5,0,0,0">
          <Label FontSize="14" Text="Interests:" TextColor="White"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=345}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0, Constant=10}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=.25,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
          <Entry Text="Xamarin.Forms" TextColor="White" BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=320}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.3, Constant=0}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=0.75,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
        </RelativeLayout>
        <RelativeLayout  Padding="5,0,0,0">
          <Label FontSize="14" Text="Ask me about:" TextColor="White"
            LineBreakMode="WordWrap"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=395}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0, Constant=10}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=.25,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
          <Entry Text="Xamarin, C#, .NET, Mono" TextColor="White"
            BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=370}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.3, Constant=0}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=0.75,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
        </RelativeLayout>
      </RelativeLayout>
    </ScrollView>
  </ContentPage.Content>
</ContentPage>
```

Le code ci-dessus génère le schéma suivant :

![](relative-layout-images/relative.png "RelativeLayout complexe")

Notez que, en raison d’une différence dans la façon dont les boutons sont rendus par Windows Phone, certaines des cercles d’ont été remplacés par boxviews dans la capture d’écran de Windows Phone.

Notez que `RelativeLayouts`s sont imbriquées, car dans certains cas d’imbrication des dispositions peut être plus facile à présenter tous les éléments dans la même disposition. Notez également que certains éléments sont `RelativeToView`, car qui permet de mise en page plus facile et intuitif lorsque les relations entre les vues de guident de positionnement.


## <a name="related-links"></a>Liens associés

- [Disposition (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Exemple BusinessTumble (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
