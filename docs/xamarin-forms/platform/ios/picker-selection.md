---
title: Sélection d’éléments de sélecteur sur iOS
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article explique comment utiliser l’iOS spécifique à la plateforme qui contrôle si la sélection d’éléments se produit dans un sélecteur.
ms.prod: xamarin
ms.assetid: 26B0604A-BD30-49FD-83A6-F0EDFBB0524B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 21c4c289a3fd30db890be6811875412ce4913cf5
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61250607"
---
# <a name="picker-item-selection-on-ios"></a>Sélection d’éléments de sélecteur sur iOS

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Cette spécifiques à la plateforme iOS contrôle si la sélection d’éléments se produit dans un [ `Picker` ](xref:Xamarin.Forms.Picker), permettant à l’utilisateur spécifier que la sélection d’éléments se produit lorsque vous parcourez les éléments dans le contrôle, ou uniquement une fois que le **fait** bouton est enfoncé. Elle est consommée dans XAML en définissant le `Picker.UpdateMode` propriété attachée à une valeur de la `UpdateMode` énumération :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <Picker ... Title="Select a monkey" ios:Picker.UpdateMode="WhenFinished">
          ...
        </Picker>
        ...
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de C# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

Le `Picker.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le `Picker.SetUpdateMode` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, permet de contrôler en cas de sélection d’élément, avec la `UpdateMode` énumération fournissant deux valeurs possibles :

- `Immediately` : sélection d’éléments se produit lorsque l’utilisateur parcourt des éléments dans le [ `Picker` ](xref:Xamarin.Forms.Picker). Il s’agit du comportement par défaut dans Xamarin.Forms.
- `WhenFinished` : sélection d’éléments se produit uniquement une fois que l’utilisateur a appuyé sur le **fait** situé dans le [ `Picker` ](xref:Xamarin.Forms.Picker).

En outre, le `SetUpdateMode` méthode peut être utilisée pour activer/désactiver les valeurs d’énumération en appelant le `UpdateMode` (méthode), qui retourne actuel `UpdateMode`:

```csharp
switch (picker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        picker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

Le résultat qui est spécifié `UpdateMode` est appliqué à la [ `Picker` ](xref:Xamarin.Forms.Picker), qui contrôle lorsque la sélection d’éléments se produit :

[![](picker-selection-images/picker-updatemode.png "Sélecteur UpdateMode spécifiques à la plateforme")](picker-selection-images/picker-updatemode-large.png#lightbox "sélecteur UpdateMode spécifiques à la plateforme")

## <a name="related-links"></a>Liens connexes

- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
