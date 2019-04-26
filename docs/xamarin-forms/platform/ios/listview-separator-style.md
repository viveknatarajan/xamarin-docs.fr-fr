---
title: Style de séparateur de ListView sur iOS
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article explique comment utiliser l’iOS spécifique à la plateforme qui détermine si le séparateur entre les cellules d’un ListView utilise toute la largeur de la ListView.
ms.prod: xamarin
ms.assetid: A4CB45CE-9FB7-47ED-8C3D-93E39BF282E4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 0fc8cdfac22ad54b73af193ce76e8fd27b8fb0da
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60949011"
---
# <a name="listview-separator-style-on-ios"></a>Style de séparateur de ListView sur iOS

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Cette spécifiques à la plateforme iOS contrôle si le séparateur entre les cellules dans un [ `ListView` ](xref:Xamarin.Forms.ListView) utilise toute la largeur de la `ListView`. Elle est consommée dans XAML en définissant le [ `ListView.SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SeparatorStyleProperty) propriété attachée à une valeur de la [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) énumération :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.SeparatorStyle="FullWidth">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de C# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetSeparatorStyle(SeparatorStyle.FullWidth);
```

Le `ListView.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le [ `ListView.SetSeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SetSeparatorStyle(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle)) (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, est utilisé pour contrôler si le séparateur entre les cellules de la [ `ListView` ](xref:Xamarin.Forms.ListView) utilise la version complète largeur de la `ListView`, avec le [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) énumération fournissant deux valeurs possibles :

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.Default) : indique le comportement de séparateur iOS par défaut. Il s’agit du comportement par défaut dans Xamarin.Forms.
- [`FullWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.FullWidth) – Indique que les séparateurs peuvent être issues d’un bord de la `ListView` à l’autre.

Le résultat qui est spécifié [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) valeur est appliquée à la [ `ListView` ](xref:Xamarin.Forms.ListView), qui contrôle la largeur du séparateur entre les cellules :

![](listview-separator-style-images/listview-separatorstyle.png "ListView SeparatorStyle spécifiques à la plateforme")

> [!NOTE]
> Une fois que le style du séparateur a été défini sur `FullWidth`, elle ne peut pas être reconvertie en `Default` lors de l’exécution.

## <a name="related-links"></a>Liens connexes

- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
