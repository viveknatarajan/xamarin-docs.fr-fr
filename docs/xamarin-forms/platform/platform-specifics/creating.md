---
title: Création de caractéristiques de la plateforme
description: Cet article montre comment exposer un effet à l’aide spécifique à la plateforme.
ms.prod: xamarin
ms.assetid: 0D0E6274-6EF2-4D40-BB77-3D8E53BCD24B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/23/2016
ms.openlocfilehash: 1d9f07a089eabedf07bef49c9815fe7e93128f09
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997254"
---
# <a name="creating-platform-specifics"></a>Création de caractéristiques de la plateforme

_Éditeurs peuvent créer leurs propres caractéristiques de la plateforme avec des effets. Un effet fournit les fonctionnalités spécifiques, ce qui sont ensuite exposée via un spécifique à la plateforme. Le résultat est un effet plus facilement consommable via XAML et un code fluent API. Cet article montre comment exposer un effet à l’aide spécifique à la plateforme._

## <a name="overview"></a>Vue d'ensemble

Le processus de création spécifique à la plateforme est comme suit :

1. Implémenter des fonctionnalités spécifiques comme conséquence. Pour plus d’informations, consultez [création d’un effet](~/xamarin-forms/app-fundamentals/effects/creating.md).
1. Créez une classe spécifique à la plateforme qui va exposer l’effet. Pour plus d’informations, consultez [création d’une classe spécifique à la plateforme](#creating).
1. Dans la classe spécifique à la plateforme, implémentez une propriété jointe pour autoriser le spécifiques à la plateforme à être consommée à partir de XAML. Pour plus d’informations, consultez [Ajout d’une propriété attachée](#attached_property).
1. Dans la classe spécifique à la plateforme, implémentez les méthodes d’extension pour permettre la spécifiques à la plateforme à consommer via une API de code fluent. Pour plus d’informations, consultez [Ajout de méthodes d’Extension](#extension_methods).
1. Modifier l’implémentation de l’effet afin que l’effet est appliqué uniquement si la plateforme spécifique a été appelé sur la même plateforme que l’effet. Pour plus d’informations, consultez [création de l’effet](#creating_the_effect).

Le résultat d’exposer un effet que spécifiques à la plateforme est que l’effet peut être consommé plus facilement via XAML et une API de code fluent.

> [!NOTE]
> Il est prévu que les fournisseurs utilisera cette technique pour créer leurs propres caractéristiques de la plateforme, pour faciliter la consommation par les utilisateurs. Bien que les utilisateurs peuvent choisir de créer leurs propres caractéristiques de la plateforme, il convient de noter qu’il nécessite davantage de code que la création et utilisation d’un effet.

L’exemple d’application montre un `Shadow` spécifique à la plateforme qui ajoute une ombre au texte affiché par un [ `Label` ](xref:Xamarin.Forms.Label) contrôle :

![](creating-images/screenshots.png "Clichés instantanés spécifiques à la plateforme")

L’application exemple implémente le `Shadow` spécifique à la plateforme sur chaque plateforme, pour faciliter la compréhension. Toutefois, à part de chaque implémentation d’effet spécifique à la plateforme, l’implémentation de la classe de clichés instantanés est en grande partie identique pour chaque plateforme. Par conséquent, ce guide est axé sur l’implémentation de la classe de clichés instantanés et l’effet associé sur une plateforme unique.

Pour plus d’informations sur les effets, consultez [personnalisation des contrôles avec des effets](~/xamarin-forms/app-fundamentals/effects/index.md).

<a name="creating" />

## <a name="creating-a-platform-specific-class"></a>Création d’une classe spécifique à la plateforme

Spécifique à la plateforme est créé comme un `public static` classe :

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
  public static Shadow
  {
    ...
  }
}
```

Les sections suivantes décrivent l’implémentation de la `Shadow` effet spécifique à la plateforme et associé.

<a name="attached_property" />

### <a name="adding-an-attached-property"></a>Ajout d’une propriété jointe

Une propriété jointe doit être ajoutée à la `Shadow` spécifique à la plateforme pour autoriser l’utilisation via XAML :

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
    using System.Linq;
    using Xamarin.Forms;
    using Xamarin.Forms.PlatformConfiguration;
    using FormsElement = Xamarin.Forms.Label;

    public static class Shadow
    {
        const string EffectName = "MyCompany.LabelShadowEffect";

        public static readonly BindableProperty IsShadowedProperty =
            BindableProperty.CreateAttached("IsShadowed",
                                            typeof(bool),
                                            typeof(Shadow),
                                            false,
                                            propertyChanged: OnIsShadowedPropertyChanged);

        public static bool GetIsShadowed(BindableObject element)
        {
            return (bool)element.GetValue(IsShadowedProperty);
        }

        public static void SetIsShadowed(BindableObject element, bool value)
        {
            element.SetValue(IsShadowedProperty, value);
        }

        ...

        static void OnIsShadowedPropertyChanged(BindableObject element, object oldValue, object newValue)
        {
            if ((bool)newValue)
            {
                AttachEffect(element as FormsElement);
            }
            else
            {
                DetachEffect(element as FormsElement);
            }
        }

        static void AttachEffect(FormsElement element)
        {
            IElementController controller = element;
            if (controller == null || controller.EffectIsAttached(EffectName))
            {
                return;
            }
            element.Effects.Add(Effect.Resolve(EffectName));
        }

        static void DetachEffect(FormsElement element)
        {
            IElementController controller = element;
            if (controller == null || !controller.EffectIsAttached(EffectName))
            {
                return;
            }

            var toRemove = element.Effects.FirstOrDefault(e => e.ResolveId == Effect.Resolve(EffectName).ResolveId);
            if (toRemove != null)
            {
                element.Effects.Remove(toRemove);
            }
        }
    }
}
```

Le `IsShadowed` propriété jointe est utilisée pour ajouter le `MyCompany.LabelShadowEffect` effet et supprimez-le, le contrôle qui le `Shadow` classe est attachée à. Registres de la propriété jointe la `OnIsShadowedPropertyChanged` méthode qui sera exécutée lorsque la valeur de la propriété change. À son tour, cette méthode appelle la `AttachEffect` ou `DetachEffect` méthode pour ajouter ou supprimer l’effet basée sur la valeur de la `IsShadowed` propriété jointe. L’effet est ajouté ou supprimé à partir du contrôle en modifiant le contrôle [ `Effects` ](xref:Xamarin.Forms.Element.Effects) collection.

> [!NOTE]
> Notez que l’effet est résolu en spécifiant une valeur qui est une concaténation de la résolution de nom de groupe et un identificateur unique qui est spécifié sur l’implémentation de l’effet. Pour plus d’informations, consultez [création d’un effet](~/xamarin-forms/app-fundamentals/effects/creating.md).

Pour plus d’informations sur les propriétés jointes, consultez [propriétés attaché](~/xamarin-forms/xaml/attached-properties.md).

<a name="extension_methods" />

### <a name="adding-extension-methods"></a>Ajout de méthodes d’Extension

Méthodes d’extension doivent être ajoutés à la `Shadow` spécifique à la plateforme pour autoriser l’utilisation via une API de code fluent :

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
    using System.Linq;
    using Xamarin.Forms;
    using Xamarin.Forms.PlatformConfiguration;
    using FormsElement = Xamarin.Forms.Label;

    public static class Shadow
    {
        ...
        public static bool IsShadowed(this IPlatformElementConfiguration<iOS, FormsElement> config)
        {
            return GetIsShadowed(config.Element);
        }

        public static IPlatformElementConfiguration<iOS, FormsElement> SetIsShadowed(this IPlatformElementConfiguration<iOS, FormsElement> config, bool value)
        {
            SetIsShadowed(config.Element, value);
            return config;
        }
        ...
    }
}
```

Le `IsShadowed` et `SetIsShadowed` appeler la méthode get de méthodes d’extension et définir des accesseurs pour le `IsShadowed` propriété jointe, respectivement. Chaque méthode d’extension opère sur le `IPlatformElementConfiguration<iOS, FormsElement>` type, qui spécifie que la plateforme spécifique peut être appelé sur [ `Label` ](xref:Xamarin.Forms.Label) instances à partir d’iOS.

<a name="creating_the_effect" />

### <a name="creating-the-effect"></a>Création de l’effet

Le `Shadow` spécifique à la plateforme ajoute le `MyCompany.LabelShadowEffect` à un [ `Label` ](xref:Xamarin.Forms.Label)et le supprime. Le code suivant montre l’exemple le `LabelShadowEffect` implémentation pour le projet iOS :

```csharp
[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace ShadowPlatformSpecific.iOS
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            UpdateShadow();
        }

        protected override void OnDetached()
        {
        }

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (args.PropertyName == Shadow.IsShadowedProperty.PropertyName)
            {
                UpdateShadow();
            }
        }

        void UpdateShadow()
        {
            try
            {
                if (((Label)Element).OnThisPlatform().IsShadowed())
                {
                    Control.Layer.CornerRadius = 5;
                    Control.Layer.ShadowColor = UIColor.Black.CGColor;
                    Control.Layer.ShadowOffset = new CGSize(5, 5);
                    Control.Layer.ShadowOpacity = 1.0f;
                }
                else if (!((Label)Element).OnThisPlatform().IsShadowed())
                {
                    Control.Layer.ShadowOpacity = 0;
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

Le `UpdateShadow` méthode indique `Control.Layer` propriétés pour créer les clichés instantanés, à condition que le `IsShadowed` propriété jointe est définie `true`et à condition que le `Shadow` spécifique à la plateforme a été appelée sur la même plateforme qui le Effet est implémenté pour. Cette vérification est effectuée avec la `OnThisPlatform` (méthode).

Si le `Shadow.IsShadowed` attaché la valeur de propriété change lors de l’exécution, les besoins de l’effet de répondre en supprimant l’ombre. Par conséquent, une version substituée de la `OnElementPropertyChanged` méthode est utilisée pour répondre à la modification de propriété pouvant être liée en appelant le `UpdateShadow` (méthode).

Pour plus d’informations sur la création d’un effet, consultez [création d’un effet](~/xamarin-forms/app-fundamentals/effects/creating.md) et [passage de paramètres en tant que propriétés attaché effet](~/xamarin-forms/app-fundamentals/effects/passing-parameters/attached-properties.md).

## <a name="consuming-a-platform-specific"></a>Utilisation spécifique à la plateforme

Le `Shadow` spécifique à la plateforme est consommé dans XAML en définissant le `Shadow.IsShadowed` propriété jointe un `boolean` valeur :

```xaml
<ContentPage xmlns:ios="clr-namespace:MyCompany.Forms.PlatformConfiguration.iOS" ...>
  ...
  <Label Text="Label Shadow Effect" ios:Shadow.IsShadowed="true" ... />
  ...
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using MyCompany.Forms.PlatformConfiguration.iOS;

...

shadowLabel.On<iOS>().SetIsShadowed(true);
```

Pour plus d’informations sur l’utilisation des caractéristiques de la plateforme, consultez [consommation caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/consuming/index.md).

## <a name="summary"></a>Récapitulatif

Cet article a montré comment exposer un effet à l’aide spécifique à la plateforme. Le résultat est un effet plus facilement consommable via XAML et un code fluent API.


## <a name="related-links"></a>Liens associés

- [ShadowPlatformSpecific (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/shadowplatformspecific/)
- [Personnalisation des contrôles avec des effets](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Propriétés jointes](~/xamarin-forms/xaml/attached-properties.md)
