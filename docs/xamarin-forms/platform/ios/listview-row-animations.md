---
title: Animations de ligne de ListView sur iOS
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article explique comment utiliser l’iOS spécifique à la plateforme qui contrôle si les animations de ligne sont désactivées lorsque la collection d’éléments ListView est en cours de mise à jour.
ms.prod: xamarin
ms.assetid: E8F5103F-4D8E-4A5A-A16C-7FA14EE786AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/21/2019
ms.openlocfilehash: 50480f5b21c6f0c855ff6f9aa22b6126c6a6787c
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557441"
---
# <a name="listview-row-animations-on-ios"></a>Animations de ligne de ListView sur iOS

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Ce contrôle de spécifique à la plateforme iOS ligne indique si les animations sont désactivées lorsque le [ `ListView` ](xref:Xamarin.Forms.ListView) collection items est en cours de mise à jour. Elle est consommée dans XAML en définissant le `ListView.RowAnimationsEnabled` propriété pouvant être liée `false`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.RowAnimationsEnabled="false">
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

listView.On<iOS>().SetRowAnimationsEnabled(false);
```

Le `ListView.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le `ListView.SetRowAnimationsEnabled` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, est utilisé pour contrôler si les animations de ligne sont désactivées lorsque le [ `ListView` ](xref:Xamarin.Forms.ListView) collection items est en cours de mise à jour. En outre, le `ListView.GetRowAnimationsEnabled` méthode peut être utilisée pour retourner si les animations de ligne sont désactivées sur le `ListView`.

> [!NOTE]
> [`ListView`](xref:Xamarin.Forms.ListView) animations de ligne sont activées par défaut. Par conséquent, une animation se produit lorsqu’une nouvelle ligne est insérée dans un `ListView`.

## <a name="related-links"></a>Liens connexes

- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
