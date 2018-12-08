---
title: Xamarin.Forms Visual
description: Cet article présente Xamarin.Forms visuel, qui restitue des vues de façon identique, ou en grande partie identique, sur iOS et Android.
ms.prod: xamarin
ms.assetid: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2018
ms.openlocfilehash: df2351e35817a6468fed236752eea8e5ad11024f
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53061884"
---
# <a name="xamarinforms-visual"></a>Xamarin.Forms Visual

![Preview](~/media/shared/preview.png)

_Cet article présente Xamarin.Forms visuel, qui restitue des vues de façon identique, ou en grande partie identique, sur iOS et Android._

De nombreux développeurs souhaitent créer des applications de Xamarin.Forms qui semblent identiques, ou en grande partie identique, sur iOS et Android. Xamarin.Forms 4.0-pre1 inclut un mécanisme d’inclusion des convertisseurs supplémentaires qui implémentent une apparence visuelle, avec les applications optant pour l’apparence via un `Visual` propriété :

```xaml
<ContentPage ...
             Visual="Material">
    ...
</ContentPage>    
```

Les convertisseurs qui implémentent l’apparence visuelle sont ensuite utilisés pour les vues de convertisseur, plutôt que les convertisseurs de valeur par défaut. Au moment de sélection du convertisseur, le `Visual` propriété de la vue est inspectée et incluse dans le processus de sélection de convertisseur. En outre, si le `Visual` propriété change lors de l’exécution, le convertisseur spécifié est recréé, ainsi que les enfants.

> [!IMPORTANT]
> Le `Visual` propriété est définie dans le `VisualElement` (classe), avec des vues qui héritent le `Visual` valeur de la propriété de leurs parents. Par conséquent, si le `Visual` propriété sur un `ContentPage` garantit que toutes les vues prises en charge dans la page utilisera cette apparence visuelle. En outre, le `Visual` propriété peut être substituée sur une vue.

Xamarin.Forms 4.0-pre1 inclut une apparence visuelle expérimentale en fonction de conception matérielle, avec les convertisseurs sont connues comme les convertisseurs de matériau. Convertisseurs de matériel sont incluses pour les vues suivantes sur iOS et Android :

- [`Button`](xref:Xamarin.Forms.Button)
- [`Entry`](xref:Xamarin.Forms.Entry)
- [`Frame`](xref:Xamarin.Forms.Frame)
- [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)

Point de vue fonctionnel, les convertisseurs de matériau ne diffèrent pour les convertisseurs de valeur par défaut. Cependant, ils sont actuellement en phase expérimentale et peut uniquement être utilisées en ajoutant la ligne suivante de code à votre `AppDelegate` classe sur iOS ou à votre `MainActivity` classe sur Android, avant d’appeler `Forms.Init`:

```csharp
Forms.SetFlags("Visual_Experimental");
```

En outre, sur iOS, votre projet de plateforme doit avoir le [Xamarin.iOS.MaterialComponents](https://www.nuget.org/packages/Xamarin.iOS.MaterialComponents/) package NuGet installé. Sur Android, Visual fonctionne uniquement avec les API 29, votre projet de plateforme doit utiliser v28 des bibliothèques de prise en charge et définir son thème à hériter d’un thème de composants de matériels ou de continuer à hériter d’un thème AppCompat lors de l’ajout de certains nouveaux attributs de thème pour le thème. Pour plus d’informations, consultez [mise en route avec les composants de matériau pour Android](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md).

Les captures d’écran suivantes montrent une interface utilisateur qui inclut les quatre vues pour lesquelles du matériel convertisseurs existent, mais restitué à l’aide des convertisseurs de valeur par défaut :

[![Par défaut de convertisseurs](visual-images/default-renderers.png "vues à l’aide des convertisseurs de valeur par défaut")](visual-images/default-renderers-large.png#lightbox)

Les captures d’écran suivantes montrent la même interface utilisateur restituée à l’aide des convertisseurs de matériel :

[![Convertisseurs de matériau](visual-images/material-renderers.png "vues à l’aide de convertisseurs de matériau")](visual-images/material-renderers-large.png#lightbox)

> [!NOTE]
> Avec les convertisseurs material les contrôles de rendu restent des contrôles natifs, et par conséquent, il y aura toujours différences d’interface utilisateur entre les plateformes pour des domaines tels que les polices, des ombres, des couleurs et une élévation.

## <a name="related-links"></a>Liens associés

- [Renderers personnalisés](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
