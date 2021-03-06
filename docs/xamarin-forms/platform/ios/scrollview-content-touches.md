---
title: ScrollView contenu a un impact sur iOS
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article explique comment utiliser l’iOS spécifique à la plateforme qui contrôle si un ScrollView gère un mouvement tactile ou le transmet à son contenu.
ms.prod: xamarin
ms.assetid: 99F823DB-B379-40F0-A343-A9783C341120
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 19a683fbd4aeb7060d97406a89c1e9308fb56b03
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65926001"
---
# <a name="scrollview-content-touches-on-ios"></a>ScrollView contenu a un impact sur iOS

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

Un minuteur implicit est déclenché lorsqu’un mouvement tactile commence dans un [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) sur iOS et le `ScrollView` décide, en fonction de l’action de l’utilisateur dans l’intervalle de minuterie, s’il doit gérer le mouvement ou passer à son contenu. Par défaut, le fichier iOS `ScrollView` finales de contenu des retards, mais cela peut entraîner des problèmes dans certaines circonstances avec le `ScrollView` contenu ne gagnante pas le mouvement lorsqu’elle le devrait. Par conséquent, ce contrôle spécifique à la plateforme si un `ScrollView` gère un mouvement tactile ou la transmet à son contenu. Elle est consommée dans XAML en définissant le `ScrollView.ShouldDelayContentTouches` propriété jointe un `boolean` valeur :

```xaml
<MasterDetailPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <MasterDetailPage.Master>
        <ContentPage Title="Menu" BackgroundColor="Blue" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <ContentPage>
            <ScrollView x:Name="scrollView" ios:ScrollView.ShouldDelayContentTouches="false">
                <StackLayout Margin="0,20">
                    <Slider />
                    <Button Text="Toggle ScrollView DelayContentTouches" Clicked="OnButtonClicked" />
                </StackLayout>
            </ScrollView>
        </ContentPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

Vous pouvez également, il peut être consommé à partir de C# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

Le `ScrollView.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le `ScrollView.SetShouldDelayContentTouches` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, est utilisée pour déterminer si un [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) gère un mouvement tactile ou la transmet à son contenu. En outre, le `SetShouldDelayContentTouches` méthode peut être utilisée pour activer/désactiver retarder finales de contenu en appelant le `ShouldDelayContentTouches` méthode à retourner si la touche de contenu est différée :

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

Le résultat est qu’un [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) peut désactiver ce qui retarde la réception de contenu finales, par conséquent, qui dans ce scénario la [ `Slider` ](xref:Xamarin.Forms.Slider) reçoit le mouvement plutôt que la [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) page de la [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage):

[![](scrollview-content-touches-images/scrollview-delay-content-touches.png "Délai de ScrollView contenu touche spécifiques à la plateforme")](scrollview-content-touches-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView délai contenu touche spécifiques à la plateforme")

## <a name="related-links"></a>Liens connexes

- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
