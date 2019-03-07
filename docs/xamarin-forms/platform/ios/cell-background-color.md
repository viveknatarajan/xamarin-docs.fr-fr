---
title: Couleur d’arrière-plan sur iOS
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article explique comment utiliser la plateforme spécifique à iOS qui définit la couleur d’arrière-plan par défaut des cellules sur iOS.
ms.prod: xamarin
ms.assetid: 2A3FDACF-5AE2-40DE-8488-6FE41733712F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 396e674bb3e5642f7c54455ee9e30ba5bf232f18
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557411"
---
# <a name="cell-background-color-on-ios"></a>Couleur d’arrière-plan sur iOS

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Cette spécifiques à la plateforme iOS définit la couleur d’arrière-plan par défaut [ `Cell` ](xref:Xamarin.Forms.Cell) instances. Elle est consommée dans XAML en définissant le `Cell.DefaultBackgroundColor` propriété pouvant être liée à un [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  IsGroupingEnabled="true">
            <ListView.GroupHeaderTemplate>
                <DataTemplate>
                    <ViewCell ios:Cell.DefaultBackgroundColor="Teal">
                        <Label Margin="10,10"
                               Text="{Binding Key}"
                               FontAttributes="Bold" />
                    </ViewCell>
                </DataTemplate>
            </ListView.GroupHeaderTemplate>
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

var viewCell = new ViewCell { View = ... };
viewCell.On<iOS>().SetDefaultBackgroundColor(Color.Teal);
```

Le `ListView.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le `Cell.SetDefaultBackgroundColor` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, définit la couleur d’arrière-plan de cellule à une certaine [ `Color` ](xref:Xamarin.Forms.Color). En outre, le `Cell.DefaultBackgroundColor` méthode peut être utilisée pour récupérer la couleur d’arrière-plan de cellule en cours.

Le résultat est que la couleur d’arrière-plan dans un [ `Cell` ](xref:Xamarin.Forms.Cell) peut être définie à un spécifique [ `Color` ](xref:Xamarin.Forms.Color):

[![Capture d’écran de cellules d’en-tête de groupe de bleu-vert, sur iOS](cell-background-color-images/group-header-cell-color.png "ListView avec des cellules d’en-tête de groupe de bleu-vert")](cell-background-color-images/group-header-cell-color-large.png#lightbox "ListView avec des cellules d’en-tête de groupe de bleu-vert")

## <a name="related-links"></a>Liens connexes

- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
