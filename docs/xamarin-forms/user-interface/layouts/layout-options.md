---
title: LayoutOptions
description: "Chaque vue Xamarin.Forms a les propriétés HorizontalOptions et VerticalOptions, de type LayoutOptions. Cet article explique l’effet de chaque valeur LayoutOptions sur l’alignement et l’expansion d’une vue."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7CAB5631-5153-4DEF-8AD7-C6011CE44307
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2017
ms.openlocfilehash: 978985c4e9803fad33760e4b40ab73d57f3ec420
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="layoutoptions"></a>LayoutOptions

_Chaque vue Xamarin.Forms a les propriétés HorizontalOptions et VerticalOptions, de type LayoutOptions. Cet article explique l’effet de chaque valeur LayoutOptions sur l’alignement et l’expansion d’une vue._

## <a name="overview"></a>Vue d'ensemble

Le [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) structure encapsule les préférences de mise en page deux :

- **Alignement** – la vue par défaut de l’alignement, qui détermine sa position et la taille de la disposition de son parent.
- **Expansion** : utilisé uniquement par un [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)et indique si la vue doit utiliser un espace supplémentaire, si elle est disponible.

Ces préférences peuvent être appliqués à un [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/), relatif à son parent, en définissant le [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) ou [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) propriété de la `View` à un des champs publics à partir de la [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) structure. Les champs publics sont les suivantes :

- [`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

Le `Start`, `Center`, `End`, et `Fill` champs sont utilisés pour définir l’alignement de la vue dans la mise en page parent :

- Pour l’alignement horizontal, [ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/) positions le [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) sur le côté gauche de la disposition de parent et pour l’alignement vertical, il positionne le `View` en haut de la disposition du parent.
- Pour l’alignement horizontal et vertical, [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/) horizontalement ou verticalement les centres de la [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/).
- Pour l’alignement horizontal, [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/) positions le [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) sur le côté droit de la disposition de parent et pour l’alignement vertical, il positionne le `View` en bas de la disposition du parent.
- Pour l’alignement horizontal, [ `Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/) garantit que le [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) remplit la largeur de la disposition de parent et pour l’alignement vertical, elle garantit que le `View` remplit le hauteur de la disposition du parent.

Le `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, et `FillAndExpand` valeurs sont utilisées pour définir les préférences d’alignement, et indique si la vue occupent davantage d’espace si elle est disponible dans le parent [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/).

> [!NOTE]
> La valeur par défaut d’une vue [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) et [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) propriétés est [ `LayoutOptions.Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/).

<a name="alignment" />

## <a name="alignment"></a>Alignement

Alignement détermine comment une vue est positionnée au sein de sa disposition parent lors de la mise en page parent contient l’espace inutilisé (autrement dit, la mise en page parent est supérieure à la taille cumulée de tous ses enfants).

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) prend uniquement en compte la `Start`, `Center`, `End`, et `Fill` [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) champs sur les vues enfants qui se trouvent dans la direction opposée à la `StackLayout` l’orientation. Par conséquent, des vues enfant au sein d’un orienté verticalement `StackLayout` peuvent définir leurs [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) à une des propriétés de la `Start`, `Center`, `End`, ou `Fill` champs. De même, des vues enfant au sein d’une orientation horizontale `StackLayout` peuvent définir leurs [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) à une des propriétés de la `Start`, `Center`, `End`, ou `Fill` champs.

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) ne respecte pas la `Start`, `Center`, `End`, et `Fill` [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) sur les vues enfants qui se trouvent dans le même sens que les champs le `StackLayout` orientation. Par conséquent, un orienté verticalement `StackLayout` ignore la `Start`, `Center`, `End`, ou `Fill` s’ils sont définis sur les champs du [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) propriétés des vues enfants. De même, un orienté horizontalement `StackLayout` ignore la `Start`, `Center`, `End`, ou `Fill` s’ils sont définis sur les champs du [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) propriétés des vues enfants.

> [!NOTE]
> [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/) généralement substitue la taille spécifiées à l’aide de demandes le [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) et [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) propriétés.

L’exemple de code XAML suivant montre un orienté verticalement [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) où chaque enfant [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) définit ses [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) propriété à un des champs d’alignement de la [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) structure :

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <Label Text="Start" BackgroundColor="Gray" HorizontalOptions="Start" />
  <Label Text="Center" BackgroundColor="Gray" HorizontalOptions="Center" />
  <Label Text="End" BackgroundColor="Gray" HorizontalOptions="End" />
  <Label Text="Fill" BackgroundColor="Gray" HorizontalOptions="Fill" />
</StackLayout>
```

Le code c# équivalent est indiqué ci-dessous :

```csharp
Content = new StackLayout
{
  Margin = new Thickness(0, 20, 0, 0),
  Children = {
    ...
    new Label { Text = "Start", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Start },
    new Label { Text = "Center", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Center },
    new Label { Text = "End", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.End },
    new Label { Text = "Fill", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Fill }
  }
};
```

Le code génère la disposition indiquée dans les captures d’écran suivants :

[![](layout-options-images/alignment.png "Options de mise en page d’alignement")](layout-options-images/alignment-large.png "Options de mise en page d’alignement")

<a name="expansion" />

## <a name="expansion"></a>Expansion

Contrôle l’expansion si une vue occupe davantage d’espace, le cas échéant, dans un [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/). Si le `StackLayout` contient l’espace inutilisé (autrement dit, le `StackLayout` est supérieure à la taille cumulée de tous ses enfants), l’espace inutilisé est également partagé par toutes les vues enfants qui demandent d’extension en définissant leurs [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)ou [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) propriétés à un [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) champ qui utilise le `AndExpand` suffixe. Notez que lorsque tout l’espace dans le `StackLayout` est utilisé, les options d’extension n’ont aucun effet.

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) pouvez développer uniquement les vues enfants dans la direction de son orientation. Par conséquent, un orienté verticalement `StackLayout` pouvez développer les vues enfants que définir leurs [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) à une des propriétés de la `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, ou `FillAndExpand` des champs, si le `StackLayout` contient l’espace inutilisé. De même, un orienté horizontalement `StackLayout` pouvez développer les vues enfants que définir leurs [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) à une des propriétés de la `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, ou `FillAndExpand` des champs, si le `StackLayout` contient l’espace inutilisé.

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) ne peut pas développer des vues enfants dans la direction opposée à son orientation. Par conséquent, sur orienté verticalement `StackLayout`, en affectant la [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) propriété sur une vue enfant à [ `StartAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/) a le même effet que la propriété [ `Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/).

> [!NOTE]
> Notez que l’activation d’extension ne change pas la taille d’une vue, sauf si elle utilise [ `LayoutOptions.FillAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/).

L’exemple de code XAML suivant montre un orienté verticalement [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) où chaque enfant [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) définit ses [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) propriété à un des champs d’extension de quatre à partir de la [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) structure :

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Start" BackgroundColor="Gray" VerticalOptions="StartAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Center" BackgroundColor="Gray" VerticalOptions="CenterAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="End" BackgroundColor="Gray" VerticalOptions="EndAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Fill" BackgroundColor="Gray" VerticalOptions="FillAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
</StackLayout>
```

Le code c# équivalent est indiqué ci-dessous :

```csharp
Content = new StackLayout
{
  Margin = new Thickness(0, 20, 0, 0),
  Children = {
    ...
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "StartAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.StartAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "CenterAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.CenterAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "EndAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.EndAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "FillAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.FillAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 }
  }
};
```

Le code génère la disposition indiquée dans les captures d’écran suivants :

[![](layout-options-images/expansion.png "Options de mise en page d’extension")](layout-options-images/expansion-large.png "Options de mise en page d’extension")

Chaque [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) occupe la même quantité d’espace dans le [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/). Toutefois, uniquement le dernier `Label`, qui fait passer son [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) propriété [ `FillAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/) a une taille différente. En outre, chaque `Label` est séparé par un petit [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/), ce qui permet de l’espace le `Label` occupe pour être affichés facilement.

## <a name="summary"></a>Récapitulatif

Cet article a expliqué l’effet que chaque [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) a de valeur de la structure sur l’alignement et l’expansion d’une vue, par rapport à son parent. Le `Start`, `Center`, `End`, et `Fill` champs sont utilisés pour définir l’alignement de la vue dans la disposition de parent et le `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, et `FillAndExpand` champs servent à définir la préférence d’alignement et pour déterminer si la vue occupe davantage d’espace, le cas échéant, dans un [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/).



## <a name="related-links"></a>Liens associés

- [LayoutOptions (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutoptions/)
- [LayoutOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/)
