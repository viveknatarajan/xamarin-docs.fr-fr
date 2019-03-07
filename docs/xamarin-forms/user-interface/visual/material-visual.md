---
title: Xamarin.Forms matériau Visual
description: Xamarin.Forms matériau Visual peut servir à créer des applications de Xamarin.Forms recherchent identique, ou en grande partie identique, sur iOS et Android.
ms.prod: xamarin
ms.assetid: B774F68C-EF9E-49E1-B738-CDC64879ADA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2019
ms.openlocfilehash: 46ffe29f898e2f7bd8fcbe759f5a2f54e3dd60e1
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557501"
---
# <a name="xamarinforms-material-visual"></a>Xamarin.Forms matériau Visual

Xamarin.Forms matériau Visual peut servir à créer des applications de Xamarin.Forms recherchent identique, ou en grande partie identique, sur iOS et Android. Pour cela, à l’aide de convertisseurs supplémentaires qui implémentent une apparence de matériau, avec les applications optant pour l’apparence via le `Visual` propriété :

```xaml
<ContentPage ...
             Visual="Material">
    ...
</ContentPage>
```

Le code c# équivalent est :

```csharp
ContentPage contentPage = new ContentPage();
contentPage.Visual = VisualMarker.Material;
```

> [!IMPORTANT]
> Le `Visual` propriété est définie dans le `VisualElement` (classe), avec des vues qui héritent le `Visual` valeur de la propriété de leurs parents. Par conséquent, si le `Visual` propriété sur un `ContentPage` garantit que toutes les vues prises en charge dans la page utilisera cette apparence visuelle. En outre, le `Visual` propriété peut être substituée sur une vue.

Convertisseurs de matériel sont incluses pour les vues suivantes sur iOS et Android :

- [`Button`](xref:Xamarin.Forms.Button)
- [`Entry`](xref:Xamarin.Forms.Entry)
- [`Frame`](xref:Xamarin.Forms.Frame)
- [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)
- [`DatePicker`](xref:Xamarin.Forms.DatePicker)
- [`TimePicker`](xref:Xamarin.Forms.TimePicker)
- [`Picker`](xref:Xamarin.Forms.Picker)
- [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)
- [`Editor`](xref:Xamarin.Forms.Editor)
- [`Slider`](xref:Xamarin.Forms.Slider)
- [`Stepper`](xref:Xamarin.Forms.Stepper)

Point de vue fonctionnel, les convertisseurs de matériau ne diffèrent pour les convertisseurs de valeur par défaut.

Sur iOS et Android, votre projet de plateforme doit avoir le [Xamarin.Forms.Visual.Material](https://www.nuget.org/packages/ https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) package NuGet installé. Après avoir installé le package NuGet, code d’initialisation est requis dans chaque projet de plateforme, *après* le `Xamarin.Forms.Forms.Init` appel de méthode. Pour iOS, utilisez le code suivant :

```csharp
global::Xamarin.Forms.Forms.Init();
FormsMaterial.Init();
```

Sur Android, vous devez passer les mêmes paramètres sont passés à `Forms.Init`:

```csharp
global::Xamarin.Forms.Forms.Init(this, bundle);
FormsMaterial.Init(this, bundle);
```

> [!IMPORTANT]
> Sur Android, matériau ne fonctionne qu’avec TargetFramework 9.0 (28 API). En outre, votre projet de plateforme doit utiliser v28 des bibliothèques de prise en charge, et son thème doit hériter d’un thème de composants de matériels ou de continuer à hériter d’un thème AppCompat. Pour plus d’informations, consultez [mise en route avec les composants de matériau pour Android](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md).

Les captures d’écran suivantes montrent une interface utilisateur qui inclut les quatre vues pour lesquelles du matériel convertisseurs existent, mais restitué à l’aide des convertisseurs de valeur par défaut :

[![Capture d’écran de convertisseurs de valeur par défaut, sur iOS et Android](material-visual-images/default-renderers.png "vues à l’aide des convertisseurs de valeur par défaut")](material-visual-images/default-renderers-large.png#lightbox)

Les captures d’écran suivantes montrent la même interface utilisateur restituée à l’aide des convertisseurs de matériel :

[![Capture d’écran de convertisseurs de matériau, sur iOS et Android](material-visual-images/material-renderers.png "vues à l’aide de convertisseurs de matériau")](material-visual-images/material-renderers-large.png#lightbox)

> [!NOTE]
> Avec les convertisseurs material les contrôles de rendu restent des contrôles natifs, et par conséquent, il y aura toujours différences d’interface utilisateur entre les plateformes pour des domaines tels que les polices, des ombres, des couleurs et une élévation.

## <a name="material-renderers"></a>Convertisseurs de matériau

Convertisseurs de matériel peuvent être personnalisés, tout comme les convertisseurs de valeur par défaut, à l’aide des classes de base suivantes :

- `MaterialButtonRenderer`
- `MaterialEntryRenderer`
- `MaterialFrameRenderer`
- `MaterialProgressBarRenderer`
- `MaterialDatePickerRenderer`
- `MaterialTimePickerRenderer`
- `MaterialPickerRenderer`
- `MaterialActivityIndicatorRenderer`
- `MaterialEditorRenderer`
- `MaterialSliderRenderer`
- `MaterialStepperRenderer`

Le code suivant montre un exemple de personnalisation la `MaterialProgressBarRenderer` classe :

```csharp
using Xamarin.Forms.Material.Android;

[assembly: ExportRenderer(typeof(ProgressBar), typeof(CustomMaterialProgressBarRenderer), new[] { typeof(VisualMarker.MaterialVisual) })]
namespace MyApp.Android
{
    public class CustomMaterialProgressBarRenderer : MaterialProgressBarRenderer
    {
        ...
    }
}
```

## <a name="related-links"></a>Liens connexes

- [Renderers personnalisés](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
