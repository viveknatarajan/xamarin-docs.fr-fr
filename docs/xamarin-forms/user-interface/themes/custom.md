---
title: Création d’un thème personnalisé Xamarin.Forms
description: Cet article explique comment créer un thème personnalisé de Xamarin.Forms pour le référencement dans une application.
ms.prod: xamarin
ms.assetid: 4FE08ADC-093F-47FA-B33C-20CF08B5D7E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 0897afeacffc89e30b28474e4530a83d05d33cd2
ms.sourcegitcommit: c77d4257f8a35c8d931538b78a2ebc2aa48c0db9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58751247"
---
# <a name="creating-a-custom-xamarinforms-theme"></a>Création d’un thème personnalisé Xamarin.Forms

![](~/media/shared/preview.png "Cette API est actuellement en version préliminaire")

Outre l’ajout d’un thème à partir d’un package Nuget (telles que la [Light](~/xamarin-forms/user-interface/themes/light.md) et [foncé](~/xamarin-forms/user-interface/themes/dark.md) thèmes), vous pouvez créer vos propres ressources des thèmes de dictionnaire qui peuvent être référencés dans votre application.

## <a name="example"></a>Exemple

Les trois `BoxView`s indiqué sur la [page de thèmes](~/xamarin-forms/user-interface/themes/index.md) se présentent en fonction de trois classes définies dans les deux thèmes téléchargeables.

Pour comprendre comment ils fonctionnent, le balisage suivant crée un style équivalent qui vous pouvez ajouter directement à votre **App.xaml**.

Remarque la `Class` attribut `Style` (par opposition à la [`x:Key`](~/xamarin-forms/user-interface/styles/inheritance.md)
attribut disponible dans les versions antérieures de Xamarin.Forms).

```xml
<ResourceDictionary>
  <!-- DEFINE ANY CONSTANTS -->
  <Color x:Key="SeparatorLineColor">#CCCCCC</Color>
  <Color x:Key="iOSDefaultTintColor">#007aff</Color>
  <Color x:Key="AndroidDefaultAccentColorColor">#1FAECE</Color>
  <OnPlatform x:TypeArguments="Color" x:Key="AccentColor">
    <On Platform="iOS" Value="{StaticResource iOSDefaultTintColor}" />
    <On Platform="Android" Value="{StaticResource AndroidDefaultAccentColorColor}" />
  </OnPlatform>
  <!--  BOXVIEW CLASSES -->
  <Style TargetType="BoxView" Class="HorizontalRule">
    <Setter Property="BackgroundColor" Value="{ StaticResource SeparatorLineColor }" />
    <Setter Property="HeightRequest" Value="1" />
  </Style>

  <Style TargetType="BoxView" Class="Circle">
    <Setter Property="BackgroundColor" Value="{ StaticResource AccentColor }" />
    <Setter Property="WidthRequest" Value="34"/>
    <Setter Property="HeightRequest" Value="34"/>
    <Setter Property="HorizontalOptions" Value="Start" />

    <Setter Property="local:ThemeEffects.Circle" Value="True" />
  </Style>

  <Style TargetType="BoxView" Class="Rounded">
    <Setter Property="BackgroundColor" Value="{ StaticResource AccentColor }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="BackgroundColor" Value="{ StaticResource AccentColor }" />

    <Setter Property="local:ThemeEffects.CornerRadius" Value="4" />
  </Style>
</ResourceDictionary>
```

Vous remarquerez que le `Rounded` classe fait référence à un effet personnalisé `CornerRadius`.
Le code de cet effet est donné ci-dessous - référencer correctement personnalisé `xmlns` doit être ajouté à la **App.xaml**d’élément racine :

```csharp
xmlns:local="clr-namespace:ThemesDemo;assembly=ThemesDemo"
```

### <a name="c-code-in-the-net-standard-library-project-or-shared-project"></a>Code C# dans le projet de bibliothèque .NET Standard ou d’un projet partagé

Le code pour la création d’un angle arrondi `BoxView` utilise [effets](~/xamarin-forms/app-fundamentals/effects/index.md).
Le rayon de l’angle est appliqué à l’aide un `BindableProperty` et est implémenté en appliquant un [effet](~/xamarin-forms/app-fundamentals/effects/index.md). L’effet nécessite du code spécifique à la plateforme dans le [iOS](#ios) et [Android](#android) projets (voir ci-dessous).

```csharp
namespace ThemesDemo
{
  public static class ThemeEffects
  {
  public static readonly BindableProperty CornerRadiusProperty =
    BindableProperty.CreateAttached("CornerRadius", typeof(double), typeof(ThemeEffects), 0.0, propertyChanged: OnChanged<CornerRadiusEffect, double>);
    private static void OnChanged<TEffect, TProp>(BindableObject bindable, object oldValue, object newValue)
              where TEffect : Effect, new()
    {
        if (!(bindable is View view))
        {
            return;
        }

        if (EqualityComparer<TProp>.Equals(newValue, default(TProp)))
        {
            var toRemove = view.Effects.FirstOrDefault(e => e is TEffect);
            if (toRemove != null)
            {
                view.Effects.Remove(toRemove);
            }
        }
        else
        {
            view.Effects.Add(new TEffect());
        }

    }
    public static void SetCornerRadius(BindableObject view, double radius)
    {
        view.SetValue(CornerRadiusProperty, radius);
    }

    public static double GetCornerRadius(BindableObject view)
    {
        return (double)view.GetValue(CornerRadiusProperty);
    }

    private class CornerRadiusEffect : RoutingEffect
    {
        public CornerRadiusEffect()
            : base("Xamarin.CornerRadiusEffect")
        {
        }
    }
  }
}
```

<a name="ios" />

### <a name="c-code-in-the-ios-project"></a>Code C# dans le projet iOS

```csharp
using System;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;
using CoreGraphics;
using Foundation;
using XFThemes;

namespace ThemesDemo.iOS
{
    public class CornerRadiusEffect : PlatformEffect
    {
        private nfloat _originalRadius;

        protected override void OnAttached()
        {
            if (Container != null)
            {
                _originalRadius = Container.Layer.CornerRadius;
                Container.ClipsToBounds = true;

                UpdateCorner();
            }
        }

        protected override void OnDetached()
        {
            if (Container != null)
            {
                Container.Layer.CornerRadius = _originalRadius;
                Container.ClipsToBounds = false;
            }
        }

        protected override void OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (args.PropertyName == ThemeEffects.CornerRadiusProperty.PropertyName)
            {
                UpdateCorner();
            }
        }

        private void UpdateCorner()
        {
            Container.Layer.CornerRadius = (nfloat)ThemeEffects.GetCornerRadius(Element);
        }
    }
}
```

<a name="android" />

### <a name="c-code-in-the-android-project"></a>Code C# dans le projet Android

```csharp
using System;
using Xamarin.Forms.Platform;
using Xamarin.Forms.Platform.Android;
using Android.Views;
using Android.Graphics;

namespace ThemesDemo.Droid
{
    public class CornerRadiusEffect : BaseEffect
    {
        private ViewOutlineProvider _originalProvider;

        protected override bool CanBeApplied()
        {
            return Container != null && Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop;
        }

        protected override void OnAttachedInternal()
        {
            _originalProvider = Container.OutlineProvider;
            Container.OutlineProvider = new CornerRadiusOutlineProvider(Element);
            Container.ClipToOutline = true;
        }

        protected override void OnDetachedInternal()
        {
            Container.OutlineProvider = _originalProvider;
            Container.ClipToOutline = false;
        }

        protected override void OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (!Attached)
            {
                return;
            }

            if (args.PropertyName == ThemeEffects.CornerRadiusProperty.PropertyName)
            {
                Container.Invalidate();
            }
        }

        private class CornerRadiusOutlineProvider : ViewOutlineProvider
        {
            private Xamarin.Forms.Element _element;

            public CornerRadiusOutlineProvider(Xamarin.Forms.Element element)
            {
                _element = element;
            }

            public override void GetOutline(Android.Views.View view, Outline outline)
            {
                var pixels =
                    (float)ThemeEffects.GetCornerRadius(_element) *
                    view.Resources.DisplayMetrics.Density;

                outline.SetRoundRect(new Rect(0, 0, view.Width, view.Height), (int)pixels);
            }
        }
    }
}
```


## <a name="summary"></a>Récapitulatif

Un thème personnalisé peut être créé en définissant des styles pour chaque contrôle qui requiert une apparence personnalisée. Plusieurs styles pour un contrôle doivent être distingués par différents `Class` attributs dans le dictionnaire de ressources, puis appliquée en définissant le `StyleClass` attribut sur le contrôle.

Un style peut également exploiter [effets](~/xamarin-forms/app-fundamentals/effects/index.md) pour personnaliser davantage l’apparence d’un contrôle.

[Styles implicites](~/xamarin-forms/user-interface/styles/implicit.md) (sans soit un `x:Key` ou `Style` attribut) continuent à être appliquées à tous les contrôles qui correspondent à la `TargetType`.
