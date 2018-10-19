---
title: Caractéristiques de la plateforme Windows
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article montre comment utiliser Windows-spécificités de la plateforme qui sont intégrés dans Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 2130bbbedbab66fac9427947ca42f21c346360ce
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2018
ms.locfileid: "38998412"
---
# <a name="windows-platform-specifics"></a>Caractéristiques de la plateforme Windows

_Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article montre comment utiliser Windows-spécificités de la plateforme qui sont intégrés dans Xamarin.Forms._

## <a name="visualelements"></a>VisualElements

Sur la plateforme Windows universelle, les fonctionnalités spécifiques à la plateforme suivante sont fournie pour les vues, les pages et les dispositions Xamarin.Forms :

- Définition d’une clé d’accès pour un [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Pour plus d’informations, consultez [clés d’accès de paramètre VisualElement](#visualelement-accesskeys).
- Désactiver le mode hérité de couleur sur la prise en charge [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Pour plus d’informations, consultez [la désactivation de Mode de couleur hérité](#legacy-color-mode).

<a name="visualelement-accesskeys" />

### <a name="setting-visualelement-access-keys"></a>Définition des clés d’accès de VisualElement

Clés d’accès sont des raccourcis clavier qui améliorent la facilité d’utilisation et l’accessibilité des applications sur la plateforme Windows universelle en fournissant une méthode intuitive permettant aux utilisateurs de naviguer rapidement et d’interagir avec l’interface utilisateur visible de l’application via un clavier au lieu de via tactile ou d’un avec la souris. Ils sont des combinaisons de la touche Alt et une ou plusieurs touches alphanumériques, généralement enfoncés séquentiellement. Raccourcis clavier sont automatiquement pris en charge pour les clés d’accès qui utilisent un seul caractère alphanumérique.

Conseils sur les touches accès sont flottantes badges affichées en regard des contrôles qui intègrent des clés d’accès. Chaque Conseil de clé d’accès contient les clés d’alphanumériques qui activent le contrôle associé. Lorsqu’un utilisateur appuie sur la touche Alt, les conseils de clé d’accès sont affichés.

Cette plate-forme spécifique est utilisée pour spécifier une clé d’accès pour un [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Elle est consommée dans XAML en définissant le [ `VisualElement.AccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyProperty) propriété attachée à une valeur alphanumérique et en définissant éventuellement le [ `VisualElement.AccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyPlacementProperty) propriété attachée à une valeur de la [ `AccessKeyPlacement` ](xref:Xamarin.Forms.AccessKeyPlacement) énumération, le [ `VisualElement.AccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyHorizontalOffsetProperty) propriété jointe un `double`et le [ `VisualElement.AccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyVerticalOffsetProperty) propriété jointe à un `double`:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <ContentPage Title="Page 1"
                 windows:VisualElement.AccessKey="1">
        <StackLayout Margin="20">
            ...
            <Switch windows:VisualElement.AccessKey="A" />
            <Entry Placeholder="Enter text here"
                   windows:VisualElement.AccessKey="B" />
            ...
            <Button Text="Access key F, placement top with offsets"
                    Margin="20"
                    Clicked="OnButtonClicked"
                    windows:VisualElement.AccessKey="F"
                    windows:VisualElement.AccessKeyPlacement="Top"
                    windows:VisualElement.AccessKeyHorizontalOffset="20"
                    windows:VisualElement.AccessKeyVerticalOffset="20" />
            ...
        </StackLayout>
    </ContentPage>
    ...
</TabbedPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

var page = new ContentPage { Title = "Page 1" };
page.On<Windows>().SetAccessKey("1");

var switchView = new Switch();
switchView.On<Windows>().SetAccessKey("A");
var entry = new Entry { Placeholder = "Enter text here" };
entry.On<Windows>().SetAccessKey("B");
...

var button4 = new Button { Text = "Access key F, placement top with offsets", Margin = new Thickness(20) };
button4.Clicked += OnButtonClicked;
button4.On<Windows>()
    .SetAccessKey("F")
    .SetAccessKeyPlacement(AccessKeyPlacement.Top)
    .SetAccessKeyHorizontalOffset(20)
    .SetAccessKeyVerticalOffset(20);
...
```

Le `VisualElement.On<Windows>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur la plateforme Windows universelle. Le [ `VisualElement.SetAccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKey(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.String)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) espace de noms, est utilisé pour définir la valeur de clé d’accès pour le `VisualElement`. Le [ `VisualElement.SetAccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},Xamarin.Forms.AccessKeyPlacement)) méthode, spécifie éventuellement la position à utiliser pour afficher le Conseil de clé d’accès, avec la [ `AccessKeyPlacement` ](xref:Xamarin.Forms.AccessKeyPlacement) énumération fournissant les valeurs possibles suivantes :

- [`Auto`](xref:Xamarin.Forms.AccessKeyPlacement.Auto) – Indique que le placement de touche d’accès sera déterminé par le système d’exploitation.
- [`Top`](xref:Xamarin.Forms.AccessKeyPlacement.Top) – Indique que le Conseil de clé d’accès s’affiche au-dessus du bord supérieur de la `VisualElement`.
- [`Bottom`](xref:Xamarin.Forms.AccessKeyPlacement.Bottom) – Indique que le Conseil de clé d’accès apparaît sous le bord inférieur de la `VisualElement`.
- [`Right`](xref:Xamarin.Forms.AccessKeyPlacement.Right) – Indique que le Conseil de clé d’accès s’affiche à droite du bord droit de la `VisualElement`.
- [`Left`](xref:Xamarin.Forms.AccessKeyPlacement.Left) – Indique que le Conseil de clé d’accès s’affiche à gauche du bord gauche de la `VisualElement`.
- [`Center`](xref:Xamarin.Forms.AccessKeyPlacement.Center) – Indique que le Conseil de clé d’accès se superposent sur le centre de la `VisualElement`.

> [!NOTE]
> En règle générale, le [ `Auto` ](xref:Xamarin.Forms.AccessKeyPlacement.Auto) le positionnement de touche est suffisant, qui inclut la prise en charge des interfaces utilisateur adaptative.

Le [ `VisualElement.SetAccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyHorizontalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Double)) et [ `VisualElement.SetAccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyVerticalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Double)) méthodes peuvent être utilisées pour un contrôle plus précis de l’emplacement de touche d’accès. L’argument pour le `SetAccessKeyHorizontalOffset` méthode indique comment présent déplacer l’accès aux clés info-bulle à gauche ou droite et l’argument à la `SetAccessKeyVerticalOffset` méthode indique à quelle distance déplacer le Conseil de clé d’accès vers le haut ou vers le bas.

>[!NOTE]
> Décalages de touche d’accès ne peut pas être définis lorsque la valeur de l’emplacement de clé d’accès `Auto`.

En outre, le [ `GetAccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKey(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})), [ `GetAccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})), [ `GetAccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyHorizontalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})), et [ `GetAccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyVerticalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})) méthodes peuvent être utilisées. pour récupérer une valeur et son emplacement de la clé.

Il en résulte qu’accès touches d’accès peuvent être affichées en regard d’une [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) instances qui définissent les clés, d’accès en appuyant sur la touche Alt :

![VisualElement accès aux clés de spécifique à la plateforme](windows-images/visualelement-accesskeys.png "VisualElement accès aux clés de spécifique à la plateforme")

Lorsqu’un utilisateur Active une clé d’accès, en appuyant sur la touche Alt suivie de l’accès de clé, l’action par défaut pour le `VisualElement` sera exécuté. Par exemple, lorsqu’un utilisateur Active la clé d’accès sur un [ `Switch` ](xref:Xamarin.Forms.Switch), le `Switch` est activé ou désactivé. Lorsqu’un utilisateur Active la clé d’accès sur un [ `Entry` ](xref:Xamarin.Forms.Entry), le `Entry` Obtient le focus. Lorsqu’un utilisateur Active la clé d’accès sur un [ `Button` ](xref:Xamarin.Forms.Button), le Gestionnaire d’événements pour le [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) événement est exécuté.

Pour plus d’informations sur les clés d’accès, consultez [clés d’accès](/windows/uwp/design/input/access-keys#key-tip-positioning).

<a name="legacy-color-mode" />

### <a name="disabling-legacy-color-mode"></a>Désactiver le Mode de couleur hérité

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

## <a name="views"></a>Affichages

Sur la plateforme de Windows universelle (UWP), les fonctionnalités spécifiques à la plateforme suivante sont fournie pour les vues Xamarin.Forms :

- Détection de l’ordre de lecture à partir du texte contenu dans [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), et [ `Label` ](xref:Xamarin.Forms.Label) instances. Pour plus d’informations, consultez [détection de l’ordre de lecture à partir du contenu](#inputview-readingorder).
- L’activation de la prise en charge des mouvements de drainage dans un [ `ListView` ](xref:Xamarin.Forms.ListView). Pour plus d’informations, consultez [activation appuyez sur mouvement prise en charge dans un ListView](#listview-selectionmode).
- L’activation une [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) pour interagir avec le moteur de vérification orthographique. Pour plus d’informations, consultez [activation SearchBar orthographique](#searchbar-spellcheck).
- L’activation une [ `WebView` ](xref:Xamarin.Forms.WebView) pour afficher les alertes de JavaScript dans une boîte de dialogue de message UWP. Pour plus d’informations, consultez [affichage des alertes de JavaScript](#webview-javascript-alert).

<a name="inputview-readingorder" />

### <a name="detecting-reading-order-from-content"></a>Détection de l’ordre de lecture à partir du contenu

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

<a name="listview-selectionmode" />

### <a name="enabling-tap-gesture-support-in-a-listview"></a>L’activation de la prise en charge des mouvements de Tap dans un ListView

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

<a name="searchbar-spellcheck" />

### <a name="enabling-searchbar-spell-check"></a>L’activation de SearchBar vérification orthographique

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

<a name="webview-javascript-alert" />

### <a name="displaying-javascript-alerts"></a>Affichage des alertes de JavaScript

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

## <a name="pages"></a>Pages

Sur la plateforme Windows universelle, les fonctionnalités suivantes de la plateforme spécifique sont fournie pour les pages Xamarin.Forms :

- Réduire le [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) barre de navigation. Pour plus d’informations, consultez [réduction d’une barre de Navigation MasterDetailPage](#collapsable_navigation_bar).
- Définition des options de placement de barre d’outils. Pour plus d’informations, consultez [changer la disposition de barre d’outils de Page](#toolbar_placement).
- L’activation des icônes de page doit être affiché sur un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) barre d’outils. Pour plus d’informations, consultez [l’activation des icônes sur un TabbedPage](#tabbedpage-icons).

<a name="collapsable_navigation_bar" />

### <a name="collapsing-a-masterdetailpage-navigation-bar"></a>Réduction d’une barre de Navigation MasterDetailPage

Cette plate-forme spécifique est utilisé pour réduire la barre de navigation sur un [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)et est utilisé dans XAML en définissant le [ `MasterDetailPage.CollapseStyle` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty) et [ `MasterDetailPage.CollapsedPaneWidth` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty)propriétés jointes :

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

Le `MasterDetailPage.On<Windows>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur Windows. Le [ `Page.SetCollapseStyle` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.SetCollapseStyle(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) espace de noms, est utilisé pour spécifier le style de la réduction, avec le [ `CollapseStyle` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) énumération fournissant deux valeurs : [ `Full` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full) et [ `Partial` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial). Le [ `MasterDetailPage.CollapsedPaneWidth` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidth(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage},System.Double)) méthode est utilisée pour spécifier la largeur d’une barre de navigation partiellement réduit.

Le résultat qui est spécifié [ `CollapseStyle` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) est appliqué à la [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) instance, avec la largeur également spécifiée :

[![](windows-images/collapsed-navigation-bar.png "Réduit la barre de Navigation spécifiques à la plateforme")](windows-images/collapsed-navigation-bar-large.png#lightbox "réduit la barre de Navigation spécifiques à la plateforme")

<a name="toolbar_placement" />

### <a name="changing-the-page-toolbar-placement"></a>Changer la disposition de la barre d’outils de Page

Spécifiques à cette plateforme est utilisé pour modifier l’emplacement d’une barre d’outils sur un [ `Page` ](xref:Xamarin.Forms.Page)et est utilisé dans XAML en définissant le [ `Page.ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty) propriété attachée à une valeur de la [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) énumération :

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

Le `Page.On<Windows>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur Windows. Le [ `Page.SetToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.SetToolbarPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.Page},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) espace de noms, est utilisé pour définir le positionnement de la barre d’outils, avec le [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) fournissant d’énumération trois valeurs : [ `Default` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default), [ `Top` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top), et [ `Bottom` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom).

Le résultat est que le placement de la barre d’outils spécifié est appliqué à la [ `Page` ](xref:Xamarin.Forms.Page) instance :

[![](windows-images/toolbar-placement.png "Barre d’outils Placement spécifique à la plateforme")](windows-images/toolbar-placement-large.png#lightbox "barre d’outils Placement spécifique à la plateforme")

<a name="tabbedpage-icons" />

### <a name="enabling-icons-on-a-tabbedpage"></a>L’activation des icônes sur un TabbedPage

Spécifiques à cette plateforme permet des icônes de page doit être affiché sur un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) barre d’outils et offre la possibilité de spécifier éventuellement la taille d’icône. Elle est consommée dans XAML en définissant le [ `TabbedPage.HeaderIconsEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.HeaderIconsEnabledProperty) propriété jointe `true`et en définissant éventuellement le [ `TabbedPage.HeaderIconsSize` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.HeaderIconsSizeProperty) propriété jointe un [ `Size` ](xref:Xamarin.Forms.Size) valeur :

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:TabbedPage.HeaderIconsEnabled="true">
    <windows:TabbedPage.HeaderIconsSize>
        <Size>
            <x:Arguments>
                <x:Double>24</x:Double>
                <x:Double>24</x:Double>
            </x:Arguments>
        </Size>
    </windows:TabbedPage.HeaderIconsSize>
    <ContentPage Title="Todo" Icon="todo.png">
        ...
    </ContentPage>
    <ContentPage Title="Reminders" Icon="reminders.png">
        ...
    </ContentPage>
    <ContentPage Title="Contacts" Icon="contacts.png">
        ...
    </ContentPage>
</TabbedPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

public class WindowsTabbedPageIconsCS : Xamarin.Forms.TabbedPage
{
  public WindowsTabbedPageIconsCS()
    {
    On<Windows>().SetHeaderIconsEnabled(true);
    On<Windows>().SetHeaderIconsSize(new Size(24, 24));

    Children.Add(new ContentPage { Title = "Todo", Icon = "todo.png" });
    Children.Add(new ContentPage { Title = "Reminders", Icon = "reminders.png" });
    Children.Add(new ContentPage { Title = "Contacts", Icon = "contacts.png" });
  }
}
```

Le `TabbedPage.On<Windows>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur la plateforme Windows universelle. Le [ `TabbedPage.SetHeaderIconsEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.SetHeaderIconsEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.TabbedPage},System.Boolean)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) espace de noms, est utilisé pour activer ou désactiver les icônes de l’en-tête. Le [ `TabbedPage.SetHeaderIconsSize` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.SetHeaderIconsSize(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.TabbedPage},Xamarin.Forms.Size)) méthode spécifie éventuellement la taille d’icône en-tête avec un [ `Size` ](xref:Xamarin.Forms.Size) valeur.

En outre, le `TabbedPage` classe dans le `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` espace de noms a également un [ `EnableHeaderIcons` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.EnableHeaderIcons*) méthode qui permet des icônes d’en-tête, un [ `DisableHeaderIcons` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.DisableHeaderIcons*) méthode qui désactive les icônes de l’en-tête, et un [ `IsHeaderIconsEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.IsHeaderIconsEnabled*) méthode qui retourne un `boolean` valeur qui indique si des icônes d’en-tête sont activés.

Le résultat est cette page icônes peuvent être affichées sur un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) barre d’outils, avec la taille d’icône éventuellement définie sur la taille souhaitée :

![TabbedPage activés des icônes spécifiques à la plateforme](windows-images/tabbedpage-icons.png "TabbedPage icônes activés spécifique à la plateforme")

## <a name="summary"></a>Récapitulatif

Cet article a montré comment utiliser Windows-spécificités de la plateforme qui sont intégrés dans Xamarin.Forms. Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets.

## <a name="related-links"></a>Liens associés

- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
