---
title: Création d’un effet
description: Effets de simplifient la personnalisation d’un contrôle. Cet article montre comment créer un effet qui modifie la couleur d’arrière-plan du contrôle d’entrée lorsque le contrôle obtient le focus.
ms.prod: xamarin
ms.assetid: 9E2C8DB0-36A2-4F13-8E3C-A66D7021DB13
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2016
ms.openlocfilehash: b29d83999724a35293882f7b9efc0158171c4fd2
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998158"
---
# <a name="creating-an-effect"></a>Création d’un effet

_Effets de simplifient la personnalisation d’un contrôle. Cet article montre comment créer un effet qui modifie la couleur d’arrière-plan du contrôle d’entrée lorsque le contrôle obtient le focus._

Le processus de création d’un effet dans chaque projet spécifique à la plateforme est comme suit :

1. Créer une sous-classe de la `PlatformEffect` classe.
1. Remplacer le `OnAttached` (méthode) et écriture logique pour personnaliser le contrôle.
1. Remplacer le `OnDetached` (méthode) et écriture logique pour nettoyer la personnalisation des contrôles, si nécessaire.
1. Ajouter un [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) d’attribut à la classe de l’effet. Cet attribut définit un espace de noms large entreprise pour les effets, empêcher des collisions avec d’autres effets portant le même nom. Notez que cet attribut peut uniquement être appliqué qu’une seule fois par projet.
1. Ajouter un [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) d’attribut à la classe de l’effet. Cet attribut enregistre l’effet avec un ID unique utilisé par Xamarin.Forms, ainsi que le nom du groupe, pour localiser l’effet avant d’appliquer à un contrôle. L’attribut accepte deux paramètres : le nom de type de l’effet et une chaîne unique qui permet de rechercher l’effet avant d’appliquer à un contrôle.

L’effet peut ensuite être consommé en le joignant au contrôle approprié.

> [!NOTE]
> Il est facultatif pour fournir un effet dans chaque projet de plateforme. Essayez d’utiliser un effet lors d’un n’est pas enregistré retournera une valeur non null qui ne fait rien.

L’exemple d’application montre un `FocusEffect` qui change la couleur d’arrière-plan d’un contrôle lorsqu’il obtient le focus. Le diagramme suivant illustre les responsabilités de chaque projet dans l’exemple d’application, ainsi que les relations entre eux :

![](creating-images/focus-effect.png "Responsabilités de projet effet le focus")

Un [ `Entry` ](xref:Xamarin.Forms.Entry) control sur le `HomePage` personnalisé par le `FocusEffect` classe dans chaque projet spécifique à la plateforme. Chaque `FocusEffect` classe dérive de la `PlatformEffect` classe pour chaque plateforme. Il en résulte le `Entry` contrôler le rendu avec une couleur d’arrière-plan spécifique à la plateforme, qui change lorsque le contrôle obtient le focus, comme indiqué dans les captures d’écran suivante :

![](creating-images/screenshots-1.png "Effet sur chaque plateforme de concentrer")
![](creating-images/screenshots-2.png "concentrer effet sur chaque plateforme")

## <a name="creating-the-effect-on-each-platform"></a>Création de l’effet sur chaque plateforme

Les sections suivantes décrivent l’implémentation spécifique à la plateforme de la `FocusEffect` classe.

## <a name="ios-project"></a>Projet iOS

Le code suivant montre l’exemple le `FocusEffect` implémentation pour le projet iOS :

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(FocusEffect), "FocusEffect")]
namespace EffectsDemo.iOS
{
    public class FocusEffect : PlatformEffect
    {
        UIColor backgroundColor;

        protected override void OnAttached ()
        {
            try {
                Control.BackgroundColor = backgroundColor = UIColor.FromRGB (204, 153, 255);
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }

        protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged (args);

            try {
                if (args.PropertyName == "IsFocused") {
                    if (Control.BackgroundColor == backgroundColor) {
                        Control.BackgroundColor = UIColor.White;
                    } else {
                        Control.BackgroundColor = backgroundColor;
                    }
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

Le `OnAttached` méthode indique le `BackgroundColor` propriété du contrôle en violet clair avec le `UIColor.FromRGB` (méthode) et stocke également cette couleur dans un champ. Cette fonctionnalité est encapsulée dans un `try` / `catch` bloquer au cas où le contrôle de l’effet est attaché à n’a pas un `BackgroundColor` propriété. Aucune implémentation n’est fournie par le `OnDetached` (méthode), car aucun nettoyage n’est nécessaire.

Le `OnElementPropertyChanged` remplacement répond aux modifications de propriété pouvant être liée sur le contrôle de Xamarin.Forms. Lorsque le [ `IsFocused` ](xref:Xamarin.Forms.VisualElement.IsFocused) propriété change, le `BackgroundColor` propriété du contrôle est modifiée en blanc si le contrôle a le focus, sinon il est modifié en violet clair. Cette fonctionnalité est encapsulée dans un `try` / `catch` bloquer au cas où le contrôle de l’effet est attaché à n’a pas un `BackgroundColor` propriété.

## <a name="android-project"></a>Projet Android

Le code suivant montre l’exemple le `FocusEffect` implémentation pour le projet Android :

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(FocusEffect), "FocusEffect")]
namespace EffectsDemo.Droid
{
    public class FocusEffect : PlatformEffect
    {
        Android.Graphics.Color backgroundColor;

        protected override void OnAttached ()
        {
            try {
                backgroundColor = Android.Graphics.Color.LightGreen;
                Control.SetBackgroundColor (backgroundColor);

            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }

        protected override void OnElementPropertyChanged (System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged (args);
            try {
                if (args.PropertyName == "IsFocused") {
                    if (((Android.Graphics.Drawables.ColorDrawable)Control.Background).Color == backgroundColor) {
                        Control.SetBackgroundColor (Android.Graphics.Color.Black);
                    } else {
                        Control.SetBackgroundColor (backgroundColor);
                    }
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

Le `OnAttached` les appels de méthode le `SetBackgroundColor` méthode pour définir la couleur d’arrière-plan du contrôle à la lumière verte et stocke également cette couleur dans un champ. Cette fonctionnalité est encapsulée dans un `try` / `catch` bloquer au cas où le contrôle de l’effet est attaché à n’a pas un `SetBackgroundColor` propriété. Aucune implémentation n’est fournie par le `OnDetached` (méthode), car aucun nettoyage n’est nécessaire.

Le `OnElementPropertyChanged` remplacement répond aux modifications de propriété pouvant être liée sur le contrôle de Xamarin.Forms. Lorsque le [ `IsFocused` ](xref:Xamarin.Forms.VisualElement.IsFocused) propriété change, la couleur d’arrière-plan du contrôle est modifiée en blanc si le contrôle a le focus, sinon il est modifié en vert clair. Cette fonctionnalité est encapsulée dans un `try` / `catch` bloquer au cas où le contrôle de l’effet est attaché à n’a pas un `BackgroundColor` propriété.

## <a name="universal-windows-platform-projects"></a>Projets de plateforme Windows universelle

Le code suivant montre l’exemple le `FocusEffect` implémentation pour les projets de plateforme universelle Windows (UWP) :

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.UWP;

[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(FocusEffect), "FocusEffect")]
namespace EffectsDemo.UWP
{
    public class FocusEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            try
            {
                (Control as Windows.UI.Xaml.Controls.Control).Background = new SolidColorBrush(Colors.Cyan);
                (Control as FormsTextBox).BackgroundFocusBrush = new SolidColorBrush(Colors.White);
            }
            catch (Exception ex)
            {
                Debug.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached()
        {
        }
    }
}
```

Le `OnAttached` méthode indique le `Background` propriété du contrôle cyan et affecte le `BackgroundFocusBrush` propriété au blanc. Cette fonctionnalité est encapsulée dans un `try` / `catch` bloquer au cas où le contrôle de l’effet est attaché à ne dispose pas de ces propriétés. Aucune implémentation n’est fournie par le `OnDetached` (méthode), car aucun nettoyage n’est nécessaire.

## <a name="consuming-the-effect"></a>Utilisation de l’effet

Le processus pour l’utilisation d’un effet à partir d’une bibliothèque Xamarin.Forms .NET Standard ou d’un projet de bibliothèque partagée est comme suit :

1. Déclarez un contrôle personnalisé par l’effet.
1. Attacher l’effet au contrôle en l’ajoutant à du contrôle [ `Effects` ](xref:Xamarin.Forms.Element.Effects) collection.

> [!NOTE]
> Une instance de l’effet ne peut être attachée à un seul contrôle. Par conséquent, un effet doit être résolu à deux reprises pour l’utiliser sur les deux contrôles.

## <a name="consuming-the-effect-in-xaml"></a>Utilisation de l’effet dans XAML

L’exemple de code XAML suivant montre une [ `Entry` ](xref:Xamarin.Forms.Entry) contrôle vers lequel le `FocusEffect` est attaché :

```xaml
<Entry Text="Effect attached to an Entry" ...>
    <Entry.Effects>
        <local:FocusEffect />
    </Entry.Effects>
    ...
</Entry>
```

Le `FocusEffect` classe dans la bibliothèque .NET Standard prend en charge la consommation d’effet dans XAML et n’est affichée dans l’exemple de code suivant :

```csharp
public class FocusEffect : RoutingEffect
{
    public FocusEffect () : base ("MyCompany.FocusEffect")
    {
    }
}
```

Le `FocusEffect` classe sous-classes le [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) (classe), qui représente un effet indépendant de la plateforme qui encapsule un effet interne qui est généralement spécifiques à la plateforme. Le `FocusEffect` classe appelle le constructeur de classe de base, en passant un paramètre comportant une concaténation du nom du groupe de résolution (spécifié à l’aide de la [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) attribut sur la classe effet), et l’ID unique qui a été spécifié à l’aide de la [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) attribut sur la classe de l’effet. Par conséquent, lorsque le [ `Entry` ](xref:Xamarin.Forms.Entry) est initialisé lors de l’exécution, une nouvelle instance de la `MyCompany.FocusEffect` est ajouté pour le contrôle [ `Effects` ](xref:Xamarin.Forms.Element.Effects) collection.

Effets peuvent également être joint à des contrôles à l’aide d’un comportement, ou à l’aide des propriétés joint. Pour plus d’informations sur l’attachement d’un effet à un contrôle à l’aide d’un comportement, consultez [réutilisable EffectBehavior](~/xamarin-forms/app-fundamentals/behaviors/reusable/effect-behavior.md). Pour plus d’informations sur l’attachement d’un effet à un contrôle à l’aide des propriétés jointes, consultez [passage de paramètres à un effet](~/xamarin-forms/app-fundamentals/effects/passing-parameters/index.md).

## <a name="consuming-the-effect-in-cnum"></a>Utilisation de l’effet en C&num;

L’équivalent [ `Entry` ](xref:Xamarin.Forms.Entry) en c# est illustré dans l’exemple de code suivant :

```csharp
var entry = new Entry {
  Text = "Effect attached to an Entry",
  ...
};
```

Le `FocusEffect` est attaché à la `Entry` instance en ajoutant l’effet du contrôle [ `Effects` ](xref:Xamarin.Forms.Element.Effects) collection, comme illustré dans l’exemple de code suivant :

```csharp
public HomePageCS ()
{
  ...
  entry.Effects.Add (Effect.Resolve ("MyCompany.FocusEffect"));
  ...
}
```

Le [ `Effect.Resolve` ](xref:Xamarin.Forms.Effect.Resolve(System.String)) retourne un [ `Effect` ](xref:Xamarin.Forms.Effect) pour le nom spécifié, qui est une concaténation du nom du groupe de résolution (spécifié à l’aide de la [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) attribut sur la classe effet) et l’ID unique qui a été spécifié à l’aide de la [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) attribut sur la classe de l’effet. Si une plate-forme ne fournit pas l’effet, le `Effect.Resolve` méthode retournera un non -`null` valeur.

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment créer un effet qui modifie la couleur d’arrière-plan de la [ `Entry` ](xref:Xamarin.Forms.Entry) contrôler quand le contrôle obtient le focus.


## <a name="related-links"></a>Liens associés

- [Renderers personnalisés](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Effet](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [Effet de couleur d’arrière-plan (exemple)](https://developer.xamarin.com/samples/xamarin-forms/effects/backgroundcoloreffect/)
- [Effet de focus (exemple)](https://developer.xamarin.com/samples/xamarin-forms/effects/focuseffect/)
