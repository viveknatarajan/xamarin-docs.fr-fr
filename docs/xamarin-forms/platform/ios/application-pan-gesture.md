---
title: Reconnaissance de mouvement panoramique simultanées sur iOS
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article explique comment utiliser l’iOS spécifique à la plateforme qui permet la reconnaissance de mouvement panoramique simultanées à utiliser dans une application.
ms.prod: xamarin
ms.assetid: 883D89DA-F8FF-4B97-9C3F-2DD05C96A495
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 32c3651734fcc94dd75372f0c47f0ffb22b4a4ee
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54209704"
---
# <a name="simultaneous-pan-gesture-recognition-on-ios"></a>Reconnaissance de mouvement panoramique simultanées sur iOS

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Quand un [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) est attachée à une vue à l’intérieur d’une vue de défilement, toutes le panoramique mouvements sont capturées par le `PanGestureRecognizer` et ne sont pas passées à la vue de défilement. Par conséquent, la vue de défilement n’est plus défile.

Cette spécifiques à la plateforme iOS permet un `PanGestureRecognizer` dans une vue de défilement pour capturer et partager le mouvement panoramique avec la vue de défilement. Elle est consommée dans XAML en définissant le [ `Application.PanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.PanGestureRecognizerShouldRecognizeSimultaneouslyProperty) propriété jointe `true`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.PanGestureRecognizerShouldRecognizeSimultaneously="true">
    ...
</Application>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetPanGestureRecognizerShouldRecognizeSimultaneously(true);
```

Le `Application.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le [ `Application.SetPanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.SetPanGestureRecognizerShouldRecognizeSimultaneously(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Application},System.Boolean)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, est utilisé pour contrôler si un module de reconnaissance de mouvement panoramique dans une vue de défilement capturer le mouvement panoramique, ou capturer et partager le panoramique mouvements avec la vue de défilement. En outre, le [ `Application.GetPanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.GetPanGestureRecognizerShouldRecognizeSimultaneously(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Application})) méthode peut être utilisée pour retourner si le mouvement panoramique est partagé avec la vue de défilement qui contient le [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer).

Par conséquent, avec cette spécifiques à la plateforme est activée, lorsque un [ `ListView` ](xref:Xamarin.Forms.ListView) contient un [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer), à la fois le `ListView` et `PanGestureRecognizer` recevront le mouvement panoramique et le traiter. Toutefois, avec cette spécifiques à la plateforme désactivée quand un `ListView` contient un `PanGestureRecognizer`, le `PanGestureRecognizer` capturer le mouvement panoramique et le traiter et le `ListView` ne reçoivent pas le mouvement panoramique.

## <a name="related-links"></a>Liens connexes

- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
