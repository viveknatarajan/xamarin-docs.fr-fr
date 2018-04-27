---
title: Passage de paramètres effet en tant que propriétés jointes
description: Propriétés jointes peuvent être utilisées pour définir les paramètres d’effet qui répondent aux modifications de propriétés d’exécution. Cet article décrit à l’aide de propriétés jointes à passer des paramètres à un effet et la modification d’un paramètre lors de l’exécution.
ms.prod: xamarin
ms.assetid: DFCDCB9F-17DD-4117-BD53-B4FB206BB387
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 5bca36189100942e21d1d750dd156dab0cf45fc4
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2018
---
# <a name="passing-effect-parameters-as-attached-properties"></a>Passage de paramètres effet en tant que propriétés jointes

_Propriétés jointes peuvent être utilisées pour définir les paramètres d’effet qui répondent aux modifications de propriétés d’exécution. Cet article décrit à l’aide de propriétés jointes à passer des paramètres à un effet et la modification d’un paramètre lors de l’exécution._

Le processus de création de paramètres d’effet qui répondent aux modifications apportées aux propriétés de runtime est la suivante :

1. Créer un `static` classe qui contient une propriété jointe pour chaque paramètre à passer à l’effet.
1. Ajoutez une propriété jointe supplémentaire à la classe qui permet de contrôler l’ajout ou la suppression de l’effet sur le contrôle qui est attachée à la classe. Assurez-vous que les registres de propriété jointe un `propertyChanged` délégué qui sera exécutée lorsque la valeur de la propriété change.
1. Créer `static` accesseurs Get et Set pour chacun de propriété jointe.
1. Implémenter la logique dans le `propertyChanged` délégué à ajouter et supprimer l’effet.
1. Implémenter une classe imbriquée dans la `static` (classe), nommé d’après l’effet, les sous-classes la `RoutingEffect` classe. Pour le constructeur, appelez le constructeur de classe de base, en passant une concaténation de la résolution de nom de groupe et l’ID unique qui a été spécifié sur chaque classe de l’effet de la plateforme spécifique.

Paramètres peuvent ensuite être passés à l’effet en ajoutant des propriétés jointes, les valeurs de propriété pour le contrôle approprié. En outre, les paramètres peuvent être modifiés lors de l’exécution en spécifiant une nouvelle valeur de propriété jointe.

> [!NOTE]
> Une propriété jointe est un type spécial de propriété pouvant être liée, définie dans une classe mais jointe à d’autres objets et reconnaissable dans XAML en tant qu’attributs qui contiennent une classe et un nom de propriété séparés par un point. Pour plus d’informations, consultez [propriétés rattaché](~/xamarin-forms/xaml/attached-properties.md).

L’exemple d’application montre un `ShadowEffect` qui ajoute une ombre au texte affiché par un [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) contrôle. En outre, la couleur de l’ombre peut être modifiée pendant l’exécution. Le diagramme suivant illustre les responsabilités de chaque projet dans l’exemple d’application, ainsi que les relations entre eux :

![](attached-properties-images/shadow-effect.png "Responsabilités de projet effet de clichés instantanés")

A [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) control sur la `HomePage` est personnalisé en le `LabelShadowEffect` dans chaque projet spécifique à la plateforme. Les paramètres sont passés à chaque `LabelShadowEffect` par le biais des propriétés attachées la `ShadowEffect` classe. Chaque `LabelShadowEffect` classe dérive de la `PlatformEffect` classe pour chaque plateforme. Cela entraîne une ombre ajoutée au texte affiché par le `Label` de contrôle, comme indiqué dans les captures d’écran suivants :

![](attached-properties-images/screenshots.png "Effet d’ombre portée sur chaque plateforme")

## <a name="creating-effect-parameters"></a>Création de paramètres d’effet

A `static` classe doit être créée pour représenter les paramètres de l’effet, comme illustré dans l’exemple de code suivant :

```csharp
public static class ShadowEffect
{
  public static readonly BindableProperty HasShadowProperty =
    BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false, propertyChanged: OnHasShadowChanged);
  public static readonly BindableProperty ColorProperty =
    BindableProperty.CreateAttached ("Color", typeof(Color), typeof(ShadowEffect), Color.Default);
  public static readonly BindableProperty RadiusProperty =
    BindableProperty.CreateAttached ("Radius", typeof(double), typeof(ShadowEffect), 1.0);
  public static readonly BindableProperty DistanceXProperty =
    BindableProperty.CreateAttached ("DistanceX", typeof(double), typeof(ShadowEffect), 0.0);
  public static readonly BindableProperty DistanceYProperty =
    BindableProperty.CreateAttached ("DistanceY", typeof(double), typeof(ShadowEffect), 0.0);

  public static bool GetHasShadow (BindableObject view)
  {
    return (bool)view.GetValue (HasShadowProperty);
  }

  public static void SetHasShadow (BindableObject view, bool value)
  {
    view.SetValue (HasShadowProperty, value);
  }
  ...

  static void OnHasShadowChanged (BindableObject bindable, object oldValue, object newValue)
  {
    var view = bindable as View;
    if (view == null) {
      return;
    }

    bool hasShadow = (bool)newValue;
    if (hasShadow) {
      view.Effects.Add (new LabelShadowEffect ());
    } else {
      var toRemove = view.Effects.FirstOrDefault (e => e is LabelShadowEffect);
      if (toRemove != null) {
        view.Effects.Remove (toRemove);
      }
    }
  }

  class LabelShadowEffect : RoutingEffect
  {
    public LabelShadowEffect () : base ("MyCompany.LabelShadowEffect")
    {
    }
  }
}
```

Le `ShadowEffect` contient cinq propriétés jointes, avec `static` accesseurs Get et Set pour chacun de propriété jointe. Quatre de ces propriétés représentent des paramètres à passer à chaque plate-forme spécifique `LabelShadowEffect`. Le `ShadowEffect` classe définit également un `HasShadow` jointe de propriété qui est utilisée pour contrôler l’ajout ou la suppression de l’effet sur le contrôle qui le `ShadowEffect` classe est attachée à. Registres de la propriété jointe la `OnHasShadowChanged` méthode qui sera exécutée lorsque la valeur de la propriété change. Cette méthode ajoute ou supprime l’effet en fonction de la valeur de la `HasShadow` propriété jointe.

Imbriqué `LabelShadowEffect` classe qui sous-classe le [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) classe prend en charge l’effet Ajout et la suppression. La `RoutingEffect` classe représente un effet indépendant de la plateforme qui encapsule un effet interne qui est généralement spécifique à la plateforme. Cela simplifie le processus de suppression d’effet, car il n’existe pas lors de la compilation d’accès aux informations de type pour un effet spécifique à la plateforme. Le `LabelShadowEffect` constructeur appelle le constructeur de classe de base, en passant un paramètre comportant une concaténation de la résolution de nom de groupe et l’ID unique qui a été spécifié sur chaque classe de l’effet de la plateforme spécifique. Cela permet l’ajout de l’effet et la suppression de la `OnHasShadowChanged` méthode, comme suit :

- **Effet Ajout** – une nouvelle instance de la `LabelShadowEffect` est ajouté au contrôle [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) collection. Cela remplace l’utilisation de la [ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/) méthode pour ajouter l’effet.
- **Effet de la suppression** : la première instance de la `LabelShadowEffect` dans du contrôle [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) collection est récupérée et supprimée.

## <a name="consuming-the-effect"></a>Utilisation de l’effet

Chaque spécifique à la plateforme `LabelShadowEffect` peuvent être consommées en ajoutant les propriétés attachées à un [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) contrôler, comme illustré dans l’exemple de code XAML suivant :

```xaml
<Label Text="Label Shadow Effect" ...
       local:ShadowEffect.HasShadow="true" local:ShadowEffect.Radius="5"
       local:ShadowEffect.DistanceX="5" local:ShadowEffect.DistanceY="5">
  <local:ShadowEffect.Color>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="Black" />
        <On Platform="Android" Value="White" />
        <On Platform="UWP" Value="Red" />
    </OnPlatform>
  </local:ShadowEffect.Color>
</Label>
```

L’équivalent [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) en c# est indiqué dans l’exemple de code suivant :

```csharp
var label = new Label {
  Text = "Label Shadow Effect",
  ...
};

Color color = Color.Default;
switch (Device.RuntimePlatform)
{
    case Device.iOS:
        color = Color.Black;
        break;
    case Device.Android:
        color = Color.White;
        break;
    case Device.UWP:
        color = Color.Red;
        break;
}

ShadowEffect.SetHasShadow (label, true);
ShadowEffect.SetRadius (label, 5);
ShadowEffect.SetDistanceX (label, 5);
ShadowEffect.SetDistanceY (label, 5);
ShadowEffect.SetColor (label, color));
```

Définissant le `ShadowEffect.HasShadow` propriété attachée `true` exécute le `ShadowEffect.OnHasShadowChanged` méthode qui ajoute ou supprime le `LabelShadowEffect` à la [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) contrôle. Dans les deux exemples de code, le `ShadowEffect.Color` propriété attachée fournit des valeurs de couleur de la plateforme spécifique. Pour plus d’informations, consultez [Device Class](~/xamarin-forms/platform/device.md).

En outre, un [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) permet la couleur d’ombrage être modifié pendant l’exécution. Lorsque le `Button` est activé, le code suivant change la couleur de l’ombre en définissant le `ShadowEffect.Color` propriété attachée :

```csharp
ShadowEffect.SetColor (label, Color.Teal);
```

### <a name="consuming-the-effect-with-a-style"></a>Utilisation de l’effet avec un Style

Effets qui peuvent être utilisés en ajoutant des propriétés jointes à un contrôle peuvent également être utilisés par un style. L’exemple de code XAML suivant montre une *explicite* style pour l’effet d’ombre portée, qui peut être appliqué aux [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) contrôles :

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="True" />
    <Setter Property="local:ShadowEffect.Radius" Value="5" />
    <Setter Property="local:ShadowEffect.DistanceX" Value="5" />
    <Setter Property="local:ShadowEffect.DistanceY" Value="5" />
  </Style.Setters>
</Style>
```

Le [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) peuvent être appliquées à un [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) en définissant son [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) propriété le `Style` instance à l’aide de la `StaticResource`extension de balisage, comme illustré dans l’exemple de code suivant :

```xaml
<Label Text="Label Shadow Effect" ... Style="{StaticResource ShadowEffectStyle}" />
```

Pour plus d’informations sur les styles, consultez [Styles](~/xamarin-forms/user-interface/styles/index.md).

## <a name="creating-the-effect-on-each-platform"></a>Création de l’effet sur chaque plateforme

Les sections suivantes décrivent l’implémentation spécifique à la plateforme de la `LabelShadowEffect` classe.

### <a name="ios-project"></a>iOS projet

Le code suivant exemple illustre le `LabelShadowEffect` implémentation pour le projet iOS :

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.iOS
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached ()
        {
            try {
                UpdateRadius ();
                UpdateColor ();
                UpdateOffset ();
                Control.Layer.ShadowOpacity = 1.0f;
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateRadius ()
        {
            Control.Layer.CornerRadius = (nfloat)ShadowEffect.GetRadius (Element);
        }

        void UpdateColor ()
        {
            Control.Layer.ShadowColor = ShadowEffect.GetColor (Element).ToCGColor ();
        }

        void UpdateOffset ()
        {
            Control.Layer.ShadowOffset = new CGSize (
                (double)ShadowEffect.GetDistanceX (Element),
                (double)ShadowEffect.GetDistanceY (Element));
        }
    }
```

Le `OnAttached` méthode appelle les méthodes qui récupèrent les valeurs de propriété jointe à l’aide de la `ShadowEffect` accesseurs Get et qui `Control.Layer` propriétés aux valeurs de propriété pour créer l’ombre. Cette fonctionnalité est encapsulée dans un `try` / `catch` bloquer au cas où le contrôle associé à l’effet n’a pas la `Control.Layer` propriétés. Aucune implémentation n’est fournie par le `OnDetached` (méthode), car aucun nettoyage n’est nécessaire.

#### <a name="responding-to-property-changes"></a>Répondre aux modifications de propriétés

Si une de la `ShadowEffect` jointe la modification des valeurs de propriété lors de l’exécution, l’effet doit répondre en affichant les modifications. Une version substituée de la `OnElementPropertyChanged` , dans la classe spécifique à la plateforme effet, est l’endroit pour répondre aux modifications de propriété pouvant être liée, comme illustré dans l’exemple de code suivant :

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.RadiusProperty.PropertyName) {
      UpdateRadius ();
    } else if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
               args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
    }
  }
  ...
}
```

Le `OnElementPropertyChanged` méthode met à jour le rayon, la couleur ou le décalage de l’ombre, à condition qu’approprié `ShadowEffect` valeur de la propriété jointe a été modifiée. Une vérification de la propriété est modifiée doit toujours être rendue, comme ce remplacement peut être appelé plusieurs fois.

### <a name="android-project"></a>Projet Android

Le code suivant exemple illustre le `LabelShadowEffect` implémentation pour le projet Android :

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.Droid
{
    public class LabelShadowEffect : PlatformEffect
    {
        Android.Widget.TextView control;
        Android.Graphics.Color color;
        float radius, distanceX, distanceY;

        protected override void OnAttached ()
        {
            try {
                control = Control as Android.Widget.TextView;
                UpdateRadius ();
                UpdateColor ();
                UpdateOffset ();
                UpdateControl ();
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateControl ()
        {
            if (control != null) {
                control.SetShadowLayer (radius, distanceX, distanceY, color);
            }
        }

        void UpdateRadius ()
        {
            radius = (float)ShadowEffect.GetRadius (Element);
        }

        void UpdateColor ()
        {
            color = ShadowEffect.GetColor (Element).ToAndroid ();
        }

        void UpdateOffset ()
        {
            distanceX = (float)ShadowEffect.GetDistanceX (Element);
            distanceY = (float)ShadowEffect.GetDistanceY (Element);
        }
    }
```

Le `OnAttached` méthode appelle les méthodes qui récupèrent les valeurs de propriété jointe à l’aide de la `ShadowEffect` accesseurs Get et appelle une méthode qui appelle le [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) méthode pour créer une ombre à l’aide des valeurs de propriété. Cette fonctionnalité est encapsulée dans un `try` / `catch` bloquer au cas où le contrôle associé à l’effet n’a pas la `Control.Layer` propriétés. Aucune implémentation n’est fournie par le `OnDetached` (méthode), car aucun nettoyage n’est nécessaire.

#### <a name="responding-to-property-changes"></a>Répondre aux modifications de propriétés

Si une de la `ShadowEffect` jointe la modification des valeurs de propriété lors de l’exécution, l’effet doit répondre en affichant les modifications. Une version substituée de la `OnElementPropertyChanged` , dans la classe spécifique à la plateforme effet, est l’endroit pour répondre aux modifications de propriété pouvant être liée, comme illustré dans l’exemple de code suivant :

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.RadiusProperty.PropertyName) {
      UpdateRadius ();
      UpdateControl ();
    } else if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
      UpdateControl ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
               args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
      UpdateControl ();
    }
  }
  ...
}
```

Le `OnElementPropertyChanged` méthode met à jour le rayon, la couleur ou le décalage de l’ombre, à condition qu’approprié `ShadowEffect` valeur de la propriété jointe a été modifiée. Une vérification de la propriété est modifiée doit toujours être rendue, comme ce remplacement peut être appelé plusieurs fois.

### <a name="universal-windows-platform-project"></a>Projet pour la plateforme Windows universelle

Le code suivant exemple illustre le `LabelShadowEffect` implémentation pour le projet de plateforme Windows universelle (UWP) :

```csharp
[assembly: ResolutionGroupName ("MyCompany")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.UWP
{
    public class LabelShadowEffect : PlatformEffect
    {
        Label shadowLabel;
        bool shadowAdded = false;

        protected override void OnAttached ()
        {
            try {
                if (!shadowAdded) {
                    var textBlock = Control as Windows.UI.Xaml.Controls.TextBlock;

                    shadowLabel = new Label ();
                    shadowLabel.Text = textBlock.Text;
                    shadowLabel.FontAttributes = FontAttributes.Bold;
                    shadowLabel.HorizontalOptions = LayoutOptions.Center;
                    shadowLabel.VerticalOptions = LayoutOptions.CenterAndExpand;

                    UpdateColor ();
                    UpdateOffset ();

                    ((Grid)Element.Parent).Children.Insert (0, shadowLabel);
                    shadowAdded = true;
                }
            } catch (Exception ex) {
                Debug.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateColor ()
        {
            shadowLabel.TextColor = ShadowEffect.GetColor (Element);
        }

        void UpdateOffset ()
        {
            shadowLabel.TranslationX = ShadowEffect.GetDistanceX (Element);
            shadowLabel.TranslationY = ShadowEffect.GetDistanceY (Element);
        }
    }
}
```

La plateforme Windows universelle ne fournit pas un effet d’ombre et donc la `LabelShadowEffect` mise en œuvre sur les deux plateformes simule un en ajoutant un deuxième offset [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) derrière le réplica principal `Label`. Le `OnAttached` méthode crée le nouvel `Label` et définit certaines propriétés de disposition sur le `Label`. Il appelle ensuite les méthodes qui récupèrent les valeurs de propriété jointe à l’aide de la `ShadowEffect` accesseurs Get et crée le cliché instantané en définissant le [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/), [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/)et [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) propriétés afin de contrôler la couleur et l’emplacement de la `Label`. Le `shadowLabel` est ensuite inséré décalage derrière le réplica principal `Label`. Cette fonctionnalité est encapsulée dans un `try` / `catch` bloquer au cas où le contrôle associé à l’effet n’a pas la `Control.Layer` propriétés. Aucune implémentation n’est fournie par le `OnDetached` (méthode), car aucun nettoyage n’est nécessaire.

#### <a name="responding-to-property-changes"></a>Répondre aux modifications de propriétés

Si une de la `ShadowEffect` jointe la modification des valeurs de propriété lors de l’exécution, l’effet doit répondre en affichant les modifications. Une version substituée de la `OnElementPropertyChanged` , dans la classe spécifique à la plateforme effet, est l’endroit pour répondre aux modifications de propriété pouvant être liée, comme illustré dans l’exemple de code suivant :

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
                      args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
    }
  }
  ...
}
```

Le `OnElementPropertyChanged` méthode met à jour la couleur ou le décalage de l’ombre, à condition qu’approprié `ShadowEffect` valeur de la propriété jointe a été modifiée. Une vérification de la propriété est modifiée doit toujours être rendue, comme ce remplacement peut être appelé plusieurs fois.

## <a name="summary"></a>Récapitulatif

Cet article a illustré à l’aide de propriétés jointes à passer des paramètres à un effet et la modification d’un paramètre lors de l’exécution. Propriétés jointes peuvent être utilisées pour définir les paramètres d’effet qui répondent aux modifications de propriétés d’exécution.


## <a name="related-links"></a>Liens associés

- [Renderers personnalisés](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Effet](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)
- [PlatformEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)
- [RoutingEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/)
- [Effet d’ombre portée (exemple)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffectruntimechange/)
