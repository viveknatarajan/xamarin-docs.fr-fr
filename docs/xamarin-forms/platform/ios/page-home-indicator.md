---
title: Accueil indicateur de visibilité sur iOS
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article explique comment utiliser la plateforme spécifique à iOS qui définit la visibilité de l’indicateur de base sur une Page.
ms.prod: xamarin
ms.assetid: F81022E0-3C6C-49C0-A000-FAF6574D3FB7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
ms.openlocfilehash: c0d6717cd8f89344be7df3dc3ec687a5f1b375f4
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65971621"
---
# <a name="home-indicator-visibility-on-ios"></a>Accueil indicateur de visibilité sur iOS

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Cette spécifiques à la plateforme iOS définit la visibilité de l’indicateur de base sur un [ `Page` ](xref:Xamarin.Forms.Page). Elle est consommée dans XAML en définissant le [ `Page.PrefersHomeIndicatorAutoHidden` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Page.PrefersHomeIndicatorAutoHiddenProperty) propriété pouvant être liée à un `boolean`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersHomeIndicatorAutoHidden="true">
    ...
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de C# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersHomeIndicatorAutoHidden(true);
```

Le `Page.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le [ `Page.SetPrefersHomeIndicatorAutoHidden` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Page.SetPrefersHomeIndicatorAutoHidden(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Page},System.Boolean)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, contrôle la visibilité de l’indicateur d’accueil. En outre, le [ `Page.PrefersHomeIndicatorAutoHidden` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Page.PrefersHomeIndicatorAutoHidden(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Page})) méthode peut être utilisée pour récupérer la visibilité de l’indicateur d’accueil.

Le résultat est que la visibilité de l’indicateur de base sur un [ `Page` ](xref:Xamarin.Forms.Page) peut être contrôlée :

![Capture d’écran de visibilité accueil indicateur sur une page iOS](page-home-indicator-images/home-indicator-visibility.png "Page visibilité de l’indicateur d’accueil")

> [!NOTE]
> Cette plateforme spécifique peut être appliquée à [ `ContentPage` ](xref:Xamarin.Forms.ContentPage), [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage), [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), et [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)objets.

## <a name="related-links"></a>Liens connexes

- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
