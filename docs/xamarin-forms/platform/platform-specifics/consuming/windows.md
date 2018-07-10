---
title: Caractéristiques de la plateforme Windows
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article montre comment utiliser Windows-spécificités de la plateforme qui sont intégrés dans Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: 52895564ef327845940d687a58b007fb1502e62b
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935116"
---
# <a name="windows-platform-specifics"></a>Caractéristiques de la plateforme Windows

_Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article montre comment utiliser Windows-spécificités de la plateforme qui sont intégrés dans Xamarin.Forms._

Sur la plateforme de Windows universelle (UWP), Xamarin.Forms contient les spécificités de plateforme suivantes :

- Définition des options de placement de barre d’outils. Pour plus d’informations, consultez [changer la disposition de la barre d’outils](#toolbar_placement).
- Réduire le [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) barre de navigation. Pour plus d’informations, consultez [réduction d’une barre de Navigation MasterDetailPage](#collapsable_navigation_bar).
- L’activation une [ `WebView` ](xref:Xamarin.Forms.WebView) pour afficher les alertes de JavaScript dans une boîte de dialogue de message UWP. Pour plus d’informations, consultez [affichage des alertes de JavaScript](#webview-javascript-alert).
- L’activation une [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) pour interagir avec le moteur de vérification orthographique. Pour plus d’informations, consultez [activation SearchBar orthographique](#searchbar-spellcheck).
- Détection de l’ordre de lecture à partir du texte contenu dans [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), et [ `Label` ](xref:Xamarin.Forms.Label) instances. Pour plus d’informations, consultez [détection de l’ordre de lecture à partir du contenu](#inputview-readingorder).
- Désactiver le mode hérité de couleur sur la prise en charge [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Pour plus d’informations, consultez [la désactivation de Mode de couleur hérité](#legacy-color-mode).
- L’activation de la prise en charge des mouvements de drainage dans un [ `ListView` ](xref:Xamarin.Forms.ListView). Pour plus d’informations, consultez [activation appuyez sur mouvement prise en charge dans un ListView](#listview-selectionmode).

<a name="toolbar_placement" />

## <a name="changing-the-toolbar-placement"></a>Changer la disposition de la barre d’outils

Spécifiques à cette plateforme est utilisé pour modifier l’emplacement d’une barre d’outils sur un [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)et est utilisé dans XAML en définissant le [ `Page.ToolbarPlacement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty/) propriété attachée à une valeur de la [ `ToolbarPlacement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) énumération :

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:Page.ToolbarPlacement="Bottom">
  ...
</TabbedPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetToolbarPlacement(ToolbarPlacement.Bottom);
```

Le `Page.On<Windows>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur Windows. Le [ `Page.SetToolbarPlacement` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.SetToolbarPlacement/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.Page}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) espace de noms, est utilisé pour définir le positionnement de la barre d’outils, avec le [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) fournissant d’énumération trois valeurs : [ `Default` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default), [ `Top` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top), et [ `Bottom` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom).

Le résultat est que le placement de la barre d’outils spécifié est appliqué à la [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) instance :

[![](windows-images/toolbar-placement.png "Barre d’outils Placement spécifique à la plateforme")](windows-images/toolbar-placement-large.png#lightbox "barre d’outils Placement spécifique à la plateforme")

<a name="collapsable_navigation_bar" />

## <a name="collapsing-a-masterdetailpage-navigation-bar"></a>Réduction d’une barre de Navigation MasterDetailPage

Cette plate-forme spécifique est utilisé pour réduire la barre de navigation sur un [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)et est utilisé dans XAML en définissant le [ `MasterDetailPage.CollapseStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty/) et [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty/)propriétés jointes :

```xaml
<MasterDetailPage ...
                  xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
                  windows:MasterDetailPage.CollapseStyle="Partial"
                  windows:MasterDetailPage.CollapsedPaneWidth="48">
  ...
</MasterDetailPage>

```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetCollapseStyle(CollapseStyle.Partial).CollapsedPaneWidth(148);
```

Le `MasterDetailPage.On<Windows>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur Windows. Le [ `Page.SetCollapseStyle` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.SetCollapseStyle/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) espace de noms, est utilisé pour spécifier le style de la réduction, avec le [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) énumération fournissant deux valeurs : [ `Full` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full) et [ `Partial` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial). Le [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/System.Double/) méthode est utilisée pour spécifier la largeur d’une barre de navigation partiellement réduit.

Le résultat qui est spécifié [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) est appliqué à la [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) instance, avec la largeur également spécifiée :

[![](windows-images/collapsed-navigation-bar.png "Réduit la barre de Navigation spécifiques à la plateforme")](windows-images/collapsed-navigation-bar-large.png#lightbox "réduit la barre de Navigation spécifiques à la plateforme")

<a name="webview-javascript-alert" />

## <a name="displaying-javascript-alerts"></a>Affichage des alertes de JavaScript

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

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

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

![Alerte WebView JavaScript spécifique à la plateforme](windows-images/webview-javascript-alert.png "alerte WebView JavaScript spécifique à la plateforme")

<a name="searchbar-spellcheck" />

## <a name="enabling-searchbar-spell-check"></a>L’activation de SearchBar vérification orthographique

Spécifiques à cette plateforme permet un [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) pour interagir avec le moteur de vérification orthographique. Elle est consommée dans XAML en définissant le [ `SearchBar.IsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) propriété jointe un `boolean` valeur :

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ... windows:SearchBar.IsSpellCheckEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

Le `SearchBar.On<Windows>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur la plateforme Windows universelle. Le [ `SearchBar.SetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.SetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar},System.Boolean)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) espace de noms, Active et désactive le vérificateur d’orthographe. En outre, le `SearchBar.SetIsSpellCheckEnabled` méthode peut être utilisée pour activer/désactiver le vérificateur d’orthographe en appelant le [ `SearchBar.GetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.GetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar})) méthode à retourner si le vérificateur d’orthographe est activé :

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

Le résultat est que le texte entré dans le [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) peut être orthographique vérifiée, avec les orthographes incorrectes indiquées à l’utilisateur :

![SearchBar orthographique vérification spécifique à la plateforme](windows-images/searchbar-spellcheck.png "SearchBar orthographique vérification spécifique à la plateforme")

> [!NOTE]
> Le `SearchBar` classe dans le [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) espace de noms a également [ `EnableSpellCheck` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*) et [ `DisableSpellCheck` ](xre:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*) méthodes qui peuvent être utilisées pour activer et désactiver le vérificateur d’orthographe sur la `SearchBar`, respectivement.

<a name="inputview-readingorder" />

## <a name="detecting-reading-order-from-content"></a>Détection de l’ordre de lecture à partir du contenu

Spécifiques à cette plateforme permet à l’ordre de lecture (de gauche à droite ou de droite à gauche) du texte bidirectionnel dans [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), et [ `Label` ](xref:Xamarin.Forms.Label) instances pour être détecté de manière dynamique. Elle est consommée dans XAML en définissant le [ `InputView.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty) (pour `Entry` et `Editor` instances) ou [ `Label.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty) propriété jointe un `boolean` valeur :

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Editor ... windows:InputView.DetectReadingOrderFromContent="true" />
        ...
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

editor.On<Windows>().SetDetectReadingOrderFromContent(true);
```

Le `Editor.On<Windows>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur la plateforme Windows universelle. Le [ `InputView.SetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.SetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView},System.Boolean)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) espace de noms, est utilisé pour contrôler si l’ordre de lecture est détectée à partir du contenu dans le [ `InputView` ](xref:Xamarin.Forms.InputView). En outre, le `InputView.SetDetectReadingOrderFromContent` méthode peut être utilisée pour activer/désactiver si l’ordre de lecture est détectée à partir du contenu en appelant le [ `InputView.GetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.GetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView})) méthode pour retourner la valeur actuelle :

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

Le résultat est que [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), et [ `Label` ](xref:Xamarin.Forms.Label) instances peuvent avoir l’ordre de lecture de leur contenu détecté dynamiquement :

[![InputView détection de l’ordre de lecture à partir du contenu spécifique à la plateforme](windows-images/editor-readingorder.png "InputView de détection de l’ordre de lecture à partir du contenu spécifique à la plateforme")](windows-images/editor-readingorder-large.png#lightbox "InputView l’ordre de lecture à partir de détection contenu spécifique à la plateforme")

> [!NOTE]
> Contrairement au paramètre la [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) propriété, la logique pour les vues qui détectent l’ordre de lecture à partir de leur contenu de texte n’affectera pas l’alignement du texte dans la vue. Au lieu de cela, il ajuste l’ordre dans lequel les blocs de texte bidirectionnel sont disposées.

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>Désactiver le Mode de couleur hérité

Certaines des vues Xamarin.Forms fonctionnalité un mode couleur hérité. Dans ce mode, lorsque le [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) propriété de la vue est définie sur `false`, la vue remplace les couleurs définies par l’utilisateur avec les couleurs natif par défaut de l’état désactivé. Pour vers l’arrière, compatibilité, ce mode hérité couleur reste le comportement par défaut pour les vues prises en charge.

Spécifiques à cette plateforme désactive ce mode couleur hérité, afin que les couleurs définies sur une vue par l’utilisateur sont conservées même lorsque la vue est désactivée. Elle est consommée dans XAML en définissant le [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.IsLegacyColorModeEnabledProperty) propriété jointe `false`:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Editor Text="Enter text here"
                TextColor="Blue"
                BackgroundColor="Bisque"
                windows:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

_legacyColorModeDisabledEditor.On<Windows>().SetIsLegacyColorModeEnabled(false);
```

Le `VisualElement.On<Windows>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur Windows. Le [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Boolean)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) espace de noms, est utilisé pour contrôler si le mode hérité est désactivé. En outre, le [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})) méthode peut être utilisée pour retourner si le mode hérité est désactivé.

Le résultat est que le mode hérité de couleur peut être désactivé, afin que les couleurs définies sur une vue par l’utilisateur restent même lorsque la vue est désactivée :

![](windows-images/legacy-color-mode-disabled.png "Mode hérité couleur désactivé")

> [!NOTE]
> Lorsque vous définissez un [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) sur une vue, le mode hérité de couleur est complètement ignoré. Pour plus d’informations sur les états visuels, consultez [le Gestionnaire d’état visuel Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

<a name="listview-selectionmode" />

## <a name="enabling-tap-gesture-support-in-a-listview"></a>L’activation de la prise en charge des mouvements de Tap dans un ListView

Sur la plateforme Windows universelle, par défaut Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) utilise natif `ItemClick` événement pour répondre à l’interaction, plutôt que du natif `Tapped` événement. Cela fournit des fonctionnalités d’accessibilité afin que le Narrateur Windows et le clavier peuvent interagir avec le `ListView`. Toutefois, mais affiche également les mouvements tap à l’intérieur de la `ListView` inutilisable.

Ce contrôle spécifique à la plateforme si les éléments dans un [ `ListView` ](xref:Xamarin.Forms.ListView) peut répondre pour appuyer sur les mouvements et par conséquent, si natif `ListView` se déclenche le `ItemClick` ou `Tapped` événement. Elle est consommée dans XAML en définissant le [ `ListView.SelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SelectionModeProperty) propriété attachée à une valeur de la [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) énumération :

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <ListView ... windows:ListView.SelectionMode="Inaccessible">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

listView.On<Windows>().SetSelectionMode(ListViewSelectionMode.Inaccessible);
```

Le `ListView.On<Windows>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur la plateforme Windows universelle. Le [ `ListView.SetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) espace de noms, est utilisée pour déterminer si les éléments dans un [ `ListView` ](xref:Xamarin.Forms.ListView) peut répondre pour appuyer sur les mouvements, avec le [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) énumération fournissant deux valeurs possibles :

- [`Accessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Accessible) – Indique que le `ListView` activeront natif `ItemClick` événement pour gérer l’interaction et par conséquent, fournir des fonctionnalités d’accessibilité. Par conséquent, le Narrateur Windows et le clavier peuvent interagir avec le `ListView`. Toutefois, les éléments dans le `ListView` ne peut pas répondre pour appuyer sur les mouvements. Il s’agit du comportement par défaut `ListView` instances sur la plateforme Windows universelle.
- [`Inaccessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Inaccessible) – Indique que le `ListView` activeront natif `Tapped` événements pour gérer l’interaction. Par conséquent, les éléments dans le `ListView` peut répondre pour appuyer sur les mouvements. Toutefois, il n’existe aucune fonctionnalité d’accessibilité et par conséquent, le Narrateur Windows et le clavier ne peut pas interagir avec le `ListView`.

> [!NOTE]
> Le `Accessible` et `Inaccessible` modes de sélection s’excluent mutuellement, et vous devez choisir entre un accessible [ `ListView` ](xref:Xamarin.Forms.ListView) ou un `ListView` qui peut répondre et profitez de mouvements.

En outre, le [ `GetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.GetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView})) méthode peut être utilisée pour retourner actuel [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode).

Le résultat qui est spécifié [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) est appliqué à la [ `ListView` ](xref:Xamarin.Forms.ListView), qui contrôle si les éléments dans le `ListView` peut répondre pour appuyer sur les mouvements et par conséquent, si natif `ListView` se déclenche le `ItemClick` ou `Tapped` événement.

## <a name="summary"></a>Récapitulatif

Cet article a montré comment utiliser Windows-spécificités de la plateforme qui sont intégrés dans Xamarin.Forms. Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets.

## <a name="related-links"></a>Liens associés

- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)
