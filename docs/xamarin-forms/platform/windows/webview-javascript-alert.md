---
title: Alertes WebView JavaScript sur Windows
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article explique comment utiliser le Windows spécifique à la plateforme qui permet un affichage Web afficher les alertes de JavaScript dans une boîte de dialogue de message UWP.
ms.prod: xamarin
ms.assetid: 95A153A1-72A0-4C0B-A452-ACE966BB12CB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 11d297f9d99b986076e8a2c548b580efe41fe463
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60852416"
---
# <a name="webview-javascript-alerts-on-windows"></a>Alertes WebView JavaScript sur Windows

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Spécifiques à cette plateforme permet un [ `WebView` ](xref:Xamarin.Forms.WebView) pour afficher les alertes de JavaScript dans une boîte de dialogue de message UWP. Elle est consommée dans XAML en définissant le [ `WebView.IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabledProperty) propriété jointe un `boolean` valeur :

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <WebView ... windows:WebView.IsJavaScriptAlertEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de C# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

var webView = new Xamarin.Forms.WebView
{
  Source = new HtmlWebViewSource
  {
    Html = @"<html><body><button onclick=""window.alert('Hello World from JavaScript');"">Click Me</button></body></html>"
  }
};
webView.On<Windows>().SetIsJavaScriptAlertEnabled(true);
```

Le `WebView.On<Windows>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur la plateforme Windows universelle. Le [ `WebView.SetIsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.SetIsJavaScriptAlertEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.WebView},System.Boolean)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) espace de noms, est utilisé pour contrôler si les alertes de JavaScript sont activées. En outre, le `WebView.SetIsJavaScriptAlertEnabled` méthode peut être utilisée pour activer/désactiver les alertes de JavaScript en appelant le [ `IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabled*) méthode à retourner si elles sont activées :

```csharp
_webView.On<Windows>().SetIsJavaScriptAlertEnabled(!_webView.On<Windows>().IsJavaScriptAlertEnabled());
```

Le résultat est que les alertes JavaScript peuvent être affichés dans une boîte de dialogue de message UWP :

![Alerte WebView JavaScript spécifique à la plateforme](webview-javascript-alert-images/webview-javascript-alert.png "alerte WebView JavaScript spécifique à la plateforme")

## <a name="related-links"></a>Liens connexes

- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
