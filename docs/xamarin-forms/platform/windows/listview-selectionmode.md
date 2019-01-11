---
title: ListView SelectionMode sur Windows
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article explique comment utiliser le Windows spécifique à la plateforme qui contrôle si les éléments dans un ListView peuvent répondre à appuyez sur les mouvements.
ms.prod: xamarin
ms.assetid: 57EF3A7F-1407-4B31-AE21-D149293D4228
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: d2a94347448af031f50341729d77c7385225d107
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54209584"
---
# <a name="listview-selectionmode-on-windows"></a>ListView SelectionMode sur Windows

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Sur la plateforme Windows universelle, par défaut Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) utilise natif `ItemClick` événement pour répondre à l’interaction, plutôt que du natif `Tapped` événement. Cela fournit des fonctionnalités d’accessibilité afin que le Narrateur Windows et le clavier peuvent interagir avec le `ListView`. Toutefois, mais affiche également les mouvements tap à l’intérieur de la `ListView` inutilisable.

Ce contrôle spécifique à la plateforme de plateforme Windows universelle si les éléments dans un [ `ListView` ](xref:Xamarin.Forms.ListView) peut répondre pour appuyer sur les mouvements et par conséquent, si natif `ListView` se déclenche le `ItemClick` ou `Tapped` événement. Elle est consommée dans XAML en définissant le [ `ListView.SelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SelectionModeProperty) propriété attachée à une valeur de la [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) énumération :

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

## <a name="related-links"></a>Liens connexes

- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
