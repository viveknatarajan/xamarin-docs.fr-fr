---
title: Caractéristiques de la plateforme d’iOS
description: Caractéristiques de la plateforme vous permettent de vous permet d’utiliser la fonctionnalité est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou les effets. Cet article montre comment consommer les iOS plateforme-spécificités qui sont intégrées dans Xamarin.Forms.
ms.prod: xamarin
ms.assetid: C0837996-A1E8-47F9-B3A8-98EE43B4A675
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/16/2017
ms.openlocfilehash: 7826962cd3bf9595a63841e3f2d9fb377d1a0574
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="ios-platform-specifics"></a>Caractéristiques de la plateforme d’iOS

_Caractéristiques de la plateforme vous permettent de vous permet d’utiliser la fonctionnalité est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou les effets. Cet article montre comment consommer les iOS plateforme-spécificités qui sont intégrées dans Xamarin.Forms._

Sur iOS, Xamarin.Forms contient les spécifications de plateforme suivantes :

- Flou prend en charge aucun [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/). Pour plus d’informations, consultez [application flou](#blur).
- Contrôle si le titre de la page est affiché comme un titre de grande taille dans la barre de navigation de page. Pour plus d’informations, consultez [affichage des grands titres](#large_title).
- Vous être assuré que contenu de la page est placé sur une zone de l’écran qui est sécurisé pour tous les appareils iOS. Pour plus d’informations, consultez [activer le repère de mise en page de zone de sans échec](#safe_area_layout).
- Une barre de navigation translucide. Pour plus d’informations, consultez [qui effectue la translucide de barre de Navigation](#translucent_navigation_bar).
- Contrôle si le texte de la barre d’état de couleur sur un [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) est ajustée pour correspondre à la luminosité de la barre de navigation. Pour plus d’informations, consultez [ajuster le Mode de couleur de texte de barre d’état](#status_bar_color_mode).
- S’assurer que vous entrez le texte s’adapte à une [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) en ajustant la taille de police. Pour plus d’informations, consultez [ajuster la taille de police d’une entrée](#adjust_font_size).
- Contrôle si la sélection d’éléments se produit dans un [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/). Pour plus d’informations, consultez [sélection d’éléments de contrôle de sélecteur de](#picker_update_mode).
- Définir la visibilité de barre d’état sur un [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/). Pour plus d’informations, consultez [définition de la visibilité de barre d’état sur une Page](#set_status_bar_visibility).
- Contrôle si un [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) gère un mouvement tactile ou passe à son contenu. Pour plus d’informations, consultez [retarde la touche contenu dans un ScrollView](#delay_content_touches).

<a name="blur" />

## <a name="applying-blur"></a>Application de flou

Cette plateforme spécifique est utilisé pour estomper le contenu en dessous et qu’il est consommé dans XAML en définissant le [ `VisualElement.BlurEffect` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.BlurEffectProperty/) à une valeur de propriété attachée la [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) énumération :

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

Vous pouvez également être utilisée à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

boxView.On<iOS>().UseBlurEffect(BlurEffectStyle.ExtraLight);
```

Le `BoxView.On<iOS>` méthode spécifie que cette plate-forme spécifique sera exécuté uniquement sur iOS. Le [ `VisualElement.UseBlurEffect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.UseBlurEffect/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) espace de noms est utilisée pour appliquer l’effet de flou, avec la [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) énumération fournissant quatre valeurs : [ `None` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.None/), [ `ExtraLight` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.ExtraLight/), [ `Light` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Light/), et [ `Dark` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Dark/).

Le résultat qui est spécifié [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) est appliqué à la [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) de l’instance, les flous le [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) en dessous :

![](ios-images/blur-effect.png "Spécifique à la plateforme effet de flou")

<a name="large_title" />

## <a name="displaying-large-titles"></a>Affichage de grands titres

Cette plateforme spécifique est utilisé pour afficher le titre de la page comme un titre de grande taille dans la barre de navigation, des appareils qui utilisent des e/s ou supérieure à 11. Un titre de grande taille est aligné à gauche et utilise une plus grande police et adopte un titre standard que l’utilisateur commence à faire défiler le contenu, afin que la taille d’écran est utilisée de manière efficace. Toutefois, en mode paysage, le titre retourne au centre de la barre de navigation pour optimiser la disposition du contenu. Elle est consommée en XAML en définissant le `NavigationPage.PrefersLargeTitles` propriété attachée un `boolean` valeur :

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

Vous pouvez également être utilisée à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

Le `NavigationPage.On<iOS>` méthode spécifie que cette plate-forme spécifique sera exécuté uniquement sur iOS. Le `NavigationPage.SetPrefersLargeTitle` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) contrôle l’espace de noms, si les grands titres sont activées.

Sous réserve que les grands titres sont activés sur le [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), toutes les pages de la pile de navigation affiche les titres de grande taille. Ce comportement peut être substitué dans les pages en définissant le `Page.LargeTitleDisplay` à une valeur de propriété attachée le `LargeTitleDisplayMode` énumération :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Large Title"
             ios:Page.LargeTitleDisplay="Never">
  ...
</ContentPage>
```

Vous pouvez également le comportement de page peut être substitué à partir de c# à l’aide de l’API fluent :

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

Le `Page.On<iOS>` méthode spécifie que cette plate-forme spécifique sera exécuté uniquement sur iOS. Le `Page.SetLargeTitleDisplay` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) espace de noms, contrôle le comportement de titre volumineux sur le [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), avec le `LargeTitleDisplayMode` énumération en fournissant trois possible valeurs :

- `Always` – force la barre de navigation et la police à utiliser le format de grand taille.
- `Automatic` – Utilisez le même style (grand ou petit) que l’élément précédent dans la pile de navigation.
- `Never` – forcer l’utilisation de la barre de navigation de format standard, small.

En outre, le `SetLargeTitleDisplay` méthode peut être utilisée pour activer/désactiver les valeurs d’énumération en appelant le `LargeTitleDisplay` (méthode), qui renvoie l’actuel `LargeTitleDisplayMode`:

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

Le résultat qui est spécifié `LargeTitleDisplayMode` est appliqué à la [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), qui contrôle le comportement de titre long :

![](ios-images/large-title.png "Spécifique à la plateforme effet de flou")

<a name="safe_area_layout" />

## <a name="enabling-the-safe-area-layout-guide"></a>Activer le repère de mise en page de zone de sécurité

Cette plateforme spécifique est utilisé pour vous assurer que le contenu de la page est positionné sur une zone de l’écran qui est sécurisé pour tous les périphériques qui utilisent iOS 11 et version ultérieure. Plus précisément, elle vous sera utile pour vous assurer que le contenu n’est pas détouré par les angles arrondis d’appareil, l’indicateur d’origine ou le boîtier de capteur sur un iPhone X. Elle est consommée en XAML en définissant le `Page.UseSafeArea` propriété attachée un `boolean` valeur :

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

Vous pouvez également être utilisée à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetUseSafeArea(true);
```

Le `Page.On<iOS>` méthode spécifie que cette plate-forme spécifique sera exécuté uniquement sur iOS. Le `Page.SetUseSafeArea` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) contrôle l’espace de noms, si le guide de mise en page de zone de sécurité est activé.

Le résultat est que le contenu de la page peut être positionné sur une zone de l’écran qui est sécurisé pour tous les iPhone :

[![](ios-images/safe-area-layout.png "Guide de mise en page de zone de sécurité")](ios-images/safe-area-layout-large.png#lightbox "Guide de mise en page de zone de sécurité")

> [!NOTE]
> La zone de sans échec définie par Apple utilisée dans Xamarin.Forms pour définir le [ `Page.Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/) propriété et remplace toutes les valeurs précédentes de cette propriété qui ont été définies.

La zone de sans échec peut être personnalisée en récupérant sa [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) valeur avec le `Page.SafeAreaInsets` méthode à partir de la [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) espace de noms. Il peut ensuite être modifié en tant que nécessaire et réaffecté à la `Padding` propriété dans le constructeur de la page ou [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) remplacer :

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

Cette plateforme spécifique est utilisée pour modifier la transparence de la barre de navigation et est utilisée en XAML en définissant le [ `NavigationPage.IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty/) propriété attachée un `boolean` valeur :

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                BackgroundColor="Blue"
                ios:NavigationPage.IsNavigationBarTranslucent="true">
  ...
</NavigationPage>
```

Vous pouvez également être utilisée à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

(App.Current.MainPage as Xamarin.Forms.NavigationPage).BackgroundColor = Color.Blue;
(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().EnableTranslucentNavigationBar();
```

Le `NavigationPage.On<iOS>` méthode spécifie que cette plate-forme spécifique sera exécuté uniquement sur iOS. Le [ `NavigationPage.EnableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.EnableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) espace de noms est utilisé pour rendre la barre de navigation translucide. En outre, le [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage/) classe dans le `Xamarin.Forms.PlatformConfiguration.iOSSpecific` espace de noms comporte également un [ `DisableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.DisableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) méthode qui restaure l’état par défaut, la barre de navigation et un [ `SetIsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetIsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/System.Boolean/) méthode qui peut être utilisé pour activer/désactiver la transparence de barre de navigation en appelant le [ `IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) méthode :

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

Le résultat est que la transparence de la barre de navigation peut être modifiée :

![](ios-images/translucent-navigation-bar.png "Barre de Navigation translucide spécifique à la plateforme")

<a name="status_bar_color_mode" />

## <a name="adjusting-the-status-bar-text-color-mode"></a>Ajustement de la barre Mode de couleur de texte d’état

Ce contrôle spécifique à la plateforme si le texte de la barre d’état de couleur sur un [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) est ajustée pour correspondre à la luminosité de la barre de navigation. Elle est consommée en XAML en définissant le [ `NavigationPage.StatusBarTextColorMode` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty/) à une valeur de propriété attachée la [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) énumération :

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

Vous pouvez également être utilisée à partir de c# à l’aide de l’API fluent :

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

Le `NavigationPage.On<iOS>` méthode spécifie que cette plate-forme spécifique sera exécuté uniquement sur iOS. Le [ `NavigationPage.SetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) espace de noms, les contrôles si le texte de la barre d’état de couleur sur la [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) est ajustée pour correspondre à la luminosité de la barre de navigation avec le [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) énumération fournissant deux valeurs possibles :

- [`DoNotAdjust`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust/) : indique que la barre de couleur du texte d’état ne doit pas être ajustée.
- [`MatchNavigationBarTextLuminosity`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity/) : indique que la barre de couleur du texte d’état doit correspondre à la luminosité de la barre de navigation.

En outre, le [ `GetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.GetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/) méthode peut être utilisée pour récupérer la valeur actuelle de la [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) énumération est appliquée à la [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/).

Le résultat est que l’état de la barre de couleur du texte sur un [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) peut être ajustée pour correspondre à la luminosité de la barre de navigation. Dans cet exemple, la barre des modifications de couleur de texte d’état en tant que l’utilisateur bascule entre le [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) et [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) les pages d’un [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

![](ios-images/status-bar-text-color-mode.png "Barre d’état texte couleur Mode spécifique à la plateforme")

<a name="adjust_font_size" />

## <a name="adjusting-the-font-size-of-an-entry"></a>Ajustement de la taille de police d’une entrée

Cette plateforme spécifique est utilisé pour mettre à l’échelle de la taille de police un [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) pour vous assurer que le texte entré tient dans le contrôle. Elle est consommée en XAML en définissant le [ `Entry.AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidthProperty/) propriété attachée un `boolean` valeur :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
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

Vous pouvez également être utilisée à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

entry.On<iOS>().EnableAdjustsFontSizeToFitWidth();
```

Le `Entry.On<iOS>` méthode spécifie que cette plate-forme spécifique sera exécuté uniquement sur iOS. Le [ `Entry.EnableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.EnableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) espace de noms est utilisé à l’échelle de la taille de police du texte entré pour vous assurer qu’elles s’ajustent à la [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/). En outre, le [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry/) classe dans le `Xamarin.Forms.PlatformConfiguration.iOSSpecific` espace de noms comporte également un [ `DisableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.DisableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) méthode qui désactive cette spécifique à la plateforme, et un [ `SetAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.SetAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/System.Boolean/) méthode qui peut être utilisé pour activer/désactiver la mise à l’échelle en appelant la taille de police la [ `AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) méthode :

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

Le résultat est que la taille de police de la [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) est mis à l’échelle pour vous assurer que le texte entré tient dans le contrôle :

![](ios-images/entry-font-size.png "Ajuster l’entrée police taille spécifique à la plateforme")

<a name="picker_update_mode" />

## <a name="controlling-picker-item-selection"></a>Contrôle de la sélection d’éléments de sélecteur

Cette plate-forme spécifique détermine si la sélection d’éléments se produit dans un [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), permettant à l’utilisateur spécifier que la sélection d’éléments se produit lorsque vous parcourez les éléments dans le contrôle, ou qu’une seule fois le **fait** bouton est enfoncé. Elle est consommée en XAML en définissant le `Picker.UpdateMode` à une valeur de propriété attachée le `UpdateMode` énumération :

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

Vous pouvez également être utilisée à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

Le `Picker.On<iOS>` méthode spécifie que cette plate-forme spécifique sera exécuté uniquement sur iOS. Le `Picker.SetUpdateMode` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) espace de noms, permet de contrôler en cas de sélection d’élément, avec la `UpdateMode` énumération fournissant deux valeurs possibles :

- `Immediately` -sélection d’éléments se produit lorsque l’utilisateur accède à des éléments dans le [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/). Il s’agit du comportement par défaut dans Xamarin.Forms.
- `WhenFinished` -sélection d’éléments ne se produit une fois que l’utilisateur a appuyé sur le **fait** situé dans le [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/).

En outre, le `SetUpdateMode` méthode peut être utilisée pour activer/désactiver les valeurs d’énumération en appelant le `UpdateMode` (méthode), qui renvoie l’actuel `UpdateMode`:

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

[![](ios-images/picker-updatemode.png "Sélecteur UpdateMode spécifique à la plateforme")](ios-images/picker-updatemode-large.png#lightbox "Picker UpdateMode Plaform-Specific")

<a name="set_status_bar_visibility" />

## <a name="setting-the-status-bar-visibility-on-a-page"></a>Définition de la barre d’état de visibilité sur une Page

Cette plateforme spécifique est utilisée pour définir la visibilité de la barre d’état sur un [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), et elle inclut la possibilité de contrôler la façon dont la barre d’état entre ou quitte le `Page`. Elle est consommée en XAML en définissant le `Page.PrefersStatusBarHidden` à une valeur de propriété attachée la `StatusBarHiddenMode` énumération et éventuellement le `Page.PreferredStatusBarUpdateAnimation` à une valeur de propriété attachée le `UIStatusBarAnimation` énumération :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersStatusBarHidden="True"
             ios:Page.PreferredStatusBarUpdateAnimation="Fade">
  ...
</ContentPage>
```

Vous pouvez également être utilisée à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersStatusBarHidden(StatusBarHiddenMode.True)
         .SetPreferredStatusBarUpdateAnimation(UIStatusBarAnimation.Fade);
```

Le `Page.On<iOS>` méthode spécifie que cette plate-forme spécifique sera exécuté uniquement sur iOS. Le `Page.SetPrefersStatusBarHidden` (méthode), dans le `Xamarin.Forms.PlatformConfiguration.iOSSpecific` espace de noms est utilisée pour définir la visibilité de la barre d’état sur un [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) en spécifiant l’une de la `StatusBarHiddenMode` valeurs d’énumération : `Default`, `True` , ou `False`. Le `StatusBarHiddenMode.True` et `StatusBarHiddenMode.False` la visibilité de barre d’état, quelle que soit l’orientation du périphérique, la valeur et le `StatusBarHiddenMode.Default` valeur masque la barre d’état dans un environnement compact verticalement.

Le résultat est que la visibilité de la barre d’état sur un [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) peuvent être définies :

![](ios-images/hide-status-bar.png "Barre d’état visibilité spécifique à la plateforme")

> [!NOTE]
> Sur un [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), spécifié `StatusBarHiddenMode` valeur d’énumération met met également à jour la barre d’état sur toutes les pages de l’enfant. Sur tous les autres [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-types dérivés, spécifiés `StatusBarHiddenMode` valeur d’énumération met à jour uniquement la barre d’état sur la page actuelle.

Le `Page.SetPreferredStatusBarUpdateAnimation` méthode est utilisée pour définir la façon dont la barre d’état entre ou quitte le [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) en spécifiant l’une de le `UIStatusBarAnimation` valeurs d’énumération : `None`, `Fade`, ou `Slide`. Si le `Fade` ou `Slide` valeur d’énumération est spécifiée, un deuxième 0,25 animation s’exécute comme la barre d’état entre ou quitte le `Page`.

<a name="delay_content_touches" />

## <a name="delaying-content-touches-in-a-scrollview"></a>Temporiser touches finales, le contenu dans un ScrollView

Un minuteur implicit est déclenché lorsqu’un mouvement tactile commence dans un [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) sur iOS et le `ScrollView` décide, en fonction de l’action de l’utilisateur dans l’intervalle du minuteur, s’il doit gérer le mouvement ou passer à son contenu. Par défaut, le fichier iOS `ScrollView` touches finales, le contenu des retards, mais cela peut entraîner des problèmes dans certaines circonstances avec le `ScrollView` contenu perdantes du mouvement quand il le devrait. Par conséquent, ce contrôle spécifique à la plateforme si un `ScrollView` gère un mouvement tactile ou passe à son contenu. Elle est consommée en XAML en définissant le `ScrollView.ShouldDelayContentTouches` propriété attachée un `boolean` valeur :

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

Vous pouvez également être utilisée à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

Le `ScrollView.On<iOS>` méthode spécifie que cette plate-forme spécifique sera exécuté uniquement sur iOS. Le `ScrollView.SetShouldDelayContentTouches` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) espace de noms est utilisée pour déterminer si un [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) gère un mouvement tactile ou passe à son contenu. En outre, le `SetShouldDelayContentTouches` méthode peut être utilisée pour activer/désactiver la retarder des touches finales, le contenu en appelant le `ShouldDelayContentTouches` retour si la touche de contenu est retardée de méthode :

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

Le résultat est qu’un [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) peut désactiver ce qui retarde la réception des touches finales, le contenu, ainsi que dans ce scénario la [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) reçoit le mouvement plutôt que la [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) page de la [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

[![](ios-images/scrollview-delay-content-touches.png "Délai de ScrollView contenu touche spécifique à la plateforme")](ios-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView Delay Content Touches Plaform-Specific")

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment consommer les iOS plateforme-spécificités qui sont intégrées dans Xamarin.Forms. Caractéristiques de la plateforme vous permettent de vous permet d’utiliser la fonctionnalité est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou les effets.


## <a name="related-links"></a>Liens associés

- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [iOSSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)
