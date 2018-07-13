---
title: Effet de passage de paramètres en tant que propriétés jointes
description: Propriétés jointes peuvent être utilisées pour définir les paramètres d’effet qui répondent aux modifications apportées aux propriétés de runtime. Cet article montre à l’aide de propriétés jointes à transmettre des paramètres à un effet et la modification d’un paramètre lors de l’exécution.
ms.prod: xamarin
ms.assetid: DFCDCB9F-17DD-4117-BD53-B4FB206BB387
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 9483e424a74a88ce3f0eb49624bb5315551f2062
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996449"
---
# <a name="passing-effect-parameters-as-attached-properties"></a>Effet de passage de paramètres en tant que propriétés jointes

_Propriétés jointes peuvent être utilisées pour définir les paramètres d’effet qui répondent aux modifications apportées aux propriétés de runtime. Cet article montre à l’aide de propriétés jointes à transmettre des paramètres à un effet et la modification d’un paramètre lors de l’exécution._

Le processus de création de paramètres d’effet qui répondent aux modifications apportées aux propriétés de runtime est comme suit :

1. Créer un `static` classe qui contient une propriété jointe pour chaque paramètre à passer à l’effet.
1. Ajoutez une propriété jointe supplémentaire à la classe qui permet de contrôler l’ajout ou la suppression de l’effet pour le contrôle à associer à la classe. Assurez-vous que cela attaché propriété registres un `propertyChanged` délégué qui sera exécutée lorsque la valeur de la propriété change.
1. Créer `static` accesseurs Get et Set pour chacun de propriété jointe.
1. Implémenter une logique dans le `propertyChanged` délégué à ajouter et supprimer l’effet.
1. Implémenter une classe imbriquée à l’intérieur de la `static` (classe), nommé d’après l’effet, que les sous-classes la `RoutingEffect` classe. Pour le constructeur, appelez le constructeur de classe de base, en passant une concaténation de la résolution de nom de groupe et l’ID unique qui a été spécifiée sur chaque classe de l’effet de spécifique à la plateforme.

Paramètres peuvent ensuite être transmis à l’effet en ajoutant des propriétés jointes, les valeurs de propriété pour le contrôle approprié. En outre, les paramètres peuvent être modifiés lors de l’exécution en spécifiant une nouvelle valeur de propriété jointe.

> [!NOTE]
> Une propriété jointe est un type spécial de propriété pouvant être liée, définie dans une classe mais jointe à d’autres objets et reconnaissable dans XAML en tant qu’attributs qui contiennent une classe et un nom de propriété séparés par un point. Pour plus d’informations, consultez [propriétés attaché](~/xamarin-forms/xaml/attached-properties.md).

L’exemple d’application montre un `ShadowEffect` qui ajoute une ombre au texte affiché par un [ `Label` ](xref:Xamarin.Forms.Label) contrôle. En outre, la couleur de l’ombre peut être modifiée lors de l’exécution. Le diagramme suivant illustre les responsabilités de chaque projet dans l’exemple d’application, ainsi que les relations entre eux :

![](attached-properties-images/shadow-effect.png "Responsabilités de projet effet de clichés instantanés")

Un [ `Label` ](xref:Xamarin.Forms.Label) control sur le `HomePage` personnalisé par le `LabelShadowEffect` dans chaque projet spécifique à la plateforme. Les paramètres sont passés à chaque `LabelShadowEffect` par le biais des propriétés jointes la `ShadowEffect` classe. Chaque `LabelShadowEffect` classe dérive de la `PlatformEffect` classe pour chaque plateforme. Il en résulte une ombre ajoutée au texte affiché par le `Label` contrôler, comme indiqué dans les captures d’écran suivante :

![](attached-properties-images/screenshots.png "Effet d’ombre portée sur chaque plateforme")

## <a name="creating-effect-parameters"></a>Création de paramètres de l’effet

Un `static` classe doit être créée pour représenter les paramètres de l’effet, comme illustré dans l’exemple de code suivant :

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

Le `ShadowEffect` contient cinq propriétés jointes, avec `static` accesseurs Get et Set pour chacun de propriété jointe. Quatre de ces propriétés représentent des paramètres à passer à chaque spécifiques à la plateforme `LabelShadowEffect`. Le `ShadowEffect` classe définit également un `HasShadow` attaché de propriété qui est utilisée pour contrôler l’ajout ou la suppression de l’effet au contrôle qui le `ShadowEffect` classe est attachée à. Registres de la propriété jointe la `OnHasShadowChanged` méthode qui sera exécutée lorsque la valeur de la propriété change. Cette méthode ajoute ou supprime l’effet en fonction de la valeur de la `HasShadow` propriété jointe.

Imbriqué `LabelShadowEffect` classe qui sous-classe le [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) classe prend en charge l’effet Ajout et la suppression. Le `RoutingEffect` classe représente un effet indépendant de la plateforme qui encapsule un effet interne qui est généralement spécifiques à la plateforme. Cela simplifie le processus de suppression d’effet, car il n’existe aucun accès lors de la compilation pour les informations de type pour un effet spécifique à la plateforme. Le `LabelShadowEffect` constructeur appelle le constructeur de classe de base, en passant un paramètre comportant une concaténation de la résolution de nom de groupe et l’ID unique qui a été spécifiée sur chaque classe de l’effet de spécifique à la plateforme. Cela permet l’ajout de l’effet et la suppression dans la `OnHasShadowChanged` méthode, comme suit :

- **Ajout d’effet** – une nouvelle instance de la `LabelShadowEffect` est ajouté pour le contrôle [ `Effects` ](xref:Xamarin.Forms.Element.Effects) collection. Cela remplace l’utilisation de la [ `Effect.Resolve` ](xref:Xamarin.Forms.Effect.Resolve(System.String)) méthode pour ajouter l’effet.
- **Suppression d’effet** : la première instance de la `LabelShadowEffect` dans le contrôle [ `Effects` ](xref:Xamarin.Forms.Element.Effects) collection est récupérée et supprimée.

## <a name="consuming-the-effect"></a>Utilisation de l’effet

Chaque spécifiques à la plateforme `LabelShadowEffect` peuvent être consommées en ajoutant les propriétés jointes à un [ `Label` ](xref:Xamarin.Forms.Label) contrôler, comme illustré dans l’exemple de code XAML suivant :

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

L’équivalent [ `Label` ](xref:Xamarin.Forms.Label) en c# est illustré dans l’exemple de code suivant :

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

Définissant le `ShadowEffect.HasShadow` propriété jointe `true` exécute le `ShadowEffect.OnHasShadowChanged` méthode qui ajoute ou supprime le `LabelShadowEffect` à la [ `Label` ](xref:Xamarin.Forms.Label) contrôle. Dans les deux exemples de code, le `ShadowEffect.Color` propriété jointe fournit des valeurs de couleur de la plateforme spécifique. Pour plus d’informations, consultez [classe de périphérique](~/xamarin-forms/platform/device.md).

En outre, un [ `Button` ](xref:Xamarin.Forms.Button) permet la couleur d’ombrage d’être modifié lors de l’exécution. Lorsque le `Button` est activé, le code suivant change la couleur de l’ombre en définissant le `ShadowEffect.Color` propriété jointe :

```csharp
ShadowEffect.SetColor (label, Color.Teal);
```

### <a name="consuming-the-effect-with-a-style"></a>Utilisation de l’effet avec un Style

Effets qui peuvent être consommées en ajoutant des propriétés jointes à un contrôle peuvent également être utilisées par un style. L’exemple de code XAML suivant montre une *explicite* style pour l’effet d’ombre, qui peut être appliqué à [ `Label` ](xref:Xamarin.Forms.Label) contrôles :

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

Le [ `Style` ](xref:Xamarin.Forms.Style) peut être appliqué à un [ `Label` ](xref:Xamarin.Forms.Label) en définissant son [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) propriété le `Style` instance à l’aide de la `StaticResource`extension de balisage, comme illustré dans l’exemple de code suivant :

```xaml
<Label Text="Label Shadow Effect" ... Style="{StaticResource ShadowEffectStyle}" />
```

Pour plus d’informations sur les styles, consultez [Styles](~/xamarin-forms/user-interface/styles/index.md).

## <a name="creating-the-effect-on-each-platform"></a>Création de l’effet sur chaque plateforme

Les sections suivantes décrivent l’implémentation spécifique à la plateforme de la `LabelShadowEffect` classe.

### <a name="ios-project"></a>Projet iOS

Le code suivant montre l’exemple le `LabelShadowEffect` implémentation pour le projet iOS :

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

Le `OnAttached` méthode appelle les méthodes qui récupèrent les valeurs de propriété jointe à l’aide de la `ShadowEffect` accesseurs Get et que définir `Control.Layer` propriétés aux valeurs de propriété pour créer l’ombre. Cette fonctionnalité est encapsulée dans un `try` / `catch` bloquer au cas où le contrôle associé à l’effet n’a pas la `Control.Layer` propriétés. Aucune implémentation n’est fournie par le `OnDetached` (méthode), car aucun nettoyage n’est nécessaire.

#### <a name="responding-to-property-changes"></a>Répondre aux modifications de propriétés

Si un de la `ShadowEffect` attaché de modification des valeurs de propriété lors de l’exécution, l’effet doit répondre en affichant les modifications. Une version substituée de la `OnElementPropertyChanged` , dans la classe spécifique à la plateforme effet, est le lieu pour répondre aux modifications de propriété pouvant être liée, comme illustré dans l’exemple de code suivant :

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

Le `OnElementPropertyChanged` méthode met à jour le radius, la couleur ou le décalage de l’ombre, à condition que le texte approprié `ShadowEffect` valeur de propriété jointe a changé. Une vérification de la propriété est modifiée doit toujours être effectuée, comme ce remplacement peut être appelé plusieurs fois.

### <a name="android-project"></a>Projet Android

Le code suivant montre l’exemple le `LabelShadowEffect` implémentation pour le projet Android :

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

Le `OnAttached` méthode appelle les méthodes qui récupèrent les valeurs de propriété jointe à l’aide de la `ShadowEffect` accesseurs Get et appelle une méthode qui appelle le [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) méthode pour créer une ombre en utilisant les valeurs de propriété. Cette fonctionnalité est encapsulée dans un `try` / `catch` bloquer au cas où le contrôle associé à l’effet n’a pas la `Control.Layer` propriétés. Aucune implémentation n’est fournie par le `OnDetached` (méthode), car aucun nettoyage n’est nécessaire.

#### <a name="responding-to-property-changes"></a>Répondre aux modifications de propriétés

Si un de la `ShadowEffect` attaché de modification des valeurs de propriété lors de l’exécution, l’effet doit répondre en affichant les modifications. Une version substituée de la `OnElementPropertyChanged` , dans la classe spécifique à la plateforme effet, est le lieu pour répondre aux modifications de propriété pouvant être liée, comme illustré dans l’exemple de code suivant :

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

Le `OnElementPropertyChanged` méthode met à jour le radius, la couleur ou le décalage de l’ombre, à condition que le texte approprié `ShadowEffect` valeur de propriété jointe a changé. Une vérification de la propriété est modifiée doit toujours être effectuée, comme ce remplacement peut être appelé plusieurs fois.

### <a name="universal-windows-platform-project"></a>Projet de plateforme Windows universelle

Le code suivant montre l’exemple le `LabelShadowEffect` implémentation pour le projet de plateforme universelle Windows (UWP) :

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

La plateforme Windows universelle ne fournit pas un effet d’ombre et donc la `LabelShadowEffect` implémentation sur les deux plateformes simule un en ajoutant un deuxième offset [ `Label` ](xref:Xamarin.Forms.Label) derrière le réplica principal `Label`. Le `OnAttached` méthode crée le nouveau `Label` et définit certaines propriétés de disposition sur le `Label`. Il appelle ensuite les méthodes qui récupèrent les valeurs de propriété jointe à l’aide de la `ShadowEffect` accesseurs Get et crée l’ombre en définissant le [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor), [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX)et [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) propriétés afin de contrôler la couleur et l’emplacement de la `Label`. Le `shadowLabel` est alors inséré décalage derrière le réplica principal `Label`. Cette fonctionnalité est encapsulée dans un `try` / `catch` bloquer au cas où le contrôle associé à l’effet n’a pas la `Control.Layer` propriétés. Aucune implémentation n’est fournie par le `OnDetached` (méthode), car aucun nettoyage n’est nécessaire.

#### <a name="responding-to-property-changes"></a>Répondre aux modifications de propriétés

Si un de la `ShadowEffect` attaché de modification des valeurs de propriété lors de l’exécution, l’effet doit répondre en affichant les modifications. Une version substituée de la `OnElementPropertyChanged` , dans la classe spécifique à la plateforme effet, est le lieu pour répondre aux modifications de propriété pouvant être liée, comme illustré dans l’exemple de code suivant :

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

Le `OnElementPropertyChanged` méthode met à jour la couleur ou le décalage de l’ombre, à condition que le texte approprié `ShadowEffect` valeur de propriété jointe a changé. Une vérification de la propriété est modifiée doit toujours être effectuée, comme ce remplacement peut être appelé plusieurs fois.

## <a name="summary"></a>Récapitulatif

Cet article a montré à l’aide de propriétés jointes à transmettre des paramètres à un effet et la modification d’un paramètre lors de l’exécution. Propriétés jointes peuvent être utilisées pour définir les paramètres d’effet qui répondent aux modifications apportées aux propriétés de runtime.


## <a name="related-links"></a>Liens associés

- [Renderers personnalisés](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Effet](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [RoutingEffect](xref:Xamarin.Forms.RoutingEffect)
- [Effet d’ombre portée (exemple)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffectruntimechange/)
