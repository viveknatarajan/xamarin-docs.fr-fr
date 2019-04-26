---
title: WebView mixte de contenu sur Android
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article explique comment utiliser l’Android spécifique à la plateforme qui affiche le contenu mixte dans un affichage Web dans les applications cette API cible 21 ou version ultérieure.
ms.prod: xamarin
ms.assetid: 68F908F3-04C5-4B91-B6E5-B7E8357B4154
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 8897736878d0ddee22cdad073cc16deb8ce824e1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61359989"
---
# <a name="webview-mixed-content-on-android"></a>WebView mixte de contenu sur Android

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Cette spécifiques à la plateforme Android contrôle si un [ `WebView` ](xref:Xamarin.Forms.WebView) pouvez affiche un contenu mixte dans les applications qui ciblent les API 21 ou version ultérieure. Contenu mixte est un contenu qui sont initialement chargée sur une connexion HTTPS, mais qui charge des ressources (telles que des images, audio, vidéo, feuilles de style, scripts) via une connexion HTTP. Elle est consommée dans XAML en définissant le [ `WebView.MixedContentMode` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty) propriété attachée à une valeur de la [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) énumération :

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de C# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

Le `WebView.On<Android>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur Android. Le [ `WebView.SetMixedContentMode` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.SetMixedContentMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.WebView},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) espace de noms, est utilisé pour contrôler si le contenu mixte peut être affiché, avec le [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) énumération en fournissant trois valeurs possibles :

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow) – Indique que le [ `WebView` ](xref:Xamarin.Forms.WebView) permettra une origine HTTPS charger le contenu à partir d’une origine HTTP.
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow) – Indique que le [ `WebView` ](xref:Xamarin.Forms.WebView) n’autorise pas une origine HTTPS charger le contenu à partir d’une origine HTTP.
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode) – Indique que le [ `WebView` ](xref:Xamarin.Forms.WebView) tentera d’être compatible avec l’approche de la dernière version du navigateur web appareil. Partie du contenu HTTP peut-être être autorisé à être chargés par une entité origin HTTPS et autres types de contenu seront bloqués. Les types de contenu qui sont bloqués ou autorisés peuvent changer avec chaque version de système d’exploitation.

Le résultat qui est spécifié [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) valeur est appliquée à la [ `WebView` ](xref:Xamarin.Forms.WebView), qui contrôle si le contenu mixte peut s’afficher :

[![WebView mixte de gestion de contenu spécifique à la plateforme](webview-mixed-content-images/webview-mixedcontent.png "WebView mixte de gestion de contenu spécifique à la plateforme")](webview-mixed-content-images/webview-mixedcontent-large.png#lightbox "WebView mixte de gestion de contenu spécifique à la plateforme")

## <a name="related-links"></a>Liens connexes

- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
