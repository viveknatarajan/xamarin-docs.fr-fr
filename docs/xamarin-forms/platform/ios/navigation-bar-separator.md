---
title: Séparateur de barre NavigationPage sur iOS
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article explique comment utiliser les clichés instantanés qui se trouve au bas de la barre de navigation sur un NavigationPage iOS spécifique à la plateforme qui masque la ligne de séparation.
ms.prod: xamarin
ms.assetid: 5A45748A-6779-4441-82F2-415BD68473B9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 7e367d742145c6f984c4485afe9201d4b7ae2e38
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65925675"
---
# <a name="navigationpage-bar-separator-on-ios"></a>Séparateur de barre NavigationPage sur iOS

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

Cette spécifiques à la plateforme iOS masque la ligne de séparation et les clichés instantanés qui se trouve en bas de la barre de navigation sur un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Elle est consommée dans XAML en définissant le [ `NavigationPage.HideNavigationBarSeparator` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.HideNavigationBarSeparatorProperty) propriété pouvant être liée `false`:

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ios:NavigationPage.HideNavigationBarSeparator="true">

</NavigationPage>
```

Vous pouvez également, il peut être consommé à partir de C# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;

public class iOSTitleViewNavigationPageCS : Xamarin.Forms.NavigationPage
{
    public iOSTitleViewNavigationPageCS()
    {
        On<iOS>().SetHideNavigationBarSeparator(true);
    }
}
```

Le `NavigationPage.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le [ `NavigationPage.SetHideNavigationBarSeparator` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetHideNavigationBarSeparator(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage},System.Boolean)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, est utilisé pour contrôler si le séparateur de barre de navigation est masqué. En outre, le [ `NavigationPage.HideNavigationBarSeparator` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.HideNavigationBarSeparator(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage})) méthode peut être utilisée pour retourner si le séparateur de barre de navigation est masqué.

Le résultat est que le séparateur de barre de navigation sur un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) peuvent être masqués :

![](navigation-bar-separator-images/navigationpage-hideseparatorbar.png "Barre de navigation NavigationPage masqué")

## <a name="related-links"></a>Liens connexes

- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
