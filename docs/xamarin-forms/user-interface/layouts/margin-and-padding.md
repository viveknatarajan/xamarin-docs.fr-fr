---
title: Marge et marge intérieure
description: La marge et le remplissage des propriétés de contrôlent le comportement de disposition quand un élément est affiché dans l’interface utilisateur. Cet article explique la différence entre les deux propriétés et comment les définir.
ms.prod: xamarin
ms.assetid: BEB096BB-51DF-410F-B0F1-D235287B0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 545468d3b02f9651c45fcaebe159351aafea6432
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="margin-and-padding"></a>Marge et marge intérieure

_La marge et le remplissage des propriétés de contrôlent le comportement de disposition quand un élément est affiché dans l’interface utilisateur. Cet article explique la différence entre les deux propriétés et comment les définir._

## <a name="overview"></a>Vue d'ensemble

Marge et marge intérieure sont les concepts de mise en page :

- Le [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) propriété représente la distance entre un élément et ses éléments adjacents et est utilisée pour contrôler la position de rendu de l’élément et la position de rendu de ses voisins. `Margin` les valeurs peuvent être spécifiés sur [disposition](~/xamarin-forms/user-interface/controls/layouts.md) et [vue](~/xamarin-forms/user-interface/controls/views.md) classes.
- Le [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) propriété représente la distance entre un élément et ses éléments enfants et est utilisée pour séparer le contrôle à partir de son propre contenu. `Padding` les valeurs peuvent être spécifiés sur [disposition](~/xamarin-forms/user-interface/controls/layouts.md) classes.

Le diagramme suivant illustre les deux concepts :

[![](margin-and-padding-images/margins-and-padding-sml.png "Marges et remplissage Concepts")](margin-and-padding-images/margins-and-padding.png#lightbox "marges et remplissage Concepts")

Notez que [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) les valeurs sont additifs. Par conséquent, si deux éléments adjacents spécifient une marge de 20 pixels, la distance entre les éléments seront 40 pixels. En outre, les marges et remplissage sont additifs lorsque les deux sont appliquées, dans la mesure où la distance entre un élément et tout son contenu sera la marge plus la marge intérieure.

## <a name="specifying-a-thickness"></a>Qui spécifie une épaisseur

Le [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) et [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) propriétés sont toutes deux de type [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/). Il existe trois possibilités lorsque vous créez un `Thickness` structure :

- Créer un [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) structure définie par une seule valeur uniforme. La valeur unique est appliquée à la gauche, haut, droite et quatre côtés de l’élément.
- Créer un [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) structure définie par les valeurs horizontales et verticales. La valeur horizontale symétriquement concerne les côtés gauche et droit de l’élément, avec la valeur verticale symétriquement appliquées sur les côtés supérieur et inférieur de l’élément.
- Créer un [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) structure définie par quatre valeurs distinctes qui sont appliqués à la gauche, haut, droite et quatre côtés de l’élément.

L’exemple de code XAML suivant montre toutes les trois possibilités :

```xaml
<StackLayout Padding="0,20,0,0">
  <Label Text="Xamarin.Forms" Margin="20" />
  <Label Text="Xamarin.iOS" Margin="10, 15" />
  <Label Text="Xamarin.Android" Margin="0, 20, 15, 5" />
</StackLayout>
```

Le code C# équivalent est affiché dans l’exemple de code suivant :

```csharp
var stackLayout = new StackLayout {
  Padding = new Thickness(0,20,0,0),
  Children = {
    new Label { Text = "Xamarin.Forms", Margin = new Thickness (20) },
    new Label { Text = "Xamarin.iOS", Margin = new Thickness (10, 25) },
    new Label { Text = "Xamarin.Android", Margin = new Thickness (0, 20, 15, 5) }
  }
};
```

> [!NOTE]
> `Thickness` valeurs peuvent être négatifs, généralement, qui fait coïncider soit dépasse le contenu.

## <a name="summary"></a>Récapitulatif

Cet article ne présente la différence entre la [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) et [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) propriétés et comment les définir. Les propriétés contrôlent le comportement de disposition quand un élément est affiché dans l’interface utilisateur.


## <a name="related-links"></a>Liens associés

- [Margin](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/)
- [Padding](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/)
- [Épaisseur](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/)
