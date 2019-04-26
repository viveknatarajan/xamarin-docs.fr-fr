---
title: Événements de cycle de vie de page sur Android
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article explique comment consommer le Android spécifique à la plateforme qui désactive le Disappearing et Appearing les événements de page sur application suspendre et reprendre, respectivement.
ms.prod: xamarin
ms.assetid: F6E3759C-D347-407A-91A2-CF9B3B7D4CBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: ab9c404fc9051014fd3a243848290087f43a46d2
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61363804"
---
# <a name="page-lifecycle-events-on-android"></a>Événements de cycle de vie de page sur Android

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Cette spécifiques à la plateforme Android est utilisée pour désactiver le [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) et [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) des événements de page sur application suspendre et reprendre la respectivement, pour les applications qui utilisent AppCompat. En outre, il inclut la possibilité de contrôler si le clavier logiciel s’affiche à la reprise, si elle était affichée sur pause, à condition que le mode de fonctionnement du clavier de manière réversible est activée [ `WindowSoftInputModeAdjust.Resize` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize).

> [!NOTE]
> Notez que ces événements sont activés par défaut pour conserver le comportement existant pour les applications qui s’appuient sur les événements. La désactivation de ces événements rend le cycle des événements AppCompat de correspondre le cycle des événements pré-AppCompat.

Cette plateforme spécifique peut être consommée dans XAML en définissant le [ `Application.SendDisappearingEventOnPause` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPauseProperty), [ `Application.SendAppearingEventOnResume` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResumeProperty), et [ `Application.ShouldPreserveKeyboardOnResume` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResumeProperty) à despropriétésjointes`boolean` valeurs :

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"             xmlns:androidAppCompat="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize"
             androidAppCompat:Application.SendDisappearingEventOnPause="false"
             androidAppCompat:Application.SendAppearingEventOnResume="false"
             androidAppCompat:Application.ShouldPreserveKeyboardOnResume="true">
  ...
</Application>
```

Vous pouvez également, il peut être consommé à partir de C# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

Xamarin.Forms.Application.Current.On<Android>()
     .UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize)
     .SendDisappearingEventOnPause(false)
     .SendAppearingEventOnResume(false)
     .ShouldPreserveKeyboardOnResume(true);
```

Le `Application.Current.On<Android>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur Android. Le [ `Application.SendDisappearingEventOnPause` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPause(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},System.Boolean)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) espace de noms, est utilisé pour activer ou désactiver le déclenchement de l’événement le [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) les événements de page, lorsque l’application passe à l’arrière-plan. Le [ `Application.SendAppearingEventOnResume` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResume(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},System.Boolean)) méthode est utilisée pour activer ou désactiver le déclenchement de l’événement le [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) événements de page, lorsque l’application reprend à partir de l’arrière-plan. Le [ `Application.ShouldPreserveKeyboardOnResume` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResume(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},System.Boolean)) sert de méthode contrôle si le clavier logiciel s’affiche à la reprise, si elle était affichée sur pause, fournies que le mode de fonctionnement du clavier de manière réversible a la valeur [ `WindowSoftInputModeAdjust.Resize` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize).

Le résultat est que le [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) et [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) les événements de page ne sont pas être déclenchés à la suspension de l’application et reprendre respectivement, et que si le clavier logiciel a été affiché lorsque l’application a été suspendu, il est également affichée lorsque l’application reprend :

[![](page-lifecycle-events-images/keyboard-on-resume.png "Cycle de vie des événements spécifiques à la plateforme")](page-lifecycle-events-images/keyboard-on-resume-large.png#lightbox "cycle de vie des événements spécifiques à la plateforme")

## <a name="related-links"></a>Liens connexes

- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
