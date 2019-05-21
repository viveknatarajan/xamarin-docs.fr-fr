---
title: Animations de Transition de Page de TabbedPage sur Android
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article explique comment utiliser l’Android spécifique à la plateforme qui désactive les animations de transition en parcourant les pages dans un TabbedPage.
ms.prod: xamarin
ms.assetid: 2DB4EA6D-9CED-4137-BAB2-B20A457B1CA3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: b57718038c43f7bc2a9e27e780a0624eb50a54fe
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65926412"
---
# <a name="tabbedpage-page-transition-animations-on-android"></a>Animations de Transition de Page de TabbedPage sur Android

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

Cette spécifiques à la plateforme Android est utilisée pour désactiver les animations de transition lors de la navigation via les pages, soit par programmation ou lors de l’utilisation de la barre d’onglet, dans un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Elle est consommée dans XAML en définissant le `TabbedPage.IsSmoothScrollEnabled` propriété pouvant être liée `false`:

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.IsSmoothScrollEnabled="false">
    ...
</TabbedPage>
```

Vous pouvez également, il peut être consommé à partir de C# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetIsSmoothScrollEnabled(false);
```

Le `TabbedPage.On<Android>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur Android. Le `TabbedPage.SetIsSmoothScrollEnabled` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) espace de noms, est utilisé pour contrôler si les animations de transition seront affichera lors de la navigation entre les pages dans un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). En outre, le `TabbedPage` classe dans le `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` espace de noms comporte également les méthodes suivantes :

- `IsSmoothScrollEnabled`, qui est utilisée pour récupérer si les animations de transition seront affichera lors de la navigation entre les pages dans un `TabbedPage`.
- `EnableSmoothScroll`, qui est utilisée pour activer les animations de transition lors de la navigation entre les pages dans un `TabbedPage`.
- `DisableSmoothScroll`, qui est utilisé pour désactiver les animations de transition lors de la navigation entre les pages dans un `TabbedPage`.

## <a name="related-links"></a>Liens connexes

- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
