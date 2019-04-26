---
title: Curseur Thumb appuyez sur iOS
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article explique comment utiliser l’iOS spécifique à la plateforme qui permet à la propriété Slider.Value à définir en appuyant sur la barre du curseur.
ms.prod: xamarin
ms.assetid: D0915D37-9A59-4728-BB6A-FE094A661275
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 43cc87f9d319295ce65d55488e1be032ae00a697
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61082193"
---
# <a name="slider-thumb-tap-on-ios"></a>Curseur Thumb appuyez sur iOS

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Cette spécifiques à la plateforme iOS permet la [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) propriété à définir en appuyant sur une position sur le [ `Slider` ](xref:Xamarin.Forms.Slider) barre, plutôt que d’avoir à faire glisser le `Slider` thumb. Elle est consommée dans XAML en définissant le [ `Slider.UpdateOnTap` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.UpdateOnTapProperty) propriété pouvant être liée `true`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Slider ... ios:Slider.UpdateOnTap="true" />
        ...
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de C# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var slider = new Xamarin.Forms.Slider();
slider.On<iOS>().SetUpdateOnTap(true);
```

Le `Slider.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le [ `Slider.SetUpdateOnTap` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.SetUpdateOnTap(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Slider},System.Boolean)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, est utilisé pour contrôler si un clic sur le `Slider` barre définira le [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) propriété. En outre, le [ `Slider.GetUpdateOnTap` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.GetUpdateOnTap(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Slider})) méthode peut être utilisée pour retourner si un clic sur le `Slider` barre définira le `Slider.Value` propriété.

Le résultat est qu’un clic sur le [ `Slider` ](xref:Xamarin.Forms.Slider) barre peut déplacer le `Slider` thumb et définissez le [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) propriété :

![](slider-thumb-images/slider-updateontap.png "Mise à jour de curseur sur Tap activé")

## <a name="related-links"></a>Liens connexes

- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
