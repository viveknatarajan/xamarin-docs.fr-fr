---
title: Caractéristiques de la plateforme d’iOS
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article montre comment consommer iOS-spécificités de la plateforme qui sont intégrés dans Xamarin.Forms.
ms.prod: xamarin
ms.assetid: C0837996-A1E8-47F9-B3A8-98EE43B4A675
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 12fd9e477e24058d36128e52b7b5dd9074598be8
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171792"
---
# <a name="ios-platform-specifics"></a>Caractéristiques de la plateforme d’iOS

_Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article montre comment consommer iOS-spécificités de la plateforme qui sont intégrés dans Xamarin.Forms._

## <a name="visualelements"></a>VisualElements

Sur iOS, les fonctionnalités spécifiques à la plateforme suivante sont fournie pour les vues, les pages et les dispositions Xamarin.Forms :

- Flou prend en charge aucun [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Pour plus d’informations, consultez [appliquant flou](#blur).
- Désactiver le mode hérité de couleur sur la prise en charge [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Pour plus d’informations, consultez [la désactivation de Mode de couleur hérité](#legacy-color-mode).
- L’activation d’une ombre sur un [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Pour plus d’informations, consultez [l’activation d’une ombre](#drop-shadow).

<a name="blur" />

### <a name="applying-blur"></a>Application de flou

Cette plate-forme spécifique sert à estomper le contenu en dessous et est consommé dans XAML en définissant le [ `VisualElement.BlurEffect` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.BlurEffectProperty) propriété attachée à une valeur de la [ `BlurEffectStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle) énumération :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
  ...
  <AbsoluteLayout HorizontalOptions="Center">
      <Image Source="monkeyface.png" />
      <BoxView x:Name="boxView" ios:VisualElement.BlurEffect="ExtraLight" HeightRequest="300" WidthRequest="300" />
  </AbsoluteLayout>
  ...
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

boxView.On<iOS>().UseBlurEffect(BlurEffectStyle.ExtraLight);
```

Le `BoxView.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le [ `VisualElement.UseBlurEffect` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.UseBlurEffect(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, est utilisé pour appliquer l’effet de flou, avec le [ `BlurEffectStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle) énumération fournissant quatre valeurs : [ `None` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.None), [ `ExtraLight` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.ExtraLight), [ `Light` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Light), et [ `Dark` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Dark).

Le résultat qui est spécifié [ `BlurEffectStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle) est appliqué à la [ `BoxView` ](xref:Xamarin.Forms.BoxView) de l’instance, les flous le [ `Image` ](xref:Xamarin.Forms.Image) en dessous :

![](ios-images/blur-effect.png "Spécifique à la plateforme effet de flou")

> [!NOTE]
> Lors de l’ajout d’un effet de flou à une [ `VisualElement` ](xref:Xamarin.Forms.VisualElement), les événements tactiles est reçus par le `VisualElement`.

<a name="legacy-color-mode" />

### <a name="disabling-legacy-color-mode"></a>Désactiver le Mode de couleur hérité

Certaines des vues Xamarin.Forms fonctionnalité un mode couleur hérité. Dans ce mode, lorsque le [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) propriété de la vue est définie sur `false`, la vue remplace les couleurs définies par l’utilisateur avec les couleurs natif par défaut de l’état désactivé. Pour vers l’arrière, compatibilité, ce mode hérité couleur reste le comportement par défaut pour les vues prises en charge.

Spécifiques à cette plateforme désactive ce mode couleur hérité, afin que les couleurs définies sur une vue par l’utilisateur sont conservées même lorsque la vue est désactivée. Elle est consommée dans XAML en définissant le [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.IsLegacyColorModeEnabledProperty) propriété jointe `false`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                ios:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

_legacyColorModeDisabledButton.On<iOS>().SetIsLegacyColorModeEnabled(false);
```

Le `VisualElement.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Boolean)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, est utilisé pour contrôler si le mode hérité est désactivé. En outre, le [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})) méthode peut être utilisée pour retourner si le mode hérité est désactivé.

Le résultat est que le mode hérité de couleur peut être désactivé, afin que les couleurs définies sur une vue par l’utilisateur restent même lorsque la vue est désactivée :

![](ios-images/legacy-color-mode-disabled.png "Mode hérité couleur désactivé")

> [!NOTE]
> Lorsque vous définissez un [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) sur une vue, le mode hérité de couleur est complètement ignoré. Pour plus d’informations sur les états visuels, consultez [le Gestionnaire d’état visuel Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

<a name="drop-shadow" />

### <a name="enabling-a-drop-shadow"></a>L’activation d’une ombre

Cette plate-forme spécifique est utilisée pour activer une ombre sur un [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Elle est consommée dans XAML en définissant le [ `VisualElement.IsShadowEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.IsShadowEnabledProperty) propriété jointe `true`, ainsi que d’un nombre d’autres facultatif des propriétés qui contrôlent l’ombre jointes :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <BoxView ...
                 ios:VisualElement.IsShadowEnabled="true"
                 ios:VisualElement.ShadowColor="Purple"
                 ios:VisualElement.ShadowOpacity="0.7"
                 ios:VisualElement.ShadowRadius="12">
            <ios:VisualElement.ShadowOffset>
                <Size>
                    <x:Arguments>
                        <x:Double>10</x:Double>
                        <x:Double>10</x:Double>
                    </x:Arguments>
                </Size>
            </ios:VisualElement.ShadowOffset>
         </BoxView>
        ...
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var boxView = new BoxView { Color = Color.Aqua, WidthRequest = 100, HeightRequest = 100 };
boxView.On<iOS>()
       .SetIsShadowEnabled(true)
       .SetShadowColor(Color.Purple)
       .SetShadowOffset(new Size(10,10))
       .SetShadowOpacity(0.7)
       .SetShadowRadius(12);
```

Le `VisualElement.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le [ `VisualElement.SetIsShadowEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetIsShadowEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Boolean)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, est utilisé pour contrôler si une ombre portée est activée sur le `VisualElement`. En outre, les méthodes suivantes peuvent être appelées pour contrôler l’ombre :

- [`SetShadowColor`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetShadowColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},Xamarin.Forms.Color)) : définit la couleur de l’ombre. La couleur par défaut est [ `Color.Default` ](xref:Xamarin.Forms.Color.Default*).
- [`SetShadowOffset`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetShadowOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},Xamarin.Forms.Size)) : définit le décalage de l’ombre. Le décalage modifie le sens de l’ombre est castée et spécifié comme un [ `Size` ](xref:Xamarin.Forms.Size) valeur. Le `Size` valeurs de la structure sont exprimées en unités indépendantes du périphérique, avec la première valeur en cours de la distance vers la gauche (valeur négative) ou la droite (valeur positive) et la deuxième valeur en cours de la distance ci-dessus (négatif) ou en dessous (valeur positive) . La valeur par défaut de cette propriété est (0.0, 0.0), ce qui entraîne le cliché instantané est castée autour de chaque côté de la `VisualElement`.
- [`SetShadowOpacity`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetShadowOpacity(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Double)) : définit l’opacité de l’ombre, avec la valeur en cours dans la plage de 0,0 (transparent) à 1,0 (opaque). La valeur d’opacité par défaut est 0,5.
- [`SetShadowRadius`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetShadowRadius(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Double)) : définit le rayon de flou utilisé pour restituer l’ombre portée. La valeur de rayon par défaut est 10.0.

> [!NOTE]
> L’état d’une ombre portée peut être interrogé en appelant le [ `GetIsShadowEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetIsShadowEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})), [ `GetShadowColor` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetShadowColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})), [ `GetShadowOffset` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetShadowOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})), [ `GetShadowOpacity` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetShadowOpacity(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})), et [ `GetShadowRadius` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetShadowRadius(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})) méthodes.

Le résultat est qu’une ombre portée peut être activée sur un [ `VisualElement` ](xref:Xamarin.Forms.VisualElement):

![](ios-images/drop-shadow.png "Ombre activée")

## <a name="views"></a>Affichages

Sur iOS, les fonctionnalités spécifiques à la plateforme suivante sont fournie pour les vues Xamarin.Forms :

- S’assurer qu’entrée texte s’adapte à un [ `Entry` ](xref:Xamarin.Forms.Entry) en ajustant la taille de police. Pour plus d’informations, consultez [ajuster la taille de police d’une entrée](#adjust_font_size).
- Définir la couleur du curseur dans un [ `Entry` ](xref:Xamarin.Forms.Entry). Pour plus d’informations, consultez [définissant la couleur du curseur entrée](#entry-cursorcolor).
- Définir le style du séparateur sur un [ `ListView` ](xref:Xamarin.Forms.ListView). Pour plus d’informations, consultez [définir le Style de séparateur sur un ListView](#listview-separatorstyle).
- Contrôle si la sélection d’éléments se produit dans un [ `Picker` ](xref:Xamarin.Forms.Picker). Pour plus d’informations, consultez [sélection d’éléments de contrôle de sélecteur de](#picker_update_mode).
- L’activation de la [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) propriété à définir en appuyant sur une position sur le [ `Slider` ](xref:Xamarin.Forms.Slider) barre, plutôt que d’avoir à faire glisser le `Slider` thumb. Pour plus d’informations, consultez [l’activation d’un curseur de défilement pour le déplacement en appuyant sur](#slider-updateontap).

<a name="adjust_font_size" />

### <a name="adjusting-the-font-size-of-an-entry"></a>Ajuster la taille de police d’une entrée

Cette plate-forme spécifique est utilisé à l’échelle la taille de police un [ `Entry` ](xref:Xamarin.Forms.Entry) pour vous assurer que le texte entré tient dans le contrôle. Elle est consommée dans XAML en définissant le [ `Entry.AdjustsFontSizeToFitWidth` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidthProperty) propriété jointe un `boolean` valeur :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    <StackLayout Margin="20">
        <Entry x:Name="entry"
               Placeholder="Enter text here to see the font size change"
               FontSize="22"
               ios:Entry.AdjustsFontSizeToFitWidth="true" />
        ...
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

entry.On<iOS>().EnableAdjustsFontSizeToFitWidth();
```

Le `Entry.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le [ `Entry.EnableAdjustsFontSizeToFitWidth` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.EnableAdjustsFontSizeToFitWidth(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry})) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, est utilisé à l’échelle la taille de police du texte entré pour vous assurer qu’elles s’ajustent à la [ `Entry` ](xref:Xamarin.Forms.Entry). En outre, le [ `Entry` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry) classe dans le `Xamarin.Forms.PlatformConfiguration.iOSSpecific` espace de noms a également un [ `DisableAdjustsFontSizeToFitWidth` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.DisableAdjustsFontSizeToFitWidth(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry})) méthode qui désactive cette spécifiques à la plateforme, et un [ `SetAdjustsFontSizeToFitWidth` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.SetAdjustsFontSizeToFitWidth(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry},System.Boolean)) méthode qui peut être utilisé pour activer/désactiver la taille de police mise à l’échelle en appelant le [ `AdjustsFontSizeToFitWidth` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidth(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry})) méthode :

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

Le résultat est que la taille de police de la [ `Entry` ](xref:Xamarin.Forms.Entry) est mis à l’échelle pour vous assurer que le texte entré tient dans le contrôle :

![](ios-images/entry-font-size.png "Ajuster la police taille plateforme spécifique de l’entrée")

<a name="entry-cursorcolor" />

### <a name="setting-the-entry-cursor-color"></a>Définir la couleur du curseur entrée

Cette spécifiques à la plateforme définit la couleur du curseur un [ `Entry` ](xref:Xamarin.Forms.Entry) une couleur spécifiée. Elle est consommée dans XAML en définissant le [ `Entry.CursorColor` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.CursorColorProperty) propriété pouvant être liée à un [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Entry ... ios:Entry.CursorColor="LimeGreen" />
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var entry = new Xamarin.Forms.Entry();
entry.On<iOS>().SetCursorColor(Color.LimeGreen);
```

Le `Entry.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le [ `Entry.SetCursorColor` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.SetCursorColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry},Xamarin.Forms.Color)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, définit la couleur du curseur à une certaine [ `Color` ](xref:Xamarin.Forms.Color). En outre, le [ `Entry.GetCursorColor` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.GetCursorColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry})) méthode peut être utilisée pour récupérer la couleur actuelle du curseur.

Le résultat est que la couleur du curseur dans un [ `Entry` ](xref:Xamarin.Forms.Entry) peut être définie à un spécifique [ `Color` ](xref:Xamarin.Forms.Color):

![](ios-images/entry-cursorcolor.png "Couleur du curseur entrée")

<a name="listview-separatorstyle" />

### <a name="setting-the-separator-style-on-a-listview"></a>Définir le Style de séparateur sur un ListView

Cette spécifiques à la plateforme contrôle si le séparateur entre les cellules dans un [ `ListView` ](xref:Xamarin.Forms.ListView) utilise toute la largeur de la `ListView`. Elle est consommée dans XAML en définissant le [ `ListView.SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SeparatorStyleProperty) propriété attachée à une valeur de la [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) énumération :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.SeparatorStyle="FullWidth">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetSeparatorStyle(SeparatorStyle.FullWidth);
```

Le `ListView.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le [ `ListView.SetSeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SetSeparatorStyle(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, est utilisé pour contrôler si le séparateur entre les cellules de la [ `ListView` ](xref:Xamarin.Forms.ListView) utilise la version complète largeur de la `ListView`, avec le [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) énumération fournissant deux valeurs possibles :

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.Default) : indique le comportement de séparateur iOS par défaut. Il s’agit du comportement par défaut dans Xamarin.Forms.
- [`FullWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.FullWidth) – Indique que les séparateurs peuvent être issues d’un bord de la `ListView` à l’autre.

Le résultat qui est spécifié [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) valeur est appliquée à la [ `ListView` ](xref:Xamarin.Forms.ListView), qui contrôle la largeur du séparateur entre les cellules :

![](ios-images/listview-separatorstyle.png "ListView SeparatorStyle spécifiques à la plateforme")

> [!NOTE]
> Une fois que le style du séparateur a été défini sur `FullWidth`, elle ne peut pas être reconvertie en `Default` lors de l’exécution.

<a name="picker_update_mode" />

### <a name="controlling-picker-item-selection"></a>Contrôle de sélection d’élément sélecteur

Cette spécifiques à la plateforme contrôle si la sélection d’éléments se produit dans un [ `Picker` ](xref:Xamarin.Forms.Picker), permettant à l’utilisateur spécifier que la sélection d’éléments se produit lorsque vous parcourez les éléments dans le contrôle, ou uniquement une fois que le **fait** bouton est enfoncé. Elle est consommée dans XAML en définissant le `Picker.UpdateMode` propriété attachée à une valeur de la `UpdateMode` énumération :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <Picker ... Title="Select a monkey" ios:Picker.UpdateMode="WhenFinished">
          ...
        </Picker>
        ...
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

Le `Picker.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le `Picker.SetUpdateMode` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, permet de contrôler en cas de sélection d’élément, avec la `UpdateMode` énumération fournissant deux valeurs possibles :

- `Immediately` : sélection d’éléments se produit lorsque l’utilisateur parcourt des éléments dans le [ `Picker` ](xref:Xamarin.Forms.Picker). Il s’agit du comportement par défaut dans Xamarin.Forms.
- `WhenFinished` : sélection d’éléments se produit uniquement une fois que l’utilisateur a appuyé sur le **fait** situé dans le [ `Picker` ](xref:Xamarin.Forms.Picker).

En outre, le `SetUpdateMode` méthode peut être utilisée pour activer/désactiver les valeurs d’énumération en appelant le `UpdateMode` (méthode), qui retourne actuel `UpdateMode`:

```csharp
switch (picker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        picker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

Le résultat qui est spécifié `UpdateMode` est appliqué à la [ `Picker` ](xref:Xamarin.Forms.Picker), qui contrôle lorsque la sélection d’éléments se produit :

[![](ios-images/picker-updatemode.png "Sélecteur UpdateMode spécifiques à la plateforme")](ios-images/picker-updatemode-large.png#lightbox "sélecteur UpdateMode spécifiques à la plateforme")

<a name="slider-updateontap" />

### <a name="enabling-a-slider-thumb-to-move-on-tap"></a>L’activation d’un curseur de défilement à déplacer, appuyez sur

Spécifiques à cette plateforme permet la [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) propriété à définir en appuyant sur une position sur le [ `Slider` ](xref:Xamarin.Forms.Slider) barre, plutôt que d’avoir à faire glisser le `Slider` thumb. Elle est consommée dans XAML en définissant le [ `Slider.UpdateOnTap` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.UpdateOnTapProperty) propriété pouvant être liée `true`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Slider ... ios:Slider.UpdateOnTap="true" />
        ...
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var slider = new Xamarin.Forms.Slider();
slider.On<iOS>().SetUpdateOnTap(true);
```

Le `Slider.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le [ `Slider.SetUpdateOnTap` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.SetUpdateOnTap(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Slider},System.Boolean)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, est utilisé pour contrôler si un clic sur le `Slider` barre définira le [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) propriété. En outre, le [ `Slider.GetUpdateOnTap` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.GetUpdateOnTap(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Slider})) méthode peut être utilisée pour retourner si un clic sur le `Slider` barre définira le `Slider.Value` propriété.

Le résultat est qu’un clic sur le [ `Slider` ](xref:Xamarin.Forms.Slider) barre peut déplacer le `Slider` thumb et définissez le [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) propriété :

![](ios-images/slider-updateontap.png "Mise à jour de curseur sur Tap activé")

## <a name="pages"></a>Pages

Sur iOS, les fonctionnalités spécifiques à la plateforme suivantes sont fournie pour les pages Xamarin.Forms :

- Masquer le séparateur de barre de navigation sur un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Pour plus d’informations, consultez [masquant le séparateur de barre de Navigation sur un NavigationPage](#navigationpage-hideseparatorbar).
- Contrôle si la barre de navigation est translucide. Pour plus d’informations, consultez [rendre le translucide de barre de Navigation](#translucent_navigation_bar).
- Contrôle si le texte de barre d’état de couleur sur un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) est ajustée pour correspondre à la luminosité de la barre de navigation. Pour plus d’informations, consultez [ajuster le Mode de couleur de texte de barre d’état](#status_bar_color_mode).
- Contrôle si le titre de page est affiché comme un titre de grande taille dans la barre de navigation de page. Pour plus d’informations, consultez [affichage des grands titres](#large_title).
- Définition de la visibilité de barre d’état sur un [ `Page` ](xref:Xamarin.Forms.Page). Pour plus d’informations, consultez [définissant la visibilité de barre d’état sur une Page](#set_status_bar_visibility).
- S’assurer de ce contenu de la page est positionné sur une zone de l’écran qui est sécurisé pour tous les appareils iOS. Pour plus d’informations, consultez [activer le repère de disposition de zone sans échec](#safe_area_layout).
- Définir le style de présentation des pages modales sur un iPad. Pour plus d’informations, consultez [définir le Style de présentation Page modale sur un iPad](#modal-page-presentation-style).

<a name="navigationpage-hideseparatorbar" />

### <a name="hiding-the-navigation-bar-separator-on-a-navigationpage"></a>Masquer la barre de Navigation séparateur sur un NavigationPage

Cette plate-forme spécifique masque la ligne de séparation et les clichés instantanés qui se trouve en bas de la barre de navigation sur un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Elle est consommée dans XAML en définissant le [ `NavigationPage.HideNavigationBarSeparator` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.HideNavigationBarSeparatorProperty) propriété pouvant être liée `false`:

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ios:NavigationPage.HideNavigationBarSeparator="true">

</NavigationPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;

public class iOSTitleViewNavigationPageCS : Xamarin.Forms.NavigationPage
{
    public iOSTitleViewNavigationPageCS()
    {
        On<iOS>().SetHideNavigationBarSeparator(true);
    }
}
```

Le `NavigationPage.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le [ `NavigationPage.SetHideNavigationBarSeparator` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetHideNavigationBarSeparator(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage},System.Boolean)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, est utilisé pour contrôler si le séparateur de barre de navigation est masqué. En outre, le [ `NavigationPage.HideNavigationBarSeparator` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.HideNavigationBarSeparator(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage})) méthode peut être utilisée pour retourner si le séparateur de barre de navigation est masqué.

Le résultat est que le séparateur de barre de navigation sur un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) peuvent être masqués :

![](ios-images/navigationpage-hideseparatorbar.png "Barre de navigation NavigationPage masqué")

<a name="translucent_navigation_bar" />

### <a name="making-the-navigation-bar-translucent"></a>Rendre la barre de Navigation translucide

Cette plate-forme spécifique est utilisé pour modifier la transparence de la barre de navigation et est consommé dans XAML en définissant le [ `NavigationPage.IsNavigationBarTranslucent` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty) propriété jointe un `boolean` valeur :

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                BackgroundColor="Blue"
                ios:NavigationPage.IsNavigationBarTranslucent="true">
  ...
</NavigationPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

(App.Current.MainPage as Xamarin.Forms.NavigationPage).BackgroundColor = Color.Blue;
(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().EnableTranslucentNavigationBar();
```

Le `NavigationPage.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le [ `NavigationPage.EnableTranslucentNavigationBar` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.EnableTranslucentNavigationBar(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage})) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, est utilisé pour rendre la barre de navigation translucide. En outre, le [ `NavigationPage` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage) classe dans le `Xamarin.Forms.PlatformConfiguration.iOSSpecific` espace de noms a également un [ `DisableTranslucentNavigationBar` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.DisableTranslucentNavigationBar(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage})) méthode qui rétablit l’état par défaut, la barre de navigation et un [ `SetIsNavigationBarTranslucent` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetIsNavigationBarTranslucent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage},System.Boolean)) méthode qui peut être utilisé pour activer/désactiver la transparence de barre de navigation en appelant le [ `IsNavigationBarTranslucent` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage})) méthode :

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

Le résultat est que la transparence de la barre de navigation peut être modifiée :

![](ios-images/translucent-navigation-bar.png "Barre de Navigation translucide spécifiques à la plateforme")

<a name="status_bar_color_mode" />

### <a name="adjusting-the-status-bar-text-color-mode"></a>Ajustement de la barre Mode de couleur de texte d’état

Ce contrôle spécifique à la plateforme si le texte de barre d’état de couleur sur un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) est ajustée pour correspondre à la luminosité de la barre de navigation. Elle est consommée dans XAML en définissant le [ `NavigationPage.StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty) propriété attachée à une valeur de la [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) énumération :

```xaml
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    x:Class="PlatformSpecifics.iOSStatusBarTextColorModePage">
    <MasterDetailPage.Master>
        <ContentPage Title="Master Page Title" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <NavigationPage BarBackgroundColor="Blue" BarTextColor="White"
                        ios:NavigationPage.StatusBarTextColorMode="MatchNavigationBarTextLuminosity">
            <x:Arguments>
                <ContentPage>
                    <Label Text="Slide the master page to see the status bar text color mode change." />
                </ContentPage>
            </x:Arguments>
        </NavigationPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>

```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

IsPresentedChanged += (sender, e) =>
{
    var mdp = sender as MasterDetailPage;
    if (mdp.IsPresented)
        ((Xamarin.Forms.NavigationPage)mdp.Detail)
          .On<iOS>()
          .SetStatusBarTextColorMode(StatusBarTextColorMode.DoNotAdjust);
    else
        ((Xamarin.Forms.NavigationPage)mdp.Detail)
          .On<iOS>()
          .SetStatusBarTextColorMode(StatusBarTextColorMode.MatchNavigationBarTextLuminosity);
};
```

Le `NavigationPage.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le [ `NavigationPage.SetStatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetStatusBarTextColorMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage},Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, les contrôles si le texte de barre d’état de couleur sur la [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) est ajustée pour correspondre à la luminosité de la barre de navigation avec le [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) énumération fournissant deux valeurs possibles :

- [`DoNotAdjust`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust) – Indique que la barre de couleur du texte d’état ne doit pas être ajustée.
- [`MatchNavigationBarTextLuminosity`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity) – Indique que la barre de couleur du texte d’état doit correspondre à la luminosité de la barre de navigation.

En outre, le [ `GetStatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.GetStatusBarTextColorMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage})) méthode peut être utilisée pour récupérer la valeur actuelle de la [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) énumération est appliquée à la [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage).

Le résultat est que l’état de la barre de couleur du texte sur un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) peut être ajustée pour correspondre à la luminosité de la barre de navigation. Dans cet exemple, le modifications de couleur de texte de la barre d’état en tant que l’utilisateur bascule entre le [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) et [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) pages d’un [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage):

![](ios-images/status-bar-text-color-mode.png "Barre d’état texte couleur Mode spécifique à la plateforme")

<a name="large_title" />

### <a name="displaying-large-titles"></a>Affichage de grands titres

Cette plate-forme spécifique est utilisé pour afficher le titre de page comme un titre de grande taille dans la barre de navigation, des appareils qui utilisent iOS 11 ou version ultérieure. Un titre volumineux est aligné à gauche et utilise une police plus grande et passe à un titre standard comme l’utilisateur commence à faire défiler un contenu, afin que l’écran est utilisé efficacement. Cependant, en mode paysage, le titre renvoie au centre de la barre de navigation pour optimiser la disposition du contenu. Elle est consommée dans XAML en définissant le `NavigationPage.PrefersLargeTitles` propriété jointe un `boolean` valeur :

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

Il peut également être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

Le `NavigationPage.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le `NavigationPage.SetPrefersLargeTitle` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, détermine si les grands titres sont activées.

Condition que les grands titres sont activés sur le [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), toutes les pages de la pile de navigation affichera les grands titres. Ce comportement peut être substitué dans les pages en définissant le `Page.LargeTitleDisplay` propriété attachée à une valeur de la `LargeTitleDisplayMode` énumération :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Large Title"
             ios:Page.LargeTitleDisplay="Never">
  ...
</ContentPage>
```

Vous pouvez également le comportement de la page peut être substitué à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSLargeTitlePageCS : ContentPage
{
    public iOSLargeTitlePageCS(ICommand restore)
    {
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        ...
    }
    ...
}
```

Le `Page.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le `Page.SetLargeTitleDisplay` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, contrôle le comportement de titre volumineux sur le [ `Page` ](xref:Xamarin.Forms.Page), avec le `LargeTitleDisplayMode` énumération en fournissant trois possible valeurs :

- `Always` – forcer la barre de navigation et la police taille à utiliser le grand format.
- `Automatic` – Utilisez le même style (petit ou grand) en tant que l’élément précédent dans la pile de navigation.
- `Never` – forcer l’utilisation de la barre de navigation standard, petit format.

En outre, le `SetLargeTitleDisplay` méthode peut être utilisée pour activer/désactiver les valeurs d’énumération en appelant le `LargeTitleDisplay` (méthode), qui retourne actuel `LargeTitleDisplayMode`:

```csharp
switch (On<iOS>().LargeTitleDisplay())
{
    case LargeTitleDisplayMode.Always:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Automatic);
        break;
    case LargeTitleDisplayMode.Automatic:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        break;
    case LargeTitleDisplayMode.Never:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Always);
        break;
}
```

Le résultat qui est spécifié `LargeTitleDisplayMode` est appliqué à la [ `Page` ](xref:Xamarin.Forms.Page), qui contrôle le comportement de titre volumineux :

![](ios-images/large-title.png "Spécifique à la plateforme effet de flou")

<a name="set_status_bar_visibility" />

### <a name="setting-the-status-bar-visibility-on-a-page"></a>Définition de la barre d’état visibilité sur une Page

Cette plate-forme spécifique est utilisé pour définir la visibilité de la barre d’état sur un [ `Page` ](xref:Xamarin.Forms.Page), et il inclut la possibilité de contrôler comment la barre d’état entre ou quitte le `Page`. Elle est consommée dans XAML en définissant le `Page.PrefersStatusBarHidden` propriété attachée à une valeur de la `StatusBarHiddenMode` énumération et éventuellement le `Page.PreferredStatusBarUpdateAnimation` propriété attachée à une valeur de la `UIStatusBarAnimation` énumération :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersStatusBarHidden="True"
             ios:Page.PreferredStatusBarUpdateAnimation="Fade">
  ...
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersStatusBarHidden(StatusBarHiddenMode.True)
         .SetPreferredStatusBarUpdateAnimation(UIStatusBarAnimation.Fade);
```

Le `Page.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le `Page.SetPrefersStatusBarHidden` (méthode), dans le `Xamarin.Forms.PlatformConfiguration.iOSSpecific` espace de noms, est utilisé pour définir la visibilité de la barre d’état sur un [ `Page` ](xref:Xamarin.Forms.Page) en spécifiant une de le `StatusBarHiddenMode` valeurs d’énumération : `Default`, `True` , ou `False`. Le `StatusBarHiddenMode.True` et `StatusBarHiddenMode.False` valeurs définir la visibilité de barre d’état, quel que soit l’orientation de l’appareil et le `StatusBarHiddenMode.Default` valeur masque la barre d’état dans un environnement compact verticalement.

Le résultat est que la visibilité de la barre d’état sur un [ `Page` ](xref:Xamarin.Forms.Page) peuvent être définies :

![](ios-images/hide-status-bar.png "Barre d’état visibilité spécifiques à la plateforme")

> [!NOTE]
> Sur un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), spécifié `StatusBarHiddenMode` valeur d’énumération mettra met également à jour la barre d’état sur toutes les pages de l’enfant. Sur tous les autres [ `Page` ](xref:Xamarin.Forms.Page)-types dérivés, spécifiés `StatusBarHiddenMode` valeur d’énumération met à jour uniquement la barre d’état sur la page actuelle.

Le `Page.SetPreferredStatusBarUpdateAnimation` méthode est utilisée pour définir la façon dont la barre d’état entre ou quitte le [ `Page` ](xref:Xamarin.Forms.Page) en spécifiant une de le `UIStatusBarAnimation` valeurs d’énumération : `None`, `Fade`, ou `Slide`. Si le `Fade` ou `Slide` valeur d’énumération est spécifiée, une seconde 0,25 animation s’exécute comme la barre d’état entre ou quitte le `Page`.

<a name="safe_area_layout" />

### <a name="enabling-the-safe-area-layout-guide"></a>Activer le repère de disposition de zone protégée

Cette plate-forme spécifique est utilisé pour vous assurer que le contenu de la page est positionné sur une zone de l’écran qui est sécurisé pour tous les appareils qui utilisent iOS 11 et supérieur. Plus précisément, il vous aidera à vous assurer que le contenu n’est pas découpé par les angles arrondis d’appareil, l’indicateur d’accueil ou le boîtier de capteur sur un iPhone X. Elle est consommée dans XAML en définissant le `Page.UseSafeArea` propriété jointe un `boolean` valeur :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Safe Area"
             ios:Page.UseSafeArea="true">
    <StackLayout>
        ...
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetUseSafeArea(true);
```

Le `Page.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le `Page.SetUseSafeArea` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) contrôle l’espace de noms, si le repère de disposition de zone protégée est activée.

Le résultat est que le contenu de la page peut être positionné sur une zone de l’écran qui est sécurisé pour tous les iPhone :

[![](ios-images/safe-area-layout.png "Repère de disposition de zone protégée")](ios-images/safe-area-layout-large.png#lightbox "repère de disposition de zone protégée")

> [!NOTE]
> La zone protégée définie par Apple est utilisée dans Xamarin.Forms pour définir le [ `Page.Padding` ](xref:Xamarin.Forms.Page.Padding) propriété et remplacent toutes les valeurs précédentes de cette propriété qui ont été définis.

La zone protégée peut être personnalisée en récupérant sa [ `Thickness` ](xref:Xamarin.Forms.Thickness) valeur avec le `Page.SafeAreaInsets` méthode à partir de la [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms. Il peut ensuite être modifié en tant que nécessaire et réaffecté à la `Padding` propriété dans le constructeur de page ou [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) remplacer :

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    var safeInsets = On<iOS>().SafeAreaInsets();
    safeInsets.Left = 20;
    Padding = safeInsets;
}
```

<a name="modal-page-presentation-style" />

### <a name="setting-the-modal-page-presentation-style-on-an-ipad"></a>Définir le Style de présentation Page modale sur un iPad

Cette plate-forme spécifique est utilisé pour définir le style de présentation d’une page modale sur un iPad. Elle est consommée dans XAML en définissant le `Page.ModalPresentationStyle` propriété pouvant être liée à un `UIModalPresentationStyle` valeur d’énumération :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.ModalPresentationStyle="FormSheet">
    ...
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSModalFormSheetPageCS : ContentPage
{
    public iOSModalFormSheetPageCS()
    {
        On<iOS>().SetModalPresentationStyle(UIModalPresentationStyle.FormSheet);
        ...
    }
}
```

Le `Page.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le `Page.SetModalPresentationStyle` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, est utilisé pour définir le style de présentation modale sur un [ `Page` ](xref:Xamarin.Forms.Page) en spécifiant une des opérations suivantes `UIModalPresentationStyle` énumération valeurs :

- `FullScreen`, qui définit le style de présentation modale pour englober la totalité de l’écran. Par défaut, les pages modales sont affichés à l’aide de ce style de présentation.
- `FormSheet`, qui définit le style de présentation modale centrée sur et inférieure à l’écran.

En outre, le `GetModalPresentationStyle` méthode peut être utilisée pour récupérer la valeur actuelle de la `UIModalPresentationStyle` énumération est appliquée à la [ `Page` ](xref:Xamarin.Forms.Page).

Le résultat est que le style de présentation modale sur un [ `Page` ](xref:Xamarin.Forms.Page) peuvent être définies :

[![](ios-images/modal-presentation-style-small.png "Styles de présentation modale sur un iPad")](ios-images/modal-presentation-style-large.png#lightbox "modale Styles de présentation sur un iPad")

> [!NOTE]
> Les pages qui utilisent cette plateforme spécifique pour définir le style de présentation modale doivent utiliser la navigation modale. Pour plus d’informations, consultez [Pages modales Xamarin.Forms](~/xamarin-forms/app-fundamentals/navigation/modal.md).

## <a name="layouts"></a>Dispositions

Sur iOS, les fonctionnalités spécifiques à la plateforme suivante sont fournie pour les dispositions Xamarin.Forms :

- Contrôle si un [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) gère un mouvement tactile ou la transmet à son contenu. Pour plus d’informations, consultez [retarder contenu finales dans un ScrollView](#delay_content_touches).

<a name="delay_content_touches" />

### <a name="delaying-content-touches-in-a-scrollview"></a>Temporiser finales contenus dans un ScrollView

Un minuteur implicit est déclenché lorsqu’un mouvement tactile commence dans un [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) sur iOS et le `ScrollView` décide, en fonction de l’action de l’utilisateur dans l’intervalle de minuterie, s’il doit gérer le mouvement ou passer à son contenu. Par défaut, le fichier iOS `ScrollView` finales de contenu des retards, mais cela peut entraîner des problèmes dans certaines circonstances avec le `ScrollView` contenu ne gagnante pas le mouvement lorsqu’elle le devrait. Par conséquent, ce contrôle spécifique à la plateforme si un `ScrollView` gère un mouvement tactile ou la transmet à son contenu. Elle est consommée dans XAML en définissant le `ScrollView.ShouldDelayContentTouches` propriété jointe un `boolean` valeur :

```xaml
<MasterDetailPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <MasterDetailPage.Master>
        <ContentPage Title="Menu" BackgroundColor="Blue" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <ContentPage>
            <ScrollView x:Name="scrollView" ios:ScrollView.ShouldDelayContentTouches="false">
                <StackLayout Margin="0,20">
                    <Slider />
                    <Button Text="Toggle ScrollView DelayContentTouches" Clicked="OnButtonClicked" />
                </StackLayout>
            </ScrollView>
        </ContentPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

Le `ScrollView.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le `ScrollView.SetShouldDelayContentTouches` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, est utilisée pour déterminer si un [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) gère un mouvement tactile ou la transmet à son contenu. En outre, le `SetShouldDelayContentTouches` méthode peut être utilisée pour activer/désactiver retarder finales de contenu en appelant le `ShouldDelayContentTouches` méthode à retourner si la touche de contenu est différée :

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

Le résultat est qu’un [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) peut désactiver ce qui retarde la réception de contenu finales, par conséquent, qui dans ce scénario la [ `Slider` ](xref:Xamarin.Forms.Slider) reçoit le mouvement plutôt que la [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) page de la [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage):

[![](ios-images/scrollview-delay-content-touches.png "Délai de ScrollView contenu touche spécifiques à la plateforme")](ios-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView délai contenu touche spécifiques à la plateforme")

## <a name="application"></a>Application

Sur iOS, les fonctionnalités spécifiques à la plateforme suivantes sont fournie pour le Xamarin.Forms [ `Application` ](xref:Xamarin.Forms.Application) classe :

- L’activation de la disposition des contrôles et le rendu des mises à jour doivent être effectuées sur le thread principal. Pour plus d’informations, consultez [contrôle de gestion des mises à jour sur le Thread principal](#update-on-main-thread).
- L’activation une [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) dans une vue de défilement pour capturer et partager le mouvement panoramique avec la vue de défilement. Pour plus d’informations, consultez [l’activation de la reconnaissance du mouvement panoramique simultanées](#simultaneous-pan-gesture).

<a name="update-on-main-thread" />

### <a name="handling-control-updates-on-the-main-thread"></a>Gestion des mises à jour de contrôle sur le Thread principal

Spécifiques à cette plateforme permet de contrôler la disposition et le rendu des mises à jour doivent être effectuées sur le thread principal, au lieu d’en cours d’exécution sur un thread d’arrière-plan. Il doit être rarement nécessaire, mais dans certains cas peut empêcher les blocages. Son consommées dans XAML en définissant le `Application.HandleControlUpdatesOnMainThread` propriété pouvant être liée `true`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.HandleControlUpdatesOnMainThread="true">
    ...
</Application>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetHandleControlUpdatesOnMainThread(true);
```

Le `Application.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le `Application.SetHandleControlUpdatesOnMainThread` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, est utilisé pour contrôler si la disposition des contrôles et le rendu des mises à jour sont effectuées sur le thread principal, au lieu d’en cours d’exécution sur un thread d’arrière-plan. En outre, le `Application.GetHandleControlUpdatesOnMainThread` méthode peut être utilisée pour retourner si la disposition des contrôles et le rendu des mises à jour sont effectuées sur le thread principal.

<a name="simultaneous-pan-gesture" />

### <a name="enabling-simultaneous-pan-gesture-recognition"></a>L’activation de la reconnaissance de mouvement panoramique simultanées

Quand un [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) est attachée à une vue à l’intérieur d’une vue de défilement, toutes le panoramique mouvements sont capturées par le `PanGestureRecognizer` et ne sont pas passées à la vue de défilement. Par conséquent, la vue de défilement n’est plus défile.

Spécifiques à cette plateforme permet un `PanGestureRecognizer` dans une vue de défilement pour capturer et partager le mouvement panoramique avec la vue de défilement. Elle est consommée dans XAML en définissant le [ `Application.PanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.PanGestureRecognizerShouldRecognizeSimultaneouslyProperty) propriété jointe `true`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.PanGestureRecognizerShouldRecognizeSimultaneously="true">
    ...
</Application>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetPanGestureRecognizerShouldRecognizeSimultaneously(true);
```

Le `Application.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le [ `Application.SetPanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.SetPanGestureRecognizerShouldRecognizeSimultaneously(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Application},System.Boolean)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, est utilisé pour contrôler si un module de reconnaissance de mouvement panoramique dans une vue de défilement capturer le mouvement panoramique, ou capturer et partager le panoramique mouvements avec la vue de défilement. En outre, le [ `Application.GetPanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.GetPanGestureRecognizerShouldRecognizeSimultaneously(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Application})) méthode peut être utilisée pour retourner si le mouvement panoramique est partagé avec la vue de défilement qui contient le [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer).

Par conséquent, avec cette spécifiques à la plateforme est activée, lorsque un [ `ListView` ](xref:Xamarin.Forms.ListView) contient un [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer), à la fois le `ListView` et `PanGestureRecognizer` recevront le mouvement panoramique et le traiter. Toutefois, avec cette spécifiques à la plateforme désactivée quand un `ListView` contient un `PanGestureRecognizer`, le `PanGestureRecognizer` capturer le mouvement panoramique et le traiter et le `ListView` ne reçoivent pas le mouvement panoramique.

## <a name="summary"></a>Récapitulatif

Cet article a montré comment consommer iOS-spécificités de la plateforme qui sont intégrés dans Xamarin.Forms. Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets.

## <a name="related-links"></a>Liens associés

- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [iOSSpecific](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
