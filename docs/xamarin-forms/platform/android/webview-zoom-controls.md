---
title: Zoom WebView sur Android
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article explique comment utiliser l’Android spécifique à la plateforme qui permet de zoomer sur un affichage Web.
ms.prod: xamarin
ms.assetid: DC1A3762-6A42-4298-929C-445F416C3E60
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
ms.openlocfilehash: 0e180ca5019bd4e5d1351cfb2f7a8c28ade2afe2
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65971641"
---
# <a name="webview-zoom-on-android"></a>Zoom WebView sur Android

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Cette spécifiques à la plateforme Android Active le pincer-zoomer et un contrôle de zoom sur un [ `WebView` ](xref:Xamarin.Forms.WebView). Elle est consommée dans XAML en définissant le `WebView.EnableZoomControls` et `WebView.DisplayZoomControls` propriétés pouvant être liées à `boolean` valeurs :

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView Source="https://www.xamarin.com"
             android:WebView.EnableZoomControls="true"
             android:WebView.DisplayZoomControls="true" />
</ContentPage>
```

Le `WebView.EnableZoomControls` propriété pouvant être liée détermine si le zoom par pincement est activé sur le [ `WebView` ](xref:Xamarin.Forms.WebView)et le `WebView.DisplayZoomControls` propriété pouvant être liée détermine si les contrôles de zoom sont superposés sur la `WebView`.

Ou bien, la plateforme spécifique peut être consommé depuis C# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>()
    .EnableZoomControls(true)
    .DisplayZoomControls(true);
```

Le `WebView.On<Android>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur Android. Le `WebView.EnableZoomControls` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) espace de noms, est utilisée pour déterminer si le zoom par pincement est activé sur le [ `WebView` ](xref:Xamarin.Forms.WebView). Le `WebView.DisplayZoomControls` (méthode), dans le même espace de noms permet de contrôler si les contrôles de zoom sont superposés sur la `WebView`. En outre, le `WebView.ZoomControlsEnabled` et `WebView.ZoomControlsDisplayed` méthodes peuvent être utilisées pour retourner si les contrôles de zoom par pincement et de zoom sont activés, respectivement.

Le résultat est que pincer pour zoomer peut être activée sur un [ `WebView` ](xref:Xamarin.Forms.WebView), et les contrôles de zoom peuvent être à superposer sur le `WebView`:

[![Capture d’écran de zoom WebView sur Android](webview-zoom-controls-images/webview-zoom.png "WebView avec zoom")](webview-zoom-controls-images/webview-zoom-large.png#lightbox "WebView avec zoom")

> [!IMPORTANT]
> Contrôles de zoom doivent être activés et affichés par le biais des propriétés pouvant être liées respectifs ou méthodes, à superposer sur un [ `WebView` ](xref:Xamarin.Forms.WebView).

## <a name="related-links"></a>Liens connexes

- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
