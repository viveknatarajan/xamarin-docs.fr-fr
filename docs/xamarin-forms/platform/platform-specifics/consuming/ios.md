---
title: Caractéristiques de la plateforme d’iOS
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article montre comment consommer iOS-spécificités de la plateforme qui sont intégrés dans Xamarin.Forms.
ms.prod: xamarin
ms.assetid: C0837996-A1E8-47F9-B3A8-98EE43B4A675
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: be378a60a9d9a7b206b64f07ee70edb432cec8e3
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935652"
---
# <a name="ios-platform-specifics"></a>Caractéristiques de la plateforme d’iOS

_Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article montre comment consommer iOS-spécificités de la plateforme qui sont intégrés dans Xamarin.Forms._

Sur iOS, Xamarin.Forms contient les spécificités de plateforme suivantes :

- Flou prend en charge aucun [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/). Pour plus d’informations, consultez [appliquant flou](#blur).
- Contrôle si le titre de page est affiché comme un titre de grande taille dans la barre de navigation de page. Pour plus d’informations, consultez [affichage des grands titres](#large_title).
- S’assurer de ce contenu de la page est positionné sur une zone de l’écran qui est sécurisé pour tous les appareils iOS. Pour plus d’informations, consultez [activer le repère de disposition de zone sans échec](#safe_area_layout).
- Une barre de navigation translucide. Pour plus d’informations, consultez [rendre le translucide de barre de Navigation](#translucent_navigation_bar).
- Contrôle si le texte de barre d’état de couleur sur un [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) est ajustée pour correspondre à la luminosité de la barre de navigation. Pour plus d’informations, consultez [ajuster le Mode de couleur de texte de barre d’état](#status_bar_color_mode).
- S’assurer qu’entrée texte s’adapte à un [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) en ajustant la taille de police. Pour plus d’informations, consultez [ajuster la taille de police d’une entrée](#adjust_font_size).
- Contrôle si la sélection d’éléments se produit dans un [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/). Pour plus d’informations, consultez [sélection d’éléments de contrôle de sélecteur de](#picker_update_mode).
- Définition de la visibilité de barre d’état sur un [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/). Pour plus d’informations, consultez [définissant la visibilité de barre d’état sur une Page](#set_status_bar_visibility).
- Contrôle si un [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) gère un mouvement tactile ou la transmet à son contenu. Pour plus d’informations, consultez [retarder contenu finales dans un ScrollView](#delay_content_touches).
- Définir le style du séparateur sur un [ `ListView` ](xref:Xamarin.Forms.ListView). Pour plus d’informations, consultez [définir le Style de séparateur sur un ListView](#listview-separatorstyle).
- Désactiver le mode hérité de couleur sur la prise en charge [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Pour plus d’informations, consultez [la désactivation de Mode de couleur hérité](#legacy-color-mode).

<a name="blur" />

## <a name="applying-blur"></a>Application de flou

Cette plate-forme spécifique sert à estomper le contenu en dessous et est consommé dans XAML en définissant le [ `VisualElement.BlurEffect` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.BlurEffectProperty/) propriété attachée à une valeur de la [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) énumération :

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

Le `BoxView.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le [ `VisualElement.UseBlurEffect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.UseBlurEffect/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, est utilisé pour appliquer l’effet de flou, avec le [ `BlurEffectStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle) énumération fournissant quatre valeurs : [ `None` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.None), [ `ExtraLight` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.ExtraLight), [ `Light` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Light), et [ `Dark` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Dark).

Le résultat qui est spécifié [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) est appliqué à la [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) de l’instance, les flous le [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) en dessous :

![](ios-images/blur-effect.png "Spécifique à la plateforme effet de flou")

<a name="large_title" />

## <a name="displaying-large-titles"></a>Affichage de grands titres

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

Le `NavigationPage.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le `NavigationPage.SetPrefersLargeTitle` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) espace de noms, détermine si les grands titres sont activées.

Condition que les grands titres sont activés sur le [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), toutes les pages de la pile de navigation affichera les grands titres. Ce comportement peut être substitué dans les pages en définissant le `Page.LargeTitleDisplay` propriété attachée à une valeur de la `LargeTitleDisplayMode` énumération :

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

Le `Page.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le `Page.SetLargeTitleDisplay` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) espace de noms, contrôle le comportement de titre volumineux sur le [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), avec le `LargeTitleDisplayMode` énumération en fournissant trois possible valeurs :

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

Le résultat qui est spécifié `LargeTitleDisplayMode` est appliqué à la [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), qui contrôle le comportement de titre volumineux :

![](ios-images/large-title.png "Spécifique à la plateforme effet de flou")

<a name="safe_area_layout" />

## <a name="enabling-the-safe-area-layout-guide"></a>Activer le repère de disposition de zone protégée

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

Le `Page.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le `Page.SetUseSafeArea` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) contrôle l’espace de noms, si le repère de disposition de zone protégée est activée.

Le résultat est que le contenu de la page peut être positionné sur une zone de l’écran qui est sécurisé pour tous les iPhone :

[![](ios-images/safe-area-layout.png "Repère de disposition de zone protégée")](ios-images/safe-area-layout-large.png#lightbox "repère de disposition de zone protégée")

> [!NOTE]
> La zone protégée définie par Apple est utilisée dans Xamarin.Forms pour définir le [ `Page.Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/) propriété et remplacent toutes les valeurs précédentes de cette propriété qui ont été définis.

La zone protégée peut être personnalisée en récupérant sa [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) valeur avec le `Page.SafeAreaInsets` méthode à partir de la [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) espace de noms. Il peut ensuite être modifié en tant que nécessaire et réaffecté à la `Padding` propriété dans le constructeur de page ou [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) remplacer :

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    var safeInsets = On<iOS>().SafeAreaInsets();
    safeInsets.Left = 20;
    Padding = safeInsets;
}
```

<a name="translucent_navigation_bar" />

## <a name="making-the-navigation-bar-translucent"></a>Rendre la barre de Navigation translucide

Cette plate-forme spécifique est utilisé pour modifier la transparence de la barre de navigation et est consommé dans XAML en définissant le [ `NavigationPage.IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty/) propriété jointe un `boolean` valeur :

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

Le `NavigationPage.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le [ `NavigationPage.EnableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.EnableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) espace de noms, est utilisé pour rendre la barre de navigation translucide. En outre, le [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage/) classe dans le `Xamarin.Forms.PlatformConfiguration.iOSSpecific` espace de noms a également un [ `DisableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.DisableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) méthode qui rétablit l’état par défaut, la barre de navigation et un [ `SetIsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetIsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/System.Boolean/) méthode qui peut être utilisé pour activer/désactiver la transparence de barre de navigation en appelant le [ `IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) méthode :

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

Le résultat est que la transparence de la barre de navigation peut être modifiée :

![](ios-images/translucent-navigation-bar.png "Barre de Navigation translucide spécifiques à la plateforme")

<a name="status_bar_color_mode" />

## <a name="adjusting-the-status-bar-text-color-mode"></a>Ajustement de la barre Mode de couleur de texte d’état

Ce contrôle spécifique à la plateforme si le texte de barre d’état de couleur sur un [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) est ajustée pour correspondre à la luminosité de la barre de navigation. Elle est consommée dans XAML en définissant le [ `NavigationPage.StatusBarTextColorMode` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty/) propriété attachée à une valeur de la [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) énumération :

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

Le `NavigationPage.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le [ `NavigationPage.SetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) espace de noms, les contrôles si le texte de barre d’état de couleur sur la [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) est ajustée pour correspondre à la luminosité de la barre de navigation avec le [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) énumération fournissant deux valeurs possibles :

- [`DoNotAdjust`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust) – Indique que la barre de couleur du texte d’état ne doit pas être ajustée.
- [`MatchNavigationBarTextLuminosity`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity) – Indique que la barre de couleur du texte d’état doit correspondre à la luminosité de la barre de navigation.

En outre, le [ `GetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.GetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/) méthode peut être utilisée pour récupérer la valeur actuelle de la [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) énumération est appliquée à la [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage).

Le résultat est que l’état de la barre de couleur du texte sur un [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) peut être ajustée pour correspondre à la luminosité de la barre de navigation. Dans cet exemple, le modifications de couleur de texte de la barre d’état en tant que l’utilisateur bascule entre le [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) et [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) pages d’un [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

![](ios-images/status-bar-text-color-mode.png "Barre d’état texte couleur Mode spécifique à la plateforme")

<a name="adjust_font_size" />

## <a name="adjusting-the-font-size-of-an-entry"></a>Ajuster la taille de police d’une entrée

Cette plate-forme spécifique est utilisé à l’échelle la taille de police un [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) pour vous assurer que le texte entré tient dans le contrôle. Elle est consommée dans XAML en définissant le [ `Entry.AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidthProperty/) propriété jointe un `boolean` valeur :

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

Le `Entry.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le [ `Entry.EnableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.EnableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) espace de noms, est utilisé à l’échelle la taille de police du texte entré pour vous assurer qu’elles s’ajustent à la [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/). En outre, le [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry/) classe dans le `Xamarin.Forms.PlatformConfiguration.iOSSpecific` espace de noms a également un [ `DisableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.DisableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) méthode qui désactive cette spécifiques à la plateforme, et un [ `SetAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.SetAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/System.Boolean/) méthode qui peut être utilisé pour activer/désactiver la taille de police mise à l’échelle en appelant le [ `AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) méthode :

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

Le résultat est que la taille de police de la [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) est mis à l’échelle pour vous assurer que le texte entré tient dans le contrôle :

![](ios-images/entry-font-size.png "Ajuster la police taille plateforme spécifique de l’entrée")

<a name="picker_update_mode" />

## <a name="controlling-picker-item-selection"></a>Contrôle de sélection d’élément sélecteur

Cette spécifiques à la plateforme contrôle si la sélection d’éléments se produit dans un [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), permettant à l’utilisateur spécifier que la sélection d’éléments se produit lorsque vous parcourez les éléments dans le contrôle, ou uniquement une fois que le **fait** bouton est enfoncé. Elle est consommée dans XAML en définissant le `Picker.UpdateMode` propriété attachée à une valeur de la `UpdateMode` énumération :

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

Le `Picker.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le `Picker.SetUpdateMode` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) espace de noms, permet de contrôler en cas de sélection d’élément, avec la `UpdateMode` énumération fournissant deux valeurs possibles :

- `Immediately` : sélection d’éléments se produit lorsque l’utilisateur parcourt des éléments dans le [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/). Il s’agit du comportement par défaut dans Xamarin.Forms.
- `WhenFinished` : sélection d’éléments se produit uniquement une fois que l’utilisateur a appuyé sur le **fait** situé dans le [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/).

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

Le résultat qui est spécifié `UpdateMode` est appliqué à la [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), qui contrôle lorsque la sélection d’éléments se produit :

[![](ios-images/picker-updatemode.png "Sélecteur UpdateMode spécifiques à la plateforme")](ios-images/picker-updatemode-large.png#lightbox "Picker UpdateMode Plaform-Specific")

<a name="set_status_bar_visibility" />

## <a name="setting-the-status-bar-visibility-on-a-page"></a>Définition de la barre d’état visibilité sur une Page

Cette plate-forme spécifique est utilisé pour définir la visibilité de la barre d’état sur un [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), et il inclut la possibilité de contrôler comment la barre d’état entre ou quitte le `Page`. Elle est consommée dans XAML en définissant le `Page.PrefersStatusBarHidden` propriété attachée à une valeur de la `StatusBarHiddenMode` énumération et éventuellement le `Page.PreferredStatusBarUpdateAnimation` propriété attachée à une valeur de la `UIStatusBarAnimation` énumération :

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

Le `Page.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le `Page.SetPrefersStatusBarHidden` (méthode), dans le `Xamarin.Forms.PlatformConfiguration.iOSSpecific` espace de noms, est utilisé pour définir la visibilité de la barre d’état sur un [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) en spécifiant une de le `StatusBarHiddenMode` valeurs d’énumération : `Default`, `True` , ou `False`. Le `StatusBarHiddenMode.True` et `StatusBarHiddenMode.False` valeurs définir la visibilité de barre d’état, quel que soit l’orientation de l’appareil et le `StatusBarHiddenMode.Default` valeur masque la barre d’état dans un environnement compact verticalement.

Le résultat est que la visibilité de la barre d’état sur un [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) peuvent être définies :

![](ios-images/hide-status-bar.png "Barre d’état visibilité spécifiques à la plateforme")

> [!NOTE]
> Sur un [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), spécifié `StatusBarHiddenMode` valeur d’énumération mettra met également à jour la barre d’état sur toutes les pages de l’enfant. Sur tous les autres [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-types dérivés, spécifiés `StatusBarHiddenMode` valeur d’énumération met à jour uniquement la barre d’état sur la page actuelle.

Le `Page.SetPreferredStatusBarUpdateAnimation` méthode est utilisée pour définir la façon dont la barre d’état entre ou quitte le [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) en spécifiant une de le `UIStatusBarAnimation` valeurs d’énumération : `None`, `Fade`, ou `Slide`. Si le `Fade` ou `Slide` valeur d’énumération est spécifiée, une seconde 0,25 animation s’exécute comme la barre d’état entre ou quitte le `Page`.

<a name="delay_content_touches" />

## <a name="delaying-content-touches-in-a-scrollview"></a>Temporiser finales contenus dans un ScrollView

Un minuteur implicit est déclenché lorsqu’un mouvement tactile commence dans un [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) sur iOS et le `ScrollView` décide, en fonction de l’action de l’utilisateur dans l’intervalle de minuterie, s’il doit gérer le mouvement ou passer à son contenu. Par défaut, le fichier iOS `ScrollView` finales de contenu des retards, mais cela peut entraîner des problèmes dans certaines circonstances avec le `ScrollView` contenu ne gagnante pas le mouvement lorsqu’elle le devrait. Par conséquent, ce contrôle spécifique à la plateforme si un `ScrollView` gère un mouvement tactile ou la transmet à son contenu. Elle est consommée dans XAML en définissant le `ScrollView.ShouldDelayContentTouches` propriété jointe un `boolean` valeur :

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

Le `ScrollView.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le `ScrollView.SetShouldDelayContentTouches` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) espace de noms, est utilisée pour déterminer si un [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) gère un mouvement tactile ou la transmet à son contenu. En outre, le `SetShouldDelayContentTouches` méthode peut être utilisée pour activer/désactiver retarder finales de contenu en appelant le `ShouldDelayContentTouches` méthode à retourner si la touche de contenu est différée :

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

Le résultat est qu’un [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) peut désactiver ce qui retarde la réception de contenu finales, par conséquent, qui dans ce scénario la [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) reçoit le mouvement plutôt que la [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) page de la [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

[![](ios-images/scrollview-delay-content-touches.png "Délai de ScrollView contenu touche spécifiques à la plateforme")](ios-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView Delay Content Touches Plaform-Specific")

<a name="listview-separatorstyle" />

## <a name="setting-the-separator-style-on-a-listview"></a>Définir le Style de séparateur sur un ListView

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

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>Désactiver le Mode de couleur hérité

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

## <a name="summary"></a>Récapitulatif

Cet article a montré comment consommer iOS-spécificités de la plateforme qui sont intégrés dans Xamarin.Forms. Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets.


## <a name="related-links"></a>Liens associés

- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [iOSSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)
