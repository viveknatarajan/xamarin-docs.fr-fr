---
title: Effet de passage de paramètres en tant que propriétés de Runtime de langage commun
description: Propriétés du Common Language Runtime (CLR) peuvent servir pour définir les paramètres d’effet qui ne répondent pas aux modifications apportées aux propriétés de runtime. Cet article montre comment utiliser des propriétés CLR pour passer des paramètres à un effet.
ms.prod: xamarin
ms.assetid: 4B50466C-5DBD-45DD-B1E6-BE9524C92F27
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 1bb357b256a7cc6d52d1d92613f38cbf48400c4c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995766"
---
# <a name="passing-effect-parameters-as-common-language-runtime-properties"></a>Effet de passage de paramètres en tant que propriétés de Runtime de langage commun

_Propriétés du Common Language Runtime (CLR) peuvent servir pour définir les paramètres d’effet qui ne répondent pas aux modifications apportées aux propriétés de runtime. Cet article montre comment utiliser des propriétés CLR pour passer des paramètres à un effet._

Le processus de création de paramètres d’effet qui ne répondent pas aux modifications apportées aux propriétés de runtime est comme suit :

1. Créer un `public` classe qui sous-classe le [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) classe. Le `RoutingEffect` classe représente un effet indépendant de la plateforme qui encapsule un effet interne qui est généralement spécifiques à la plateforme.
1. Créez un constructeur qui appelle le constructeur de classe de base, en passant une concaténation de la résolution de nom de groupe et l’ID unique qui a été spécifiée sur chaque classe de l’effet de spécifique à la plateforme.
1. Ajouter des propriétés à la classe pour chaque paramètre à passer à l’effet.

Paramètres peuvent ensuite être transmis à l’effet en spécifiant des valeurs pour chaque propriété lors de l’instanciation de l’effet.

L’exemple d’application montre un `ShadowEffect` qui ajoute une ombre au texte affiché par un [ `Label` ](xref:Xamarin.Forms.Label) contrôle. Le diagramme suivant illustre les responsabilités de chaque projet dans l’exemple d’application, ainsi que les relations entre eux :

![](clr-properties-images/shadow-effect.png "Responsabilités de projet effet de clichés instantanés")

Un [ `Label` ](xref:Xamarin.Forms.Label) control sur le `HomePage` personnalisé par le `LabelShadowEffect` dans chaque projet spécifique à la plateforme. Les paramètres sont passés à chaque `LabelShadowEffect` via les propriétés dans la `ShadowEffect` classe. Chaque `LabelShadowEffect` classe dérive de la `PlatformEffect` classe pour chaque plateforme. Il en résulte une ombre ajoutée au texte affiché par le `Label` contrôler, comme indiqué dans les captures d’écran suivante :

![](clr-properties-images/screenshots.png "Effet d’ombre portée sur chaque plateforme")

## <a name="creating-effect-parameters"></a>Création de paramètres de l’effet

Un `public` classe qui sous-classe le [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) classe doit être créée pour représenter les paramètres de l’effet, comme illustré dans l’exemple de code suivant :

```csharp
public class ShadowEffect : RoutingEffect
{
  public float Radius { get; set; }

  public Color Color { get; set; }

  public float DistanceX { get; set; }

  public float DistanceY { get; set; }

  public ShadowEffect () : base ("MyCompany.LabelShadowEffect")
  {            
  }
}
```

Le `ShadowEffect` contient quatre propriétés qui représentent les paramètres à passer à chaque spécifiques à la plateforme `LabelShadowEffect`. Le constructeur de classe appelle le constructeur de classe de base, en passant un paramètre comportant une concaténation de la résolution de nom de groupe et l’ID unique qui a été spécifiée sur chaque classe de l’effet de spécifique à la plateforme. Par conséquent, une nouvelle instance de la `MyCompany.LabelShadowEffect` sera ajouté à un contrôle [ `Effects` ](xref:Xamarin.Forms.Element.Effects) collection lorsqu’un `ShadowEffect` est instancié.

## <a name="consuming-the-effect"></a>Utilisation de l’effet

L’exemple de code XAML suivant montre un [ `Label` ](xref:Xamarin.Forms.Label) contrôle vers lequel le `ShadowEffect` est attaché :

```xaml
<Label Text="Label Shadow Effect" ...>
  <Label.Effects>
    <local:ShadowEffect Radius="5" DistanceX="5" DistanceY="5">
      <local:ShadowEffect.Color>
        <OnPlatform x:TypeArguments="Color">
            <On Platform="iOS" Value="Black" />
            <On Platform="Android" Value="White" />
            <On Platform="UWP" Value="Red" />
        </OnPlatform>
      </local:ShadowEffect.Color>
    </local:ShadowEffect>
  </Label.Effects>
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

label.Effects.Add (new ShadowEffect {
  Radius = 5,
  Color = color,
  DistanceX = 5,
  DistanceY = 5
});
```

Dans les deux exemples de code, une instance de la `ShadowEffect` classe est instanciée avec des valeurs qui est spécifiés pour chaque propriété, avant d’être ajouté pour le contrôle [ `Effects` ](xref:Xamarin.Forms.Element.Effects) collection. Notez que le `ShadowEffect.Color` propriété utilise des valeurs de couleur de la plateforme spécifique. Pour plus d’informations, consultez [classe de périphérique](~/xamarin-forms/platform/device.md).

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
                var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                if (effect != null) {
                    Control.Layer.CornerRadius = effect.Radius;
                    Control.Layer.ShadowColor = effect.Color.ToCGColor ();
                    Control.Layer.ShadowOffset = new CGSize (effect.DistanceX, effect.DistanceY);
                    Control.Layer.ShadowOpacity = 1.0f;
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

Le `OnAttached` méthode récupère le `ShadowEffect` instance et définit `Control.Layer` propriétés aux valeurs de propriété spécifié pour créer l’ombre. Cette fonctionnalité est encapsulée dans un `try` / `catch` bloquer au cas où le contrôle associé à l’effet n’a pas la `Control.Layer` propriétés. Aucune implémentation n’est fournie par le `OnDetached` (méthode), car aucun nettoyage n’est nécessaire.

### <a name="android-project"></a>Projet Android

Le code suivant montre l’exemple le `LabelShadowEffect` implémentation pour le projet Android :

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.Droid
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached ()
        {
            try {
                var control = Control as Android.Widget.TextView;
                var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                if (effect != null) {
                    float radius = effect.Radius;
                    float distanceX = effect.DistanceX;
                    float distanceY = effect.DistanceY;
                    Android.Graphics.Color color = effect.Color.ToAndroid ();
                    control.SetShadowLayer (radius, distanceX, distanceY, color);
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

Le `OnAttached` méthode récupère le `ShadowEffect` instance et appelle le [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) méthode pour créer une ombre en utilisant les valeurs de propriété spécifiée. Cette fonctionnalité est encapsulée dans un `try` / `catch` bloquer au cas où le contrôle associé à l’effet n’a pas la `Control.Layer` propriétés. Aucune implémentation n’est fournie par le `OnDetached` (méthode), car aucun nettoyage n’est nécessaire.

### <a name="universal-windows-platform-project"></a>Projet de plateforme Windows universelle

Le code suivant montre l’exemple le `LabelShadowEffect` implémentation pour le projet de plateforme universelle Windows (UWP) :

```csharp
[assembly: ResolutionGroupName ("Xamarin")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.UWP
{
    public class LabelShadowEffect : PlatformEffect
    {
        bool shadowAdded = false;

        protected override void OnAttached ()
        {
            try {
                if (!shadowAdded) {
                    var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                    if (effect != null) {
                        var textBlock = Control as Windows.UI.Xaml.Controls.TextBlock;
                        var shadowLabel = new Label ();
                        shadowLabel.Text = textBlock.Text;
                        shadowLabel.FontAttributes = FontAttributes.Bold;
                        shadowLabel.HorizontalOptions = LayoutOptions.Center;
                        shadowLabel.VerticalOptions = LayoutOptions.CenterAndExpand;
                        shadowLabel.TextColor = effect.Color;
                        shadowLabel.TranslationX = effect.DistanceX;
                        shadowLabel.TranslationY = effect.DistanceY;

                        ((Grid)Element.Parent).Children.Insert (0, shadowLabel);
                        shadowAdded = true;
                    }
                }
            } catch (Exception ex) {
                Debug.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

La plateforme Windows universelle ne fournit pas un effet d’ombre et donc la `LabelShadowEffect` implémentation sur les deux plateformes simule un en ajoutant un deuxième offset [ `Label` ](xref:Xamarin.Forms.Label) derrière le réplica principal `Label`. Le `OnAttached` méthode récupère le `ShadowEffect` d’une instance, crée la nouvelle `Label`et définit certaines propriétés de disposition sur le `Label`. Il crée ensuite l’ombre en définissant le [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor), [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX), et [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) propriétés afin de contrôler la couleur et l’emplacement de la `Label`. Le `shadowLabel` est alors inséré décalage derrière le réplica principal `Label`. Cette fonctionnalité est encapsulée dans un `try` / `catch` bloquer au cas où le contrôle associé à l’effet n’a pas la `Control.Layer` propriétés. Aucune implémentation n’est fournie par le `OnDetached` (méthode), car aucun nettoyage n’est nécessaire.

## <a name="summary"></a>Récapitulatif

Cet article a montré à l’aide des propriétés CLR pour passer des paramètres à un effet. Propriétés CLR peuvent être utilisées pour définir les paramètres d’effet qui ne répondent pas aux modifications apportées aux propriétés de runtime.


## <a name="related-links"></a>Liens associés

- [Renderers personnalisés](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Effet](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [RoutingEffect](xref:Xamarin.Forms.RoutingEffect)
- [Effet d’ombre portée (exemple)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
