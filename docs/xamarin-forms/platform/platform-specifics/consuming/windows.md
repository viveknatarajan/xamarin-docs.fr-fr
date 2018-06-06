---
title: Plateforme-spécifiques à Windows
description: Caractéristiques de la plateforme vous permettent de vous permet d’utiliser la fonctionnalité est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou les effets. Cet article montre comment utiliser les plateforme-spécifiques à Windows qui sont intégrées dans Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: 7299de658a3491928e9bbeaa4dd192a8e95c435e
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732799"
---
# <a name="windows-platform-specifics"></a>Plateforme-spécifiques à Windows

_Caractéristiques de la plateforme vous permettent de vous permet d’utiliser la fonctionnalité est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou les effets. Cet article montre comment utiliser les plateforme-spécifiques à Windows qui sont intégrées dans Xamarin.Forms._

Sur la plate-forme de Windows universelle (UWP), Xamarin.Forms contient les spécifications de plateforme suivantes :

- Définition des options de placement de barre d’outils. Pour plus d’informations, consultez [la modification de l’emplacement de la barre d’outils](#toolbar_placement).
- Réduction de la [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) barre de navigation. Pour plus d’informations, consultez [réduction d’une barre de Navigation MasterDetailPage](#collapsable_navigation_bar).
- L’activation une [ `WebView` ](xref:Xamarin.Forms.WebView) pour afficher les alertes de JavaScript dans une boîte de dialogue de message UWP. Pour plus d’informations, consultez [affichage des alertes JavaScript](#webview-javascript-alert).
- L’activation une [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) pour interagir avec le moteur de vérification orthographique. Pour plus d’informations, consultez [Vérifiez l’orthographe de l’activation de SearchBar](#searchbar-spellcheck).
- Détection de l’ordre de lecture à partir du texte contenu dans [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), et [ `Label` ](xref:Xamarin.Forms.Label) instances. Pour plus d’informations, consultez [détection de l’ordre de lecture à partir du contenu](#inputview-readingorder).
- Désactivez le mode hérité de couleur sur la prise en charge [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Pour plus d’informations, consultez [désactiver le Mode hérité couleur](#legacy-color-mode).
- L’activation de la prise en charge des mouvements de drainage dans un [ `ListView` ](xref:Xamarin.Forms.ListView). Pour plus d’informations, consultez [activation appuyez sur mouvement prise en charge dans un ListView](#listview-selectionmode).

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

<a name="searchbar-spellcheck" />

## <a name="enabling-searchbar-spell-check"></a>L’activation de vérification orthographique de SearchBar

Spécifiques à cette plateforme permet un [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) pour interagir avec le moteur de vérification orthographique. Elle est consommée en XAML en définissant le [ `SearchBar.IsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) propriété attachée un `boolean` valeur :

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ... windows:SearchBar.IsSpellCheckEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Vous pouvez également être utilisée à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

Le `SearchBar.On<Windows>` méthode spécifie que cette plate-forme spécifique sera exécuté uniquement sur la plateforme Windows universelle. Le [ `SearchBar.SetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.SetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar},System.Boolean)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) espace de noms, Active et désactive le vérificateur d’orthographe. En outre, le `SearchBar.SetIsSpellCheckEnabled` méthode peut être utilisée pour activer/désactiver le vérificateur d’orthographe en appelant le [ `SearchBar.GetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.GetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar})) méthode à retourner si le vérificateur d’orthographe est activé :

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

Le résultat est que le texte entré dans le [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) peut être l’orthographe vérifiée, avec des orthographes incorrects qui est indiqués à l’utilisateur :

![SearchBar orthographique vérification spécifique à la plateforme](windows-images/searchbar-spellcheck.png "SearchBar orthographique vérification spécifique à la plateforme")

> [!NOTE]
> Le `SearchBar` classe dans le [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) espace de noms a également [ `EnableSpellCheck` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*) et [ `DisableSpellCheck` ](xre:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*) méthodes qui peuvent être utilisés pour activer et désactiver le vérificateur d’orthographe sur la `SearchBar`, respectivement.

<a name="inputview-readingorder" />

## <a name="detecting-reading-order-from-content"></a>Détection de l’ordre de lecture à partir du contenu

Spécifiques à cette plateforme permet à l’ordre de lecture (de gauche à droite ou de droite à gauche) de texte bidirectionnel dans [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), et [ `Label` ](xref:Xamarin.Forms.Label) instances pour être détecté de manière dynamique. Elle est consommée en XAML en définissant le [ `InputView.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty) (pour `Entry` et `Editor` instances) ou [ `Label.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty) propriété attachée un `boolean` valeur :

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Editor ... windows:InputView.DetectReadingOrderFromContent="true" />
        ...
    </StackLayout>
</ContentPage>
```

Vous pouvez également être utilisée à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

editor.On<Windows>().SetDetectReadingOrderFromContent(true);
```

Le `Editor.On<Windows>` méthode spécifie que cette plate-forme spécifique sera exécuté uniquement sur la plateforme Windows universelle. Le [ `InputView.SetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.SetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView},System.Boolean)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) espace de noms est utilisé pour contrôler si l’ordre de lecture est détecté à partir du contenu dans le [ `InputView` ](xref:Xamarin.Forms.InputView). En outre, le `InputView.SetDetectReadingOrderFromContent` méthode peut être utilisée pour basculer entre l’ordre de lecture est détecté à partir du contenu en appelant le [ `InputView.GetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.GetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView})) méthode pour retourner la valeur actuelle :

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

Le résultat est que [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), et [ `Label` ](xref:Xamarin.Forms.Label) instances peuvent avoir l’ordre de lecture de leur contenu détecté de façon dynamique :

[![InputView de détection de l’ordre de lecture à partir du contenu spécifique à la plateforme](windows-images/editor-readingorder.png "InputView de détection de l’ordre de lecture à partir du contenu spécifique à la plateforme")](windows-images/editor-readingorder-large.png#lightbox "InputView de détection de l’ordre de lecture à partir de contenu spécifique à la plateforme")

> [!NOTE]
> Contrairement au paramètre la [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) propriété, la logique pour les vues qui détectent l’ordre de lecture à partir de leur contenu de texte n’affectera pas l’alignement du texte dans la vue. Au lieu de cela, il ajuste l’ordre dans lequel sont disposés les blocs de texte bidirectionnel.

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>Désactivez le Mode hérité couleur

Certaines des vues Xamarin.Forms fonctionnalité un mode de couleur hérité. Dans ce mode, lorsque le [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) de la vue est définie sur `false`, la vue remplace les couleurs définies par l’utilisateur avec les couleurs natif par défaut de l’état désactivé. Pour en amont la compatibilité, ce mode hérité couleur reste le comportement par défaut pour les vues prises en charge.

Spécifiques à cette plateforme désactive ce mode hérité de couleur, afin que les couleurs définie sur une vue par l’utilisateur sont conservées même si la vue est désactivée. Elle est consommée en XAML en définissant le [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.IsLegacyColorModeEnabledProperty) propriété attachée `false`:

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

Vous pouvez également être utilisée à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

_legacyColorModeDisabledEditor.On<Windows>().SetIsLegacyColorModeEnabled(false);
```

Le `VisualElement.On<Windows>` méthode spécifie que cette plate-forme spécifique sera exécuté uniquement sur Windows. Le [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Boolean)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) espace de noms est utilisé pour contrôler si le mode hérité est désactivé. En outre, le [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})) méthode peut être utilisée pour retourner si le mode hérité est désactivé.

Le résultat est que le mode hérité peut être désactivé, afin que les couleurs définie sur une vue par l’utilisateur restent même lorsque le mode est désactivé :

![](windows-images/legacy-color-mode-disabled.png "Mode hérité couleur désactivé")

> [!NOTE]
> Lorsque vous définissez un [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) sur une vue, le mode hérité est complètement ignoré. Pour plus d’informations sur les états visuels, consultez [le Gestionnaire d’état visuel Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

<a name="listview-selectionmode" />

## <a name="enabling-tap-gesture-support-in-a-listview"></a>Prise en charge de mouvement Tap dans un ListView

Sur la plateforme Windows universelle, par défaut le Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) utilise natif `ItemClick` événement pour répondre à l’interaction, plutôt que natif `Tapped` événement. Cela fournit des fonctionnalités d’accessibilité afin que le Narrateur Windows et le clavier peuvent interagir avec le `ListView`. Toutefois, il affiche également les mouvements tap à l’intérieur de la `ListView` inutilisable.

Ce contrôle spécifique à la plateforme si les éléments dans un [ `ListView` ](xref:Xamarin.Forms.ListView) peut répondre pour appuyer sur les mouvements et par conséquent, si natif `ListView` se déclenche le `ItemClick` ou `Tapped` événement. Elle est consommée en XAML en définissant le [ `ListView.SelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SelectionModeProperty) à une valeur de propriété attachée la [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) énumération :

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

Vous pouvez également être utilisée à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

listView.On<Windows>().SetSelectionMode(ListViewSelectionMode.Inaccessible);
```

Le `ListView.On<Windows>` méthode spécifie que cette plate-forme spécifique sera exécuté uniquement sur la plateforme Windows universelle. Le [ `ListView.SetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) espace de noms est utilisée pour déterminer si les éléments dans un [ `ListView` ](xref:Xamarin.Forms.ListView) peut répondre pour appuyer sur les mouvements, avec le [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) énumération fournissant deux valeurs possibles :

- [`Accessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Accessible) – Indique que le `ListView` déclenchera natif `ItemClick` événements pour gérer l’interaction et par conséquent, fournissent des fonctionnalités d’accessibilité. Par conséquent, le Narrateur Windows et le clavier peuvent interagir avec le `ListView`. Toutefois, les éléments dans le `ListView` ne peut pas répondre pour appuyer sur les mouvements. Il s’agit du comportement par défaut `ListView` instances sur la plateforme Windows universelle.
- [`Inaccessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Inaccessible) – Indique que le `ListView` déclenchera natif `Tapped` événements pour gérer l’interaction. Par conséquent, les éléments dans le `ListView` peut répondre pour appuyer sur les mouvements. Toutefois, il n’existe aucune fonctionnalité d’accessibilité et par conséquent, le Narrateur Windows et le clavier ne peut pas interagir avec le `ListView`.

> [!NOTE]
> Le `Accessible` et `Inaccessible` modes de sélection s’excluent mutuellement, et vous devez choisir entre un accessible [ `ListView` ](xref:Xamarin.Forms.ListView) ou un `ListView` qui peut répondre à collecter des mouvements.

En outre, le [ `GetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.GetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView})) méthode peut être utilisée pour retourner l’actuel [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode).

Le résultat qui est spécifié [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) est appliqué à la [ `ListView` ](xref:Xamarin.Forms.ListView), qui contrôle si les éléments dans le `ListView` peut répondre pour appuyer sur les mouvements et par conséquent, si natif `ListView` se déclenche le `ItemClick` ou `Tapped` événement.

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment utiliser les plateforme-spécifiques à Windows qui sont intégrées dans Xamarin.Forms. Caractéristiques de la plateforme vous permettent de vous permet d’utiliser la fonctionnalité est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou les effets.

## <a name="related-links"></a>Liens associés

- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)
