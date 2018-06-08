---
title: Création d’un effet
description: Effets simplifient la personnalisation d’un contrôle. Cet article montre comment créer un effet qui modifie la couleur d’arrière-plan du contrôle d’entrée lorsque le contrôle obtient le focus.
ms.prod: xamarin
ms.assetid: 9E2C8DB0-36A2-4F13-8E3C-A66D7021DB13
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2016
ms.openlocfilehash: d308ea4a9d4055c75ef3a4a1283b5d6a5ab24b0a
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846803"
---
# <a name="creating-an-effect"></a>Création d’un effet

_Effets simplifient la personnalisation d’un contrôle. Cet article montre comment créer un effet qui modifie la couleur d’arrière-plan du contrôle d’entrée lorsque le contrôle obtient le focus._

Le processus de création d’un effet dans chaque projet spécifique à la plateforme est comme suit :

1. Créer une sous-classe de la `PlatformEffect` classe.
1. Remplacer la `OnAttached` logique de méthode et d’écriture pour personnaliser le contrôle.
1. Remplacer la `OnDetached` méthode et écriture logique pour nettoyer la personnalisation du contrôle, si nécessaire.
1. Ajouter un [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) d’attribut à la classe de l’effet. Cet attribut définit un espace de noms large entreprise des effets, empêcher les collisions avec d’autres effets portant le même nom. Notez que cet attribut peut uniquement être appliqué qu’une seule fois par projet.
1. Ajouter un [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) d’attribut à la classe de l’effet. Cet attribut enregistre l’effet avec un ID unique utilisé par Xamarin.Forms, ainsi que le nom du groupe pour localiser l’effet avant d’appliquer à un contrôle. L’attribut accepte deux paramètres : le nom de type de l’effet et une chaîne unique qui permet de rechercher l’effet avant d’appliquer à un contrôle.

L’effet peut ensuite être consommé en le joignant au contrôle approprié.

> [!NOTE]
> Il est facultatif pour fournir un effet dans chaque projet de plateforme. Tente d’utiliser un effet quand un n’est pas enregistré retourne une valeur non null qui ne fait rien.

L’exemple d’application montre un `FocusEffect` qui modifie la couleur d’arrière-plan d’un contrôle lorsqu’il acquiert un focus. Le diagramme suivant illustre les responsabilités de chaque projet dans l’exemple d’application, ainsi que les relations entre eux :

![](creating-images/focus-effect.png "Responsabilités de projet effet le focus")

Un [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) control sur la `HomePage` est personnalisé en le `FocusEffect` classe dans chaque projet spécifique à la plateforme. Chaque `FocusEffect` classe dérive de la `PlatformEffect` classe pour chaque plateforme. Cela entraîne le `Entry` contrôler le rendu avec une couleur d’arrière-plan de plateforme spécifique, qui change lorsque le contrôle obtient le focus, comme indiqué dans les captures d’écran suivantes :

![](creating-images/screenshots-1.png "Effet sur chaque plateforme de concentrer")
![](creating-images/screenshots-2.png "concentrer effet sur chaque plateforme")

## <a name="creating-the-effect-on-each-platform"></a>Création de l’effet sur chaque plateforme

Les sections suivantes décrivent l’implémentation spécifique à la plateforme de la `FocusEffect` classe.

## <a name="ios-project"></a>iOS projet

Le code suivant exemple illustre le `FocusEffect` implémentation pour le projet iOS :

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

Le `OnAttached` méthode définit la `BackgroundColor` propriété du contrôle à marron avec la `UIColor.FromRGB` (méthode) et stocke cette couleur dans un champ. Cette fonctionnalité est encapsulée dans un `try` / `catch` bloquer au cas où le contrôle de l’effet est attaché à n’a pas un `BackgroundColor` propriété. Aucune implémentation n’est fournie par le `OnDetached` (méthode), car aucun nettoyage n’est nécessaire.

Le `OnElementPropertyChanged` remplacement répond aux modifications de propriété pouvant être liée sur le contrôle de Xamarin.Forms. Lorsque le [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/) de propriété est modifiée, le `BackgroundColor` propriété du contrôle est modifiée en blanc si le contrôle a le focus, sinon il devient violet clair. Cette fonctionnalité est encapsulée dans un `try` / `catch` bloquer au cas où le contrôle de l’effet est attaché à n’a pas un `BackgroundColor` propriété.

## <a name="android-project"></a>Projet Android

Le code suivant exemple illustre le `FocusEffect` implémentation pour le projet Android :

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

Le `OnAttached` les appels de méthode le `SetBackgroundColor` pour définir la couleur d’arrière-plan du contrôle à la lumière verte et stocke également cette couleur dans un champ. Cette fonctionnalité est encapsulée dans un `try` / `catch` bloquer au cas où le contrôle de l’effet est attaché à n’a pas un `SetBackgroundColor` propriété. Aucune implémentation n’est fournie par le `OnDetached` (méthode), car aucun nettoyage n’est nécessaire.

Le `OnElementPropertyChanged` remplacement répond aux modifications de propriété pouvant être liée sur le contrôle de Xamarin.Forms. Lorsque le [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/) de propriété est modifiée, la couleur d’arrière-plan du contrôle est modifiée en blanc si le contrôle a le focus, sinon il est modifié en vert. Cette fonctionnalité est encapsulée dans un `try` / `catch` bloquer au cas où le contrôle de l’effet est attaché à n’a pas un `BackgroundColor` propriété.

## <a name="universal-windows-platform-projects"></a>Projets de plateforme Windows universelle

Le code suivant exemple illustre le `FocusEffect` implémentation pour les projets de plateforme Windows universelle (UWP) :

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

Le `OnAttached` méthode définit la `Background` propriété du contrôle cyan et affecte le `BackgroundFocusBrush` propriété blanc. Cette fonctionnalité est encapsulée dans un `try` / `catch` bloquer au cas où le contrôle n’est attaché à l’effet ne dispose pas de ces propriétés. Aucune implémentation n’est fournie par le `OnDetached` (méthode), car aucun nettoyage n’est nécessaire.

## <a name="consuming-the-effect"></a>Utilisation de l’effet

Le processus pour l’utilisation d’un effet à partir d’une bibliothèque Xamarin.Forms .NET Standard ou d’un projet de bibliothèque partagée est comme suit :

1. Déclarez un contrôle qui sera personnalisé par l’effet.
1. Attacher l’effet au contrôle en l’ajoutant à du contrôle [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) collection.

> [!NOTE]
> Une instance d’effet peut uniquement être attachée à un seul contrôle. Par conséquent, un effet doit être résolu à deux reprises pour l’utiliser sur les deux contrôles.

## <a name="consuming-the-effect-in-xaml"></a>Utilisation de l’effet en XAML

L’exemple de code XAML suivant montre une [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôle auquel la `FocusEffect` est attachée :

```xaml
<Entry Text="Effect attached to an Entry" ...>
    <Entry.Effects>
        <local:FocusEffect />
    </Entry.Effects>
    ...
</Entry>
```

La `FocusEffect` classe dans la bibliothèque .NET Standard prend en charge la consommation d’effet en XAML et est indiquée dans l’exemple de code suivant :

```csharp
public class FocusEffect : RoutingEffect
{
    public FocusEffect () : base ("MyCompany.FocusEffect")
    {
    }
}
```

Le `FocusEffect` classe sous-classes le [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) (classe), qui représente un effet indépendant de la plateforme qui encapsule un effet interne qui est généralement spécifique à la plateforme. Le `FocusEffect` classe appelle le constructeur de classe de base, en passant un paramètre comportant une concaténation du nom du groupe de résolution (spécifié à l’aide de la [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) attribut sur la classe effet), et l’ID unique qui a été spécifié à l’aide de la [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) attribut sur la classe de l’effet. Par conséquent, lorsque le [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) est initialisé lors de l’exécution, une nouvelle instance de la `MyCompany.FocusEffect` est ajouté au contrôle [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) collection.

Effets peuvent également être joint à des contrôles à l’aide d’un comportement ou à l’aide des propriétés joint. Pour plus d’informations sur l’attachement d’un effet à un contrôle à l’aide d’un comportement, consultez [réutilisable EffectBehavior](~/xamarin-forms/app-fundamentals/behaviors/reusable/effect-behavior.md). Pour plus d’informations sur l’attachement d’un effet à un contrôle à l’aide des propriétés jointes, consultez [passage de paramètres à un effet](~/xamarin-forms/app-fundamentals/effects/passing-parameters/index.md).

## <a name="consuming-the-effect-in-cnum"></a>Utilisation de l’effet en C&num;

L’équivalent [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) en c# est indiqué dans l’exemple de code suivant :

```csharp
var entry = new Entry {
  Text = "Effect attached to an Entry",
  ...
};
```

Le `FocusEffect` est attaché à la `Entry` en ajoutant l’effet du contrôle [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) collection, comme illustré dans l’exemple de code suivant :

```csharp
public HomePageCS ()
{
  ...
  entry.Effects.Add (Effect.Resolve ("MyCompany.FocusEffect"));
  ...
}
```

Le [ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/) retourne un [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) pour le nom spécifié, qui est une concaténation du nom du groupe de résolution (spécifié à l’aide de la [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) attribut sur la classe effet) et l’ID unique qui a été spécifié à l’aide de la [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) attribut sur la classe de l’effet. Si une plate-forme ne fournit pas l’effet, le `Effect.Resolve` méthode retournera non -`null` valeur.

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment créer un effet qui modifie la couleur d’arrière-plan de la [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôler quand le contrôle obtient le focus.


## <a name="related-links"></a>Liens associés

- [Renderers personnalisés](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Effet](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)
- [PlatformEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)
- [Effet de couleur d’arrière-plan (exemple)](https://developer.xamarin.com/samples/xamarin-forms/effects/backgroundcoloreffect/)
- [Effet de focus (exemple)](https://developer.xamarin.com/samples/xamarin-forms/effects/focuseffect/)
