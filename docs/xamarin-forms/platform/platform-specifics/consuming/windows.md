---
title: Plateforme-spécifiques à Windows
description: Caractéristiques de la plateforme vous permettent de vous permet d’utiliser la fonctionnalité est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou les effets. Cet article montre comment utiliser les plateforme-spécifiques à Windows qui sont intégrées dans Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/23/2018
ms.openlocfilehash: d4ddb662bf167a0c80561cce097104a7f5fc8096
ms.sourcegitcommit: 4f646dc5c51db975b2936169547d625c78a22b30
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2018
---
# <a name="windows-platform-specifics"></a>Plateforme-spécifiques à Windows

_Caractéristiques de la plateforme vous permettent de vous permet d’utiliser la fonctionnalité est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou les effets. Cet article montre comment utiliser les plateforme-spécifiques à Windows qui sont intégrées dans Xamarin.Forms._

Sur la plate-forme de Windows universelle (UWP), Xamarin.Forms contient les spécifications de plateforme suivantes :

- Définition des options de placement de barre d’outils. Pour plus d’informations, consultez [la modification de l’emplacement de la barre d’outils](#toolbar_placement).
- Réduction de la [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) barre de navigation. Pour plus d’informations, consultez [réduction d’une barre de Navigation MasterDetailPage](#collapsable_navigation_bar).
- L’activation une [ `WebView` ](xref:Xamarin.Forms.WebView) pour afficher les alertes de JavaScript dans une boîte de dialogue de message UWP. Pour plus d’informations, consultez [affichage des alertes JavaScript](#webview-javascript-alert).

<a name="toolbar_placement" />

## <a name="changing-the-toolbar-placement"></a>Modification de l’emplacement de la barre d’outils

Cette plateforme spécifique est utilisée pour modifier le placement d’une barre d’outils sur un [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)et est utilisée en XAML en définissant le [ `Page.ToolbarPlacement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty/) à une valeur de propriété attachée le [ `ToolbarPlacement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) énumération :

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:Page.ToolbarPlacement="Bottom">
  ...
</TabbedPage>
```

Vous pouvez également être utilisée à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetToolbarPlacement(ToolbarPlacement.Bottom);
```

Le `Page.On<Windows>` méthode spécifie que cette plate-forme spécifique sera exécuté uniquement sur Windows. Le [ `Page.SetToolbarPlacement` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.SetToolbarPlacement/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.Page}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) espace de noms est utilisée pour définir l’emplacement de la barre d’outils, avec le [ `ToolbarPlacement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) fournissant d’énumération trois valeurs : [ `Default` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default/), [ `Top` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top/), et [ `Bottom` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom/).

Le résultat est que le placement de la barre d’outils spécifiée est appliqué à la [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) instance :

[![](windows-images/toolbar-placement.png "Barre d’outils emplacement spécifique à la plateforme")](windows-images/toolbar-placement-large.png#lightbox "barre d’outils emplacement spécifique à la plateforme")

<a name="collapsable_navigation_bar" />

## <a name="collapsing-a-masterdetailpage-navigation-bar"></a>Réduction d’une barre de Navigation MasterDetailPage

Cette plateforme spécifique est utilisé pour réduire la barre de navigation sur une [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)et est utilisée en XAML en définissant le [ `MasterDetailPage.CollapseStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty/) et [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty/)propriétés jointes :

```xaml
<MasterDetailPage ...
                  xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
                  windows:MasterDetailPage.CollapseStyle="Partial"
                  windows:MasterDetailPage.CollapsedPaneWidth="48">
  ...
</MasterDetailPage>

```

Vous pouvez également être utilisée à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetCollapseStyle(CollapseStyle.Partial).CollapsedPaneWidth(148);
```

Le `MasterDetailPage.On<Windows>` méthode spécifie que cette plate-forme spécifique sera exécuté uniquement sur Windows. Le [ `Page.SetCollapseStyle` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.SetCollapseStyle/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) espace de noms est utilisé pour spécifier le style de réduction, avec la [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) énumération fournissant deux valeurs : [ `Full` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full/) et [ `Partial` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial/). Le [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/System.Double/) méthode est utilisée pour spécifier la largeur d’une barre de navigation partiellement réduit.

Le résultat qui est spécifié [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) est appliqué à la [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) instance, avec la largeur spécifiée également :

[![](windows-images/collapsed-navigation-bar.png "Réduit la barre de Navigation spécifiques à une plateforme")](windows-images/collapsed-navigation-bar-large.png#lightbox "réduit la barre de Navigation spécifique à la plateforme")

<a name="webview-javascript-alert" />

## <a name="displaying-javascript-alerts"></a>Affichage des alertes de JavaScript

Spécifiques à cette plateforme permet un [ `WebView` ](xref:Xamarin.Forms.WebView) pour afficher les alertes de JavaScript dans une boîte de dialogue de message UWP. Elle est consommée en XAML en définissant le [ `WebView.IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabledProperty) propriété attachée un `boolean` valeur :

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <WebView ... windows:WebView.IsJavaScriptAlertEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Vous pouvez également être utilisée à partir de c# à l’aide de l’API fluent :

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

Le `WebView.On<Windows>` méthode spécifie que cette plate-forme spécifique sera exécuté uniquement sur la plateforme Windows universelle. Le [ `WebView.SetIsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.SetIsJavaScriptAlertEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.WebView},System.Boolean)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) espace de noms est utilisé pour contrôler si les alertes JavaScript sont activés. En outre, le `WebView.SetIsJavaScriptAlertEnabled` méthode peut être utilisée pour activer/désactiver les alertes JavaScript en appelant le [ `IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabled*) méthode à retourner si elles sont activées :

```csharp
_webView.On<Windows>().SetIsJavaScriptAlertEnabled(!_webView.On<Windows>().IsJavaScriptAlertEnabled());
```

Le résultat est que les alertes JavaScript peuvent être affichés dans une boîte de dialogue de message UWP :

![Alerte de WebView JavaScript spécifiques à une plateforme](windows-images/webview-javascript-alert.png "WebView JavaScript alerte spécifique à la plateforme")

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment utiliser les plateforme-spécifiques à Windows qui sont intégrées dans Xamarin.Forms. Caractéristiques de la plateforme vous permettent de vous permet d’utiliser la fonctionnalité est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou les effets.

## <a name="related-links"></a>Liens associés

- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)
