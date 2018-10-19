---
title: Caractéristiques de la plateforme Android
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article montre comment consommer les spécificités de plateforme Android qui sont intégrées dans Xamarin.Forms.
ms.prod: xamarin
ms.assetid: C5D4AA65-9BAA-4008-8A1E-36CDB78A435D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/03/2018
ms.openlocfilehash: c422b9ac5af9417523f349537fda1bb0c01aa7bc
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2018
ms.locfileid: "39175175"
---
# <a name="android-platform-specifics"></a>Caractéristiques de la plateforme Android

_Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article montre comment consommer les spécificités de plateforme Android qui sont intégrées dans Xamarin.Forms._

## <a name="visualelements"></a>VisualElements

Sur Android, les fonctionnalités spécifiques à la plateforme suivante sont fournie pour les vues, les pages et les dispositions Xamarin.Forms :

- Contrôler l’ordre de plan des éléments visuels pour déterminer l’ordre de dessin. Pour plus d’informations, consultez [contrôlant l’élévation d’éléments visuels](#elevation).
- Désactiver le mode hérité de couleur sur la prise en charge [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Pour plus d’informations, consultez [la désactivation de Mode de couleur hérité](#legacy-color-mode).

<a name="elevation" />

### <a name="controlling-the-elevation-of-visual-elements"></a>Contrôle de l’élévation d’éléments visuels

Cette plate-forme spécifique est utilisé pour contrôler l’élévation, ou ordre de plan des éléments visuels sur les applications qui ciblent API 21 ou version ultérieure. L’élévation d’un élément visuel détermine son ordre de dessin, avec des éléments visuels avec des valeurs Z supérieures OCCLUSION des éléments visuels avec les valeurs Z inférieure. Elle est consommée dans XAML en définissant le `VisualElement.Elevation` propriété jointe un `boolean` valeur :

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             Title="Elevation">
    <StackLayout>
        <Grid>
            <Button Text="Button Beneath BoxView" />
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>        
        <Grid Margin="0,20,0,0">
            <Button Text="Button Above BoxView - Click Me" android:VisualElement.Elevation="10"/>
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

public class AndroidElevationPageCS : ContentPage
{
    public AndroidElevationPageCS()
    {
        ...
        var aboveButton = new Button { Text = "Button Above BoxView - Click Me" };
        aboveButton.On<Android>().SetElevation(10);

        Content = new StackLayout
        {
            Children =
            {
                new Grid
                {
                    Children =
                    {
                        new Button { Text = "Button Beneath BoxView" },
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                },
                new Grid
                {
                    Margin = new Thickness(0,20,0,0),
                    Children =
                    {
                        aboveButton,
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                }
            }
        };
    }
}
```

Le `Button.On<Android>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur Android. Le `VisualElement.SetElevation` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) espace de noms, est utilisé pour définir l’élévation de l’élément visuel en un type nullable `float`. En outre, le `VisualElement.GetElevation` méthode peut être utilisée pour récupérer la valeur de l’élévation d’un élément visuel.

Le résultat est que l’élévation d’éléments visuels peut être contrôlée afin que les éléments visuels avec les valeurs Z supérieures occlude des éléments visuels avec les valeurs Z inférieure. Par conséquent, dans cet exemple le deuxième [ `Button` ](xref:Xamarin.Forms.Button) est affiché au-dessus de la [ `BoxView` ](xref:Xamarin.Forms.BoxView) , car il a une valeur d’élévation plus élevée :

![](android-images/elevation.png)

<a name="legacy-color-mode" />

### <a name="disabling-legacy-color-mode"></a>Désactiver le Mode de couleur hérité

Certaines des vues Xamarin.Forms fonctionnalité un mode couleur hérité. Dans ce mode, lorsque le [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) propriété de la vue est définie sur `false`, la vue remplace les couleurs définies par l’utilisateur avec les couleurs natif par défaut de l’état désactivé. Pour vers l’arrière, compatibilité, ce mode hérité couleur reste le comportement par défaut pour les vues prises en charge.

Spécifiques à cette plateforme désactive ce mode couleur hérité, afin que les couleurs définies sur une vue par l’utilisateur sont conservées même lorsque la vue est désactivée. Elle est consommée dans XAML en définissant le [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.IsLegacyColorModeEnabledProperty) propriété jointe `false`:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                android:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

_legacyColorModeDisabledButton.On<Android>().SetIsLegacyColorModeEnabled(false);
```

Le `VisualElement.On<Android>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur Android. Le [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement},System.Boolean)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) espace de noms, est utilisé pour contrôler si le mode hérité est désactivé. En outre, le [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement})) méthode peut être utilisée pour retourner si le mode hérité est désactivé.

Le résultat est que le mode hérité de couleur peut être désactivé, afin que les couleurs définies sur une vue par l’utilisateur restent même lorsque la vue est désactivée :

![](android-images/legacy-color-mode-disabled.png "Mode hérité couleur désactivé")

> [!NOTE]
> Lorsque vous définissez un [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) sur une vue, le mode hérité de couleur est complètement ignoré. Pour plus d’informations sur les états visuels, consultez [le Gestionnaire d’état visuel Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="views"></a>Affichages

Sur Android, les fonctionnalités spécifiques à la plateforme suivante sont fournie pour les vues Xamarin.Forms :

- À l’aide de la marge intérieure par défaut et les valeurs de clichés instantanés des boutons Android. Pour plus d’informations, consultez [à l’aide des boutons Android](#button-padding-shadow).
- Définition des options de l’éditeur pour le clavier logiciel pour la méthode d’entrée une [ `Entry` ](xref:Xamarin.Forms.Entry). Pour plus d’informations, consultez [Options d’éditeur de méthode d’entrée de paramètre entrée](#entry-imeoptions).
- Activer le défilement rapide dans un [ `ListView` ](xref:Xamarin.Forms.ListView) pour plus d’informations, consultez [l’activation rapide le défilement dans un ListView](#fastscroll).
- Contrôle si un [ `WebView` ](xref:Xamarin.Forms.WebView) peut afficher un contenu mixte. Pour plus d’informations, consultez [activation du contenu mixte dans une WebView](#webview-mixed-content).

<a name="button-padding-shadow" />

### <a name="using-android-buttons"></a>À l’aide des boutons Android

Cette spécifiques à la plateforme contrôle si les boutons de Xamarin.Forms utilisent la marge intérieure par défaut et les valeurs de clichés instantanés des boutons Android. Elle est consommée dans XAML en définissant le [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPaddingProperty) et [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadowProperty) propriétés jointes à `boolean` valeurs :

```xaml
<ContentPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button ...
                android:Button.UseDefaultPadding="true"
                android:Button.UseDefaultShadow="true" />         
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

button.On<Android>().SetUseDefaultPadding(true).SetUseDefaultShadow(true);
```

Le `Button.On<Android>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur Android. Le [ `Button.SetUseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) et[ `Button.SetUseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) méthodes, dans le [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) espace de noms, permettent de contrôler si les boutons de Xamarin.Forms utilisent la valeur par défaut marge intérieure et les valeurs de clichés instantanés des boutons Android. En outre, le [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) et [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) méthodes peuvent être utilisées pour retourner si un bouton utilise la valeur par défaut de remplissage de valeur et la valeur de clichés instantanés par défaut, respectivement.

Le résultat est que les boutons de Xamarin.Forms pouvez utiliser la marge intérieure par défaut et les valeurs de clichés instantanés des boutons Android :

![](android-images/button-padding-and-shadow.png "Mode hérité couleur désactivé")

Notez que dans la capture d’écran au-dessus de chaque [ `Button` ](xref:Xamarin.Forms.Button) a des définitions identiques, à ceci près que le droit `Button` utilise la marge intérieure par défaut et les valeurs de clichés instantanés des boutons Android.

<a name="entry-imeoptions" />

### <a name="setting-entry-input-method-editor-options"></a>Options de l’éditeur de méthode d’entrée de paramètre entrée

Cette plate-forme spécifique définit la méthode d’entrée options de l’éditeur (IME) pour le clavier logiciel pour un [ `Entry` ](xref:Xamarin.Forms.Entry). Cela inclut la définition de bouton d’action de l’utilisateur dans l’angle inférieur du clavier logiciel et les interactions avec le `Entry`. Elle est consommée dans XAML en définissant le [ `Entry.ImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.ImeOptionsProperty) propriété attachée à une valeur de la [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) énumération :

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Entry ... android:Entry.ImeOptions="Send" />
        ...
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

entry.On<Android>().SetImeOptions(ImeFlags.Send);
```

Le `Entry.On<Android>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur Android. Le [ `Entry.SetImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.SetImeOptions(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Entry},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) espace de noms, est utilisé pour définir l’option d’action de méthode d’entrée pour le clavier logiciel pour le [ `Entry` ](xref:Xamarin.Forms.Entry), avec le [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) énumération fournissant les valeurs suivantes :

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Default) : indique qu’aucune action spécifique de clé est requise, et que le contrôle sous-jacent génère ses propres si possible. Il s’agit de `Next` ou `Done`.
- [`None`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.None) : indique qu’aucune clé de l’action ne sera disponible.
- [`Go`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Go) – Indique que la clé d’action effectue une opération de « go », qui mène l’utilisateur à la cible du texte tapé.
- [`Search`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Search) – Indique que la clé d’action effectue une opération de « recherche », qui mène l’utilisateur pour les résultats de recherche pour le texte qu’il a entré.
- [`Send`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Send) – Indique que la clé d’action effectuera une opération « Envoyer », en fournissant le texte à sa cible.
- [`Next`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Next) – Indique que la clé d’action effectuera une opération « suivante », qui mène l’utilisateur dans le champ suivant qui accepte le texte.
- [`Done`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Done) – Indique que la clé d’action effectuera une opération « done », de fermeture du clavier de manière réversible.
- [`Previous`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Previous) – Indique que la clé d’action effectuera une opération « précédente », qui mène l’utilisateur sur le champ précédent qui acceptera le texte.
- [`ImeMaskAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.ImeMaskAction) – le masque pour sélectionner les options d’action.
- [`NoPersonalizedLearning`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoPersonalizedLearning) – Indique que le vérificateur orthographique sera en savoir plus à partir de l’utilisateur, ni suggérer des corrections selon ce que l’utilisateur a tapé précédemment.
- [`NoFullscreen`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoFullscreen) – Indique que l’interface utilisateur ne doit pas atteindre en plein écran.
- [`NoExtractUi`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoExtractUi) : indique qu’aucune interface utilisateur ne doivent être affichée pour le texte extrait.
- [`NoAccessoryAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoAccessoryAction) : indique qu’aucune interface utilisateur ne s’affichera pour les actions personnalisées.

Le résultat qui est spécifié [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) valeur est appliquée pour le clavier logiciel pour le [ `Entry` ](xref:Xamarin.Forms.Entry), qui définit la méthode d’entrée options de l’éditeur :

[![Entrée d’entrée de méthode éditeur spécifique à la plateforme](android-images/entry-imeoptions.png "entrée d’entrée de méthode éditeur spécifique à la plateforme")](android-images/entry-imeoptions-large.png#lightbox "entrée d’entrée de méthode éditeur spécifique à la plateforme")

<a name="fastscroll" />

### <a name="enabling-fast-scrolling-in-a-listview"></a>Activer le défilement rapide dans un ListView

Cette plate-forme spécifique est utilisé pour activer le défilement rapide des données dans un [ `ListView` ](xref:Xamarin.Forms.ListView). Elle est consommée dans XAML en définissant le `ListView.IsFastScrollEnabled` propriété jointe un `boolean` valeur :

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        ...
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  GroupDisplayBinding="{Binding Key}"
                  IsGroupingEnabled="true"
                  android:ListView.IsFastScrollEnabled="true">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var listView = new Xamarin.Forms.ListView { IsGroupingEnabled = true, ... };
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "GroupedEmployees");
listView.GroupDisplayBinding = new Binding("Key");
listView.On<Android>().SetIsFastScrollEnabled(true);
```

Le `ListView.On<Android>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur Android. Le `ListView.SetIsFastScrollEnabled` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) espace de noms, est utilisé pour activer le défilement rapide des données dans un [ `ListView` ](xref:Xamarin.Forms.ListView). En outre, le `SetIsFastScrollEnabled` méthode peut être utilisée pour activer/désactiver le défilement rapide en appelant le `IsFastScrollEnabled` méthode à retourner si le défilement rapide est activé :

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

Il en résulte que défilement rapide des données dans un [ `ListView` ](xref:Xamarin.Forms.ListView) peut être activé, ce qui modifie la taille du curseur de défilement :

[![](android-images/fastscroll.png "ListView FastScroll spécifique à la plateforme")](android-images/fastscroll-large.png#lightbox "ListView FastScroll Plaform-Specific")

<a name="webview-mixed-content" />

### <a name="enabling-mixed-content-in-a-webview"></a>L’activation de contenu mixte dans un affichage Web

Ce contrôle spécifique à la plateforme si un [ `WebView` ](xref:Xamarin.Forms.WebView) pouvez affiche un contenu mixte dans les applications qui ciblent les API 21 ou version ultérieure. Contenu mixte est un contenu qui sont initialement chargée sur une connexion HTTPS, mais qui charge des ressources (telles que des images, audio, vidéo, feuilles de style, scripts) via une connexion HTTP. Elle est consommée dans XAML en définissant le [ `WebView.MixedContentMode` ](x:ref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty) propriété attachée à une valeur de la [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) énumération :

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

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

[![WebView mixte de gestion de contenu spécifique à la plateforme](android-images/webview-mixedcontent.png "WebView mixte de gestion de contenu spécifique à la plateforme")](android-images/webview-mixedcontent-large.png#lightbox "WebView mixte de gestion de contenu spécifique à la plateforme")

## <a name="pages"></a>Pages

Sur Android, les fonctionnalités suivantes de la plateforme spécifique sont fournie pour les pages Xamarin.Forms :

- Définition de la hauteur de la barre de navigation sur un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Pour plus d’informations, consultez [définissant la hauteur de la barre de Navigation sur un NavigationPage](#navigationpage-barheight).
- L’activation de glissement entre les pages dans un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Pour plus d’informations, consultez [activation passant entre les Pages dans un TabbedPage](#enable_swipe_paging).
- Définition de la position de la barre d’outils et les couleurs sur un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Pour plus d’informations, consultez [emplacement des paramètres de barre d’outils TabbedPage et couleur](#tabbedpage-toolbar).

<a name="navigationpage-barheight" />

### <a name="setting-the-navigation-bar-height-on-a-navigationpage"></a>Définition de la barre de Navigation hauteur sur un NavigationPage

Cette plate-forme spécifique définit la hauteur de la barre de navigation sur un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Elle est consommée dans XAML en définissant le [ `NavigationPage.BarHeight` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.BarHeightProperty) propriété pouvant être liée à une valeur entière :

```xaml
<NavigationPage ...
                xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
                android:NavigationPage.BarHeight="450">
    ...
</NavigationPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

public class AndroidNavigationPageCS : Xamarin.Forms.NavigationPage
{
    public AndroidNavigationPageCS()
    {
        On<Android>().SetBarHeight(450);
    }
}
```

Le `NavigationPage.On<Android>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur la compatibilité des applications Android. Le [ `NavigationPage.SetBarHeight` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.SetBarHeight(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.NavigationPage},System.Int32)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) espace de noms, est utilisé pour définir la hauteur de la barre de navigation sur un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). En outre, le [ `NavigationPage.GetBarHeight` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.GetBarHeight(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.NavigationPage})) méthode peut être utilisée pour retourner la hauteur de la barre de navigation dans le `NavigationPage`.

Le résultat est que la hauteur de la barre de navigation sur un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) peuvent être définies :

![](android-images/navigationpage-barheight.png "Hauteur de barre de navigation NavigationPage")

<a name="enable_swipe_paging" />

### <a name="enabling-swiping-between-pages-in-a-tabbedpage"></a>L’activation de glissement entre les Pages dans un TabbedPage

Spécifiques à cette plateforme est utilisée pour activer le balayage avec un mouvement du doigt horizontal entre les pages dans un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Elle est consommée dans XAML en définissant le [ `TabbedPage.IsSwipePagingEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty) propriété jointe un `boolean` valeur :

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.OffscreenPageLimit="2"
            android:TabbedPage.IsSwipePagingEnabled="true">
    ...
</TabbedPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetOffscreenPageLimit(2)
             .SetIsSwipePagingEnabled(true);
```

Le `TabbedPage.On<Android>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur Android. Le [ `TabbedPage.SetIsSwipePagingEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetIsSwipePagingEnabled(Xamarin.Forms.BindableObject,System.Boolean)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) espace de noms, est utilisée pour activer le glissement entre les pages dans un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). En outre, le `TabbedPage` classe dans le `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` espace de noms a également un [ `EnableSwipePaging` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.EnableSwipePaging(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage})) méthode qui permet ce spécifiques à la plateforme, et un [ `DisableSwipePaging` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.DisableSwipePaging(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage})) méthode désactive Cette plateforme spécifique. Le [ `TabbedPage.OffscreenPageLimit` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty) propriété jointe, et [ `SetOffscreenPageLimit` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetOffscreenPageLimit(Xamarin.Forms.BindableObject,System.Int32)) (méthode), sont utilisées pour définir le nombre de pages doit être conservé dans un état d’inactivité de chaque côté de la page actuelle.

Le résultat est que la pagination de balayage via les pages affichées par un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) est activé :

![](android-images/tabbedpage-swipe.png)

<a name="tabbedpage-toolbar" />

### <a name="setting-tabbedpage-toolbar-placement-and-color"></a>Couleur et emplacement des paramètres de barre d’outils TabbedPage

Ces caractéristiques de la plateforme sont utilisées pour définir le positionnement et la couleur de la barre d’outils sur un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Ils sont utilisés dans XAML en définissant le [ `TabbedPage.ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.ToolbarPlacementProperty) propriété attachée à une valeur de la [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) énumération et le [ `TabbedPage.BarItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.BarItemColorProperty) et [ `TabbedPage.BarSelectedItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.BarSelectedItemColorProperty) propriétés jointes à un [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.ToolbarPlacement="Bottom"
            android:TabbedPage.BarItemColor="Black"
            android:TabbedPage.BarSelectedItemColor="Red">
    ...
</TabbedPage>
```

Ils peuvent également être consommés à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetToolbarPlacement(ToolbarPlacement.Bottom)
             .SetBarItemColor(Color.Black)
             .SetBarSelectedItemColor(Color.Red);
```

Le `TabbedPage.On<Android>` méthode spécifie que ces caractéristiques de la plateforme s’exécutera uniquement sur Android. Le [ `TabbedPage.SetToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetToolbarPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) espace de noms, est utilisé pour définir le positionnement de la barre d’outils sur un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), avec la [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) énumération fournissant les valeurs suivantes :

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Default) – Indique que la barre d’outils est placé à l’emplacement par défaut sur la page. Il s’agit en haut de la page sur les téléphones et le bas de la page sur les autres idiomes de l’appareil.
- [`Top`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Top) – Indique que la barre d’outils est placé en haut de la page.
- [`Bottom`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Bottom) – Indique que la barre d’outils est placé au bas de la page.

En outre, le [ `TabbedPage.SetBarItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetBarItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage},Xamarin.Forms.Color)) et [ `TabbedPage.SetBarSelectedItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetBarSelectedItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage},Xamarin.Forms.Color)) méthodes sont utilisées pour définir la couleur de barre d’outils et éléments de barre d’outils sélectionné, respectivement.

> [!NOTE]
> Le [ `GetToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.GetToolbarPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage})), [ `GetBarItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.GetBarItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage})), et [ `GetBarSelectedItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.GetBarSelectedItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage})) méthodes peuvent être utilisées pour récupérer le positionnement et la couleur de la [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) barre d’outils.

Le résultat est que le placement de la barre d’outils, la couleur des éléments de barre d’outils et la couleur de l’élément de barre d’outils sélectionné peuvent être définies sur une [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage):

![](android-images/tabbedpage-toolbar-placement.png)

## <a name="application"></a>Application

Sur Android, les fonctionnalités suivantes de la plateforme spécifique sont fournie pour le Xamarin.Forms [ `Application` ](xref:Xamarin.Forms.Application) classe :

- Définition du mode de fonctionnement d’un clavier logiciel. Pour plus d’informations, consultez [définition du Mode d’entrée clavier réversible](#soft_input_mode).
- La désactivation de la [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) et [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) page événements de cycle de vie sur Suspendre et reprendre respectivement, pour les applications qui utilisent AppCompat. Pour plus d’informations, consultez [désactivant les Disappearing et les événements de cycle de vie de Page figurant](#disable_lifecycle_events).

<a name="soft_input_mode" />

### <a name="setting-the-soft-keyboard-input-mode"></a>Définition du Mode d’entrée de clavier logiciel

Cette plate-forme spécifique est utilisé pour définir le mode de fonctionnement pour une zone d’entrée de clavier logiciel et est consommé dans XAML en définissant le [ `Application.WindowSoftInputModeAdjust` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.WindowSoftInputModeAdjustProperty) propriété attachée à une valeur de la [ `WindowSoftInputModeAdjust` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust) énumération :

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize">
  ...
</Application>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

App.Current.On<Android>().UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize);
```

Le `Application.On<Android>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur Android. Le [ `Application.UseWindowSoftInputModeAdjust` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.UseWindowSoftInputModeAdjust(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) espace de noms, est utilisé pour définir le mode de fonctionnement de zone d’entrée clavier logiciel, avec la [ `WindowSoftInputModeAdjust` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust) énumération en fournissant deux valeurs : [ `Pan` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Pan) et [ `Resize` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize). Le `Pan` valeur utilise le [ `AdjustPan` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustPan/) option ajustement, ce qui ne se redimensionne la fenêtre quand un contrôle d’entrée a le focus. Au lieu de cela, le contenu de la fenêtre est panoramique afin que le focus actuel n’est pas masqué par le clavier logiciel. Le `Resize` valeur utilise le [ `AdjustResize` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustResize/) option ajustement, ce qui redimensionne la fenêtre lorsqu’un contrôle d’entrée a le focus, pour libérer de l’espace pour le clavier logiciel.

Le résultat est que le clavier logiciel entrée zone mode d’opération peut être défini quand un contrôle d’entrée a le focus :

[![](android-images/pan-resize.png "Clavier logiciel fonctionne en Mode spécifique à la plateforme")](android-images/pan-resize-large.png#lightbox "Soft Keyboard Operating Mode Plaform-Specific")

<a name="disable_lifecycle_events" />

### <a name="disabling-the-disappearing-and-appearing-page-lifecycle-events"></a>Désactiver les événements de cycle de vie de Page disparition et affichage

Cette plate-forme spécifique est utilisée pour désactiver le [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) et [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) des événements de page sur application suspendre et reprendre la respectivement, pour les applications qui utilisent AppCompat. En outre, il inclut la possibilité de contrôler si le clavier logiciel s’affiche à la reprise, si elle était affichée sur pause, à condition que le mode de fonctionnement du clavier de manière réversible est activée [ `WindowSoftInputModeAdjust.Resize` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize).

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

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

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

[![](android-images/keyboard-on-resume.png "Cycle de vie des événements spécifiques à la plateforme")](android-images/keyboard-on-resume-large.png#lightbox "cycle de vie des événements spécifiques à la plateforme")

## <a name="summary"></a>Récapitulatif

Cet article a montré comment consommer les spécificités de plateforme Android qui sont intégrées dans Xamarin.Forms. Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets.

## <a name="related-links"></a>Liens associés

- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [AndroidSpecific](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
