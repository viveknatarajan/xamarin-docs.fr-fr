---
title: Style d’en-tête de groupe ListView sur iOS
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article explique comment utiliser l’iOS spécifique à la plateforme qui contrôle si les cellules d’en-tête ListView flottant pendant le défilement.
ms.prod: xamarin
ms.assetid: 099B2C7F-727F-4BCF-903B-87E728108C24
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 58b3787b71cff9b78f1c6b577be6c320367f1cee
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557451"
---
# <a name="listview-group-header-style-on-ios"></a>Style d’en-tête de groupe ListView sur iOS

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Ce contrôle de spécifique à la plateforme iOS si [ `ListView` ](xref:Xamarin.Forms.ListView) cellules d’en-tête flottant pendant le défilement. Elle est consommée dans XAML en définissant le `ListView.GroupHeaderStyle` propriété pouvant être liée à une valeur de la `GroupHeaderStyle` énumération :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.GroupHeaderStyle="Grouped">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetGroupHeaderStyle(GroupHeaderStyle.Grouped);
```

Le `ListView.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le `ListView.SetGroupHeaderStyle` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, est utilisée pour déterminer si [ `ListView` ](xref:Xamarin.Forms.ListView) cellules d’en-tête flottant pendant le défilement. Le `GroupHeaderStyle` énumération fournit deux valeurs possibles :

- `Plain` – Indique que les cellules d’en-tête flottant lorsque le [ `ListView` ](xref:Xamarin.Forms.ListView) est défilé (valeur par défaut).
- `Grouped` – Indique que les cellules d’en-tête flotter pas lorsque le [ `ListView` ](xref:Xamarin.Forms.ListView) défile.

En outre, le `ListView.GetGroupHeaderStyle` méthode peut être utilisée pour retourner le `GroupHeaderStyle` qui est appliqué à la [ `ListView` ](xref:Xamarin.Forms.ListView).

Le résultat qui est spécifié `GroupHeaderStyle` valeur est appliquée à la [ `ListView` ](xref:Xamarin.Forms.ListView), qui contrôle si les cellules d’en-tête flottant pendant le défilement :

[![Capture d’écran de flottants et non flottante ListView cellules d’en-tête, sur iOS](listview-group-header-style-images/group-header-styles.png "ListView avec des cellules d’en-tête flottant et non flottante")](listview-group-header-style-images/group-header-styles-large.png#lightbox "ListView avec des cellules d’en-tête flottant et non flottante")

## <a name="related-links"></a>Liens connexes

- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
